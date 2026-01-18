# 🎟️ BookMyShow – Database Design & SQL Assignment

## 📌 Problem Statement
BookMyShow is a ticket booking platform where users can book tickets for movie shows.  
For a given **theatre**, users can:
- View the **next 7 dates**
- Select a date
- See the **list of movies running** along with their **show timings**

This assignment focuses on **database design, normalization, and SQL queries**.

---

## 🧩 Part P1 – Entity Identification & Database Design

### 📦 Identified Entities
1. Theatre  
2. Screen  
3. Movie  
4. Show  

The design follows:
- No data redundancy  
- High scalability  
- Compliance with **1NF, 2NF, 3NF, and BCNF**

---

## 🗂️ Entity Details & Attributes

### 1️⃣ Theatre
| Column | Type | Description |
|------|------|-------------|
| theatre_id | INT (PK) | Unique theatre identifier |
| name | VARCHAR | Theatre name |
| city | VARCHAR | City |
| address | VARCHAR | Theatre address |

---

### 2️⃣ Screen
| Column | Type | Description |
|------|------|-------------|
| screen_id | INT (PK) | Unique screen |
| theatre_id | INT (FK) | Theatre reference |
| screen_number | INT | Screen number |
| capacity | INT | Total seats |

---

### 3️⃣ Movie
| Column | Type | Description |
|------|------|-------------|
| movie_id | INT (PK) | Unique movie |
| title | VARCHAR | Movie name |
| language | VARCHAR | Language |
| duration_minutes | INT | Duration |
| certification | VARCHAR | UA / U / A |

---

### 4️⃣ Show
| Column | Type | Description |
|------|------|-------------|
| show_id | INT (PK) | Unique show |
| movie_id | INT (FK) | Movie reference |
| screen_id | INT (FK) | Screen reference |
| show_date | DATE | Show date |
| start_time | TIME | Start time |
| end_time | TIME | End time |

---

## ✅ Normalization

### ✔ First Normal Form (1NF)
- Atomic values
- No repeating groups

### ✔ Second Normal Form (2NF)
- No partial dependency

### ✔ Third Normal Form (3NF)
- No transitive dependency

### ✔ Boyce-Codd Normal Form (BCNF)
- Every determinant is a candidate key

✔ **Schema satisfies BCNF**

---

## 🛠️ SQL Table Creation

```sql
CREATE TABLE theatre (
    theatre_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    city VARCHAR(50),
    address VARCHAR(255)
);

CREATE TABLE screen (
    screen_id INT PRIMARY KEY AUTO_INCREMENT,
    theatre_id INT,
    screen_number INT,
    capacity INT,
    FOREIGN KEY (theatre_id) REFERENCES theatre(theatre_id)
);

CREATE TABLE movie (
    movie_id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(150),
    language VARCHAR(50),
    duration_minutes INT,
    certification VARCHAR(10)
);

CREATE TABLE show_details (
    show_id INT PRIMARY KEY AUTO_INCREMENT,
    movie_id INT,
    screen_id INT,
    show_date DATE,
    start_time TIME,
    end_time TIME,
    FOREIGN KEY (movie_id) REFERENCES movie(movie_id),
    FOREIGN KEY (screen_id) REFERENCES screen(screen_id)
);
```

### 📊 Sample Data Insertion
```sql
INSERT INTO theatre (name, city, address)
VALUES ('PVR Nexus Forum', 'Bengaluru', 'Koramangala');

INSERT INTO screen (theatre_id, screen_number, capacity)
VALUES 
(1, 1, 200),
(1, 2, 180);

INSERT INTO movie (title, language, duration_minutes, certification)
VALUES 
('Dasara', 'Telugu', 156, 'UA'),
('Kisi Ka Bhai Kisi Ki Jaan', 'Hindi', 144, 'UA');

INSERT INTO show_details (movie_id, screen_id, show_date, start_time, end_time)
VALUES
(1, 1, '2024-04-25', '12:15:00', '14:50:00'),
(1, 1, '2024-04-25', '18:30:00', '21:05:00'),
(2, 2, '2024-04-25', '16:40:00', '19:05:00');
```

## 🔍 Part P2 – Query Requirement
### 🎯 Requirement
List all movies and their show timings for a given:
 - Theatre
 - Date

### SQL Query
```sql
SELECT 
    m.title AS movie_name,
    sd.show_date,
    sd.start_time,
    sd.end_time,
    sc.screen_number
FROM show_details sd
JOIN movie m ON sd.movie_id = m.movie_id
JOIN screen sc ON sd.screen_id = sc.screen_id
JOIN theatre t ON sc.theatre_id = t.theatre_id
WHERE t.name = 'PVR Nexus Forum'
  AND sd.show_date = '2024-04-25'
ORDER BY m.title, sd.start_time;
```

### Sample Output
| Movie Name | Date | Start Time | Date | Start Time |
|------|------|-------------|--------------|----------|
| Dasara | 2024-04-25 | 12:15 | 14:50 | 1 |
| Dasara | 2024-04-25 | 18:30 | 21:05 | 1 |
| Kisi Ka Bhai Kisi Ki Jaan | 2024-04-25 | 16:40 | 19:05 | 2 |
