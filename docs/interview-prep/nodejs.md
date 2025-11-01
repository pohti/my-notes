# NodeJS

## **1. Core Concepts**

### ❓What is Node.js?

**Answer:**
Node.js is a **runtime environment** that allows JavaScript to run outside of a browser.
It’s built on **Chrome’s V8 engine** and uses an **event-driven, non-blocking I/O model**, making it efficient for I/O-heavy applications like APIs, real-time apps, or microservices.

---

### ❓Why is Node.js single-threaded?

**Answer:**
Node.js uses a **single-threaded event loop** to handle concurrent requests efficiently without creating multiple threads.
It delegates heavy I/O operations (like file access or network calls) to **libuv’s thread pool**, freeing the main thread to continue processing.

---

### ❓Explain the Event Loop.

**Answer:**
The **Event Loop** is how Node.js handles asynchronous operations on a single thread.
It works in **phases** (timers, pending callbacks, poll, check, close callbacks) and picks up events/callbacks from queues to execute.

**Example:**

```js
console.log('A')
setTimeout(() => console.log('B'), 0)
console.log('C')
```

**Output:**

```
A
C
B
```

→ Because `setTimeout` is asynchronous, it gets queued for the event loop to execute later.

---

### ❓Difference between `process.nextTick()`, `setImmediate()`, and `setTimeout()`?

**Answer:**

| Function             | When it runs                                            |
| -------------------- | ------------------------------------------------------- |
| `process.nextTick()` | Runs **before** the next event loop phase               |
| `setImmediate()`     | Runs in the **check** phase, after I/O events           |
| `setTimeout(fn, 0)`  | Runs after a minimum 1ms delay, in the **timers** phase |

---

### ❓What are Promises and async/await?

**Answer:**
A **Promise** represents a value that may be available now, later, or never.
`async/await` is syntactic sugar over Promises to write asynchronous code in a synchronous style.

**Example:**

```js
async function run() {
  const data = await fetchData()
  console.log(data)
}
```

Even though it “waits,” the **event loop is not blocked**, so it’s still asynchronous.

---

## **2. Modules and Architecture**

### ❓Difference between CommonJS and ES Modules?

**Answer:**

| CommonJS                          | ES Modules                   |
| --------------------------------- | ---------------------------- |
| Uses `require()`                  | Uses `import/export`         |
| Synchronous loading               | Asynchronous loading         |
| Default in older Node.js versions | Modern standard (Node ≥ v14) |

---

### ❓How does Node.js resolve modules?

**Answer:**
Node looks in this order:

1. Built-in modules (like `fs`, `http`)
2. `node_modules` folder
3. Relative paths (`./`, `../`)
4. Fallback → throws `MODULE_NOT_FOUND`

---

## **3. Performance and Scaling**

### ❓How can you scale a Node.js application?

**Answer:**

* **Cluster mode** (using `cluster` or **PM2**) to use all CPU cores.
* Use **load balancing** (Nginx, ELB).
* Optimize async operations, caching, and connection pools.
* Move CPU-heavy tasks to **worker threads** or background queues.

---

### ❓What are Streams?

**Answer:**
Streams handle large data efficiently without loading it all into memory.
Types:

* **Readable:** e.g. reading a file
* **Writable:** e.g. writing a file
* **Duplex:** both (e.g. TCP sockets)
* **Transform:** modifies data (e.g. gzip compression)

**Example:**

```js
fs.createReadStream('input.txt').pipe(fs.createWriteStream('output.txt'))
```

---

## **4. Express.js / Frameworks**

### ❓What is middleware in Express?

**Answer:**
Middleware is a function that processes requests in the pipeline.

```js
app.use((req, res, next) => {
  console.log('Request received')
  next()
})
```

It can modify `req`, `res`, or end the response.
Order matters!

---

### ❓How do you handle errors in Express?

**Answer:**
Use a middleware with 4 arguments:

```js
app.use((err, req, res, next) => {
  res.status(500).json({ message: err.message })
})
```

Or wrap async functions:

```js
app.get('/', async (req, res, next) => {
  try {
    const data = await getData()
    res.send(data)
  } catch (err) {
    next(err)
  }
})
```

---

## **5. Security**

### ❓How do you secure a Node.js app?

**Answer:**

* Use **Helmet** to set secure headers.
* Sanitize user input to prevent XSS/SQL injection.
* Use **parameterized queries**.
* Store passwords with **bcrypt**.
* Use **JWTs** or secure cookies for authentication.
* Enable **rate limiting** (e.g., `express-rate-limit`).

---

### ❓How do you store passwords securely?

**Answer:**
Hash them using `bcrypt`:

```js
const hash = await bcrypt.hash(password, 10)
const match = await bcrypt.compare(password, hash)
```

---

## **6. Testing and Debugging**

### ❓How do you test a Node.js API?

**Answer:**
Use **Jest** or **Mocha + Supertest**.

**Example (Supertest + Jest):**

```js
const request = require('supertest')
const app = require('../app')

test('GET /users returns 200', async () => {
  await request(app).get('/users').expect(200)
})
```

---

### ❓How do you handle uncaught errors?

**Answer:**

```js
process.on('uncaughtException', err => {
  console.error('Uncaught Exception', err)
  process.exit(1)
})

process.on('unhandledRejection', err => {
  console.error('Unhandled Rejection', err)
})
```

---

## **7. Database and Caching**

### ❓How do you connect Node.js with a database?

**Answer:**
Using a library or ORM:

* **Postgres:** `pg`, Sequelize, Prisma
* **MongoDB:** `mongoose`

**Example (Postgres):**

```js
const { Pool } = require('pg')
const pool = new Pool()
const res = await pool.query('SELECT * FROM users')
```

---

### ❓How do you implement caching?

**Answer:**
Use **Redis** for distributed cache.

```js
const redis = require('redis')
const client = redis.createClient()
await client.set('user:1', JSON.stringify(user))
```

---

## **8. Node.js + AWS / Cloud**

### ❓How does Node.js interact with AWS?

**Answer:**

* Use **AWS SDK** to access S3, SNS, SQS, DynamoDB, etc.
* Run Node.js apps on **Lambda**, **ECS**, or **EKS**.
* Use **CloudWatch** for logging/monitoring.

**Example (S3 upload):**

```js
await s3.putObject({
  Bucket: 'my-bucket',
  Key: 'file.txt',
  Body: data
}).promise()
```

---

## **9. Performance Optimization**

### ❓How do you improve Node.js performance?

**Answer:**

* Use async I/O (avoid sync methods).
* Cache repeated queries.
* Use gzip compression.
* Use connection pooling.
* Minimize middleware.
* Profile bottlenecks using `clinic.js` or Chrome DevTools.

---

## **10. Common Practical Questions**

| Question                                   | Short Answer                                      |
| ------------------------------------------ | ------------------------------------------------- |
| **How to handle rate limiting?**           | Use Redis + `express-rate-limit`.                 |
| **How to handle file uploads?**            | Use `multer` or streams.                          |
| **How to gracefully shut down a server?**  | Listen to SIGTERM → close connections, then exit. |
| **How to share variables across modules?** | Use environment variables or config files.        |
| **How to handle large JSON input?**        | Stream parsing or limit `body-parser` size.       |

---

## 🧩 **11. Example Scenario Question**

**Q:** How would you design a Node.js microservice for handling user uploads?
**A:**

* Use **Express** for routes
* Upload files to **S3** via stream
* Store metadata in **Postgres**
* Publish message to **SQS** for processing
* Use **JWT auth** for security
* Cache frequently accessed metadata in **Redis**
