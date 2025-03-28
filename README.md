# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

> In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

In the case of BambangShop, we do not need an interface or trait for the Subscriber because there is only a single observer involved, which is the Subscriber itself. Typically, an interface or trait is useful when we plan to have multiple types of observers, allowing them to be managed in separate classes. However, in this case, a single Model struct is sufficient to handle our requirements.

That being said, from a best practice perspective, defining the Subscriber as an interface can provide more flexibility in the future, ensuring better scalability and adherence to SOLID principles. By using an interface, we can easily add new subscribers without modifying existing code, making the system more maintainable and reducing dependencies.

> id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

In this case, using DashMap is necessary and more efficient than Vec for storing Subscriber data with unique IDs and URLs because DashMap provides better performance. If we use Vec, each lookup operation would have a time complexity of O(n), whereas DashMap allows lookups with O(1) complexity. This means that lookup time remains constant regardless of the number of Subscribers. As a result, DashMap not only speeds up the search process but also makes managing Subscriber data more effective and scalable, making it the better choice in this case.

> When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

In this case, while the Singleton pattern ensures that only one instance of an object exists, it is not sufficient to handle safe concurrent access in a multithreading context. The Singleton pattern can guarantee that only one instance of the HashMap is used, but it does not ensure safe access by multiple threads simultaneously, which could lead to race conditions.

Therefore, using DashMap is essential because it supports concurrent access, ensuring that multiple threads can safely read and write data. In this case, DashMap and the Singleton pattern complement each other—Singleton ensures that only one instance exists, while DashMap guarantees thread-safe concurrency. Thus, they are not mutually exclusive but rather work together to achieve both uniqueness and safe access.

#### Reflection Publisher-2

> In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

Separating Service and Repository from the Model in the Model-View-Controller (MVC) pattern is important to follow design principles such as the Single Responsibility Principle (SRP). In MVC, the Model is responsible for representing data, while the Repository handles data access and storage, and the Service manages business logic.

By separating Service and Repository from the Model, our code becomes more maintainable and easier to understand because each component has a clear and distinct responsibility. This separation also improves testability, allowing each component to be tested independently. Additionally, it enhances flexibility and scalability, as changes to one part of the system do not directly affect the others, making modifications and future development more manageable.

> What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

If we only use the Model without separating it into Service and Repository, we will face dependency issues between components in the system. The Model would be responsible for both data storage and business logic, violating the Single Responsibility Principle (SRP).

This would make the code harder to maintain because any changes to one part could significantly affect other parts, increasing overall code complexity. Additionally, tightly coupling data management with business logic would make testing more difficult and slow down further development, as modifications could introduce unintended side effects across multiple models such as Program, Subscriber, and Notification.

> Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

I have explored Postman, and I find it to be a very useful tool for testing web application endpoints using HTTP requests. With Postman, I can test endpoints without writing code, which makes it much easier to quickly verify the functionality of my application.

Some of the features that I find particularly helpful include the ability to save requests and responses, control request headers and body, and manage cookies, which is very useful when testing authentication and session-based APIs. Additionally, the API documentation feature is extremely valuable as it allows me to easily share API documentation with my teammates.

This tool will be highly beneficial for my future software engineering projects as it enables efficient testing and facilitates collaboration with my team.

#### Reflection Publisher-3