CREATE DATABASE campus_events;

USE campus_events;

CREATE TABLE events (

    event_id INT PRIMARY KEY AUTO_INCREMENT,

    event_name VARCHAR(100) NOT NULL
);

CREATE TABLE registrations (

    id INT PRIMARY KEY AUTO_INCREMENT,

    student_name VARCHAR(100) NOT NULL,

    register_no VARCHAR(30) NOT NULL,

    email VARCHAR(100) NOT NULL,

    event_id INT NOT NULL,

    registration_date TIMESTAMP
        DEFAULT CURRENT_TIMESTAMP,

    FOREIGN KEY (event_id)
        REFERENCES events(event_id),

    UNIQUE (register_no, event_id)
);

INSERT INTO events (event_name)
VALUES
('Hackathon'),
('Coding Competition'),
('Sports Meet'),
('Cultural Fest');
