# Java Backend Developer – Complete Guide

---

## 1. Java Fundamentals

### Variables & Data Types
Java is statically typed — every variable must have a declared type.

- **Primitive types**: `int`, `long`, `double`, `float`, `boolean`, `char`, `byte`, `short`
- **Reference types**: Objects, arrays, Strings

```java
int age = 25;
double salary = 75000.50;
boolean isActive = true;
String name = "Abhay";
```

### Loops
- `for`, `while`, `do-while`, and enhanced `for-each`
```java
for (int i = 0; i < 10; i++) { }
for (String item : list) { }  // for-each
```

### Methods
Reusable blocks of code. Can be static or instance-level. Java supports method overloading (same name, different parameters).
```java
public int add(int a, int b) { return a + b; }
public double add(double a, double b) { return a + b; } // overloaded
```

### Arrays
Fixed-size, zero-indexed collections of the same type.
```java
int[] nums = {1, 2, 3};
String[] names = new String[5];
```

### Strings
Immutable sequence of characters. Common methods: `length()`, `substring()`, `contains()`, `split()`, `trim()`, `toUpperCase()`, `equals()`, `charAt()`.

`StringBuilder` is mutable and preferred for concatenation in loops.

### Java 8+ Features
**Lambdas** — anonymous functions:
```java
List<String> names = Arrays.asList("Alice", "Bob");
names.forEach(n -> System.out.println(n));
```

**Stream API** — functional-style operations on collections:
```java
List<Integer> evens = numbers.stream()
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList());
```

**Optional** — avoids `NullPointerException`:
```java
Optional<String> opt = Optional.ofNullable(value);
opt.ifPresent(v -> System.out.println(v));
```

**Default & Static methods in interfaces** — interfaces can now have concrete implementations.

**java.time API** — modern date/time: `LocalDate`, `LocalDateTime`, `ZonedDateTime`, `Duration`.

**var (Java 10+)** — local type inference:
```java
var list = new ArrayList<String>();
```

**Records (Java 16+)** — concise immutable data classes:
```java
record Person(String name, int age) {}
```

**Sealed Classes (Java 17+)** — restrict which classes can extend a type.

---

## 2. Object-Oriented Programming (OOP)

### Classes & Objects
A **class** is a blueprint; an **object** is an instance of that blueprint.
```java
public class Car {
    String brand;
    int speed;
    void accelerate() { speed += 10; }
}
Car myCar = new Car();
```

### Inheritance
A child class inherits fields and methods from a parent class using `extends`. Java supports single inheritance for classes (but multiple via interfaces).
```java
class Animal { void speak() { System.out.println("..."); } }
class Dog extends Animal {
    @Override
    void speak() { System.out.println("Woof!"); }
}
```

### Polymorphism
One interface, many implementations. Achieved via method overriding (runtime) and method overloading (compile-time).
```java
Animal a = new Dog();  // Dog object referenced as Animal
a.speak();             // calls Dog's speak() at runtime
```

### Abstraction
Hiding implementation details, exposing only what's necessary. Achieved via **abstract classes** and **interfaces**.
```java
abstract class Shape { abstract double area(); }
interface Drawable { void draw(); }
```

### Encapsulation
Bundling data and methods, restricting direct access using access modifiers (`private`, `protected`, `public`). Expose via getters/setters.
```java
public class Account {
    private double balance;
    public double getBalance() { return balance; }
    public void deposit(double amount) { balance += amount; }
}
```

### SOLID Principles (closely tied to OOP)
- **S** – Single Responsibility: one class, one job
- **O** – Open/Closed: open for extension, closed for modification
- **L** – Liskov Substitution: subtypes must be substitutable for base types
- **I** – Interface Segregation: prefer small, specific interfaces
- **D** – Dependency Inversion: depend on abstractions, not concretions

---

## 3. Collections Framework

Java's `java.util` package provides ready-made data structures.

### List (ordered, allows duplicates)
- `ArrayList` – backed by array, fast random access, O(1) get
- `LinkedList` – backed by doubly-linked list, fast insert/delete at ends

```java
List<String> list = new ArrayList<>();
list.add("A"); list.get(0);
```

### Set (no duplicates)
- `HashSet` – O(1) add/contains, no order
- `LinkedHashSet` – insertion order maintained
- `TreeSet` – sorted order, O(log n)

### Map (key-value pairs)
- `HashMap` – O(1) get/put, no order, allows null key
- `LinkedHashMap` – insertion order
- `TreeMap` – sorted by key
- `ConcurrentHashMap` – thread-safe HashMap

```java
Map<String, Integer> scores = new HashMap<>();
scores.put("Alice", 95);
scores.get("Alice");
```

### Queue & Deque
- `LinkedList` implements `Queue` – FIFO
- `PriorityQueue` – elements ordered by natural/custom ordering
- `ArrayDeque` – double-ended queue, faster than `Stack`

### Comparable vs Comparator
- `Comparable` – the object defines its own natural ordering (`compareTo`)
- `Comparator` – external ordering logic, passed separately

```java
// Comparable
class Student implements Comparable<Student> {
    public int compareTo(Student s) { return this.name.compareTo(s.name); }
}

// Comparator (lambda)
list.sort((a, b) -> a.age - b.age);
```

---

## 4. Exception Handling

### Types of Exceptions
- **Checked**: must be caught or declared (`IOException`, `SQLException`)
- **Unchecked** (RuntimeException): `NullPointerException`, `ArrayIndexOutOfBoundsException`
- **Errors**: `OutOfMemoryError`, `StackOverflowError` – not meant to be caught

### try-catch-finally
```java
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Error: " + e.getMessage());
} finally {
    System.out.println("Always runs");
}
```

### Try-with-Resources (Java 7+)
Auto-closes `AutoCloseable` resources:
```java
try (FileReader fr = new FileReader("file.txt")) {
    // use fr
} // fr.close() called automatically
```

### Custom Exceptions
```java
public class InsufficientFundsException extends RuntimeException {
    public InsufficientFundsException(String message) {
        super(message);
    }
}
throw new InsufficientFundsException("Balance too low");
```

### Best Practices
- Catch specific exceptions, not just `Exception`
- Never swallow exceptions silently (`catch (Exception e) {}`)
- Use custom exceptions for business logic errors
- Log exceptions with proper context
- Don't use exceptions for flow control
- Prefer `RuntimeException` for programming errors, checked exceptions for recoverable conditions

---

## 5. Multithreading & Concurrency

### Threads
Two ways to create:
```java
// Extend Thread
class MyThread extends Thread {
    public void run() { System.out.println("Running"); }
}

// Implement Runnable (preferred)
Thread t = new Thread(() -> System.out.println("Running"));
t.start();
```

### Executors (thread pool)
Avoid creating raw threads. Use `ExecutorService`:
```java
ExecutorService pool = Executors.newFixedThreadPool(4);
pool.submit(() -> doWork());
pool.shutdown();
```

Types: `newFixedThreadPool`, `newCachedThreadPool`, `newSingleThreadExecutor`, `newScheduledThreadPool`.

### Synchronization
Prevents race conditions when multiple threads access shared data:
```java
synchronized void increment() { count++; }

// Or using locks
ReentrantLock lock = new ReentrantLock();
lock.lock();
try { count++; } finally { lock.unlock(); }
```

### Atomic Classes
Lock-free thread-safe operations: `AtomicInteger`, `AtomicLong`, `AtomicReference`.
```java
AtomicInteger counter = new AtomicInteger(0);
counter.incrementAndGet();
```

### CompletableFuture (Java 8+)
Asynchronous, non-blocking pipeline:
```java
CompletableFuture.supplyAsync(() -> fetchData())
    .thenApply(data -> process(data))
    .thenAccept(result -> save(result))
    .exceptionally(ex -> { log(ex); return null; });
```

### Key Concepts
- **Deadlock**: two threads wait on each other forever
- **Race condition**: outcome depends on thread scheduling
- **volatile**: ensures visibility of variable changes across threads
- **happens-before**: memory consistency guarantee
- `ConcurrentHashMap`, `CopyOnWriteArrayList` — thread-safe collection alternatives

---

## 6. JDBC (Java Database Connectivity)

JDBC is the standard API for connecting Java applications to relational databases.

### Core Steps
1. Load driver (automatic with JDBC 4+)
2. Get connection
3. Create statement
4. Execute query
5. Process `ResultSet`
6. Close resources

```java
String url = "jdbc:mysql://localhost:3306/mydb";
try (Connection conn = DriverManager.getConnection(url, "user", "pass");
     PreparedStatement ps = conn.prepareStatement("SELECT * FROM users WHERE id = ?")) {
    ps.setInt(1, 1);
    ResultSet rs = ps.executeQuery();
    while (rs.next()) {
        System.out.println(rs.getString("name"));
    }
}
```

### PreparedStatement vs Statement
- Always use `PreparedStatement` — prevents SQL injection, better performance via caching

### Transactions
```java
conn.setAutoCommit(false);
try {
    // multiple operations
    conn.commit();
} catch (Exception e) {
    conn.rollback();
}
```

### Connection Pooling
Raw connections are expensive. Use pools like **HikariCP** (default in Spring Boot), **DBCP**, or **C3P0** to reuse connections.

---

## 7. SQL

### Joins
- `INNER JOIN` – rows matching in both tables
- `LEFT JOIN` – all from left + matching from right (NULLs for no match)
- `RIGHT JOIN` – opposite of LEFT
- `FULL OUTER JOIN` – all rows from both, NULLs where no match
- `SELF JOIN` – table joined with itself

```sql
SELECT u.name, o.total
FROM users u
INNER JOIN orders o ON u.id = o.user_id;
```

### Indexing
Indexes speed up reads but slow down writes. B-Tree index is the default.
- **Clustered index**: defines physical order of rows (primary key)
- **Non-clustered index**: separate structure pointing to rows
- Avoid over-indexing; index columns used in WHERE, JOIN, ORDER BY

### Normalization
- **1NF**: atomic values, no repeating groups
- **2NF**: no partial dependencies (non-key columns depend on full PK)
- **3NF**: no transitive dependencies
- **BCNF**: stricter 3NF

### Transactions & ACID
- **Atomicity**: all or nothing
- **Consistency**: DB stays valid
- **Isolation**: concurrent transactions don't interfere
- **Durability**: committed data survives crashes

Isolation levels: READ UNCOMMITTED → READ COMMITTED → REPEATABLE READ → SERIALIZABLE

### Query Optimization
- Use `EXPLAIN` / `EXPLAIN ANALYZE` to inspect query plans
- Avoid `SELECT *` — fetch only needed columns
- Use indexed columns in WHERE/JOIN
- Avoid functions on indexed columns in WHERE (`WHERE YEAR(created_at) = 2024` breaks index)
- Use pagination (`LIMIT`/`OFFSET` or cursor-based)
- Avoid N+1 queries

---

## 8. Spring Core

### Dependency Injection (DI)
Instead of a class creating its own dependencies, they are provided from outside. This decouples code and makes it testable.

```java
// Without DI
class OrderService { PaymentService ps = new PaymentService(); }

// With DI
class OrderService {
    private final PaymentService ps;
    public OrderService(PaymentService ps) { this.ps = ps; } // injected
}
```

### Inversion of Control (IoC)
The Spring container (IoC container) manages object creation and wiring — control is inverted from the developer to the framework.

### Types of DI in Spring
- **Constructor injection** (preferred — immutable, testable)
- **Setter injection**
- **Field injection** (`@Autowired` — avoid, hard to test)

### Bean Lifecycle
1. Instantiate
2. Inject dependencies
3. `@PostConstruct` / `afterPropertiesSet()` – init logic
4. Bean in use
5. `@PreDestroy` / `destroy()` – cleanup

### Key Annotations
- `@Component`, `@Service`, `@Repository`, `@Controller` – stereotype annotations (mark beans)
- `@Autowired` – inject a dependency
- `@Qualifier` – disambiguate when multiple beans of same type
- `@Primary` – default bean when multiple candidates
- `@Scope` – `singleton` (default) or `prototype`
- `@Configuration` + `@Bean` – Java-based config

---

## 9. Spring Boot

### Auto Configuration
Spring Boot automatically configures beans based on what's on the classpath. Add `spring-boot-starter-web` → Tomcat, Jackson, DispatcherServlet all configured automatically. Controlled via `@EnableAutoConfiguration` (included in `@SpringBootApplication`).

### Profiles
Environment-specific configs using `@Profile` and `application-{profile}.yml`.
```yaml
# application-dev.yml
spring.datasource.url: jdbc:h2:mem:devdb
```
Activate with: `spring.profiles.active=dev`

### Spring Security
Authentication and authorization framework.
- `SecurityFilterChain` – define HTTP security rules
- `UserDetailsService` – load user info
- JWT or session-based auth
- Method-level security: `@PreAuthorize("hasRole('ADMIN')")`

### Validation
Using Bean Validation (Hibernate Validator):
```java
public class UserDTO {
    @NotBlank String name;
    @Email String email;
    @Min(18) int age;
}

// In controller
@PostMapping("/users")
public ResponseEntity<?> create(@Valid @RequestBody UserDTO dto) { ... }
```

### application.properties / application.yml
Central configuration: DB URL, port, logging, custom props. Access custom props with `@Value("${my.prop}")` or `@ConfigurationProperties`.

### Spring Boot Actuator
Exposes endpoints for health, metrics, env, beans — useful for monitoring.

---

## 10. REST APIs

### HTTP Methods
- `GET` – retrieve resource (idempotent, no body)
- `POST` – create resource
- `PUT` – replace resource entirely
- `PATCH` – partial update
- `DELETE` – remove resource

### Status Codes
- **2xx**: Success — 200 OK, 201 Created, 204 No Content
- **3xx**: Redirect — 301 Moved Permanently
- **4xx**: Client error — 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 409 Conflict, 422 Unprocessable Entity
- **5xx**: Server error — 500 Internal Server Error, 503 Service Unavailable

### REST Controller (Spring Boot)
```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) { ... }

    @PostMapping
    public ResponseEntity<User> createUser(@Valid @RequestBody UserDTO dto) { ... }
}
```

### Authentication
- **Basic Auth** – username:password in header (only over HTTPS)
- **JWT (JSON Web Token)** – stateless; token contains claims, signed with secret. Sent in `Authorization: Bearer <token>` header.
- **OAuth 2.0** – delegated authorization (Google Login, etc.)
- **API Keys** – simple, sent in header or query param

### Documentation
- **Springdoc / Swagger (OpenAPI 3)** – auto-generates interactive API docs from annotations
```java
@Operation(summary = "Get user by ID")
@GetMapping("/{id}")
public User getUser(@PathVariable Long id) { ... }
```

---

## 11. JPA / Hibernate

### ORM (Object-Relational Mapping)
Maps Java classes to database tables, eliminating most raw SQL. JPA is the specification; Hibernate is the most popular implementation.

```java
@Entity
@Table(name = "users")
public class User {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String email;
}
```

### Relationships
```java
@OneToMany(mappedBy = "user", cascade = CascadeType.ALL)
private List<Order> orders;

@ManyToOne
@JoinColumn(name = "user_id")
private User user;

@ManyToMany
@JoinTable(...)
private Set<Role> roles;
```

### Spring Data JPA Repositories
```java
public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByEmail(String email);
    @Query("SELECT u FROM User u WHERE u.age > :age")
    List<User> findUsersOlderThan(@Param("age") int age);
}
```

### JPQL vs Native Queries
- **JPQL** – queries on entity objects, DB-agnostic
- **Native SQL** – raw SQL when JPQL isn't enough

### Performance Optimization
- **Lazy vs Eager loading** – prefer LAZY; EAGER causes N+1 queries
- **N+1 problem** – use `JOIN FETCH` or `@EntityGraph` to fetch associations in one query
- **Pagination** – `Pageable` interface
- **Second-level cache** – Hibernate cache (EhCache, Redis) to avoid repeated DB hits
- **DTO projections** – fetch only needed fields instead of full entities

---

## 12. Microservices

### What are Microservices?
Breaking a monolith into small, independently deployable services — each owning its domain and database.

### Service Communication
- **Synchronous**: REST (HTTP) or gRPC — caller waits for response
- **Asynchronous**: Kafka, RabbitMQ — caller doesn't wait; event-driven

### API Gateway
Single entry point for all client requests. Handles routing, auth, rate limiting, load balancing. Examples: **Spring Cloud Gateway**, **Kong**, **AWS API Gateway**.

### Service Discovery
Services register themselves and discover others dynamically.
- **Eureka** (Netflix/Spring Cloud) – server-side discovery
- **Consul** – with health checking
- In Kubernetes, service discovery is built-in

### Resilience Patterns
- **Circuit Breaker** – stop calling a failing service (Resilience4j)
- **Retry** – retry failed calls with backoff
- **Bulkhead** – isolate failures to one part of the system
- **Timeout** – don't wait forever
- **Fallback** – return default response when service fails

```java
@CircuitBreaker(name = "paymentService", fallbackMethod = "paymentFallback")
public Payment processPayment(Order order) { ... }
```

### Distributed Tracing
Track a request across multiple services using correlation IDs. Tools: **Zipkin**, **Jaeger**, **OpenTelemetry**.

### Saga Pattern
Manages distributed transactions across services using a sequence of local transactions, each publishing events or messages.

---

## 13. Kafka

### What is Kafka?
A distributed, fault-tolerant, high-throughput message streaming platform. Used for event-driven architectures, decoupling services, and real-time data pipelines.

### Core Concepts
- **Topic** – a named stream/channel of messages
- **Producer** – publishes messages to a topic
- **Consumer** – reads messages from a topic
- **Consumer Group** – multiple consumers sharing the load (each partition read by one consumer per group)
- **Partition** – topics are split into partitions for parallelism and scalability
- **Broker** – a Kafka server
- **Offset** – position of a message in a partition (consumers track their offset)
- **Zookeeper / KRaft** – manages cluster metadata

### Message Flow
```
Producer → Topic (Partition) → Consumer Group → Consumer
```

### Spring Kafka
```java
// Producer
@Autowired KafkaTemplate<String, String> kafkaTemplate;
kafkaTemplate.send("orders", "order-created-payload");

// Consumer
@KafkaListener(topics = "orders", groupId = "inventory-service")
public void handleOrder(String message) {
    // process the event
}
```

### Key Benefits
- **Decoupling** – producer and consumer are independent
- **Durability** – messages persisted on disk
- **Replay** – consumers can re-read past messages
- **Scalability** – horizontal scaling via partitions

### Delivery Guarantees
- **At most once** – may lose messages
- **At least once** – may duplicate (most common)
- **Exactly once** – guaranteed, requires transactions (more overhead)

---

## 14. Docker

### What is Docker?
A platform to build, ship, and run applications in containers — lightweight, isolated environments that package code + all dependencies.

### Why Docker?
"Works on my machine" problem solved. Dev, staging, and prod environments are identical.

### Key Concepts
- **Image** – read-only template (built from Dockerfile)
- **Container** – running instance of an image
- **Dockerfile** – instructions to build an image
- **Registry** – storage for images (Docker Hub, ECR, GCR)
- **Volume** – persistent storage for containers
- **Network** – communication between containers

### Dockerfile (Spring Boot app)
```dockerfile
FROM eclipse-temurin:21-jre
WORKDIR /app
COPY target/myapp.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### Common Commands
```bash
docker build -t myapp:1.0 .          # build image
docker run -p 8080:8080 myapp:1.0    # run container
docker ps                             # list running containers
docker stop <id>                      # stop container
docker logs <id>                      # view logs
docker exec -it <id> /bin/bash        # shell into container
```

### Docker Compose
Define and run multi-container apps:
```yaml
services:
  app:
    build: .
    ports: ["8080:8080"]
    depends_on: [db]
  db:
    image: postgres:15
    environment:
      POSTGRES_PASSWORD: secret
```

---

## 15. Kubernetes (K8s)

### What is Kubernetes?
Container orchestration platform — automates deployment, scaling, and management of containerized applications.

### Core Concepts
- **Node** – a machine (VM or physical) in the cluster
- **Pod** – smallest deployable unit; wraps one or more containers
- **Deployment** – manages replica sets and rolling updates
- **Service** – stable network endpoint for pods (load balances between them)
- **ConfigMap** – externalize non-sensitive config
- **Secret** – externalize sensitive data (passwords, keys)
- **Namespace** – logical isolation within a cluster
- **Ingress** – HTTP routing rules; single entry point
- **PersistentVolume** – durable storage

### Key YAML Example
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    spec:
      containers:
      - name: myapp
        image: myapp:1.0
        ports:
        - containerPort: 8080
```

### Scaling
```bash
kubectl scale deployment myapp --replicas=5
# or via HPA (Horizontal Pod Autoscaler) based on CPU/memory
```

### Common Commands
```bash
kubectl get pods
kubectl describe pod <name>
kubectl logs <pod>
kubectl apply -f deployment.yaml
kubectl rollout status deployment/myapp
kubectl rollout undo deployment/myapp   # rollback
```

---

## 16. Cloud & CI/CD

### AWS (most common for Java backends)
- **EC2** – virtual machines to run apps
- **ECS / EKS** – managed container/Kubernetes service
- **RDS** – managed relational DB (MySQL, Postgres, etc.)
- **S3** – object/file storage
- **SQS/SNS** – messaging queues and pub/sub
- **Lambda** – serverless functions
- **CloudWatch** – logs, metrics, alarms
- **IAM** – identity and access management

### Azure & GCP
- Similar services under different names
- Azure: AKS (Kubernetes), Azure SQL, Blob Storage, Service Bus
- GCP: GKE (Kubernetes), Cloud SQL, Cloud Storage, Pub/Sub

### CI/CD Pipeline
Automates build → test → deploy cycle.

**CI (Continuous Integration)**:
- Code pushed → pipeline triggers
- Compile, run tests, static analysis, build Docker image

**CD (Continuous Delivery/Deployment)**:
- Deploy to staging automatically
- Deploy to production (manually triggered or fully automated)

**Tools**: GitHub Actions, Jenkins, GitLab CI, CircleCI, ArgoCD

```yaml
# GitHub Actions example
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-java@v3
        with: { java-version: '21' }
      - run: mvn test
      - run: docker build -t myapp .
```

### Monitoring
- **Prometheus** – metrics collection
- **Grafana** – dashboards
- **ELK Stack** – Elasticsearch + Logstash + Kibana for log aggregation
- **Spring Boot Actuator** – expose `/health`, `/metrics` endpoints

---

## 17. System Design

### Scalability
- **Vertical scaling** – bigger machine (limited)
- **Horizontal scaling** – more machines (preferred); requires stateless services

### Caching
Reduce DB load, improve latency.
- **In-memory**: Redis, Memcached
- **CDN**: for static assets
- **Application cache**: `@Cacheable` in Spring
- **Cache strategies**: Cache-Aside (most common), Write-Through, Write-Behind

```java
@Cacheable(value = "users", key = "#id")
public User getUser(Long id) { return repo.findById(id); }
```

### Load Balancing
Distributes traffic across multiple instances.
- **Round Robin**, **Least Connections**, **IP Hash**
- Tools: NGINX, HAProxy, AWS ALB, Kubernetes Service

### Databases
- **Relational (SQL)**: strong consistency, ACID, joins — use for transactional data
- **NoSQL**: MongoDB (documents), Cassandra (wide-column), Redis (key-value), Elasticsearch (search)
- **Read replicas** – scale reads
- **Database sharding** – partition data across multiple DB nodes

### Distributed Systems Concepts
- **CAP Theorem**: a distributed system can only guarantee 2 of 3 — Consistency, Availability, Partition tolerance
- **Eventual Consistency** – data will be consistent eventually (not immediately)
- **Message Queues** – decouple services, handle traffic spikes (Kafka, SQS)
- **Idempotency** – same operation can be called multiple times without different outcomes (critical for retries)

### API Design Patterns
- **Pagination** – cursor-based preferred over offset for large datasets
- **Rate Limiting** – protect APIs from abuse (token bucket, sliding window)
- **Versioning** – `/api/v1/users` or header-based

### Real-world Design Examples
- **URL Shortener**: DB + cache (Redis) + unique ID generation (Snowflake)
- **Notification Service**: Kafka consumer + worker pool + retry queue
- **E-Commerce Order Flow**: Saga pattern across Order, Inventory, Payment, Shipping microservices

---

*This guide covers the full Java backend stack — from language fundamentals to production-grade system design.*
