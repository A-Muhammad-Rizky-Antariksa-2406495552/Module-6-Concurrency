## Commit 1 Reflection notes
Pada fungsi `handle_connection`, program bertugas untuk membaca dan memproses HTTP request yang masuk melalui koneksi TCP. 

Di sini digunakan `BufReader` untuk membaca data dari `TcpStream` secara efisien. Data dibaca baris demi baris menggunakan `.lines()`, kemudian dikumpulkan ke dalam sebuah Vector menggunakan `.collect()`. Proses pembacaan ini akan berhenti ketika menemui baris kosong (`take_while(|line| !line.is_empty())`), yang mana menandakan akhir dari HTTP request header.