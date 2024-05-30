# ironhack-lab9

## Exercise: Monolithic app migration to micorservices architecture
 
### Monolithic E-Commerce Application Description:

The application is a traditional e-commerce platform that encompasses all functionalities within a single, unified software architecture. The application handles the following key operations:

**User Management:** Manages user profiles, authentication, and authorization. It stores personal information, manages login sessions, and handles user preferences.

**Product Catalog:** Maintains a comprehensive list of products, including descriptions, pricing, images, and inventory levels. It supports product search and categorization functionalities.

**Order Processing:** Manages all aspects of the ordering process, from cart management to order placement, payment processing, and order history tracking.

**Customer Support:** Handles customer inquiries, returns, complaints, and feedback through a ticket-based system integrated with the user and order databases.

The application is built on a single relational database that holds all user data, product information, orders, and customer support interactions. It currently operates on a single code base with a web-based frontend that communicates directly with the backend server.

The platform has been experiencing challenges with scaling during high-traffic periods, frequent downtimes during updates, and increasing difficulty in implementing new features without affecting existing functionalities. The goal is to decompose this monolithic architecture into a microservices-based architecture to address these issues and improve overall agility and scalability.

## Deliverables:

### Roadmap:

1. Assessment and Planning: 
	* Evaluate the existing monolithic application to understand its architecture, dependencies, and functionality.
	* Identify areas that would benefit from microservices decomposition, such as modules with high coupling or scalability constraints.
	* Define clear objectives and success criteria for the migration process.
	* Develop a detailed migration plan outlining the sequence of steps, resources required, and timelines
2. Decomposition:
	* Break down the monolithic application into smaller, more manageable components or services based on business functionality.
	* Identify boundaries for service decomposition, considering factors like domain boundaries, data ownership, and dependencies.
3. Service Identification:
	* Identify the core services that will form the foundation of the microservices architecture.
	* Prioritize services based on business impact and technical feasibility.
4. Data Management:
	* Decouple the data layer from the monolithic database.
	* Consider using separate databases for each microservice or adopting a polyglot persistence approach.
	* Implement data synchronization mechanisms between services as needed.
5. API Gateway and Communication:
	* Set up an API gateway to manage external communication with new microservices.
	* Define APIs for each service and ensure proper authentication and authorization.
	* Use lightweight communication mechanisms like HTTP or messaging queues for inter-service communication.
6. Deployment and Scaling:
	* Containerize the microservices using technologies like Docker.
	* Deploy services independently, allowing for easier scaling and updates.
	* Implement auto-scaling strategies to handle varying traffic loads.
7. Monitoring and Observability:
	* Set up monitoring tools to track the performance, health, and reliability of the microservices.
	* Implement centralized logging and distributed tracing.
	* Use tools like Prometheus, Grafana, and Jaeger.
8. Testing and Rollout:
	* Create comprehensive test suites for each microservice.
	* Perform integration testing to ensure seamless communication between services.
	* Gradually roll out microservices in production, starting with non-critical functionalities.
9. Continuous Integration and Deployment (CI/CD):
	* Implement CI/CD pipelines for automated builds, testing, and deployment.
	* Use tools like Jenkins, GitLab CI/CD, or GitHub Actions.
10. Security and Authorization:
	* Implement security measures such as OAuth, JWT, or API tokens.
	* Ensure proper authorization and access control for each service.

### Architecture design:

![Ecomerce-ironhack](https://github.com/gustavoleonh/ironhack-lab9/assets/116121540/bff7c234-7a51-45e7-ac36-ee78d551b2c8)

### Rational:

1.  Sync vs. Async Communication:

    -   Synchronous Communication:

        -   In synchronous communication, services directly call each other over HTTP or other protocols.
        -   **Pros**:
            -   **Simplicity**: Easier to implement and understand.
            -   **Immediate response**: The calling service receives a response right away.
        -   **Cons**:
            -   **Tight coupling**: Services are dependent on each other's availability.
            -   **Scalability challenges**: High load on one service can affect others.
        -   Use sync communication for low-latency interactions, such as user authentication or product search.
    -   Asynchronous Communication:

        -   In asynchronous communication, services communicate indirectly via message queues (SQS).
        -   **Pros**:
            -   **Loose coupling**: Services are decoupled, allowing independent scaling and fault tolerance.
            -   **Resilience**: Services can process messages at their own pace.
        -   **Cons**:
            -   **Complexity**: Requires additional infrastructure (message brokers).
            -   **Eventual consistency**: Data might not be immediately up-to-date.
        -   Use async communication for non-critical tasks like order processing or notifications.
2.  Separate Databases for Each Microservice:

    -   **Isolation and Autonomy**:
        -   Each microservice has its own database, ensuring data isolation.
        -   **Pros**:
            -   **Independence**: Services can evolve independently without affecting others.
            -   **Technology choice**: Different services can use databases optimized for their specific needs (e.g., NoSQL for product catalog, relational for orders).
        -   **Cons**:
            -   **Data consistency**: Requires careful synchronization between services.
            -   **Increased operational complexity**: Managing multiple databases.
        -   Use separate databases to avoid tight coupling and enable autonomy.
3.  Relational Database for Order Service:

    -   **Strong Consistency and Transactions**:
        -   Order processing involves critical operations (e.g., payment, inventory updates).
        -   **Pros**:
            -   **ACID transactions**: Relational databases provide strong consistency.
            -   **Complex queries**: Relational databases handle complex joins and aggregations.
        -   **Cons**:
            -   **Scalability challenges**: Relational databases can be harder to scale horizontally.
            -   **Schema changes**: Migrating schemas requires careful planning.
        -   Use a relational database for the order service to ensure data integrity and consistency.

In summary, the decisions aim to strike a balance between simplicity, scalability, and data consistency. Sync communication is suitable for real-time interactions, while async communication provides flexibility for background tasks. Separate databases allow services to evolve independently, and a relational database ensures transactional integrity for critical order-related operations.
