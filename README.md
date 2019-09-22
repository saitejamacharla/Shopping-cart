# Hipster Shop: Cloud-Native Microservices Demo Application

This project contains a 10-tier microservices application. The application is a
web-based e-commerce app called **“Hipster Shop”** where users can browse items,
add them to the cart, and purchase them.

If you’re using this demo, please **★Star** this repository to show your interest!

> 👓**Note to Googlers:** Please fill out the form at
> [go/microservices-demo](http://go/microservices-demo) if you are using this
> application.

## Screenshots

| Home Page                                                                                                         | Checkout Screen                                                                                                    |
| ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| [![Screenshot of store homepage](./docs/img/hipster-shop-frontend-1.png)](./docs/img/hipster-shop-frontend-1.png) | [![Screenshot of checkout screen](./docs/img/hipster-shop-frontend-2.png)](./docs/img/hipster-shop-frontend-2.png) |

## Service Architecture

**Hipster Shop** is composed of many microservices written in different
languages that talk to each other over gRPC.

[![Architecture of
microservices](./docs/img/architecture-diagram.png)](./docs/img/architecture-diagram.png)

Find **Protocol Buffers Descriptions** at the [`./pb` directory](./pb).

| Service                                              | Language      | Description                                                                                                                       |
| ---------------------------------------------------- | ------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| [frontend](./src/frontend)                           | Go            | Exposes an HTTP server to serve the website. Does not require signup/login and generates session IDs for all users automatically. |
| [cartservice](./src/cartservice)                     | C#            | Stores the items in the user's shipping cart in Redis and retrieves it.                                                           |
| [productcatalogservice](./src/productcatalogservice) | Go            | Provides the list of products from a JSON file and ability to search products and get individual products.                        |
| [currencyservice](./src/currencyservice)             | Node.js       | Converts one money amount to another currency. Uses real values fetched from European Central Bank. It's the highest QPS service. |
| [paymentservice](./src/paymentservice)               | Node.js       | Charges the given credit card info (mock) with the given amount and returns a transaction ID.                                     |
| [shippingservice](./src/shippingservice)             | Go            | Gives shipping cost estimates based on the shopping cart. Ships items to the given address (mock)                                 |
| [emailservice](./src/emailservice)                   | Python        | Sends users an order confirmation email (mock).                                                                                   |
| [checkoutservice](./src/checkoutservice)             | Go            | Retrieves user cart, prepares order and orchestrates the payment, shipping and the email notification.                            |
| [recommendationservice](./src/recommendationservice) | Python        | Recommends other products based on what's given in the cart.                                                                      |
| [adservice](./src/adservice)                         | Java          | Provides text ads based on given context words.                                                                                   |
| [loadgenerator](./src/loadgenerator)                 | Python/Locust | Continuously sends requests imitating realistic user shopping flows to the frontend.                                              |





## 1.) Deploying your application using Amazon EKS.

Amazon EKS is a managed service that makes it easy for you to use Kubernetes on AWS without needing to install and operate your own Kubernetes control plane.

Step1: Create a VPC for your Cluster.

Step2: Create an EKS cluster.

Step3: Join 3 Worker nodes to the Cluster ( A 3 node Cluster is enough to run our application).

Step4: Clone the repo https://github.com/saitejamacharla/Shopping-cart.

Step5: Now we need to build docker images for the microservices.

Step6: Before building the docker images, we need a registry to store our images( So we are going to utilize amazon ECR for this).

Step 7: Create a repository for each microservice(ex: adservice, emailservice, ............).


Step8: Here are some of the commands:
- $(aws ecr get-login --no-include-email --region us-east-1)
- docker build -t adservice .
- docker tag adservice:latest 153439452303.dkr.ecr.us-east-1.amazonaws.com/adservice:latest
- docker push 153439452303.dkr.ecr.us-east-1.amazonaws.com/adservice:latest


Step9: After pushing the images, its to deploy them as kubernetes pods.


Step10: Now, we need to create kubernetes secret for the docker to pull image from ECR.
Note: https://dab.io/getting-started-with-kubernetes.html for more details on creating secrets



Step11. change your directory to cd/Shopping-cart/kubernetes-manifests/ 

Step12: Now make sure you change the image in the manifest file to your ecr repository name like this 153439452303.dkr.ecr.us-east-1.amazonaws.com/adservice:latest and also insert kubernetes secrets



Step13: Now create the deployment by - kubectl create -f adservice.yaml

Step14: create the deployments for all the microservices in the same way.

Step15: Kubectl get deployments (make sure all the deployments running)

Step16: kubectl get svc (copy the entrypoint for the frontend service)

Step17: got to web browser http://www.[your-ip]:[entrypoint]




