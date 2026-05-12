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
![alt text](image-1.png)
Saat program subscriber dijalankan, ia akan membuka koneksi aktif ke message broker RabbitMQ untuk mendengarkan pesan. Ketika publisher dieksekusi, program tersebut mengirimkan 5 buah event berisi data pengguna ke broker, yang kemudian langsung diterima dan diproses oleh subscriber secara asinkron. Hal ini membuktikan bahwa komunikasi antar layanan melalui message bus telah berhasil terhubung dan berfungsi dengan baik.
![alt text](image-3.png)
Spike yang terlihat pada grafik terjadi setiap kali program publisher dijalankan. Hal ini menunjukkan adanya aktivitas pengiriman pesan dari publisher ke message broker secara real-time. Ketika pesan-pesan tersebut masuk ke dalam antrean dan kemudian diproses oleh subscriber, grafik akan mencatat kenaikan laju pesan (publish dan deliver) yang memvalidasi bahwa alur data sedang berjalan aktif melalui RabbitMQ.
![alt text](image-4.png)
Ketika subscriber diberikan delay, pesan-pesan yang dikirim oleh publisher secara cepat tidak bisa langsung diproses semuanya, sehingga menumpuk di dalam antrean (queue). Hal ini menyebabkan jumlah "Queued Messages" pada grafik meningkat tajam. Pesan-pesan tersebut akan tetap berada di antrean sampai subscriber selesai memprosesnya satu per satu secara perlahan.