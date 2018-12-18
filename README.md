# About

This project is designed to automate the deployment of Jfrog's Xray security-utility. Components will be deployed onto STIG-harderend, EL7-compatible Amazon EC2 instances and related AWS resources. Specifically, this project provides CloudFormation (CFn) templates for:

* Security Groups
* Simple Storage Service (S3)
* Identity and Access Management (IAM)
* "Classic" Elastic LoadBalancer
* Application LoadBalancer (a.k.a., "ELBv2")
* Standalone EC2 Xray instance
* Auto-scaling EC2 Xray instance
* "One-button" Xray service-host deployment
* "One-button" deployment of Xray-supporting AWS elements

## Design Assumption

These templates are intended for use within AWS [VPCs](https://aws.amazon.com/vpc/). It is further expected that the deployed-to VPCs will be configured with public and private subnets. All Xray elements &mdash; other than the Elastic LoadBalancer(s) &mdash; are expected to be deployed into private subnets. The Elastic LoadBalancers provide transit of Internet-originating requests to the the Xray service's web-based management-interface.

These templates _may_ work outside of such a networking-topology but have not been so tested.

## Notes on Templates' Usage

It is generally expected that the use of the various, individual-service templates will be run either via the "parent" templates or some other control framework (e.g. Jenkins).

The included "parent" template allows for a kind of "one-button" deployment method, executed with wholly AWS native tools. The "parent" templates attempt to make it so that all the template-user needs to worry about is populating the template's fields and ensuring that CFn can find the child templates. 

In order to use the "parent" template, it is recommended that the child templates be hosted in an S3 bucket separate from the one created for backups by this stack-set. The template-hosting bucket may be public or not. The files may be set to public or not. CFn typically has sufficient privileges to read the templates from a bucket without requiring the containing bucket or files being set public. Use of S3 for hosting eliminates the need to find other hosting-locations or sort out access-particulars of those hosting locations.

The EC2-related templates &mdash; either autoscale or instance &mdash; currently require that the scripts be anonymously curlable. The scripts can still be hosted in a non-public S3 bucket, however, either:
* The scripts' file-ACLs will need to allow `public-read`; or,
* The scripts will need to be referenced by way of [pre-signed URLs](https://docs.aws.amazon.com/AmazonS3/latest/dev/ShareObjectPreSignedURL.html).
This limitation may change in future releases &mdash; likely via an enhancement to the IAM template or fetch-method.

These templates (currently) do _not_ include Route53 functionality. It is assumed that the requisite Route53 or other DNS aliases will be configured separate from the instantiation of the public-facing ELB.


## Further Documentation:

See the `docs` subdirectory. 
