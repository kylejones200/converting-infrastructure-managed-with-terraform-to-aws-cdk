---
author: "Kyle Jones"
date_published: "October 18, 2024"
date_exported_from_medium: "November 10, 2025"
canonical_link: "https://medium.com/@kyle-t-jones/converting-infrastructure-managed-with-terraform-to-aws-cdk-f2c012288f48"
---

# Converting Infrastructure managed with Terraform to AWS CDK Terraform is a popular infrastructure-as-code tool. And I'm a big fan.
I've written some other articles about using Terraform on AWS.

### Converting Infrastructure managed with Terraform to AWS CDK
Terraform is a popular infrastructure-as-code tool. And I'm a big fan. I've written some other articles about using Terraform on AWS.

But sometimes teams want to move from Terraform to CDK and my goal here is to help provide an approach for that.

#### Understanding Terraform vs. CDK
Terraform defines infrastructure using its language, HCL (HashiCorp Configuration Language). Like CloudFormation, it is declarative and operates across multiple cloud providers. AWS CDK, on the other hand, is primarily focused on AWS services and allows infrastructure to be defined imperatively in a programming language.

There are several reasons an organization might migrate from Terraform to CDK

**AWS-Specific Features:** CDK often provides better support for AWS-specific features and services. For teams heavily invested in AWS, CDK's native integration can simplify development and deployment.

**Programming Flexibility:** While Terraform's HCL is a capable language, it doesn't offer the full power of traditional programming languages like TypeScript or Python. CDK allows you to write more dynamic and reusable code.

#### Migrating a Simple Terraform Resource to CDK
Let's start with an example where Terraform provisions an S3 bucket with versioning enabled:

Terraform Configuration (HCL)

``` 
resource "aws_s3_bucket" "example" {
  bucket = "my-bucket"
  acl    = "private"
  versioning {
    enabled = true
  }
}
```

To migrate this to CDK, you would write the following TypeScript code:

```python
import * as s3 from 'aws-cdk-lib/aws-s3';
import * as cdk from 'aws-cdk-lib';

export class MyS3Stack extends cdk.Stack {
  constructor(scope: cdk.Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    const bucket = new s3.Bucket(this, 'MyBucket', {
      versioned: true,
      publicReadAccess: false,
    });
  }
}
```

#### Challenges with State Migration
One of the main challenges in migrating from Terraform to CDK is handling the state file. Terraform maintains a state file to track which resources have been provisioned, which helps it manage resources over time. CDK, however, uses CloudFormation as the underlying state manager.

To migrate infrastructure that's already deployed with Terraform, you may need to

Import existing resources into CDK**:** CDK supports resource importing, which allows you to bring existing infrastructure under CDK management without recreating the resources. You can use the cdk import command to import the state of existing resources.

#### Automating the Migration Process
No tool exists that directly converts Terraform configuration files into CDK constructs. Therefore, migration from Terraform to CDK usually involves manually rewriting the code. However, tools like Terraformer can help generate a Terraform state file from existing infrastructure, which can then be used to guide the CDK migration process.

### Step by step guide for migrating large applications
Migrating a large-scale application to AWS CDK requires careful planning and a phased approach to minimize risk and downtime.

A step-by-step guide to help you navigate this process.

#### Assessment and Discovery
Start by assessing your current infrastructure. Identify all resources currently deployed, such as compute resources (EC2 instances, Lambda functions), storage resources (S3, RDS, DynamoDB), and networking resources (VPC, Load Balancers). It's essential to map out how these resources interact with each other and identify critical dependencies.

Once you have a complete picture of the infrastructure, prioritize which resources to migrate first. It's often helpful to start with non-critical resources such as development environments before tackling production systems.

#### Breaking Down the Migration
It's crucial to break the migration down into manageable phases for large-scale applications. Trying to migrate everything simultaneously increases the risk of something going wrong and makes troubleshooting more difficult. A common strategy is to group resources by service type (e.g., databases, compute, storage) or environment (e.g., staging, production).

1.  [**Storage Resources (S3, RDS, DynamoDB)** Begin with relatively static resources like databases or object storage. These services often don't change frequently, which reduces the risk of disruption.]
2.  [**Compute Resources (EC2, Lambda)** Next, migrate your compute resources, including any EC2 instances, Auto Scaling groups, and Lambda functions. This may require re-architecting parts of your application to ensure compatibility with CDK's patterns.]
3.  [**Networking and Security (VPC, Security Groups, Load Balancers)** Once the core components are in place, move on to networking resources like VPCs, subnets, security groups, and load balancers. Ensure that all connections between resources remain intact.]

#### Testing and Validation
Thoroughly test the new infrastructure before each phase is completed. This should include functional testing (to ensure the application works as expected) and performance testing (to ensure the new setup performs well under load).

CDK makes it easy to spin up test environments using cdk deploy and destroy them afterwards using cdk destroy. Use these features to create test environments that mirror your production setup, allowing you to test without risking your live environment.

#### Full Rollout
Once you've completed the migration and all testing, deploy the new CDK-managed infrastructure into production. Consider using a Blue-Green Deployment approach, where both the old and new environments run side-by-side, allowing you to route traffic to the new infrastructure gradually.
