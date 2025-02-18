# App creation-

registration-app/

Created an application that will have registraion page with -
Wellcome to the Registration Page!!
Ask user for entering Name, Email and Country.

Once user enter the information and click on submit button, display the entered information saying- You have entered <info>.
Save the user data in postgresSQL database (will use postgresSQL database docker image for storing data).
Created app with javascript and/or HTML.


usersDashboard-app/

Create another app that with retrieve the user registration data from the registration database and users table
- display the number of user already registered.
- also give an option to search the user list by country name and display data in a tabler form.


Current project structure-

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


Project repo- https://github.com/masumshamsur/registration-app



Project instalation instruction-

To get the `node_modules`, `package.json`, and `package-lock.json` files in your project, follow these steps:

1. Initialize the `package.json` File**

Navigate to the directory of your project (`registration/` or `usersDashboard/`) in the terminal and run:


npm init -y


- This creates a basic `package.json` file with default values.
- The `y` flag accepts all the default settings (name, version, entry point, etc.).

If you already have a `package.json` file, you can skip this step.

2. Install Dependencies

In the same directory, run:

npm install express pg


This will:
- Install the required dependencies (`express` and `pg` in this case).
- Create a `node_modules/` directory with the installed libraries and their dependencies.
- Automatically create or update the `package-lock.json` file to lock the versions of the installed libraries.

If you're using a Mac M1/M2 and targeting a Linux environment, build the image for the correct platform:

docker buildx build --platform linux/amd64 -t masum012924/registration-app:v1 .


push the image to docker repo-
docker push masum012924/registration-app:v1


run the image in local machine-
docker run -d -p 3000:3000 registration-app:v1


Troubleshooting—-
Verify Database Existence and Table Structure**

Ensure that the `users` table exists in your `registration` database. You can connect to the database from within the PostgreSQL pod and verify:

1. **Get the PostgreSQL pod name**:
    
    kubectl get pods
    
    
2. **Connect to the PostgreSQL pod**:
    
    kubectl exec -it <postgres-pod-name> -- /bin/bash

    
3. **Access PostgreSQL**:

    psql -U postgres -d registration

    
4. **Check if the `users` table exists**:
    
    ```sql
    \dt
    ```
    
    If you don't see the `users` table, you can create it using:
    
    CREATE TABLE users (
        id SERIAL PRIMARY KEY,
        name VARCHAR(100),
        email VARCHAR(100),
        country VARCHAR(100)

    

Here's how you can view the records in the `users` table:

1. While still connected to your `registration` database (you're at the `registration=#` prompt), run the following SQL query to see all the records in the `users` table:

    SELECT * FROM users;

    
2. This will display all the rows in the `users` table. If you don't have any data yet, it will show an empty result set, like:
    
    id | name | email | country
    ----+------+-------+---------
    (0 rows)

    

### Inserting Data Manually (Optional)

If you want to manually insert some test data into the table to check everything works, you can run the following SQL:

INSERT INTO users (name, email, country) VALUES
('John Doe', 'john.doe@example.com', 'USA'),
('Jane Smith', 'jane.smith@example.com', 'Canada');


Now, when you visit the `/users` route in your app, it should return the inserted data.