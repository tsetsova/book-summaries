# Site Reliability Engineering

## Practices 

> We can characterize the health of a service—in much the same way that Abraham Maslow categorized human needs [Mas43]—from the most basic requirements needed for a system to function as a service at all to the higher levels of function—permitting self-actualization and taking active control of the direction of the service rather than reactively fighting fires.

Below you'll find the *Hierarchy of Production Needs*:

<img src="srle_01.png" alt="Basic requirements of a system based on Maslow's pyramid of needs" width="400"/>

### Monitoring

> Without monitoring, you have no way to tell whether the service is even working; absent a thoughtfully designed monitoring infrastructure, you’re flying blind. Monitoring enables service owners to make rational decisions about the impact of changes to the service, apply the scientific method to incident response, and of course ensure their reason for existence: to measure the service’s alignment with business goals.


>We need monitoring systems that allow us to alert for high-level service objectives, but retain the granularity to inspect individual components as needed.
* white-box monitoring
* black-box monitoring


### Incident Response

### Postmortem and Root-Cause Analysis

> We aim to be alerted on and manually solve only new and exciting problems presented by our service; it’s woefully boring to “fix” the same issue over and over. In fact, this mindset is one of the key differentiators between the SRE philosophy and some more traditional operations-focused environments. 

### Testing

### Capacity Planning

### Development

### Product
