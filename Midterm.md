# Midterm Exam
## The Code
### Main Class
```
package sdccd;
import java.util.*;

class WrongAgeException extends Exception {
    public WrongAgeException(String message) {
        super(message); //Exception for Invalid age groups. 
    }
}

class Item {
    String name;
    String company;
    double weight;

    public Item(String name, String company, double weight) {
        this.name = name;
        this.company = company;
        this.weight = weight;
    }

    public void display() {
        System.out.println("Name: " + name + ", Company: " + company + ", Weight: " + weight + " lbs");
    }


    public void update(String name, String company) {
        this.name = name;
        this.company = company;
    }

    public void update(double weight) {
        this.weight = weight;
    }
}

class Painkiller extends Item {
    String expiryDate;
    String ageGroup;

    public Painkiller(String name, String company, String expiryDate, String ageGroup) throws WrongAgeException {
        super(name, company, 0); 
        this.expiryDate = expiryDate;
        this.ageGroup = ageGroup;
        
        //Placing a If with Or statements to ensure that one of the 3 are chosen below.
        if (!(ageGroup.equals("child") || ageGroup.equals("adult") || ageGroup.equals("senior"))) {
            throw new WrongAgeException("Age group is Either Wrong, Or Doesnt exist.");
        }
    }

    @Override
    public void display() {
        System.out.println("Painkiller - v ");
        System.out.println("Name: " + name + ", Company: " + company +
                ", Expiration Date: " + expiryDate + ", Age Group: " + ageGroup);
    }
}

class Bandage extends Item {
    String expiryDate;
    String ageGroup;
    boolean isWaterproof;

    public Bandage(String name, String company, String expiryDate, String ageGroup, boolean isWaterproof) throws WrongAgeException {
        super(name, company, 0); 
        this.expiryDate = expiryDate;
        this.ageGroup = ageGroup;
        this.isWaterproof = isWaterproof;

        if (!(ageGroup.equals("child") || ageGroup.equals("adult") || ageGroup.equals("senior"))) {
            throw new WrongAgeException("Patient is Not of Age Group.");
        }
    }

    @Override
    public void display() {
        System.out.println("Bandage - v ");
        System.out.println("Name: " + name + ", Company: " + company +
                ", Expiration Date: " + expiryDate +
                ", Age Group: " + ageGroup +
                ", Waterproof: " + (isWaterproof ? "Yes" : "No")); //Gives yes or no Depending on boolean 
    }
}

class Equipment extends Item {
    public Equipment(String name, String company, double weight) {
        super(name, company, weight);
    }

    @Override
    public void display() {
        System.out.println("Equipment - v ");
        super.display();
    }
}


public class Main {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        boolean o = true;

        while (o) {
            System.out.println("\n__ Inventory Control System __");
            System.out.println("Enter 1. to Add Painkiller/s");
            System.out.println("Enter 2. to Add Bandage/s");
            System.out.println("Enter 3. to Add Equipment/s");
            System.out.println("Enter 4. to End Code.");
            System.out.print("Enter # Here: ");

            String choice = sc.nextLine();

            try {
                if (choice.equals("1")) {
                    addPainkiller(sc);
                } else if (choice.equals("2")) {
                    addBandage(sc);
                } else if (choice.equals("3")) {
                    addEquipment(sc);
                } else if (choice.equals("4")) {
                    o = false;
                    System.out.println("Ending System. . .");
                } else {
                    System.out.println("Invalid choice. Try again.");
                }
            } catch (NumberFormatException e) {
                System.out.println("Error: Weight must be a number.");
            } catch (IllegalArgumentException e) {
                System.out.println("Input error: " + e.getMessage());
            } catch (WrongAgeException e) {
                System.out.println("Age Group Exception Caught: " + e.getMessage());
            }
        }
    }



    public static void addPainkiller(Scanner sc) throws WrongAgeException {
        System.out.print("Enter Painkiller name: ");
        String name = sc.nextLine();

        System.out.print("Enter company name: ");
        String company = sc.nextLine();

        System.out.print("Enter expiration date (YYYY-MM-DD): ");
        String expiration = sc.nextLine();

        System.out.print("Enter age group (child/adult/senior): ");
        String ageGroup = sc.nextLine().toLowerCase();

        Painkiller p = new Painkiller(name, company, expiration, ageGroup);
        p.display();
    }

    public static void addBandage(Scanner sc) throws WrongAgeException {
        System.out.print("Enter Bandage name: ");
        String name = sc.nextLine();

        System.out.print("Enter company name: ");
        String company = sc.nextLine();

        System.out.print("Enter expiration date (YYYY-MM-DD): ");
        String expiration = sc.nextLine();

        System.out.print("Enter age group (child/adult/senior): ");
        String ageGroup = sc.nextLine().toLowerCase();

        String waterproofInput;
        boolean isWaterproof = false;

        while (true) {
            System.out.print("Is it waterproof? (y/n): ");
            waterproofInput = sc.nextLine().trim().toLowerCase();
            if (waterproofInput.equals("y")) {
                isWaterproof = true;
                break;
            } else if (waterproofInput.equals("n")) {
                isWaterproof = false;
                break;
            } else {
                System.out.println("Invalid input. Please enter 'y' or 'n'.");
            }
        }

        Bandage b = new Bandage(name, company, expiration, ageGroup, isWaterproof);
        b.display();
    }

    public static void addEquipment(Scanner sc) throws NumberFormatException {
        System.out.print("Enter item name: ");
        String name = sc.nextLine();

        System.out.print("Enter company name: ");
        String company = sc.nextLine();

        System.out.print("Enter weight in lbs: ");
        double weight = Double.parseDouble(sc.nextLine()); //Allows Decimals using a double.

        if (weight < 0) throw new IllegalArgumentException("The weight cannot be negative.");

        Equipment e = new Equipment(name, company, weight);
        e.display();
    }
}
```
