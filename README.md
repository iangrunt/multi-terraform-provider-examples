# Multi-provider Terraform and Terragrunt examples

### Overview
The two directories, `terraform-example`, and `terragrunt-example`, show small examples of calling modules that expect more than one Terraform provider with `terraform` or `terragrunt`. It's advisable to use `aws-vault` when testing locally, and you should call a profile in your `aws-vault exec` that can assume both of those roles. Different approaches to authentication are outside of the scope of this example. The purpose is to show two basic ways to configure your root Terraform module or your monolithic Terragrunt module.

### Extended examples
The two directories, `terraform-example-extended`, and `terragrunt-example-extended`, are where we can begin to see the benefits of Terragrunt when there is an unknown number of copies of the same provider required. In the Terraform example, we'll have to create many providers outside the default one. In the Terragrunt example, we could take that `production` folder, rename it to `development`, and customize how we'd want to use different aliases, accounts, and regions to launch modules requiring multiple providers. Additionally, the Terragrunt example is meant to show that a default provider configuration would be global in the root `terragrunt.hcl` file, and we can relegate those duplicate provider situations to the exact environments we need them in.

For example, a native Terraform root module that launches an Elastic Kubernetes Service (EKS) cluster, some S3 buckets, and then has to do a cross-account provider call will have that additional provider configuration next to modules that don't need it; the EKS cluster and the S3 buckets. Using Terragrunt, the only time you see that more complicated provider configuration is next to the modules that use them. This would prevent the EKS cluster or S3 bucket modules from inadvertently using the incorrect module and launching in an unexpected place.

### Authentication
Since these modules only need to authenticate to Amazon Web Services (AWS), and don't create anything, the role used in this example is incredibly small; it only allows itself to be assumed. No other permissions are required:

```
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "DoNothing",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::673239105528:user/ian@gruntwork.io"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

### Visuals
The basic Terragrunt example:
![Terragrunt example](/images/terragrunt-example.gif)

The extended Terragrunt example, which shows how to keep `provider` information DRY:
![Terragrunt example extended](/images/terragrunt-example-extended.gif)

