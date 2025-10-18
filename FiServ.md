<div class="details-wrapper_rendered details-wrapper">

  <details>
    <summary>How do you secure APIs using Spring Security?</summary>
    <div class="preserve-format">
      
      Securing REST APIs with Spring Security primarily involves stateless, token-based authentication (JWT or OAuth2), 
      ensuring every request carries proof of identity and authorization.
      
      Component::
      
      1. Authentication:-
         Role: Verifies the client's identity (who they are).
         Config: Use a UsernamePasswordAuthenticationFilter for login, and custom JWT/Token Filters to intercept 
                 every subsequent request, validating the token signature and expiration.
      
      2. Authorization:-
         Role: Determines what the authenticated client can access.
         Config: Achieved via URL-based matching (http.authorizeHttpRequests()) or 
                 method-level annotations (@PreAuthorize('hasRole("ADMIN")')).
      
      3. Authentication Type:-
         Role: Stateless Bearer Tokens (JWT/OAuth2).
         Config: Disable session management (sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)). 
                 The token is validated using a JwtDecoder or similar mechanism.
      
      4. Entry Point:-
         Role: Handles unauthenticated requests (sends 401 Unauthorized).
         Config: Custom AuthenticationEntryPoint to return a clean JSON response instead of the default HTML login page.
      
      5. CORS/CSRF:-
         Role: Cross-Origin Request/Response and prevention of harmful requests.
         Config: Configure CORS globally and disable CSRF for stateless APIs, as JWTs are immune to session-based CSRF attacks.
    </div>
  </details>

  <details>
    <summary>How do you handle thread safety in a multi threaded Java application ?</summary>
    <div class="preserve-format">
      
      Thread safety ensures that shared mutable state is accessed and updated correctly by multiple threads, 
      preventing race conditions and data corruption. I employ a layered approach:
      
      Immutability: The most reliable way. Design classes (Value Objects) whose state cannot change after construction
      (e.g., using final fields, defensive copying). Stateless services (no instance variables) are inherently thread-safe.
      
      Synchronization/Locking:
      
        - Intrinsic Locks (synchronized): Used on methods or blocks to ensure only one thread can execute a 
          critical section at a time. Must be used sparingly due to performance cost.
        - Explicit Locks (ReentrantLock): Offers more fine-grained control, allowing for separate read/write locks 
          (ReadWriteLock) and providing non-blocking attempts (tryLock()).
      
      Atomic Variables: Use classes from the java.util.concurrent.atomic package (e.g., AtomicLong, AtomicReference) 
       for single-variable updates, which use Compare-and-Swap (CAS) operations instead of heavy locking, 
       significantly improving throughput for counters.
      
      Concurrent Collections: Use thread-safe alternatives for collections (e.g., ConcurrentHashMap, CopyOnWriteArrayList) 
        to manage concurrent access efficiently without explicitly writing synchronization logic.
      
      ThreadLocal: Used to provide a local copy of a variable for each thread, eliminating sharing and thus ensuring safety 
      (common for storing request context or transaction IDs).
    </div>
  </details>

  <details>
    <summary>What are some strategies to implement rate limiting in APIs?</summary>
    <div class="preserve-format">
      
      Rate limiting is crucial for protecting APIs against abuse, DoS attacks, and ensuring fair usage. 
      Implementation is typically done at the API Gateway or a dedicated middleware layer.
      
      Strategy (Algorithm)::
      
      1. Token Bucket: 
            A virtual bucket holds a fixed capacity of "tokens." Each request consumes one token. 
            Tokens are refilled at a fixed rate. If the bucket is empty, the request is dropped/throttled.
            **Allows short bursts of traffic (burst capacity) up to the bucket size, but strictly limits the long-term rate.**
      
      2. Sliding Window Log: 
            Timestamps of all requests from a client are stored (e.g., in Redis). 
            When a new request comes in, the system removes stale timestamps (outside the window) and counts the remaining ones. 
            If the count exceeds the limit, the request is rejected.
            **Highly accurate and prevents the "burst at the edge" problem of the Fixed Window method.**
      
      3. Fixed Window Counter: 
            The simplest method. Requests are counted in a fixed time window (e.g., 60 seconds). 
            All requests after the limit is reached are denied until the next window starts.
            **Easy to implement but can allow a double-burst at the window boundary.**

      Key Implementation Details:
       Storage: Use a fast, distributed store like Redis to keep counters/timestamps synchronized across multiple microservice instances.
       Response: Return HTTP status code 429 Too Many Requests along with Retry-After and X-RateLimit-* headers.
  </div>
  </details>
  <details>
    <summary>Can you explain statelessness in REST and how it's enforced in your services?</summary>
    <div class="preserve-format">

      Statelessness is a core REST constraint meaning that each request from the client to the server must contain all 
      the information needed to understand and process the request. 
      The server must not store any client session context between requests.
      
      Enforcement in Services: 
      Token-Based Authentication: We use JWTs (JSON Web Tokens). The token contains all necessary user and role information (claims). 
      The server validates the token (signature and expiration) but never stores a session ID, making the request self-contained.
      
      No Server-Side Sessions: We explicitly disable session management in our Spring Boot configuration, as mentioned in the 
      Spring Security answer, to prevent the accidental use of server-side state (e.g., HttpSession).
      
      Client Manages State: Any application state must be maintained by the client (e.g., in a browser's local storage, or embedded 
      in the request/resource path). For example, the client holds the orderId to request /orders/{orderId}.
      
      Idempotency: While not directly statelessness, idempotent methods (GET, PUT, DELETE) reinforce the idea that each request is 
      an isolated operation, independent of previous ones.
      
      Benefit: Statelessness dramatically improves scalability and reliability because any available server can handle any request, 
      simplifying load balancing and failure recovery.
      
  </div>
  </details>
  <details>
    <summary>What are common challenges when breaking a monolith into mico services?</summary>
    <div class="preserve-format">

      Category::
      
      1. Data Management
          Challenge	: Shared Database: The single largest obstacle. 
             Tightly coupled data models make independent deployment impossible.
          Mitigating Strategy: Database per Service: Isolate data into separate service-owned databases. 
             Use the Strangler Fig Pattern to redirect read/write access gradually.
      
      2. Communication
          Challenge	: Replacing in-process function calls with network calls, introducing latency, 
             serialization overhead, and inherent unreliability.
          Mitigating Strategy: Implement reliable inter-service communication: Synchronous (REST/gRPC) for queries;
            Asynchronous (Kafka/RabbitMQ) for commands/events. Use the Circuit Breaker Pattern.
      
      3. Distributed Transactions
          Challenge	: Atomic transactions spanning multiple services are difficult to guarantee 
             (no more simple COMMIT/ROLLBACK).
          Mitigating Strategy: Use the Saga Pattern (a sequence of local transactions coordinated via events or commands) 
            with compensating actions for failure recovery.
      
      4. Monitoring/Tracing
          Challenge	: Tracing a request across dozens of services becomes complex without centralized tooling.
          Mitigating Strategy: Implement a centralized logging system (ELK/Loki), and distributed tracing 
           with tools like Jaeger/Zipkin to visualize request flow.
      
      5. Organizational
          Challenge	: 
             Team Restructuring: Teams must shift from working on features across a monolith
             to owning the full lifecycle of a few services ("You build it, you run it").
          Mitigating Strategy: 
             Adopt the Conway's Law approach: structure teams around business capabilities
             (e.g., "Order Team") to align with service boundaries.
  </div>
  </details>
  <details>
    <summary>What is the role of an API Gateway and how do you configure one?</summary>
    <div class="preserve-format">

      An API Gateway acts as the single entry point for all clients, routing external traffic 
      to the appropriate internal microservices. It is the "front door" of the microservices architecture.

      Key Role
      1. Traffic Routing: 
          Routes client requests to the correct service instance using a dynamic routing configuration based on the URL path.
     
      2. Authentication/Authorization: 
          Offloads security by authenticating tokens (JWT) and performing authorization checks, centralizing this logic away 
          from individual services.
      
      3. Rate Limiting/Throttling:
          Enforces usage policies to protect backend services from being overwhelmed.
      
      4. Response Aggregation:
          Allows combining multiple microservice responses into a single client response for complex data needs.
      
      5. Protocol Translation:
          Can handle different client protocols (e.g., REST) and translate them into different backend protocols (e.g., gRPC, Kafka).
      
      6. Observability:
          Centralizes logging, monitoring, and request tracing before traffic hits the backend.

       Configuration (Example: Spring Cloud Gateway / AWS API Gateway)
          Route Definitions: Define mapping rules based on path, host, or header 
           (e.g., "If path is /users/**, route to users-service on port 8081").
          
          Filters/Plugins: Apply global or route-specific filters. For example, a Pre-Filter to inject a validated 
           User ID into the HTTP header for backend services, or a Post-Filter for JSON response transformation.
         
          Load Balancing: Integrate with service discovery (e.g., Eureka, Consul) 
           or AWS ALB to distribute traffic across healthy service instances.
        
  </div>
  </details>
  <details>
    <summary>How do you deploy a Spring boot microservice on AWS ECS and Elastic bean stack?</summary>
    <div class="preserve-format">

      Both are used for deployment, but they cater to different levels of control and complexity.
      
      1. AWS Elastic Beanstalk (EB)
      
      Approach: Platform as a Service (PaaS). It's the simplest method for deploying a standard Spring Boot fat JAR/WAR.
      
      Mechanism: You upload the JAR file, and EB automatically provisions and manages the entire infrastructure 
        (EC2 instances, Load Balancer, Auto Scaling Group, health checks, environment setup).
      
      Pros: Extremely fast to deploy (minimal configuration), handles patches/OS maintenance. 
            Great for single-service applications or monolithic apps.
      Cons: Less granular control over the underlying EC2/networking configuration.

     2. AWS Elastic Container Service (ECS)
     
     Approach: Container Orchestration (Container as a Service). Used for containerized (Docker) applications.
     
     Mechanism:
     Dockerize: Create a Docker image of the Spring Boot app and push it to ECR.
     
     Task Definition: Define a JSON blueprint that specifies the container image, 
       resource limits (CPU/memory), port mappings, and IAM role.
     
     Service: Create an ECS Service that maintains the desired number of instances (tasks) 
         of the Task Definition, managing deployment and scaling.
     
     Cluster: The service runs on an ECS Cluster (either on EC2 for full control or 
              Fargate for serverless container management).
     
     Pros: High control over infrastructure and scaling; 
           standardized deployment via containers; 
           ideal for microservices architectures.
     Cons: Higher complexity in initial setup (IAM roles, ECR, Task Definitions).
      
  </div>
  </details>
  <details>
    <summary>Describe your use of Cloudwatch and Cloudtrail?</summary>
    <div class="preserve-format">

    These are complementary AWS services essential for observability, auditing, and compliance.
    
    1. CloudWatch Service
       Primary Role	: Monitoring and Observability. 
         Collects metrics, logs, and events to monitor application performance and infrastructure health.
       
       Key Use Case :
          Application Performance Monitoring (APM): We use CloudWatch Logs for centralized Spring Boot application logging. 
          We create Metric Filters (e.g., count occurrences of the word ERROR or HTTP 500) and 
          set up Alarms on those custom metrics to trigger SNS notifications to PagerDuty or Slack.
       
    2. CloudTrail Service
       Primary Role	: Governance, Compliance, and Auditing. 
         Logs every API call made to AWS services (who did what, when, and from where).
         
       Key Use Case :
         Security and Compliance: Critical for a FinTech environment. 
         Used to track changes to sensitive resources like S3 bucket policies, IAM roles, or database deletion events. 
         If a security incident occurs, CloudTrail is the forensic audit log to determine the root cause and the actor.
  </div>
  </details>
  <details>
    <summary>How do you rollback a failed deployment in a microservices architecture?</summary>
    <div class="preserve-format">

      Rollbacks in a microservices environment are complex because they often involve coordinating multiple services 
      and potentially database changes. 
      Our strategy relies on deployment patterns designed for quick failure recovery.

    1. Blue/Green Deployment (Preferred):
    
       Maintain two identical production environments: Blue (current version) and Green (new version).
       New version is deployed to Green, tested in isolation, and then traffic is instantaneously 
       switched from Blue to Green at the load balancer/API Gateway.
       
       Rollback: If issues arise, we instantly switch traffic back to the stable Blue environment. 
                 This is the fastest rollback mechanism, taking seconds.
    
    2. Canary Deployment (For high-risk changes):
    
      The new version (Canary) is deployed only to a small subset of servers or users (e.g., 1-5% of traffic).
      Monitor key metrics (error rates, latency) on the Canary for a grace period.
    
      Rollback: If the Canary metrics breach thresholds, the canary traffic is 
                immediately stopped and the small Canary group is terminated.
    
    3. Feature Toggles (Decoupling Release from Deployment):
    
       New, potentially risky logic is wrapped in a Feature Flag.
    
       Rollback: If the feature causes issues, the flag is flipped off in the configuration 
       (e.g., a simple call to a feature flag service like LaunchDarkly), 
       instantly disabling the faulty code without a full code rollback or redeployment.

  </div>
  </details>
  <details>
    <summary>Describe a time when you had to optimize performance in your backend APIs?</summary>
    <div class="preserve-format">

    (Use the STAR method, focusing on metrics and results.)
    Situation: 
       We had a critical /client/dashboard API that was taking over 2,500ms to load, significantly 
       impacting user experience and causing high load on the database during peak hours.
    
    Task: Reduce the P95 latency of this API to under 500ms.
    
    Action:
     Profiling: Used APM tools (e.g., Dynatrace/New Relic) to trace the request and found that 80% of the time was spent
     on three sequential database calls for static reference data (e.g., currency codes, user roles).
     
     Caching: Implemented a local, in-memory cache (using Caffeine or Guava) for the static reference data, 
       configuring a 1-hour Time-To-Live (TTL).
     
     Database Optimization: Found a complex, unoptimized join. Added a missing index and 
        modified the query to a faster union operation.
    
    Result: The P95 latency dropped from 2,500ms to 350ms. The database load from this API was virtually eliminated, 
        and the client-facing dashboard experience was significantly improved.
    

   </div>
  </details>
  <details>
    <summary>Have you mentored juniors or Lead a feature from scratch?</summary>
      <div class="preserve-format">
            
      (Answer with a direct 'Yes' followed by specific examples using the STAR format.)
      Yes, I have done both.
      
      A. Feature Leadership (Example: New Payment Gateway Integration)
        Feature: Led the design and development of integrating a new, high-volume payment processor
          to diversify our transaction routes.
      
        Leadership/Architectural Role: I owned the System Design Document, defined the Interface Contract (API design) 
         for our internal services, and championed the Circuit Breaker implementation (Hystrix/Resilience4j) 
         to ensure resilience against the third-party processor's outages.
      
        Result: Delivered the feature on time, allowing us to process 20% of our transaction volume through the new processor, 
         successfully mitigating vendor lock-in risk.
      
      B. Mentoring Juniors (Example: Onboarding a New College Hire)
         Mentoring Role: I formally mentored a new college hire on our core transaction service team.
      
         Process: I started them with small, self-contained bugs (low-risk) to learn the codebase. 
          I then assigned them to shadow my code reviews, explaining why I requested changes 
          (e.g., concurrency issues, testing best practices) rather than just what to change. 
          I dedicated an hour each day for code review and architectural whiteboarding.
      
         Result: Within three months, the junior was contributing independently to medium-sized features, 
           demonstrating a strong understanding of our thread-safety patterns, and effectively increasing the team's capacity.    
  </div>
  </details>
  <details>
    <summary>What do you do when you're assigned to a code base, you are not faimiliar with?</summary>
    <div class="preserve-format">

    My primary goal is to minimize risk while rapidly building context, following a "Top-Down, Inside-Out" approach:

    Top-Down (External Context):
    
    Identify the Edges: 
      Find the API documentation, main entry points (e.g., Controllers, Message Handlers), and deployment pipeline.
      
      Run the App/Tests: 
      Get it running locally and execute the full test suite. A working test suite is a living, 
      functional document of the system's intended behavior.
      
      Talk to Experts: 
      Schedule 1:1s with the domain expert or original author to get the "10,000-foot view"—the "why" 
      and key design decisions (the happy path).
    
    Inside-Out (Code Tracing):
    
      Trace the Happy Path: 
      Pick a simple, common use case (e.g., GET /user/{id}) and follow the execution flow using a debugger. 
      Trace it from the Controller,through the Service layer, to the Repository/Database. 
      This establishes the mental model.
      
      Code Coverage/Metrics: 
      Review APM/monitoring dashboards (CloudWatch, Prometheus) to see which parts of the code are 
       heavily used and which are error-prone. 
       Focus learning efforts on the most critical paths.
      
      Small, Contained Change: 
      Start with the first task being a low-risk bug fix or a minor refactoring. 
      This forces you to validate your understanding against a real-world change and build confidence quickly.
  </div>
  </details>
  <details>
    <summary>Explain about implementing audit log logging, data encryption and failover mechanism? How to set up ?</summary>
    <div class="preserve-format">

    These are foundational pillars of a robust financial services architecture.

    1. Audit Log Logging
    
    Purpose: To create an unalterable record of all security-relevant and business-critical actions 
    (who, what, when, where). Essential for compliance (e.g., PCI DSS, GDPR).
    
    Implementation:
    
    Point of Capture: Intercept actions at the Service Layer (just before state change) or 
    via Spring AOP to ensure every method call is logged.
    
    Data: Log must include: Timestamp, Actor ID, Action Type (e.g., USER_CREATE, ACCOUNT_UPDATE), Resource ID, 
    and before/after state (for critical changes).
    
    Storage: Send logs asynchronously to a dedicated, write-only, centralized platform (CloudWatch Logs, Splunk, ElasticSearch) 
    with strict access control and long-term retention policies.
    
    2. Data Encryption
    Purpose: Protect sensitive data (Data At Rest and Data In Transit) from unauthorized access.
    
    Implementation:
    
    Data In Transit (TLS/SSL): Enforce HTTPS/TLS 1.2+ for all communication (client ↔ Gateway, Gateway ↔ Microservice). 
    This is configured at the Load Balancer/API Gateway.
    
    Data At Rest (Database/S3):
    
    Databases: Use Transparent Data Encryption (TDE) provided by the RDBMS 
      (e.g., AWS RDS/Aurora) and encrypt disk volumes.
    
    S3: Enforce Server-Side Encryption (SSE) using AWS KMS (Key Management Service) or 
        S3-Managed Keys (SSE-S3) on all buckets storing sensitive files.
    
    Sensitive Fields (Field-Level): For critical PII (e.g., Credit Card Numbers), 
      encrypt the specific database column using an AES-256 algorithm, 
      storing the key securely in a Vault (HashiCorp, AWS Secrets Manager).
    
    3. Failover Mechanism
    Purpose: Ensure application availability by automatically switching to a backup system upon a primary system failure.
    
    Setup:
    
    Database: Use Active-Passive/Active-Active Replication (e.g., AWS RDS Multi-AZ or Aurora Global Database). 
    Set up automatic failover to the replica when the primary is unresponsive.
    
    Application Tier: Utilize Elastic Load Balancer (ELB/ALB) across multiple Availability Zones (AZs) with Auto Scaling Groups (ASG). 
    The ELB constantly checks health checks; if an entire AZ fails, traffic is automatically routed to the remaining healthy AZs.
    
    Disaster Recovery (DR): For catastrophic regional failure, implement a Regional Failover using AWS Route 53 to switch traffic to a 
    secondary region (e.g., switching from us-east-1 to us-west-2) based on health checks.

   </div>
  </details>
  <details>
    <summary>If one of your department services becomes slow, how would you protect your service from timeout ?</summary>
    <div class="preserve-format">

    I would implement the Circuit Breaker Pattern and enhance resource management 
    to gracefully handle the dependency's degradation.
      
    Circuit Breaker Pattern (The Primary Defense):
      
      Setup: Wrap all calls to the slow dependency with a Circuit Breaker library (e.g., Resilience4j).
      
      Function: If the dependency fails or exceeds a configured threshold 
      (e.g., 10 failures in 60 seconds or a 200ms latency threshold), 
      the breaker moves from Closed → Open.
      
      Protection: In the Open state, all subsequent calls fail immediately (Fast-Fail) 
      with an exception without waiting for the actual timeout. 
      This prevents my service's thread pool from becoming exhausted waiting for the slow dependency.
      
      Recovery: After a timeout period (e.g., 30 seconds),
      the breaker moves to Half-Open and allows a single test request. 
      If the test succeeds, it moves back to Closed.
      
    Fallback Mechanism:
      
      Provide a fallback method within the Circuit Breaker logic. 
      If the breaker is open, the fallback method executes, returning cached data, 
      a default value, or a standardized error response, 
      allowing the user experience to be degraded but not completely broken.
      
    Dedicated Thread Pools:
      
      Use Bulkhead Pattern (often implemented alongside the Circuit Breaker) by allocating a dedicated, 
      small thread pool for the slow dependency. 
      This prevents the slow dependency from consuming the entire service's thread pool, 
      ensuring other, healthy APIs remain responsive.
             
  </div>
  </details>
  <details>
    <summary>How do you implement secure file upload to S3?</summary>
    <div class="preserve-format">

    Secure file upload to S3 requires end-to-end security, not just encryption.
    
    1. Client-Side Preparation (The Secure Way):
    
      The client never uploads directly to S3 with its own credentials.
      
      The client makes a request to my secure backend API (e.g., /api/v1/upload-url).
      
      My backend service authenticates the user, generates a Pre-Signed URL using the AWS SDK, and sends it back to the client.
      
    2. Secured Upload (The Upload):
    
        The client uses the temporary Pre-Signed URL to upload the file directly to S3.
        
        The URL has a short expiration time (e.g., 5-15 minutes) and is restricted to a single action 
        (s3:PutObject) on a specific key path, drastically limiting the attack surface.
    
    3. S3 Bucket Configuration (The Enforcement):
    
      Block Public Access: Must be enabled on the bucket.
      
      Least Privilege IAM: The IAM role used by the backend service to generate the URL only has 
      s3:PutObject permission on the specific path (e.g., user/{userId}/*).
      
      Server-Side Encryption (SSE): Enforce SSE-KMS or SSE-S3 at the bucket policy level to ensure all data is encrypted at rest.
      
      Virus Scanning: Set up an S3 Event Notification to trigger a Lambda function on s3:ObjectCreated:* 
      to automatically scan the file for malware before it is processed by the main application logic. 
      The file remains in a "pending" or "quarantine" bucket until it is verified clean.
  </div>
</details>

    
<style>
.details-arrow {
    left: 16px !important;
    top: 42% !important;
    transition: revert;
    transform: rotate(317deg);
}
.details-wrapper details {
    border: 1px dotted var(--e360-base-light-gray, #ccc) !important;
    transition: border 0.3s ease;
    padding: 8px;
    margin-bottom: 12px;
    border-radius: 4px;
    background: #f9f9f9;
}
.details-wrapper details:hover {
    border: 1px solid var(--e360-base-light-gray, #aaa) !important;
}
.details-wrapper_rendered summary {
    left: 19px;
    position: relative;
    font-weight: 600;
    cursor: pointer;
    padding: 4px 0;
}
.details-wrapper details[open]+.details-arrow {
    transform: rotate(45deg);
}
details summary {
    outline: none;
}
details[open] summary {
    color: #3178c6;
}

/* Preserve whitespace and line breaks for answer paragraphs */
.preserve-format {
    white-space: pre-wrap;
    font-family: inherit;
    font-size: 1rem;
}
</style>


  </details>
</div>
