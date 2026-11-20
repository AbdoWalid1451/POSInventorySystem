# POS & Inventory Management System

A desktop Point of Sale (POS) and Inventory Management System built with **C# WinForms**, **ADO.NET**, and **SQL Server**, following a **3-tier architecture**.

The system lets a store sell products quickly, keeps stock accurate automatically, manages purchases and suppliers, and provides reports on sales and inventory.

---

## Features

- **Authentication & roles:** secure login with two roles, Admin and Cashier
- **POS screen:** search or scan products, cart, discounts, tax, multiple payment methods, receipt printing, hold and resume orders
- **Sales returns:** returned items go back to stock
- **Inventory:** real-time stock, manual adjustments, low-stock alerts, full stock movement log
- **Products:** categories, units, barcodes, images, prices
- **Purchases & suppliers:** record purchase invoices; stock increases automatically
- **Customers:** customer list and purchase history
- **Dashboard & reports:** daily sales, top products, sales by date/cashier/payment method, export
- **Settings:** store info, currency, tax rate, database backup

## Architecture (3-tier)

```
POS_Presentation  (Presentation)    →  WinForms screens
POS_Business      (Business Logic)  →  rules, calculations, validation, permissions
POS_Data          (Data Access)     →  ADO.NET + SQL Server
POS_Models                          →  shared entity classes
```

Dependency rule:

```
POS_Presentation → POS_Business → POS_Data → POS_Models
```

The presentation layer never talks to the data layer directly, and the data layer never knows about the UI. `POS_Models` is referenced by all three layers.

## Tech Stack

| Area | Technology |
|---|---|
| Language | C# |
| UI | Windows Forms |
| Data access | ADO.NET (`SqlConnection`, `SqlCommand`, `SqlDataReader`) |
| Database | Microsoft SQL Server |
| IDE | Visual Studio |
| Version control | Git & GitHub |

## Project Structure

```
.
├── .gitignore
├── README.md
├── docs/                          Requirements, ERD, workflow
├── Database/
│   ├── 01_schema.sql              Creates the database, tables, indexes
│   ├── 02_seed.sql                Initial data (roles, units, admin user)
│   └── 03_stored_procedures.sql
└── src/
    └── POSInventorySystem/
        ├── POSInventorySystem.sln
        ├── POS_Models/            Entity classes
        ├── POS_Data/              Repositories and DB helpers
        ├── POS_Business/          Services and validators
        └── POS_Presentation/      WinForms application (startup project)
```

## Database

14 tables: `roles`, `users`, `categories`, `units`, `products`, `customers`, `suppliers`, `sales`, `sale_items`, `payments`, `purchases`, `purchase_items`, `stock_movements`, `settings`.

Key rules:
- Stock can never go below zero
- Each sale line keeps the price at the time of sale
- A sale and its stock update run in a single transaction
- Records with history are deactivated, not deleted

## Getting Started

### Prerequisites
- Visual Studio 2022 (with the **.NET desktop development** workload)
- Microsoft SQL Server (Express is enough) and SQL Server Management Studio (SSMS)

### Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/<your-username>/<repo-name>.git
   cd <repo-name>
   ```

2. **Create the database**
   Open SSMS and run, in order:
   1. `Database/01_schema.sql`
   2. `Database/02_seed.sql`
   3. `Database/03_stored_procedures.sql` (when available)

3. **Configure the connection string**
   Open `src/POSInventorySystem/POS_Presentation/App.config` and update it for your server:
   ```xml
   <connectionStrings>
     <add name="POSConnection"
          connectionString="Data Source=.\SQLEXPRESS;Initial Catalog=POS_Inventory;Integrated Security=True;TrustServerCertificate=True"
          providerName="System.Data.SqlClient" />
   </connectionStrings>
   ```
   Change `Data Source` to your SQL Server instance name.

4. **Build and run**
   Open `src/POSInventorySystem/POSInventorySystem.sln` in Visual Studio, set `POS_Presentation` as the startup project, then press **F5**.

### Default login
| Username | Password |
|---|---|
| admin | Set during setup (change it on first login) |

> The seed script inserts a placeholder password hash. Generate a real hash from the application and update the admin row before first use.

## Git Workflow

- `main`: stable releases only
- `develop`: integration branch
- `feature/<name>`: one branch per feature, merged into `develop` through a Pull Request

Commit message style:
```
feat: add ProductRepository with CRUD methods
fix: prevent negative stock on sale
docs: update database requirements
```

## Roadmap

- [ ] Database scripts (schema, seed, stored procedures)
- [ ] Models and data layer repositories
- [ ] Business layer services (auth, sales, purchases, inventory)
- [ ] Login and main form
- [ ] POS screen
- [ ] Product, customer, supplier, and user management screens
- [ ] Inventory and purchases screens
- [ ] Dashboard and reports
- [ ] Testing and bug fixes
- [ ] Final documentation and demo

## Team

| Member | Responsibility |
|---|---|
| _Name_ | Data tier (database and POS_Data) |
| _Name_ | Business logic tier (POS_Business) |
| _Name_ | Presentation tier (POS_Presentation) |

## License

This project is for educational purposes.
