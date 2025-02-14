A2 Assignment

![image](https://github.com/user-attachments/assets/a87890ad-079a-4262-847a-e356d0254c99)

The application is a microservices-based system designed for a pet store, supporting both customer-facing and internal operations. Customers interact with the Store Front to browse products and place orders, which are handled by the Order Service and queued in Azure Service Bus for asynchronous processing. The Makeline Service retrieves and processes orders from the queue, updating the MongoDB order database. Employees use the Store Admin interface to manage inventory and track orders via the Product Service and Makeline Service. The Product Service also integrates with the AI Service to generate product descriptions and images using Azure OpenAI services (GPT-4 and DALL-E). The architecture ensures modularity, scalability, and efficient communication, with Azure Service Bus replacing RabbitMQ for message queuing, while MongoDB provides persistent order storage. This design allows seamless coordination between frontend interfaces, backend services, and AI-powered functionalities.


# Step-by-Step Instructions to Deploy the Application in a Kubernetes Cluster

1. Prerequisites
Set up Kubernetes: Use a cloud provider (e.g., AKS, EKS, GKE) or local setup (e.g., Minikube, Docker Desktop).

       - Install Tools: Install and configure kubectl to connect to the cluster.
       - Docker Images: Build and push images for each microservice to a registry like Docker Hub.
       - Prepare YAML Files: Ensure deployment, service, and secrets YAML files are ready.
3. Build and Push Docker Images
For each microservice:

Navigate to the directory with the Dockerfile.
Build the image:

       - docker build -t <dockerhub-username>/<service-name>:latest .
       - docker push <dockerhub-username>/<service-name>:latest
       
3. Create Kubernetes Secrets
Store sensitive information like API keys:

       - kubectl create secret generic openai-api-secret --from-literal=OPENAI_API_KEY=<your-key>
       - kubectl create secret generic azure-servicebus-secret --from-literal=SERVICEBUS_CONNECTION_STRING=<connection-string>
   
5. Apply Config Maps (If Needed)

       - Add non-sensitive configuration settings:
       - kubectl apply -f config-maps.yaml
       
6. Deploy Services
Apply deployment and service YAML files:

       - kubectl apply -f apps-all-in-one.yaml
   
8. Verify Deployments
Ensure all pods are running:

       - kubectl get pods
       - Expected status: Running.
   
10. Expose Frontend Services
Expose store-front and store-admin:

       - Use a LoadBalancer or NodePort in their service YAML files.
       - Apply the YAML files:
              - kubectl apply -f store-front-service.yaml
              - kubectl apply -f store-admin-service.yaml
         
12. Verify Services
       - kubectl get services
Access the application via the EXTERNAL-IP for store-front and store-admin.

13. Test the Application
       - Open the external IP for store-front in a browser to test customer functionality.
       - Open the external IP for store-admin to manage inventory and orders.
14. Debug Issues (If Needed)
       - Check logs:
              - kubectl logs <pod-name>
       - Describe pods:
              - kubectl describe pod <pod-name>
       
       
       | **Service**         | **Repository Link**                       |
       |---------------------|-------------------------------------------|
       | Store-Front         | `https://github.com/NikkiShaker/store-front-A2`                           |
       | Order-Service       | `https://github.com/NikkiShaker/order-service-A2`                           |
       | Store-Admin         | `https://github.com/NikkiShaker/store-admin-A2`                           |
       | Product-Service     | `https://github.com/NikkiShaker/product-service-A2`                           |
       | AI-Service          | `https://github.com/NikkiShaker/ai-service-A2`                           |
       | Makeline-Service    | `https://github.com/NikkiShaker/makeline-service-A2`                           |


       

       | **Service**         | **Docker Image Link**                     |
       |---------------------|-------------------------------------------|
       | Store-Front         | `https://hub.docker.com/repository/docker/nikkishaker/store-front-a2/general`                           |
       | Order-Service       | `https://hub.docker.com/repository/docker/nikkishaker/order-service-a2/general`                           |
       | Store-Admin         | `https://hub.docker.com/repository/docker/nikkishaker/store-admin-a2/general`                           |
       | Product-Service     | `https://hub.docker.com/repository/docker/nikkishaker/product-service-a2/general`                           |
       | AI-Service          | `https://hub.docker.com/repository/docker/nikkishaker/ai-service-a2/general`                           |
       | Makeline-Service    | `https://hub.docker.com/repository/docker/nikkishaker/makeline-service-a2/general`                           |

I was able to create a CI/CD pipeline for all of the microservices except product-service. Here's a screenshot of the error:

![image](https://github.com/user-attachments/assets/7a1cb615-c409-48ca-b3ce-f721523eb482)
