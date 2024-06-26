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
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. In the observer pattern, Subscriber is typically an interface or trait for flexibility with multiple implementations. In this case, using a single struct suffices if we do not anticipate different subscriber behaviors. It simplifies design but consider adding an interface if future requirements might introduce varies subscriber types.

2. If the list of subscribers is small and speed isn't a priority, a Vec could suffice. However, as the list grows and we prioritize quick lookups and updates, DashMap shines with its efficient performance.

3. DashMap effortlessly manages concurrent access across multiple threads, which is incredibly beneficial in Rust's concurrency model. While Singleton could be an option, its primary focus is on restricting instances rather than handling thread safety. Unless there's a compelling need for a single instance, DashMap remains the preferred choice in Rust, ensuring thread safety without the added complexity.

#### Reflection Publisher-2
1. Splitting Services and Repositories from the Model in MVC is crucial for maintaining, scaling, and testing our code effectively. This separation ensures adherence to key design principles like the Single Responsibility Principle (SRP), Separation of Concerns (SoC), and Don't Repeat Yourself (DRY). By organizing our code this way, we make it easier to manage, scale, and test. It simplifies maintenance and accommodates changes as our system becomes more complex.

2. Depending solely on the Model can overload it with various tasks, making the code complex and harder to maintain. However, if we distribute responsibilities and focus each model on its specific tasks, we can better manage complexity and maintain a more organized codebase. Interactions between models like Program, Subscriber, and Notification can further complicate code, but efficient management helps mitigate this complexity.

3. Postman makes testing API endpoints easier. It helps check if programs work as expected by organizing endpoint calls. Its best feature is grouping endpoints into collections, which makes testing simpler and lets us automate tests.

#### Reflection Publisher-3
1. In this tutorial, we use the Push model variation of the Observer Pattern. This occurs because in the tutorial, the program publisher proactively dispatches notifications to subscribers whenever there is a change in the program's status.

2. Using the Pull model from the Observer Pattern involves subscribers actively requesting data from the publisher for updates. This model offers advantages such as resource efficiency, ensuring subscribers only update information when necessary, and providing greater control over data updates. However, implementing the Pull model introduces complexity due to managing request and response exchanges between the parties. Additionally, there's a risk of outdated information if subscribers don't regularly update their data, potentially leading to discrepancies between their data and the latest information from the publisher.

3. Without multi-threading, notifications are sent sequentially, potentially causing delays, especially with many subscribers or slow responders. This approach can also slow down or freeze the application if subscribers take time processing notifications.