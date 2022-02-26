# Banking-Management-System

import java.util.*;
abstract class BankAccount{

    protected int balance;
    protected String customerName;
    private int customerId;
    private String Password;
    ArrayList<Integer> transactions;

    int getCustomerId() {
        return customerId;
    }

    String getPassword() {
        return Password;
    }

    BankAccount(){
        this.customerName=null;
        this.customerId=-1;
        this.Password=null;
        this.balance=0;
    }

    void open() {
        Scanner Sign = new Scanner(System.in);
        System.out.println("Welcome to banking application!");
        System.out.println("1. Sign in");
        System.out.println("2. Sign up");
        int type = Sign.nextInt();

        if(type==1) {
            login();
        }
        else {
            Scanner Signup = new Scanner(System.in);
            System.out.println("Type your name:");
            String sname = Signup.next();

            System.out.println("Set your id:");
            int sid = Signup.nextInt();

            System.out.println("Set your password:");
            String spassword = Signup.next();
            customerName=sname;
            customerId=sid;
            Password=spassword;
            setupAccount();
            login();
        }
    }

    protected void setupAccount() {
        transactions = new ArrayList<>();
    }

    void login() {

        System.out.println("Please log in!"+"\n");
        Scanner p = new Scanner(System.in);

        System.out.println("Enter your id first:");
        int id=p.nextInt();

        System.out.println("Enter your password:");
        String password=p.next();

        if(getCustomerId()==id && getPassword().equals(password)) {
            showMenu();
        }

        else {
            System.out.println("\nYour id or password was incorrect!"+"\n");
            login();
        }
    }
    
    protected void showBalance() {
        System.out.println("-------------------");
        System.out.println("Balance = "+balance);
        System.out.println("-------------------");
        System.out.println("\n");
    }

    void deposit(int amount) {
        if(amount>0) {
            balance = balance + amount;
            transactions.add(amount);
        }
    }

    void withdraw(int amount) {
        if(balance>=amount) {
            balance = balance - amount;
            transactions.add(-1*amount);
        }

        else {
            System.out.println("Sorry! Insufficient balance.");
        }
    }

    private void showPreviousNTransactions(int N) {
        if(transactions.size() == 0) {
            System.out.println("You haven't made any transaction so far");
            return;
        }

        if(transactions.size() < N) {
            System.out.println("Showing last " + transactions.size() + " transactions");
        } else {
            System.out.println("Showing last " + N + " transactions");
        }
        for(int i=transactions.size()-N; i<transactions.size(); i++) {
            showIndexedTransaction(i);
        }
    }
    
    private void previousTransaction() {
        if(transactions.size() == 0) {
            System.out.println("You haven't made any transaction so far");
            return;
        }

        showIndexedTransaction(transactions.size()-1);
    }

    private void showIndexedTransaction(int index) {

        int previousTransaction = transactions.get(transactions.size()-1);

        if(previousTransaction>0) {
            System.out.println("Deposited: "+previousTransaction);
        } else if(previousTransaction<0) {
            System.out.println("Withdrawn or transfered: "+Math.abs(previousTransaction));
        }
    }

    void showMenu() {
        char option = '\0';
        Scanner scanner = new Scanner(System.in);

        System.out.println("\nWelcome "+customerName+"!"+"\n");
        System.out.println("A. Check balance");
        System.out.println("B. Deposit");
        System.out.println("C. Withdraw");
        System.out.println("D. Transfer money");
        System.out.println("E. Previous transaction");
        System.out.println("F. Show last specified amount of transactions");
        System.out.println("G. Main menu");
        System.out.println("H. Exit");

        do
        {
            System.out.println("================");
            System.out.println("Enter an option");
            System.out.println("================");
            option = scanner.next().charAt(0);
            System.out.println("\n");

            switch(option) {

                case 'A':
                    showBalance();
                    break;

                case 'B':
                    System.out.println("---------------------------");
                    System.out.println("Enter an amount to deposit:");
                    System.out.println("---------------------------");
                    int amount = scanner.nextInt();
                    deposit(amount);
                    System.out.println("\n");
                    break;

                case 'C':
                    System.out.println("----------------------------");
                    System.out.println("Enter an amount to withdraw:");
                    System.out.println("----------------------------");
                    int amount2 = scanner.nextInt();
                    withdraw(amount2);
                    System.out.println("\n");
                    break;

                case 'D':
                    System.out.println("-------------------------------------------");
                    System.out.println("Enter the account number to transfer money:");
                    System.out.println("-------------------------------------------");
                    int transferaccount=scanner.nextInt();
                    System.out.println("----------------------------");
                    System.out.println("Enter an amount to transfer:");
                    System.out.println("----------------------------");
                    int amount3=scanner.nextInt();
                    withdraw(amount3);
                    System.out.println("\n");
                    break;

                case 'E':
                    System.out.println("------------------------------");
                    previousTransaction();
                    System.out.println("------------------------------");
                    System.out.println("\n");
                    break;

                case 'F':
                    System.out.println("------------------------------");
                    System.out.println("Please input the number of transactions you want to see");
                    int N = scanner.nextInt();
                    showPreviousNTransactions(N);
                    System.out.println("------------------------------");
                    System.out.println("\n");
                    break;

                case 'G':
                    open();

                case 'H':
                    System.out.println("************************");
                    break;

                default:
                    System.out.println("Invalid option!. Please try again");
                    break;

            }
        }while(option != 'H');

        System.out.println("Thank you for using our services.");
    }
    
}


class LoanAccount extends BankAccount {
	private static final int LIMIT_AMOUNT = 100000;

    @Override
    protected void showBalance() {
    	super.showBalance();
        System.out.println("\n-------------------");
        System.out.println("Your available credit is " + balance);
        System.out.println("Account limit: " + LIMIT_AMOUNT);
        System.out.println("-------------------\n");
        
    }

    @Override
    protected void setupAccount() {
    	super.setupAccount();
        deposit(LIMIT_AMOUNT);
        System.out.println("--------------------------\n");
        System.out.println("Your account is set up with limit " + LIMIT_AMOUNT);
        System.out.println("Current available credit: " + balance);
        System.out.println("\n--------------------------");
        
    }
}

class SavingsAccount extends BankAccount{

}

public class bankingapplicationupdated {

    public static void main(String[] args) {
    	 Scanner accountSelector = new Scanner(System.in);
         int option;
         do {
             System.out.println("Welcome to our online banking service");
             System.out.println("Please select your account type");
             System.out.println("1. Savings Account");
             System.out.println("2. Loan Account");
             System.out.println("-------------------------");
             System.out.println("Enter 0 to exit");

             option = accountSelector.nextInt();

             BankAccount a;
             switch (option) {
                 case 1:
                     a = new SavingsAccount();
                     a.open();
                     break;
                 case 2:
                     a = new LoanAccount();
                     a.open();
                     break;
                 case 0:
                     System.out.println("Exiting from application");
                     System.out.println("-------------------------");
                     break;
                 default:
                     System.out.println("Please enter an valid option");
             }
         } while (option != 0);
    }

}
