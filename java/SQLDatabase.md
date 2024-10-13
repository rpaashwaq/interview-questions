
```markdown
Delete vs Truncate difference
```
## Comparison of DELETE and TRUNCATE in SQL

| Feature | DELETE | TRUNCATE |
|---|---|---|
| **Purpose** | Removes specific rows from a table. | Removes all rows from a table. |
| **Type of Command** | DML (Data Manipulation Language) | DDL (Data Definition Language) |
| **Speed** | Slower due to logging of individual row deletions. | Faster due to bulk removal. |
| **Rollback Capability** | Can be rolled back. | Cannot be rolled back. |
| **Row Specificity** | Can use a WHERE clause to specify rows. | Removes all rows. |
| **Identity Column** | Does not reset the identity column. | Resets the identity column. |
