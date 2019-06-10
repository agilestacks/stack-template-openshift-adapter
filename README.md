# OpenShift (in AWS) Adapter Stack

### Import Requirements

In order to properly import OpenShift or OKD cluster into AgileStacks infrastructure the following
requirements must be met:

1. OpenShift or OKD cluster MUST be provisioned in AWS infrastructure.
2. Cloud Account of selected Environment MUST be the Cloud Account where OpenShift cluster is provisioned, otherwise, AgileStacks automation will not be able to properly import the cluster.
3. OpenShift user whose token is used to onboard the cluster MUST have `cluster-admin` role.
4. Certain AgileStacks Components, such as ingress controller (Traefik) or TLS certificate manager needs access to AWS resources in order to function properly. For example, such resources are (but no limited to) Route53 (for DNS ACME challenges) or Elastic Load Balancer (ELB) (in order to expose Kubernetes services to outside). 
The following (or less restrictive) AWS IAM policy should be assigned to the instance profile(s) of OpenShift master and worker nodes:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ecr:CompleteLayerUpload",
        "ecr:UploadLayerPart",
        "ecr:InitiateLayerUpload",
        "ecr:GetAuthorizationToken",
        "ecr:BatchCheckLayerAvailability",
        "ecr:GetDownloadUrlForLayer",
        "ecr:GetRepositoryPolicy",
        "ecr:DescribeRepositories",
        "ecr:ListImages",
        "ecr:BatchGetImage",
        "acm:GetCertificate",
        "acm:ListCertificates",
        "route53:ListHostedZonesByName",
        "route53:ListResourceRecordSets",
        "route53:ChangeResourceRecordSets",
        "route53:GetChange",
        "ec2:DescribeVolume*",
        "ec2:CreateVolume",
        "ec2:CreateTags",
        "ec2:DescribeInstance*",
        "ec2:AttachVolume",
        "ec2:DetachVolume",
        "ec2:DeleteVolume",
        "ec2:DescribeSubnets",
        "ec2:CreateSecurityGroup",
        "ec2:DescribeSecurityGroups",
        "ec2:DescribeRouteTables",
        "ec2:AuthorizeSecurityGroupIngress",        
        "elasticloadbalancing:DescribeTags",
        "elasticloadbalancing:CreateLoadBalancerListeners",
        "elasticloadbalancing:ConfigureHealthCheck",
        "elasticloadbalancing:DeleteLoadBalancerListeners",
        "elasticloadbalancing:RegisterInstancesWithLoadBalancer",
        "elasticloadbalancing:DescribeLoadBalancers",
        "elasticloadbalancing:CreateLoadBalancer",
        "elasticloadbalancing:DeleteLoadBalancer",
        "elasticloadbalancing:ModifyLoadBalancerAttributes",
        "elasticloadbalancing:DescribeLoadBalancerPolicies",
        "elasticloadbalancing:DescribeLoadBalancerAttributes",
        "elasticloadbalancing:CreateLoadBalancerPolicy",
        "elasticloadbalancing:SetLoadBalancerPoliciesOfListener",
        "s3:*"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
```

### Import Instructions (from Controlplane)

#### Steps

1. Select Cluster type OpenShift Cluster.
2. Select existing adapter template for OpenShift Cluster or select Create a new one.
3. Select Environment which is using onboarded Cloud Account. NOTE: Cloud Account of selected Environment MUST be the Cloud Account where OpenShift cluster is provisioned! This field is required.
4. Enter the name of OpenShift Cluster. This field is required.
5. Enter endpoint of the cluster. Must contain hostname and port. Example `shifty.superhub.io:8443`. This field is required.
6. Enter OpenShift session token. In order to get a token go to OpenShift web console -> in upper right corner click on your username -> click `Copy Login Command`. This field is required. NOTE: OpenShift user whose token is used to onboard the cluster MUST have `cluster-admin` role.
7. If self-signed certificate or root CA of your organization was used to provision your OpenShift cluster, then the root CA certificate must be provided in `Certificate of authority` field.
8. Select one or more core components.
9. Click Import.
