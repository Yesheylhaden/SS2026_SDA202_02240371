# SS2026_SDA202_02240371
# Chapter 8: Design a URL Shortener — Critical Analysis

## 1. Analysis of the problem statement:

The major aim of this is to design a URL shortening service (like TinyURL) which will have two major tasks:

- **URL Shortening**  
  It converts a long URL into a short URL for easy usage.

- **URL Redirection**  
  URL Redirection The short URL will point back to the original, longer URL.

### Example:
- Long URL: `https://github.com/Yesheylhaden/SS2026_SDA202_02240371`
- Short URL: `https://tinyurl.com/y7keocwj`

### Key Requirements of URL Shortener

- **Handles High Traffic:** They should be able to handle at least ~100 million URL requests per day.  
- **High Availability & Scalability:** The server should be always available and be able to add more servers to handle more traffic.  
- **Fast Redirection:** The user should be able to reach the original long URL instantly.  
- **Short & Unique URLs:** The shortened URL must be short and easy to share so that it can be remembered. Each of those short URL must maps to one long URL.  
- **Long-term Storage:** The system should be able to store billions of URLs for over time (~365B in 10 years).

The problem goes beyond just shortening URLs; it requires designing a large, distributed system..

---

## 2. Analysis of the Author’s Solution

### (A) API Design

The author proposed two RESTful APIs. They are:

1. **POST /api/v1/data/shorten**
   - This API will generate an output of short URL though an input from long URL.
   ### Example:
   - Input: `https://github.com/Yesheylhaden/SS2026_SDA202_02240371` (Long URL)
   - Output: `https://tinyurl.com/y7keocwj` (Short URL)

2. **GET /api/v1/shortUrl**
   - This API will redirect the users from the short URL to the original long URL.
   ### Example:
   - Input: `https://tinyurl.com/y7keocwj` (Short URL)
   - Output: `https://github.com/Yesheylhaden/SS2026_SDA202_02240371` (Long URL)
   
---

### (B) URL Redirection

- The system should store mappings whereby: `<shortURL, longURL>`
- The **cache** is mainly used for fast access.
- The **database** should store mappings permanently.

#### Two Redirect Types are:
- **301 Redirect (Permanent)**
  - The browser will Cached it and then reduce the server load but harder to track its analysis

- **302 Redirect (Temporary)**
  - Requests will always go through the server for better analysis but it will be slow for repeated requests.

---

### 🔹 (C) Database Design

#### Basic table structure:

| id | shortURL | longURL |
|----|----------|--------|

Store mapping in a table:
(id, shortURL, longURL)
id: unique identifier
shortURL: shortened link
longURL: original link

#### Scaling:
- **Replication:** It is the coping of database to multiple servers mainly to improve its performance & reliability
- **Sharding:** It is the spliting of data across servers where it will handle a large data (billions of URLs).

---

### (D) Hashing technique:

#### 1. Hash + Collision Handling
- It uses hash functions like MD5 & SHA-1.
- They take the first 7 characters and handles them manually.

#### Issues:
- They can create a collisions. (same short URL used for different links)
- Needs extra databas checkups.

---

#### 2. Base62 Encoding (Preferred ✅)
- It Converts numeric ID into short strings
- Uses characters like:
  - `0–9`, `a–z`, `A–Z` (62 total)

#### Advantages:
- There is no collisions as all the input is unique.
- They are simple and predictable.
- They work well for large systems.
- They are suitable for large scale applications.
- They are case sensitive hence it provides more uniqueness.
- They doesnot contain any characters.

---

### (E) URL Shortening Flow

1. When a user gives a long URL
2. They check if it already exists in the database
3. If it already exists then it will return the same existing short URL
4. If it doesn't exists:
   - They will generate a unique ID
   - And then convert ID into Base62
   - After that they will store in database

This method is efficient and avoids duplications so that every short URL has unique links.

---

### (F) URL Redirecting Flow

1. When user clicks the short URL
2. Then the system will check the cache
3. If it is not found then they will check database
4. Then it will redirect the user to the original long URL

#### Cache is important because:
- The read operations are much higher than writes (10:1 ratio)
- They have faster response times and reduces load on database

---

### (G) Scalability Considerations

- The Load balancer will distribute the traffic loads of a system
- Stateless web servers are easy to add/remove the servers for scalling
- Database scaling uses sharding and replication of data
- The Rate limited for security
- Analytics for tracking usage

Therefore this will make the system more scalable, reliable, and ready to use in real-world.

---

## 3. My Review of URL Shortener Design:

### 🧠 My Understanding

- In this chapter, I have learned what a URL shortener would look like as a large-scale system. The idea is to take a long URL and make it short with a unique URL, then redirect users to the original long URL when they click on the short URL.  

The system would consist of:
- A **database** to keep track of short-long URL pairs  
- A **cache** to optimize frequent requests that users would make  
- A **unique ID generator** and **Base62** to make short URLs  
- **Load balancing and scaling techniques** to accommodate a large number of users  

I also learned that read operations are way more frequent than write operations. This means that optimization is critical in this case.

### My Confusions

Although I had a general idea of how the design works, I still had some confusions:

- What is the real-world implementation of a **distributed unique ID generator**?  
- What is the step-by-step process of **Base62 conversion**?  
- What is the use of **Bloom filters**, and how to apply them?  
- What is the implementation of **database sharding** in large-scale systems?  

---

### 🔍 Topics I Want to Explore Further

Now to expand my current understandings of these concepts, I would like to learn more about:

- The implementations of the distributed unique ID generators (Snowflake)  
- The caching techniques (Redis, cache eviction algorithms)  
- The techniques of scaling the databases (sharding and replication)  
- The techniques of load balancing  
- The techniques of rate limiting for security purposes  
- The system design techniques (CAP theorem)

## 4. Brief Summary

In this chapter, I have now learned how to create a highly scalable URL shortener system with efficient hashing, database, and caching. The Base62 encoding strategy, together with the unique ID generator, ensures that there are no collisions at all. This strategy is highly scalable. Even though this strategy is great and applicable, some of the advanced features, such as distributed ID generation and Bloom filters, are highly understood.

---