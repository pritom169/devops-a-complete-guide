## demo app - developing with Docker

This demo app shows a simple user profile app set up using 
- index.html with pure js and css styles
- nodejs backend with express module
- mongodb for data storage

All components are docker-based

### With Docker

#### To start the application

Step 1: Create docker network

    docker network create mongo-network 

Step 2: start mongodb 

    docker run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --name mongodb --net mongo-network mongo    

Step 3: start mongo-express
    
    docker run -d -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password --net mongo-network --name mongo-express -e ME_CONFIG_MONGODB_SERVER=mongodb -e ME_CONFIG_MONGODB_URL=mongodb://mongodb:27017 mongo-express   

_NOTE: creating docker-network in optional. You can start both containers in a default network. In this case, just emit `--net` flag in `docker run` command_

Step 4: open mongo-express from browser

    http://localhost:8081

Step 5: create `user-account` _db_ and `users` _collection_ in mongo-express

Step 6: Start your nodejs application locally - go to `app` directory of project 

    cd app
    npm install 
    node server.js
    
Step 7: Access you nodejs application UI from browser

    http://localhost:3000

### With Docker Compose

#### To start the application

Step 1: start mongodb and mongo-express

    docker-compose -f docker-compose.yaml up
    
_You can access the mongo-express under localhost:8080 from your browser_
    
Step 2: in mongo-express UI - create a new database "user-account"

Step 3: in mongo-express UI - create a new collection "users" in the database "user-account"       
    
Step 4: start node server 

    cd app
    npm install
    node server.js
    
Step 5: access the nodejs application from browser 

    http://localhost:3000

#### To build a docker image from the application

    docker build -t my-app:1.0 .

The dot "." at the end of the command denotes location of the Dockerfile.

## Pushing Docker Image to AWS ECR

#### Prerequisites

1. **Install AWS CLI** (if not installed)

    ```bash
    brew install awscli
    ```

2. **Configure AWS CLI**

    ```bash
    aws configure
    ```

    You'll be prompted for:
    - AWS Access Key ID
    - AWS Secret Access Key
    - Default region name (e.g., `eu-central-1`)
    - Default output format (e.g., `json`)

3. **Add ECR Permissions to IAM User**

    - Go to AWS Console → IAM → Users → Your User → Add Permissions
    - Select "Attach policies directly"
    - Search for `AmazonEC2ContainerRegistryFullAccess`
    - Select it and click "Add permissions"

#### Steps to Push Image to ECR

Step 1: Create an ECR Repository (via AWS Console or CLI)

    aws ecr create-repository --repository-name js-app --region eu-central-1

Step 2: Authenticate Docker with ECR

    aws ecr get-login-password --region eu-central-1 | docker login --username AWS --password-stdin <account-id>.dkr.ecr.eu-central-1.amazonaws.com

Step 3: Tag the local image for ECR

    docker tag js-app:latest <account-id>.dkr.ecr.eu-central-1.amazonaws.com/js-app:latest

Step 4: Push the image to ECR

    docker push <account-id>.dkr.ecr.eu-central-1.amazonaws.com/js-app:latest

Step 5: Verify the push (optional)

    aws ecr describe-images --repository-name js-app --region eu-central-1

#### Changing Image Tags

To use a different tag (e.g., version number instead of `latest`):

    docker tag js-app:latest <account-id>.dkr.ecr.eu-central-1.amazonaws.com/js-app:v1.0.0
    docker push <account-id>.dkr.ecr.eu-central-1.amazonaws.com/js-app:v1.0.0

_NOTE: Replace `<account-id>` with your 12-digit AWS Account ID_
