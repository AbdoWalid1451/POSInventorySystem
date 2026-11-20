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


## License

This project is for educational purposes.
