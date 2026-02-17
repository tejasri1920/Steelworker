## lots
- lot_id (Primary Key)
- start_date
- end_date

## production_records
- production_id (Primary Key)
- lot_id (Foreign Key)
- production_date
- production_line
- quantity_produced

## inspection_records
- inspection_id (Primary Key)
- lot_id (Foreign Key)
- inspection_date
- inspection_result
- issue_flag (Boolean)

## shipping_records
- shipping_id (Primary Key)
- lot_id (Foreign Key)
- ship_date
- shipment_status
- destination

## data_completeness
- lot_id (Primary Key / Foreign Key)
- has_production_data (Boolean)
- has_inspection_data (Boolean)
- has_shipping_data (Boolean)
- overall_completeness

## Relationships
- One lots can have many production_records.
- One lots can have many inspection_records.
- One lots can have many shipping_records.
- One lots has one data_completeness.

## ERD
```mermaid
erDiagram
    lots ||--o{ production_records : tracks
    lots ||--o{ inspection_records : verified_by
    lots ||--o{ shipping_records : delivered_via
    lots ||--|| data_completeness : audit_summary

    lots {
        int lot_id PK
        datetime start_date
        datetime end_date
    }

    production_records {
        int production_id PK
        int lot_id FK
        datetime production_date
        string production_line
        int quantity_produced
    }

    inspection_records {
        int inspection_id PK
        int lot_id FK
        datetime inspection_date
        string inspection_result
        boolean issue_flag
    }

    shipping_records {
        int shipping_id PK
        int lot_id FK
        datetime ship_date
        string shipment_status
        string destination
    }

    data_completeness {
        int lot_id PK, FK
        boolean has_production_data
        boolean has_inspection_data
        boolean has_shipping_data
        float overall_completeness
    }
```
