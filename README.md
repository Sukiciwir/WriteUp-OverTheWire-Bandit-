# OverTheWire: Bandit - Writeup Lengkap (0-33)

Dokumen ini adalah panduan dan catatan perjalanan (*walkthrough*) untuk menyelesaikan wargame OverTheWire Bandit dari Level 0 hingga 33. Setiap level dijelaskan dengan tantangan, logika penyelesaian, dan perintah yang digunakan.

---

### **Level 0 → 1: Langkah Pertama**
- **Tantangan:** Membaca isi file `readme` di home direktori.
- **Logika:** Menggunakan perintah dasar Linux untuk membaca file.
- **Perintah:**
    ```bash
    cat readme
    ```
- **Password:** `ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If`

---

### **Level 1 → 2: File Bernama Aneh**
- **Tantangan:** Membaca file yang namanya hanya `-` (karakter strip).
- **Logika:** Karakter `-` adalah karakter spesial. Kita harus menggunakan path relatif `./` agar shell tidak salah mengartikannya sebagai opsi.
- **Perintah:**
    ```bash
    cat ./-
    ```
- **Password:** `263JGJPfgU6LtdEvgfWU1XP5yac29mFx`

---

### **Level 2 → 3: Nama File dengan Spasi**
- **Tantangan:** Membaca file yang namanya mengandung spasi (`spaces in this filename`).
- **Logika:** Nama file harus dibungkus dengan tanda kutip (`"`) agar dianggap sebagai satu argumen utuh oleh shell.
- **Perintah:**
    ```bash
    cat "spaces in this filename"
    ```
- **Password:** `MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx`

---

### **Level 3 → 4: File Tersembunyi**
- **Tantangan:** Password ada di dalam sebuah direktori, namun filenya tersembunyi.
- **Logika:** Di Linux, file yang namanya diawali dengan titik (`.`) dianggap tersembunyi dan tidak muncul di `ls` biasa. Gunakan `ls -la` untuk menampilkannya.
- **Perintah:**
    ```bash
    ls -la inhere
    cat inhere/...Hiding-From-You
    ```
- **Password:** `2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ`

---

### **Level 4 → 5: Mencari di Tumpukan Jerami**
- **Tantangan:** Password ada di salah satu dari banyak file, dimana mayoritas berisi data sampah (non-readable).
- **Logika:** File password biasanya berupa teks biasa dan hanya berisi satu baris. Kita bisa memfilter semua file berdasarkan kriteria ini.
- **Perintah:**
    ```bash
    # Cari file yang hanya punya 1 baris
    find inhere -type f -exec wc -l {} \;
    # Setelah menemukan kandidat (misal: -file07), baca isinya
    cat inhere/-file07
    ```
- **Password:** `4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw`

---

### **Level 5 → 6: Pencarian Berbasis Properti File**
- **Tantangan:** Mencari file dengan kombinasi properti spesifik: *human-readable*, ukuran 1033 byte, dan tidak bisa dieksekusi.
- **Logika:** Perintah `find` sangat ideal untuk pencarian berbasis metadata file.
- **Perintah:**
    ```bash
    # Cari file dengan kriteria yang diberikan
    find inhere -type f -size 1033c ! -executable
    # Baca file yang ditemukan
    cat inhere/maybehere07/.file2
    ```
- **Password:** `HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`

---

### **Level 6 → 7: Pencarian Lintas Direktori**
- **Tantangan:** File password bisa ada di mana saja di server. Kriterianya: dimiliki user `bandit7`, group `bandit6`, ukuran 33 byte.
- **Logika:** Gunakan `find` dari direktori root (`/`) dan buang semua pesan error "Permission denied" agar output bersih.
- **Perintah:**
    ```bash
    find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
    cat /var/lib/dpkg/info/bandit7.password
    ```
- **Password:** `morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`

---

### **Level 7 → 8: Mencari Kata Kunci**
- **Tantangan:** Password ada di baris yang sama dengan kata `millionth` di file `data.txt`.
- **Logika:** `grep` adalah perintah terbaik untuk mencari teks di dalam file.
- **Perintah:**
    ```bash
    grep "millionth" data.txt
    ```
- **Password:** `dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc`

---

### **Level 8 → 9: Mencari Baris Unik**
- **Tantangan:** Password adalah satu-satunya baris yang unik di file `data.txt`.
- **Logika:** Kombinasikan `sort` untuk mengurutkan dan `uniq -u` untuk memfilter hanya baris yang muncul sekali.
- **Perintah:**
    ```bash
    sort data.txt | uniq -u
    ```
- **Password:** `4CKMh1JI91bUIZZPXDqGanal4xvAg0JM`

---

### **Level 9 → 10: Teks di Tengah Biner**
- **Tantangan:** Password adalah string teks yang bisa dibaca di dalam file biner.
- **Logika:** Gunakan `strings` untuk mengekstrak semua teks, lalu `grep` untuk mencari pola yang mencurigakan (dalam kasus ini, baris yang diawali `==`).
- **Perintah:**
    ```bash
    strings data.txt | grep "=="
    ```
- **Password:** `FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey`

---

### **Level 10 → 11: Dekripsi Base64**
- **Tantangan:** Password di-encode menggunakan Base64.
- **Logika:** Gunakan command `base64 -d` untuk men-decode.
- **Perintah:**
    ```bash
    cat data.txt | base64 -d
    ```
- **Password:** `dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr`

---

### **Level 11 → 12: Dekripsi ROT13**
- **Tantangan:** Password dienkripsi dengan ROT13.
- **Logika:** Gunakan `tr` untuk translasi karakter, menggeser setiap huruf 13 posisi.
- **Perintah:**
    ```bash
    cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
    ```
- **Password:** `7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4`

---

### **Level 12 → 13: Rantai Dekompresi**
- **Tantangan:** File adalah hexdump dari file yang dikompresi berlapis-lapis.
- **Logika:** Lakukan dekompresi berantai satu per satu, dengan memeriksa tipe file di setiap langkah. Urutannya: `xxd -r` -> `gunzip` -> `bzip2` -> `gunzip` -> `tar` -> `tar` -> `bzip2` -> `tar` -> `gunzip`.
- **Password:** `FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn`

---

### **Level 13 → 14: Otentikasi dengan SSH Key**
- **Tantangan:** Login sebagai user `bandit14` menggunakan private key.
- **Logika:** Gunakan opsi `-i` pada `ssh` untuk menentukan file private key yang akan digunakan.
- **Perintah:**
    ```bash
    ssh -i sshkey.private -p 2220 bandit14@bandit.labs.overthewire.org 'cat /etc/bandit_pass/bandit14'
    ```
- **Password:** `MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS`

---

### **Level 14 → 15: Interaksi via Network Port**
- **Tantangan:** Mengirim password ke port 30000 di localhost untuk mendapatkan password berikutnya.
- **Logika:** Gunakan `echo` dan `nc` (netcat) untuk mengirim data ke port jaringan.
- **Perintah:**
    ```bash
    echo "MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS" | nc localhost 30000
    ```
- **Password:** `8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo`

---

### **Level 15 → 16: Interaksi via SSL**
- **Tantangan:** Sama seperti sebelumnya, tapi kali ini koneksi harus melalui SSL/TLS.
- **Logika:** Gunakan `openssl s_client` sebagai pengganti `netcat` untuk membuat koneksi terenkripsi.
- **Perintah:**
    ```bash
    echo "8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo" | openssl s_client -connect localhost:30001 -ign_eof
    ```
- **Password:** `kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx`

---

### **Level 16 → 17: Port Scanning & SSL**
- **Tantangan:** Menemukan port SSL yang benar di antara range 31000-32000.
- **Logika:** Gunakan `nmap` untuk memindai port yang terbuka, lalu coba konek satu per satu menggunakan `openssl s_client`.
- **Perintah:**
    ```bash
    nmap -p 31000-32000 localhost
    # Setelah menemukan port (misal 31790), konek ke sana
    echo "kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx" | openssl s_client -connect localhost:31790 -ign_eof
    ```
- **Password:** *Private SSH key yang diterima dari server.*

---

### **Level 17 → 18: Membandingkan File**
- **Tantangan:** Password adalah baris yang ada di `passwords.new` tapi tidak ada di `passwords.old`.
- **Logika:** Perintah `diff` dibuat khusus untuk tugas ini.
- **Perintah:**
    ```bash
    diff passwords.old passwords.new
    ```
- **Password:** `x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO`

---

### **Level 18 → 19: Shell yang Langsung Keluar**
- **Tantangan:** Sesi SSH langsung tertutup.
- **Logika:** Jalankan perintah yang diinginkan langsung sebagai argumen `ssh` agar dieksekusi sebelum shell sempat keluar.
- **Perintah:**
    ```bash
    sshpass -p '...' ssh bandit18@... 'cat readme'
    ```
- **Password:** `cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8`

---

### **Level 19 → 20: Pemanfaatan SUID**
- **Tantangan:** Membaca file yang hanya bisa diakses `bandit20` menggunakan program SUID `bandit20-do`.
- **Logika:** Program SUID akan berjalan dengan hak akses owner-nya. Kita bisa "menitip" perintah `cat` untuk dijalankan oleh program ini.
- **Perintah:**
    ```bash
    ./bandit20-do cat /etc/bandit_pass/bandit20
    ```
- **Password:** `0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO`

---

### **Level 20 → 21: Reverse Connection**
- **Tantangan:** Sebuah program SUID akan konek ke port yang kita tentukan.
- **Logika:** Buat server listener sendiri menggunakan `nc` di background, yang siap mengirim password saat program SUID itu konek.
- **Perintah:**
    ```bash
    # Terminal 1:
    echo "password-level-20" | nc -l -p 12345 &
    # Terminal 2:
    ./suconnect 12345
    ```
- **Password:** `EeoULMCra2q0dSkYj561DX7s1CpBuOBt`

---

### **Level 21-23: Eksploitasi Cron Job**
- **Tantangan:** Menemukan dan menganalisa cron job yang berjalan periodik untuk membocorkan password.
- **Logika:** Periksa direktori `/etc/cron.d/`, baca skrip yang dijalankan, dan cari tahu di mana skrip itu menyimpan outputnya (biasanya di `/tmp`).
- **Password 22:** `tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q`
- **Password 23:** `0Zf11ioIjMVN551jX3CmStKLYqjk54Ga`

---

### **Level 23 → 24: Injeksi Skrip via Cron Job**
- **Tantangan:** Sebuah cron job akan mengeksekusi skrip apa pun yang kita taruh di direktori tertentu.
- **Logika:** Buat skrip sederhana yang isinya `cat /etc/bandit_pass/bandit24 > /tmp/lokasi_aman`, lalu letakkan skrip itu di direktori yang dieksekusi cron.
- **Password:** `gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8`

---

### **Level 24 → 25: Brute Force PIN**
- **Tantangan:** Menebak PIN 4 digit yang dikirim bersamaan dengan password.
- **Logika:** Buat skrip `for` loop untuk mencoba semua 10.000 kemungkinan PIN (0000-9999) dan kirim semuanya dalam satu koneksi `netcat`.
- **Perintah:**
    ```bash
    for i in $(seq -w 0000 9999); do echo "password-saat-ini $i"; done | nc localhost 30002 | grep -v "Wrong!"
    ```
- **Password:** `iCi86ttT4KSNe1armKiwbQNmB3YJP3q4`

---

### **Level 25 → 27: Shell Escape**
- **Tantangan:** Kabur dari shell terbatas.
- **Logika (Insight dari user):** Manfaatkan perilaku pager `more` yang akan berhenti jika ukuran terminal terlalu kecil. Saat `more` berhenti, tekan `v` untuk masuk `vi`, lalu gunakan `:set shell=/bin/bash` dan `:shell` untuk mendapatkan shell penuh.
- **Password 27:** `upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB`

---

### **Level 27 → 32: Trik-trik Git**
- **Tantangan:** Serangkaian level yang menguji pengetahuan mendalam tentang `git`, harus dieksekusi dari mesin lokal.
- **Logika:**
  - **27→28:** `git clone` biasa.
  - **28→29:** Password ada di histori, gunakan `git log` dan `git diff <commit_hash>` untuk melihat perubahan.
  - **29→30:** Password ada di branch lain, gunakan `git branch -a` dan `git checkout dev`.
  - **30→31:** Password ada di *tag*, gunakan `git tag` dan `git show <nama_tag>`.
  - **31→32:** Memicu *server-side hook* dengan `git push` file yang seharusnya di-ignore.
- **Password 32:** `3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K`

---

### **Level 32 → 33: Uppercase Shell Escape**
- **Tantangan:** Kabur dari shell yang hanya menerima input huruf besar.
- **Logika:** Perintah `$0` di shell seringkali merujuk ke nama skrip/program itu sendiri. Mengeksekusinya bisa me-respawn proses shell dan memberimu shell normal.
- **Perintah:**
    ```bash
    $0
    /bin/bash
    cat /etc/bandit_pass/bandit33
    ```
- **Password:** `tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0`

---

### **Level 33 → Selesai**
- **Tantangan:** Level terakhir.
- **Logika:** Cukup baca `README.txt` untuk pesan kelulusan.
- **Pesan:** *Congratulations on solving the last level of this game!*
