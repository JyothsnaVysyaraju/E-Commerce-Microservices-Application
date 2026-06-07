# Product Inventory Microservice

This Docker image provides a microservice for managing product inventory. It allows retrieving inventory for all products, fetching inventory for a single product by its ID, and reducing the quantity of a product when an order is placed.

```
docker run -p 3002:3002 -e POSTGRES_HOST=<postgres_host> -e POSTGRES_PORT=<postgres_port> -e POSTGRES_DB=<postgres_db> -e POSTGRES_USER=<postgres_user> -e POSTGRES_PASSWORD=<postgres_password> product-inventory-microservice
```

## Usage

To run a container from this image, use the following command:

```bash
docker run -p 3002:3002 product-inventory-microservice
```

The microservice will be accessible at `http://localhost:3002`.

### API Endpoints

- `GET /api/inventory`: Retrieves inventory for all products.
- `GET /api/inventory/<product_id>`: Retrieves inventory for a single product by its ID.
- `POST /api/order/<product_id>`: Reduces the quantity of a product by 1 when an order is placed.

## Environment Variables

This microservice does not require any environment variables to be set.

## Volumes

This microservice does not use any volumes.

## Building the Image

If you want to build the Docker image yourself, follow these steps:

1. Clone the repository containing the application code.
2. Navigate to the directory where the Dockerfile is located.
3. Run the following command to build the image:
   ```bash
   docker build -t jyothsnav/product-inventory-microservice .
   ```
   This command will build the Docker image using the provided Dockerfile and tag it as `product-inventory-microservice`.
