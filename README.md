# Full-Stack Cloud-Native Project (Algonquin Pet Store Application)

A microservices-based pet store application designed to handle customer orders, inventory management, and product descriptions using AI services. The system is deployed on Azure using Kubernetes for scalability and reliability.
<br>
<br>

![pet_store_microservices](https://github.com/user-attachments/assets/d42e53cf-74b8-4beb-b276-7f6c1c260cd5)


The application is a microservices-based system designed for a pet store, supporting both customer-facing and internal operations. Customers interact with the Store Front to browse products and place orders, which are handled by the Order Service and queued in Azure Service Bus for asynchronous processing. The Makeline Service retrieves and processes orders from the queue, updating the MongoDB order database. Employees use the Store Admin (also known as the Management service) interface to manage inventory and track orders via the Product Service and Makeline Service. The Product Service also integrates with the AI Service to generate product descriptions and images using Azure OpenAI services (GPT-4 and DALL-E). The architecture ensures modularity, scalability, and efficient communication, with Azure Service Bus replacing RabbitMQ for message queuing, while MongoDB provides persistent order storage. This design allows seamless coordination between frontend interfaces, backend services, and AI-powered functionalities.

## Technologies Used

### Cloud & Kubernetes
- **Azure** (Azure Kubernetes Service - AKS, Azure Storage, Azure OpenAI, Azure Service Bus)
- **Kubernetes** (Container orchestration for microservices)
- **Azure Kubernetes Service (AKS)** (Managed Kubernetes service on Azure)
- **Docker** (For containerizing microservices)
- **Helm** (If used for deployment management)

### Programming Languages
- **Python** (For developing the REST API-based management microservice)

### Microservices & Backend
- **RESTful APIs** (For communication between microservices)
- **MongoDB** (For order database storage)
- **Azure Service Bus** (For message queuing)
- **Docker** (For containerization of microservices)
- **Kubernetes** (For orchestrating and deploying microservices)

### AI Integration
- **GPT-4** (Via Azure OpenAI for product descriptions)
- **DALL-E** (Via Azure OpenAI for image generation)

### CI/CD & Automation
- **GitHub Actions** (For automating continuous integration and deployment workflows)

### Version Control
- **GitHub** (For managing code repositories and collaboration)

## Microservice Descriptions

This application is designed with a microservices architecture, where each service has a specific responsibility.

1. **Order Service**: Handles customer orders, receives requests from `store-front`, and processes them into the order queue.
2. **Product Service**: Manages product information, including fetching and updating product details, which is used by both the `store-front` and `store-admin`.
3. **Makeline Service**: Processes orders from the `order-queue` and updates the order database, handling the order preparation workflow.
4. **Store-Admin Service**: Provides an interface for employees to manage products and monitor orders. It is linked to `product-service` and `makeline-service`. It's also known as the Management microservice.
6. **AI Service**:
   - The **Large Language Model (LLM)**: Used to generate product descriptions based on product data, integrated via API.
   - The **Image Generation Model**: Uses DALL-E to generate product images for items in the inventory.

## Step-by-Step Instructions to Deploy the Application in a Kubernetes Cluster

1. Prerequisites
Set up Kubernetes: Use a cloud provider (e.g., AKS, EKS, GKE) or local setup (e.g., Minikube, Docker Desktop).

       - Install Tools: Install and configure kubectl to connect to the cluster.
       - Docker Images: Build and push images for each microservice to a registry like Docker Hub.
       - Prepare YAML Files: Ensure deployment, service, and secrets YAML files are ready.
2. Build and Push Docker Images
For each microservice:

       - Navigate to the directory with the Dockerfile.
       - Build the image:
              - docker build -t <dockerhub-username>/<service-name>:latest .
              - docker push <dockerhub-username>/<service-name>:latest
       
3. Create Kubernetes Secrets
Store sensitive information like API keys:

       - kubectl create secret generic openai-api-secret --from-literal=OPENAI_API_KEY=<your-key>
       - kubectl create secret generic azure-servicebus-secret --from-literal=SERVICEBUS_CONNECTION_STRING=<connection-string>
   
4. Apply Config Maps (If Needed)

       - Add non-sensitive configuration settings:
       - kubectl apply -f config-maps.yaml
       
5. Deploy Services
Apply deployment and service YAML files:

       - kubectl apply -f apps-all-in-one.yaml
   
6. Verify Deployments
Ensure all pods are running:

       - kubectl get pods
       - Expected status: Running.
   
7. Expose Frontend Services

       - Expose store-front and store-admin:
              - Use a LoadBalancer or NodePort in their service YAML files.
              - Apply the YAML files:
                     - kubectl apply -f store-front-service.yaml
                     - kubectl apply -f store-admin-service.yaml
         
9. Verify Services
       - kubectl get services
Access the application via the EXTERNAL-IP for store-front and store-admin.

10. Test the Application
       - Open the external IP for store-front in a browser to test customer functionality.
       - Open the external IP for store-admin to manage inventory and orders.
11. Debug Issues (If Needed)
       - Check logs:
              - kubectl logs <pod-name>
       - Describe pods:
              - kubectl describe pod <pod-name>

   - ### Here is the GitHub Repository links to each microservice
         

       | **Service**         | **Repository Link**                       |
       |---------------------|-------------------------------------------|
       | Store-Front         | `https://github.com/NikkiShaker/store-front-A2`                           |
       | Order-Service       | `https://github.com/NikkiShaker/order-service-A2`                           |
       | Store-Admin         | `https://github.com/NikkiShaker/store-admin-A2`                           |
       | Product-Service     | `https://github.com/NikkiShaker/product-service-A2`                           |
       | AI-Service          | `https://github.com/NikkiShaker/ai-service-A2`                           |
       | Makeline-Service    | `https://github.com/NikkiShaker/makeline-service-A2`                           |

   - ### Here is the Docker Image links to each microservice

       | **Service**         | **Docker Image Link**                     |
       |---------------------|-------------------------------------------|
       | Store-Front         | `https://hub.docker.com/repository/docker/nikkishaker/store-front-a2/general`                           |
       | Order-Service       | `https://hub.docker.com/repository/docker/nikkishaker/order-service-a2/general`                           |
       | Store-Admin         | `https://hub.docker.com/repository/docker/nikkishaker/store-admin-a2/general`                           |
       | Product-Service     | `https://hub.docker.com/repository/docker/nikkishaker/product-service-a2/general`                           |
       | AI-Service          | `https://hub.docker.com/repository/docker/nikkishaker/ai-service-a2/general`                           |
       | Makeline-Service    | `https://hub.docker.com/repository/docker/nikkishaker/makeline-service-a2/general`                           |

## Usage Instructions

1. **Customer Interaction**: 
   - Navigate to the external IP of `store-front` in a browser to access the customer-facing part of the application. Customers can browse products and place orders.
   
2. **Employee Interaction**:
   - Navigate to the external IP of `store-admin` to manage inventory and track orders. Employees can update stock levels and order statuses via the Store Admin interface.

3. **Interacting with the Services**: 
   - Once deployed, all services communicate with each other through the API endpoints defined in their respective microservices.

4. **Debugging**: 
   - If you face any issues, you can view the logs:
     ```bash
     kubectl logs <pod-name>
     ```

## AI Service Description

The AI service integrates two models to enhance product descriptions and images:

1. **Large Language Model (LLM)**: 
   - Uses Azure OpenAI's GPT-4 to generate creative and detailed product descriptions based on minimal input from the product service. This helps in creating more engaging product details for the store.
   
2. **Image Generation Model**:
   - Integrates with **DALL-E** (also from Azure OpenAI) to automatically generate images for products that might not have pre-existing visuals, providing a more attractive and dynamic shopping experience for customers.


