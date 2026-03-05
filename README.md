# Flight Reservation System

## Project Overview
The Flight Reservation System is a comprehensive application designed to manage flight bookings efficiently. It provides a user-friendly interface for users to search, book, and manage their flight reservations. This project utilizes modern software engineering principles and best practices in DevOps to ensure reliability and scalability.

## Tools & Technologies
- **Programming Language**: Python
- **Framework**: Flask
- **Database**: PostgreSQL
- **Frontend**: React
- **Containerization**: Docker
- **CI/CD**: GitHub Actions
- **Version Control**: Git

## Features
- User Registration and Authentication
- Flight Search and Booking
- View and Manage Bookings
- Admin Dashboard for Flight Management
- Email Notifications for Booking Confirmation
- Payment Gateway Integration

## Step-by-Step Setup Instructions
1. **Clone the repository**:
   ```bash
   git clone https://github.com/yash-deshpande24/Flight-Reservation-System.git
   cd Flight-Reservation-System
2. **Set up the Python environment**:
   ```bash
    python -m venv venv
   source venv/bin/activate  # On Windows use venv\Scripts\activate
## Installation & Setup

### Install dependencies:
```bash
pip install -r requirements.txt
```

### Set up the database:
Ensure PostgreSQL is installed and running. Create a database named `flight_reservation` and update the ORM configuration in `config.py`.

```bash
psql -U postgres
CREATE DATABASE flight_reservation;
```

### Run migrations:
```bash
flask db upgrade
```

### Start the application:
```bash
flask run
```

### Access the application:
Open your web browser and go to `http://127.0.0.1:5000/`.

---

## Cleanup Procedures

### Stop the application:
```bash
# If running in the terminal, you can stop it with CTRL+C
```

### Remove the virtual environment:
```bash
deactivate
rm -rf venv  # Be careful with this command
```

### Drop the database (if necessary):
```bash
psql -U postgres
DROP DATABASE flight_reservation;
```

### Delete the cloned repository:
```bash
cd ..
rm -rf Flight-Reservation-System
```

---

This documentation provides a solid foundation for understanding, setting up, and managing the Flight Reservation System DevOps project.   
   
   
   
