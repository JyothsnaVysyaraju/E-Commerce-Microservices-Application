# Product Catalog Microservice

This Docker image provides a microservice for managing a product catalog. It allows retrieving all products and fetching a single product by its ID.

```
docker run -p 3001:3001 -e MONGODB_HOST=<mongo_host> -e MONGODB_PORT=<mongo_port> -e MONGODB_DATABASE=<mongo_database> product-catalog-microservice
```

## Usage

To run a container from this image, use the following command:

```bash
docker run -p 3001:3001 product-catalog-microservice
```

The microservice will be accessible at `http://localhost:3001`.

### API Endpoints

- `GET /api/products`: Retrieves all products.
- `GET /api/products/:id`: Retrieves a single product by its ID.

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
   docker build -t jyothsnav/product-catalog-microservice .
   ```
   This command will build the Docker image using the provided Dockerfile and tag it as `product-catalog-microservice`.
