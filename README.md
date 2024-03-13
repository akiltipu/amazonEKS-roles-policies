# amazonEKS-roles-policies

### Contents

**aws-worker-node-role:** An IAM role that allows EC2 instances (worker nodes) to call AWS services on your behalf.

**AmazonEC2ContainerRegistryReadOnly:** An IAM policy that provides read-only access to Amazon Elastic Container Registry (Amazon ECR) repositories.

**AmazonEKS_CNI_Policy:** An IAM policy that grants permissions required by the Amazon VPC CNI Plugin (amazon-vpc-cni-k8s) to modify IP address configurations on EKS worker nodes.

**AmazonEKSWorkerNodePolicy:** An IAM policy that allows Amazon EKS worker nodes to connect to Amazon EKS clusters and perform necessary operations.

**node-group-autoscale-policy:** An IAM policy that enables AWS Auto Scaling to interact with the Kubernetes Cluster Autoscaler for automatically scaling node groups.

### Usage
These roles and policies can be used in conjunction with Amazon EKS to secure and manage access for various components within your Kubernetes cluster. Refer to the AWS documentation and best practices for detailed instructions on how to apply and utilize these resources effectively.


### aws-worker-node-role

Allows EC2 instances to call AWS services on your behalf.

**AmazonEC2ContainerRegistryReadOnly**

Provides read-only access to Amazon EC2 Container Registry repositories.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecr:GetAuthorizationToken",
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:GetRepositoryPolicy",
                "ecr:DescribeRepositories",
                "ecr:ListImages",
                "ecr:DescribeImages",
                "ecr:BatchGetImage",
                "ecr:GetLifecyclePolicy",
                "ecr:GetLifecyclePolicyPreview",
                "ecr:ListTagsForResource",
                "ecr:DescribeImageScanFindings"
            ],
            "Resource": "*"
        }
    ]
}
```

**AmazonEKS_CNI_Policy**

This policy provides the Amazon VPC CNI Plugin (amazon-vpc-cni-k8s) the permissions it requires to modify the IP address configuration on your EKS worker nodes. This permission set allows the CNI to list, describe, and modify Elastic Network Interfaces on your behalf. More information on the AWS VPC CNI Plugin is available here: https://github.com/aws/amazon-vpc-cni-k8s

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AmazonEKSCNIPolicy",
            "Effect": "Allow",
            "Action": [
                "ec2:AssignPrivateIpAddresses",
                "ec2:AttachNetworkInterface",
                "ec2:CreateNetworkInterface",
                "ec2:DeleteNetworkInterface",
                "ec2:DescribeInstances",
                "ec2:DescribeTags",
                "ec2:DescribeNetworkInterfaces",
                "ec2:DescribeInstanceTypes",
                "ec2:DescribeSubnets",
                "ec2:DetachNetworkInterface",
                "ec2:ModifyNetworkInterfaceAttribute",
                "ec2:UnassignPrivateIpAddresses"
            ],
            "Resource": "*"
        },
        {
            "Sid": "AmazonEKSCNIPolicyENITag",
            "Effect": "Allow",
            "Action": [
                "ec2:CreateTags"
            ],
            "Resource": [
                "arn:aws:ec2:*:*:network-interface/*"
            ]
        }
    ]
}
```

**AmazonEKSWorkerNodePolicy**

This policy allows Amazon EKS worker nodes to connect to Amazon EKS Clusters.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "WorkerNodePermissions",
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeInstances",
                "ec2:DescribeInstanceTypes",
                "ec2:DescribeRouteTables",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeSubnets",
                "ec2:DescribeVolumes",
                "ec2:DescribeVolumesModifications",
                "ec2:DescribeVpcs",
                "eks:DescribeCluster",
                "eks-auth:AssumeRoleForPodIdentity"
            ],
            "Resource": "*"
        }
    ]
}

```
**node-group-autoscale-policy**

Node Group Autoscaling policy for use AWS Auto scaling to k8 autoscaler

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "autoscaling:DescribeAutoScalingGroups",
                "autoscaling:DescribeAutoScalingInstances",
                "autoscaling:DescribeLaunchConfigurations",
                "autoscaling:DescribeTags",
                "autoscaling:SetDesiredCapacity",
                "autoscaling:TerminateInstanceInAutoScalingGroup",
                "ec2:DescribeLaunchTemplateVersions"
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}

```
