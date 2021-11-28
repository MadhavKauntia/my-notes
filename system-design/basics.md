# System Design Basics

## Horizontal and Vertical Scaling

| Horizontal Scaling                    | Vertical Scaling        |
| ------------------------------------- | ----------------------- |
| More machines                         | Bigger machine          |
| Load balanding required               | No load balancing       |
| Resilient                             | Single point of failure |
| Network calls between systems is slow | Hardware limit          |

## Consistent Hashing

Load balancers utilise _hashing_. Each request that comes into a server has a request ID. This ID can be hashed to return a number, using which we can decide the server to which this request must be sent.

```
hash(r) modulo (number of servers) = Server to which request is sent
```

This method is simple and intuitive and works fine, until the number of servers changes. If the number of servers changes, then keys need to be redistributed to account for the missing server. the problem in this case is that `hash modulo N` will change, so most keys will be moved to a different server.

_Consistent Hashing_ is a distributed hashing scheme that operated independently of the number of servers in a distributed hash table by assigning them a position on a _hash ring_. Requests are also hashed and mapped on the _hash ring_. Each request key will then belong to the server whose key is closest, in a counterclockwise direction.

![Consistent Hashing](https://uploads.toptal.io/blog/image/129308/toptal-blog-image-1551794723809-3b70e8ff86f1510ad4c24a09bcd3942c.png)

Even in this method, the distribution of requests can be skewed, as it would depend on the distance between the servers. This problem can be solved by creating virtual servers. Suppose k=3, then each server will have a total of 3 allocations on the hash ring, thus reducing the chances of skewed distribution.

## Monolith and Microservices

### Monolith

Pros:

- Good for small cohesive teams
- Fewer moving parts
- Less duplication
- Faster, since calls are within the app and not over network

Cons:

- Onboarding is harder
- Deployments are complicated. Even a small update requires deployment of entire app
- Single point of failure

### Microservices

Pros:

- Easier to scale
- Easier for new developers
- Easier to work in parallel
- Easier to scale servers. You can see which services require scaling.

Cons:

- Harder to design

Consider this [blog post](https://m.signalvnoise.com/the-majestic-monolith/) while deciding.

## Database Sharding

Sharding is a database architecture pattern related to horizontal partitioning — the practice of separating one table’s rows into multiple different tables, known as partitions. Each partition has the same schema and columns, but also entirely different rows. Likewise, the data held in each is unique and independent of the data held in other partitions.

![Partitioning](https://assets.digitalocean.com/articles/understanding_sharding/DB_image_1_cropped.png)

Sharding involves breaking up one’s data into two or more smaller chunks, called _logical shards_. The logical shards are then distributed across separate database nodes, referred to as _physical shards_, which can hold multiple logical shards. Despite this, the data held within all the shards collectively represent an entire logical dataset.
