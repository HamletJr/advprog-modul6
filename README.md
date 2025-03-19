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

## Commit 2 Reflection Notes
![Commit 2 screen capture](assets/images/commit2.png)
Berikut adalah hasil tangkapan layar untuk *commit* ke-2.

Saat ini, isi dari fungsi `handle_connection()` adalah:
```rs
fn handle_connection(mut stream: TcpStream) {
    let buf_reader = BufReader::new(&mut stream);
    let http_request: Vec<_> = buf_reader
        .lines()
        .map(|result| result.unwrap())
        .take_while(|line| !line.is_empty())
        .collect();

    let status_line = "HTTP/1.1 200 OK";
    let contents = fs::read_to_string("hello.html").unwrap();
    let length = contents.len();

    let response =
        format!("{status_line}\r\nContent-Length: {length}\r\n\r\n{contents}");
        
    stream.write_all(response.as_bytes()).unwrap();
}
```
Baris-baris di atas masih sama dengan sebelumnya, tetapi sekarang ada baris-baris baru di bawah. Yang pertama, kita membaut variabel baru yaitu `status_line` yang berisi baris *status* untuk response HTTP. Kemudian, terdapat `contents`, yaitu variabel yang berisi isi dari response HTTP kita, yaitu file `hello.html`. File tersebut akan di-*parse* ke *string* menggunakan `fs::read_to_string()` dan di-*unwrap* lagi. Terakhir, panjang dari *string* tersebut akan dihitung dan disimpan dalam `length`. Ini akan berguna nanti untuk baris terakhir, yaitu `response`, yaitu HTTP response kita. Kita menggunakan *macro* `format!()` dan menaruh variabel `status_line` untuk *status response* HTTP kita, kemudian informasi `Content-Length` diisi dengan `length`, yaitu panjang dari *string* yang menjadi isi *response* kita, dan terakhir kita memasukkan isi *response* kita, yaitu *file* HTML yang sudah diubah menjadi *string*. *Response* ini akan ditulis ke `TcpStream` dan akhirnya diterima oleh *browser* kita.