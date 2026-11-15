import java.sql.*;

import java.util.*;

public class RegistrationDAO {

    public List<String> getEvents()
            throws SQLException {

        List<String> events = new ArrayList<>();

        String sql =
            "SELECT event_name FROM events";

        try (
            Connection con =
                DBConnection.getConnection();

            PreparedStatement ps =
                con.prepareStatement(sql);

            ResultSet rs = ps.executeQuery()
        ) {

            while (rs.next()) {

                events.add(
                    rs.getString("event_name")
                );
            }
        }

        return events;
    }

    public boolean checkDuplicate(
            String regNo, int eventId)
            throws SQLException {

        String sql =
            "SELECT id FROM registrations " +
            "WHERE register_no = ? AND event_id = ?";

        try (
            Connection con =
                DBConnection.getConnection();

            PreparedStatement ps =
                con.prepareStatement(sql)
        ) {

            ps.setString(1, regNo);
            ps.setInt(2, eventId);

            try (ResultSet rs = ps.executeQuery()) {

                return rs.next();
            }
        }
    }

    public void insertRegistration(
            String name,
            String regNo,
            String email,
            int eventId)
            throws SQLException {

        String sql =
            "INSERT INTO registrations " +
            "(student_name, register_no, email, event_id) " +
            "VALUES (?, ?, ?, ?)";

        try (
            Connection con =
                DBConnection.getConnection();

            PreparedStatement ps =
                con.prepareStatement(sql)
        ) {

            ps.setString(1, name);
            ps.setString(2, regNo);
            ps.setString(3, email);
            ps.setInt(4, eventId);

            ps.executeUpdate();
        }
    }

    public List<String[]> getRegistrations(
            int eventId, int limit, int offset)
            throws SQLException {

        List<String[]> records = new ArrayList<>();

        String sql =
            "SELECT r.register_no, r.student_name, " +
            "r.email, e.event_name " +
            "FROM registrations r " +
            "JOIN events e ON r.event_id = e.event_id ";

        if (eventId != -1) {
            sql += "WHERE r.event_id = ? ";
        }

        sql +=
            "ORDER BY r.id LIMIT ? OFFSET ?";

        try (
            Connection con =
                DBConnection.getConnection();

            PreparedStatement ps =
                con.prepareStatement(sql)
        ) {

            int index = 1;

            if (eventId != -1) {
                ps.setInt(index++, eventId);
            }

            ps.setInt(index++, limit);
            ps.setInt(index, offset);

            try (ResultSet rs = ps.executeQuery()) {

                while (rs.next()) {

                    records.add(
                        new String[]{
                            rs.getString("register_no"),
                            rs.getString("student_name"),
                            rs.getString("email"),
                            rs.getString("event_name")
                        }
                    );
                }
            }
        }

        return records;
    }

    public int getEventId(String eventName)
            throws SQLException {

        String sql =
            "SELECT event_id FROM events " +
            "WHERE event_name = ?";

        try (
            Connection con =
                DBConnection.getConnection();

            PreparedStatement ps =
                con.prepareStatement(sql)
        ) {

            ps.setString(1, eventName);

            try (ResultSet rs = ps.executeQuery()) {

                if (rs.next()) {
                    return rs.getInt("event_id");
                }
            }
        }

        return -1;
    }
}
