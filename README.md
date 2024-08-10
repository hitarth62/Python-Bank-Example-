Here's a detailed description of the provided Python code for an ATM system. This code uses the pyttsx3 library for text-to-speech functionality and provides a simple command-line interface for managing user accounts, transactions, and other banking operations.

Overview
The ATM system allows users to create an account, log in, perform transactions, check balances, and manage their account details. It uses a text-to-speech engine to provide auditory feedback and instructions to users.

Code Breakdown
1. Imports and Initialization
python
Copy code
import pyttsx3
import os
engine = pyttsx3.init()
pyttsx3: A library used for text-to-speech conversion.
os: Provides functions to interact with the operating system (e.g., file operations).
The engine object initializes the text-to-speech engine.

2. Text-to-Speech Function
python
Copy code
def speek(ex):
    engine.say(ex)
    engine.runAndWait()
speek(ex): A function that takes a string ex, converts it to speech, and waits until the speech has been completed.
3. Welcome Message and Account Creation
python
Copy code
print("Welcome to ATM")
speek("\nDo you have account with us")

while True:
    account = input("Do you have account with us (y/n) :")
    if account.lower() == "n":
        print("Ok , Let's create one")
        while True:
            speek("Enter Username")
            CUsername = input("Enter Username :")
            speek("Enter Password")
            Cpassword = input("Enter Password :")
            
            if CUsername == "" or Cpassword == "":
                print("invalid username or password")
            else:
                with open(f"D:/BANK DATA/DATA/ACCOUNT/ACCOUNT_NO_{CUsername}.txt", "w") as f:
                    f.write(f"0\n{Cpassword}")
                print(f"Congratulation {CUsername} Your Account has been created")    
                break  
        break  

    elif account.lower() == "y":
        break
    else:
        speek("Invalid output")
        print("Invalid output")
    break
Greeting: Prints and speaks a welcome message.
Account Check: Asks if the user has an account. If not, it prompts for account creation.
Account Creation:
Prompts for a username and password.
Validates that neither is empty.
Saves the account details in a file named with the username.
Initializes the balance as 0 and stores the password.
4. User Authentication
python
Copy code
while True:
    speek("Enter Your Username")
    username = input("Enter Your Username :")
    speek("Enter Your Password")
    password = input("Enter Your Password :")
    if username == "" or password == "":
        speek("Invalid username or password")
        print("Invalid username or password")
    else:
        with open(f"D:/BANK DATA/DATA/ACCOUNT/ACCOUNT_NO_{username}.txt", "r") as f:
            data = f.read().split("\n")
        if password == data[1]:
            speek(f"Welcome {username}")
            print(f"Welcome {username}")
            break
        else:
            speek("Invalid username or password")
            print("Invalid username or password")
User Login:
Prompts for username and password.
Reads the corresponding account file to check credentials.
Confirms successful login or displays an error if credentials are incorrect.
5. Main ATM Operations
python
Copy code
while True:
    with open(f"D:/BANK DATA/DATA/ACCOUNT/ACCOUNT_NO_{username}.txt", "r") as f:
        data1 = f.read().split("\n")
    balance = data1[0]
    speek("Welcome to ATM")
    speek("please select the option")
    print("ATM")
    print("1. Deposit")
    print("2. Withdraw")
    print("3. Check Balance")
    print("4. Exit")
    print("5. other")
    option = input("Enter Your Option :")
    if option == "1":
        speek("Enter the amount to deposit")
        print("Enter the amount to deposit")
        amount = int(input("Enter the amount to deposit :"))
        with open(f"D:/BANK DATA/DATA/ACCOUNT/ACCOUNT_NO_{username}.txt", "w") as f:
            f.write(f"{int(balance) + amount}\n{password}")
        print("Amount Deposited to Your account")
        speek("Amount Deposited to Your account")
    elif option == "2":
        speek("Enter the amount to withdraw")
        print("Enter the amount to withdraw")
        amount = int(input("Enter the amount to withdraw :"))
        if amount > int(balance):
            print("Insufficient Balance")
            speek("Insufficient Balance")
        else:
            with open(f"D:/BANK DATA/DATA/ACCOUNT/ACCOUNT_NO_{username}.txt", "w") as f:
                f.write(f"{int(balance) - amount}\n{password}")
            print("Amount Withdrawn from Your account")
            speek("Amount Withdrawn from Your account")
    elif option == "3":
        print(f"Your Balance is {balance}")
        speek(f"Your Balance is {balance}")
    elif option == "4":
        speek("Thank You for using ATM")
        print("Thank You for using ATM")
        break
    elif option == "5":
        while True:
            print("ATM menu")
            speek("ATM menu")
            print("1. Change Password")
            print("2. Transfer Money")
            print("3. Delete Account")
            print("4. Exit")
            option1 = input("Enter Your Option :")
            if option1 == "1":
                speek("Enter the new password")
                print("Enter the new password")
                password = input("Enter the new password :")
                with open(f"D:/BANK DATA/DATA/ACCOUNT/ACCOUNT_NO_{username}.txt", "w") as f:
                    f.write(f"{balance}\n{password}")
                print("Password Changed Successfully")
                speek("Password Changed Successfully")
                break
            elif option1 == "2":
                speek("Enter the account number")
                print("Enter the account number")
                account_number = input("Enter the account number :")
                with open(f"D:/BANK DATA/DATA/ACCOUNT/ACCOUNT_NO_{account_number}.txt", "r") as f:
                    balance1 = f.readline()
                if balance1 == "":
                    print("Account Number Does not Exist")
                    speek("Account Number Does not Exist")
                    break
                else:
                    speek("Enter the amount to transfer")
                    print("Enter the amount to transfer")
                    amount = int(input("Enter the amount to transfer :"))
                    if amount > int(balance):
                        print("Insufficient Balance")
                        speek("Insufficient Balance")
                        break
                    else:
                        with open(f"D:/BANK DATA/DATA/ACCOUNT/ACCOUNT_NO_{username}.txt", "w") as f:
                            f.write(f"{int(balance) - amount}\n{password}")
                        with open(f"D:/BANK DATA/DATA/ACCOUNT/ACCOUNT_NO_{account_number}.txt", "r") as f:
                            balance1 = f.readline()
                        with open(f"D:/BANK DATA/DATA/ACCOUNT/ACCOUNT_NO_{account_number}.txt", "w") as f:
                            f.write(f"{int(balance1) + amount}\n{password}")
                        print("Amount Transferred Successfully")
                        speek("Amount Transferred Successfully")
                        break
            elif option1 == "3":
                speek("Are you sure you want to delete your account")
                print("Are you sure you want to delete your account (y/n)")
                option2 = input("Enter Your Option :")
                if option2.lower() == "y":
                    os.remove(f"D:/BANK DATA/DATA/ACCOUNT/ACCOUNT_NO_{username}.txt")
                    print("Account Deleted Successfully")
                    speek("Account Deleted Successfully")
                    exit()
                elif option2.lower() == "n":
                    print("Account Not Deleted")
                    speek("Account Not Deleted")
                    break
            elif option1 == "4":
                print("Thank You For Using Our Services")
                speek("Thank You For Using Our Services")
                break
            else: 
                print("Invalid Option") 
                speek("Invalid Option")
    else:
        speek("Invalid Option")
        print("Invalid Option")
Main Menu:
Deposit: Prompts for the deposit amount, updates the balance, and saves it.
Withdraw: Checks for sufficient balance, updates the balance, and saves it.
Check Balance: Displays the current balance.
Additional Options:
Change Password: Allows the user to update their password.
Transfer Money: Transfers money to another account after verifying the account number and ensuring sufficient balance.
Delete Account: Prompts for confirmation and deletes the user’s account if confirmed.
Exit: Ends the session and thanks the user.
Error Handling and User Prompts
Invalid Inputs: Provides feedback for invalid inputs and ensures the correct flow of operations.
File Operations: Handles file reading and writing for account data.
Summary
This ATM system is a basic Python script that demonstrates user account management, transactions, and basic ATM functionalities. It provides auditory feedback using text-to-speech and handles several



