# Product App AWS

A Spring Boot REST API application for managing products, containerized and deployed on AWS.

## Features

- RESTful API for Product CRUD operations
- Spring Boot 3.4.0
- Java 17
- Maven build
- Docker containerization
- AWS ECS deployment
- CI/CD with Jenkins and AWS CodePipeline

## API Endpoints

- `GET /product` - Get all products
- `GET /product/{id}` - Get product by ID
- `POST /product` - Create new product
- `PUT /product/{id}` - Update product
- `DELETE /product/{id}` - Delete product

## Local Development

### Prerequisites

- Java 17
- Maven 3.6+

### Build and Run

```bash
mvn clean package
java -jar target/ProductAppAWS-0.0.1-SNAPSHOT.jar
```

Application will start on port 8080.

## Docker

### Build Docker Image

```bash
docker build -t product-app .
```

### Run Docker Container

```bash
docker run -p 8080:8080 product-app
```

## AWS Deployment

### ECR & ECS Setup

1. Create ECR repository
2. Build and push Docker image to ECR
3. Create ECS cluster, task definition, and service
4. Access via load balancer URL

### CI/CD Pipeline

- Jenkins pipeline for automated builds
- AWS CodePipeline for continuous deployment
- CloudFormation for infrastructure as code

## Testing

Access the API at: `http://localhost:8080/product`

Sample product JSON:
```json
{
  "name": "Laptop",
  "quantity": 10,
  "price": 50000
}
```
