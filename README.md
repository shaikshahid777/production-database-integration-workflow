<!-- SHOWCASE_START --><div align="center">[![Typing](https://readme-typing-svg.demolab.com?font=Fira+Code&weight=700&size=22&duration=2800&pause=900&color=58A6FF&center=true&vCenter=true&width=900&lines=production%20database%20integration%20workflow;AI%20%7C%20Automation%20%7C%20Engineering;Explore%20the%20project%20%F0%9F%9A%80)](https://github.com/shaikshahid777/production-database-integration-workflow)<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0D1117,50:161B22,100:58A6FF&height=110&section=header&text=production-database-integration-workflow&fontSize=26&fontColor=FFFFFF&animation=twinkling&fontAlignY=65" width="100%" alt="Animated project banner"/>

[![Repository](https://img.shields.io/badge/Repository-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/shaikshahid777/production-database-integration-workflow) [![Issues](https://img.shields.io/badge/Report-Issue-red?style=for-the-badge&logo=github)](https://github.com/shaikshahid777/production-database-integration-workflow/issues/new) [![Stars](https://img.shields.io/github/stars/shaikshahid777/production-database-integration-workflow?style=for-the-badge&logo=github)](https://github.com/shaikshahid777/production-database-integration-workflow/stargazers) [![Fork](https://img.shields.io/github/forks/shaikshahid777/production-database-integration-workflow?style=for-the-badge&logo=github)](https://github.com/shaikshahid777/production-database-integration-workflow/fork) [![Profile](https://img.shields.io/badge/Profile-Visit-0A66C2?style=for-the-badge&logo=github)](https://github.com/shaikshahid777)</div>

> ✨ **Project Showcase Mode:** animated banner • interactive navigation • live repository actions

[🚀 Repository](https://github.com/shaikshahid777/production-database-integration-workflow) · [🐞 Report Issue](https://github.com/shaikshahid777/production-database-integration-workflow/issues/new) · [⭐ Star](https://github.com/shaikshahid777/production-database-integration-workflow/stargazers) · [🔱 Fork](https://github.com/shaikshahid777/production-database-integration-workflow/fork) · [👤 Profile](https://github.com/shaikshahid777)

<!-- SHOWCASE_END -->

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
