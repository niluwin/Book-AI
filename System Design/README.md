# System Design

Video resource: YouTube – **“System Design Explained: APIs, Databases, Caching, CDNs, Load Balancing & Production Infra”** by **Hayk Simonyan**\
Link: [https://www.youtube.com/watch?v=adOkTjIIDnk](https://www.youtube.com/watch?v=adOkTjIIDnk)\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​

***

### 1. Role of System Design (0:00–2:27) <a href="#id-1-role-of-system-design-000227" id="id-1-role-of-system-design-000227"></a>

* Most developers can extend existing architectures, but they often freeze when asked to design systems from scratch with only rough requirements; being able to make design trade-offs is what differentiates mid-level from senior engineers.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​
* Companies pay senior engineers for architectural decisions that affect performance, data storage, and customer experience, so system design skills are key both for real work and interviews.\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
* The video’s goal is to teach the core system design concepts the author used to pass system design interviews and reach senior roles early in his career.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​

***

### 2. Single-Server Architecture & Request Flow (2:28–6:34) <a href="#id-2-single-server-architecture--request-flow-228634" id="id-2-single-server-architecture--request-flow-228634"></a>

* Starting point: a **single server setup** for a small user base where the web application, API endpoints, database, cache, and other components all run on one machine.\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
* Users on web or mobile access the system using a domain like `app.demo.com`, not the raw IP address.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​

Request flow in this baseline design:\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​

* The browser or mobile app sends the domain to a **DNS (Domain Name System)** provider.
* DNS maps the domain to the server’s IP address and returns that IP to the client.
* With the IP in hand, the client sends an HTTP request to the server.
* The server processes the request and sends back:
  * An HTML page for web clients.
  * A JSON response for mobile clients.

Responsibilities in this single server:\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​

* For web users: handle business logic, data storage, and presentation (HTML/CSS/JS).
* For mobile users: expose API endpoints that return JSON payloads (e.g., `GET /product/{id}` returning product details).

This setup is ideal to learn core components and flows but will struggle under heavy traffic and has no redundancy.\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​

***

### 3. Relational (SQL) Databases & ACID (6:35–12:54) <a href="#id-3-relational-sql-databases--acid-6351254" id="id-3-relational-sql-databases--acid-6351254"></a>

### 3.1 Data Model and Tables

* Relational databases (RDBMS) include PostgreSQL, MySQL, Oracle, SQLite.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​
* Data is stored in **tables** similar to spreadsheets, where:\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
  * Columns represent fields/attributes (e.g., `id`, `name`, `age`, `email`).
  * Rows represent individual records (e.g., each customer).

Example: a `customers` table with rows such as `(id=1, name=John, age=30, email=...)`.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​

### 3.2 Joins and Relationships

* SQL supports complex **join** operations that combine data from multiple tables.\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
* Example: with `customers` and `products`, an `orders` table can connect customers to purchased products using foreign keys.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​
* Joins make it easy to answer questions like “which customer bought which product” by combining tables logically.\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​

### 3.3 Transactions and ACID

* A **transaction** is a sequence of one or more SQL operations executed as a single, atomic unit.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​
* Classic example: bank transfer, where money must be deducted from one account and added to another, both or none.\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​

ACID properties:\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​

* Atomicity: the entire transaction either fully succeeds or fully fails.
* Consistency: the database transitions from one valid state to another without violating constraints.
* Isolation: concurrent transactions do not interfere with each other’s intermediate states.
* Durability: once committed, data survives system or server failures.

### 3.4 When to Use SQL

* When application data is **well structured** with clear relationships (e.g., customers, orders, products in e-commerce).\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
* When you need **strong consistency** and transactional integrity, such as in financial or banking systems where correctness is critical.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​

***

### 4. Non-Relational (NoSQL) Databases (6:35–12:54) <a href="#id-4-non-relational-nosql-databases-6351254" id="id-4-non-relational-nosql-databases-6351254"></a>

### 4.1 Types of NoSQL

The video covers several NoSQL families:\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​

* **Document stores** (e.g., MongoDB):
  * Store data as JSON-like documents.
  * Enable complex nested structures inside a single record.
* **Wide-column stores** (e.g., Cassandra, Cosmos DB):
  * Store data in tables, rows, and dynamic columns.
  * Optimized for massive scale and write-heavy workloads.
* **Graph databases** (e.g., Neo4j, Amazon Neptune):
  * Represent entities and relationships as nodes and edges.
  * Useful for recommendation systems and social networks (e.g., Amazon using Neptune for product recommendations).
* **Key-value stores** (e.g., Redis, Memcache):
  * Store data in key-value pairs.
  * Often kept primarily in RAM, making reads/writes extremely fast.

### 4.2 Advantages and Use Cases

* A NoSQL document store could collapse `customer`, `orders`, and `products` into a **single document**, enabling very fast reads at the cost of denormalization.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​
* NoSQL databases handle highly dynamic and large datasets better by avoiding rigid schemas enforced by relational models.\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
* They are optimized for **low latency** and **scalability**, which is important for large-scale applications.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​

### 4.3 Choosing SQL vs NoSQL

Use **SQL** when:\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​

* Data has a clear relational structure.
* Strong consistency and ACID transactions are necessary (e.g., core financial records).

Use **NoSQL** when:\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​

* You need very low latency and high throughput.
* Data is unstructured or semi-structured (JSON).
* Relationships are not critical, or you can embed them in documents.
* You need flexible, scalable storage for massive data volumes, such as user activity logs or key-value event data for recommendation engines.\[[youtube](https://www.youtube.com/watch?v=Y-xBxnqaF4Y)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​

***

### 5. Vertical vs Horizontal Scaling (12:55–24:30) <a href="#id-5-vertical-vs-horizontal-scaling-12552430" id="id-5-vertical-vs-horizontal-scaling-12552430"></a>

### 5.1 Vertical Scaling (Scale Up) (12:55–15:44)

* Vertical scaling means adding more resources (CPU, RAM, disk) to the **same server**.\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
* Pros:
  * Simple approach; architecture doesn’t change.
  * Works well for low to moderate traffic.
* Cons:
  * **Resource limits**: hardware upgrades have a hard cap (you can’t scale a single machine indefinitely).
  * **Lack of redundancy**: if that server fails, the entire application goes down.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​

### 5.2 Horizontal Scaling (Scale Out) (12:55–24:30)

* Horizontal scaling means adding **more servers** and distributing the load across them.\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
* Instead of a single server, you might have three servers running the same application, all behind some routing mechanism.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​

Benefits:\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​

* Higher **fault tolerance**: if one server fails, others keep serving requests.
* Better **scalability**: you can add more servers as traffic grows.

Requirement:\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​

* To implement horizontal scaling, you need a **load balancer** to distribute incoming requests across multiple servers.

***

### 6. Load Balancers and Algorithms (15:45–24:30, 1083s–1466s) <a href="#id-6-load-balancers-and-algorithms-15452430-1083s1466" id="id-6-load-balancers-and-algorithms-15452430-1083s1466"></a>

### 6.1 What a Load Balancer Does (15:45–24:30)

* A load balancer sits in front of your application servers and decides **which server** handles each incoming request.\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
* It prevents any single server from bearing too much load and increases the overall reliability and scalability of the system.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​

### 6.2 Core Algorithms (1083s–1466s)

1. **Round Robin** (1094s–1132s)
   * Simplest algorithm; assigns each new request to the next server in sequence and wraps around when reaching the end.\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
   * Works well when servers have similar hardware and capacity.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​
2. **Least Connections** (1138s–1179s)
   * Routes new requests to the server with the smallest number of **active connections**.\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
   * Good when session lengths vary significantly, because it considers actual load rather than just request count.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​
3. **Least Response Time** (1185s–1239s)
   * Chooses the server with the lowest response time and relatively few active connections.\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
   * Prioritizes fast responses and is helpful when servers have different performance characteristics.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​
4. **IP Hash** (1247s–1286s)
   * Hashes the client’s IP address to decide which server handles its requests.\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
   * Ensures a client consistently connects to the same backend server—useful for maintaining session affinity.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​
5. **Weighted Round Robin / Weighted Least Connections** (1302s–1345s)
   * Servers are assigned **weights** based on capacity (e.g., RAM size), and more traffic is sent to higher-weight servers.\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
   * Example: a server with 64 GB RAM gets more requests than one with 16 GB.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​
6. **Geographical (Geo-based) Algorithms** (1350s–1408s)
   * Route users to servers closest to them geographically (e.g., US East, US West, Europe).\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
   * Reduces latency for global applications.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​
7. **Consistent Hashing** (1410s–1459s)
   * Uses a hash function to map clients or keys onto a **hash ring** and then to the nearest server on that ring.\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
   * Ensures that when servers are added/removed, most clients’ mappings stay the same, minimizing data movement and rebalancing.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​

***

### 7. Health Checks and Types of Load Balancers (1482s–1639s) <a href="#id-7-health-checks-and-types-of-load-balancers-1482s1" id="id-7-health-checks-and-types-of-load-balancers-1482s1"></a>

* Most load balancers support **health checks**, regularly pinging servers to see if they are healthy.\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
* If a server fails health checks, the LB stops sending traffic to it; once it recovers and passes health checks, it can be added back.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​

Types of load balancers mentioned:\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​

* **Software load balancers**:
  * NGINX is a very common software LB where you can configure upstream servers, algorithms, and health checks.
* **Hardware load balancers**:
  * Examples include F5 and Citrix devices used for high performance and enterprise setups.
* **Cloud provider load balancers**:
  * AWS load balancers and Google Cloud load balancing integrate with auto-scaling and security features, making configuration easier when servers are in the cloud.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​

***

### 8. Single Point of Failure (SPOF) (27:23–31:02, 1643s–1739s) <a href="#id-8-single-point-of-failure-spof-27233102-1643s1739s" id="id-8-single-point-of-failure-spof-27233102-1643s1739s"></a>

* A **single point of failure** is a component whose failure brings down the entire system.\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
* Example from the video: a setup with a single database behind multiple API servers; if the DB goes down, all APIs fail despite the load balancer and servers being fine.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​

Issues caused by SPOFs:\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​

* Reliability: failure of one component leads to complete outage.
* Scalability: bottlenecks because many components depend on a single resource.
* Security: attacking that one component compromises the entire system.

Good system design aims to avoid or reduce SPOFs by introducing redundancy and replication (e.g., replicated databases, multiple LBs, and redundant infrastructure).\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​

***

### 9. Caching, Key-Value Stores, and CDNs (6:35–12:54, plus substack article) <a href="#id-9-caching-key-value-stores-and-cdns-6351254-plus-s" id="id-9-caching-key-value-stores-and-cdns-6351254-plus-s"></a>

* Key-value stores like Redis and Memcache primarily store data in RAM, which makes reading and writing cached data extremely fast.[youtube+1](https://www.youtube.com/watch?v=Y-xBxnqaF4Y)\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
* Principle: you should avoid asking the database for the same information repeatedly if it has not changed; cache it and serve from cache instead.\[[youtube](https://www.youtube.com/watch?v=Y-xBxnqaF4Y)]​
* **Application caching**:
  * Use Redis to cache frequently accessed data such as user sessions, product details, or precomputed ML features.\[[youtube](https://www.youtube.com/watch?v=Y-xBxnqaF4Y)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
* **CDNs (Content Delivery Networks)**:
  * Serve static assets (images, CSS, videos) from edge servers located physically closer to users to reduce latency.\[[youtube](https://www.youtube.com/watch?v=Y-xBxnqaF4Y)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
  * For example, users in London should fetch assets from a European PoP instead of a server in New York.\[[youtube](https://www.youtube.com/watch?v=Y-xBxnqaF4Y)]​

Combining caching and CDNs significantly improves system performance and reduces load on origin databases and application servers.\[[youtube](https://www.youtube.com/watch?v=Y-xBxnqaF4Y)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​

***

### 10. Client Types, APIs, and JSON (2:28–6:34) <a href="#id-10-client-types-apis-and-json-228634" id="id-10-client-types-apis-and-json-228634"></a>

* The system serves both **web applications** and **mobile applications**.\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
* Web users typically receive HTML pages that the browser renders; mobile apps mostly communicate via HTTP APIs and receive JSON responses.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​

Example API discussed:\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​

* A `GET /product/{id}` request from a client to fetch product details.
* The server responds with JSON containing fields like `productId`, `name`, `description`, `price`, and other useful metadata used by clients to render their UI.

***

### 11. Practical Heuristics for Interviews (0:00–1:49:48) <a href="#id-11-practical-heuristics-for-interviews-00014948" id="id-11-practical-heuristics-for-interviews-00014948"></a>

* Start design answers from a **simple single-server** architecture, then grow it: add separate DB tier, introduce caching, then horizontal scaling with load balancers, and finally address SPOFs and global distribution.\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​
* When choosing data stores in your design, explicitly justify **SQL vs NoSQL** using the rules above – consistency vs scalability, structure vs flexibility.[youtube+1](https://www.youtube.com/watch?v=Y-xBxnqaF4Y)\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​
* Show awareness of **load balancing algorithms**, health checks, and how to handle failures; interviewers want to see trade-off thinking, not just buzzwords.\[[youtube](https://www.youtube.com/watch?v=adOkTjIIDnk)]​\[[hayksimonyan.substack](https://hayksimonyan.substack.com/p/system-design-explained-apis-databases)]​

These notes follow your original detailed structure and now include the video link, channel name, and timestamps so you can paste them directly into GitBook and navigate back to relevant sections quickly.

Add to follow-upCheck sources
