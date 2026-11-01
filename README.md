# COLORS – COP 4331 LAMP Stack Color Manager

## Description

COLORS is a simple web application built as a class project for **COP 4331 – Processes of Object-Oriented Software Development** at the University of Central Florida. The application demonstrates a full LAMP stack workflow by allowing authenticated users to manage a personal collection of colors. Users can log in, add new colors to their account, and search through their saved colors using partial-match queries.

### Key Features

- **User Authentication** – Cookie-based login with a 20-minute session expiry.
- **Add Colors** – Logged-in users can add color names ("Red", "Blue") to their personal collection.
- **Search Colors** – Users can search their saved colors with substring matching.
- **Logout** – Clears the session cookie and redirects to the login page.

---

## Technologies Used

| Layer | Technology |
|-------|------------|
| **Frontend** | HTML, CSS, JavaScript |
| **Backend** | PHP |
| **Database** | MySQL (via `mysqli` with prepared statements) |
| **Web Server** | Apache (LAMP stack) |
| **Hosting** | Deployed to `nicks711.com` |

---

## Project Structure

```
LAMP Stack/
├── index.html              # Login page (entry point)
├── color.html              # Main application page (add & search colors)
├── css/
│   └── styles.css          # Application styles
├── images/
│   └── background.png      # Background image asset
├── js/
│   ├── code.js             # Client-side logic (login, logout, add/search colors, cookies)
│   └── md5.js              # MD5 hashing library
└── LAMPAPI/
    ├── config.php           # Database credentials (gitignored)
    ├── Login.php            # POST – authenticates a user
    ├── AddColor.php         # POST – adds a color for a user
    └── SearchColors.php     # POST – searches colors by partial name match
```

---

## Setup Instructions

### Prerequisites

- A LAMP server (Linux, Apache, MySQL, PHP) 

### 1. Set Up the Database

Create a MySQL database named `COP4331` and the required tables:

```sql
CREATE DATABASE IF NOT EXISTS COP4331;
USE COP4331;

CREATE TABLE Users (
    ID        INT AUTO_INCREMENT PRIMARY KEY,
    Login     VARCHAR(255) NOT NULL,
    Password  VARCHAR(255) NOT NULL,
    firstName VARCHAR(255) NOT NULL,
    lastName  VARCHAR(255) NOT NULL
);

CREATE TABLE Colors (
    UserId INT          NOT NULL,
    Name   VARCHAR(255) NOT NULL
);
```

### 2. Configure Database Credentials

Create a file at `LAMPAPI/config.php` and fill in your MySQL credentials:

```php
<?php
    $db_host = "localhost";
    $db_user = "your_username";
    $db_pass = "your_password";
    $db_name = "COP4331";
?>
```

> **Note:** `LAMPAPI/config.php` is listed in `.gitignore` and should never be committed.

### 3. Deploy the Files

Copy the entire project directory into your Apache web root (e.g., `/var/www/html/`).

### 4. Update the API Base URL

Open `js/code.js` and update the `urlBase` variable on line 1 to point to your server:

```js
const urlBase = 'http://<your-domain-or-localhost>/LAMPAPI';
```

---

## How to Run and Access the Application

1. Start your Apache and MySQL services.
2. Navigate to `http://<your-domain-or-localhost>/index.html` in a web browser.
3. Log in with a valid user account.
4. After logging in you will be redirected to the color management page where you can add and search colors.

The production deployment is accessible at **http://nicks711.com**.

---

## API Endpoints

All endpoints accept and return **JSON** via **POST** requests.

| Endpoint | Request Body | Response |
|----------|-------------|----------|
| `/LAMPAPI/Login.php` | `{ login, password }` | `{ id, firstName, lastName, error }` |
| `/LAMPAPI/AddColor.php` | `{ color, userId }` | `{ error }` |
| `/LAMPAPI/SearchColors.php` | `{ search, userId }` | `{ results: [...], error }` |

---

## Assumptions & Limitations

- The application is designed as a class project to demonstrate LAMP stack fundamentals.
- Users are added directly to the database.
- The application supports adding and searching colors.

---

## AI Usage Disclosure

Used Claude to review and verify my plan for organizing the repository along with assistance analyzing the repo and assisting the creation of the README.
