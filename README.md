# Hotel Management System

This is a comprehensive Hotel Management System built as a Windows Forms desktop application using C# and .NET Framework. It provides functionalities for managing guests, rooms, and reservations, interacting with a MySQL database as the backend.

## Features

- **User Authentication**: Secure login system for authorized personnel.
- **Dashboard**: A central hub providing an overview and quick access to different modules.
- **Guest Management**: Full CRUD (Create, Read, Update, Delete) operations for guest records.
- **Room Management**:
    - Add, update, and delete rooms.
    - Categorize rooms (e.g., Single, Double, Suite).
    - Manage room status (Free/Busy).
- **Reservation Management**:
    - Create, update, and delete reservations for guests.
    - Automatically updates room status upon reservation.
    - Filters available rooms by type for easy booking.
- **Intuitive UI**: A clean and user-friendly interface built with the Guna UI2 Framework for a modern look and feel.

## Technology Stack

- **Frontend**: C# with Windows Forms (.NET Framework 4.7.2)
- **Database**: MySQL
- **UI Library**: Guna UI2 for WinForms

## Prerequisites

- **Visual Studio**: Version 2019 or later recommended.
- **.NET Framework**: Version 4.7.2.
- **MySQL Server**: A running instance of MySQL or a compatible server like MariaDB.
- **MySQL Client Tools**: MySQL Workbench or any other tool to interact with the database.

## Installation and Setup

### 1. Clone the Repository

```bash
git clone https://github.com/Arsany-Naim/Hotel-Management-System.git
cd Hotel-Management-System
```

### 2. Database Setup

You need to create a MySQL database named `hotel_data` and set up the required tables.

1.  Connect to your MySQL server.
2.  Create the database:
    ```sql
    CREATE DATABASE hotel_data;
    ```
3.  Use the new database:
    ```sql
    USE hotel_data;
    ```
4.  Run the following SQL scripts to create the necessary tables:

    ```sql
    -- Table for room categories (types)
    CREATE TABLE `category` (
      `CategoryId` int(11) NOT NULL AUTO_INCREMENT,
      `Label` varchar(100) NOT NULL,
      `Price` decimal(10,2) NOT NULL,
      PRIMARY KEY (`CategoryId`)
    );
    
    -- Table for user login
    CREATE TABLE `users` (
      `id` int(11) NOT NULL AUTO_INCREMENT,
      `username` varchar(50) NOT NULL,
      `password` varchar(50) NOT NULL,
      PRIMARY KEY (`id`)
    );

    -- Table for guest information
    CREATE TABLE `guest` (
      `GuestId` varchar(20) NOT NULL,
      `GuestFirstName` varchar(100) NOT NULL,
      `GuestLastName` varchar(100) DEFAULT NULL,
      `GuestPhone` varchar(20) NOT NULL,
      `GuestCity` varchar(100) DEFAULT NULL,
      PRIMARY KEY (`GuestId`)
    );
    
    -- Table for room details
    CREATE TABLE `room` (
      `RoomId` varchar(10) NOT NULL,
      `RoomType` int(11) NOT NULL,
      `RoomPhone` varchar(20) DEFAULT NULL,
      `RoomStatus` varchar(20) NOT NULL DEFAULT 'Free',
      PRIMARY KEY (`RoomId`),
      KEY `fk_type_id` (`RoomType`),
      CONSTRAINT `fk_type_id` FOREIGN KEY (`RoomType`) REFERENCES `category` (`CategoryId`) ON DELETE CASCADE
    );

    -- Table for reservations
    CREATE TABLE `reservation` (
      `RecervId` int(11) NOT NULL AUTO_INCREMENT,
      `GuestId` varchar(20) NOT NULL,
      `RoomNo` varchar(10) NOT NULL,
      `DateIn` date NOT NULL,
      `DateOut` date NOT NULL,
      PRIMARY KEY (`RecervId`),
      KEY `fk_guest_id` (`GuestId`),
      KEY `fk_room_no` (`RoomNo`),
      CONSTRAINT `fk_guest_id` FOREIGN KEY (`GuestId`) REFERENCES `guest` (`GuestId`) ON DELETE CASCADE,
      CONSTRAINT `fk_room_no` FOREIGN KEY (`RoomNo`) REFERENCES `room` (`RoomId`) ON DELETE CASCADE
    );
    ```

5.  **(Optional but Recommended)** Insert sample data for categories and a default user to log in.

    ```sql
    -- Insert room types
    INSERT INTO `category` (`CategoryId`, `Label`, `Price`) VALUES
    (1, 'Single', 100.00),
    (2, 'Double', 180.00),
    (3, 'Family', 250.00),
    (4, 'Suite', 400.00);

    -- Insert a default user (username: admin, password: admin)
    INSERT INTO `users` (`username`, `password`) VALUES ('admin', 'admin');
    ```

### 3. Configure Connection String

Open the project in Visual Studio and navigate to the `DBConnect.cs` file. Modify the connection string to match your MySQL server's configuration (username and password).

```csharp
// Path: Hotel Management System/DBConnect.cs

private MySqlConnection connection = new MySqlConnection("datasource=localhost;port=3306;username=root;password=your_password;database=hotel_data");
```
Replace `root` and `your_password` with your MySQL username and password.

### 4. Build and Run

1.  Open the `Hotel Management System.sln` file in Visual Studio.
2.  The necessary NuGet packages (`Guna.UI2.WinForms` and `MySql.Data`) should be restored automatically. If not, right-click the solution in the Solution Explorer and select "Restore NuGet Packages".
3.  Press `F5` or click the "Start" button to build and run the application.

## How to Use

1.  The application will start with a splash screen, followed by the Login form.
2.  Enter the user credentials. If you used the sample data, the credentials are:
    - **Username**: `admin`
    - **Password**: `admin`
3.  Upon successful login, the main dashboard will be displayed.
4.  Use the navigation panel on the left to access different management modules:
    - **Guest**: Add new guests or manage existing ones.
    - **Room**: Add new rooms, specify their type, and manage their status.
    - **Reception**: Manage all guest reservations. You can add, update, or cancel a reservation. Available rooms are filtered by the selected room type.
