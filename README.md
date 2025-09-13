# Banking-System
#include <iostream>
#include <fstream>
#include <vector>
#include <iomanip>

using namespace std;

class Account {
public:
    int accountNumber;
    string name;
    double balance;

    void createAccount() {
        cout << "Enter Account Number: ";
        cin >> accountNumber;
        cin.ignore();
        cout << "Enter Account Holder Name: ";
        getline(cin, name);
        cout << "Enter Initial Balance: ";
        cin >> balance;
        cout << "Account created successfully!\n";
    }

    void showAccount() const {
        cout << "Account Number: " << accountNumber << endl;
        cout << "Account Holder: " << name << endl;
        cout << "Balance: $" << fixed << setprecision(2) << balance << endl;
    }

    void deposit(double amount) {
        balance += amount;
        cout << "Amount deposited successfully!\n";
    }

    bool withdraw(double amount) {
        if (balance >= amount) {
            balance -= amount;
            cout << "Amount withdrawn successfully!\n";
            return true;
        } else {
            cout << "Insufficient balance.\n";
            return false;
        }
    }

    int getAccountNumber() const {
        return accountNumber;
    }
};

// Global vector to hold accounts
vector<Account> accounts;

// Function declarations
void createAccount();
void depositMoney();
void withdrawMoney();
void checkBalance();
void showAllAccounts();
int findAccount(int accNum);

int main() {
    int choice;

    do {
        cout << "\n===== BANKING SYSTEM MENU =====\n";
        cout << "1. Create Account\n";
        cout << "2. Deposit Money\n";
        cout << "3. Withdraw Money\n";
        cout << "4. Check Balance\n";
        cout << "5. Display All Accounts\n";
        cout << "6. Exit\n";
        cout << "Select Your Option (1-6): ";
        cin >> choice;

        switch (choice) {
        case 1: createAccount(); break;
        case 2: depositMoney(); break;
        case 3: withdrawMoney(); break;
        case 4: checkBalance(); break;
        case 5: showAllAccounts(); break;
        case 6: cout << "Exiting...\n"; break;
        default: cout << "Invalid choice. Try again.\n";
        }

    } while (choice != 6);

    return 0;
}

void createAccount() {
    Account acc;
    acc.createAccount();
    accounts.push_back(acc);
}

void depositMoney() {
    int accNum;
    double amount;
    cout << "Enter Account Number: ";
    cin >> accNum;
    int index = findAccount(accNum);
    if (index != -1) {
        cout << "Enter Amount to Deposit: ";
        cin >> amount;
        accounts[index].deposit(amount);
    } else {
        cout << "Account not found.\n";
    }
}

void withdrawMoney() {
    int accNum;
    double amount;
    cout << "Enter Account Number: ";
    cin >> accNum;
    int index = findAccount(accNum);
    if (index != -1) {
        cout << "Enter Amount to Withdraw: ";
        cin >> amount;
        accounts[index].withdraw(amount);
    } else {
        cout << "Account not found.\n";
    }
}

void checkBalance() {
    int accNum;
    cout << "Enter Account Number: ";
    cin >> accNum;
    int index = findAccount(accNum);
    if (index != -1) {
        accounts[index].showAccount();
    } else {
        cout << "Account not found.\n";
    }
}

void showAllAccounts() {
    if (accounts.empty()) {
        cout << "No accounts to display.\n";
    } else {
        cout << "\n--- All Bank Accounts ---\n";
        for (const auto& acc : accounts) {
            acc.showAccount();
            cout << "-------------------------\n";
        }
    }
}

int findAccount(int accNum) {
    for (int i = 0; i < accounts.size(); ++i) {
        if (accounts[i].getAccountNumber() == accNum) {
            return i;
        }
    }
    return -1;
}
