### Ingress controller

Reference
```
https://kubernetes-sigs.github.io/aws-load-balancer-controller/v2.6/
```

1. eksctl utils associate-iam-oidc-provider \  (IAM OIDC provider bcz authentication aws and eks)
    --region <region-code> \us-east-1
    --cluster <your-cluster-name> \eks-spot-cluster
    --approve
   install this in workstation. oidc provider

2. we are giving access to the node to create load balancer.

curl -o iam-policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.6.1/docs/install/iam_policy.json

3. create policy

aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam-policy.json

4. eksctl create iamserviceaccount \
--cluster=<cluster-name> \
--namespace=kube-system \
--name=aws-load-balancer-controller \
--attach-policy-arn=arn:aws:iam::<AWS_ACCOUNT_ID>:policy/AWSLoadBalancerControllerIAMPolicy \
--override-existing-serviceaccounts \
--region <region-code> \
--approve

5. add the EKS chart repo to Helm  --->actual controllers comes frim helm charts.

helm repo add eks https://aws.github.io/eks-charts

6. helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=<cluster-name> --set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer-controller


installed some controllers in kube-system namespace.