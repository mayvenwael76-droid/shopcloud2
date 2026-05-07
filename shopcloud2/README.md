# ShopCloud 🛍️

A cloud-based Online Store deployed on AWS, built with FastAPI, PostgreSQL, and Docker.

---

## Tech Stack

| Layer     | Technology                    |
|-----------|-------------------------------|
| Backend   | Python 3.11 + FastAPI         |
| Database  | PostgreSQL 15                 |
| Frontend  | HTML / CSS / Vanilla JS       |
| Container | Docker + Docker Compose       |
| Cloud     | AWS EC2 (Amazon Linux 2023)   |

---

## Run Locally with Docker

### Prerequisites
- Docker Desktop installed
- Docker Compose installed

### Steps

```bash
# 1. Clone the repo
git clone https://github.com/your-team/shopcloud.git
cd shopcloud

# 2. Build and start all services
docker-compose up --build

# 3. Access the app
# Frontend:  http://localhost
# API docs:  http://localhost:8000/docs
# API base:  http://localhost:8000/api
```

### Stop the app
```bash
docker-compose down
```

### Stop and remove database data
```bash
docker-compose down -v
```

---

## API Endpoints

### Products
| Method | Endpoint                  | Description              |
|--------|---------------------------|--------------------------|
| GET    | /api/products/            | Get all products         |
| POST   | /api/products/            | Create a new product     |
| GET    | /api/products/{id}        | Get product by ID        |
| PUT    | /api/products/{id}        | Update product           |
| DELETE | /api/products/{id}        | Delete product           |

### Users
| Method | Endpoint                  | Description              |
|--------|---------------------------|--------------------------|
| POST   | /api/users/register       | Register new user        |
| GET    | /api/users/               | Get all users            |
| GET    | /api/users/{id}           | Get user by ID           |
| DELETE | /api/users/{id}           | Delete user              |

### Orders
| Method | Endpoint                        | Description              |
|--------|---------------------------------|--------------------------|
| POST   | /api/orders/                    | Create an order          |
| GET    | /api/orders/                    | Get all orders           |
| GET    | /api/orders/{id}                | Get order by ID          |
| PUT    | /api/orders/{id}/status         | Update order status      |
| DELETE | /api/orders/{id}                | Delete order             |

---

## Access Deployed App on AWS

```
Frontend:  http://<EC2-PUBLIC-IP>
API Docs:  http://<EC2-PUBLIC-IP>:8000/docs
```

---

## Project Structure

```
shopcloud/
├── backend/
│   ├── app/
│   │   ├── main.py          # FastAPI app entry point
│   │   ├── database.py      # DB connection
│   │   ├── models/          # SQLAlchemy models
│   │   ├── schemas/         # Pydantic schemas
│   │   └── routers/         # API route handlers
│   ├── requirements.txt
│   └── Dockerfile
├── frontend/
│   └── index.html           # Web UI
├── docs/
│   └── report.md            # Project report
└── docker-compose.yml
```
