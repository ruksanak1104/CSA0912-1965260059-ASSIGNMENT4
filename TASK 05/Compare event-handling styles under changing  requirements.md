Task 5.1 – Four Implementations 

The currency converter can be implemented using four different event-handling styles.

The application accepts an amount in INR and converts it into USD or EUR.

The conversion rates provided in the assignment are:

- USD: 0.012

- EUR: 0.011

A. Frame Implements ActionListener

Java Program: ConverterA.java

import java.awt.*;

import java.awt.event.*;

public class ConverterA extends Frame
        implements ActionListener {

    TextField amount = new TextField(10);

    Choice target = new Choice();

    Button convert = new Button("Convert");

    Label result = new Label("Result");

    public ConverterA() {

        setTitle("Currency Converter");

        setLayout(new FlowLayout());

        target.add("USD");
        target.add("EUR");

        add(new Label("INR"));
        add(amount);
        add(target);
        add(convert);
        add(result);

        convert.addActionListener(this);

        addWindowListener(new WindowAdapter() {

            public void windowClosing(WindowEvent e) {
                dispose();
            }
        });

        setSize(350, 200);
        setVisible(true);
    }

    @Override
  
    public void actionPerformed(ActionEvent e) {

        try {

            double value =
                Double.parseDouble(amount.getText());

            double rate =
                target.getSelectedItem().equals("USD")
                ? 0.012 : 0.011;

            double converted = value * rate;

            result.setText(
                String.format("%.2f", converted)
            );

        } catch (NumberFormatException ex) {

            result.setText("Invalid amount");
        }
    }

    public static void main(String[] args) {
        new ConverterA();
    }
}


B. Named Inner Class

Java Program: ConverterB.java

import java.awt.*;

import java.awt.event.*;

public class ConverterB extends Frame {

    TextField amount = new TextField(10);

    Choice target = new Choice();

    Button convert = new Button("Convert");

    Label result = new Label("Result");

    public ConverterB() {

        setTitle("Currency Converter");

        setLayout(new FlowLayout());

        target.add("USD");
        target.add("EUR");

        add(new Label("INR"));
        add(amount);
        add(target);
        add(convert);
        add(result);

        convert.addActionListener(
            new ConvertHandler()
        );

        addWindowListener(new WindowAdapter() {

            public void windowClosing(WindowEvent e) {
                dispose();
            }
        });

        setSize(350, 200);
        setVisible(true);
    }

    class ConvertHandler implements ActionListener {

        @Override
       
        public void actionPerformed(ActionEvent e) {

            try {

                double value =
                    Double.parseDouble(amount.getText());

                double rate =
                    target.getSelectedItem().equals("USD")
                    ? 0.012 : 0.011;

                result.setText(
                    String.format("%.2f", value * rate)
                );

            } catch (NumberFormatException ex) {

                result.setText("Invalid amount");
            }
        }
    }

    public static void main(String[] args) {
        new ConverterB();
    }
}


C. Anonymous Inner Class

Java Program: ConverterC.java

import java.awt.*;

import java.awt.event.*;

public class ConverterC extends Frame {

    TextField amount = new TextField(10);

    Choice target = new Choice();

    Button convert = new Button("Convert");

    Label result = new Label("Result");

    public ConverterC() {

        setTitle("Currency Converter");

        setLayout(new FlowLayout());

        target.add("USD");
        target.add("EUR");

        add(new Label("INR"));
        add(amount);
        add(target);
        add(convert);
        add(result);

        convert.addActionListener(
            new ActionListener() {

                @Override
                public void actionPerformed(ActionEvent e) {

                    try {

                        double value =
                            Double.parseDouble(
                                amount.getText()
                            );

                        double rate =
                            target.getSelectedItem()
                            .equals("USD")
                            ? 0.012 : 0.011;

                        result.setText(
                            String.format(
                                "%.2f", value * rate
                            )
                        );

                    } catch (NumberFormatException ex) {

                        result.setText("Invalid amount");
                    }
                }
            }
        );

        addWindowListener(new WindowAdapter() {

            public void windowClosing(WindowEvent e) {
                dispose();
            }
        });

        setSize(350, 200);
        setVisible(true);
    }

    public static void main(String[] args) {
        new ConverterC();
    }
}


D. Lambda Expression

Java Program: ConverterD.java

import java.awt.*;

import java.awt.event.*;

public class ConverterD extends Frame {

    TextField amount = new TextField(10);

    Choice target = new Choice();

    Button convert = new Button("Convert");

    Label result = new Label("Result");

    public ConverterD() {

        setTitle("Currency Converter");

        setLayout(new FlowLayout());

        target.add("USD");
        target.add("EUR");

        add(new Label("INR"));
        add(amount);
        add(target);
        add(convert);
        add(result);

        convert.addActionListener(e -> {

            try {

                double value =
                    Double.parseDouble(amount.getText());

                double rate =
                    target.getSelectedItem().equals("USD")
                    ? 0.012 : 0.011;

                result.setText(
                    String.format("%.2f", value * rate)
                );

            } catch (NumberFormatException ex) {

                result.setText("Invalid amount");
            }
        });

        addWindowListener(new WindowAdapter() {

            public void windowClosing(WindowEvent e) {
                dispose();
            }
        });

        setSize(350, 200);
        setVisible(true);
    }

    public static void main(String[] args) {
        new ConverterD();
    }
}


Expected Output for all four programs:

- A currency converter window appears.

- The user enters an amount in INR.

- The user selects USD or EUR.

- Clicking Convert displays the converted amount.

- Invalid input displays "Invalid amount".

Example:

- Input: 1000

- Target: USD

- Output: 12.00

Task 5.2 – Change the Requirement

New Requirements:

- The converter must also work when the user presses Enter in the amount field.

- The conversion result must be displayed in a dialog box.

Changes Required in Each Implementation:

A. Frame Implements ActionListener

- Register the frame as the listener for both the Convert button and the amount field.

- Check the action source if different operations are required.

- Replace the result label update with JOptionPane.showMessageDialog().

convert.addActionListener(this);

amount.addActionListener(this);


B. Named Inner Class

- Create one ConvertHandler object.

- Register the same object with both components.

ConvertHandler handler = new ConvertHandler();

convert.addActionListener(handler);

amount.addActionListener(handler);


C. Anonymous Inner Class

- Store the anonymous listener in an ActionListener variable.

- Register that listener with both components.

ActionListener handler = new ActionListener() {

    public void actionPerformed(ActionEvent e) {
       
        // Conversion logic
    }
};

convert.addActionListener(handler);

amount.addActionListener(handler);


D. Lambda Expression

- Store the lambda expression in one ActionListener variable.

- Register it with both components.

ActionListener handler = e -> {

    try {

        double value =
            Double.parseDouble(amount.getText());

        double rate =
            target.getSelectedItem().equals("USD")
            ? 0.012 : 0.011;

        double converted = value * rate;

        JOptionPane.showMessageDialog(
            this,
            String.format("%.2f", converted)
        );

    } catch (NumberFormatException ex) {

        JOptionPane.showMessageDialog(
            this, "Invalid amount"
        );
    }
};

convert.addActionListener(handler);

amount.addActionListener(handler);


Conclusion:

All four implementations can support the changed requirement. A shared listener avoids writing the conversion logic twice.

Task 5.3 – Compare the Styles

1. Frame Implements ActionListener

- Readability: Suitable for small programs but can become lengthy.

- Field Access: Direct access to frame fields.

- Reusability: Limited because the frame itself performs the action.

- Ease of Change: Becomes difficult when multiple buttons and operations are added.

2. Named Inner Class

- Readability: Good because event-handling logic is separated.

- Field Access: Can directly access the outer class fields.

- Reusability: The same handler object can be registered with multiple components.

- Ease of Change: Easy to modify and maintain.

3. Anonymous Inner Class

- Readability: Suitable for short event-handling operations.

- Field Access: Can access the outer class fields.

- Reusability: Limited because the class is defined at the registration location.

- Ease of Change: Becomes difficult when the event-handling logic is lengthy.

4. Lambda Expression

- Readability: Short and concise.

- Field Access: Can access the outer class fields.

- Reusability: Can be improved by storing the lambda in an ActionListener variable.

- Ease of Change: Easy for simple event-handling operations.

Best Choice for a Large Application:

A named inner class or a separate handler class is suitable for a large application because it separates event-handling logic from the GUI. It improves code 
organisation, readability, reusability and maintainability.Alternative Concept and Complexity Analysis
Using Method Reference:

Instead of writing a lambda expression, a method reference can be used.

convertButton.addActionListener(
   
    e -> convert()
);


It can be simplified to:

convertButton.addActionListener(

    e -> convert()
);


For an exact method reference matching ActionListener, define an overloaded conversion method that accepts the event:

private void convert(ActionEvent e) {
  
    convert();
}


Then register:

convertButton.addActionListener(this::convert);

amount.addActionListener(this::convert);


Using One Shared Action Object:

- Create one AbstractAction object.

- Register the same Action with a button and menu item.

- Use a keyboard action binding for a keyboard shortcut.

- This avoids duplicating the conversion logic.

Time Complexity:

- All four approaches perform the same conversion operation.

- Parsing the input and calculating the converted amount take O(1) time.

- Event-handler selection is independent of the conversion calculation.

Space Complexity:

- A single shared listener requires one listener object.

- Separate listener objects may require additional memory.

- Lambda expressions and method references are concise ways of defining handlers, but their exact object allocation is implementation-dependent.

Conclusion:

A shared Action object is useful when the same operation must be triggered by buttons, menu items and keyboard shortcuts. It provides centralised event handling 
and improves maintainability.

