# [Content Delivery Network](https://github.com/donnemartin/system-design-primer/blob/master/README.md#content-delivery-network:~:text=DNS%20articles-,Content%20delivery%20network,-Source%3A%20Why%20use)
- Push CDN works well for controlled, predictable content delivery where manual updates are feasible, and storage costs are not a primary concern.
- Pull CDN is more adaptable for dynamic, high-traffic websites where content changes frequently and performance optimization is crucial.
- Content might be stale if it is updated before the TTL expires it.
- CDNs require changing URLs for static content to point to the CDN.

