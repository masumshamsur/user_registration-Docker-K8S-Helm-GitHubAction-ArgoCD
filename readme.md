# Registration App

## Overview
This project consists of two applications:
1. **registration-app** - A user registration application.
2. **usersDashboard-app** - A dashboard to retrieve and display registered user data.

---

## App Details

### **registration-app**
This application provides a registration page where users can enter their details:
- Name
- Email
- Country

Upon submission:
- The entered information is displayed as confirmation.
- Data is stored in a PostgreSQL database (hosted using a PostgreSQL Docker image).
- Built with JavaScript and HTML.

### **usersDashboard-app**
A dashboard application to:
- Retrieve registered user data from the database.
- Display the total number of registered users.
- Provide a search functionality to filter users by country.
- Display the user data in a tabular format.

---

## Project Structure

```
USER_REGISTRATION/
├── k8s/
│   ├── config-map.yaml
│   ├── postgres-deployment.yaml
│   ├── secret.yaml
│   ├── user-dashboard-deployment.yaml
│   ├── user-registration-deployment.yaml
│
├── registration-app/
│   ├── node_modules/
│   ├── src/
│   ├── .dockerignore
│   ├── .gitignore
│   ├── Dockerfile
│   ├── package-lock.json
│   ├── package.json
│
├── user-registration-chart/
│   ├── charts/
│   ├── templates/
│   │   ├── configMap_secret.yaml
│   │   ├── dashboard-deployment.yaml
│   │   ├── postgres-deployment.yaml
│   │   ├── registration-deployment.yaml
│   ├── .helmignore
│   ├── Chart.yaml
│   ├── values.yaml
│
├── users-dashboard-app/
│   ├── node_modules/
│   ├── src/
│   ├── .dockerignore
│   ├── .gitignore
│   ├── Dockerfile
│   ├── package-lock.json
│   ├── package.json
│
├── readme.md
```

---

## Project Repository

[GitHub Repository](https://github.com/infosecsingh/Flask-App-GitHub-Actions-ArgoCD)

---

## Installation Instructions

### **Step 1: Initialize the Project**
Navigate to the respective project directory (`registration-app/` or `users-dashboard-app/`) and run:

```sh
npm init -y
```

This will create a `package.json` file if it does not already exist.

### **Step 2: Install Dependencies**
Run the following command to install the required dependencies:

```sh
npm install express pg
```

This will:
- Install required dependencies (`express` and `pg`).
- Create a `node_modules/` directory.
- Generate a `package-lock.json` file.

---

## Docker Setup

### **Building the Docker Image**
If you're using an M1/M2 Mac and targeting a Linux environment, build the image using:

```sh
docker buildx build --platform linux/amd64 -t masum012924/registration-app:v1 .
```

### **Pushing the Image to Docker Hub**

```sh
docker push masum012924/registration-app:v1
```

### **Running the Image Locally**

```sh
docker run -d -p 3000:3000 masum012924/registration-app:v1
```

---

## Troubleshooting

### **Verify Database and Table Structure**
Ensure the `users` table exists in the PostgreSQL database:

1. **Get PostgreSQL Pod Name**:
    ```sh
    kubectl get pods
    ```
2. **Connect to the PostgreSQL Pod**:
    ```sh
    kubectl exec -it <postgres-pod-name> -- /bin/bash
    ```
3. **Access PostgreSQL**:
    ```sh
    psql -U postgres -d registration
    ```
4. **Check for Existing Tables**:
    ```sql
    \dt
    ```
   If `users` table does not exist, create it using:
    ```sql
    CREATE TABLE users (
        id SERIAL PRIMARY KEY,
        name VARCHAR(100),
        email VARCHAR(100),
        country VARCHAR(100)
    );
    ```

### **View User Records**
To view registered users, run:

```sql
SELECT * FROM users;
```

If no data exists, you will see:
```
id | name | email | country
----+------+-------+---------
(0 rows)
```

### **Insert Sample Data (Optional)**
If needed, manually insert test data:

```sql
INSERT INTO users (name, email, country) VALUES
('John Doe', 'john.doe@example.com', 'USA'),
('Jane Smith', 'jane.smith@example.com', 'Canada');
```

After inserting, visiting `/users` in the dashboard should return the test data.

---

## License
This project is open-source and available under the [MIT License](LICENSE).

