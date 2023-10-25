# Kubernetes Specialization Module - Three-Tier Application Deployment

This module focuses on hands-on experience with Kubernetes and its components, specifically deploying a three-tier application on a multi-node Kubernetes cluster. We will use Kubespray or KinD to set up the cluster on your local machine. Once the cluster and application are manually deployed, we will move on to automating the application deployment using GitOps tools.

![Alt Text](kube.png)

## Tools and Components

We will use the following tools and components for this project:

- **Multinde minikube cluster** A cluster required for deployment of application and CICD pipeline.

- **Tekton for Continuous Integration (CI):** Tekton will help automate the CI pipeline for building and testing the application.

- **ArgoCD for Continuous Deployment (CD):** ArgoCD will be used to automate the CD pipeline for application deployment and updates.

## Project Deliverables

1. **Deployment of Kubernetes Cluster:** This step involves setting up a multi-node Kubernetes cluster. You can use Minikube for a local setup or other tools like Kubespray or KinD.

2. **Create Kube Manifest Files:** Define Kubernetes manifest files for the three components of the three-tier application: front end, backend, and database.

3. **Helm chart of whole application:** Helm chart helps quick deployment of a whole application.

4. **Verification and Testing:** Ensure that the deployment and services work correctly, both locally and via the CI/CD pipeline.

5. **NodePort for Frontend:** Expose the front end of the application using a NodePort service to make it accessible.

6. **Node Affinity and Replica Set Checks:** Implement node affinity checks for the front end and ensure that replica sets are correctly configured for the frontend and backend applications and shoould be 2.

7. **Build CI Pipeline Using Tekton:** Create a CI pipeline using Tekton to automate tasks like building, testing, and deploying the application.

8. **Build CD Pipeline Using ArgoCD:** Use ArgoCD to automate the CD pipeline for continuous deployment and updates.

9. **Create ReadMe File:** Document the project, including installation steps, usage instructions, and troubleshooting tips.

## Installation Instructions

### Setting Up Kubernetes Cluster

You can find a guide to install Minikube and deploy a Kubernetes cluster locally [here](https://www.linuxtechi.com/how-to-install-minikube-on-ubuntu/). Once the cluster is set up, run the following command:

#### Add a node to Minikube
minikube node add [flags] 

### Installing Tekton and ArgoCD

Installation instructions for Tekton and ArgoCD can be found in their respective documentation. Please follow the provided guides for setting up these tools in your Kubernetes cluster.
Issues Faced and Solutions

Run the following command to install Tekton and ArgoCD in the cluster.

#### for Tekton 

kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml


#### for ArgoCD

#### install ArgoCD in k8s
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

#### access ArgoCD UI
kubectl get svc -n argocd
kubectl port-forward svc/argocd-server 8080:443 -n argocd

#### login with admin user and below token (as in documentation):
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode && echo


### Issue 1: No Internet Access to Pods in Multi-Node Minikube Cluster

#### Solution: 
Run the following command to resolve the POD networking issue:


minikube start -n 3 --enable-default-cni=false --network-plugin=cni --cni='calico'

Here we are deploying minikube with calico which handles all networking.

### Issue 2: 

Tekton CI Pipeline Tasks are being failed becaue of the memoru issue with error "exit 137" 

#### Solution: 

Minikube nodes requires more resources for builing and pushing front end image to avoid out of memory issue.

#### Conclusion

This project will provide hands-on experience with Kubernetes, GitOps, and the CI/CD pipeline. By completing the steps outlined in this readme, you will deploy a three-tier application and automate the deployment process, enabling efficient application development and updates.
