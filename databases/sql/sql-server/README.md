# MSSQL Docker Container Setup

This guide provides instructions on setting up and accessing a Microsoft SQL Server (MSSQL) Docker container using environment variables defined in an .env file.

## Prerequisites

- Docker installed on your machine.
- Docker Compose (if you prefer to use Docker Compose).

## Environment Variables

Create a file named .env in your project directory with the following content:

```ini
# .env file

# SQL Server Administrator Password
MSSQL_SA_PASSWORD=your_password_here

# Docker Platform (e.g., linux/amd64)
DOCKER_PLATFORM=your_platform_here

# Port Mapping (e.g., 1433)
DOCKER_PORT=your_port_here

# Host directories for data, logs, and secrets
DATA_PATH=/path/to/your/data_directory
LOG_PATH=/path/to/your/log_directory
SECRETS_PATH=/path/to/your/secrets_directory
```

## Example

Here is an example with actual values filled in:

```
ini
# .env file

# SQL Server Administrator Password
MSSQL_SA_PASSWORD=frMm3ydgJz6AGF9GQi?3c8YRC?T63$

# Docker Platform (e.g., linux/amd64)
DOCKER_PLATFORM=linux/amd64

# Port Mapping (e.g., 1433)
DOCKER_PORT=1433

# Host directories for data, logs, and secrets
DATA_PATH=/Users/yourusername/Projects/mssql-data
LOG_PATH=/Users/yourusername/Projects/mssql-logs
SECRETS_PATH=/Users/yourusername/Projects/mssql-secrets
```

## Running the Container

You can run the MSSQL Docker container using the following command:

```bash
docker run --platform ${DOCKER_PLATFORM} \
-e "ACCEPT_EULA=Y" \
-e "MSSQL_SA_PASSWORD=${MSSQL_SA_PASSWORD}" \
-p ${DOCKER_PORT}:1433 \
-v <DATA_PATH>:/var/opt/mssql/data \
-v ${LOG_PATH}:/var/opt/mssql/log \
-v ${SECRETS_PATH}:/var/opt/mssql/secrets \
--name mssql-test-container \
--hostname mssql-test-host \
-d mcr.microsoft.com/mssql/server:2022-latest
```

## Accessing the MSSQL Server

- Connection String: Use the following connection string to connect to the MSSQL server from applications or tools like SQL Server Management Studio or Azure Data Studio:

    ```plaintext
    Server=localhost,${DOCKER_PORT};Database=TestDB;User Id=sa;Password=${MSSQL_SA_PASSWORD};Encrypt=True;TrustServerCertificate=True;
    ```

- Example with Actual Values: Based on the example .env values provided, the connection string would be:

    ```plaintext
    Server=localhost,1433;Database=TestDB;User Id=sa;Password=frMm3ydgJz6AGF9GQi?3c8YRC?T63$;Encrypt=True;TrustServerCertificate=True;
    ```

SQL Server Management Studio (SSMS) or Azure Data Studio:

- Server Name: localhost,1433
- Authentication Type: SQL Server Authentication
- Login: sa
- Password: frMm3ydgJz6AGF9GQi?3c8YRC?T63$

## Persisting Data

The container uses volumes to persist data and logs:

- Data Volume: /var/opt/mssql/data mapped to ${DATA_PATH}
- Log Volume: /var/opt/mssql/log mapped to ${LOG_PATH}
- Secrets Volume: /var/opt/mssql/secrets mapped to ${SECRETS_PATH}

Ensure the host directories (```${DATA_PATH}```, ```${LOG_PATH}```, and ```${SECRETS_PATH}```) exist before running the container.

## Stopping and Removing the Container

To stop and remove the container, use the following commands:

```bash
docker stop mssql-test-container
docker rm mssql-test-container
```

## Troubleshooting

- Container Logs: View logs to troubleshoot issues with:

    ```bash
    docker logs mssql-test-container
    ```

- Check Container Status: Verify that the container is running with:

    ```bash
    docker ps
    ```

## Additional Resources

- Docker Documentation
- SQL Server Docker Image Documentation