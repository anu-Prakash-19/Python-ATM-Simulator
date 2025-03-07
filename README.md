# Python-ATM-Simulator
Python ATM Simulator
import datetime

actual_pin = None  # Global variable for storing PIN
exit_program = False
inserted = False
balance = 1000
transactions = []  # List to store transaction history

while not exit_program:
    print("Welcome to SBI")
    
    if not inserted:
        try:
            a = int(input("Enter the ATM card\n1 - Yes\n2 - No\n"))
            if a != 1:
                print("Please insert your ATM card to proceed.")
                continue
        except ValueError:
            print("Invalid input. Please enter a number (1 or 2).")
            continue

        if not actual_pin:
            actual_pin = input("Set your actual PIN: ")
        
        pin = input("Enter your PIN: ")
        
        if pin == actual_pin:
            while True:
                print('''
                1. Deposit
                2. Withdrawal 
                3. Mini Statement
                4. PIN Change
                5. Exit
                ''')
                
                try:
                    option = int(input("Choose an option: "))
                except ValueError:
                    print("Invalid input. Please enter a number between 1 and 5.")
                    continue

                current_time = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")  # Get current date and time

                if option == 1:  # Deposit
                    while True:
                        try:
                            amount = int(input("Enter deposit amount: "))
                            if amount % 100 == 0:
                                balance += amount
                                transactions.append(f"{current_time} | Deposited: ₹{amount}")
                                print("Cash accepted successfully.")
                                break
                            else:
                                print("Please enter an amount in multiples of 100.")
                        except ValueError:
                            print("Invalid input. Please enter a valid numeric amount.")

                elif option == 2:  # Withdrawal
                    try:
                        amount = int(input("Enter withdrawal amount: "))
                        if amount % 100 == 0:
                            if amount <= balance:
                                balance -= amount
                                transactions.append(f"{current_time} | Withdrawn: ₹{amount}")
                                print("Please collect your cash.")
                            else:
                                print("Insufficient balance.")
                        else:
                            print("Enter only multiples of 100.")
                    except ValueError:
                        print("Invalid input. Please enter a valid numeric amount.")

                elif option == 3:  # Mini Statement
                    print("\n===== Mini Statement =====")
                    print("State Bank of India")
                    print(f"Date: {datetime.datetime.now().strftime('%Y-%m-%d')}  Time: {datetime.datetime.now().strftime('%H:%M:%S')}")
                    print("Available Balance: ₹", balance)
                    print("\nRecent Transactions:")
                    for transaction in transactions[-5:]:  # Show last 5 transactions
                        print(transaction)
                    print("\n")
                elif option == 4:  # PIN Change
                    new_pin = input("Enter new PIN: ")
                    actual_pin = new_pin
                    print("PIN changed successfully.")

                elif option == 5:  # Exit
                    print("Thank you for using SBI ATM!")
                    exit_program = True
                    break

                else:
                    print("Invalid option. Please choose between 1 and 5.")
        else:
            print("Incorrect PIN. Try again.")


                
