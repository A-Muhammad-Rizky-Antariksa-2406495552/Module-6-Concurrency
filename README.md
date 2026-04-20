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