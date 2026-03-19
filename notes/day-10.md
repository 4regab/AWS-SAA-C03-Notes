# Day 10 \- March 17

**Elastic Beanstalk** 

- Elastic Beanstalk is a developer-centric view of deploying an application on AWS  
- It uses all the components we've seen before: EC2, ASG, ELB, RDS, etc.  
- Managed service:  
  - Automatically handles capacity provisioning, load balancing, scaling, application health monitoring, and instance configuration  
  - Only the application code is the responsibility of the developer  
- We still have full control over configuration  
- Beanstalk is free, but you still pay for instances and other AWS services  
- Supports a wide range of languages

**Elastic Beanstalk - Components** 

- Application  
- Application version  
- Environment

**Elastic Beanstalk - Deployment Modes**

- Single instance: great for dev  
- High availability with load balancer: great for prod

**CloudFormation**

- Used to deploy stacks arbitrarily with any kind of infrastructure  
- Defines and provisions a wide range of AWS infrastructure resources using declarative templates (YAML or JSON).
