import java.sql.SQLException;

public class RegistrationService {

    private RegistrationDAO dao =
        new RegistrationDAO();

    public String register(
            String name,
            String regNo,
            String email,
            int eventId) {

        if (name == null || name.trim().isEmpty()) {
            return "Student name is required";
        }

        if (regNo == null || regNo.trim().isEmpty()) {
            return "Register number is required";
        }

        if (email == null ||
            !email.matches(
                "^[A-Za-z0-9+_.-]+@[A-Za-z0-9.-]+$"
            )) {

            return "Invalid email address";
        }

        if (eventId <= 0) {
            return "Please select an event";
        }

        try {

            if (dao.checkDuplicate(regNo, eventId)) {

                return "Already registered for this event";
            }

            dao.insertRegistration(
                name, regNo, email, eventId
            );

            return "Registration successful";

        } catch (SQLException e) {

            if (e.getErrorCode() == 1062) {
                return "Already registered for this event";
            }

            return "Database error: " + e.getMessage();
        }
    }
}
