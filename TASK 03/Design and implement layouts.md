Task 3.1 – Choose the Layout Managers 

The Campus Library Search Screen requires different layout managers to organise the components and support window resizing.

1. BorderLayout

- Used for the main JFrame.

- Divides the window into North, South, East, West and Center regions.

- The results area is placed in the Center.

- The results area automatically expands when the window is resized.

2. FlowLayout

- Used for the search panel.

- Arranges the search field, category Choice and Search button horizontally.

- Maintains the preferred size of the components.

3. FlowLayout

- Used for the bottom button panel.

- Arranges Prev, Next and Reserve buttons horizontally.

- Keeps the buttons at their preferred sizes.

4. GridLayout

- Can be used for arranging components inside smaller panels when equal-sized rows and columns are required.

- It is not necessary for the main results area.

5. CardLayout

- Can be used when multiple result screens or views must occupy the same area.

- It is optional for this particular design.

Task 3.2 – Design the Nested Containers 

Component Structure:

JFrame – Library Search Screen

|

|-- JPanel – Main Panel (BorderLayout)
     
     |
     
     |-- NORTH: JPanel – Search Panel (FlowLayout)
     
     |       |
     
     |       |-- JLabel – Search
     
     |       |-- JTextField – Search Field
     
     |       |-- JComboBox – Category
     
     |       |-- JButton – Search
     
     |
     
     |-- CENTER: JPanel – Results Panel
     
     |       |
     
     |       |-- JTextArea / JTable – Search Results
     
     |
     
     |-- SOUTH: JPanel – Bottom Panel (BorderLayout)
     
             |
             
             |-- CENTER: JPanel – Button Panel (FlowLayout)
             
             |       |
             
             |       |-- JButton – Prev
             
             |       |-- JButton – Next
             
             |       |-- JButton – Reserve
             
             |
             
             |-- SOUTH: JLabel – Status Message


Task 3.3 – Test the Resizing Behaviour 

Perform the following three tests and capture screenshots.

Test 1 – Small Window

- Set the window size to approximately 450 × 300.

- The search panel remains at the top.

- The results area becomes smaller.

- The bottom buttons remain visible.

Test 2 – Medium Window

- Set the window size to approximately 650 × 450.

- All components are displayed properly.

- The results area occupies the available centre space.

Test 3 – Large Window

- Set the window size to approximately 900 × 650.

- The results area expands automatically.

- The search panel and bottom buttons retain their preferred component sizes.

Conclusion:

The nested BorderLayout and FlowLayout arrangement satisfies the resizing requirement. The centre results area expands while the search and button panels maintain their preferred sizes.

Alternative Concept and Complexity Analysis

Approach 1: Nested BorderLayout and FlowLayout

- Uses separate panels to organise components.

- Automatically adjusts components when the window is resized.

- Maintains a flexible and responsive layout.

- Requires a small number of additional container objects.

Approach 2: GridBagLayout

- Uses constraints to position components in rows and columns.

- Provides more control over component placement and resizing.

- Requires more configuration than BorderLayout and FlowLayout.

- Suitable for complex forms.

Approach 3: Absolute Positioning

- Uses setLayout(null) and setBounds() to position components manually.

- Every component requires fixed coordinates and dimensions.

- Components may overlap or become misaligned when the window is resized.

- It is difficult to maintain across different screen sizes.

Time Complexity:

- Nested layouts: Layout computation is approximately O(n) for n components in a typical layout pass.

- GridBagLayout: Also generally requires processing component constraints, approximately O(n) for a layout pass, with additional constraint calculations.

- Absolute positioning: Assigning bounds is approximately O(n) when processing all components, but does not automatically provide responsive resizing.

Space Complexity:

- Nested layouts: O(n + p), where n is the number of components and p is the number of panels.

- GridBagLayout: O(n + c), where c represents the layout constraints.

- Absolute positioning: O(n) for component references and stored bounds.

Conclusion:
 
Nested BorderLayout and FlowLayout are the best choices for this library screen because they provide simple organisation and responsive resizing. Absolute 
positioning is not recommended because it makes resizing difficult.
