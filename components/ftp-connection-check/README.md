Creates a Route 53 health check that indicates if an FTP server is properly handling basic FTP connections.

Each stack deployed using the included CloudFormation template tests a single FTP server.

It's expected that, for any given production FTP server, several of these stacks will be deployed, testing connectivity to the server from multiple geographic regions. For example, if an FTP server is running in us-east-1, stacks may be deployed in us-east-2, us-west-2, and ca-central-1, with each one targetting that server running in us-east-1.

The status of the Route 53 health check matches the state of a CloudWatch alarm that is also created in the stack. If the Lambda function that is actually running the connection tests fails, the CloudWatch alarm will move into an ALARM state, which will cause the health check to move into an UNHEALTHY state.

On their own, these health checks don't have any impact on other Route 53 resources, like DNS records. Other Route 53 health checks should be created that list these health checks as _child health checks_.

### Deployment

Deployment is expected to be handled by a continuous deployment GitHub Action (see deploy.yml), which uses a matrix strategy to deploy the correct combination of check- and target-regions.
