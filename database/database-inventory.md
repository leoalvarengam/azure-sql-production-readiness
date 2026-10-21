# Database Technical Inventory 

This document outlines the structural analysis and catalog information for the `AdventureWorksLT` sample database based on catalog view queries.

## 1. Overview
- **Database Engine:** Azure SQL Database (vCore Serverless)
- **Primary Schema:** `SalesLT`
- **Total Tables:** 10 core tables (excluding system/versioning tables)

## 2. Core Business Domains & Row Counts
| Table Name | Description | Row Count |
| :--- | :--- | :--- |
| `SalesLT.Customer` | Client profiles and contact details | 847 |
| `SalesLT.ProductDescription` | Descriptions of products in various languages | 762 |
| `SalesLT.ProductModelProductDescription` | Cross-reference table linking models, descriptions, and culture | 762 |
| `SalesLT.SalesOrderDetail` | Line items for customer orders | 542 |
| `SalesLT.Address` | Shipping and billing addresses | 450 |
| `SalesLT.CustomerAddress` | Cross-reference table linking customers to addresses | 417 |
| `SalesLT.Product` | Inventory of goods/products | 295 |
| `SalesLT.ProductModel` | High-level product models | 128 |
| `SalesLT.ProductCategory` | Hierarchical product categories | 41 |
| `SalesLT.SalesOrderHeader` | Order headers and header-level metadata | 32 |

## 3. Foreign Key Relationships
The schema enforces relational integrity using the following foreign key constraints:

- **`SalesLT.CustomerAddress`**
  - `FK_CustomerAddress_Customer_CustomerID`: `CustomerID` -> `SalesLT.Customer(CustomerID)`
  - `FK_CustomerAddress_Address_AddressID`: `AddressID` -> `SalesLT.Address(AddressID)`

- **`SalesLT.Product`**
  - `FK_Product_ProductCategory_ProductCategoryID`: `ProductCategoryID` -> `SalesLT.ProductCategory(ProductCategoryID)`
  - `FK_Product_ProductModel_ProductModelID`: `ProductModelID` -> `SalesLT.ProductModel(ProductModelID)`

- **`SalesLT.ProductCategory`**
  - `FK_ProductCategory_ProductCategory_ParentProductCategoryID`: `ParentProductCategoryID` -> `SalesLT.ProductCategory(ProductCategoryID)` (Self-referencing for hierarchy)

- **`SalesLT.ProductModelProductDescription`**
  - `FK_ProductModelProductDescription_ProductModel_ProductModelID`: `ProductModelID` -> `SalesLT.ProductModel(ProductModelID)`
  - `FK_ProductModelProductDescription_ProductDescription_ProductDescriptionID`: `ProductDescriptionID` -> `SalesLT.ProductDescription(ProductDescriptionID)`

- **`SalesLT.SalesOrderDetail`**
  - `FK_SalesOrderDetail_SalesOrderHeader_SalesOrderID`: `SalesOrderID` -> `SalesLT.SalesOrderHeader(SalesOrderID)`
  - `FK_SalesOrderDetail_Product_ProductID`: `ProductID` -> `SalesLT.Product(ProductID)`

- **`SalesLT.SalesOrderHeader`**
  - `FK_SalesOrderHeader_Customer_CustomerID`: `CustomerID` -> `SalesLT.Customer(CustomerID)`
  - `FK_SalesOrderHeader_Address_ShipTo_AddressID`: `ShipToAddressID` -> `SalesLT.Address(AddressID)`
  - `FK_SalesOrderHeader_Address_BillTo_AddressID`: `BillToAddressID` -> `SalesLT.Address(AddressID)`