# Sales Order Management App - SAP ABAP RAP

## Overview
This repository contains an end-to-end Sales Order Management application built using the **ABAP RESTful Application Programming Model (RAP)** on the **SAP BTP ABAP Environment (Steampunk)**. The application exposes an OData V4 service consumed by an SAP Fiori Elements frontend, complete with draft handling and complex transactional business logic.

## Architecture
The application follows a strict Managed RAP architecture:
*   **Database Layer:** Custom persistent and draft database tables for Sales Order Header and Items utilizing `sysuuid_x16` keys.
*   **Data Model Layer:** Core Data Services (CDS) Interface and Projection views establishing parent-child composition.
*   **Business Logic Layer:** Behavior Definitions (BDEF) and ABAP Behavior Implementation classes (ABAP OO) managing CRUD operations, validations, and determinations.
*   **Service Layer:** Service Definition and OData V4 UI Service Binding.
*   **UI Layer:** SAP Fiori Elements List Report and Object Page driven by CDS `@UI` metadata annotations.

## Key Features
*   **Draft Capabilities:** Full stateful draft handling (`with draft`), allowing users to save incomplete work and resume later before activating the data to the persistent tables.
*   **Automated Determinations:** Backend ABAP logic (`calculateNetAmount`) that automatically triggers on save to aggregate line item totals (Quantity × Net Price) and updates the header-level Net Amount.
*   **Strict Validations:** Business validation rules (`validateQuantity`, `validateItemQuantity`) preventing the activation of orders with a Net Amount or Quantity of zero or less.
*   **Defaulting Logic:** Automatic status assignment (`setInitialStatus`) to 'New' upon creation.
*   **Fiori Elements UI:** Fully annotated CDS Projection views dictating UI facets, line items, and identification references for a seamless user experience.

## Technical Implementation Details
### Object Directory
*   `ZSOH` / `ZSOI` - Persistent Database Tables (Header & Item)
*   `ZSOH_D` / `ZSOI_D` - Auto-generated Draft Tables
*   `ZISO` / `ZISOI` - CDS Interface Views (Data Definition)
*   `ZCSO` / `ZCSOI` - CDS Projection Views (Consumption / UI Definition)
*   `ZISO` / `ZCSO` - Behavior Definitions (Managed / Projection)
*   `ZBP_ZISO` - Behavior Implementation Class (Local Handler)
*   `ZSD_SALESORDER` - Service Definition
*   `ZSB_SALESORDER` - Service Binding (OData V4)

### Development Tools Used
*   Eclipse with ABAP Development Tools (ADT)
*   SAP BTP ABAP Environment (Trial)
*   abapGit for version control

## Deployment & Testing
1. Clone this repository into an SAP BTP ABAP environment using the **abapGit** Eclipse plugin.
2. Activate all objects starting from the Data Dictionary up to the Service Binding.
3. Open the `ZSB_SALESORDER` service binding and click **Publish**.
4. Select the `SalesOrder` entity and click **Preview** to launch the SAP Fiori Elements sandbox environment.

---
**Author:** Vidhya Sai Kothakota
