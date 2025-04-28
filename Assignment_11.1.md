# Assignment 11.1
## 1. Flow Chart
[Flow Chart Link](https://drive.google.com/file/d/1o8UFhtveH1QnTX9SBE48sSB9t0pUfJfB/view?usp=sharing)
## 2. Things I Found Challenging
Something I found Challenging was figuring Out how to Put everything we learned from the lectures together from scratch. After a bit getting used to it, It only became easier from there. 

## 4. The Code
### SalaryCalc Class
```
package sdccd;

import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

public class SalaryCalc extends JFrame implements ActionListener {
    private JTextField wageField;
    private JTextField hoursField;
    private JLabel resultLabel;
    private JButton calcButton;

    public SalaryCalc() {
        // Sets the title header
        setTitle("Yearly Salary Calculator");

        // Creates the Labels along with Their Individual Text boxes
        JLabel wageLabel = new JLabel("  Hourly Wage:");
        JLabel hoursLabel = new JLabel("  Hours per Week:");
        wageField = new JTextField(10);
        hoursField = new JTextField(10);
        calcButton = new JButton("Calculate");
        resultLabel = new JLabel("  Yearly Salary: ");
        calcButton.addActionListener(this);

        // Creates a panel with Spacing between the borders
        JPanel panel = new JPanel();
        panel.setLayout(new GridLayout(4, 2, 5, 5));
        panel.setBorder(BorderFactory.createEmptyBorder(10, 10, 10, 10));
        panel.add(wageLabel);
        panel.add(wageField);
        panel.add(hoursLabel);
        panel.add(hoursField);
        panel.add(new JLabel("")); // Empty cell
        panel.add(calcButton);
        panel.add(resultLabel);
        add(panel);

        // Closing operation and size of the Window is Below
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setSize(320, 220);
        setVisible(true);
    }

    public void actionPerformed(ActionEvent e) {
        try {
            double hourlyWage = Double.parseDouble(wageField.getText());
            double hoursPerWeek = Double.parseDouble(hoursField.getText());
            double yearlySalary = hourlyWage * hoursPerWeek * 52;
            resultLabel.setText(String.format("Yearly Salary: $%.1f", yearlySalary));
        } catch (NumberFormatException ex) {
            resultLabel.setText("Please enter valid numbers.");
        }
    }

    public static void main(String[] args) {
        new SalaryCalc();
    }
}
```
