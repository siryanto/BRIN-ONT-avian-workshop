# BRIN ONT Avian Sequencing Workshop

## 1. Mengakses MAHAMERU BRIN HPC dengan SSH
Gunakan Terminal, Command Prompt, atau CLI software favorit Anda untuk menjalankan perintah SSH.
```
ssh [Akun_SSO_BRIN]@login2.hpc.brin.go.id
```
Jika berhasil, Anda masuk di login node bernama `trembesi02` seperti tampilan di bawah ini

![Screenshot 2024-07-09 at 11 52 51](https://github.com/siryanto/BRIN-ONT-avian-workshop/assets/30887367/649bc235-0039-4e7a-a2de-23776572c2b9)

## 2. Bekerja dengan mode interaktif di MAHAMERU BRIN HPC
Gunakan perintah `srun` untuk bekerja dengan cara interaktif di MAHAMERU BRIN HPC seperti berikut ini
```
srun --partition=interactive --cpus-per-task=4 --pty bash
```
Jika berhasil, maka Anda akan mendapatkan compute node bermana `trembesi91` atau `trembesi92`. 

![Screenshot 2024-07-09 at 11 57 17](https://github.com/siryanto/BRIN-ONT-avian-workshop/assets/30887367/ba3cb29c-c1f4-42d4-9647-f004d086ff88)

## 3. Menyiapkan Conda Environment
PERHATIAN!
Jika Anda sudah memiliki Conda Environment, silakan SKIP langkah ini dan lanjut ke langkah 4.

Install miniconda dengan perintah berikut.
```
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh

```
Baca dan ikuti langkah proses instalasi, ketik 'Y' jika diperlukan.
Setelah proses instalasi selesai, logout terlebih dahulu dengan perintah `exit` sebanyak 2 kali. Exit pertama untuk keluar dari mode interaktif dan exit kedua untuk keluar dari MAHAMERU BRIN HPC.

Login kembali ke MAHAMERU BRIN HPC dengan perintah SSH. Jika instalasi conda berhasil dan environment `base` diaktifkan otomatis, maka prompt di login node muncul `(base)` sebelum tulisan akun Anda, seperti contoh berikut.

<img width="238" alt="image" src="https://github.com/siryanto/BRIN-ONT-avian-workshop/assets/30887367/12a21b43-8ae7-4c43-908b-988af8387a89">

## 4. Buat dan aktifkan environment untuk analisis
Buat environment dengan perintah `conda create` seperti berikut. Sebagai contoh, nama environment yang dibuat adakah 'avianworkshop'. 
```
conda create -n avianworkshop
```
Tekan tombol 'Y' untuk konfirmasi ketika ditanya `Proceed ([y]/n)?`.
Aktifkan environment yang telah kita buat
