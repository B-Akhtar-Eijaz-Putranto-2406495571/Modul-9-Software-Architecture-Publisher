## Refleksi

1. Berapa banyak data yang dikirim oleh program publisher dalam satu kali jalan?
Jika melihat pada fungsi main() di file main.rs (atau publisher/src/main.rs), program kamu melakukan pemanggilan fungsi p.publish_event sebanyak 5 kali.

Setiap pemanggilan mengirimkan satu instance struct UserCreatedEventMessage dengan data yang berbeda (Amir, Budi, Cica, Dira, dan Emir). Jadi, dalam satu kali jalan (cargo run), program publisher akan mengirimkan 5 pesan ke message broker.

2. Apa artinya URL amqp://guest:guest@localhost:5672 sama antara program publisher dan subscriber?
Artinya kedua program tersebut (baik yang mengirim pesan maupun yang menerima pesan) terhubung ke instance atau server Message Broker yang sama. Dalam arsitektur Message-Oriented Middleware:
    - Publisher perlu tahu ke mana harus mengirim pesan.
    - Subscriber perlu tahu dari mana harus mengambil atau mendengarkan pesan.

    Karena keduanya menggunakan URL yang sama, mereka "bertemu" di broker yang sama (dalam hal ini RabbitMQ yang berjalan secara lokal di komputer kamu). Jika URL-nya berbeda (misalnya host atau port-nya beda), maka subscriber tidak akan pernah menerima pesan yang dikirim oleh publisher karena mereka berada di jalur komunikasi yang berbeda.

![alt text](image.png)