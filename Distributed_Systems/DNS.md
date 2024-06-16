# Domain name system
A Domain Name System (DNS) translates a domain name such as www.example.com to an IP address.
## An interesting thing
Given it can decide how to route traffic to different servers, it can act as a simple load balancer. [here](https://github.com/donnemartin/system-design-primer/blob/master/README.md#anki-flashcards:~:text=Some%20DNS%20services%20can%20route%20traffic%20through%20various%20methods%3A)
### Some routing (load balancing) methods
- [(Weighted) Round Robin](https://www.jscape.com/blog/load-balancing-algorithms)
- [Latency-based, Geolocation-based](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html#routing-policy-latency)
