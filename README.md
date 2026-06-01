# Converting Infrastructure managed with Terraform to AWS CDK

Published: 2024-10-18
Medium: [https://medium.com/@kyle-t-jones/converting-infrastructure-managed-with-terraform-to-aws-cdk-f2c012288f48](https://medium.com/@kyle-t-jones/converting-infrastructure-managed-with-terraform-to-aws-cdk-f2c012288f48)

## Business context

Terraform is a popular infrastructure-as-code tool. And I'm a big fan. I've written some other articles about using Terraform on AWS.

But sometimes teams want to move from Terraform to CDK and my goal here is to help provide an approach for that.

Terraform defines infrastructure using its language, HCL (HashiCorp Configuration Language). Like CloudFormation, it is declarative and operates across multiple cloud providers. AWS CDK, on the other hand, is primarily focused on AWS services and allows infrastructure to be defined imperatively in a programming language.



## Disclaimer

Educational/demo code only. Not financial, safety, or engineering advice. Use at your own risk. Verify results independently before any production or operational use.

## License

MIT — see [LICENSE](LICENSE).