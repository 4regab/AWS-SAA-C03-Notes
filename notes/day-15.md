# Day 15 \- March 22

## CloudFront - ALB or EC2 as an origin using VPC origins
- Allows you to deliver content from your applications hosted in your VPC private subnets (no need to expose them on the internet).
- Deliver traffic to private
  - application load balancer
  - network load balancer
  - ec2 instance
## CloudFront - ALB or EC2 as an origin using public network
- You need to have an EC2 instance that is public.
- You need to have an Application Load Balancer that is public with a security group that allows traffic from CloudFront.
## CloudFront Geo Restriction
- You can restrict who can access your distribution
  - Allowlist: allow your users to access your distribution only if they are on the list of allowed countries
  - Blocklist: block your users from accessing your distribution if they are on the list of blocked countries
- The "country" is determined using third-party geolocation databases.
- Use case: copyright laws, content licensing, regulatory compliance, security
- Only for pay as you go distributions, not for free tier distributions
## CloudFront - Cache Invalidations
- In case you update the backend origin, CloudFront does not know about it and will only get the refreshed content after the TTL expires.
- However, you can force an entire or partial cache refresh, thus bypassing the TTL, by performing a CloudFront invalidation.
- You can invalidate all files (`*`) or specific files (e.g., `/images/*`).
- Use case: you updated the backend origin and want to make sure that your users get the updated content immediately, you want to remove sensitive data from the cache, or you want to fix a bug in the cached content.
## AWS Global Accelerator
- Leverages the AWS internal network to route traffic to your application.
- Two anycast IPs are created for your application.
- The anycast IPs send traffic directly to edge locations.
- The edge locations send the traffic to your application.
- Works with Elastic IPs, EC2 instances, Application Load Balancers, and Network Load Balancers.
- Consistent performance: intelligent routing to the lowest latency and fast regional failover.
- Health checks: performs health checks of your applications.
- Security: only two external IPs need to be whitelisted, and DDoS protection is provided by AWS Shield.
## AWS Global Accelerator vs CloudFront
- They both use the AWS global network to route traffic to your application, but they have different use cases and features.
- Both services integrate with AWS Shield for DDoS protection.
- CloudFront improves performance for both cacheable content, such as images and videos, and dynamic content, such as API acceleration and dynamic site delivery. Content is served at the edge.
- Global Accelerator improves performance for a wide range of applications over TCP or UDP by proxying packets at the edge to applications running in one or more AWS Regions. It is a good fit for non-HTTPS applications, gaming, media streaming, VoIP, HTTP use cases that require static IP addresses, and deterministic fast regional failover.
