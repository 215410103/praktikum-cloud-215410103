# Tutorial Dasar Podman by Maria Anita Oka Mite 215410103

## 1. Pendahuluan
Podman (Pod Manager) adalah sebuah alat manajemen container yang dirilis di bawah lisensi Apache License 2.0. Artinya, Podman bersifat open-source, bebas digunakan, dimodifikasi, dan didistribusikan baik untuk keperluan pribadi maupun komersial. Podman dikembangkan oleh Red Hat dan didesain sebagai alternatif pengganti Docker yang aman, ringan, dan fleksibel.
Secara fungsi, Podman dapat digunakan untuk:
- Membuat container dari sebuah image.
- Menjalankan container di latar belakang atau interaktif.
- Mengelola container, image, dan pod (sekumpulan container yang berbagi network namespace).
### 1.1 Perbedaan Utama dengan Docker
Meskipun Podman menawarkan fungsionalitas yang mirip dengan Docker, terdapat satu perbedaan mendasar:
- Docker berjalan menggunakan daemon (proses tunggal yang selalu aktif di latar belakang) untuk mengelola semua container. Proses ini biasanya dijalankan oleh user dengan hak root.
- Podman tidak menggunakan daemon. Sebaliknya, setiap perintah Podman langsung mengeksekusi container sebagai proses terpisah yang dikelola oleh sistem. Karena itu, Podman dapat dijalankan secara rootless (tanpa hak akses root), sehingga lebih aman dan ramah untuk sistem multi-user.
### 1.2 Keunggulan Desain Podman
- Rootless Container <br>
  Dengan rootless mode, pengguna biasa bisa membuat dan menjalankan container tanpa risiko keamanan yang biasanya muncul saat daemon Docker dijalankan sebagai root.
- CLI yang Kompatibel dengan Docker <br>
  Perintah podman sengaja dibuat kompatibel dengan docker. Bahkan, Kita bisa membuat alias:
    bash
    alias docker=podman
    
    sehingga perintah Docker lama bisa langsung digunakan.
- Manajemen Pod <br>
  Podman mendukung konsep pod seperti di Kubernetes, yaitu sekumpulan container yang berjalan bersama dalam satu network namespace.
- Integrasi yang Kuat dengan Sistem Operasi <br>
  Karena tidak bergantung pada daemon, Podman bekerja lebih dekat dengan kernel Linux melalui libpod dan runc, sehingga konsumsi sumber daya lebih ringan.

## 2. Perbedaan Podman vs Docker
| Fitur            | Podman                        | Docker                |
|------------------|--------------------------------|-----------------------|
| Mode Daemon      | Tidak menggunakan daemon       | Menggunakan daemon    |
| Rootless Support | Ya                             | Terbatas              |
| CLI Compatibility| Kompatibel dengan docker CLI | Native Docker CLI     |
| Keamanan         | Lebih aman untuk multi-user    | Memerlukan root untuk daemon |

## 3. Instalasi Podman
Dibawah ini silakan pilih salah satu sesuai kebutuhan
### 3.1 Menggunakan Ubuntu/Debian
bash
sudo apt update
sudo apt install podman -y

- sudo apt update
  - sudo → Menjalankan perintah dengan hak akses administrator (root). Ini diperlukan karena instalasi software akan memodifikasi sistem.
  - apt → Singkatan dari Advanced Package Tool, yaitu manajer paket bawaan pada sistem berbasis Debian (seperti Ubuntu). Fungsinya untuk mengelola instalasi, pembaruan, dan penghapusan paket.
  - update → Memperbarui index daftar paket di sistem. <br>
  Artinya, sistem akan mengambil informasi terbaru dari semua repository yang terdaftar, sehingga kita bisa menginstal versi terbaru dari Podman atau paket lainnya.
- sudo apt install podman -y
  - install → Memerintahkan apt untuk menginstal sebuah paket.
  - podman → Nama paket yang ingin diinstal.
  - -y → Flag untuk menjawab "yes" secara otomatis pada semua konfirmasi selama proses instalasi.
Biasanya, saat menginstal, apt akan bertanya:
    bash
    Do you want to continue? [Y/n]
    
    Dengan -y, proses instalasi langsung dilanjutkan tanpa menunggu input manual.
### 3.2 Menggunakan Fedora
bash
sudo dnf install podman -y

- sudo
  - Menjalankan perintah dengan hak akses administrator (root).
  - Diperlukan karena proses instalasi akan mengubah file dan konfigurasi di level sistem.
- dnf
  - DNF (Dandified YUM) adalah manajer paket bawaan Fedora, Red Hat Enterprise Linux (RHEL), dan CentOS (versi terbaru).
  - Fungsinya sama seperti apt di Ubuntu/Debian, yaitu untuk menginstal, menghapus, dan memperbarui paket software.
  - DNF menggantikan yum dengan performa lebih baik dan dependency resolution yang lebih cepat.
- install podman
  - install → Perintah untuk menginstal sebuah paket baru dari repository resmi Fedora.
  - podman → Nama paket yang ingin diinstal.
- -y
  - Menjawab otomatis "Yes" untuk semua konfirmasi selama instalasi.
  - Tanpa -y, DNF akan menampilkan pesan:
      bash
      Is this ok [y/N]:
      
      dan menunggu input pengguna.
  - Dengan -y, proses berjalan otomatis tanpa interaksi manual.
### 3.3 Menggunakan Arch Linux
bash
sudo pacman -S podman

- pacman
  - Pacman adalah manajer paket bawaan Arch Linux dan turunannya (misalnya Manjaro).
  - Fungsinya mirip seperti apt di Ubuntu atau dnf di Fedora, yaitu mengelola instalasi, pembaruan, dan penghapusan software.
  - Nama "pacman" berasal dari Package Manager.
- -S
  - Opsi -S (sync) digunakan untuk menginstal paket baru atau memperbarui paket yang sudah ada.
  - Saat perintah dijalankan, pacman akan mengecek apakah paket podman tersedia di repository resmi, mengunduh dan menginstalnya, memasang semua dependencies (paket lain yang dibutuhkan Podman).
- podman <br>
  Nama paket yang akan diinstal, dalam hal ini adalah Podman.
### 3.4 Menggunakan Manual
- Unduh installer Windows dari situs resmi Podman atau GitHub.<br>
  https://podman.io/docs/installation<br>
  https://podman-desktop.io/docs/installation/windows-install
- Jalankan .exe installer, bisa juga menggunakan opsi silent (/S), Chocolatey, Scoop, atau Winget.
- Installer akan mengurus setup WSL2 dan mesin virtual untuk menjalankan Linux guest. Setelah selesai, Kita bisa menggunakan GUI untuk mengelola container dan Kubernetes.
### 3.5 Menggunakan Command-Line (MSI + podman machine)
- Download dan jalankan podman-vX.Y.Z.msi dari GitHub Releases.
- Setelah install, jalankan perintah PowerShell:
  bash
  podman machine init
  podman machine start
  
  Ini membuat mesin virtual yang menjalankan Podman daemon di WSL2.<br>
- Sekarang perintah podman bisa digunakan langsung dari PowerShell atau CMD.
### 3.6 Mengecek Versi
bash
podman --version

- podman
  - Ini adalah command line interface (CLI) Podman.
  - Saat mengetik podman di terminal, kita memanggil program utama Podman yang berfungsi untuk mengelola container, image, dan pod.
- --version
  - --version adalah opsi/flag yang digunakan hampir di semua aplikasi CLI untuk menampilkan informasi versi software.
  - Dalam konteks Podman, perintah ini akan menampilkan nomor versi Podman yang terpasang dan terkadang juga informasi build atau commit hash (tergantung paket dan distro Linux yang digunakan).
- Contoh output
  bash
  $ podman --version
  podman version 4.9.3
  
## 4. Perintah Dasar Podman
### 4.1 Menarik (Pull) Image
bash
podman pull nginx

Contoh Output
bash
$ podman pull nginx
Trying to pull docker.io/library/nginx:latest...
Getting image source signatures
Copying blob 5c2aa87436b4 done
Copying blob 7f676cc4b9b1 done
Copying blob e99c654a7e85 done
...
Storing signatures
docker.io/library/nginx:latest

Proses yang Terjadi : <br>
- Podman mencari image nginx di registry default.
- Jika image ditemukan, Podman mengunduhnya ke cache lokal.
- Image tersimpan di lokal dan siap digunakan untuk membuat container.

- pull
  - Perintah pull digunakan untuk mengunduh (menarik) image container dari sebuah container registry ke sistem lokal.
  - Container registry adalah repositori tempat image disimpan.
- nginx
  - Nama image yang ingin diunduh.
  - nginx adalah image resmi (official image) web server Nginx yang tersedia di Docker Hub.
  - Jika tidak menyebutkan tag versi (misalnya nginx:1.25), Podman akan mengambil tag default latest.

### 4.2 Menjalankan Container
bash
podman run -d -p 8080:80 nginx

Contoh Output <br>
Output yang muncul biasanya hanya berupa container ID panjang (sekitar 64 karakter hex)
bash
$ podman run -d -p 8080:80 nginx
4c01db0b339c3d7c90dd7ec9a04915efccf9d4b7cbbd6a3d6e27b7f0e4b20d4b

- podman run – Menjalankan container baru.
- -d – Mode detached, container berjalan di latar belakang.
- -p 8080:80 – Port mapping, mengarahkan port 80 di dalam container ke port 8080 di host, sehingga kita bisa mengaksesnya di http://localhost:8080.
- nginx – Nama image yang digunakan (dalam hal ini Nginx web server).
### 4.3 Melihat Daftar Container
bash
podman ps
podman ps -a

Contoh Output
bash
$ podman ps
CONTAINER ID  IMAGE                           COMMAND               CREATED         STATUS             PORTS                 NAMES
4c01db0b339c  docker.io/library/nginx:latest  nginx -g daemon off;  5 seconds ago   Up 5 seconds ago   0.0.0.0:8080->80/tcp  inspiring_turing

- podman ps digunakan untuk melihat container yang sedang berjalan
- podman ps - a digunakan untuk melihat semua container
### 4.4 Menghentikan Container
bash
podman stop <CONTAINER_ID>

- podman stop → Perintah untuk menghentikan container secara teratur (gracefully).
- <CONTAINER_ID> → ID atau nama container yang ingin dihentikan.
### 4.5 Menghapus Container
bash
podman rm <CONTAINER_ID>

Contoh Output
bash
$ podman rm <CONTAINER_ID>
<CONTAINER_ID>

- podman rm menghapus container dari daftar Podman.
- <CONTAINER_ID> → ID atau nama container yang ingin dihentikan.
### 4.6 Menghapus Image
bash
podman rmi <IMAGE_NAME>

Contoh Output
bash
$ podman rmi <IMAGE_NAME>
Untagged: docker.io/library/nginx:latest
Deleted: 3f8a4339aaddc912eae556e80d6a764f06b1bdb3c988f3ddf7869a4abf123456
Deleted: 0f2a3c42b9fcb6b28d7b689ba5e07e8f3bbdd0b12d0c42f9f6e3f5f2ad789abc
Deleted: 42d3b7e568acbfa9b2bb4a3ab73b1e47e8f0e2c84a0ec3d00a212d34a56789de

- Fungsi: Menghapus image dari sistem lokal.
- Harus pastikan tidak ada container yang menggunakan image tersebut.
- <IMAGE_NAME> bisa berupa nama atau ID image.
## 5. Contoh Lengkap
### 5.1 Pull image alpine
bash
podman pull alpine

### 5.2 Jalankan container dengan perintah
bash
podman run -it alpine sh

### 5.3 Di dalam container jalankan
bash
echo "Test Podman"

### 5.4 Keluar dari container
bash
exit

## 6. Kesimpulan
Podman adalah solusi container yang aman, fleksibel, dan kompatibel dengan Docker CLI. Cocok untuk pengembangan maupun produksi. Secara keseluruhan, Podman merupakan pilihan tepat untuk lingkungan pengembangan, pengujian, dan produksi, khususnya bagi organisasi atau individu yang mengutamakan keamanan, keterbukaan standar, dan kontrol penuh terhadap container yang dijalankan.
## 7. Referensi
https://podman.io/ <br>
https://www.redhat.com/en/blog/run-podman-windows
