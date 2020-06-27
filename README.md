# first_try
//simulation of a ATM 
#include <iostream>

using namespace std;

const int MAX_TRANS = 30;
const float DEPOSIT_FEE = 1.00;
const float WITHDRAWAL_FEE = 1.50;
const float ENQUIRY_FEE = 0.50;
const float OVERDRAWN_FEE = 5.00;

struct Transaction
{
    char type;
    float amount;
};

class Account
{
public:
    Account();
    void deposit(float a);
    float balanceEnquiry();
    void withdrawal(float a);
    void displayStatement() const;
private:
    float balance;
    float feeTotal;
    Transaction transacts[MAX_TRANS];
    int numTransactions;
};

Account::Account()
{
    balance = 0;
    feeTotal = 0;
    numTransactions = 0;
}

void Account::deposit(float a)
{
    balance += a;
    balance -= DEPOSIT_FEE;
    feeTotal += DEPOSIT_FEE;
    transacts[numTransactions].type = 'D';
    transacts[numTransactions].amount = a;
    numTransactions++;
}

float Account::balanceEnquiry()
{
    balance -= ENQUIRY_FEE;
    feeTotal += ENQUIRY_FEE;
    transacts[numTransactions].type = 'B';
    transacts[numTransactions].amount = 0;
    numTransactions++;
    return balance;
}

void Account::withdrawal(float a)
{
    balance -= a;
    if (balance < 0)
    {
        feeTotal += OVERDRAWN_FEE;
        balance -= OVERDRAWN_FEE;
    }
    else
    {
        feeTotal += WITHDRAWAL_FEE;
        balance -= WITHDRAWAL_FEE;
    }
    transacts[numTransactions].type = 'W';
    transacts[numTransactions].amount = a;
    numTransactions++;
}

void Account::displayStatement() const
{
    cout << endl << "Monthly statement"<< endl;
    cout << "================================="<< endl;
    for (int i = 0; i < numTransactions; i++)
    {
        switch(transacts[i].type)
        {
        case 'W':
            cout << "Withdrawal\tR"<< transacts[i].amount<< endl;
            break;
        case 'D':
            cout << "Deposit\tR"<< transacts[i].amount<< endl;
            break;
        case 'B':
            cout << "Balance enquiry"<< endl;
        }
    }
    cout << "================================="<< endl<< endl;
    cout << "Total fee\tR"<< feeTotal<< endl;
    cout << "------------------------------"<< endl;
    cout << "closing balance\tR"<< balance<< endl;
}

int main()
{
    Account account1;
    char type;
    float amount;

    cout << "Enter the transaction code "<< endl << "(W)withdrawals, (D)deposits, (B)balance enquiry or (X)exit: ";
    cin >> type;
    while (toupper(type) != 'X')
    {
        switch(toupper(type))
        {
        case 'W':
            cout << "Amount: ";
            cin >> amount;
            account1.withdrawal(amount);
            break;
        case 'D':
            cout << "Amount: ";
            cin >> amount;
            account1.deposit(amount);
            break;
        case 'B':
            cout << "current balance: "<< account1.balanceEnquiry() << endl;
            break;
        default:
            cout << "*** INVALID TRANSACTION CODE ***"<< endl;
        }
        cout << "Transaction code: ";
        cin >> type;
    }

    cout << endl;
    account1.displayStatement();

    return 0;
}
