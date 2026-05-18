# Job Application Tracker (Flask & MySQL)

A Full-Stack web application designed to help job seekers organize, track, and manage their job applications in one place. This project demonstrates CRUD operations, database integration, and dynamic web rendering using Python and MySQL.

## 🚀 Features
- **Create:** Add new job applications with Company Name, Job Title, and Date.
- **Read:** View a dynamically updated table of all submitted applications.
- **Update:** Easily change the status of an application (Applied, Interview, Rejected, No Answer) using a dropdown menu.
- **Delete:** Remove outdated or incorrect entries with a single click and a safety confirmation prompt.

## 🛠️ Tech Stack
- **Backend:** Python, Flask Framework
- **Database:** MySQL
- **Frontend:** HTML5, CSS3, Jinja2 Templating
- **Database Connector:** `mysql-connector-python`

## 💻 Setup & Installation
1. **Database Setup:**
   - Start MySQL via XAMPP or MySQL Workbench.
   - Create a database named `job_tracker_db` and execute the SQL query to create the `applications` table.
2. **Clone the repository:**
   ```bash
   git clone [https://github.com/Dovah-bah/job-application-tracker-python.git](https://github.com/Dovah-bah/job-application-tracker-python.git)

### 🔑 Environment Variables Setup

This project uses `python-dotenv` to manage database credentials securely. Before running the application, you need to set up your local environment file:

1. Create a file named `.env` in the root directory of the project.
2. Add your MySQL database configuration variables as follows:

```env
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_password_here
DB_NAME=job_tracker_db