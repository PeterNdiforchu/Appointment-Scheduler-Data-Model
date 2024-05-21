# Sample Coconut Software Appointment Scheduler Data Model

To create a data model similar to that of an appointment scheduling platform, we need to consider the key entities and their relationships. Here are the primary entities we need:

1. **User**: Represents individuals using the platform.
2. **EventType**: Represents different types of events that users can create and manage.
3. **Event**: Represents scheduled events/appointments.
4. **Availability**: Represents the time slots during which users are available for scheduling.
5. **Booking**: Represents the actual booking of an event by another user.
6. **Notification**: Represents notifications sent to users about bookings and events.
### Step-by-Step Breakdown of the Data Model:
1. **User Table**:
    - **UserID** (Primary Key)
    - **Name**
    - **Email**
    - **Password** (Encrypted)
    - **Timezone**
    - **CreatedAt**
    - **UpdatedAt**
2. **EventType Table**:
    - **EventTypeID** (Primary Key)
    - **UserID** (Foreign Key to User)
    - **Title**
    - **Description**
    - **Duration** (e.g., 30 minutes, 1 hour)
    - **CreatedAt**
    - **UpdatedAt**
3. **Event Table**:
    - **EventID** (Primary Key)
    - **EventTypeID** (Foreign Key to EventType)
    - **UserID** (Foreign Key to User, for the organizer)
    - **Title**
    - **Description**
    - **StartTime**
    - **EndTime**
    - **Location** (e.g., Zoom link, physical address)
    - **CreatedAt**
    - **UpdatedAt**
4. **Availability Table**:
    - **AvailabilityID** (Primary Key)
    - **UserID** (Foreign Key to User)
    - **DayOfWeek** (e.g., Monday, Tuesday)
    - **StartTime** (Time)
    - **EndTime** (Time)
    - **CreatedAt**
    - **UpdatedAt**
5. **Booking Table**:
    - **BookingID** (Primary Key)
    - **EventID** (Foreign Key to Event)
    - **UserID** (Foreign Key to User, for the person booking the event)
    - **Status** (e.g., Pending, Confirmed, Cancelled)
    - **CreatedAt**
    - **UpdatedAt**
6. **Notification Table**:
    - **NotificationID** (Primary Key)
    - **UserID** (Foreign Key to User)
    - **Message**
    - **SentAt**
### Relationships and Constraints:
- **User** can create multiple **EventTypes**.
- **User** can have multiple **Availabilities**.
- **EventType** is associated with one **User** but can generate multiple **Events**.
- **Event** can have multiple **Bookings** but each **Booking** is associated with only one **Event**.
- **Booking** is made by one **User** but can have multiple **Notifications**.
- **Notification** is sent to one **User**.
### Example of SQL Table Creation:
```sql
CREATE TABLE User (
    UserID INT PRIMARY KEY AUTO_INCREMENT,
    Name VARCHAR(100),
    Email VARCHAR(100) UNIQUE,
    Password VARCHAR(255),
    Timezone VARCHAR(50),
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UpdatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

CREATE TABLE EventType (
    EventTypeID INT PRIMARY KEY AUTO_INCREMENT,
    UserID INT,
    Title VARCHAR(100),
    Description TEXT,
    Duration INT,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UpdatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (UserID) REFERENCES User(UserID)
);

CREATE TABLE Event (
    EventID INT PRIMARY KEY AUTO_INCREMENT,
    EventTypeID INT,
    UserID INT,
    Title VARCHAR(100),
    Description TEXT,
    StartTime DATETIME,
    EndTime DATETIME,
    Location VARCHAR(255),
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UpdatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (EventTypeID) REFERENCES EventType(EventTypeID),
    FOREIGN KEY (UserID) REFERENCES User(UserID)
);

CREATE TABLE Availability (
    AvailabilityID INT PRIMARY KEY AUTO_INCREMENT,
    UserID INT,
    DayOfWeek VARCHAR(10),
    StartTime TIME,
    EndTime TIME,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UpdatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (UserID) REFERENCES User(UserID)
);

CREATE TABLE Booking (
    BookingID INT PRIMARY KEY AUTO_INCREMENT,
    EventID INT,
    UserID INT,
    Status VARCHAR(20),
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UpdatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (EventID) REFERENCES Event(EventID),
    FOREIGN KEY (UserID) REFERENCES User(UserID)
);

CREATE TABLE Notification (
    NotificationID INT PRIMARY KEY AUTO_INCREMENT,
    UserID INT,
    Message TEXT,
    SentAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (UserID) REFERENCES User(UserID)
);
```
This data model captures the essential aspects of an appointment scheduling system, enabling users to manage their availability, create event types, schedule events, and handle bookings and notifications efficiently.

