import java.awt.*;

import java.awt.event.*;

import javax.swing.*;

public class LibrarySearch extends JFrame {

    JTextField searchField;
    JComboBox<String> category;
    JTextArea results;
    JLabel status;

    public LibrarySearch() {

        setTitle("Campus Library Search");
        setLayout(new BorderLayout(10, 10));

        // Search panel
        JPanel searchPanel = new JPanel(
            new FlowLayout(FlowLayout.LEFT)
        );

        searchField = new JTextField(15);

        category = new JComboBox<>(
            new String[]{
                "All", "Books", "Journals", "Authors"
            }
        );

        JButton search = new JButton("Search");

        searchPanel.add(new JLabel("Search:"));
        searchPanel.add(searchField);
        searchPanel.add(category);
        searchPanel.add(search);

        // Results panel
        results = new JTextArea();
        results.setEditable(false);

        JScrollPane scroll = new JScrollPane(results);

        // Bottom panel
        JPanel buttonPanel = new JPanel(
            new FlowLayout()
        );

        JButton prev = new JButton("Prev");
        JButton next = new JButton("Next");
        JButton reserve = new JButton("Reserve");

        buttonPanel.add(prev);
        buttonPanel.add(next);
        buttonPanel.add(reserve);

        status = new JLabel("Ready");

        JPanel bottomPanel = new JPanel(
            new BorderLayout()
        );

        bottomPanel.add(buttonPanel, BorderLayout.CENTER);
        bottomPanel.add(status, BorderLayout.SOUTH);

        // Adding panels
        add(searchPanel, BorderLayout.NORTH);
        add(scroll, BorderLayout.CENTER);
        add(bottomPanel, BorderLayout.SOUTH);

        // Search event
        search.addActionListener(e -> {

            String text = searchField.getText();

            if (text.trim().isEmpty()) {
                status.setText("Enter a search keyword");
            } else {
                results.setText(
                    "Search Results for: " + text +
                    "\nCategory: " +
                    category.getSelectedItem()
                );

                status.setText("Search completed");
            }
        });

        prev.addActionListener(e ->
            status.setText("Previous page selected")
        );

        next.addActionListener(e ->
            status.setText("Next page selected")
        );

        reserve.addActionListener(e ->
            status.setText("Reservation selected")
        );

        setSize(650, 450);

        setDefaultCloseOperation(
            JFrame.EXIT_ON_CLOSE
        );

        setVisible(true);
    }

    public static void main(String[] args) {

        SwingUtilities.invokeLater(
            () -> new LibrarySearch()
        );
    }
}
