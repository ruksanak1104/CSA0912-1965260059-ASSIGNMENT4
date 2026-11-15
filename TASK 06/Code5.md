import java.awt.*;

import java.io.*;

import java.util.*;

import javax.swing.*;

import javax.swing.table.DefaultTableModel;

public class RegistrationGUI extends JFrame {

    JTextField nameField =
        new JTextField(15);

    JTextField regNoField =
        new JTextField(15);

    JTextField emailField =
        new JTextField(15);

    JComboBox<String> eventCombo =
        new JComboBox<>();

    JComboBox<String> filterCombo =
        new JComboBox<>();

    JTable table;

    DefaultTableModel model;

    JLabel status =
        new JLabel("Ready");

    RegistrationDAO dao =
        new RegistrationDAO();

    RegistrationService service =
        new RegistrationService();

    int currentPage = 0;

    final int PAGE_SIZE = 20;

    public RegistrationGUI() {

        setTitle("Campus Event Registration");

        setLayout(new BorderLayout());

        // Registration panel

        JPanel form = new JPanel(
            new GridLayout(5, 2, 8, 8)
        );

        form.setBorder(
            BorderFactory.createTitledBorder(
                "Student Registration"
            )
        );

        JButton register =
            new JButton("Register");

        JButton clear =
            new JButton("Clear");

        form.add(new JLabel("Student Name:"));
        form.add(nameField);

        form.add(new JLabel("Register Number:"));
        form.add(regNoField);

        form.add(new JLabel("Email:"));
        form.add(emailField);

        form.add(new JLabel("Select Event:"));
        form.add(eventCombo);

        form.add(register);
        form.add(clear);

        // Table

        model = new DefaultTableModel(
            new String[]{
                "Register No",
                "Student Name",
                "Email",
                "Event"
            }, 0
        );

        table = new JTable(model);

        JScrollPane scroll =
            new JScrollPane(table);

        // Filter panel

        JPanel filterPanel = new JPanel();

        filterPanel.add(
            new JLabel("Filter Event:")
        );

        filterPanel.add(filterCombo);

        JButton filter =
            new JButton("Filter");

        JButton previous =
            new JButton("Previous");

        JButton next =
            new JButton("Next");

        JButton export =
            new JButton("Export CSV");

        filterPanel.add(filter);
        filterPanel.add(previous);
        filterPanel.add(next);
        filterPanel.add(export);

        // Menu bar

        JMenuBar menuBar = new JMenuBar();

        JMenu fileMenu = new JMenu("File");

        JMenu viewMenu = new JMenu("View");

        JMenu helpMenu = new JMenu("Help");

        JMenuItem exportItem =
            new JMenuItem("Export CSV");

        JMenuItem refreshItem =
            new JMenuItem("Refresh");

        JMenuItem exitItem =
            new JMenuItem("Exit");

        JMenuItem aboutItem =
            new JMenuItem("About");

        fileMenu.add(exportItem);
        fileMenu.add(exitItem);

        viewMenu.add(refreshItem);

        helpMenu.add(aboutItem);

        menuBar.add(fileMenu);
        menuBar.add(viewMenu);
        menuBar.add(helpMenu);

        setJMenuBar(menuBar);

        // Adding components

        add(form, BorderLayout.NORTH);
        add(scroll, BorderLayout.CENTER);

        JPanel bottom = new JPanel(
            new BorderLayout()
        );

        bottom.add(filterPanel, BorderLayout.CENTER);
        bottom.add(status, BorderLayout.SOUTH);

        add(bottom, BorderLayout.SOUTH);

        // Load events

        loadEvents();

        // Register button

        register.addActionListener(e ->
            registerStudent()
        );

        // Clear button

        clear.addActionListener(e -> {

            nameField.setText("");
            regNoField.setText("");
            emailField.setText("");

            eventCombo.setSelectedIndex(-1);
        });

        // Filter button

        filter.addActionListener(e -> {

            currentPage = 0;
            loadRegistrations();
        });

        // Previous button

        previous.addActionListener(e -> {

            if (currentPage > 0) {

                currentPage--;

                loadRegistrations();
            }
        });

        // Next button

        next.addActionListener(e -> {

            currentPage++;

            loadRegistrations();
        });

        // Export button

        export.addActionListener(e ->
            exportCSV()
        );

        exportItem.addActionListener(e ->
            exportCSV()
        );

        refreshItem.addActionListener(e -> {

            currentPage = 0;

            loadRegistrations();
        });

        exitItem.addActionListener(e ->
            dispose()
        );

        aboutItem.addActionListener(e ->
            JOptionPane.showMessageDialog(
                this,
                "Campus Event Registration System"
            )
        );

        setDefaultCloseOperation(
            JFrame.EXIT_ON_CLOSE
        );

        setSize(850, 550);

        setLocationRelativeTo(null);

        setVisible(true);
    }

    // Load event names

    private void loadEvents() {

        new SwingWorker<java.util.List<String>, Void>() {

            protected java.util.List<String> doInBackground()
                    throws Exception {

                return dao.getEvents();
            }

            protected void done() {

                try {

                    java.util.List<String> events =
                        get();

                    eventCombo.removeAllItems();

                    filterCombo.removeAllItems();

                    filterCombo.addItem("All Events");

                    for (String event : events) {

                        eventCombo.addItem(event);

                        filterCombo.addItem(event);
                    }

                    eventCombo.setSelectedIndex(-1);

                } catch (Exception e) {

                    status.setText(
                        "Unable to load events"
                    );
                }
            }

        }.execute();
    }

    // Register student

    private void registerStudent() {

        String name = nameField.getText();

        String regNo = regNoField.getText();

        String email = emailField.getText();

        String selectedEvent =
            (String) eventCombo.getSelectedItem();

        if (selectedEvent == null) {

            JOptionPane.showMessageDialog(
                this, "Please select an event"
            );

            return;
        }

        register.setEnabled(false);

        status.setText("Processing registration...");

        new SwingWorker<String, Void>() {

            protected String doInBackground()
                    throws Exception {

                int eventId =
                    dao.getEventId(selectedEvent);

                return service.register(
                    name, regNo, email, eventId
                );
            }

            protected void done() {

                try {

                    String message = get();

                    JOptionPane.showMessageDialog(
                        RegistrationGUI.this,
                        message
                    );

                    status.setText(message);

                    if (message.equals(
                        "Registration successful"
                    )) {

                        currentPage = 0;

                        loadRegistrations();
                    }

                } catch (Exception e) {

                    status.setText(
                        "Registration failed"
                    );

                } finally {

                    register.setEnabled(true);
                }
            }

        }.execute();
    }

    // Load registrations using pagination

    private void loadRegistrations() {

        String selected =
            (String) filterCombo.getSelectedItem();

        new SwingWorker<java.util.List<String[]>, Void>() {

            protected java.util.List<String[]> doInBackground()
                    throws Exception {

                int eventId = -1;

                if (selected != null &&
                    !selected.equals("All Events")) {

                    eventId = dao.getEventId(selected);
                }

                return dao.getRegistrations(
                    eventId,
                    PAGE_SIZE,
                    currentPage * PAGE_SIZE
                );
            }

            protected void done() {

                try {

                    java.util.List<String[]> records = get();

                    model.setRowCount(0);

                    for (String[] row : records) {

                        model.addRow(row);
                    }

                    status.setText(
                        "Records loaded: " + records.size()
                    );

                } catch (Exception e) {

                    status.setText(
                        "Unable to load registrations"
                    );
                }
            }

        }.execute();
    }

    // Export registrations

    private void exportCSV() {

        JFileChooser chooser =
            new JFileChooser();

        chooser.setSelectedFile(
            new File("registrations.csv")
        );

        int option = chooser.showSaveDialog(this);

        if (option != JFileChooser.APPROVE_OPTION) {
            return;
        }

        File file = chooser.getSelectedFile();

        try (
            PrintWriter writer =
                new PrintWriter(file)
        ) {

            for (int i = 0; i < model.getColumnCount(); i++) {

                if (i > 0) writer.print(",");

                writer.print(
                    csv(model.getColumnName(i))
                );
            }

            writer.println();

            for (int i = 0; i < model.getRowCount(); i++) {

                for (int j = 0;
                     j < model.getColumnCount(); j++) {

                    if (j > 0) writer.print(",");

                    writer.print(
                        csv(String.valueOf(
                            model.getValueAt(i, j)
                        ))
                    );
                }

                writer.println();
            }

            JOptionPane.showMessageDialog(
                this,
                "CSV exported successfully"
            );

        } catch (Exception e) {

            JOptionPane.showMessageDialog(
                this,
                "Export failed: " + e.getMessage()
            );
        }
    }

    private String csv(String value) {

        return "\"" +
            value.replace("\"", "\"\"") +
            "\"";
    }

    public static void main(String[] args) {

        SwingUtilities.invokeLater(
            () -> new RegistrationGUI()
        );
    }
}
