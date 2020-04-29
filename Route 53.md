# Route 53

Created By: Keishin CHOU
Last Edited: Apr 20, 2020 4:18 PM

### Overview

- Route 53 is a managed DNS(Domain Name System)
- DNS is a collection of rules and records which helps clients understand how to reach a server through URLs.
- How Route 53 works

![Route%2053/Untitled.png](Route%2053/Untitled.png)

- Common records in AWS
    - A: URL to IPv4
    - AAAA: URL to IPv6
    - CNAME: URL to another URL
        - **Only for non root domains**
        - [app.mydomain.com](http://app.mydomain.com) ⇒ something.anything.com
    - Alias: URL to AWS resource
        - Works for both root domain and non root domain
        - [app.mydomain.com](http://app.mydomain.com) ⇒ something.amazonaws.com
        - Free of charge
        - Native health check

### Routing policy

- Simple
    - A random direction will be chosen by the **Client**.
    - Usually used when there is only one single direction(resource).
    - Cannot be associated with health check.
- Weighted
    - Control the % of the requests that go to specific endpoint.
    - Can be associated with health check.
- Latency
    - Redirect to the server that has the least latency close to the user.
    - Can be associated with health check.
- Failover
    - Use when you want to configure active-passive failover.
    - One primary and one for secondary.
    - The primary record must be associated with health check. The secondary one do not need to.
- Geo location
    - Routing based on user location.
    - Should create a default policy in case there is no match on location.
- Multi value
    - Use when routing traffic to multiple resources.
    - Want to associate a Route 53 health check with records.
    - Up to 8 healthy records are returned for each multi value query.
    - Multi Value is not substitute for having an ELB.