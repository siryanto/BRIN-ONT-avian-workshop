# BRIN ONT Avian Sequencing Workshop

## 1. Mengakses MAHAMERU BRIN HPC dengan SSH
Gunakan Terminal, Command Prompt, atau CLI software favorit Anda untuk menjalankan perintah SSH.

```
ssh [Akun_SSO_BRIN]@login2.hpc.brin.go.id
```
## 2. Bekerja dengan mode interaktif di MAHAMERU BRIN HPC
Gunakan perintah `srun` untuk bekerja dengan cara interaktif di MAHAMERU BRIN HPC seperti berikut ini
```
srun --partition=interactive --cpus-per-task=4 --pty bash
```
## 3. Menyiapkan Conda Environment
Install miniconda dengan perintah berikut.
```
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh

```
## 4. Buat dan aktifkan environment untuk analisis
Buat environment dengan perintah `conda create` seperti berikut. Sebagai contoh, nama environment yang dibuat adakah 'avianworkshop'. 
```
conda create -n avianworkshop
```
Tekan tombol 'Y' untuk konfirmasi ketika ditanya `Proceed ([y]/n)?`.
Aktifkan environment yang telah kita buat
