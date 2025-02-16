1. Link to My Flowchart  https://drive.google.com/file/d/1U_d8fQWj_TQF1HyTof03pn0cvU91KXTL/view?usp=sharing

2. Well Things That I found Challenging Was the Fact that We had to Not Only Find the Tax Percentage of Each Number, But also in a Array, And in all Honesty.. Arrays aren't really my specialty.



//(Section 1: TaxTableTools)---------------------------------------------------------------------------------------

public class TaxTableTools {


    private int [] search =   {   0,  20000, 50000, 100000, Integer.MAX_VALUE };
    private double [] value = { 0.0,   0.10,  0.20,   0.30,              0.40 };
    private int nEntries;


    public TaxTableTools () {
        nEntries  = search.length;
    }

    public TaxTableTools(int[] newSearch, double[] newValue) {
        setTables(newSearch, newValue);
    }
    // ***********************************************************************

    // FIXED
    public void setTables(int[] newSearch, double[] newValue) {
        this.search = newSearch;
        this.value = newValue;
        this.nEntries = newSearch.length;  // Update number of entries
    }

    // ***********************************************************************

    // Method to get a value from one table based on a range in the other table

    public double getValue(int searchArgument) {
        double result;
        boolean keepLooking;
        int i;

        result = 0.0;
        keepLooking = true;
        i = 0;

        while ((i < nEntries) && keepLooking) {
            if (searchArgument <= search[i]) {
                result = value[i];
                keepLooking = false;
            }
            else {
                ++i;
            }
        }

        return result;
    }
}

//(Section 2: IncomeTaxMain)*------------------------------------------------------------------------------------------------------*

import java.util.*;

public class IncomeTaxMain {

    // Method to prompt for and input an integer
    public static int getInteger(Scanner input, String prompt) {
        System.out.println(prompt + ": ");
        return input.nextInt();
    }
    //The Main Class, Allowing for Scanners.
    public static void main(String [] args) {
        final String PROMPT_SALARY = "\nEnter all annual salaries (-1 to exit)";
        Scanner sc = new Scanner(System.in);
        int annualSalary;
        double taxRate;
        int taxToPay;

        // Define the salary and tax rate tables
        int []    salary   = {   0,  20000, 50000, 100000, Integer.MAX_VALUE };
        double [] taxTable = { 0.0,   0.10,  0.20,   0.30,              0.40 };

        // Create an TaxTables Constructor
        TaxTableTools table = new TaxTableTools(salary, taxTable);

        // Gets the first annual salary to process
        annualSalary = getInteger(sc, PROMPT_SALARY);

        while (annualSalary >= 0) {
            taxRate = table.getValue(annualSalary);
            taxToPay= (int)(annualSalary * taxRate);     // Truncate tax to an integer amount
            System.out.println("Annual Salary: " + annualSalary +
                    "\tTax rate: " + taxRate +
                    "\tTax to pay: " + taxToPay);

            // Get the next annual salary
            annualSalary = getInteger(sc, PROMPT_SALARY);
        }
    }
}
