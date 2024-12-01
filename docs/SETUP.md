# Microservices Setup Guide

## Prerequisites

1. Install Docker:
   ```bash
   # Windows (using winget)
   winget install Docker.DockerDesktop

   # macOS
   brew install --cask docker
   ```

2. Install Docker Compose:
   ```bash
   # Windows & macOS: Included with Docker Desktop
   docker-compose --version
   ```

3. Install MongoDB Compass (optional):
   ```bash
   # Windows (using chocolatey)
   choco install mongodb-compass

   # macOS
   brew install --cask mongodb-compass
   ```

## Quick Start

1. Clone repository:
   ```bash
   git clone https://github.com/nived2/microservices.git
   cd microservices
   ```

2. Start services:
   ```bash
   docker-compose up -d
   ```

3. Verify services:
   ```bash
   docker-compose ps
   ```

## Service Configuration

1. API Gateway (Port 3000):
   ```yaml
   api-gateway:
     build: ./api-gateway
     environment:
       - NODE_ENV=production
       - REDIS_URL=redis://redis:6379
       - MONGO_URL=mongodb://mongodb:27017/microservices
   ```

2. Service A (Port 3001):
   ```yaml
   service-a:
     build: ./service-a
     environment:
       - NODE_ENV=production
       - REDIS_URL=redis://redis:6379
   ```

3. Service B (Port 3002):
   ```yaml
   service-b:
     build: ./service-b
     environment:
       - NODE_ENV=production
       - MONGO_URL=mongodb://mongodb:27017/microservices
   ```

## Database Setup

1. MongoDB:
   ```bash
   # Access MongoDB shell
   docker exec -it microservices_mongodb_1 mongosh

   # Create database
   use microservices

   # Create user
   db.createUser({
     user: "admin",
     pwd: "secret",
     roles: ["readWrite"]
   })
   ```

2. Redis:
   ```bash
   # Access Redis CLI
   docker exec -it microservices_redis_1 redis-cli

   # Test connection
   ping
   ```

## Nginx Configuration

1. Load balancer setup:
   ```nginx
   upstream api {
       server api-gateway:3000;
   }

   server {
       listen 80;
       location / {
           proxy_pass http://api;
       }
   }
   ```

2. SSL configuration (optional):
   ```nginx
   server {
       listen 443 ssl;
       ssl_certificate /etc/nginx/certs/server.crt;
       ssl_certificate_key /etc/nginx/certs/server.key;
   }
   ```

## Scaling Services

1. Scale specific service:
   ```bash
   docker-compose up -d --scale service-a=3
   ```

2. View scaled instances:
   ```bash
   docker-compose ps service-a
   ```

## Monitoring

1. Container stats:
   ```bash
   docker stats
   ```

2. Logs:
   ```bash
   # All services
   docker-compose logs -f

   # Specific service
   docker-compose logs -f api-gateway
   ```

3. Health checks:
   ```bash
   # API Gateway
   curl http://localhost:3000/health

   # Service A
   curl http://localhost:3001/health
   ```

## Backup and Restore

1. MongoDB backup:
   ```bash
   # Backup
   docker exec microservices_mongodb_1 mongodump --out /backup/

   # Restore
   docker exec microservices_mongodb_1 mongorestore /backup/
   ```

2. Redis backup:
   ```bash
   # Backup
   docker exec microservices_redis_1 redis-cli save

   # Restore
   docker cp dump.rdb microservices_redis_1:/data/dump.rdb
   ```

## Troubleshooting

1. Container issues:
   ```bash
   # Check container logs
   docker-compose logs [service-name]

   # Restart service
   docker-compose restart [service-name]
   ```

2. Network issues:
   ```bash
   # List networks
   docker network ls

   # Inspect network
   docker network inspect microservices_default
   ```

3. Common problems:
   - Connection refused: Check if service is running
   - MongoDB connection: Verify credentials and URL
   - Redis connection: Check if Redis is running

## Security

1. Network security:
   - Internal network for services
   - Exposed ports only when necessary
   - SSL/TLS for external communication

2. Authentication:
   - API Gateway authentication
   - Service-to-service authentication
   - Database access control

## Development

1. Local development:
   ```bash
   # Start dependencies
   docker-compose up -d mongodb redis

   # Run service locally
   cd service-a
   npm install
   npm run dev
   ```

2. Testing:
   ```bash
   # Unit tests
   npm test

   # Integration tests
   npm run test:integration
   ```

## Contributing

1. Fork repository
2. Create feature branch
3. Make changes
4. Submit pull request

## Support

For issues and support:
- Create GitHub issue
- Email: nivedmahendran547@gmail.com
