import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.time.OffsetDateTime;
import java.time.format.DateTimeFormatter;

public class FlightPredictorUI extends JFrame {

    private JLabel flightNumberLabel;
    private JTextField flightNumberField;
    private JLabel airlineCodeLabel;
    private JTextField airlineCodeField;
    private JLabel dateLabel;
    private JTextField dateField;
    private JButton predictButton;
    private JLabel resultLabel;

    public FlightPredictorUI() {
        super("Flight Time Predictor");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setSize(400, 250);
        setLayout(new GridLayout(5, 2, 10, 10));
        setLocationRelativeTo(null);

        // Flight Number
        flightNumberLabel = new JLabel("Flight Number:");
        flightNumberField = new JTextField();
        add(flightNumberLabel);
        add(flightNumberField);

        // Airline Code
        airlineCodeLabel = new JLabel("Airline Code:");
        airlineCodeField = new JTextField();
        add(airlineCodeLabel);
        add(airlineCodeField);

        // Date
        dateLabel = new JLabel("Date (YYYY-MM-DD):");
        dateField = new JTextField();
        add(dateLabel);
        add(dateField);

        // Predict Button
        predictButton = new JButton("Predict");
        predictButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String flightNumber = flightNumberField.getText();
                String airlineCode = airlineCodeField.getText();
                String date = dateField.getText();

                FlightData flightData = FlightPredictor.getFlightData(flightNumber, airlineCode, date);

                if (flightData != null) {
                    resultLabel.setText("<html>Predicted Takeoff Time: " + flightData.getTakeoffTime() + "<br>" +
                            "Predicted Arrival Time: " + flightData.getArrivalTime() + "</html>");
                } else {
                    resultLabel.setText("No flight data found.");
                }
            }
        });
        add(predictButton);

        // Result Label
        resultLabel = new JLabel("");
        add(new JLabel()); // Empty label to balance layout
        add(resultLabel);

        setVisible(true);
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> new FlightPredictorUI());
    }

    // FlightData class (same as before)
    static class FlightData {
        private OffsetDateTime takeoffTime;
        private OffsetDateTime arrivalTime;

        public FlightData(String takeoffTimeStr, String arrivalTimeStr) {
            DateTimeFormatter formatter = DateTimeFormatter.ISO_OFFSET_DATE_TIME;
            this.takeoffTime = OffsetDateTime.parse(takeoffTimeStr, formatter);
            this.arrivalTime = OffsetDateTime.parse(arrivalTimeStr, formatter);
        }

        public OffsetDateTime getTakeoffTime() {
            return takeoffTime;
        }

        public OffsetDateTime getArrivalTime() {
            return arrivalTime;
        }
    }

    // FlightPredictor class (same as before)
    static class FlightPredictor {
        private static final String API_KEY = "YOUR_API_KEY"; // Replace with your Aviation Edge API key
        private static final String BASE_URL = "https://api.aviationedge.com/v2/public/flights";

        private static FlightData getFlightData(String flightNumber, String airlineCode, String date) {
            // (Implementation is the same as before)
        }
    }
}
