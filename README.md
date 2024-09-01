
![Awesome ReadME](https://i.ibb.co/tZ4ktrM/Untitled.png)

# A To-Do List App Deployment On Bare-metal Cluster

This repository contains the configuration files and deployment manifests for deploying "My To-Do List" app onto a bare metal Kubernetes cluster using ArgoCD for GitOps-style deployments.

## Getting Started

These instructions will give you a copy of the project up and running on
your machine for development and testing purposes.

### Prerequisites

Before diving in, we need to configure our cluster environment

- Deploy Nginx Ingress Controller by applying the following manifest

      kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.10.1/deploy/static/provider/baremetal/deploy.yaml

- Install ArgoCD in the cluster, you can refer to: [ArgoCD Documentation](https://argo-cd.readthedocs.io/en/stable/getting_started/)

### Installing

You'll first need to clone the repository

    git clone https://github.com/BounebRayan/Devops-To-do-app.git

Navigate to the k8s_manifests folder and apply the wanted manifests

    kubectl apply -f .

## ArgoCD Intergration

To integrate ArgoCD, first ensure that ArgoCD is installed and configured in your Kubernetes cluster. Create a namespace named workshop to organize your resources. Then, deploy and synchronize the guestbook application using ArgoCD. To enable automatic synchronization, set the sync policy to automated with the following command:

    argocd app set guestbook --sync-policy automated
## Exposing your Application

To access the Nginx Ingress Controller service of NodePort type, you should use one of your Kubernetes nodes' IP addresses and the assigned NodePort.

    kubectl get svc -n ingress-nginx

You'll need to ensure that your domain (as specified in the `host` field of your `ingress.yaml`) is correctly mapped to your nodes' IP addresses. If your nodes lack external IP addresses (common in nested clusters), you can set up port forwarding on your host machine.

**Enable IP forwarding:**

    sudo sysctl -w net.ipv4.ip_forward=

**Set Up Port Forwarding with iptables:**

For example, if your Kubernetes node's internal IP is `192.168.50.10` and the NodePort is `30255`, use the following commands:


    sudo iptables -t nat -A PREROUTING -p tcp --dport 30255 -j DNAT --to-destination 192.168.50.10:30255
    sudo iptables -t nat -A POSTROUTING p tcp --dport 30255 -j MASQUERADE

You can use ports 80 and 443 on your VPS, but you’ll need to configure Ingress to use a TLS certificate. You can obtain one with Certbot:

Install Certbot:


    sudo snap install certbot --classic


Obtain a TLS certificate (temporarily disable any port forwarding on port 80 to validate challenges):

    sudo certbot certonly --standalone


## Contributing

Contributions are welcome! If you find any issues or have suggestions for improvements, please open an issue or submit a pull request.
