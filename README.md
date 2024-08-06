# eks-prometheus-grafana

#Create EKS cluster 

eksctl create cluster --node-volume-size=15 --zones=us-east-1a,us-east-1b
#Install csi driver for EBS 

#get your clustername 
aws eks list-clusters
cluster_name=floral-creature-1722365091

#Create an IAM OIDC identity provider
eksctl utils associate-iam-oidc-provider --cluster $cluster_name --approve
Create iam role 
eksctl create iamserviceaccount \
    --name ebs-csi-controller-sa \
    --namespace kube-system \
    --cluster $cluster_name \
    --role-name AmazonEKS_EBS_CSI_DriverRole \
    --role-only \
    --attach-policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy \
    --approve

eksctl create addon --name aws-ebs-csi-driver --cluster $cluster_name --service-account-role-arn arn:aws:iam::533267287911:role/AmazonEKS_EBS_CSI_DriverRole --force
