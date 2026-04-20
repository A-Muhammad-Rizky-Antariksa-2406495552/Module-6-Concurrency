## Commit 1 Reflection notes
Pada fungsi `handle_connection`, program bertugas untuk membaca dan memproses HTTP request yang masuk melalui koneksi TCP. 

Di sini digunakan `BufReader` untuk membaca data dari `TcpStream` secara efisien. Data dibaca baris demi baris menggunakan `.lines()`, kemudian dikumpulkan ke dalam sebuah Vector menggunakan `.collect()`. Proses pembacaan ini akan berhenti ketika menemui baris kosong (`take_while(|line| !line.is_empty())`), yang mana menandakan akhir dari HTTP request header.

## Commit 2 Reflection notes

Pada tahap ini, fungsi `handle_connection` telah diperbarui untuk memberikan respon balik berupa file HTML yang sebenarnya (`hello.html`), bukan sekadar teks mentah.
1.  **Pemisahan Response**
Saya membagi response menjadi tiga bagian utama: `status_line` (status sukses 200 OK), `contents` (isi dari file HTML), dan `length` (panjang dari konten tersebut).
2.  **Pembacaan File**
Menggunakan `fs::read_to_string("hello.html")` untuk mengambil seluruh isi file HTML. Penggunaan `.unwrap()` di sini berfungsi untuk menangani *error* jika file tidak ditemukan.
3.  **Format HTTP Response**
Respon disusun mengikuti protokol HTTP yang benar:
* Header diawali dengan status line.
* Diikuti dengan `Content-Length` agar browser tahu berapa banyak data yang harus dibaca.
* Diakhiri dengan isi file (`contents`) setelah pemisah baris kosong (`\r\n\r\n`).

### Cuplikan Layar (Screenshots):
But this is from my machine. You may need to edit the hello.html to write your own message.

![Commit 2 screen capture](/assets/images/commit2.png)

## Commit 3 Reflection notes
Pada Milestone 3, dilakukan penambahan validasi request untuk membedakan antara halaman utama dan halaman yang tidak ditemukan (404 Not Found), serta melakukan refactoring pada kode.

### Cara Memisahkan Response:
Alih-alih menggunakan `.collect()` untuk mengambil seluruh isi request, saya menggunakan `.next()` pada `buf_reader.lines()` untuk mengambil baris pertama saja (`request_line`). Baris ini berisi informasi metode HTTP dan path (misal: `GET / HTTP/1.1`). 

Dengan struktur `if...else`, program mengecek isi `request_line`:
- Jika isinya adalah request ke path `/`, maka program menetapkan status `200 OK` dan file `hello.html`.
- Jika isinya selain itu (seperti `/bad`), maka program menetapkan status `404 NOT FOUND` dan file `404.html`.

### Refactoring
Refactoring diperlukan untuk menerapkan prinsip **DRY (Don't Repeat Yourself)**. Sebelum direfaktorisasi, blok kode untuk membaca file dan mengirim respons (seperti `fs::read_to_string` dan `stream.write_all`) harus ditulis berulang kali di dalam setiap kondisi `if` dan `else`.

Beberapa keuntungan refactoring:
1. **Kode lebih modular**: Kita hanya menentukan bagian yang berbeda (status line dan nama file) di dalam blok logika.
2. **Maintenance lebih mudah**: Logika utama pengiriman respons hanya ada di satu tempat di akhir fungsi, sehingga jika ada perubahan protokol ke depannya, kita hanya perlu mengubah satu bagian saja.

### Cuplikan Layar (Commit 3 - 404 Page):
![Commit 3 screen capture](/assets/images/commit3.png)