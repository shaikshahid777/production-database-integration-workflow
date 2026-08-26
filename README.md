# Production Database Integration Workflow — Lesson 5 Assessment

Production-oriented n8n workflow using PostgreSQL for pending order-lead retrieval, transactional locking, order processing, customer upsert, and final status management.

## Workflow Architecture

```text
Manual Trigger
      ↓
SQL_FetchAndLockRow
      ↓
ACTION_ProcessOrder
      ↓
SQL_UpdateStatusCompleted
      ↓
SQL_UpsertCustomer
```

## Database Schema

### customers
- `id` — primary key
- `email` — unique customer key
- `name` — customer name

### order_leads
- `id` — primary key
- `customer_email` — foreign key to customers(email)
- `customer_name`
- `product`
- `amount`
- `lead_status`

## Processing Logic

1. Fetch one pending order lead.
2. Use PostgreSQL `FOR UPDATE SKIP LOCKED` to avoid duplicate concurrent processing.
3. Mark/process the selected lead.
4. Run the order-processing action.
5. Mark the lead as completed after successful processing.
6. Upsert the customer using email as the unique key.

## Upsert Logic

The customer operation uses `ON CONFLICT (email) DO UPDATE` so an existing customer is updated rather than duplicated.

## Files

```text
production-database-integration-workflow/
├── README.md
├── workflow/
│   ├── Lesson_5_Order_Lead_DB_Integration.json
│   └── Lesson_5_DB_Setup_Run_Once.json
├── documentation/
│   └── Lesson_5_Production_Database_Integration_Documentation.pdf
├── screenshots/
└── tests/
```

## Security

Do not commit database passwords, connection strings containing credentials, private keys, or other secrets. PostgreSQL credentials should remain in n8n Credential Manager.

## Reproduction

1. Configure the PostgreSQL credential in n8n.
2. Run the one-time schema/seed workflow.
3. Execute the main order-lead workflow.
4. Verify the lead moves from pending to processing/completed and the customer upsert executes.
5. Capture execution evidence for the LMS submission.
