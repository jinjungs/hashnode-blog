---
title: "Design a URL Shortener Like Bit.ly:  Notes on the Unknowns"
seoTitle: "Create Your Own URL Shortener: Key Insights"
seoDescription: "Explore insights and design principles for creating a URL shortener like Bit.ly with focus on unknowns and key system assumptions"
datePublished: Sat Jan 17 2026 06:11:41 GMT+0000 (Coordinated Universal Time)
cuid: cmkhwtesz000002i950focx1m
slug: system-design-url-shortener-like-bitly
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1768630205500/989111a4-b66c-4a4c-a24e-00ea79ac2794.jpeg
tags: system-design, url-shortener, bitly, system-design-interview

---

> This article is not a step-by-step solution.
> 
> It’s a record of the assumptions I had, the gaps I discovered, and the details I didn’t fully understand until I tried to design a URL shortener myself.

## 1\. Solved: 2026-01-15

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1768611254716/6d017bf9-8348-4774-8646-e58ebeae4887.png align="center")

---

## 2\. Reference Solution

[https://www.hellointerview.com/learn/system-design/problem-breakdowns/bitly](https://www.hellointerview.com/learn/system-design/problem-breakdowns/bitly)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1768611234208/5e2a2f15-9435-49f9-942e-8f0da4f257ad.png align="center")

---

## 3\. Known - Unknown

### 3.1. 301 / 302 Redirect

* 301: Permanent Redirect
    
    * Browser caches the redirection result
        
    * Pros
        
        * Reduces load on the server
            
    * Cons
        
        * Hard to change the destination URL once cached
            
* 302: Temporary Redirect
    
    * Browser always sends the request to the server
        
    * Pros
        
        * Easy to change the destination URL
            
        * Easier to collect analytics
            
    * Cons
        
        * Higher load on the server
            

> If we want analytics and flexibility, we usually choose 302 despite the higher server load.

---

### 3.2. Hash Functions

* MD5 / SHA
    
    * Fixed-length output (e.g. MD5 produces 32 hex characters)
        
    * Collisions are possible
        
    * Output length does not fit the requirement that short URLs should be around 6–7 characters
        
    * Deterministic (same input → same output)
        
* Therefore, URL shorteners usually use:
    
    * Global counter + Base62 encoding
        
    * Snowflake-style distributed IDs
        

> Cryptographic hashes are not ideal because they produce long, fixed-length outputs and are not collision-free.

---

### 3.3. User Requests (Traffic Estimation)

* DAU != QPS
    
    * DAU represents the number of users
        
    * QPS represents the number of requests
        
    * We need assumptions to convert DAU into QPS
        
* We usually separate traffic into:
    
    * Redirect traffic (read)
        
    * Shorten traffic (write)
        

Concepts:

* C = average number of clicks per user per day (typically 1–10)
    
* Daily Requests = DAU × C
    
* Average QPS = Daily Requests / 86,400
    
* Peak QPS = Average QPS × Peak factor (usually 3–10)
    

Redirect QPS assumptions:

* DAU = 100M
    
* C = 1
    
* Daily redirects = 100M
    
* Average QPS ≈ 100,000,000 / 86,400 ≈ 1,157 QPS
    
* Peak QPS (×5) ≈ 5,800 QPS
    

> The average QPS is around this range, and considering traffic spikes, I assume the peak QPS to be about five times higher.

---

### 3.4. Database Storage Estimation

* The key question is how many bytes per row
    
* In interviews, a reasonable assumption is more important than an exact number
    

Given assumptions:

* 1B shortened URLs (1 billion total records)
    
* With Base62 encoding:
    
    * 62^5 ≈ 916,132,832
        
    * 62^6 ≈ 56,800,235,584
        
    * So 6 characters are sufficient for 1B URLs
        

Units:

* b = bit
    
* B = Byte
    
* 1 Byte = 8 bits
    
* 1 KB ≈ 1,000 bytes
    
* 1 MB ≈ 1,000,000 bytes
    
* 1 GB ≈ 1,000,000,000 bytes
    

Estimated row size:

* id: 8B
    
* short\_code: approximately 6–10 characters (6–10B)
    
* long\_url: average ~100B (can be up to 200B)
    
* created\_at: 8B
    
* optional expire\_at: 8B
    
* user\_id (if exists): 8B
    
* status / metadata
    
* index and storage engine overhead
    

Conservative estimate:

* ~200B per row
    

Total storage:

* Raw data: 1B × 200B ≈ 200GB
    
* Including indexes, replication, and engine overhead:
    
    * several hundred GB in total
        

> Hundreds of GB is still manageable on a single database, but with 100M DAU, the read QPS and availability requirements make a single primary risky. I would start with aggressive caching and read replicas, and consider sharding once the primary or operational limits become bottlenecks.

---

### 3.5. Why Use a CDN in a URL Shortener?

> Since redirects are read-heavy and the response payload is very small, CDN caching is highly effective.

* Isn’t a CDN usually for images?
    
    * CDNs are commonly used for images and static assets
        
    * Fundamentally, a CDN caches and serves HTTP responses at edge locations worldwide
        

Why CDN works well here:

* Redirect responses are small
    
* Traffic is heavily read-dominated
    
* Serving 301/302 responses from the edge significantly reduces latency
    
* Origin server QPS can be greatly reduced
    

What about Redis:

* Redis is typically an internal cache within the data center or cloud
    
* CDN is an edge cache, closer to users
    

Cache hierarchy:

* L1: CDN
    
* L2: Redis
    
* L3: Database
    

Trade-offs:

* If all redirects are cached at the CDN, analytics events may be missed
    
* Possible approaches:
    
    * Use 302 with a short TTL
        
    * Use edge logging
        
    * Forward a subset of requests to the origin
        

---

### 3.6 Vertical Scaling

* Scaling up a single machine
    
* Increase CPU, memory, or disk capacity
    
* Simple but limited and still a single point of failure
    

---

### 3.7. Horizontal Scaling

(A) Read scaling: Read replicas

* One primary for writes, multiple replicas for reads
    
* Very common for read-heavy systems
    
* Fits URL shortener redirect traffic well
    

(B) Write / data scaling: Sharding

* Split data across multiple database instances
    
* Used when:
    
    * Data volume becomes too large
        
    * Write throughput exceeds a single primary’s capacity
        
* Shard key is usually short\_code or ID
    

---

### 3.8. Memory Access Time

This difference in access speed is significant:

* Memory access time: ~100 nanoseconds (0.0001 ms)
    
* SSD access time: ~0.1 milliseconds
    
* HDD access time: ~10 milliseconds
    

This means memory access is about 1,000 times faster than SSD and 100,000 times faster than HDD. In terms of operations per second:

* Memory: Can support millions of reads per second
    
* SSD: ~100,000 IOPS (Input/Output Operations Per Second)
    
* HDD: ~100-200 IOPS
    

---

## 4\. Unknown - Unknown

### 4.1. Global Counter with Base62 Encoding

* The long URL itself is not encoded
    
* Instead:
    
    1. Store the long URL in the database
        
    2. Convert the row’s integer ID (counter) into a Base62 string
        

What is a counter:

* A monotonically increasing integer
    
* Example:
    
    * row id = 125
        
    * Base62(125) → cb
        

What is Base62:

* Representing numbers in base-62 instead of base-10
    
* Character set:
    
    * 0–9 (10)
        
    * a–z (26)
        
    * A–Z (26)
        
    * Total 62 characters
        

Why no collisions:

* Each row has a unique integer ID
    
* Base62 encoding is a one-to-one transformation
    
* Different IDs always produce different short codes
    

Where the counter is stored:

1. Database auto-increment (simple, good for MVP)
    
2. Redis INCR (fast, but durability must be considered)
    
3. Dedicated ID service (Snowflake, range allocation) for large scale
    

---

## 5\. Deep Dive Questions

### 1) How can we ensure short urls are unique?

* Unique Global Counter with Base62 Encoding
    

### 2) How can we ensure that redirects are fast?

* Implementing an In-Memory Cache (eg., Redis)
    
* Leveraging Content Delivery Network (CDNs) and Edge Computing
    
    * Allow redirect requests to be handled close to the user's location.
        

### 3) How can we scale to support 1B shortened urls and 100M DAU?

* If we store 1B mappings, we're looking at `500 bytes * 1B rows = 500G` of data.
    
* Reads are much more frequent than writes, we can scale our Primary Server by separating the read and write operations.
    
* But what about our counter?
    
    * For our short code generation to remain globally unique, we need a single source of truth for the counter.
        
    * We could solve this by using a centralized Redis instance to store the counter.
        
    * Redis is single-threaded and is very fast for this use case.