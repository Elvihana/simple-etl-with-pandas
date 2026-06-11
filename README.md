# Simple ETL Project with Pandas

Proyek ini mendemonstrasikan implementasi sederhana dari proses **ETL (Extract, Transform, Load)** menggunakan library Python Python Pandas. Dokumentasi ini dirancang untuk membersihkan dan memformat data peserta kompetisi agar siap digunakan oleh tim logistik dan analisis lanjutan.

## Latar Belakang
Pada sistem registrasi kompetisi, data mentah yang masuk sering kali memiliki format yang tidak seragam (seperti nomor telepon dengan gaya penulisan berbeda) atau kekurangan informasi spesifik yang dibutuhkan oleh tim operasional (seperti kode pos untuk pengiriman logistik). Oleh karena itu, diperlukan proses ETL untuk menyaring, mentransformasikan, dan merapikan data tersebut agar valid dan konsisten.

## Tujuan Proyek
1. Mengekstrak data mentah peserta dari sumber eksternal [cite_0].
2. Memisahkan atau mengekstrak informasi penting seperti kode pos dari kolom alamat untuk mempermudah distribusi logistik.
3. Melakukan standarisasi format nomor telepon peserta.
4. Menyaring data sehingga hanya menghasilkan daftar peserta yang memenuhi kriteria keabsahan tertentu untuk siap dimuat ke database.

## Dataset
Dataset yang digunakan dalam proyek ini berisikan data registrasi peserta sebanyak 5000 baris dengan kolom-kolom sebagai berikut:
* `participant_id`: ID unik peserta
* `first_name` & `last_name`: Nama peserta
* `birth_date`: Tanggal lahir
* `address`: Alamat lengkap peserta
* `phone_number`: Nomor telepon
* `country` & `institute` & `occupation`: Negara asal, institusi, dan pekerjaan
* `register_time`: Waktu pendaftaran dalam format timestamp

*Sumber data*: `https://dqlabcdn.xeratic.com/dqlab-dataset/dqthon-participants.csv`

## Tahapan Analisis (Proses ETL)
Proyek ini terbagi ke dalam 3 tahapan utama:

1. **Extract**: Mengunduh dan membaca data mentah format CSV langsung dari URL menggunakan fungsi `pd.read_csv()` di Pandas.
2. **Transform**:
   * **Bagian I (Kode Pos)**: Membuat kolom baru `postal_code` dengan mengekstrak 5 digit angka terakhir yang berada di bagian akhir string kolom `address` menggunakan Regex (`(\d+)$`).
   * **Bagian II (Kota)**: Mengekstrak nama kota dari kolom `address` menggunakan Regex.
   * **Bagian III (Nama Unik)**: Membuat kolom nama lengkap yang sudah dibersihkan.
   * **Bagian IV (Nomor Telepon)**: Melakukan standarisasi nomor telepon agar diawali dengan kode negara `62` dan membersihkan karakter non-numerik (seperti tanda kurung atau spasi).
   * **Bagian V (Format Email)**: Membuat kolom email berbasis nama peserta dan institusinya dengan format huruf kecil.
   * **Bagian VI (Tanggal Daftar)**: Mengubah kolom `register_time` yang berupa format timestamp menjadi format tanggal yang mudah dibaca (`YYYY-MM-DD`).
3. **Load**: Menyimpan atau memuat hasil akhir data yang telah bersih dan bertransformasi ke dalam struktur DataFrame baru yang siap diekspor ke database target.

## Model yang Digunakan
Proyek ini tidak menggunakan model pembelajaran mesin (*Machine Learning*) karena berfokus pada rekayasa data (*Data Engineering*). "Model" atau pendekatan utama yang diimplementasikan di sini adalah pencocokan pola menggunakan **Regular Expression (Regex)** via method `.str.extract()` dan manipulasi string bawaan dari **Pandas DataFrame**.

## Hasil
Dari proses ETL yang dijalankan menggunakan skrip Python:
* Informasi kode pos (`postal_code`) berhasil dipisahkan secara otomatis dari kolom alamat utama.
* Seluruh nomor telepon peserta berhasil distandarisasi ke format internasional yang seragam.
* Kolom baru yang membantu operasional logistik telah terbentuk, meminimalisir kesalahan manual akibat alamat atau format kontak yang tidak konsisten.

## Kesimpulan
Penggunaan pustaka Pandas dan ekspresi reguler (Regex) terbukti sangat efektif untuk menangani alur kerja manipulasi data berskala kecil hingga menengah secara cepat. Melalui proyek ETL ini, data mentah yang awalnya berantakan berhasil disulap menjadi kumpulan data yang bersih, terstruktur, dan siap pakai demi mendukung kelancaran operasional analisis bisnis lebih lanjut.
