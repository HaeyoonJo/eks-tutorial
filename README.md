# EKS Tutorial

> Opened Jira for EKS Handson https://haeyoon-devops.atlassian.net/browse/DEVOPSS-61?atlOrigin=eyJpIjoiZmU2ZjliYTg4YTdjNGIxMGI4Yjg1ZTA2MTU4MTVjNGMiLCJwIjoiaiJ9


> Note that EKS access permission via CLI is only given to the user created its EKS Cluster

### 1. Create VPC using CloudFormation stack
```
aws cloudformation create-stack \
  --region eu-west-1 \
  --stack-name my-eks-vpc-stack \
  --template-url https://amazon-eks.s3.us-west-2.amazonaws.com/cloudformation/2020-10-29/amazon-eks-vpc-private-subnets.yaml
```

### 2. Create IAM Role and cluster

```
aws iam create-role \
  --role-name myAmazonEKSClusterRole \
  --assume-role-policy-document file://"cluster-role-trust-policy.json"

aws iam attach-role-policy \
  --policy-arn arn:aws:iam::aws:policy/AmazonEKSClusterPolicy \
  --role-name myAmazonEKSClusterRole


aws eks update-kubeconfig --region eu-west-1 --name my-cluster


  aws eks update-kubeconfig \
    --region eu-west-1 \
    --name my-eks-cluster \
    --role-arn arn:aws:iam::356971723581:role/myAmazonEKSClusterRole
```