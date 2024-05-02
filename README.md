# Docker-project

Steps to download & run the project

DOWNLOAD:

1. Download and install Docker into you Machine (linux/mac or windonws) from official website
 # link : 
    https://www.docker.com/products/docker-desktop/
 # pull the code from Repository into you local machine
    git pull
2. All Instruction to create a Docker image is in the hello-node/Dockerfile

3. Also The docker-compose.yml file has the code to run the docker-compose

HOW TO RUN:

1. After Installing Docker RUN the below commands.
  # Build the Docker image
    docker build -t hello-node .

  # Run the Docker container, mapping port 3000 of the container to port 3000 on the host
    docker run -p 3000:3000 -d hello-node
    
2. To RUN docker-compose
  # Run the docker-compose
    docker-compose up

3. Access the API in browser
   # access the API by navigating to
       http://localhost:3000
   --------------------------------------------------------------------------------------------
4. install minikube and kubernetes-cli as per you OS and build the docker-image with name hello-node:v1
   # build
       docker build -t hello-node:v1 .
   
   # run the minikube
       minikube start
   
   # Ensure Metrics Server is installed in your Minikube cluster
       minikube addons enable metrics-server
   
   # apply the deployment of deployment.yaml, service.yaml and hpa.yaml files
       kubectl apply -f deployment.yaml
   
       kubectl apply -f service.yaml
   
       kubectl apply -f hpa.yaml
   
   # check the pods are runnig or not
       kubectl get pods

5. download and install hey from github
   # Install hey
       go get -u github.com/rakyll/hey
   
   # get the minicube ip
       minikube ip
   
   # apply the load 
       hey -c 50 -n 1000 http://$(minikube ip):3000

   # run the hpa command to check whether the load is building or not
       kubectl get hpa

   # observer the Autoscaling with get pods command
       kubectl get pods

# Monitoring PostgreSQL Using Prometheus & Grafana with HELM charts

# 1. Install helm
Helm is a package manager for Kubernetes, which is an open-source platform designed to automate deploying, scaling, and operating application containers. Helm helps users manage Kubernetes applications by defining, installing, and upgrading even the most complex Kubernetes applications. It uses charts, which are packages of pre-configured Kubernetes resources.

The official website for Helm is: https://helm.sh/docs/intro/install/

download helm based on your OS.

# 2. Install Prometheus and Grafana Using helm-charts
Referance Documentation is Below.
https://github.com/prometheus-community/helm-charts/

The kube-prometheus-stack is a collection of Kubernetes manifests, Grafana dashboards, and Prometheus rules combined with community best practices for monitoring Kubernetes clusters using Prometheus and Grafana. It includes a set of pre-configured monitoring components such as Prometheus, Grafana, Alertmanager, and kube-state-metrics, among others, that are commonly used for monitoring Kubernetes environments.

The kube-prometheus-stack is often used by Kubernetes administrators and operators to implement robust monitoring solutions for their clusters.

follow the below steps to install Prometheus & Grafana

    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    helm repo add grafana https://grafana.github.io/helm-charts
    helm repo update

    kubectl create namespace monitoring
    kubectl config set-context --current --namespace=monitoring

    helm install prometheus prometheus-community/kube-prometheus-stack
    helm upgrade --install grafana prometheus-community/kube-prometheus-stack
    
Use Port-forwarding to access the the Prometheus & Grafana over browser

    kubectl get svc
    kubectl port-forward svc/postgres-kube-prometheus-prometheus 9090
    kubectl port-forward svc/grafana 3000:80

# 3. Install PostgreSQL using bitnami Helm-chart
There are 3 ways you can deploy the persistent database

1. Using bitnami Helm-chart
2. Using deployment with persistent volume and persistent volume claim
3. Using statefulSet with Storage Class

The Bitnami PostgreSQL chart is a Helm chart provided by Bitnami, a company that specializes in packaging, deploying, and managing applications in various environments. The Bitnami PostgreSQL chart is specifically designed to simplify the deployment of PostgreSQL, an open-source relational database management system, on Kubernetes clusters using Helm.

By using the Bitnami PostgreSQL chart, users can streamline the process of deploying and managing PostgreSQL instances on Kubernetes, making it easier to set up and maintain databases for their applications.

    helm repo add bitnami https://charts.bitnami.com/bitnami
    helm install postgresql-dev bitnami/postgresql
    kubectl get pods
get the secret from secrets.yaml and decode the password with base64

    kubectl get secret/postgresql-dev -oyaml
    echo OTYwV0FRemZHZA== | base64 -d
Login to the database and create a sample table and data

    kubectl exec -it pod/postgresql-dev-0 sh
    
    POSTGRES_PASSWORD="UAiJI0d7d9"
    
    PGPASSWORD="$POSTGRES_PASSWORD" psql --host 127.0.0.1 -U postgres -d postgres -p 5432

    CREATE DATABASE testdb;

    CREATE TABLE hello_world 
    (
    region      text,
    country     text,
    year        int,
    production  int,
    consumption int
    );

    INSERT INTO hello_world (region, country, year, production, consumption) VALUES ('America', 'USA', 1998, 2014, 12897);
change the type of svc/postgresql-dev to LoadBalancer so to access over Grafana

    kubectl get svc
    kubectl edit svc/postgresql-dev
