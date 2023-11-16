
# Kubernetes Project

<img src="banner.png">

This repository contains the implementation of a RESTful API in Node.js using the Express framework. The API is containerized into a Docker image, with implementation details in the Dockerfile.

## Navigating to the Docker Image

The Docker image is hosted on Docker Hub and can be found [here](https://hub.docker.com/repository/docker/sareenv/test-repo).

## Running Docker Container

**Assumption**: Docker must be installed on your system.

To run the project as a Docker container:

* **Pull the Image**:
    ```bash
    docker pull sareenv/test-repo:1
    ```
* **Run the Container**:
    ```bash
    docker run -p 8080:8080 -d test-repo
    ```
    The `-p` option maps the port, and `-d` runs it in detached mode.

## Kubernetes Configuration Files

All Kubernetes specifications are in the `k8s` folder with `.yaml` extensions. Use the following command to apply these configurations:
```bash
kubectl apply -f file_name
```
The `mongo-express` folder also contains YAML configurations for learning purposes and is not directly linked to our project. *This directory will be removed soon.*

## Google Kubernetes Engine (GKE)

We use GCP (Google Cloud Platform) tools for deploying node clusters, taking advantage of the $300 free credit available for 90 days. The Google Cloud command-line interface is used for managing the cluster.

- **Install the CLI Tool**:
    ```bash
    docker pull google/cloud-sdk
    docker run -it google/cloud-sdk:latest
    ```
    The above command runs the container in interactive mode.
- **Authenticate to Your Project**:
    ```bash
    gcloud auth login
    ```
    Follow the generated link to log in to your Google account.
- **Create and Configure the Project**:
    ```bash
    gcloud projects create gke-project-comp6231
    gcloud config set project gke-project-comp6231
    ```
- **Select Region and Create the Cluster**:
    ```bash
    gcloud container get-server-config --zone asia-east2-c
    ```
    ```bash
    gcloud container clusters create gke-project-comp6231 \
    --cluster-version 1.18.16-gke.500 \
    --disk-size 200 \
    --num-nodes 1 \
    --machine-type e2-small \
    --no-enable-cloud-logging \
    --no-enable-cloud-monitoring \
    --zone asia-east2-c
    ```
- **Delete the Cluster After Testing**:
    ```bash
    gcloud container clusters delete gke-project-comp6231 --zone asia-east2-c
    ```

## Manual MongoDB ReplicaSet Scaling

```bash
kubectl scale statefulsets mongo --replicas=4
```

## AutoScaling & Load Testing

We use HPA (Horizontal Pod Autoscaler) in Kubernetes:
```bash
kubectl autoscale deployment app-deployment --cpu-percent=60 --min=3 --max=10
```
For load testing, we use the Autocannon library from npm ([Autocannon npm package](https://www.npmjs.com/package/autocannon)). After installation, the following command sends traffic to the Node.js API's load-balancing service:
```bash
autocannon -c 2000 -d 10 -p 200 EXTERNAL_IP
```

## Authors

This Kubernetes project is developed for the COMP6231 module and maintained by:

- Vinayak Sareen
- Nadip
- Tanzia
- Protim
