import java.io.*;
import java.util.Scanner;

class User implements Serializable {
    String phone;
    String password;
}

public class BankManagementJava {
    static String name;
    static String accountNumber;
    static int balance = 10000;
    static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        User user = new User();
        String filename;
        String phone;
        String password;
        char cont = 'y';

        while (true) {
            System.out.println("\n\t\t\t\t\t*Money Management Solution*\t\t\t\t\t");
            System.out.println("\n\t\t\t\t\t1. Register your account");
            System.out.println("\n\t\t\t\t\t2. Login to your account");
            System.out.print("\n\t\t\t\t\tPlease enter your choice: ");
            int opt = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            if (opt == 1) {
                System.out.print("\n\t\t\t\t\tEnter your name: ");
                name = scanner.nextLine();
                System.out.print("\n\t\t\t\t\tEnter your account number: ");
                accountNumber = scanner.nextLine();
                System.out.print("\n\t\t\t\t\tEnter your phone number: ");
                user.phone = scanner.nextLine();
                System.out.print("\n\t\t\t\t\tEnter your new password: ");
                user.password = scanner.nextLine();

                filename = user.phone + ".dat";
                try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(filename))) {
                    oos.writeObject(user);
                    System.out.println("\n\t\t\t\t\tSuccessfully registered\n");
                } catch (IOException e) {
                    System.out.println("Error saving user data.");
                }
            } else if (opt == 2) {
                System.out.print("\n\n\t\t\t\t\tPhone No.: ");
                phone = scanner.nextLine();
                System.out.print("\n\t\t\t\t\tPassword: ");
                password = scanner.nextLine();

                filename = phone + ".dat";
                try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream(filename))) {
                    user = (User ) ois.readObject();
                    if (password.equals(user.password)) {
                        while (cont == 'y') {
                            System.out.println("\n\t\t\t\t\tMENU");
                            System.out.println("--------------------------------------------------");
                            System.out.println("\n\t\t\t\t\t1. Deposit Money");
                            System.out.println("\n\t\t\t\t\t2. Withdraw Money");
                            System.out.println("\n\t\t\t\t\t3. Account details");
                            System.out.println("\n\t\t\t\t\t4. Exit");
                            System.out.print("\n\t\t\t\t\tEnter your choice: ");
                            int choice = scanner.nextInt();

                            switch (choice) {
                                case 1:
                                    depositMoney();
                                    break;
                                case 2:
                                    withdrawMoney();
                                    break;
                                case 3:
                                    accountDetails();
                                    break;
                                case 4:
                                    exitProgram();
                                    System.exit(0);
                                default:
                                    System.out.println("\n\t\t\t\t\t*Invalid choice*");
                            }
                            System.out.print("\n\t\t\t\t\tDo you want to continue? [y/n]: ");
                            cont = scanner.next().charAt(0);
                            scanner.nextLine(); // Consume newline
                        }
                    } else {
                        System.out.println("\n\t\t\t\t\tInvalid password");
                    }
                } catch (IOException | ClassNotFoundException e) {
                    System.out.println("Phone number not registered");
                }
                System.out.println("\n\n\t\t\t\t\t*Thank you for banking*\n");
            }
        }
    }

    public static void depositMoney() {
        System.out.print("\n\t\t\t\t\t*DEPOSITING MONEY*\n");
        System.out.print("\n\n\t\t\t\t\tEnter the amount you want to deposit: ");
        int depositAmount = scanner.nextInt();
        balance += depositAmount;
        System.out.println("\n\t\t\t\t\t**Money Deposited");
        System.out.println("\n\t\t\t\t\tNow balance: $" + balance);
    }

    public static void withdrawMoney() {
        System.out.print("\n\t\t\t\t\t*WITHDRAWING MONEY*\n");
        System.out.print("\n\n\t\t\t\t\tEnter the amount you want to withdraw: ");
        int withdrawAmount = scanner.nextInt();
        if (balance < withdrawAmount) {
            System.out.println("\n\t\t\t\t\t*Insufficient balance*\n");
        }
    }
    
    public static void accountDetails() {
    System.out.println("\n\t\t\t\t\t*ACCOUNT DETAILS*");
    System.out.println("\n\t\t\t\t\tAccount Holder: " + name);
    System.out.println("\t\t\t\t\tAccount Number: " + accountNumber);
    System.out.println("\t\t\t\t\tCurrent Balance: $" + balance);
}

public static void exitProgram() {
    System.out.println("\n\t\t\t\t\t*Thank you for using the Money Management Solution*");
    System.out.println("\t\t\t\t\t*Goodbye!*");
}

}
            