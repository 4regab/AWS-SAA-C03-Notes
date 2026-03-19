# Day 2 \- March 9

**EC2 instance types**

* **General purpose** \- balanced for diverse general workloads  
* **Compute optimized** \- extensive compute  
* **Memory optimized** \- high-performance memory  
* **Storage optimized** \- high sequential read/write

**EC2 instance purchasing options** 

- **On-Demand** \- pay for what you use  
- **Reserved Instances** \- up to 72% discount. Specific attributes, reservation period 1 year/3 years, no upfront, partial upfront, or all upfront, for steady-state usage. You can convert Reserved Instances.  
- **Savings Plans** \- up to 72% discount. Commit to a certain type of usage, e.g. $10/hr for 1 or 3 years. Locked to a specific instance family and instance Region, flexible across instance sizes.  
- **Spot Instances** \- up to 90% discount. Instances that you can lose at any point if your max price is less than the current spot price. Most cost-efficient, best for workloads that are resilient to failure, 2-minute grace period. Cancel the Spot request before terminating. You must first cancel the Spot request before terminating the instances.  
- **Dedicated Hosts** \- a physical server fully dedicated to your use case, addresses compliance requirements, use your existing server-bound software licenses, purchasing options: On-Demand and Reserved, most expensive option, bring your own license.  
- **Dedicated Instances** \- instances that run on hardware dedicated to you, may share with other instances in the same account, no control over instance placement.  
- **Capacity Reservations** \- reserved instances in a specific AZ for any duration, always accessible, no time commitment, no discounts, perfect for short-term uninterrupted workloads that need to be in a specific AZ.  
- **Spot Fleets** \- set of Spot Instances + optional On-Demand Instances, meet target capacity with price constraints, lets you define instance type, OS, AZ. Multiple launch pools the fleet can pick from: lowestPrice, diversified, capacityOptimized, priceCapacityOptimized (best choice), allows us to automatically request Spot Instances with the lowest price.
