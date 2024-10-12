Explain a scenario where you have used Liskov substitution principle in your application?

A practical scenario where I applied the **Liskov Substitution Principle (LSP)** involved designing a system that processed different types of bank accounts: savings accounts and fixed deposit accounts. Both types of accounts inherited from a base class, but at one point, the design violated LSP, and we had to refactor the code.

### Initial Design (LSP Violation)

We started with a base class `BankAccount` and derived two subclasses: `SavingsAccount` and `FixedDepositAccount`.

#### Base Class

```java
public class BankAccount {
    protected double balance;

    public void deposit(double amount) {
        balance += amount;
    }

    public void withdraw(double amount) {
        balance -= amount;
    }

    public double getBalance() {
        return balance;
    }
}
```

#### Subclasses

```java
public class SavingsAccount extends BankAccount {
    // Savings accounts allow both deposits and withdrawals
}

public class FixedDepositAccount extends BankAccount {
    // Fixed deposit accounts do not allow withdrawals
    @Override
    public void withdraw(double amount) {
        throw new UnsupportedOperationException("Withdrawals are not allowed on fixed deposit accounts.");
    }
}
```

### The Problem

In the above design, we violated the **Liskov Substitution Principle (LSP)**. According to LSP, the subclass (`FixedDepositAccount`) should be substitutable for the base class (`BankAccount`) without affecting the functionality of the system. However, the `FixedDepositAccount`'s `withdraw()` method throws an exception, which breaks the expectation that `withdraw()` should always work for any `BankAccount`.

If we wrote a function to handle bank accounts like this:

```java
public void processWithdrawal(BankAccount account, double amount) {
    account.withdraw(amount);
}
```

Passing a `FixedDepositAccount` would cause the application to throw an `UnsupportedOperationException`, which violates LSP because the `FixedDepositAccount` cannot behave like a proper `BankAccount` in this context.

### Refactor to Fix LSP Violation

To fix the LSP violation, I introduced a common interface that clearly distinguished the behavior of accounts that allow withdrawals from those that don't.

#### Step 1: Create an Interface

I defined an interface `WithdrawableAccount` for accounts that can perform withdrawals.

```java
public interface WithdrawableAccount {
    void withdraw(double amount);
}
```

#### Step 2: Update the Classes

- The `BankAccount` class remained the base class for all accounts, handling common behavior like deposits.
- The `SavingsAccount` class implemented `WithdrawableAccount`, as it allows withdrawals.
- The `FixedDepositAccount` class no longer overrides the `withdraw()` method because it shouldn't have the method in the first place.

```java
public class BankAccount {
    protected double balance;

    public void deposit(double amount) {
        balance += amount;
    }

    public double getBalance() {
        return balance;
    }
}

public class SavingsAccount extends BankAccount implements WithdrawableAccount {
    @Override
    public void withdraw(double amount) {
        balance -= amount;
    }
}

public class FixedDepositAccount extends BankAccount {
    // No withdraw method because withdrawals are not allowed
}
```

### Step 3: Updated Usage

Now, we can safely use `SavingsAccount` wherever withdrawals are needed, and `FixedDepositAccount` is still a valid `BankAccount` without breaking LSP.

```java
public void processWithdrawal(WithdrawableAccount account, double amount) {
    account.withdraw(amount);
}

// Safe to pass only SavingsAccount objects
WithdrawableAccount savings = new SavingsAccount();
processWithdrawal(savings, 100.0);

// No longer possible to pass FixedDepositAccount to processWithdrawal
BankAccount fixedDeposit = new FixedDepositAccount();
// This avoids any incorrect behavior or exceptions
```

### Result

By refactoring the design:

- We made sure that the `FixedDepositAccount` could still be substituted as a `BankAccount` without breaking functionality.
- Withdrawals were restricted to accounts that were supposed to support them (`SavingsAccount`), and our system became more robust and correct.

This approach follows LSP because now all derived classes (`SavingsAccount`, `FixedDepositAccount`) behave consistently with their base class, and there's no unexpected behavior (like exceptions) that violates the substitutability of the base type.
