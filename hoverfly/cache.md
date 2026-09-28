<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Cache](#cache)
  - [Caching matches](#caching-matches)
  - [Headers](#headers)
  - [Eager caching](#eager-caching)
  - [Cache invalidation](#cache-invalidation)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Cache

- In simulate mode, hoverfly use cache
- Cache is key value
    - key is the request hash
        - hash of all request excluding headers
    - value is the responses

### Caching matches

- In simulate mode, receive the request
- Convert request into hash
- Check cache, if found reply with cache response
- If not found on cache, try to find a match on list of matchers and add later to cache

### Headers

- Headers tends to change over clients, thats why is not included on the cache key

### Eager caching

- Cache automatically pre-populated when switching to simulate mode
- only for matchers where every field is `exactMatch`

### Cache invalidation

- Happens when simulation is modified