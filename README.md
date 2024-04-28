## Reflection Module 9
**Syauqi Armanaya Syaki**<br>
**2206829010**<br>
**Pemrograman Lanjut C**<br>

1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?

    * Unary RPC: <br>
    Pada Unary RPC, klien mengirimkan permintaan ke server dan menunggu tanggapan dari server. Metode ini sederhana dan sinkron karena klien menunggu sampai server menyelesaikan pemrosesan. Cocok digunakan ketika klien hanya memerlukan respons tunggal dari server dan tidak ada kebutuhan untuk streaming data atau interaksi berkelanjutan. Contohnya adalah Payment Service.
    * Server Streaming RPC: <br>
    Dalam Server Streaming RPC, klien mengirimkan permintaan ke server dan menerima respons berupa stream data yang terus berlangsung dari server, sehingga memungkinkan server untuk mengirimkan sejumlah besar data yang mungkin diperlukan klien secara bertahap. Cocok digunakan ketika klien membutuhkan data dalam jumlah besar dari server tanpa harus menunggu hingga semua data tersedia sekaligus. Contohnya adalah Transaction Service.
    * Bi-directional Streaming RPC: <br>
    Pada Bi-Directional Streaming RPC, klien dan server saling mengirimkan stream data secara bersamaan. Baik klien maupun server dapat mengirim dan menerima data tanpa harus menunggu respons satu sama lain. Cocok digunakan ketika ada kebutuhan untuk komunikasi dua arah antara klien dan server, dan keduanya perlu mengirim dan menerima data secara independen. Contohnya adalah Chat Service.

2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?

    Dalam implementasi layanan gRPC di Rust, keamanan dari tahap autentikasi hingga enkripsi data sangatlah penting. Tahap autentikasi adalah langkah pertama untuk menentukan akses ke sistem, baik melalui token JWT atau integrasi dengan OAuth2. Otorisasi yang ketat diperlukan untuk mengontrol akses pengguna terhadap sumber daya dan bisa menggunakan middleware untuk implementasinya. Selain itu, enkripsi data melalui TLS juga krusial dalam mengamankan komunikasi antara klien dan server, mencegah penyadapan data, dan menjaga kerahasiaan informasi yang ditransmisikan.

3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?

    Dalam menghadapi bidirectional streaming di Rust gRPC, terutama dalam konteks aplikasi chat, berbagai tantangan menanti. Pengelolaan memory dan asinkronisitas menjadi fokus utama, memerlukan perhatian khusus terhadap pengelolaan kepemilikan data dan siklus hidupnya untuk mencegah kebocoran memori atau race conditions. Selain itu, menjaga keamanan data sensitif seperti pesan pribadi melalui enkripsi dan autentikasi merupakan keharusan, mengingat transmisi yang terus-menerus. Tentunya, mengelola koneksi secara efisien dan menangani aliran pesan dengan concurrency menjadi tantangan lainnya, memastikan server dapat menangani banyak pesan tanpa beban berlebih. Sinkronisasi pesan untuk memastikan urutan dan konsistensi dalam chat real-time juga menjadi prioritas dalam membangun aplikasi yang solid.

4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?

    Keunggulan:
    * Terintegrasi dengan ekosistem Tokio.
    * Data dikelola dengan asynchronous.

    Kekurangan:
    * Menambah kompleksitas saat mengelola.
    * Berpotensi overhead saat traffic tinggi.
    * Ketergantungan dengan Tokio, karena ReceiverStream berasal dari Tokio.

5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?

    Dalam pengembangan gRPC di Rust, strategi modularitas dan penggunaan ulang kode dapat digunakan dengan memisahkan fungsi dan layanan gRPC ke dalam modul yang berbeda, seperti layanan gRPC `services.proto`, implementasi server `grpc_server.rs`, dan klien `grpc_client.rs` yang memungkinkan manajemen kode yang lebih baik dan penggunaan kembali yang efisien. Selain itu, menggunakan trait untuk mendefinisikan fungsionalitas yang dapat dibagi antar komponen dapat meningkatkan fleksibilitas dan konfigurasi ulang. Teknik lainnya termasuk mengadopsi library eksternal untuk tugas-tugas umum guna meningkatkan konsistensi dan mengurangi duplikasi. Dengan demikian, implementasi gRPC di Rust akan menjadi lebih modular, mudah dipelihara, dan dapat diperluas secara efisien.

6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?

    Implementasi MyPaymentService untuk pemrosesan pembayaran yang lebih kompleks memerlukan langkah-langkah tambahan yang cukup penting. Pertama, validasi data pada PaymentRequest untuk memastikan keamanan dan kelengkapan data sebelum diproses. Selain itu, integrasi yang aman dengan gateway pembayaran menjadi kunci untuk memproses transaksi dengan lancar. Terakhir, penggunaan server streaming lebih disarankan daripada unary untuk mendukung proses transaksi dalam jumlah besar secara bersamaan, memastikan efisiensi dan kehandalan dalam transfer data yang terus-menerus.

7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?

    Penggunaan gRPC dalam sistem terdistribusi memberikan dampak yang signifikan pada arsitektur dan desain sistem. Dibandingkan dengan pendekatan REST yang memerlukan konfigurasi manual untuk metode HTTP, gRPC menyederhanakan interaksi antarmodul melalui definisi yang jelas dalam file proto. Dengan berbasis HTTP/2, gRPC memungkinkan komunikasi yang efisien dan cepat antar layanan dengan overhead rendah dan kemampuan streaming yang baik. Dengan demikian, sistem yang menggunakan gRPC dapat berkomunikasi lebih efektif, efisien, meningkatkan kinerja, dan memudahkan integrasi antar komponen sistem.

8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?

    Keunggulan:
    * Multiplexing dalam HTTP/2 memungkinkan pengelolaan multiple requests dan responses melalui satu koneksi saja, sehingga dapat mengurangi latency dan meningkatkan performa aplikasi.
    * Fitur prioritas request dan server push mempercepat waktu pemuatan halaman dengan mengizinkan pengiriman sumber daya sebelum diminta oleh klien.

    Kekurangan:
    * Implementasi HTTP/2 lebih kompleks dan membutuhkan enkripsi SSL/TLS yang menambah overhead biaya.
    * Meskipun WebSocket di HTTP/1.1 memungkinkan komunikasi dua arah dan real-time yang efektif, penggunaannya bisa lebih sederhana dalam situasi tertentu dibandingkan dengan HTTP/2. Migrasi ke HTTP/2 mungkin memerlukan pertimbangan terkait dengan kompatibilitas dan biaya overhead yang lebih tinggi.

9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?

    Dua pendekatan berbeda digunakan dalam REST APIs dan bidirectional streaming di gRPC untuk memfasilitasi komunikasi real-time dan responsivitas. Model request-respons yang digunakan oleh REST APIs, terutama dalam konteks HTTP/1.1, cenderung membatasi interaksi di mana klien mengirim sebuah request dan menunggu sebuah response dari server, sehingga memerlukan koneksi baru untuk setiap respons yang menyebabkan latensi yang lebih tinggi. Sebaliknya, gRPC menggunakan HTTP/2 yang mendukung bidirectional streaming sehingga memungkinkan pertukaran data yang kontinu antara klien dan server dalam satu koneksi yang terbuka dan latensi yang berkurang. Sehingga dapat meningkatkan efisiensi komunikasi dan cocok untuk aplikasi yang memerlukan respons real-time seperti aplikasi chat atau pemantauan status. Dengan demikian, gRPC menawarkan pendekatan yang lebih responsif dan efisien dalam menangani komunikasi real-time dibandingkan dengan REST APIs.

10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?

    Pendekatan berbasis skema dalam gRPC dengan Protocol Buffers menawarkan kelebihan yang signifikan dibandingkan dengan pendekatan tanpa skema yang lebih fleksibel dari JSON dalam payload REST API nya. Protocol Buffers di gRPC secara otomatis memvalidasi dan mengoptimisasi data, serta membantu mengurangi penggunaan bandwidth dan meningkatkan kecepatan pemrosesan data yang sangat berguna untuk aplikasi dengan kebutuhan efisiensi tinggi dan format data yang tetap atau konsisten. Di sisi lain, JSON dalam REST API yang tidak perlu skema memberikan fleksibilitas yang lebih besar dalam penanganan data yang berubah-ubah. Meskipun fleksibel, JSON bisa lebih lambat dan memakan lebih banyak sumber daya karena perlu melakukan parsing setiap kali data diterima atau dikirim, serta dapat meningkatkan risiko kesalahan runtime dan interpretasi data yang tidak konsisten.