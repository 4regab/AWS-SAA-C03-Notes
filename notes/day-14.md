# Day 14 \- March 21

## CloudFront
- Content delivery network (CDN) service
- Improves read performance by caching content at edge locations around the world
- Improves user experience
- Hundreds of points of presence (edge location caches) around the world
- DDoS protection, plus integration with Shield and AWS Web Application Firewall
## CloudFront Origins
- S3 bucket
 - For distributing files and caching them at edge locations
 - For uploading files to S3 through CloudFront
 - Secured using Origin Access Control (OAC)
- VPC origin
  - For applications hosted in VPC private subnets
  - Private Application Load Balancer, Network Load Balancer, or EC2 instance
- Custom origin (HTTP)
    - S3 website (must first enable the bucket as a static S3 website)
    - Any public HTTP backend you want (for example, a public ALB)
## CloudFront vs. S3 Cross-Region Replication
- CloudFront: global edge network, files are cached for a TTL, great for static content that must be available everywhere
- S3 Cross-Region Replication: must be set up for each Region you want, files are updated in near real time, read-only, great for dynamic content that needs to be available at low latency in a few Regions
