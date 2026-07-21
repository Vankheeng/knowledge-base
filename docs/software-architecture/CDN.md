# Content Delivery Network (CDN)

## What is a CDN?

A **Content Delivery Network (CDN)** is a globally distributed network of **edge servers** that cache content closer to users.

Instead of every request going to the origin server, users are served by the nearest CDN edge server whenever possible.

Benefits:

- Faster page loading
- Lower latency
- Reduced origin server load
- Better scalability
- Improved availability

---

# Without CDN

```text
             User (Japan)
                  |
                  |
           Long Distance
                  |
                  |
          Origin Server (US)
```

Problems:

- High latency
- Slow loading
- Heavy load on origin server

---

# With CDN

```text
                User (Japan)
                     |
             Nearby Edge Server
                     |
             (Cached Content)
                     |
      Only if cache miss/stale
                     |
              Origin Server (US)
```

The user gets content from the closest edge server.

---

# How CDN Works

```text
Client

↓

CDN Edge Server

↓

Is content cached?

↓

YES -----------------> Return cached content

NO

↓

Request origin server

↓

Cache response

↓

Return content to client
```

---

# Cache Hit vs Cache Miss

## Cache Hit

```text
Client

↓

CDN

↓

Image already cached ✅

↓

Return immediately
```

Fast response.

---

## Cache Miss

```text
Client

↓

CDN

↓

Image not found

↓

Origin Server

↓

Return image

↓

Store in CDN

↓

Return to client
```

The first request is slower, but later requests are faster.

---

# Types of CDN

## 1. Pull CDN

The CDN automatically fetches content from the origin server when needed.

Example:

```text
Client

↓

CDN

↓

Image not cached

↓

Origin Server

↓

Return image

↓

Store in CDN
```

### TTL (Time To Live)

The CDN keeps cached content for a configured time.

Example:

TTL = 1 hour

```text
12:00

↓

Image cached

↓

1:00 PM

↓

Cache expires
```

**Important:** After the TTL expires, the CDN typically **does not immediately contact the origin**. Instead, the **next client request** triggers the CDN to fetch a fresh copy from the origin.

### Advantages

- Easy to set up
- Minimal maintenance
- Automatically caches requested content

### Disadvantages

- First request after expiration is slower
- Users may temporarily receive stale content depending on cache settings

---

## 2. Push CDN

The origin server uploads content to the CDN whenever content changes.

Example:

```text
Origin Server

↓

Upload image

↓

CDN

↓

Edge Servers
```

Client requests:

```text
Client

↓

CDN

↓

Return cached image
```

The CDN does not need to fetch from the origin because the content was already pushed.

### Advantages

- Content is immediately available on the CDN
- Better control over cached content
- Good for predictable static assets

### Disadvantages

- More complex deployment
- Need synchronization/upload logic
- More storage management

---

# Static vs Dynamic Content

## Static Content

Best suited for CDNs.

Examples:

- Images
- CSS
- JavaScript
- Videos
- PDFs
- Fonts

---

## Dynamic Content

Usually comes from the origin server.

Examples:

- Login
- Bank transfer
- Shopping cart
- User profile

Some modern CDNs can accelerate dynamic content, but they generally don't cache personalized responses by default.

---

# Popular CDN Providers

- Cloudflare
- Amazon CloudFront
- Akamai
- Fastly
- Google Cloud CDN
- Azure CDN

---

# Advantages of CDN

- Faster page loading
- Lower latency
- Reduced bandwidth usage
- Reduced origin server load
- Better scalability
- Improved availability
- Protection against traffic spikes
- Can help mitigate DDoS attacks (depending on the provider)

---

# Considerations

- Dynamic data is usually not cached.
- Choose an appropriate TTL.
- Update or invalidate the cache when content changes.
- CDN is not a replacement for your origin server.

---

# Interview Questions

## Q1. What is a CDN?

**Answer**

A CDN is a distributed network of edge servers that caches content closer to users to reduce latency and improve performance.

---

## Q2. Why do we use a CDN?

**Answer**

To reduce latency, improve page loading speed, reduce origin server load, increase scalability, and improve availability.

---

## Q3. What is the difference between Pull CDN and Push CDN?

| Pull CDN | Push CDN |
|-----------|-----------|
| CDN fetches content when needed | Origin uploads content to CDN |
| Easy to configure | More complex setup |
| First request may be slower | Content is immediately available |
| Good for most websites | Good for controlled static assets |

---

## Q4. What is TTL?

**Answer**

TTL (Time To Live) defines how long the CDN keeps cached content before considering it stale and needing a fresh copy from the origin.

---

## Q5. What is a Cache Hit?

**Answer**

The requested content already exists in the CDN cache, so it can be returned immediately without contacting the origin server.

---

## Q6. What is a Cache Miss?

**Answer**

The content is not in the CDN cache (or the cache is stale), so the CDN retrieves it from the origin server, stores it, and then returns it to the client.

---

## Q7. Can a CDN cache API responses?

**Answer**

Yes, if the API responses are cacheable (for example, public product catalogs or news articles). Personalized or sensitive responses are usually not cached.