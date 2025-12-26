# Nginx Forwarder

A Docker-based Nginx reverse proxy/forwarder solution for routing traffic to multiple backend services.

## 📋 Overview

This project provides a containerized Nginx reverse proxy configuration that can forward requests to various backend services. It's designed to work with Docker Compose for easy deployment and management.

## 🏗️ Project Structure

```
nginx-forwarder/
├── conf.d/           # Nginx site-specific configuration files
├── logs/             # Nginx log files directory
├── ssl/              # SSL certificates and keys
├── .idea/            # IDE configuration (optional)
├── .nopanel          # Panel configuration flag
├── docker-compose.yml # Docker Compose configuration
└── nginx.conf        # Main Nginx configuration file
```

## 🚀 Features

- **Reverse Proxy**: Forward requests to multiple backend services
- **SSL Support**: Built-in SSL certificate directory for HTTPS traffic
- **Centralized Logging**: Dedicated logs directory for monitoring and debugging
- **Docker-based**: Easy deployment and portability using Docker containers
- **Modular Configuration**: Site-specific configurations in `conf.d/` directory

## 📦 Prerequisites

- Docker (version 20.10 or higher)
- Docker Compose (version 1.29 or higher)
- Basic understanding of Nginx configuration

## 🔧 Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/roshandelpoor/nginx-forwarder.git
   cd nginx-forwarder
   ```

2. **Configure your backend services**
   
   Edit the configuration files in the `conf.d/` directory to define your proxy rules:
   ```bash
   nano conf.d/your-service.conf
   ```

3. **Set up SSL certificates** (optional)
   
   Place your SSL certificates in the `ssl/` directory:
   ```bash
   cp /path/to/your/certificate.crt ssl/
   cp /path/to/your/private.key ssl/
   ```

4. **Review the main configuration**
   
   Check and modify `nginx.conf` if needed:
   ```bash
   nano nginx.conf
   ```

## 🎯 Usage

### Starting the Service

```bash
docker-compose up -d
```

### Stopping the Service

```bash
docker-compose down
```

### Viewing Logs

```bash
# Real-time logs
docker-compose logs -f

# Nginx access logs
tail -f logs/access.log

# Nginx error logs
tail -f logs/error.log
```

### Reloading Configuration

After making changes to Nginx configuration files:

```bash
docker-compose exec nginx nginx -s reload
```

Or restart the container:

```bash
docker-compose restart
```

## ⚙️ Configuration

### Adding a New Proxy Configuration

1. Create a new configuration file in `conf.d/`:

```nginx
# conf.d/example.conf
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://backend-service:port;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

2. Reload Nginx configuration:
```bash
docker-compose exec nginx nginx -s reload
```

### SSL Configuration Example

```nginx
# conf.d/secure-example.conf
server {
    listen 443 ssl;
    server_name secure.example.com;

    ssl_certificate /etc/nginx/ssl/certificate.crt;
    ssl_certificate_key /etc/nginx/ssl/private.key;

    location / {
        proxy_pass http://backend-service:port;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

## 🐳 Docker Compose Configuration

The `docker-compose.yml` file defines the Nginx service with necessary volume mounts for configuration, logs, and SSL certificates.

Typical structure:
```yaml
version: '3'
services:
  nginx:
    image: nginx:latest
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
      - ./conf.d:/etc/nginx/conf.d:ro
      - ./ssl:/etc/nginx/ssl:ro
      - ./logs:/var/log/nginx
    restart: unless-stopped
```

## 🔍 Troubleshooting

### Configuration Test

Test your Nginx configuration for syntax errors:

```bash
docker-compose exec nginx nginx -t
```

### Check Container Status

```bash
docker-compose ps
```

### View Container Logs

```bash
docker-compose logs nginx
```

### Common Issues

1. **Port Already in Use**: Ensure ports 80 and 443 are not being used by other services
2. **Permission Denied**: Check file permissions for configuration and SSL files
3. **Configuration Errors**: Use `nginx -t` to validate configuration syntax

## 📝 Logs

Nginx logs are stored in the `logs/` directory:

- `access.log`: HTTP access logs
- `error.log`: Error and diagnostic logs

## 🔒 Security Considerations

- Keep SSL certificates secure and never commit them to version control
- Use strong SSL/TLS configurations
- Regularly update the Nginx Docker image
- Implement rate limiting and access controls as needed
- Review and restrict access to log files

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 👤 Author

**Roshandelpoor Mohammad**

- GitHub: [@roshandelpoor](https://github.com/roshandelpoor)

## 🙏 Acknowledgments

- Nginx for the powerful web server and reverse proxy
- Docker for containerization support

## 📚 Additional Resources

- [Nginx Documentation](https://nginx.org/en/docs/)
- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Nginx Reverse Proxy Guide](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/)

---

**Note**: This README provides general guidance based on the project structure. Please refer to the actual configuration files in the repository for specific implementation details.