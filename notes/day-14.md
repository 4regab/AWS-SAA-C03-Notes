# Day 14 \- March 21

## Cloudfront
- Content delivery network (CDN) service
- improves read performance by caching content at edge locations around the world
- improves user experience
- hundreds of points of presence (edge locations caches) around the world
- ddos protection (because worldwide)integration with shield, AWS Web application firewall 
## cloudfront origins 
- s3 bucket
 - for distributing files and chaching them at the edge locations
 - for uploading files to S3 through cloudfront
 - secured using origin access control (OAC)
- vpc origin
  - for applications hosted in VPC private subnets
  - private application load balancer/network load balancer/ec2 instance
- Custom Origin (http)
    - s3 website (must first enable the bucket as a static s3 website)
    - any public http backend you want (example:Public ALB)
## Cloudfront vs s3 cross region replication
- cloudfront: global edge network, files are cached for A TTL, great for static content that must be available everywhere
- s3 cross region replication: must be setup for each region you want, files are updated in near realtime, read only, great for dynamic conteent that needs to be available at low latency in few regions
