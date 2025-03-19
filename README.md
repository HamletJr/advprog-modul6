# Module 6
**Nama:**   Joshua Montolalu<br>
**NPM:**    2306275746<br>
**Kelas:**  Pengjut A<br>

## Commit 1 Reflection Notes
Saat ini, isi dari fungsi `handle_connection()` adalah sebagai berikut:
```rs
fn handle_connection(mut stream: TcpStream) {
    let buf_reader = BufReader::new(&mut stream);
    let http_request: Vec<_> = buf_reader
        .lines()
        .map(|result| result.unwrap())
        .take_while(|line| !line.is_empty())
        .collect();
    println!("Request: {:#?}", http_request);
}
```
Fungsi ini menerima 1 argumen yaitu `TcpStream`, sebuah *stream* yang mengandung data untuk protokol TCP. Dalam kasus ini, *stream* tersebut akan berisi data yang diterima server kita, yaitu request dari *client*. Pertama, fungsi `handle_connection` membuat sebuah `BufReader` baru (*buffer*) untuk membaca isi dari `TcpStream`. Kemudian, fungsi ini mengambil *iterator* dari `buffer` dengan *method* `.lines()` yang akan memisahkan data per baris baru (`\n`). Untuk mendapatkan setiap data *string* tersebut, kita menggunakan `.map()` untuk melakukan `.unwrap()` pada setiap baris tersebut (yang berupa tipe `Result<String, std::io::Error>`). 

*Method* `.unwrap()` akan mengembalikan string jika hasilnya valid, atau membuat program kita *panic* (sama halnya dengan *exception* dalam Java) jika ada masalah dalam membaca data. Method `.take_while(|line| !line.is_empty())` memastikan bahwa *iterator* akan terus dipanggil sampai predikat bernilai *false*, yaitu ketika baris sudah kosong. Terakhir, method `.collect()` mengubah hasil *iterator* menjadi sebuah *collection*, yaitu `Vec<_>`. Hasil ini kemudian akan di-*print* ke terminal, sehingga kita mendapatkan hasilnya, yaitu data dari protokol TCP yang diterima oleh server kita.