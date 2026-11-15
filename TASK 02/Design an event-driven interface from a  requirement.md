Task 2.1 – Model the Events 

The hospital appointment system uses the following event sources and listeners:

- Patient ID TextField

  - Event object: ActionEvent

  - Listener interface: ActionListener

  - Action: Accepts or validates the patient ID.

- Department ComboBox

  - Event object: ItemEvent

  - Listener interface: ItemListener

  - Action: Refreshes the doctor list according to the selected department.

- Doctor ComboBox

  - Event object: ItemEvent

  - Listener interface: ItemListener

  - Action: Updates the selected doctor.

- Time Slot ComboBox

  - Event object: ItemEvent

  - Listener interface: ItemListener

  - Action: Updates the selected appointment time.

- Book Appointment Button

  - Event object: ActionEvent

  - Listener interface: ActionListener

  - Action: Validates the details and books the appointment.

- Window Close Button

  - Event object: WindowEvent

  - Listener interface: WindowListener or WindowAdapter

  - Action: Checks whether a booking is in progress and asks for confirmation before closing.
  
Task 2.2 – Design the Component Hierarchy 

Component Containment Tree:

JFrame – Hospital Appointment Desk

|

|-- JPanel – Main Panel (BorderLayout)

    |
    
    |-- JPanel – Input Panel (GridLayout)
    
    |    |
    
    |    |-- JLabel – Patient ID
    
    |    |-- JTextField – Patient ID
    
    |    |-- JLabel – Department
    
    |    |-- JComboBox – Department
    
    |    |-- JLabel – Doctor
    
    |    |-- JComboBox – Doctor
    
    |    |-- JLabel – Time Slot
    
    |    |-- JComboBox – Time Slot
    
    |
    
    |-- JPanel – Button Panel (FlowLayout)
    
    |    |
    
    |    |-- JButton – Book Appointment
    
    |    |-- JButton – Clear
    
    |
    
    |-- JLabel – Status Message


Components Used:

- JFrame: Main application window.

- JPanel: Organises related components.

- JLabel: Displays field names and messages.

- JTextField: Accepts the patient ID.

- JComboBox: Allows department, doctor and time selection.

- JButton: Performs booking and clearing operations.

- JOptionPane: Displays confirmation and error messages.

Task 2.3 – Design the Handler Logic 

A. Pseudocode for Refreshing the Doctor List

BEGIN

    GET selected department

    CLEAR doctor list

    IF selected department is valid THEN

        GET doctors belonging to selected department

        FOR each doctor in doctor list
           
            ADD doctor to doctor combo box
      
        END FOR

    ELSE

        DISPLAY "Select a valid department"

    END IF

END


Explanation:

- When the receptionist selects a department, an ItemEvent is generated.

- The existing doctor list is cleared.

- Doctors belonging to the selected department are retrieved.

- The doctor ComboBox is updated with the available doctors.

B. Pseudocode for Validating and Booking an Appointment

BEGIN

    GET patient ID
    
    GET selected department
    
    GET selected doctor
    
    GET selected time slot

    IF patient ID is empty THEN
    
        DISPLAY "Enter patient ID"
        
        STOP
   
    END IF

    IF department is not selected THEN
        
        DISPLAY "Select department"
       
        STOP
    
    END IF

    IF doctor is not selected THEN
        
        DISPLAY "Select doctor"
        
        STOP
   
    END IF

    IF time slot is not selected THEN
       
        DISPLAY "Select time slot"
        
        STOP
    
    END IF

    VALIDATE appointment details

    IF booking is successful THEN
       
        DISPLAY "Appointment booked successfully"
   
    ELSE
    
        DISPLAY "Booking failed"
    
    END IF

END


Task 2.4 – Trace the Event Sequence 

Order of method calls:

1. The main() method starts the application.

2. The JFrame object is created.

3. The constructor creates the panels and components.

4. Layout managers arrange the components.

5. Event listeners are registered.

6. setVisible(true) displays the window.

7. The receptionist enters the patient ID.

8. The receptionist selects a department.

9. The department listener executes and refreshes the doctor list.

10. The receptionist selects a doctor and time slot.

11. The Book button generates an ActionEvent.

12. The booking listener validates the information.

13. The appointment is booked and a confirmation message is displayed.

14. When the receptionist clicks Close, the windowClosing() method is invoked.

15. If a booking is in progress, a confirmation dialog is displayed.

16. If closing is permitted, dispose() closes the window.

Comparison with Applet Life Cycle:

- init() initializes the applet.

- start() starts or resumes applet execution.

- paint() displays the applet contents.

- stop() pauses applet execution.

- destroy() releases applet resources.

A GUI window uses event-driven methods such as constructors, listeners and window-event methods. An applet follows a predefined life cycle controlled by the 
browser or applet environment.

Alternative Concept and Complexity Analysis

Loading doctors from a database every time versus storing them in a Map:



Approach 1: Fetch doctors from the database every time

- Whenever the department changes, a database query is executed.

- The database returns the doctors belonging to the selected department.

- This increases database communication and response time.

Approach 2: Load doctors once into a Map

- All department and doctor information is loaded into a Map<String, List<Doctor>>.

- The department name acts as the key.

- The corresponding doctor list is retrieved whenever the department changes.Example:

Map<String, List<Doctor>> doctors = new HashMap<>();

doctors.put("Cardiology",

    Arrays.asList(
    
        new Doctor("Dr. Kumar"),
        
        new Doctor("Dr. Ravi")
    )
    
);

List<Doctor> selected =
    
    doctors.get("Cardiology");


Time Complexity:

- Database fetching:

  - Each department change requires a database query.

  - Response time depends on database access and the number of returned doctors.

  - If m doctors are returned, processing them takes O(m).

- Map approach:

  - Retrieving the department's list takes O(1) average time.

  - Displaying m doctors takes O(m).

  - Total time: O(1 + m).

Space Complexity:

- Database fetching:

  - Only the currently retrieved doctor list needs to be stored.

  - Space complexity: O(m).

- Map approach:

  - Stores all doctors for d departments.

  - If each department has m doctors, space complexity is O(d × m).

Conclusion:

The Map approach reduces repeated database requests and provides faster department switching. However, it requires more memory to store the doctor information.
