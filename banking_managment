#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct BankAccount {
    int accountNumber;
    char accountHolder[50];
    char fatherName[50];
    char motherName[50];
    char dateOfBirth[15];
    char address[100];
    char accountType[20];
    float balance;
    char username[50];
    char password[50];
};

// Define a static variable to keep track of the last assigned account number
static int lastAccountNumber = 1000;

// Function to generate a unique account number
int generateAccountNumber() {
    return ++lastAccountNumber;
}

void deposit(struct BankAccount *account, float amount) {
    account->balance += amount;
    printf("Amount deposited successfully. New balance: %.2f\n", account->balance);
}

void withdraw(struct BankAccount *account, float amount) {
    if (amount > account->balance) {
        printf("Insufficient funds. Withdrawal failed.\n");
    } else {
        account->balance -= amount;
        printf("Amount withdrawn successfully. New balance: %.2f\n", account->balance);
    }
}

void checkBalance(struct BankAccount *account) {
    printf("Account Holder: %s\n", account->accountHolder);
    printf("Account Number: %d\n", account->accountNumber);
    printf("Father's Name: %s\n", account->fatherName);
    printf("Mother's Name: %s\n", account->motherName);
    printf("Date of Birth: %s\n", account->dateOfBirth);
    printf("Address: %s\n", account->address);
    printf("Account Type: %s\n", account->accountType);
    printf("Current Balance: %.2f\n", account->balance);
}

void createAccount() {
    struct BankAccount newAccount;

    // Initialize accountNumber
    newAccount.accountNumber = generateAccountNumber();

    printf("Enter your name: ");
    getchar();  // Consume the newline character left in the buffer
    fgets(newAccount.accountHolder, sizeof(newAccount.accountHolder), stdin);
    newAccount.accountHolder[strcspn(newAccount.accountHolder, "\n")] = '\0';

    printf("Enter your father's name: ");
    fgets(newAccount.fatherName, sizeof(newAccount.fatherName), stdin);
    newAccount.fatherName[strcspn(newAccount.fatherName, "\n")] = '\0';

    printf("Enter your mother's name: ");
    fgets(newAccount.motherName, sizeof(newAccount.motherName), stdin);
    newAccount.motherName[strcspn(newAccount.motherName, "\n")] = '\0';

    printf("Enter your date of birth (DD-MM-YYYY): ");
    fgets(newAccount.dateOfBirth, sizeof(newAccount.dateOfBirth), stdin);
    newAccount.dateOfBirth[strcspn(newAccount.dateOfBirth, "\n")] = '\0';

    printf("Enter your address: ");
    fgets(newAccount.address, sizeof(newAccount.address), stdin);
    newAccount.address[strcspn(newAccount.address, "\n")] = '\0';

    printf("Enter your account type: ");
    fgets(newAccount.accountType, sizeof(newAccount.accountType), stdin);
    newAccount.accountType[strcspn(newAccount.accountType, "\n")] = '\0';

    printf("Enter your initial balance: ");
    scanf("%f", &newAccount.balance);

    printf("Enter a username: ");
    getchar();  // Consume the newline character left in the buffer
    fgets(newAccount.username, sizeof(newAccount.username), stdin);
    newAccount.username[strcspn(newAccount.username, "\n")] = '\0';

    printf("Enter a password: ");
    fgets(newAccount.password, sizeof(newAccount.password), stdin);
    newAccount.password[strcspn(newAccount.password, "\n")] = '\0';

    // Open the file in append mode
    FILE *file = fopen("bank_accounts.txt", "a");
    if (file == NULL) {
        perror("Error opening file");
        exit(EXIT_FAILURE);
    }

    // Write the new account details to the file
    fprintf(file, "%d %s %s %s %s %s %s %.2f %s %s\n",
            newAccount.accountNumber, newAccount.accountHolder, newAccount.fatherName, newAccount.motherName,
            newAccount.dateOfBirth, newAccount.address, newAccount.accountType,
            newAccount.balance, newAccount.username, newAccount.password);

    fclose(file);

    printf("Account created successfully!\n");
}

int signIn(char enteredUsername[50], char enteredPassword[50], struct BankAccount *loggedInAccount) {
    FILE *file = fopen("bank_accounts.txt", "r");
    if (file == NULL) {
        perror("Error opening file");
        exit(EXIT_FAILURE);
    }

    char buffer[500];
    int matchFound = 0;

    while (fgets(buffer, sizeof(buffer), file) != NULL) {
        struct BankAccount account;
        sscanf(buffer, "%d %s %s %s %s %s %s %f %s %s",
               &account.accountNumber, account.accountHolder, account.fatherName, account.motherName,
               account.dateOfBirth, account.address, account.accountType,
               &account.balance, account.username, account.password);

        if (strcmp(enteredUsername, account.username) == 0 &&
            strcmp(enteredPassword, account.password) == 0) {
            matchFound = 1;
            *loggedInAccount = account;
            break;
        }
    }

    fclose(file);

    return matchFound;
}

void deleteAccount(int accountNumber) {
    FILE *file = fopen("bank_accounts.txt", "r");
    if (file == NULL) {
        perror("Error opening file");
        exit(EXIT_FAILURE);
    }

    FILE *tempFile = fopen("temp.txt", "w");
    if (tempFile == NULL) {
        perror("Error creating temporary file");
        fclose(file);
        exit(EXIT_FAILURE);
    }

    char buffer[500];
    int matchFound = 0;

    while (fgets(buffer, sizeof(buffer), file) != NULL) {
        struct BankAccount account;
        sscanf(buffer, "%d %s %s %s %s %s %s %f %s %s",
               &account.accountNumber, account.accountHolder, account.fatherName, account.motherName,
               account.dateOfBirth, account.address, account.accountType,
               &account.balance, account.username, account.password);

        if (account.accountNumber == accountNumber) {
            matchFound = 1;
            printf("Account deleted successfully!\n");
        } else {
            fprintf(tempFile, "%d %s %s %s %s %s %s %.2f %s %s\n",
                    account.accountNumber, account.accountHolder, account.fatherName, account.motherName,
                    account.dateOfBirth, account.address, account.accountType,
                    account.balance, account.username, account.password);
        }
    }

    fclose(file);
    fclose(tempFile);

    remove("bank_accounts.txt");
    rename("temp.txt", "bank_accounts.txt");

    if (!matchFound) {
        printf("Account not found. Deletion failed.\n");
    }
}

int main() {
    int choice;
    char enteredUsername[50];
    char enteredPassword[50];
    struct BankAccount loggedInAccount;

    do {
        printf("\nBank Management System\n");
        printf("1. Create Account\n");
        printf("2. Sign In\n");
        printf("3. Delete Account\n");
        printf("4. Exit\n");

        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                createAccount();
                break;
            case 2:
                getchar();  // Consume the newline character left in the buffer
                printf("Enter your username: ");
                fgets(enteredUsername, sizeof(enteredUsername), stdin);
                enteredUsername[strcspn(enteredUsername, "\n")] = '\0';

                printf("Enter your password: ");
                fgets(enteredPassword, sizeof(enteredPassword), stdin);
                enteredPassword[strcspn(enteredPassword, "\n")] = '\0';

                if (signIn(enteredUsername, enteredPassword, &loggedInAccount)) {
                    printf("Login successful! Account Number: %d\n", loggedInAccount.accountNumber);
                    int loggedInChoice;

                    do {
                        printf("\nLogged In Menu\n");
                        printf("1. Deposit\n");
                        printf("2. Withdraw\n");
                        printf("3. Check Balance\n");
                        printf("4. Sign Out\n");

                        printf("Enter your choice: ");
                        scanf("%d", &loggedInChoice);

                        switch (loggedInChoice) {
                            case 1:
                                printf("Enter amount to deposit: ");
                                float depositAmount;
                                scanf("%f", &depositAmount);
                                deposit(&loggedInAccount, depositAmount);
                                break;
                            case 2:
                                printf("Enter amount to withdraw: ");
                                float withdrawAmount;
                                scanf("%f", &withdrawAmount);
                                withdraw(&loggedInAccount, withdrawAmount);
                                break;
                            case 3:
                                checkBalance(&loggedInAccount);
                                break;
                            case 4:
                                printf("Signing out. Thank you!\n");
                                break;
                            default:
                                printf("Invalid choice. Please try again.\n");
                        }
                    } while (loggedInChoice != 4);

                } else {
                    printf("Invalid username or password. Please try again.\n");
                }
                break;
            case 3:
                printf("Enter the account number to delete: ");
                int accountToDelete;
                scanf("%d", &accountToDelete);
                deleteAccount(accountToDelete);
                break;
            case 4:
                printf("Exiting program. Thank you!\n");
                break;
            default:
                printf("Invalid choice. Please try again.\n");
        }

    } while (choice != 4);

    return 0;
}
