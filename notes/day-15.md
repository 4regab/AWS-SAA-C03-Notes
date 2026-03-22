# Day 15 \- March 22

## Cloudfront - ALB or EC2 as an origin using vpc origins
- allows oyu to deliver content from your applications hosted in your vpc private subnets (no need to expose them on the internet)
- Deliver traffic to private
  - application load balancer
  - network load balancer
  - ec2 instance
## Cloudfront - ALB or EC2 as an origin using public network
- you need to have ec2 instance that is public
- you need to have an application load balancer that is public with security group that allows traffic from cloudfront
## Cloudfront Geo Restriction
- You can restrict who can access your distribution
  -  Allowlist: allow your uers to access your distribution only if they are on the list of allowed countries
  - Blocklist: block your users from accessing your distribution if they are on the list of blocked countries
- The "country" is determined using 3rd party geolocation databases
- Use case: copyright laws, content licensing, regulatory compliance, security
- Only for pay as you go distributions, not for free tier distributions
## Cloudfront - Cache Invalidations
- in case you updae the backend origin, Cloudfront doesnt know about it and will only get the refreshed content after the TTL expires
- however, you can force an entire or partial cache refresh (thus bypassing the TTL) by performing a Cloudfront Invalidation
- You can invalidate all files (*) or specific files (e.g., /images/*)
- Usecase: you updated the backend origin and want to make sure that your users get the updated content immediately, you want to remove sensitive data from the cache, you want to fix a bug in the cached content
## AWS global accelerator
- leverages the AWS internal network to route to your application
- 2 any cast IP are created for your application
- the anycast IP send traffic directly to EDGE locations
- the end locations send the traffic to your application
- works with Elastic IP, ec2 instance, application load balancer, network load balancer, and elastic IP
- Consistent performance: intelligent routing to lowest latency and fast regional failover
- Health checks: performs health check of your applications
- Securty: only 2 external IP need to be whitelisted, ddos protection thanks to aws shield
## AWS global accelerator vs cloudfront
- They both use the AWS global network to route traffic to your application, but they have different use cases and features
- both services integrate AWS shield for DDOS protection
- Cloudfront: improves performance for both cacheable content (such as images and videos), dynamic content (such as API acceleration and dynamic site delivery) content is served at the edge
- Global accelerator: improves performance for wide range of applications over TCP or UDP, proxiying packet s at the edge to applications running in one or more AWS regions. good fit for non-https applications, gaming, media streaming, voice over IP (VoIP), good for http use cases that require static ip addresses, required determiministic, fast regional failover