
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

```markdown
What is difference between procedure and function
```
## Comparison of Procedures and Functions in SQL

| Feature | Procedure | Function |
|---|---|---|
| **Purpose** | Performs a specific task. | Performs a task and returns a value. |
| **Return Value** | Does not return a value. | Returns a value. |
| **Usage in Expressions** | Cannot be used in expressions. | Can be used in expressions. |
| **Parameters/Arguments** | Can have both parameters and arguments. | Can have parameters. |
| **Examples** | Printing to the console. | Calculating a sum, finding the length of a string. |


