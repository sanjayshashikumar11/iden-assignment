# iden-assignment
This project demonstrates the provisioning of an Amazon EKS cluster using Terraform and deploying a simple Hello World application to the cluster using Kubernetes manifests. The application UI is accessible via an endpoint using an Ingress controller.

🧱 Project Structure

project-root/
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   └── modules/
│       ├── vpc/
│       ├── eks/
├── k8s-manifests/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── ingress.yaml

Task 1: Provisioning EKS using Terraform.

✅ Steps Followed:

1. Terraform Initialization:

Used modular approach by creating vpc, eks, and node_group modules.

2. VPC Module:

Created a custom VPC with public and private subnets across 3 AZs.

3. EKS Cluster Module:

Provisioned EKS cluster using aws_eks_cluster.

4. Node Group Module:

Created managed node group with t2.medium instance type.

5. IAM Roles & Policies:

Attached necessary IAM roles and policies for EKS cluster and worker nodes.

6. Outputs:

EKS cluster endpoint, EKS cluster name, VPC ID.

🖥 Terraform Commands:

terraform init
terraform validate
terraform plan
terraform apply -auto-approve

------

Task 2: Deploy Hello World App with Ingress

✅ Prerequisites:

Cluster access configured via aws eks --update-kubeconfig

kubectl and helm installed

🔧 Step-by-Step:

1. Install NGINX Ingress Controller:
   
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install nginx-ingress ingress-nginx/ingress-nginx \
  --namespace ingress-nginx --create-namespace

2. Apply the Manifests:

kubectl apply -f k8s-manifests/

3. Get Ingress Address:

 kubectl get all -n ingress-nginx
 
NAME                                                          READY   STATUS    RESTARTS   AGE
pod/nginx-ingress-ingress-nginx-controller-76468dcf68-f58nh   1/1     Running   0          120m

NAME                                                       TYPE           CLUSTER-IP       EXTERNAL-IP                                                              PORT(S)                      AGE
service/nginx-ingress-ingress-nginx-controller             LoadBalancer   172.20.217.60    a86d45a86417a405c9ac688c59f776b0-302734910.us-west-2.elb.amazonaws.com   80:32707/TCP,443:32441/TCP   120m
service/nginx-ingress-ingress-nginx-controller-admission   ClusterIP      172.20.228.185   <none>                                                                   443/TCP                      120m

NAME                                                     READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-ingress-ingress-nginx-controller   1/1     1            1           120m

NAME                                                                DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-ingress-ingress-nginx-controller-76468dcf68   1         1         1       120m


Access the app in the browser using the EXTERNAL-IP shown, that is a86d45a86417a405c9ac688c59f776b0-302734910.us-west-2.elb.amazonaws.com 










