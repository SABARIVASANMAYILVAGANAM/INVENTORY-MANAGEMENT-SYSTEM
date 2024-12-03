# INVENTORY-MANAGEMENT-SYSTEM_JAVA

import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import javax.swing.table.DefaultTableModel;

public class InventoryManagementSystem extends JFrame {

  private JTable table;
    private DefaultTableModel tableModel;
    private JTextField nameField, priceField, quantityField;
    private JButton addButton, updateButton, deleteButton;
    private JLabel itemsLabel, costLabel;
    private int totalItems = 0;
    private double totalCost = 0.0;

   public InventoryManagementSystem() {
        // Set up JFrame
        setTitle("Inventory Management System");
        setSize(600, 450);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);
        setLayout(new BorderLayout());

    // Header Section
  JPanel inputPanel = new JPanel();
        inputPanel.setLayout(new GridLayout(3, 2, 10, 10));
        inputPanel.setBackground(new Color(0, 204, 255));
        inputPanel.setBorder(BorderFactory.createEmptyBorder(10, 10, 10, 10));

  JLabel nameLabel = new JLabel("Item Name:");
        nameField = new JTextField();
        JLabel priceLabel = new JLabel("Price:");
        priceField = new JTextField();
        JLabel quantityLabel = new JLabel("Quantity:");
        quantityField = new JTextField();

  inputPanel.add(nameLabel);
        inputPanel.add(nameField);
        inputPanel.add(priceLabel);
        inputPanel.add(priceField);
        inputPanel.add(quantityLabel);
        inputPanel.add(quantityField);

        // Table Section
  tableModel = new DefaultTableModel(new Object[]{"Item Name", "Price", "Quantity"}, 0);
        table = new JTable(tableModel);
        table.setRowHeight(25);
        table.setGridColor(new Color(135, 206, 235)); // Set the grid lines to sky blue
        table.setShowGrid(true); // Ensure grid lines are displayed
        table.getTableHeader().setFont(new Font("Arial", Font.BOLD, 14));
        table.getTableHeader().setBackground(new Color(200, 200, 255));

  JScrollPane scrollPane = new JScrollPane(table);
        scrollPane.setBorder(BorderFactory.createEmptyBorder(10, 10, 10, 10));

        // Footer Section
  JPanel buttonPanel = new JPanel();
        buttonPanel.setLayout(new GridLayout(2, 3, 10, 10));
        buttonPanel.setBorder(BorderFactory.createEmptyBorder(10, 10, 10, 10));
        buttonPanel.setBackground(new Color(240, 240, 255));

  addButton = new JButton("Add Item");
        addButton.setBackground(new Color(102, 204, 255));
        updateButton = new JButton("Update Item");
        updateButton.setBackground(new Color(255, 204, 102));
        deleteButton = new JButton("Delete Item");
        deleteButton.setBackground(new Color(255, 102, 102));

   itemsLabel = new JLabel("Total Items: 0");
        costLabel = new JLabel("Total Cost: $0.00");

 buttonPanel.add(addButton);
        buttonPanel.add(updateButton);
        buttonPanel.add(deleteButton);
        buttonPanel.add(new JLabel(""));
        buttonPanel.add(itemsLabel);
        buttonPanel.add(costLabel);

        // Add components to the frame
  add(inputPanel, BorderLayout.NORTH);
        add(scrollPane, BorderLayout.CENTER);
        add(buttonPanel, BorderLayout.SOUTH);

        // Add button listeners
   addButton.addActionListener(e -> addItem());
        updateButton.addActionListener(e -> updateItem());
        deleteButton.addActionListener(e -> deleteItem());
    }

  private void addItem() {
     String name = nameField.getText();
        String priceText = priceField.getText();
        String quantityText = quantityField.getText();
    if (name.isEmpty() || priceText.isEmpty() || quantityText.isEmpty()) {
            JOptionPane.showMessageDialog(this, "All fields must be filled!");
            return;
        }

   try {
            double price = Double.parseDouble(priceText);
            int quantity = Integer.parseInt(quantityText);
      tableModel.addRow(new Object[]{name, price, quantity});
            totalItems++;
            totalCost += price * quantity;
     itemsLabel.setText("Total Items: " + totalItems);
            costLabel.setText("Total Cost: $" + String.format("%.2f", totalCost));
            clearFields();
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(this, "Invalid input for price or quantity!");
        }
    }

 private void updateItem() {
        int selectedRow = table.getSelectedRow();
        if (selectedRow == -1) {
            JOptionPane.showMessageDialog(this, "Select a row to update!");
            return;
        }

  String name = nameField.getText();
        String priceText = priceField.getText();
        String quantityText = quantityField.getText();

  if (name.isEmpty() || priceText.isEmpty() || quantityText.isEmpty()) {
            JOptionPane.showMessageDialog(this, "All fields must be filled!");
            return;
        }
      try {
            double price = Double.parseDouble(priceText);
            int quantity = Integer.parseInt(quantityText);
     tableModel.setValueAt(name, selectedRow, 0);
            tableModel.setValueAt(price, selectedRow, 1);
            tableModel.setValueAt(quantity, selectedRow, 2);
       recalculateTotals();
            clearFields();
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(this, "Invalid input for price or quantity!");
        }
    }
    private void deleteItem() {
        int selectedRow = table.getSelectedRow();
        if (selectedRow == -1) {
            JOptionPane.showMessageDialog(this, "Select a row to delete!");
            return;
        }
        tableModel.removeRow(selectedRow);
        totalItems--;
        recalculateTotals();
        itemsLabel.setText("Total Items: " + totalItems);
        costLabel.setText("Total Cost: $" + String.format("%.2f", totalCost));
    }
    private void recalculateTotals() {
        totalCost = 0;
        for (int i = 0; i < tableModel.getRowCount(); i++) {
            double price = (double) tableModel.getValueAt(i, 1);
            int quantity = (int) tableModel.getValueAt(i, 2);
            totalCost += price * quantity;
        }
        itemsLabel.setText("Total Items: " + totalItems);
        costLabel.setText("Total Cost: $" + String.format("%.2f", totalCost));
    }
    private void clearFields() {
        nameField.setText("");
        priceField.setText("");
        quantityField.setText("");
    }
    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> new InventoryManagementSystem().setVisible(true));
    }
}

