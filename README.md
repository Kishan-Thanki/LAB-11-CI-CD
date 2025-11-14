# Microservices CI/CD Pipeline with Monitoring

This project demonstrates a complete CI/CD pipeline for a microservices application with automated testing, Docker containerization, GitHub Actions deployment, and Prometheus/Grafana monitoring.

## Project Structure

```
.
├── user-service/          # User management microservice
│   ├── index.js          # Main service file
│   ├── metrics.js        # Prometheus metrics
│   ├── Dockerfile        # Docker image definition
│   ├── package.json      # Dependencies
│   └── __tests__/        # Test files
├── order-service/        # Order management microservice
│   ├── index.js          # Main service file
│   ├── metrics.js        # Prometheus metrics
│   ├── Dockerfile        # Docker image definition
│   ├── package.json      # Dependencies
│   └── __tests__/        # Test files
├── .github/
│   └── workflows/
│       └── ci-cd.yml     # GitHub Actions CI/CD pipeline
├── prometheus/
│   └── prometheus.yml    # Prometheus configuration
├── grafana/
│   ├── provisioning/     # Grafana datasource and dashboard provisioning
│   └── dashboards/       # Grafana dashboard definitions
├── docker-compose.yml    # Local development setup
└── README.md            # This file
```

## Local Setup

### 1. Install Dependencies

```bash
# Install dependencies for all services
npm run install:all

# Or install individually
cd user-service && npm install
cd ../order-service && npm install
```

### 2. Run Tests

```bash
# Test user-service
cd user-service
npm test

# Test order-service
cd order-service
npm test
```

### 3. Run Services Locally (Development)

```bash
# Terminal 1 - User Service
cd user-service
npm run dev

# Terminal 2 - Order Service
cd order-service
npm run dev
```

Services will be available at:
- User Service: http://localhost:8003
- Order Service: http://localhost:8002

### 4. Run with Docker Compose (Full Stack with Monitoring)

```bash
# Build and start all services
docker-compose up --build

# Or run in detached mode
docker-compose up -d --build
```

This will start:
- **User Service**: http://localhost:8003
- **Order Service**: http://localhost:8002
- **Prometheus**: http://localhost:9090
- **Grafana**: http://localhost:3000 (admin/admin)

### 5. Access Monitoring Dashboards

1. **Prometheus**: 
   - URL: http://localhost:9090
   - Check targets: http://localhost:9090/targets
   - Query metrics: http://localhost:9090/graph

2. **Grafana**:
   - URL: http://localhost:3000
   - Username: `admin`
   - Password: `admin`
   - Dashboard: Navigate to Dashboards → Microservices Monitoring Dashboard

## API Endpoints

### User Service (Port 8003)

- `GET /health` - Health check
- `GET /metrics` - Prometheus metrics
- `GET /users` - Get all users
- `GET /users/:id` - Get user by ID
- `POST /users` - Create new user
- `PATCH /users/:id/balance` - Update user balance
- `DELETE /users/:id` - Delete user

### Order Service (Port 8002)

- `GET /health` - Health check
- `GET /metrics` - Prometheus metrics
- `GET /products` - Get all products
- `GET /products/:id` - Get product by ID
- `POST /orders` - Create new order
- `GET /orders` - Get all orders
- `GET /orders/:id` - Get order by ID
- `GET /orders/user/:userId` - Get orders by user ID

## CI/CD Pipeline

The GitHub Actions workflow (`.github/workflows/ci-cd.yml`) includes:

### 1. Test Stage
- Runs Jest tests for both services
- Validates code quality
- Runs on every push and pull request

### 2. Build Stage
- Builds Docker images for both services
- Pushes images to GitHub Container Registry
- Only runs on push events

### 3. Deploy Stage
- Deploys to production (placeholder for actual deployment)
- Only runs on main/master branch pushes

### Setting Up GitHub Actions

1. **Push your code to GitHub**:
   ```bash
   git init
   git add .
   git commit -m "Initial commit with CI/CD pipeline"
   git remote add origin <your-github-repo-url>
   git push -u origin main
   ```

2. **View Pipeline Execution**:
   - Go to your GitHub repository
   - Click on "Actions" tab
   - You'll see the pipeline running automatically

3. **Access Built Images**:
   - Images are pushed to GitHub Container Registry
   - Format: `ghcr.io/<username>/<repo>/<service-name>:<tag>`

## Monitoring Metrics

### Prometheus Metrics Exposed

**User Service:**
- `http_requests_total` - Total HTTP requests
- `http_request_duration_seconds` - Request duration histogram
- `active_users_count` - Current number of active users

**Order Service:**
- `http_requests_total` - Total HTTP requests
- `http_request_duration_seconds` - Request duration histogram
- `orders_total` - Total orders placed
- `order_value_dollars` - Order value distribution

### Grafana Dashboard

The dashboard includes:
- HTTP Request Rate
- HTTP Request Duration (50th and 95th percentiles)
- Active Users Count
- Total Orders
- Order Value Distribution
- HTTP Status Code Distribution

## Testing the Pipeline

### 1. Test Locally

```bash
# Run all tests
cd user-service && npm test
cd ../order-service && npm test
```

### 2. Test Docker Build

```bash
# Build user-service
cd user-service
docker build -t user-service:test .

# Build order-service
cd ../order-service
docker build -t order-service:test .
```

### 3. Test Full Stack

```bash
# Start everything
docker-compose up --build

# Test endpoints
curl http://localhost:8003/health
curl http://localhost:8002/health
curl http://localhost:8003/metrics
curl http://localhost:8002/metrics
```

### 4. Generate Load for Monitoring

```bash
# Create some users
curl -X POST http://localhost:8003/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Test User","email":"test@example.com","balance":1000}'

# Create some orders
curl -X POST http://localhost:8002/orders \
  -H "Content-Type: application/json" \
  -d '{"productId":1,"quantity":1,"userId":1}'

# Check metrics
curl http://localhost:8003/metrics
curl http://localhost:8002/metrics
```


Kishan Thanki

