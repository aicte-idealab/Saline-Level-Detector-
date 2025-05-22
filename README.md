
# Saline Level Detector for Hospital Management

## Introduction
The **Saline Level Detector** is a system designed to monitor and track saline levels in hospital patients. It provides real-time alerts to healthcare professionals when saline levels reach critical thresholds, helping to prevent complications related to improper saline administration.

## Features
- **Real-Time Monitoring**: Constantly tracks saline levels in real time.
- **Threshold Alerts**: Sends automatic alerts when saline levels are too low or too high.
- **Data Logging**: Stores saline level readings for later analysis.
- **Hospital Management Integration**: Can be integrated with hospital management software for better coordination.
- **User-Friendly Interface**: Easy-to-use web interface for medical staff to monitor and adjust saline levels.

## Technology Stack
- **Hardware**:
  - Microcontroller (e.g., Arduino Uno, Raspberry Pi)
  - Saline level sensor (e.g., ultrasonic or capacitive)
  - Wi-Fi module (e.g., ESP8266 for Arduino or built-in for Raspberry Pi)
- **Software**:
  - **Frontend**: HTML, CSS, JavaScript
  - **Backend**: Node.js, Express
  - **Database**: MongoDB or MySQL
  - **Communication Protocol**: MQTT for real-time notifications
  - **Firmware**: Arduino IDE (for Arduino) or Python (for Raspberry Pi)

## Requirements
- Microcontroller (e.g., Arduino Uno or Raspberry Pi)
- Saline level sensor (capacitive recommended for non-conductive containers)
- Wi-Fi module (e.g., ESP8266 for Arduino)
- Power supply
- Laptop or server to run the backend and frontend
- MQTT broker (e.g., Mosquitto, HiveMQ, or AWS IoT)

## Installation

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/saline-level-detector.git
```
*Note*: Replace `yourusername` with the actual repository owner.

### 2. Install Dependencies
- **Backend (Node.js)**:
  ```bash
  cd saline-level-detector/backend
  npm install
  ```
- **Frontend (HTML, CSS, JavaScript)**:
  - The frontend is a static web application (no build tools required).
  - Ensure the `frontend` directory contains `index.html`, `styles.css`, and `script.js`.
  - Serve the frontend using a simple HTTP server (e.g., `http-server`):
    ```bash
    cd saline-level-detector/frontend
    npm install -g http-server
    http-server
    ```
  - Access the frontend at `http://localhost:8080`.

### 3. Set Up Database
- **MongoDB**:
  - Install MongoDB locally or use [MongoDB Atlas](https://www.mongodb.com/cloud/atlas).
  - Create a database named `saline_level_db`.
  - Example collection schema:
    ```json
    {
      "timestamp": ISODate("2025-05-08T12:00:00Z"),
      "saline_level": 75.5,
      "patient_id": "PAT123",
      "alert_status": "normal"
    }
    ```
- **MySQL**:
  - Install MySQL locally or use a cloud-based service.
  - Create a database named `saline_level_db` with a table:
    ```sql
    CREATE TABLE saline_readings (
      id INT AUTO_INCREMENT PRIMARY KEY,
      timestamp DATETIME,
      saline_level FLOAT,
      patient_id VARCHAR(50),
      alert_status VARCHAR(20)
    );
    ```

### 4. Configure Microcontroller
- **Arduino**:
  - Open the firmware code in Arduino IDE (located in the `firmware` directory).
  - Configure Wi-Fi credentials:
    ```cpp
    #include <WiFi.h>
    const char* ssid = "your-SSID";
    const char* password = "your-PASSWORD";
    void setup() {
      WiFi.begin(ssid, password);
      while (WiFi.status() != WL_CONNECTED) {
        delay(1000);
      }
    }
    ```
  - Connect the saline level sensor to the appropriate pins (refer to sensor documentation).
  - Upload the firmware to the Arduino.
- **Raspberry Pi**:
  - Install Python and required libraries (e.g., `paho-mqtt`).
  - Configure the Wi-Fi and sensor in the Python script (located in the `firmware` directory).
  - Run the script:
    ```bash
    python saline_sensor.py
    ```
- Calibrate the sensor to ensure accurate readings (e.g., set thresholds for empty and full saline containers).
- Configure MQTT communication:
  ```cpp
  #include <PubSubClient.h>
  const char* mqtt_server = "your-mqtt-broker.com";
  WiFiClient espClient;
  PubSubClient client(espClient);
  void setup() {
    client.setServer(mqtt_server, 1883);
  }
  ```

### 5. Set Up MQTT Broker
- **Local Broker**:
  - Install Mosquitto:
    ```bash
    sudo apt-get install mosquitto
    sudo systemctl enable mosquitto
    ```
  - Enable MQTT over WebSocket if needed (edit `mosquitto.conf`).
- **Cloud Broker**:
  - Use a service like [HiveMQ](https://www.hivemq.com/) or [AWS IoT](https://aws.amazon.com/iot/).
- Configure the microcontroller to connect to the MQTT broker using the broker’s address and credentials.

### 6. Run the Backend Server
- Start the backend server:
  ```bash
  cd saline-level-detector/backend
  npm start
  ```
- The backend will run on `http://localhost:3000` (or the configured port).
- Ensure the backend is connected to the database and MQTT broker.

### 7. Access the Frontend
- Open a browser and navigate to `http://localhost:8080` (or the port used by `http-server`).
- The frontend displays real-time saline levels and alerts, fetched from the backend via API calls.

## Images
The following images provide visual references for setting up and using the Saline Level Detector system:

- **Hardware Setup**:
  - *Arduino with Sensor and Wi-Fi Module*: Shows the Arduino Uno connected to a capacitive saline level sensor and an ESP8266 Wi-Fi module. Refer in the repository for pin connections.
        ![Screenshot 2025-05-08 103935](https://github.com/user-attachments/assets/10a87263-161e-422e-8f81-0bb77908862d)

  - *Raspberry Pi Setup*: Illustrates the Raspberry Pi with a connected sensor.

- **Frontend Interface**:

  - *Dashboard Screenshot*: Displays the web interface with real-time saline level readings, alerts, and historical data charts. Available at ![Screenshot 2025-05-08 102122](https://github.com/user-attachments/assets/075bbe2a-22f2-4873-a546-fe739d1503bf)


- **System Architecture**:

  - *Architecture Diagram*: A diagram showing the data flow from the microcontroller to the MQTT broker, backend, database, and frontend. See ![Screenshot 2025-05-08 102211](https://github.com/user-attachments/assets/da7afc8e-5fac-4f4d-9ee1-f0fa304e80dc)


## Testing
1. Simulate saline level changes (e.g., adjust the sensor to mimic low or high levels).
2. Verify that the microcontroller sends data to the MQTT broker.
3. Check that the backend receives and logs data in the database.
4. Confirm that the frontend displays updated saline levels and alerts for critical thresholds (e.g., saline level < 10% or > 90%).



## Security Considerations
- **MQTT**: Use TLS/SSL for secure communication (configure the broker to support encrypted connections).
- **Database**: Encrypt sensitive patient data and restrict access (e.g., use authentication and role-based access control).
- **Wi-Fi**: Secure the network with a strong password and WPA3 encryption if available.
- **Frontend**: Sanitize API responses to prevent XSS attacks.

## Troubleshooting
- **Wi-Fi Disconnection**: Implement reconnection logic in the microcontroller code:
  ```cpp
  void reconnect() {
    while (!client.connected()) {
      if (client.connect("MicrocontrollerClient")) {
        client.subscribe("saline/level");
      }
    }
  }
  ```
- **Sensor Errors**: Verify sensor calibration and check for loose connections.
- **MQTT Issues**: Ensure the broker is running and accessible (test with an MQTT client like MQTT Explorer).
- **Frontend Issues**: Check browser console for JavaScript errors and verify API connectivity.

## Conclusion
The Saline Level Detector system enables accurate monitoring of saline levels, reducing errors and improving patient care. The integration with real-time alerting, data logging, and a user-friendly web interface ensures reliable operation in hospital environments.

## Contributing
Contributions are welcome! Please submit a pull request or open an issue on the [GitHub repository](https://github.com/yourusername/saline-level-detector).

## License
This project is licensed under the MIT License. See the `LICENSE` file for details.
