# BambangShop Receiver App
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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [√] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [√] Commit: `Create Notification model struct.`
    -   [√] Commit: `Create SubscriberRequest model struct.`
    -   [√] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [√] Commit: `Implement add function in Notification repository.`
    -   [√] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [√] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [√] Commit: `Create Notification service struct skeleton.`
    -   [√] Commit: `Implement subscribe function in Notification service.`
    -   [√] Commit: `Implement subscribe function in Notification controller.`
    -   [√] Commit: `Implement unsubscribe function in Notification service.`
    -   [√] Commit: `Implement unsubscribe function in Notification controller.`
    -   [√] Commit: `Implement receive_notification function in Notification service.`
    -   [√] Commit: `Implement receive function in Notification controller.`
    -   [√] Commit: `Implement list_messages function in Notification service.`
    -   [√] Commit: `Implement list function in Notification controller.`
    -   [√] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

1. RwLock<> is necessary in this case because we have multiple threads that need to access the Vec<Notification> concurrently, with some threads needing read access and others needing write access. RwLock allows multiple readers to access the data simultaneously while ensuring exclusive access for writers, which is more efficient than Mutex<> that allows only one thread to access the data at a time regardless of whether it's reading or writing. Since our notification system likely has more read operations (listing notifications) than write operations (adding notifications), RwLock provides better performance by allowing concurrent reads.

2. In Rust, static variables are required to have a fixed size known at compile time and must be initialized with a constant expression. Unlike Java, Rust enforces memory safety and thread safety at compile time through its ownership system. Allowing direct mutation of static variables would violate these safety guarantees since multiple threads could modify the static data simultaneously, leading to data races. This is why we need to use synchronization primitives like lazy_static combined with RwLock or Mutex to safely mutate static data - it ensures proper synchronization and thread safety at runtime while maintaining Rust's compile-time safety guarantees.

#### Reflection Subscriber-2

1. Yes, I explored src/lib.rs and other parts of the codebase beyond the tutorial steps. From src/lib.rs, I learned about how the application handles errors through the custom Result type and AppError enum. This helped me understand Rust's error handling patterns better. I also explored how the different modules are organized and exported through the lib.rs file, which taught me about Rust's module system and visibility rules.

2. The Observer pattern makes it very easy to add new subscribers (Receiver instances) to the system. Each subscriber just needs to register itself by calling the subscribe endpoint with its product type, and it will automatically start receiving relevant notifications. The publisher doesn't need to know about or manage individual subscribers - it just broadcasts notifications and the pattern handles delivery. For multiple Main app instances, it would still be relatively straightforward since each instance would maintain its own set of subscribers independently. However, you would need to ensure proper synchronization between Main instances if they need to share subscriber state.

3. Yes, I enhanced the Postman collection documentation by adding detailed descriptions for each endpoint, including expected request/response formats and example payloads. I also added environment variables to easily switch between different receiver instances. This made testing much easier as I could quickly verify behavior across multiple receivers. Additionally, I wrote some basic integration tests using Rust's testing framework to verify the notification flow works correctly. Having these tests helped catch issues early and will make it easier to maintain the code.
