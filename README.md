# Go Microservices Architecture with GraphQL and gRPC

![License](https://img.shields.io/badge/License-MIT-blue.svg)
![Go](https://img.shields.io/badge/Go-1.21%2B-00ADD8)
![GraphQL](https://img.shields.io/badge/GraphQL-API-E10098)
![gRPC](https://img.shields.io/badge/gRPC-Service-00B4AB)

## 📋 Overview

This project implements a modern microservices architecture using Go, with GraphQL as the API gateway and gRPC for inter-service communication.

## 🏗️ Architecture

The system follows a microservices architecture with the following components:

- **GraphQL API Gateway**: Provides a unified entry point for clients
- **Account Service**: Manages user accounts and authentication
- **Catalog Service**: Handles product information and search functionality
- **Order Service**: Processes orders and manages the purchase lifecycle

### Technology Stack

- **Go**: Primary programming language
- **GraphQL**: API gateway interface
- **gRPC**: Inter-service communication
- **PostgreSQL**: Database for Account and Order services
- **Elasticsearch**: Database for Catalog service
- **Docker**: Containerization and deployment

## 🚀 Getting Started

### Prerequisites

- Go 1.21 or higher
- Docker and Docker Compose
- PostgreSQL
- Elasticsearch

### Installation

1. Clone the repository

2. Start the services using Docker Compose:

   ```
   docker-compose up -d --build
   ```

3. Access the GraphQL playground at `http://localhost:8000/playground`

## 🔄 gRPC Setup

For generating gRPC files, follow these steps:

1. Install Protocol Buffers compiler:

   ```
   wget https://github.com/protocolbuffers/protobuf/releases/download/v23.0/protoc-23.0-linux-x86_64.zip
   unzip protoc-23.0-linux-x86_64.zip -d protoc
   sudo mv protoc/bin/protoc /usr/local/bin/
   ```

2. Install Go plugins for Protocol Buffers:

   ```
   go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
   go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
   ```

3. Update PATH:

   ```
   export PATH="$PATH:$(go env GOPATH)/bin"
   source ~/.bashrc
   ```

4. Generate gRPC code:
   ```
   mkdir -p pb
   # Add this to your proto files: option go_package = "./pb";
   protoc --go_out=./pb --go-grpc_out=./pb account.proto
   ```

## 📡 API Usage

### GraphQL Examples

#### Query Accounts

```graphql
query {
  accounts {
    id
    name
  }
}
```

#### Create an Account

```graphql
mutation {
  createAccount(account: { name: "New Account" }) {
    id
    name
  }
}
```

#### Query Products

```graphql
query {
  products {
    id
    name
    price
  }
}
```

#### Create a Product

```graphql
mutation {
  createProduct(
    product: { name: "New Product", description: "A new product", price: 19.99 }
  ) {
    id
    name
    price
  }
}
```

#### Create an Order

```graphql
mutation {
  createOrder(
    order: {
      accountId: "account_id"
      products: [{ id: "product_id", quantity: 2 }]
    }
  ) {
    id
    totalPrice
    products {
      name
      quantity
    }
  }
}
```

#### Query Account with Orders

```graphql
query {
  accounts(id: "account_id") {
    name
    orders {
      id
      createdAt
      totalPrice
      products {
        name
        quantity
        price
      }
    }
  }
}
```

### Advanced Queries

#### Pagination and Filtering

```graphql
query {
  products(pagination: { skip: 0, take: 5 }, query: "search_term") {
    id
    name
    description
    price
  }
}
```

## 📦 Service Details

### Account Service

Manages user accounts, authentication, and authorization.

### Catalog Service

Provides product catalog functionality with advanced search capabilities using Elasticsearch.

### Order Service

Handles order creation, processing, and management with a complete history of purchases.

### GraphQL API Gateway

Unifies all services under a single GraphQL API, providing a seamless experience for client applications.
