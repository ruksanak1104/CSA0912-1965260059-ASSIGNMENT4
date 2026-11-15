Task 1.1 – Diagnose the Design 

The given student fee-payment program contains the following design deficiencies:

1. Poor separation of interface and application logic

- The complete payment and database operations are written inside the actionPerformed() method.

- This makes the program difficult to understand, test and maintain.

- The solution is to separate the GUI from the business logic using a FeeService class.

2. Incorrect String comparison

- The code uses == to compare the action command with "Pay".

- In Java, == compares object references instead of String contents.

- The correct method is .equals().

3. No input validation

- The program does not check whether the registration number and amount fields are empty.

- It does not check whether the amount is a valid positive number.

- Invalid input may cause NumberFormatException or incorrect database entries.

4. Database operations on the Event Dispatch Thread (EDT)

- Database operations are performed inside actionPerformed().

- The GUI event-dispatch thread becomes busy while processing the database request.

- As a result, the window may freeze and become unresponsive.

5. Use of Thread.sleep()

- Thread.sleep(3000) pauses the GUI thread for three seconds.

- The user cannot interact with the window during this time.

- Background processing using SwingWorker should be used instead.

6. No proper user feedback

- The program only displays "Paid" using System.out.println().

- The user does not receive a proper confirmation message on the GUI.

- A JOptionPane dialog or status label should display the payment result.

7. Receipt button is not implemented

- The program handles only the Pay operation.

- Clicking the Receipt button executes the else block and clears the fields.

- A separate receipt-generation operation must be implemented.

8. SQL injection vulnerability

- User input is directly concatenated into the SQL query.

- Malicious input could modify the database query.

- PreparedStatement must be used to execute parameterized SQL statements.

9. No proper window closing and connection management

- The program does not handle the window-closing event.

- Database connections and statements may remain open.

- Resources should be closed using try-with-resources, and the window should be disposed of properly.

10. Unhandled checked exceptions

- Database operations may generate SQLException.

- Thread.sleep() may generate InterruptedException.

- These exceptions are not handled, which can prevent successful compilation.

Task 1.2 – Repair the Design 

Aim: To separate the GUI from the payment-processing logic using a FeeService class.

Class Structure:

- FeeWindow

- Creates the user interface.

- Accepts registration number and payment amount.

- Handles Pay, Clear and Receipt button events.

- Displays confirmation and error messages.

- FeeService

  - Validates registration number and payment amount.

  - Performs payment processing.

  - Communicates with the database using JDBC.

  - Returns the payment result.

Task 1.3 – Design Defence 

The redesigned program is easier to test and maintain because the GUI and business logic are separated. The FeeWindow class handles user interactions, while the FeeService class performs validation and payment processing. Each class has a single responsibility, making debugging and modification easier. The service class can also be tested independently without opening the GUI. Therefore, the redesigned system provides better code reusability, readability and maintainability.

Alternative Concept and Complexity Analysis

Using separate listener objects instead of one listener with an if-else chain:

- Each button can have its own ActionListener object.

- The Pay button performs payment processing.

- The Clear button clears the input fields.

- The Receipt button generates the receipt.

- A Swing Action can also be used to share the same action between buttons and menu items.

Time Complexity:

- Single listener with an if-else chain:

  - In the worst case, it performs up to k comparisons for k buttons.

  - Time complexity: O(k).

- Separate listener objects:

  - Each button directly invokes its registered listener.

  - Event dispatch does not require searching through all button commands.

  - Time complexity: O(1) for listener selection, excluding the actual action processing.

Space Complexity:

- Single listener: O(1) additional listener objects.

- Separate listeners: O(k) listener objects for k buttons.

Conclusion:

Separate listeners improve readability and reduce conditional checking. However, they require additional memory because each button has its own listener object.
