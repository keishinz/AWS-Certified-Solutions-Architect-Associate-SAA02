# Global Accelerator

Created By: Keishin CHOU
Last Edited: Apr 22, 2020 11:44 AM

### Overview

- Work with Elastic IP, EC2 instance, ALB, NLB, public or private.
- Leverage the AWS internal network to route to your application.
- 2 Anycast IP address will be created for your application.
- The Anycast IP send traffic directly to Edge Location.
- The Edge Location sends the traffic to your application.
- Consistent performance
    - Intelligent routing to lowest latency and fast regional failover.
    - No issue with client cache. The IP doesn't change.
    - Internal AWS network.
- Health check
    - Disaster recovery.

### Global Accelerator vs CloudFront

- Both use AWS global network and Edge Locations.
- CloudFront
    - Improves performance for both cached content and dynamic content.
    - Contents are served at the Edge. So user get content from the Edge.
- Global Accelerator
    - Improves performance for a wide range of applications over TCP or UDP.
    - All the requests go to origin application end.
    - No cached contents available.