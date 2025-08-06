# Setup Environment Development Odoo 13 di Docker

Panduan ini menjelaskan langkah-langkah untuk melakukan setup lingkungan pengembangan **Odoo 13** menggunakan **Docker**. Semua pengaturan dilakukan dengan konfigurasi standar untuk pengembangan aplikasi berbasis Docker.

## **Lingkungan Pengembangan**

### **WSL (Windows Subsystem for Linux)**
- **Version**: 2.5.9.0
- **Kernel Version**: 6.6.87.2-1
- **WSLg Version**: 1.0.66
- **MSRDC Version**: 1.2.6074
- **Direct3D Version**: 1.611.1-81528511
- **DXCore Version**: 10.0.26100.1-240331-1435.ge-release
- **Windows Version**: 10.0.26100.3194

### **Docker**
- **Docker Engine Version**: 27.5.1, build 27.5.1-0ubuntu3~24.04.2

### **Docker Compose**
- **Version**: 1.29.2, build unknown

## **Langkah-langkah Setup**

1. **Instalasi Docker Desktop**
    - Unduh dan instal **Docker Desktop** versi 27.5.1 pada Windows, yang mendukung integrasi dengan WSL 2.
    - Docker Compose akan diinstal secara otomatis bersama dengan Docker Desktop.

2. **Konfigurasi Docker Compose untuk Odoo 13**
    - Siapkan file konfigurasi `docker-compose.yml` untuk menjalankan **Odoo 13** dan **PostgreSQL** dalam container terpisah.
    - Contoh konfigurasi minimal:
      ```yaml
      version: '2'
      services:
        web:
          image: odoo:13
          depends_on:
            - db
          ports:
            - "9999:8069"
          volumes:
            - ./config:/etc/odoo
            - ./addons:/mnt/extra-addons
            - ./odoo-web-data:/var/lib/odoo
        db:
          image: postgres:9.5
          environment:
            - POSTGRES_DB=postgres
            - POSTGRES_USER=odoo
            - POSTGRES_PASSWORD=salimatmin
          volumes:
            - ./postgres-data:/var/lib/postgresql/data
      ```

3. **Pengaturan Konfigurasi Odoo**
    - Buat file `config/odoo.conf`:
      ```ini
      [options]
      admin_passwd = kasihsayangatminkepadamember
      db_host = db
      db_port = 5432
      db_user = odoo
      db_password = salimatmin
      addons_path = /mnt/extra-addons
      ```

4. **Pengaturan Volume dan Jaringan**
    - Pastikan volume dan jaringan dikonfigurasi dengan benar agar data Odoo dan PostgreSQL dapat dipertahankan di container terpisah.

5. **Verifikasi Versi Docker dan Docker Compose**
    - Verifikasi versi Docker dan Docker Compose yang terinstal menggunakan perintah:
      ```bash
      docker --version
      docker-compose --version
      ```

6. **Jalankan Docker Compose**
    - Jalankan perintah berikut untuk memulai container:
      ```bash
      docker-compose up
      ```
    - Setelah container berjalan, Odoo dapat diakses melalui `http://localhost:2513`.

7. **Debugging dan Monitoring**
    - Jika terjadi masalah, lakukan debugging dengan melihat log container menggunakan:
      ```bash
      docker logs <nama-container>
      ```
    - Pastikan kedua container (Odoo dan PostgreSQL) berjalan tanpa masalah.

## **Hasil Setup**
- **Odoo 13** dapat dijalankan pada port yang telah dikonfigurasi.
- **PostgreSQL** berjalan di container terpisah dan terhubung dengan Odoo.
  
## **Kesimpulan**
Dengan menggunakan Docker versi **27.5.1** dan Docker Compose versi **1.29.2**, pengaturan **Odoo 13** dapat dilakukan dengan lancar di lingkungan pengembangan berbasis WSL 2. Konfigurasi ini menyediakan cara yang efisien dan mudah untuk menjalankan Odoo dalam container terisolasi.

## **Referensi**
- [Dokumentasi Odoo](https://www.odoo.com/)
- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)

---
