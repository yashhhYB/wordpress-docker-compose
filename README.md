# WordPress Docker Compose

A simple and efficient way to set up a WordPress environment using Docker Compose. This repository provides a fully customizable and ready-to-use WordPress stack.

---

## Features
- WordPress with MariaDB as the database
- Support for multi-environment setups (development, staging, production)
- Automatic SSL with Let's Encrypt (optional)
- Preconfigured themes and plugins (optional)
- WP-CLI support for managing WordPress via the command line
- Health checks for WordPress and database services

---

## Prerequisites
Make sure you have the following installed:
- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)

---

## Installation

### 1. Clone the Repository
```bash
git clone https://github.com/YOUR_USERNAME/wordpress-docker-compose.git
cd wordpress-docker-compose
```

### 2. Set Up Environment Variables
Copy the `.env.example` file and update the variables as needed:
```bash
cp .env.example .env
```
Key variables include:
- `WORDPRESS_DB_USER`: Database username
- `WORDPRESS_DB_PASSWORD`: Database password
- `WORDPRESS_DB_NAME`: Database name
- `WORDPRESS_TABLE_PREFIX`: Table prefix for WordPress

### 3. Start the Services
Run the following command to start WordPress and MariaDB:
```bash
docker-compose up -d
```

### 4. Access WordPress
Visit `http://localhost:8000` in your browser.

---

## Folder Structure
```
wordpress-docker-compose/
├── docker-compose.yml        # Main Docker Compose configuration
├── wp-content/               # WordPress content directory for themes, plugins, and uploads
├── .env.example              # Example environment variables
├── ssl/                      # Directory for SSL certificates (optional)
└── README.md                 # Documentation
```

---

## Customization

### Multi-Environment Setup
To use different configurations for development, staging, and production, create separate `.env` files for each environment (e.g., `.env.dev`, `.env.prod`). Then run:
```bash
docker-compose --env-file .env.dev up -d
```

### Adding Themes and Plugins
Modify the `docker-compose.yml` file to include commands for downloading themes and plugins. For example:
```yaml
services:
  wordpress:
    volumes:
      - ./themes:/var/www/html/wp-content/themes
      - ./plugins:/var/www/html/wp-content/plugins
```
Place your theme files in the `themes/` directory and plugin files in the `plugins/` directory.

### WP-CLI Integration
To use WP-CLI, run the following command:
```bash
docker-compose exec wordpress wp <command>
```
Example:
```bash
docker-compose exec wordpress wp plugin install jetpack
```

---

## Automatic SSL
To enable SSL with Let's Encrypt:
1. Add a new service to `docker-compose.yml` for SSL management.
2. Use the [certbot Docker image](https://hub.docker.com/r/certbot/certbot) to generate SSL certificates.
3. Update the Nginx configuration to use these certificates.

---

## Health Checks
Add health checks to ensure WordPress and MariaDB are running properly. For example:
```yaml
services:
  wordpress:
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000"]
      interval: 30s
      timeout: 10s
      retries: 3
```

---

## Contributing
We welcome contributions! To contribute:
1. Fork the repository.
2. Create a new branch for your feature.
3. Commit your changes.
4. Open a pull request.

---

## License
This project is licensed under the MIT License.

