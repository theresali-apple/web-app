To serve a simple and static website, use cloudfront and S3 serving as origin is the easiest way, but since this solution doesn't meet the requirements, so here is my solution.

**Architecture** as show in Architecture.pdf.

# How to deploy:
Please refer to the doc **Instruction.md** which with step by step screenshots.

Before testing, you can lower the CPU threshold to 20% so that it is more sensitive to the stress testing.
And adding port 22 to security group inbound rule, so that you can login to the instance.

# Test scalability:
Install stress on the instance: sudo apt install stress,
then run: sudo stress --cpu 30 --timeout 3600

You will soon receive cloudwatch alarm regarding CPU usage, then the instance number increased by the auto scaling, to maintain average CPU load to 20%.
Once you stop running the stress, and the cloudwatch alarm cleared, the instance number will be back to 1. But this is not as sensitive as while scaling out, sometimes scaling in only happens 20 mins later when the average CPU usage is lower than 20% by observation.

Test result is attached to another doc testresult.pdf with screenshots.

All the following templates are in templates folder.

# main.yaml:
main stack which calls all nested stacks and defines dependency.

# rolepolicy.json:
Define the role cloudformation is going to use to provision this stack.

# cloudfront.yaml:
Provision cloud front stack which redirects http requests to https, and provison an aws managed certificate for SSL connection, this can reduce admin overhead to renew SSL certificate, and improve security.

The backend application load balancer is serving as the origin, which accepts http requests.

Cloud front can cache the web contents on edge location, so that it can speed up the response time to users, and reduce the backend system's load. From security point of view, it enables https easily.
You can use it to enable geo restriction.
And can improve security by adding WAF working together with cloud front.

You can also add Lambda working together with cloud front to serve customised content.

# targetgroup.yaml:
Provision application load balancer.
The ALB is listening on port 80, you can specify port 443 to improve security, so that the communication channel between cloudfront and the ALB is encrypted as well. And the certificate can be aws managed again, and use r53 to be the domain registrar, so that aws can manage all SSL certificates for you.

ALB is used here, it can help to offload the heavy lifting of encrypting and decrypting network traffic from the backend web server when necessary.
However, if end to end encryption is a requirement then need to use NLB.

You can turn on load balancer access log for audit.

# webapp-ASG.yaml:
Provision auto scaling group which scales out or in EC2 instances number based on average CPU usage, and start up new instance to replace the faulty one when necessary. It provides scalability and reliability of the system.

This stack enable the administrator to specify the environment size as well, small size provisons t2.micro instances, medium size provisions t2.small instances, while large size provisions t2.medium instances.

The launch template uses ubuntu 18.04 image, and install nginx once the instances up, then modify the nginx default page a little bit to show the "Hello World" message.

# SG.yaml:
Provision security group, which allows 80 and 443 inbound traffic.

# webappvpc.yaml:
Provision VPC, 2 public subnets, 2 private subnets across 2 AZs, route tables, internet gateway and NAT gateway accordingly.
This template takes reference from a aws tutorial session provided by Adrian Cantrill in Linux Academy.



