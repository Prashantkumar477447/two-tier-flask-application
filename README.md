# Flask App with MySQL Docker Setup

This is a simple **Flask** application that interacts with a **MySQL** database. The app allows users to submit messages, which are then stored in the database and displayed on the frontend.

## Prerequisites

Before you begin, make sure you have the following installed:

- **Docker**
- **Git** (optional, for cloning the repository)

## Setup

### 1. Clone the Repository

Clone this repository to your local machine (if you haven't already)


```bash
git clone https://github.com/your-username/your-repo-name.git
```

Navigate to the project directory:

```bash
cd your-repo-name
```

### 2. Configure Environment Variables

Create a `.env` file in the project directory to store your MySQL environment variables:

```bash
touch .env
```

Open the `.env` file and add your MySQL configuration:

```env
MYSQL_HOST=mysql
MYSQL_USER=your_username
MYSQL_PASSWORD=your_password
MYSQL_DB=your_database
```

### 3. Docker Compose Setup

Start the containers using Docker Compose:

```bash
docker-compose up --build
```

### 4. Access the Flask App

Once the containers are up and running, access the application in your web browser:

- **Frontend**: [http://localhost](http://localhost)
- **Backend**: [http://localhost:5000](http://localhost:5000)

### 5. Create the Messages Table

Use a MySQL client or tool (e.g., phpMyAdmin) to execute the following SQL commands and create the `messages` table in your MySQL database:

```sql
CREATE TABLE messages (
    id INT AUTO_INCREMENT PRIMARY KEY,
    message TEXT
);
```

### 6. Interact with the App

1. Visit [http://localhost](http://localhost) to see the frontend where you can submit new messages using the form.
2. Visit [http://localhost:5000/insert_sql](http://localhost:5000/insert_sql) to insert a message directly into the messages table via an SQL query.

![Screenshot 2024-11-27 003400](https://github.com/user-attachments/assets/53f3c248-fa3b-446a-ab94-edb5708f09a1)

![Screenshot 2024-11-27 003540](https://github.com/user-attachments/assets/f2a9dbf7-8662-46fa-b3c1-70f4e879228d)

![Screenshot 2024-11-27 004115](https://github.com/user-attachments/assets/85720b22-5b03-403f-bca8-0052a82d84d5)


## Cleaning Up

To stop and remove the Docker containers, press `Ctrl+C` in the terminal where the containers are running, or use the following command:

```bash
docker-compose down
```

## Run the Application Without Docker Compose

### 1. Build the Docker Image

First, create a Docker image from the `Dockerfile`:

```bash
docker build -t flaskapp .
```

### 2. Create Docker Network

Create a custom network for the containers to communicate with each other:

```bash
docker network create twotier
```

### 3. Run the Containers

#### i) MySQL Container

Run the MySQL container on the custom network:

```bash
docker run -d \
    --name mysql \
    -v mysql-data:/var/lib/mysql \
    --network=twotier \
    -e MYSQL_DATABASE=mydb \
    -e MYSQL_ROOT_PASSWORD=admin \
    -p 3306:3306 \
    mysql:5.7
```

#### ii) Backend (Flask) Container

Run the Flask backend container, linking it to the same network:


```bash
docker run -d \
    --name flaskapp \
    --network=twotier \
    -e MYSQL_HOST=mysql \
    -e MYSQL_USER=root \
    -e MYSQL_PASSWORD=admin \
    -e MYSQL_DB=mydb \
    -p 5000:5000 \
    flaskapp:latest
```


## Notes

- Make sure to replace the placeholders (`your_username`, `your_password`, `your_database`) with your actual MySQL configuration in the `.env` file.
- This setup is a basic demonstration. For production environments, follow best practices for security and performance.
- Be cautious when executing SQL queries directly. Validate and sanitize user inputs to prevent vulnerabilities like SQL injection.
- If you encounter issues, check Docker logs and error messages for troubleshooting.

---

This version is properly structured, and each section is clearly marked with headings for ease of understanding and navigation.
