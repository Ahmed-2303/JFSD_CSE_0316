package mini_project_java;

import java.util.Scanner;
import java.util.ArrayList;
import java.util.List;
import java.util.Random;

interface BankOperations {
    void deposit(double amount);
    void withdraw(double amount);
    void checkBalance();
    String getAccountNumber();
}

abstract class AbstractBankAccount implements BankOperations {
    protected double balance;
    protected String accountNumber;
    protected String pin;

    public AbstractBankAccount(double initialBalance, String pin) {
        this.accountNumber = generateAccountNumber();
        if (initialBalance >= 0) {
            this.balance = initialBalance;
        } else {
            System.out.println("Initial deposit cannot be negative. Setting balance to 0.");
            this.balance = 0;
        }
        this.pin = pin;
    }

    private String generateAccountNumber() {
        Random random = new Random();
        long number = 1000000000L + Math.abs(random.nextLong() % 9000000000L); // Use Math.abs to prevent negative numbers
        return String.valueOf(number);
    }

    public boolean verifyPin(String inputPin) {
        return this.pin.equals(inputPin);
    }

    public String getAccountNumber() {
        return accountNumber;
    }

}

class BankAccount extends AbstractBankAccount {
    public BankAccount(double initialBalance, String pin) {
        super(initialBalance, pin);
    }

    @Override
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println("Successfully deposited: Rs." + amount);
        } else {
            System.out.println("Invalid deposit amount.");
        }
    }

    @Override
    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            System.out.println("Successfully withdrawn: Rs." + amount);
        } else {
            System.out.println("Insufficient balance or invalid amount.");
        }
    }

    @Override
    public void checkBalance() {
        System.out.println("Current balance: Rs." + balance);
    }
}

public class BankingApp {

    private static List<BankOperations> accounts = new ArrayList<>();

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("\nBanking System Menu:");
            System.out.println("1. Create Account");
            System.out.println("2. Deposit");
            System.out.println("3. Withdraw");
            System.out.println("4. Check Balance");
            System.out.println("5. Exit");
            System.out.print("Choose an option: ");

            if (scanner.hasNextInt()) { // Check if input is an integer to prevent InputMismatchException
                int choice = scanner.nextInt();
                scanner.nextLine(); // Consume the newline character

                switch (choice) {
                    case 1:
                        createAccount(scanner);
                        break;
                    case 2:
                    case 3:
                    case 4:
                        performTransaction(scanner, choice); // Pass choice directly
                        break;
                    case 5:
                        System.out.println("Exiting... Thank you for using the banking system!");
                        scanner.close();
                        return;
                    default:
                        System.out.println("Invalid choice. Please try again.");
                }
            } else {
                System.out.println("Invalid input. Please enter a number.");
                scanner.next(); // Clear the invalid input
            }
        }
    }

    private static void createAccount(Scanner scanner) {
        double initialBalance;
        do {
            System.out.print("Enter initial deposit amount: ");
            while (!scanner.hasNextDouble()) { // Check if the input is a double
                System.out.println("Invalid input. Please enter a number.");
                scanner.next(); // Clear the invalid input
            }
            initialBalance = scanner.nextDouble();
            if (initialBalance < 0) {
                System.out.println("Initial deposit cannot be negative. Please enter a valid amount.");
            }
        } while (initialBalance < 0);

        scanner.nextLine(); // consume the newline character left by nextDouble
        String pin;
        do {
            System.out.print("Set a 4-digit PIN for your account: ");
            pin = scanner.nextLine();
            if (!pin.matches("\\d{4}")) {
                System.out.println("PIN must be 4 digits. Try again.");
            }
        } while (!pin.matches("\\d{4}"));

        BankOperations account = new BankAccount(initialBalance, pin);
        accounts.add(account);
        System.out.println("Account created successfully!");
        System.out.println("Your Account Number is: " + account.getAccountNumber());
    }

    private static void performTransaction(Scanner scanner, int choice) { // Modified to accept choice
        if (accounts.isEmpty()) {
            System.out.println("No accounts exist. Please create an account first.");
            return;
        }

        System.out.print("Enter your Account Number: ");
        String accountNumber = scanner.nextLine();

        BankOperations selectedAccount = null;
        for (BankOperations account : accounts) {
            if (account.getAccountNumber().equals(accountNumber)) {
                selectedAccount = account;
                break;
            }
        }

        if (selectedAccount == null) {
            System.out.println("Account not found.");
            return;
        }

        System.out.print("Enter your PIN: ");
        String inputPin = scanner.nextLine();

        if (selectedAccount instanceof AbstractBankAccount && ((AbstractBankAccount) selectedAccount).verifyPin(inputPin)) {
            switch (choice) { // Use the choice directly
                case 2: // Deposit
                    System.out.print("Enter deposit amount: ");
                    while (!scanner.hasNextDouble()) {
                        System.out.println("Invalid input. Please enter a number.");
                        scanner.next();
                    }
                    double depositAmount = scanner.nextDouble();
                    selectedAccount.deposit(depositAmount);
                    break;
                case 3: // Withdraw
                    System.out.print("Enter withdrawal amount: ");
                    while (!scanner.hasNextDouble()) {
                        System.out.println("Invalid input. Please enter a number.");
                        scanner.next();
                    }
                    double withdrawAmount = scanner.nextDouble();
                    selectedAccount.withdraw(withdrawAmount);
                    break;
                case 4: // Check Balance
                    selectedAccount.checkBalance();
                    break;
            }
        } else {
            System.out.println("Incorrect PIN. Try again.");
        }
    }
}
