# ShopCloud – Project Report
**Cloud Development Final Project**

---

## 1. Project Idea and Objectives

**Project Name:** ShopCloud – Cloud-Based Online Store

**Objective:**
ShopCloud is a fully functional e-commerce platform deployed on Amazon Web Services (AWS). The system allows users to browse products, place orders, and manage inventory through a web interface. The backend exposes a RESTful API, and all data is persisted in a relational database.

**Real-World Problem Solved:**
Small businesses need a simple, scalable online store that can be deployed quickly with minimal infrastructure setup. ShopCloud provides exactly this — a cloud-hosted store with full CRUD capabilities for products, users, and orders.

---

## 2. Functional Features

| Feature            | Details                                              |
|--------------------|------------------------------------------------------|
| Product Management | Create, Read, Update, Delete products with stock tracking |
| User Registration  | Register users with hashed passwords                 |
| Order System       | Place orders with multiple items, auto-calculates total |
| Order Status       | Update order status (pending → shipped → delivered)  |
| Web Interface      | Responsive frontend to browse, add to cart, checkout |
| API Documentation  | Auto-generated Swagger UI at `/docs`                 |

---

## 3. AWS Services Used

### Primary: Amazon EC2

- **Service:** Amazon EC2 (Elastic Compute Cloud)
- **Instance Type:** t2.micro (Free Tier eligible)
- **OS:** Amazon Linux 2023
- **Region:** us-east-1

**Why EC2?**
EC2 gives full control over the server environment. Docker and Docker Compose can be installed directly, making it straightforward to deploy all three containers (backend, database, frontend) on a single instance.

### Security Group Configuration

| Port | Protocol | Purpose          |
|------|----------|------------------|
| 22   | TCP      | SSH access       |
| 80   | TCP      | Frontend (Nginx) |
| 8000 | TCP      | FastAPI backend  |
| 5432 | TCP      | PostgreSQL (optional, can keep private) |

---

## 4. Deployment Steps on AWS EC2

### Step 1: Launch EC2 Instance
1. Go to AWS Console → EC2 → Launch Instance
2. Choose **Amazon Linux 2023 AMI**
3. Select **t2.micro** (free tier)
4. Create or use existing Key Pair (.pem file)
5. Configure Security Group to allow ports: 22, 80, 8000
6. Launch the instance

### Step 2: Connect to EC2
```bash
chmod 400 your-key.pem
ssh -i your-key.pem ec2-user@<EC2-PUBLIC-IP>
```

### Step 3: Install Docker on EC2
```bash
sudo yum update -y
sudo yum install -y docker
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ec2-user

# Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Re-login to apply docker group
exit
ssh -i your-key.pem ec2-user@<EC2-PUBLIC-IP>
```

### Step 4: Upload Project Files
```bash
# From your local machine
scp -i your-key.pem -r ./shopcloud ec2-user@<EC2-PUBLIC-IP>:~/
```

### Step 5: Run the Application
```bash
cd ~/shopcloud
docker-compose up --build -d
```

### Step 6: Verify
```bash
# Check running containers
docker ps

# Check logs
docker-compose logs backend
```

### Access the App
```
Frontend:  http://<EC2-PUBLIC-IP>
API:       http://<EC2-PUBLIC-IP>:8000
API Docs:  http://<EC2-PUBLIC-IP>:8000/docs
```

---

## 5. Architecture Diagram

```
                        Internet
                           |
                    [EC2 Instance]
                    ┌──────────────┐
                    │  Port 80     │──► Nginx (Frontend HTML)
                    │  Port 8000   │──► FastAPI (Backend API)
                    │  Port 5432   │──► PostgreSQL (Database)
                    └──────────────┘
                    
       [Docker Compose manages all 3 containers on EC2]
```

---

## 6. Non-Functional Requirements Addressed

| Requirement   | How it's handled                                          |
|---------------|-----------------------------------------------------------|
| Scalability   | Docker containers can be scaled; EC2 instance type can be upgraded |
| Reliability   | `restart: on-failure` in docker-compose; health checks on DB |
| Error Handling| HTTP exceptions with meaningful error messages in all endpoints |
| Logging       | Python `logging` module logs all key events to stdout     |
| Clean Code    | Separated into models, schemas, routers following MVC pattern |

---

## 7. Team Members

| Name | Role |
|------|------|
| Member 1 | Backend API + Database |
| Member 2 | Frontend + Docker setup |
| Member 3 | AWS Deployment + Documentation |

---

## 8. Conclusion

ShopCloud successfully demonstrates a cloud-deployed, containerized web application using real-world technologies. The system uses FastAPI for a high-performance REST API, PostgreSQL for persistent relational data, Docker for containerization, and AWS EC2 for cloud deployment — satisfying all mandatory project requirements.
