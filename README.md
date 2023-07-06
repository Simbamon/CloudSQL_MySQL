# CloudSQL_MySQL

## Install MySQL on Compute Engine
- Spec (Example)
    - Region: asia-northeast1 (Tokyo)
    - Zone: asia-northeast1-b
    - Machine Type: e2-medium (2 vCPU, 4 GB memoery)
    - Boot Disk: Ubuntu 20.04 LTS
    - Size: 10 GB

- Update the system
    ```bash
    sudo apt update
    ```

- Install MySQL
    ```bash
    sudo apt install mysql-server
    ```

- Check the status of MySQL
    ```bash
    sudo systemctl status mysql
    ```
    Check the log to see if there is `active (running)` in green text

- Login to MySQL (root)
    ```bash
    sudo mysql
    ```

- (Optional) Change the password for root user
    ```
    ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '<YOUR_PASSWORD>';
    FLUSH PRIVILEGES;
    ```

## Create Database w/ Sample data
- Create Database (airbnb) w/ sample data 
    ```SQL
    CREATE DATABASE airbnb;

    USE airbnb;

    CREATE TABLE Users(
        id INT PRIMARY KEY AUTO_INCREMENT,
        user_name VARCHAR(255),
        email VARCHAR(255) NOT NULL UNIQUE,
        bio TEXT,
        country VARCHAR(2)
    )

    INSERT INTO Users (user_name, email, bio, country)
    VALUES 
        ('Jeff Pepe', 'jeffp513@gmail.com', 'Sample description 1', 'US'),
        ('Kent Harlan', 'kharl201@gmail.com','Sample description 2', 'US'),
        ('Lory Smelser','smelsL11@gmail.com','Sample description 3', 'US'),
        ('Wesley Harris','harrison73@gmail.com','Sample description 4', 'US'),
        ('Virgil Turner','turner99@gmail.com','Sample description 5', 'US');

    CREATE INDEX email_index ON Users(email);

    CREATE TABLE Rooms (
        id INT AUTO_INCREMENT,
        owner_id INT NOT NULL,
        price INT,
        hot_tub BOOLEAN,
        total_bedrooms int,
        total_bathrooms int,
        address VARCHAR(255),
        created_at datetime,
        updated_at datetime,
        PRIMARY KEY (id),
        FOREIGN KEY (owner_id) REFERENCES Users(id)
    )

    INSERT INTO Rooms (owner_id, price, hot_tub, total_bedrooms, total_bathrooms, address, created_at, updated_at)
    VALUES 
        (1, 100, True, 2, 1, '496 Twp Rd #8 U, Liberty Center, Ohio(OH), 43532', '2021-06-19', '2023-01-23'),
        (1, 150, True, 3, 2, '15136 Township Rd #126, Bellevue, Ohio(OH), 44811', '2015-08-19', '2023-05-15' ),
        (1, 200, False, 6, 3, '4361 Lookout Dr, Loveland, Colorado(CO), 80537', '2019-09-18', '2021-06-11'),
        (2, 200, False, 2, 2, '3925 Blackberry Cir, Saint Cloud, Florida(FL), 34769', '2018-02-28', '2022-08-10'),
        (5, 300, True, 5, 2, '763 Wurlitzer Dr, North Tonawanda, New York(NY), 14120', '2020-03-15', '2023-12-25' );

    CREATE TABLE Bookings(
        id INT AUTO_INCREMENT,
        guest_id INT NOT NULL,
        room_id INT NOT NULL,
        start_date DATETIME,
        end_date DATETIME,
        price int,
        PRIMARY KEY (id),
        FOREIGN KEY (guest_id) REFERENCES Users(id),
        FOREIGN KEY (room_id) REFERENCES Rooms(id)
    );

    INSERT INTO Bookings (guest_id, room_id, start_date, end_date, price)
    VALUES 
        (3, 2, '2023-06-15', '2023-06-20', 750),
        (1, 4, '2023-07-11', '2023-07-12', 400),
        (2, 5, '2023-08-12', '2023-08-15', 1500),
        (4, 1, '2023-09-27', '2023-09-30', 300),
        (5, 3, '2023-10-01', '2023-10-09', 1600);
    ```
    