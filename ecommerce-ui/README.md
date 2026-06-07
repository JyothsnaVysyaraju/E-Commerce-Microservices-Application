# E-commerce UI

This Docker image provides a React application with a Node.js server for an e-commerce user interface. The application connects to six different microservices to provide the optimal e-commerce experience.

## Prerequisites

Before running a container from this image, ensure that you have the following:

- Docker installed on your system
- Access to the six microservices required by the application

## Usage

To run a container from this image, use the following command:

```bash
docker run --network=my-network -p 4000:4000 \\
-e REACT_APP_PROFILE_API_HOST=http://profile-management \\
-e REACT_APP_PRODUCT_API_HOST=http://product-catalog \\
-e REACT_APP_INVENTORY_API_HOST=http://product-inventory \\
-e REACT_APP_ORDER_API_HOST=http://order-management \\
-e REACT_APP_SHIPPING_API_HOST=http://shipping-and-handling \\
-e REACT_APP_CONTACT_API_HOST=http://contact-support-team \\
--name ecommerce-ui thallavi/ecommerce-ui
```

Replace `my-network` with the name of the Docker network where the required microservices are running. The container exposes port 4000, which you can map to a desired port on your host machine using the `-p` flag.

## Environment Variables

The container requires the following environment variables to be set:

- `REACT_APP_PROFILE_API_HOST`: The URL of the profile management microservice.
- `REACT_APP_PRODUCT_API_HOST`: The URL of the product catalog microservice.
- `REACT_APP_INVENTORY_API_HOST`: The URL of the product inventory microservice.
- `REACT_APP_ORDER_API_HOST`: The URL of the order management microservice.
- `REACT_APP_SHIPPING_API_HOST`: The URL of the shipping and handling microservice.
- `REACT_APP_CONTACT_API_HOST`: The URL of the contact support team microservice.

The application uses the provided environment variables to construct the full URL for making requests to the respective microservices. Each variable should be set to the scheme and hostname of the corresponding service, such as REACT_APP_PROFILE_API_HOST=http://profile-management. The application will then append the appropriate port and path, like :3003/api/update, to complete the URL and send the request to the specified microservice container.

## Building the Image

If you want to build the Docker image yourself, follow these steps:

1. Clone the repository containing the application code.

2. Navigate to the directory where the Dockerfile is located.

3. Run the following command to build the image:

   ```bash
   docker build -t jyothsnav/ecommerce-ui .
   ```

   This command will build the Docker image using the provided Dockerfile and tag it as `jyothsnav/ecommerce-ui`.
