
```markdown
Delete va Truncate difference
```
Delete removes specific rows while truncate removes all rows from a table.
Delete is a DML command while truncate is a DDL command.
Delete is slower than truncate as it logs each row deletion while truncate does not.
Delete can be rolled back while truncate cannot be rolled back.
Delete can have a WHERE clause to specify which rows to delete while truncate removes all rows.
Delete does not reset the identity of the table while truncate resets the identity of the table.