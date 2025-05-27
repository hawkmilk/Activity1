# Final Assignment: Project Database


## The Code
### AutoDataBase Class
```
package sdccd;

import java.io.*;
import java.sql.*;

public class AutoDataBase {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/Auto";
        String user = "root";
        String password = "Robert123!";
        String filePath = "src/main/java/sdccd/auto-mpg.tsv";

        try (
                Connection conn = DriverManager.getConnection(url, user, password);
                BufferedReader reader = new BufferedReader(new FileReader(filePath))
        ) {
            String insertSQL = "INSERT INTO Cars (mpg, cylinders, displacement, horsepower, weight, acceleration, model_year, origin, car_name) VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?)";
            PreparedStatement stmt = conn.prepareStatement(insertSQL);

            String line;
            int rowsInserted = 0;

            while ((line = reader.readLine()) != null) {
                if (line.trim().isEmpty() || line.startsWith("#")) continue;

                String[] split = line.split("\t", 2);
                if (split.length < 2) {
                    System.out.println("Skipping line (missing tab/car name): " + line);
                    continue;
                }

                String[] parts = split[0].trim().split("\\s+");
                if (parts.length != 8) {
                    System.out.println("Skipping line (expected 8 fields): " + line);
                    continue;
                }

                try {
                    float mpg = tryParseFloat(parts[0]);
                    int cylinders = tryParseInt(parts[1]);
                    float displacement = tryParseFloat(parts[2]);
                    float horsepower = tryParseFloat(parts[3]);
                    float weight = tryParseFloat(parts[4]);
                    float acceleration = tryParseFloat(parts[5]);
                    int modelYear = tryParseInt(parts[6]);
                    int origin = tryParseInt(parts[7]);
                    String carName = split[1].trim().replace("\"", "");

                    stmt.setFloat(1, mpg);
                    stmt.setInt(2, cylinders);
                    stmt.setFloat(3, displacement);
                    stmt.setFloat(4, horsepower);
                    stmt.setFloat(5, weight);
                    stmt.setFloat(6, acceleration);
                    stmt.setInt(7, modelYear);
                    stmt.setInt(8, origin);
                    stmt.setString(9, carName);

                    stmt.executeUpdate();
                    rowsInserted++;

                } catch (Exception e) {
                    System.out.println("Skipping line due to error: " + e.getMessage());
                }
            }

            System.out.println("auto-mpg Data inserted successfully: " + rowsInserted + " rows.");
        } catch (Exception e) {
            System.out.println("Error inserting data:");
            e.printStackTrace();
        }
    }

    private static float tryParseFloat(String s) {
        try {
            return s.equalsIgnoreCase("NA") ? 0f : Float.parseFloat(s);
        } catch (NumberFormatException e) {
            return 0f;
        }
    }

    private static int tryParseInt(String s) {
        try {
            if (s.matches("\\d+\\.0?")) {
                s = s.substring(0, s.indexOf('.'));
            }
            return Integer.parseInt(s.trim());
        } catch (NumberFormatException e) {
            System.out.println("Invalid int: " + s);
            return 0;
        }
    }
}
```

### AutoGUI Class
```
package sdccd;

import javax.swing.*;
import java.awt.*;
import java.sql.*;

public class AutoGUI extends JFrame {
    private JTextField inputField;
    private JTextArea resultArea;

    public AutoGUI() {
        setTitle("Car Inventory Sorter");
        setSize(800, 600);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        JPanel topPanel = new JPanel();
        topPanel.setLayout(new FlowLayout());

        topPanel.add(new JLabel("Search Car Data:"));
        inputField = new JTextField(20);
        topPanel.add(inputField);

        JButton refreshButton = new JButton("Search");
        topPanel.add(refreshButton);

        add(topPanel, BorderLayout.NORTH);

        // Text box for the Car Inventory
        resultArea = new JTextArea();
        resultArea.setEditable(false);
        resultArea.setFont(new Font("Monospaced", Font.PLAIN, 12)); // Alligns collumns
        add(new JScrollPane(resultArea), BorderLayout.CENTER);

        refreshButton.addActionListener(e -> fetchData());

        setVisible(true);
    }

    private void fetchData() {
        resultArea.setText(""); // clear previous results
        String searchText = inputField.getText().trim();

        String url = "jdbc:mysql://localhost:3306/Auto";
        String user = "root";
        String password = "Robert123!";

        String query;
        if (searchText.equalsIgnoreCase("ALL") || searchText.isEmpty()) {
            query = "SELECT * FROM Cars";
        } else {
            query = "SELECT * FROM Cars WHERE car_name LIKE ?";
        }

        try (Connection conn = DriverManager.getConnection(url, user, password);
             PreparedStatement stmt = conn.prepareStatement(query)) {

            if (!searchText.equalsIgnoreCase("ALL") && !searchText.isEmpty()) {
                stmt.setString(1, "%" + searchText + "%");
            }

            ResultSet rs = stmt.executeQuery();

            // Header
            resultArea.append(String.format("%-5s %-5s %-10s %-10s %-8s %-10s %-10s %-6s %-20s\n",
                    "MPG", "Cyl", "Displ", "HP", "Weight", "Accel", "Year", "Org", "Car Name"));
            resultArea.append("---------------------------------------------------------------------------------------------------------\n");

            while (rs.next()) {
                String row = String.format(
                        "%-5.1f %-5d %-10.1f %-10.1f %-8.0f %-10.1f %-10d %-6d %-20s\n",
                        rs.getFloat("mpg"),
                        rs.getInt("cylinders"),
                        rs.getFloat("displacement"),
                        rs.getFloat("horsepower"),
                        rs.getFloat("weight"),
                        rs.getFloat("acceleration"),
                        rs.getInt("model_year"),
                        rs.getInt("origin"),
                        rs.getString("car_name")
                );
                resultArea.append(row);
            }

        } catch (SQLException e) {
            resultArea.setText("Error fetching data:\n" + e.getMessage());
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(AutoGUI::new);
    }
}

```
