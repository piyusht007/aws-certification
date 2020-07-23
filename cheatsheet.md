# Cheatsheet
##### CloudFormation
* Exported output values must have unique names within a single region.
* `!findInMap` intrinsic function - Returns values from a 2-level Map defined in the `Mapping` section.
* `!GetAtt` intrinsic function returns the value of an attribute from a resource in the template.
* Parameters cannot have `conditions`.

##### CodeBuild
* Can be run locally to check for errors.
* Provides full control over deployment steps using BLUE-GREEN deployment.

##### CodeDeploy
* To verify the success of a deployment, `ValidateService` hook can be used.

##### API Gateway
* `STS` is not supported for API gateway.
* `Lambda Authorizer` should be used while controlling access using a 3rd party authorization mechanism.
* Response can be manipulated/modified using `Mapping Templates`.
* API gateway needs proper permissions (`AmazonAPIGatewayPushToCloudWatchLogs`) to read/write CloudWatch logs.
* Authentication can be added to requests using `IAM permissions with sigv4`.
* `Stage Variables` and `Lambda Aliases` can be used to point to multiple environments (DEV, TEST, PROD - Precisely lambda aliases). Lambda Version can be updated over time behind Lambda Aliases. 

##### Elastic BeanStalk
* To encrypt payloads in Elastic BeanStalk, `AWS Encryption SDK` can be used.

##### AWS Route53
* `CNAME` records can be created to have hostnames mappings to 3rd party hosted domains.
    * www.mydomain.com -> yourapp.3rdparty.com (Hosted by 3rd party)

##### KMS (Key-Management Service)
* SSE-C (Server-side encryption using customer provided encryption keys)
* Maximum data size supported by KMS is `4` KB. Anything over `4` KB, `AWS Encryption SDK` should be used. 
* 
    
    
