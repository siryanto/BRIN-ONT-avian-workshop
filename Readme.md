# BRIN ONT Avian Sequencing Workshop

## 1. Mengakses MAHAMERU BRIN HPC dengan SSH
Gunakan Terminal, Command Prompt, atau CLI software favorit Anda untuk menjalankan perintah SSH.
```
ssh [akun SSO BRIN]@login2.hpc.brin.go.id
```
Jika berhasil, Anda masuk di login node bernama `trembesi02` seperti tampilan di bawah ini

![Screenshot 2024-07-09 at 11 52 51](https://github.com/siryanto/BRIN-ONT-avian-workshop/assets/30887367/649bc235-0039-4e7a-a2de-23776572c2b9)

## 2. Bekerja dengan mode interaktif di MAHAMERU BRIN HPC
Gunakan perintah `srun` untuk bekerja dengan cara interaktif di MAHAMERU BRIN HPC seperti berikut ini
```
srun --partition=interactive2 --cpus-per-task=4 --pty bash
```
Jika berhasil, maka Anda akan mendapatkan compute node bermana `trembesi91` atau `trembesi92`. 

![Screenshot 2024-07-11 at 09 51 58](https://github.com/siryanto/BRIN-ONT-avian-workshop/assets/30887367/d88383e2-75d8-4798-b68a-8bcb91f0ca78)


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

![Screenshot 2024-07-09 at 13 06 21](https://github.com/siryanto/BRIN-ONT-avian-workshop/assets/30887367/8b0f3297-6ac4-4b74-8989-e81a19cae6e9)


Anda dapat juga mengecek apakah Conda sudah terinstall atau belum dengan mengetikkan perintah 
```
 conda --version
```
Jika Conda telah berhasil terpasang, maka akan muncul nomor versi Conda yang terpasang, misal `conda 24.3.0`. 
## 4. Buat dan aktifkan environment aplikasi untuk analisis
Agar proses pembuatan environment dan instalasi aplikasi berjalan lancar, lakukan proses tersebut di interactive compute node dengan cara menjalankan perintah `srun` seperti pada langkah 2.

Dalam workshop ini, ada 3 aplikasi yang akan digunakan, yaitu `NanoPlot` untuk QC, `flye` untuk melakukan denovo assembly, dan `medaka` untuk polishing hasil assembly. Ketiga aplikasi ini tersedia di Conda channel yang bernama 'bioconda'. 

Karena dependencies dari masing-masing aplikasi yang digunakan tidak selalu seragam, terutama versi `python`, maka menggunakan satu environment untuk ketiga aplikasi di atas tidak memungkinkan. Dalam workshop ini, setiap aplikasi akan menggunakan environment sendiri. 
### 4.1 Install NanoPlot
```
conda create -n nanoplot -c bioconda NanoPlot
```
Ketik 'Y' untuk melanjutkan instalasi saat proses meinta konfirmasi. Ada banyak paket yang dibutuhkan NanoPlot, sehingga semua paket tersebut akan diunduh dan selanjutnya dipasang. Tunggu hingga proses instalasi selesai.

Jika proses selesai dan berhasil, periksa perintah NanoPlot dengan menjalankan perintah sebagai berikut.
```
conda activate nanoplot
NanoPlot --version

```
Jika berhasil, maka layar akan menampilkan kode versi NanoPlot. 
### 4.2 Install flye
```
conda create -n flye -c bioconda flye
```
Ketik 'Y' untuk melanjutkan instalasi saat proses meinta konfirmasi. Ada banyak paket yang dibutuhkan NanoPlot, sehingga semua paket tersebut akan diunduh dan selanjutnya dipasang. Tunggu hingga proses instalasi selesai.

Jika proses selesai dan berhasil, periksa perintah flye dengan menjalankan perintah sebagai berikut.
```
conda activate flye
flye --version

```
### 4.3 Install medaka
```
conda create -n medaka -c bioconda medaka
```
Ketik 'Y' untuk melanjutkan instalasi saat proses meinta konfirmasi. Ada banyak paket yang dibutuhkan NanoPlot, sehingga semua paket tersebut akan diunduh dan selanjutnya dipasang. Tunggu hingga proses instalasi selesai.

Jika proses selesai dan berhasil, periksa perintah medaka dengan menjalankan perintah sebagai berikut.
```
conda activate medaka
medaka --version

```
## 5. Menyiapkan data sampel
Sampel berupa berkas dengan format FASTQ telah tersedia di server. Untuk memudahkan, buat symbolic link (symlink) ke direktori kerja Anda. Sebelum melanjutkan, agar memudahkan Anda, aktifkan dahulu environment base dengan perintah berikut.
```
conda activate base
```
### 5.1 Buat direktori kerja
Buatlah sebuah direktori untuk proyek ini, contoh: avian_worskhop.
```
mkdir avian_workshop
```
Masuk ke direktori kerja Anda
```
cd avian_workshop
```
### 5.2 Buat symlink untuk sampel
Buat symlink untuk sampel dengan perintah berikut
```
ln -s /mgpfs/data/workshop/SRR15421342.sampled10K.fastq
```
Lihat hasilnya dengan perintah `ls` seperti berikut
```
ls
```
Jika berhasil, akan ada sebuah berkas bernama `SRR15421342.sampled10K.fastq`.
Berkas FASTQ tersebut merupakan berkas SRR15421342 yang diunduh dari DDBJ dan telah dilakukan resampling sebanyak 10K reads. Berkas asli berukuran 24GB, adapun berkas resampling hanya berukuran 0.5GB.
## 6. Periksa sampel dengan NanoPlot
Aktifkan NanoPlot env
```
conda activate nanoplot
```
Jalankan NanoPlot
```
NanoPlot --fastq SRR15421342.sampled10K.fastq -t 8 -o nanoplot_result
```
Jika berhasil, output NanoPlot ada di direktori 'nanoplot_result'.
```
ls nanoplot_result
```
Tampilan hasilnya seperti gambar berikut.
![Screenshot 2024-07-11 at 06 54 00](https://github.com/siryanto/BRIN-ONT-avian-workshop/assets/30887367/31c225ad-1d9b-4c8f-b768-07806a0f58f2)
Lihat summary statistik dengan perintah `less` seperti berikut
```
less nanoplot_result/NanoStats.txt
```
Hasilnya seperti di bawah ini
```
General summary:
Mean read length:               26,687.2
Mean read quality:                  11.5
Median read length:             19,427.0
Median read quality:                13.0
Number of reads:                10,000.0
Read length N50:                45,270.0
STDEV read length:              26,049.6
Total bases:               266,871,520.0
Number, percentage and megabases of reads above quality cutoffs
>Q10:   8400 (84.0%) 228.8Mb
>Q15:   1588 (15.9%) 44.3Mb
>Q20:   0 (0.0%) 0.0Mb
>Q25:   0 (0.0%) 0.0Mb
>Q30:   0 (0.0%) 0.0Mb
Top 5 highest mean basecall quality scores and their read lengths
1:      19.3 (262)
2:      19.0 (1)
3:      18.5 (349)
4:      18.5 (1076)
5:      18.2 (274)
Top 5 longest reads and their mean basecall quality score
1:      347973 (15.0)
2:      238044 (14.0)
3:      225172 (13.6)
4:      202808 (16.2)
5:      199725 (14.9)
```
Untuk keluar dari perintah `less`, tekan tombol 'q' di keyboard Anda.

Hasil plot berupa citra PNG dan report HTML dapat diunduh dengan SFTP manager seperti: FileZilla, CyberDuck, dll. (Akan dijelaskan di akhir sesi jika masih ada waktu)
## 7. Assembly menggunakan Flye
Aktifkan Flye env
```
conda activate flye
```
Jalankan flye
```
flye --nano-raw SRR15421342.sampled10K.fastq --threads 4 --out-dir flye_result --scaffold
```
Jika berhasil, akan ada direktori baru bernama `flye_result`. Hasil flye tersimpan di direktori tersebut, lihat dengan perintah `ls`.
```
ls flye_result
```
Berikut contoh hasil luaran Flye.
![Screenshot 2024-07-11 at 10 01 42](https://github.com/siryanto/BRIN-ONT-avian-workshop/assets/30887367/a071a757-5c78-4d5e-9bc9-7401f0bfc344)


## 8. Polishing menggunakan Medaka
Aktifkan Medaka env
```
conda activate medaka
```
Jalankan `medaka_consensus` sebagai berikut
```
medaka_consensus -i SRR15421342.sampled10K.fastq -d flye_result/assembly.fasta -o medaka_consensus_result -t 4 -f -m  r1041_e82_400bps_hac_g632
```
Catatan: opsi `-m  r1041_e82_400bps_hac_g632` merupakan model yang digunakan, disesuaikan dengan platform yang digunakan. Untuk kasus ini, karena sampel dilakukan seqeuncing dengan PromethION dan kita asumsikan menggunakan basecalling mode HAC, maka model tersebut yang digunakan. Selain itu, jika model tersebut belum diunduh, maka saat menjalankan `medaka_consensus` komputer/server/node harus dapat mengakses internet untuk mengunduh model yang digunakan.
