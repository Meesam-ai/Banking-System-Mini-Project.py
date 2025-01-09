# Banking-System-Mini-Project.py
This project includes functionalities for users to perform banking operations and for the bank (admin) to manage accounts and view financial statistics.
class BankAccount:
    def __init__(self, account_number, account_holder):
        self.account_number = account_number
        self.account_holder = account_holder
        self.balance = 0.0
        self.transactions = []

    def deposit(self, amount):
        if amount > 0:
            self.balance += amount
            self.add_transaction(f"Deposited: {amount}")
        else:
            print("Zero amount Deposited.")

    def withdraw(self, amount):
        if 0 < amount <= self.balance:
            self.balance -= amount
            self.add_transaction(f"Withdrew: {amount}")
        else:
            print("Zero amount Withdrewn.")

    def check_balance(self):
        return self.balance

    def add_transaction(self, description):
        self.transactions.append(description)

    def print_statement(self):
        print(f"Transaction Statement for Account {self.account_number}:")
        for transaction in self.transactions:
            print(transaction)


class Bank:
    def __init__(self):
        self.accounts = {}

    def open_account(self, account_holder):
        account_number = len(self.accounts) + 1
        new_account = BankAccount(account_number, account_holder)
        self.accounts[account_number] = new_account
        print(f"Account created successfully! Account Number: {account_number}")

    def get_account(self, account_number):
        return self.accounts.get(account_number, None)

    def transfer(self, sender_account_number, receiver_account_number, amount):
        sender_account = self.get_account(sender_account_number)
        receiver_account = self.get_account(receiver_account_number)

        if sender_account and receiver_account:
            if sender_account.balance >= amount:
                sender_account.withdraw(amount)
                receiver_account.deposit(amount)
                print(f"Transferred {amount} from Account {sender_account_number} to Account {receiver_account_number}.")
            else:
                print("Insufficient amount unable to Transfer.")
        else:
            print("Invalid account number(s).")

    def admin_check_total_deposit(self):
        total_deposits = sum(account.balance for account in self.accounts.values())
        return total_deposits

    def admin_check_total_accounts(self):
        return len(self.accounts)



def main():
    bank = Bank()

    while True:
        print("\n--- Banking System ---")
        print("1. Open an Account")
        print("2. Deposit Money")
        print("3. Withdraw Money")
        print("4. Check Balance")
        print("5. Transfer Money")
        print("6. View Transaction Statement")
        print("7. Admin: View Total Deposits")
        print("8. Admin: View Total Accounts")
        print("9. Exit")

        choice = int(input("Enter a number: "))

        if choice == 1:
            name = input("Enter name of account holder: ")
            bank.open_account(name)

        elif choice == 2:
            account_number = int(input("Enter account number: "))
            account = bank.get_account(account_number)
            if account:
                amount = float(input("Enter amount to deposit: "))
                account.deposit(amount)
            else:
                print("Account not found.")

        elif choice == 3:
            account_number = int(input("Enter account number: "))
            account = bank.get_account(account_number)
            if account:
                amount = float(input("Enter amount to withdraw: "))
                account.withdraw(amount)
            else:
                print("Account not found.")

        elif choice == 4:
            account_number = int(input("Enter account number: "))
            account = bank.get_account(account_number)
            if account:
                print(f"Current Balance: {account.check_balance()}")
            else:
                print("Account not found.")

        elif choice == 5:
            sender = int(input("Enter sender's account number: "))
            receiver = int(input("Enter receiver's account number: "))
            amount = float(input("Enter amount to transfer: "))
            bank.transfer(sender, receiver, amount)

        elif choice == 6:
            account_number = int(input("Enter account number: "))
            account = bank.get_account(account_number)
            if account:
                account.print_statement()
            else:
                print("Account not found.")

        elif choice == 7:
            print(f"Total Deposits in Bank: {bank.admin_check_total_deposit()}")

        elif choice == 8:
            print(f"Total Number of Accounts: {bank.admin_check_total_accounts()}")

        elif choice == 9:
            print("Exiting. Goodbye!")
            break

        else:
            print("Invalid choice.")


if __name__ == "__main__":
    main()

