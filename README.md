To serve a simple and static website, use cloudfront and S3 serving as origin is the easiest way, but since this solution doesn't meet the requirements, so here is my solution.

cloudfront.yaml:
Provision cloud front stack which redirects http requests to https, and provison an aws managed certificate for SSL connection.
The backend application load balancer is serving as the origin, which accepts http requests.
Cloud front can cache the web contents on edge location, so that it can speed up the response to users, and reduce the backend system's load. From security point of view, it enables https easily. And can improve security by adding WAF working together with cloud front.

targetgroup.yaml:
Provision application load balancer.

webapp-ASG.yaml:
Provision auto scaling group which scales out or in EC2 instances number based on average CPU usage, and start up new instance to replace the faulty one when necessary. It provides scalability and reliability of the system.
This stack enable the administrator to specify the environment size as well, small size provisons t2.micro instances, medium size provisions t2.small instances, while large size provisions t2.medium instances.
The launch template uses ubuntu 18.04 image, and install nginx once the instances up, then modify the nginx default page a little bit to show the "Hello World" message.

SG.yaml:
Provision security group, which allows 80 and 443 inbound traffic.

webappvpc.yaml:
Provision VPC, subnets, route tables, internet gateway and NAT gateway accordingly.
This template takes reference from a tutorial session provided by Adrian Cantrill in Linux Academy.


Prerequisites:
1. You are in ap-southeast-2 region, otherwise, the template will throw errors as it cannot find any subnet meet requirements.
2. There is IAM role available with sufficient privileges for cloud formation to create all the resources.
