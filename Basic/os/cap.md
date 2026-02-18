# CAP

<table><thead><tr><th width="173.47845458984375">CAP</th><th>concept</th></tr></thead><tbody><tr><td>Consistency</td><td>all clients have the same, most recent data view</td></tr><tr><td>Availability</td><td>all clients can read and write, regardless of data recency</td></tr><tr><td>Partition Tolerance</td><td>the system’s guarantees hold even in the face of network faults between each distributed system node</td></tr></tbody></table>

When a network partition failure happens should we decide to?

* CP -> 限流、熔断：Cancel the operation and thus decrease the availability but ensure consistency
* AP -> 缓存、降级：Proceed with the operation and thus provide availability but risk inconsistency
