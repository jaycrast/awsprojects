- Route 53 Hosted Zones
    - hosted zone is a dns DB for a domain
    - globally resilient
    - created with domain registration via R53
    - host dns records
    - splitview zones allow you to have one hosted zone, split in to both private and public

- CNAME vs ALIAS
    - A maps name to an IP
    - CNAME maps name to another name
        - can't use cname to map apex (naked) domain to another name
    - Alias records let you map the apex domain name to an AWS resource
        - no charge for alias records

- R53 Simple Routing
    - each record can have multiple values (multiple web servers attached to one A record)
    - doesn't support health checks, all values are provided when requested

- R53 Health Checks
    - health checks are seperate from but are used by records
    - health checkers located globally
    - not just used for AWS services, can be used on anything in the public internet
    - health checkers check every 30s, or every 10s for extra cost
    - TCP, HTTP/s, or HTTP/HTTPs with string matching

- R53 Weighted Routing
    - you're able to provide a weight to the multi value routing
        - record weight of 40 out of 100, the record is returned 40% of the time
    - used as a simple load balancer or testing new software version

- R53 Latency Based Routing
    - used when trying to optimize performance and user experience
    - instead of providing a number value for weighted routing, you provide a region
        - the A record is returned to the user based off the best possible latency

- R53 Geolocation Routing
    - specify a country, continent or default
    - IP check verifies the location of the user, and provides relevant records
        - if you want to serve specific content to a specific location
        - if there is no default location tagged, and there's no geolocation that matches the requester, no answer is returned
        - restrict content to target countries 

- R53 Interoperability
    - 