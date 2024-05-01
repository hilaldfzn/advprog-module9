# **Rust Tutorial & Exercise**
**Muhammad Hilal Darul Fauzan**<br/>
**2206830542**<br/>
**Pemrograman Lanjut C**<br/>

## **Tutorial Modul 9: High-Level Networking**

### Reflection

1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?
    - *Unary RPC methods* melibatkan satu request dan satu response. Metode ini cocok untuk operasi sederhana yang dilakukan sekali saja, di mana client mengirim request dan menunggu response.
    - *Server streaming RPC methods* melibatkan satu request dan beberapa response. Metode ini cocok untuk skenario di mana client perlu menerima aliran data dari server, seperti *real-time updates* atau *data feeds*.
    - *Bi-directional streaming RPC methods* melibatkan beberapa request dan beberapa response. Metode ini cocok untuk skenario di mana baik client maupun server perlu mengirim dan menerima aliran data secara bersamaan, seperti aplikasi chat atau *collaborative editing*.
    
2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
    - *Authentication*: Penting untuk memastikan bahwa hanya client yang berwenang yang dapat mengakses service. Saat mengimplementasikan gRPC service dalam Rust, autentikasi dapat diimplementasikan menggunakan berbagai mekanisme, seperti JWT (JSON Web Tokens), OAuth, atau *custom authentication schemes*.

    - *Authorization*: Setelah client diautentikasi, otorisasi dapat diimplementasikan untuk mengontrol akses ke resources atau operasi tertentu dalam gRPC service. Ini dapat dilakukan menggunakan *role-based access control* (RBAC), *attribute-based access control* (ABAC), atau mekanisme otorisasi lainnya.

    - *Data Encryption*: Untuk memastikan kerahasiaan dan integritas data yang ditransmisikan melalui jaringan, disarankan untuk menggunakan Transport Layer Security (TLS) encryption untuk gRPC communication. Ini dapat dicapai dengan mengonfigurasi server dan client gRPC untuk menggunakan TLS certificates.

3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?
    - Tantangan utamanya meliputi pengelolaan *lifetime* dari aliran data, penanganan error dan exception, serta memastikan keamanan thread. Dalam aplikasi chat, kompleksitas tambahan dapat muncul dari pengelolaan beberapa aliran data yang bersamaan, pemeliharaan urutan pesan, serta penanganan disconnects atau timeouts.

4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?
    - Keuntungannya adalah metode ini menyediakan cara yang baik untuk mengubah `tokio::sync::mpsc::Receiver` menjadi `Stream`, yang dapat digunakan untuk *streaming responses* dalam gRPC. Kerugiannya adalah metode ini memerlukan `Receiver` yang mendasarinya agar tetap valid selama *lifetime* `Stream`, yang dapat mempersulit pengelolaan *lifetime*.

5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?
    - Untuk memfasilitasi penggunaan ulang kode dan modularitas, kita dapat mendefinisikan *common functionality* dalam *module* atau *shared library*, menggunakan traits untuk mendefinisikan *common interface*, dan menggunakan generics untuk menulis kode yang dapat bekerja dengan berbagai tipe. kita juga dapat menggunakan *builder pattern* untuk membuat objek yang kompleks, dan *strategy pattern* untuk mengubah behavior pada saat runtime.

6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?
    - Langkah tambahannya mungkin mencakup validasi informasi pembayaran, memeriksa ketersediaan dana, memproses pembayaran melalui payment gateway, menangani potensi error atau exception, serta mengirimkan notifikasi konfirmasi atau kegagalan.

7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?
    - *The adoption of gRPC* dapat mengarah pada komunikasi yang lebih efisien karena penggunaannya terhadap HTTP/2 dan Protocol Buffers. Ini juga mendukung beberapa bahasa pemrograman, yang dapat meningkatkan interoperabilitas. Namun, ini mungkin memerlukan perubahan pada infrastruktur dan tools yang ada, dan mungkin tidak mudah untuk di-debug atau di-inspect seperti HTTP/1.1-based protocols.

8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?
    - HTTP/2 memberikan keuntungan seperti multiplexing, header compression, dan server push, yang dapat meningkatkan kinerja dan efisiensi. Namun, mungkin tidak semua client atau server mendukung sepenuhnya. WebSocket menyediakan komunikasi full-duplex, yang berguna untuk aplikasi real-time, tetapi memerlukan koneksi terpisah dan tidak mendukung fitur seperti multiplexing atau server push.

9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?
    - REST APIs menggunakan request-response model yang sederhana dan tidak menyimpan state, namun mungkin tidak ideal untuk real-time communication karena overhead dari HTTP request. gRPC mendukung bidirectional streaming, yang memungkinkan real-time, full-duplex communication, tetapi mungkin lebih kompleks untuk diimplementasikan dan dikelola.

10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?
    - Implikasi dari *schema-based approach of gRPC* yang menggunakan Protocol Buffers dibandingkan dengan yang lebih fleksibel dan *schema-less nature* dari JSON dalam REST API payloads adalah sebagai berikut:
        * Strong Typing: Protocol Buffers menerapkan strict schema, yang berarti struktur dan tipe data sudah ditentukan sebelumnya. Ini menyediakan strong typing dan membantu menangkap error pada saat compile, memastikan konsistensi data dan mengurangi error saat runtime. Sebaliknya, JSON memberikan lebih banyak fleksibilitas dalam mendefinisikan struktur dan tipe data, tetapi ini bisa menyebabkan ketidakkonsistenan dan error saat runtime.

        * Efficiency: Protocol Buffers menggunakan binary serialization format, yang lebih ringkas dan efisien dibandingkan dengan format berbasis teks JSON. Ini menghasilkan ukuran pesan yang lebih kecil dan serialisasi/deserialisasi yang lebih cepat, membuat gRPC lebih efisien dalam hal network bandwidth dan processing speed.

        * Code Generation: Protocol Buffers memerlukan langkah compile terpisah untuk menghasilkan kode untuk message types dan service interfaces. Proses code generation ini menyediakan strongly-typed APIs dalam berbagai bahasa pemrograman, memudahkan dalam bekerja dengan gRPC services. Sebaliknya, JSON tidak memerlukan code generation dan dapat langsung diparse dan dimanipulasi menggunakan built-in language features atau libraries.

        * Versioning and Evolution: Protocol Buffers mendukung versioning and evolution skema melalui penggunaan message and field-level options. Hal ini memungkinkan backward and forward compatibility saat melakukan perubahan pada API. Di sisi lain, JSON tidak memiliki dukungan bawaan untuk versioning and evolution, yang dapat membuatnya lebih sulit untuk menangani perubahan dalam API dari waktu ke waktu.

        * Tooling and Ecosystem: Protocol Buffers memiliki ecosystem of tools and libraries yang kaya yang menyediakan fitur tambahan seperti validasi, documentation generation, dan pemeriksaan kompatibilitas. JSON juga memiliki berbagai rangkaian tooling and libraries, tetapi ekosistem untuk Protocol Buffers lebih baik dan luas.

    - Secara keseluruhan, *schema-based approach of gRPC* menggunakan Protocol Buffers memberikan keuntungan dalam hal strong typing, efficiency, code generation, versioning, dan tooling. Namun, ini juga memperkenalkan beberapa kompleksitas tambahan dibandingkan dengan yang lebih fleksibel dari JSON dalam REST API payloads. Pilihan antara gRPC dan REST bergantung pada kebutuhan spesifik dan trade-off dari project tersebut.