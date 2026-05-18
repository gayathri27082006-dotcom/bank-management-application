#include <iostream>
#include <fstream>
#include <string>

using namespace std;

class BankAccount
{
private:
    int accNo;
    string name;
    float balance;

public:
    void createAccount()
    {
        cout << "\nEnter Account Number: ";
        cin >> accNo;

        cin.ignore();

        cout << "Enter Customer Name: ";
        getline(cin, name);

        cout << "Enter Initial Balance: ";
        cin >> balance;

        ofstream file("bank.txt", ios::app);

        file << accNo << endl;
        file << name << endl;
        file << balance << endl;

        file.close();

        cout << "\nAccount Created Successfully!\n";
    }

    void displayAccount()
    {
        cout << "\nAccount Number : " << accNo;
        cout << "\nCustomer Name  : " << name;
        cout << "\nBalance        : " << balance << endl;
    }

    void deposit()
    {
        int searchAcc;
        float amount;
        bool found = false;

        cout << "\nEnter Account Number: ";
        cin >> searchAcc;

        ifstream file("bank.txt");
        ofstream temp("temp.txt");

        while (file >> accNo)
        {
            file.ignore();
            getline(file, name);
            file >> balance;

            if (accNo == searchAcc)
            {
                cout << "Enter Deposit Amount: ";
                cin >> amount;

                balance += amount;

                cout << "\nAmount Deposited Successfully!\n";
                found = true;
            }

            temp << accNo << endl;
            temp << name << endl;
            temp << balance << endl;
        }

        file.close();
        temp.close();

        remove("bank.txt");
        rename("temp.txt", "bank.txt");

        if (!found)
        {
            cout << "\nAccount Not Found!\n";
        }
    }

    void withdraw()
    {
        int searchAcc;
        float amount;
        bool found = false;

        cout << "\nEnter Account Number: ";
        cin >> searchAcc;

        ifstream file("bank.txt");
        ofstream temp("temp.txt");

        while (file >> accNo)
        {
            file.ignore();
            getline(file, name);
            file >> balance;

            if (accNo == searchAcc)
            {
                cout << "Enter Withdrawal Amount: ";
                cin >> amount;

                if (amount <= balance)
                {
                    balance -= amount;
                    cout << "\nWithdrawal Successful!\n";
                }
                else
                {
                    cout << "\nInsufficient Balance!\n";
                }

                found = true;
            }

            temp << accNo << endl;
            temp << name << endl;
            temp << balance << endl;
        }

        file.close();
        temp.close();

        remove("bank.txt");
        rename("temp.txt", "bank.txt");

        if (!found)
        {
            cout << "\nAccount Not Found!\n";
        }
    }

    void checkBalance()
    {
        int searchAcc;
        bool found = false;

        cout << "\nEnter Account Number: ";
        cin >> searchAcc;

        ifstream file("bank.txt");

        while (file >> accNo)
        {
            file.ignore();
            getline(file, name);
            file >> balance;

            if (accNo == searchAcc)
            {
                displayAccount();
                found = true;
                break;
            }
        }

        file.close();

        if (!found)
        {
            cout << "\nAccount Not Found!\n";
        }
    }
};

int main()
{
    BankAccount b;
    int choice;

    do
    {
        cout << "\n====== BANK MANAGEMENT SYSTEM ======";
        cout << "\n1. Create Account";
        cout << "\n2. Deposit Money";
        cout << "\n3. Withdraw Money";
        cout << "\n4. Check Balance";
        cout << "\n5. Exit";

        cout << "\nEnter Your Choice: ";
        cin >> choice;

        switch (choice)
        {
        case 1:
            b.createAccount();
            break;

        case 2:
            b.deposit();
            break;

        case 3:
            b.withdraw();
            break;

        case 4:
            b.checkBalance();
            break;

        case 5:
            cout << "\nThank You!\n";
            break;

        default:
            cout << "\nInvalid Choice!\n";
        }

    } while (choice != 5);

    return 0;
}
