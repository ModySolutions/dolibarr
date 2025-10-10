# Dolibarr ERP/CRM with Docker Compose

This repository provides a simple setup for deploying a Dolibarr ERP/CRM instance using Docker Compose. The configuration includes two main services: a `dolibarr/dolibarr:21` application container and a `mariadb:11.4.7` database container.

## Prerequisites

Ensure you have the following installed on your system:
* Docker
* Docker Compose

## Getting Started

1.  **Create `.env` file:**
    Copy the provided `sample.env` file to `.env`. This file will store your configuration secrets, and it is listed in the `.gitignore` to prevent tracking in Git.

    ```bash
    cp sample.env .env
    ```

2.  **Review Configuration:**
    Edit the newly created `.env` file to customize the database credentials and Dolibarr application secrets.

    Example values from `sample.env`:
    * `DOLI_URL_ROOT=https://dolibarr.localhost`
    * `DOLI_ADMIN_LOGIN=admin`
    * `DOLI_ADMIN_PASSWORD=password`

3.  **Start Services:**
    Run the following command in the directory containing `compose.yml`:

    ```bash
    docker compose up -d
    ```

    The `dolibarr` service is configured to wait for the `mariadb` service to be healthy before attempting to start.

## Configuration

Configuration is managed primarily through environment variables defined in the `.env` file, which are then passed to the Docker containers via `compose.yml`. The `compose.yml` file also provides default values for these variables if they are not set in the `.env` file.

### Database Settings (MariaDB)

| Variable | Sample Value (from `sample.env`) | Default (if not set in `.env`) | Purpose |
| :--- | :--- | :--- | :--- |
| `MYSQL_ROOT_PASSWORD` | `password` | `root` | Root password for the MariaDB service. |
| `DOLI_DB_NAME` | `dolibarr` | `dolidb` | Database name for Dolibarr. |
| `DOLI_DB_USER` | `dolibarr` | `dolidbuser` | Database user for Dolibarr. |
| `DOLI_DB_PASSWORD` | `password` | `dolidbpass` | Database password for Dolibarr user. |

### Dolibarr Application Settings

| Variable | Sample Value (from `sample.env`) | Default (if not set in `.env`) | Purpose |
| :--- | :--- | :--- | :--- |
| `DOLI_URL_ROOT` | `https://dolibarr.localhost` | `http://dolibarr.localhost` | The root URL to access Dolibarr. |
| `DOLI_ADMIN_LOGIN` | `admin` | `admin` | Default administrator login. |
| `DOLI_ADMIN_PASSWORD` | `password` | `password` | Default administrator password. |
| `DOLI_CRON_KEY` | `some-secure-key` | `cron-secure-key` | Secure key for cron jobs. |
| `DOLI_COMPANY_NAME` | `"My Company"` | `company-name` | Company name for the Dolibarr setup. |

## Accessing Dolibarr

The application will be accessible at the URL configured in `DOLI_URL_ROOT`. You may need to modify your host machine's `/etc/hosts` file to resolve the custom hostname (`dolibarr.localhost`) if you use the default or sample configuration.

**Default Administrator Credentials (from `sample.env`):**
* **Login**: `admin`
* **Password**: `password`

## Data Persistence

The `compose.yml` file uses named Docker volumes to ensure that important data persists even if the containers are removed or re-created:

* `dolibarr_db_data`: Maps to `/var/lib/mysql` in the `mariadb` container for database files.
* `dolibarr_documents`: Maps to `/var/www/documents` in the `dolibarr` container for uploaded files and documents.
* `dolibarr_custom`: Maps to `/var/www/html/custom` in the `dolibarr` container for custom modules and extensions.

## Ignored Files

The `.gitignore` file specifies files and directories that should be excluded from Git tracking: