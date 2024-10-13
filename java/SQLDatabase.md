
```markdown
Delete va Truncate difference
```
Criteria,Delete,Truncate
Purpose,Removes specific rows,Removes all rows from a table
Command Type,DML (Data Manipulation Language),DDL (Data Definition Language)
Performance,Slower (logs each row deletion),Faster (does not log each row deletion)
Rollback Capability,Can be rolled back,Cannot be rolled back
Condition,Can have a WHERE clause to specify rows,Cannot have a WHERE clause (removes all rows)
Identity Reset,Does not reset table identity,Resets table identity

Delete removes specific rows while truncate removes all rows from a table.
Delete is a DML command while truncate is a DDL command.
Delete is slower than truncate as it logs each row deletion while truncate does not.
Delete can be rolled back while truncate cannot be rolled back.
Delete can have a WHERE clause to specify which rows to delete while truncate removes all rows.
Delete does not reset the identity of the table while truncate resets the identity of the table.