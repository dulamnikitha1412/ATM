# ATM

from random import randint

atm_users = {}

class ATM:
    def __init__(self, user_id):
        self.user_id = user_id
        self.balance = atm_users[user_id]["balance"]

    def check_balance(self):
        print(f"Your current balance is: ${self.balance:.2f}")

    def deposit(self, amount):
        if amount > 0:
            self.balance += amount
            atm_users[self.user_id]["balance"] = self.balance
            print(f"Successfully deposited ${amount:.2f}")
        else:
            print("Invalid deposit amount.")

    def withdraw(self, amount):
        if amount <= 0:
            print("Invalid withdrawal amount.")
        elif amount > self.balance:
            print("Insufficient balance.")
        else:
            self.balance -= amount
            atm_users[self.user_id]["balance"] = self.balance
            print(f"Successfully withdrew ${amount:.2f}")

    def menu(self):
        while True:
            print("====== ATM MENU ======")
            print("1. Check Balance")
            print("2. Deposit")
            print("3. Withdraw")
            print("4. Exit")

            choice = input("Enter your choice (1-4): ")

            if(choice == '1'):
                self.check_balance()
            elif(choice == '2'):
                try:
                    amount = float(input("Enter amount to deposit: $"))
                    self.deposit(amount)
                except ValueError:
                    print("Please enter a valid number.")
            elif(choice == '3'):
                try:
                    amount = float(input("Enter amount to withdraw: $"))
                    self.withdraw(amount)
                except ValueError:
                    print("Please enter a valid number.")
            elif(choice == '4'):
                print("Thank you for using our ATM. Have a nice day!")
                break
            else:
                print("Invalid choice. Please try again.")

def register_user():
    print("=== ATM User Registration ===")
    name = input("Enter your full name: ")
    pin = input("Set a 4-digit PIN: ")

    if not pin.isdigit() or len(pin) != 4:
        print("Invalid PIN. Must be exactly 4 digits.")
        return

    initial_deposit = input("Enter the initial deposit amount: ")

    try:
        balance = float(initial_deposit)
    except ValueError:
        print("Invalid deposit amount.")
        return

    user_id = randint(10000, 99999)
    while user_id in atm_users:
        user_id = randint(10000, 99999)

    atm_users[user_id] = {
        "name": name,
        "pin": pin,
        "balance": balance
    }

    print(f"User registered successfully! Your ATM ID is {user_id}")

def display_users():
    print("=== Registered Users ===")
    if not atm_users:
        print("No users registered yet.")
        return
    for user_id, info in atm_users.items():
        print(f"ID: {user_id}, Name: {info['name']}, Balance: ${info['balance']:.2f}")

def login_user():
    print("=== ATM Login ===")
    try:
        user_id = int(input("Enter your ATM ID: "))
        pin = input("Enter your 4-digit PIN: ")

        if user_id in atm_users and atm_users[user_id]["pin"] == pin:
            print(f"Welcome {atm_users[user_id]['name']}!")
            atm = ATM(user_id)
            atm.menu()
        else:
            print("Invalid ID or PIN.")
    except ValueError:
        print("Invalid input. Please enter numbers only for ID.")


while True:
    print("====== MAIN MENU ======")
    print("1. Register ATM User")
    print("2. View Registered Users")
    print("3. Use ATM")
    print("4. Exit")

    choice = input("Choose an option: ")

    if(choice == "1"):
        register_user()
    elif(choice == "2"):
        display_users()
    elif(choice == "3"):
        login_user()
    elif(choice == "4"):
        print("Exiting. Thank you!")
        break
    else:
        print("Invalid choice. Try again.")
