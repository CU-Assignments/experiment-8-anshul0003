import java.io.*;
import java.sql.*;
import javax.servlet.*;
import javax.servlet.http.*;

public class EmployeeServlet extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        String empId = request.getParameter("empId");
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();

        try {
            Class.forName("com.mysql.jdbc.Driver");
            Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydb", "root", "password");

            PreparedStatement ps;
            if (empId != null && !empId.isEmpty()) {
                ps = con.prepareStatement("SELECT * FROM employees WHERE id=?");
                ps.setString(1, empId);
            } else {
                ps = con.prepareStatement("SELECT * FROM employees");
            }

            ResultSet rs = ps.executeQuery();

            out.println("<table border='1'><tr><th>ID</th><th>Name</th><th>Position</th></tr>");
            while (rs.next()) {
                out.println("<tr><td>" + rs.getInt("id") + "</td><td>" +
                            rs.getString("name") + "</td><td>" +
                            rs.getString("position") + "</td></tr>");
            }
            out.println("</table>");
            con.close();
        } catch (Exception e) {
            out.println("Error: " + e.getMessage());
        }
    }
}
