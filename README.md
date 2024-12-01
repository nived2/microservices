# Microservices Architecture

A microservices-based application using Docker Compose, implementing service discovery and load balancing.

## Features

- Containerized microservices
- Service discovery
- Load balancing with Nginx
- Redis caching
- MongoDB database
- API Gateway
- Centralized logging
- Health monitoring

## Architecture

```
                                   │
                                   ▼
                               [Nginx]
                                   │
                    ┌──────────────┴──────────────┐
                    ▼                             ▼
              [API Gateway]                  [Web Client]
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
   [Service A]  [Service B]  [Service C]
        │           │           │
        └─────┐ ┌───┴───┐ ┌────┘
              ▼ ▼       ▼ ▼
            [Redis]   [MongoDB]
```

## Prerequisites

- Docker
- Docker Compose
- Node.js (for local development)
- MongoDB Compass (optional)

## Project Structure

```
microservices/
├── docker-compose.yml          # Main compose file
├── nginx/                      # Nginx configuration
│   ├── Dockerfile
│   └── nginx.conf
├── api-gateway/               # API Gateway service
│   ├── Dockerfile
│   └── src/
├── service-a/                 # Microservice A
│   ├── Dockerfile
│   └── src/
├── service-b/                 # Microservice B
│   ├── Dockerfile
│   └── src/
├── service-c/                 # Microservice C
│   ├── Dockerfile
│   └── src/
└── config/                    # Configuration files
    ├── redis.conf
    └── mongo.conf
```

## Quick Start

For detailed setup instructions, please refer to our [Setup Guide](docs/SETUP.md).

1. Clone the repository:
```bash
git clone https://github.com/nived2/microservices.git
cd microservices
```

2. Start the services:
```bash
docker-compose up -d
```

3. Check service status:
```bash
docker-compose ps
```

## Services

### API Gateway
- Routes requests to appropriate services
- Handles authentication
- Rate limiting
- Request/Response transformation

### Service A
- Core business logic
- REST API endpoints
- MongoDB interaction
- Redis caching

### Service B
- Secondary business logic
- Async processing
- Event handling
- Data aggregation

### Service C
- Auxiliary services
- Reporting
- Analytics
- Background jobs

## Configuration

### Nginx
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

### Docker Compose
```yaml
version: '3.8'
services:
  nginx:
    build: ./nginx
    ports:
      - "80:80"
    
  api-gateway:
    build: ./api-gateway
    environment:
      - NODE_ENV=production
    
  mongodb:
    image: mongo:latest
    volumes:
      - mongodb_data:/data/db
    
  redis:
    image: redis:alpine
    volumes:
      - redis_data:/data

volumes:
  mongodb_data:
  redis_data:
```

## Scaling

To scale services:
```bash
docker-compose up -d --scale service-a=3
```

## Monitoring

- Health check endpoints
- Docker stats monitoring
- Redis monitoring
- MongoDB monitoring
- Nginx access logs

## Security

- API Gateway authentication
- Service-to-service authentication
- Network isolation
- Rate limiting
- CORS configuration

## Development

1. Install dependencies:
```bash
npm install
```

2. Run services locally:
```bash
docker-compose up -d mongodb redis
npm run dev
```

## Testing

```bash
# Run unit tests
npm test

# Run integration tests
npm run test:integration

# Run all tests
npm run test:all
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

MIT License - feel free to use this project for your own portfolio

## Contact

Nived Mahendran - nivedmahendran547@gmail.com
