# iden-assignment

This project demonstrates the provisioning of an Amazon EKS cluster using Terraform and the deployment of a simple "Hello World" application using Kubernetes manifests. The application UI is accessible through an endpoint using an Ingress controller.

---

## 🧱 Project Structure

```
project-root/
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   └── modules/
│       ├── vpc/
│       └── eks/
├── k8s-manifests/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── ingress.yaml
└── README.md
```

---

## 📅 Task 1: Provisioning EKS using Terraform

### ✅ Steps Followed:

#### 1. **Terraform Initialization**
- Followed modular approach by creating separate modules for VPC, EKS, and node group.

#### 2. **VPC Module**
- Created a custom VPC with both public and private subnets spread across 3 Availability Zones (AZs).

#### 3. **EKS Cluster Module**
- Provisioned the EKS cluster using `aws_eks_cluster` resource.

#### 4. **Node Group Module**
- Created managed node groups using the `aws_eks_node_group` resource.
- Used `t2.medium` instance type for worker nodes.

#### 5. **IAM Roles & Policies**
- Attached necessary IAM roles and policies for EKS cluster and worker nodes.

#### 6. **Outputs**
- Exported cluster endpoint, cluster name, and VPC ID as Terraform outputs.

### 💻 Terraform Commands
```bash
terraform init
terraform validate
terraform plan
terraform apply -auto-approve
```

---

## 🚀 Task 2: Deploy Hello World App with Ingress

### ✅ Prerequisites
- Cluster access configured via:
  ```bash
  aws eks --region <region> --name <cluster_name> --update-kubeconfig
  ```
- `kubectl` and `helm` installed and configured.

### 🔧 Step-by-Step:

#### 1. **Install NGINX Ingress Controller**
```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install nginx-ingress ingress-nginx/ingress-nginx \
  --namespace ingress-nginx --create-namespace
```

#### 2. **Apply the Kubernetes Manifests**
```bash
kubectl apply -f k8s-manifests/
```

#### 3. **Verify Ingress Controller is Running**
```bash
kubectl get all -n ingress-nginx
```
Example Output:
```
NAME                                                        READY   STATUS    RESTARTS   AGE
pod/nginx-ingress-ingress-nginx-controller-76468dcf68-f58nh 1/1     Running   0          2m

NAME                                                        TYPE           CLUSTER-IP     EXTERNAL-IP                                                              PORT(S)                      AGE
service/nginx-ingress-ingress-nginx-controller              LoadBalancer   172.20.217.60  a86d45a86417a405c9ac688c59f776b0-302734910.us-west-2.elb.amazonaws.com   80:32707/TCP,443:32441/TCP   2m
service/nginx-ingress-ingress-nginx-controller-admission    ClusterIP      172.20.228.185 <none>                                                                   443/TCP                      2m

NAME                                                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-ingress-ingress-nginx-controller      1/1     1            1           2m

NAME                                                        DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-ingress-ingress-nginx-controller-76468dcf68   1         1         1       2m
```

#### 4. **Access the Hello World App**
Open your browser and go to the `EXTERNAL-IP` of the ingress controller:
```
http://a86d45a86417a405c9ac688c59f776b0-302734910.us-west-2.elb.amazonaws.com
```
You should see the Hello World application UI.

---



