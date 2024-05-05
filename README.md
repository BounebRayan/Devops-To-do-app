# My To-Do List Deployment with ArgoCD on Bare Metal Kubernetes Cluster

## Introduction
This repository contains the configuration files and deployment manifests for deploying "My To-Do List" app onto a bare metal Kubernetes cluster using ArgoCD for GitOps-style deployments. The App is a web application consisting of frontend, backend, and MongoDB components.

## ArgoCD Integration
ArgoCD is used to automate and simplify the deployment process of My To-Do List. With ArgoCD, deployments are managed through GitOps principles, allowing us to declare the desired state of our application in Git repositories. ArgoCD continuously monitors these repositories and ensures that the actual state of the cluster matches the desired state defined in the Git repository.

For ArgoCD installation guide, refer to: [ArgoCD Documentation](https://argo-cd.readthedocs.io/en/stable/getting_started/)

## Bare Metal Nginx Ingress Controller
To expose the application to the external world, we've configured a bare metal Nginx Ingress Controller. This controller is set up as a NodePort service to act as the entry point for incoming traffic. Here's how it's configured:

- **NodePort Service**: The Nginx Ingress Controller is exposed using a NodePort service type, allowing external traffic to access it through any of the Kubernetes cluster nodes.
- **Node IP Configuration**: Each node's IP address is added to the `/etc/hosts` file for DNS resolution. This ensures that requests to the application are properly directed to the Nginx Ingress Controller.

To deploy Nginx Ingress Controller, apply the following manifest:
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.10.1/deploy/static/provider/baremetal/deploy.yaml

## Instructions 
1.Make sure you have ArgoCD installed and configured in your Kubernetes cluster.
2.Apply the Nginx Ingress Controller manifest provided above.
3.Create a namespace called workshop.
4.Create and sync the guestbook app.
5.Automate the sync with argocd app set guestbook --sync-policy automated.
6.Map your domain to your nodes' IP addresses.
7.Access the website using the Nginx Controller NodePort service.

## k8s manifests
frontend-deployment: The frontend of the application, providing the user interface.
backend-deployment: The backend logic of the application.
deploy: The database used by the application.
frontend-service: ClusterIP service for the frontend component.
backend-service: ClusterIP service for the backend component.
service: ClusterIP service for the MongoDB component.
full_stack_lb: An ingress controller to route traffic to the frontend and backend services.
secrets : the secret manifest.

## Support
If you encounter any issues or have questions regarding the deployment process, feel free to open an issue in this repository or reach out to the maintainers for assistance.
