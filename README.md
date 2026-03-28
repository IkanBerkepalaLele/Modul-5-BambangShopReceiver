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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create SubscriberRequest model struct.`
    -   [x] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [x] Commit: `Implement add function in Notification repository.`
    -   [x] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Commit: `Implement receive_notification function in Notification service.`
    -   [x] Commit: `Implement receive function in Notification controller.`
    -   [x] Commit: `Implement list_messages function in Notification service.`
    -   [x] Commit: `Implement list function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
- Menurut saya, RwLock digunakan agar notifikasi bisa dibaca secara bersamaan oleh banyak thread, kenapa digunakan dibandingkan mutex? karena ketentuan rwlock yang dimana bisa banyak reader namun hanya ada 1 writer (server) dan saat ada yang read, tidak boleh write dan vice versa, reader disini adalah receiver agar notifikasi bisa dilakukan secara bersamaan. Kalau digunakan dengan mutex, proses notifikasi akan lebih lama, karena hanya ada 1 thread yang boleh mengakses vector tersebut. 

- Karena rust lebih ketat pada memory safety. Java bisa melakukan hal tersebut karena aturannya tidak seketat rust, dimana java lebih membebaskan kepada developer. Maka rust memberikan fungsi bawaan agar mencegah data race dan lebih pointer safety serta aman secara memory. 

#### Reflection Subscriber-2
- Ya, lib.rs dipakai untuk menyimpang shared library dan setting global yang dipakai oleh aplikasi. cargo.toml dipakai untuk list dependency apa saja yang dipakai, main.rs untuk inisialisasi aplikasi, rocket.toml mirip seperti application.properties di spring untuk set port dan set profile. dan folder target seperti folder build pada gradle.

- Observer memudahkan penambahan subscriber karena jika ingin menambah, tinggal taro di vector atau hashmap saja, lalu aplikasi akan mengakses vector tersebut dan notify setiap subscriber. Lalu endpoint receiver semua sama, tidak perlu mengubah kode lagi. Jika yang ditambah instance main masih tetap mudah, namun setiap instance memiliki subscriber masing2 karena disimpan dalam vector, tetapi ada opsi lain yaitu memakai database agar setiap instance mengakses database static tersebut dan pengiriman notifikasi bisa dilakukan oleh setiap instance main bersama2.

- Menurut saya fitur test tersebut bisa berguna untuk TK, saya bisa bayangkan test untuk setiap api yang ada di modul saya atau 1 app secara keseluruhan, dimana setelah saya tambahkan fitur baru, saya cek apakah api yang sudah ada masih berfungsi atau tidak. Lalu documentation bisa berguna saat ingin membuat front-end dan menganalisis bagaimana modul teman sekelompok bekerja.