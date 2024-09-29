# Smart Ambulance Service

Smart Ambulance is a novel solution to streamline emergency medical services. With just a click, patients can request an ambulance, track its location in real-time, and receive critical details about the driver and vehicle. The platform offers a dual interface: one for patients in need and another for ambulance drivers.

## Features

- **User Interface**:
  - **Patient Side**: Users can request an ambulance in an emergency, view real-time location updates on a map, and get details about the assigned ambulance and driver.
  - **Driver Side**: Drivers share their live location with the system upon starting their service. Future plans include integrating a map for drivers to navigate directly to patients using Google Maps.

- **Real-time Driver Assignment**:
  - Patient requests are queued in Redis using a list data structure.
  - A dedicated service processes the queue, assigns the nearest available ambulance, and makes a gRPC call to the Login server (connected to MongoDB) to retrieve the driver and vehicle details.
  - Once the assignment is complete, the user receives driver coordinates, and the driver is connected to the user via WebSocket for live tracking.

- **WebSocket Communication**:
  - **User Side**: Once an ambulance is assigned, the patient is placed in a socket room using the driver’s ID. This allows for continuous live location updates of the ambulance.
  - **Driver Side**: Drivers are automatically connected to the socket server upon app launch and join a room where they wait for a user connection. Their location is continuously updated and sent to a Kafka topic.

- **Kafka Event Streaming**:
  - Kafka is used as the intermediary for real-time event streaming. The socket server acts as a producer, sending driver locations to Kafka, while another service consumes the data to track and update all active ambulances.
  - This ensures scalability and efficient management of location updates across the platform.

- **Geo-Spatial Data in Redis**:
  - Driver locations are stored in Redis using geospatial indexing with the driver’s ID as the key. This allows for fast and efficient geolocation queries, enabling the system to assign the closest available driver to a patient.

## Architecture
<img width="1460" alt="Smart_Ambulance_System_Architecture" src="https://github.com/user-attachments/assets/9f318e3c-0280-4ff0-ba8d-1efb174fdbff">


- **Frontend**:
  - Patient and driver apps are built with map integration to track live locations.
  - Map integration for drivers is planned and will redirect to Google Maps for navigation with patient coordinates.

- **Backend**:
  - Redis for queuing and geospatial data storage.
  - gRPC for communication between the assignment server and the login server (MongoDB).
  - Kafka for scalable event streaming.
  - WebSocket for real-time communication between patients and drivers.

## Future Enhancements

- Complete map integration for drivers to allow real-time navigation directly within the app.
- Further scalability improvements and optimizations for handling high traffic situations.
- Advanced features such as predictive analysis for better ambulance dispatch using historical data.

## Tech Stack

- **Frontend**: React (for patient and driver apps)
- **Backend**: Node.js, Redis, MongoDB, Kafka, gRPC, Socket.IO

## Contributing

I welcome contributions! If you'd like to help, please fork the repository and submit a pull request. Make sure to include relevant tests and documentation updates.

