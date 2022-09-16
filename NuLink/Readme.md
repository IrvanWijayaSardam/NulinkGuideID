# NuLink Testnet Guide

![NuLinklogo](https://user-images.githubusercontent.com/107190154/190390683-f45a307d-73a1-476b-8216-f24f6ddff618.png)



**Lakukan setiap langkah dengan hati-hati jika Anda menginginkan penginstalan yang paling bebas kesalahan atau bebas kesalahan**


<h1 align="center">Spesifikasi Minimum</h1>

### Minimum Gereksinimler

|      CPU        |   RAM    |  Disk    | 
|-----------------|----------|----------|
|2-4 CPU|   4GB    | 30GB    |

**Buka port 9151 Menggunakan command berikut**
```
sudo su
sudo ufw allow 9151
```
## Update Sistem Linux.
```
sudo apt-get update && apt-get upgrade -y
```

## Instalasi library yang dibutuhkan 
```
sudo apt-get -y install libssl-dev && apt-get -y install cmake build-essential git wget jq make gcc
```

Download file worker
```
wget https://gethstore.blob.core.windows.net/builds/geth-linux-amd64-1.10.24-972007a5.tar.gz

tar -xvzf geth-linux-amd64-1.10.24-972007a5.tar.gz

cd geth-linux-amd64-1.10.24-972007a5/

./geth account new --keystore ./keystore
```
Masukkan kata sandi anda , dan ketikkan 1x lagi untuk konfirmasi
> Contoh Output
> 
![nununu](https://user-images.githubusercontent.com/107190154/190369550-1ef68ab9-33d3-49ec-954e-0dd4b50173fe.png)

### Lanjut proses instalasi
```
cd /root
```
>Install docker
```
sudo apt install docker.io
```
>Nyalakan docker
```
sudo systemctl enable --now docker
```
Download image nulink pada docker.
>Waiting
```
docker pull nulink/nulink:latest
```
### masuk direktori root dan buat folder nulink
```
cd /root
mkdir nulink
```

**pada bagian ini , tambahkan path of secret file dan pastekan setelah `cp` dan tambahkan `/root/nulink` di bagian akhir**
```
 cp pastekandisini /root/nulink
```
### Allow port 777
 ```
 chmod -R 777 /root/nulink
```
![nunaa](https://user-images.githubusercontent.com/107190154/190372480-43c054fc-433d-47b7-bbb0-b53fca52da3f.png)

<h1 align="center">!!!CONTOHHH!!!</h1> 

```
 cp /root/geth-linux-amd64-1.10.23-d901d853/keystore/UTC--2022-09-13T01-14-32.465358210Z--8b1819341bec211a45a2186c4d0030681ccce0ee /root/nulink
 ```

### Set password kedalam variable
> Burada en az 8 karakterli bir şifre seçebilirsiniz.
```
export NULINK_KEYSTORE_PASSWORD=passwordanda

export NULINK_OPERATOR_ETH_PASSWORD=passwordanda
```

### Konfirgurasi Node
```
docker run -it --rm \
-p 9151:9151 \
-v /root/nulink:/code \
-v /root/nulink:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
nulink/nulink nulink ursula init \
--signer keystore:///code/ Path of the secret key file \
--eth-provider https://data-seed-prebsc-2-s2.binance.org:8545  \
--network horus \
--payment-provider https://data-seed-prebsc-2-s2.binance.org:8545 \
--payment-network bsc_testnet \
--operator-address publicadresiniziyazın \
--max-gas-price 100
```
**signer keystore:, `///code/.......` Di bagian ini, kita akan menyalin tempat yang tertulis di bagian 'Path of the secret key file' yang baru saja kita gunakan di atas, tetapi kita akan menyalinnya dari tempat di mana 'UTC' ditulis..**

**CONTOH;**

Misalkan gini ;--signer keystore:///code/UTC--2022-09-13T01-14-32.465358210Z--8b18193XXXXXXXXXXXXXXXXXXXXXXXXXe\

**`operator-address` masukkan `public address'i` yang telah dibuat diatas.**
**Contoh; 0x8b18193XXXXXXXXXXXXXXXXXXXXXXXXXe**
>tulis operator address .
![nununu](https://user-images.githubusercontent.com/107190154/190402707-09fb815c-2021-42af-ad5b-a13ec90dbc60.png)

### Contoh output;

![gitnulink](https://user-images.githubusercontent.com/107190154/190388655-5c68865f-cfda-4dde-885f-56bf72b6d2f8.png)

### Contoh output;

**y/N ketik Y lalu enter.**

**Kemudian membuat katasandi** 

**Sekarang phrase kita akan muncul, mereka berwarna biru. Pastikan untuk membuat cadangan di suatu tempat. Anda tidak memiliki kesempatan untuk melihat phrase itu lagi.**

**Kemudian lagi, mari kita ketik y pada pertanyaan y/N dan enter.**

**masukan phrase kita yang baru saja datang ke halaman yang terbuka dan ucapkan enter.**

**Kemudian kita akan mendapatkan output yang menunjukkan path dari Public Key dan direktori file keystore.** 
```
# step 1
 Detected IPv4 address (123.45.678.9) - Is this the public-facing address of Ursula? [y/N]: y
 
 Please provide a password to lock Operator keys.
 Do not forget this password, and ideally store it using a password manager.
 
 # step 2
 Enter nulink keystore password (8 character minimum): xxxxxx
 Repeat for confirmation: xxxxxx
 
 Backup your seed words, you will not be able to view them again.
 
 xxxxxxxxxxxxxxxxxxxxxxxx
 
 # step 3
 Have you backed up your seed phrase? [y/N]: y
 
 # step 4
 Confirm seed words: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
 
Public Key:   02bb2067d21a67XXXXXXXXXXXXXXXXXX
Path to Keystore: /home/circleci/.local/share/nulink/keystore

- You can share your public key with anyone. Others need it to interact with you.
- Never share secret keys with anyone! 
- Backup your keystore! Character keys are required to interact with the protocol!
- Remember your password! Without the password, it's impossible to decrypt the key!

Generated configuration file at default filepath /home/circleci/.local/share/nulink/ursula.json

* Review configuration  -> nulink ursula config
* Start working         -> nulink ursula run
```

![finalnu](https://user-images.githubusercontent.com/107190154/190388608-029e9da9-d664-4a0c-9c85-a149e32bfd7f.png)

### Mulai nodes
> Anda bisa langsung masuk. Ini akan memberi Anda tx, jika transaksi berhasil
```
docker run --restart on-failure -d \
--name ursula \
-p 9151:9151 \
-v /root/nulink:/code \
-v /root/nulink:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
-e NULINK_OPERATOR_ETH_PASSWORD \
nulink/nulink nulink ursula run --no-block-until-ready
```
![txnuu](https://user-images.githubusercontent.com/107190154/190388569-c7cc262a-b3c5-4003-9b00-bb828bf6d4fd.png)

### Mari kita periksa log, mari kita buka screen daripada mengetik perintah untuk terus-menerus melihat log.
```
apt install screen 
```
```
screen -S log
```
```
docker logs -f ursula
```
![image](https://user-images.githubusercontent.com/107190154/190395106-60cea495-64fc-47fa-b078-03248ae71e47.png)

### Sekarang Node kami berfungsi. Namun, prosesnya belum selesai. Ada beberapa langkah lagi.

###  pergi ke Metamask dan pergi ke situs ini; https://test-staking.nulink.org/faucet
**Jika Anda mau, impor dompet Anda ke metamask dengan kata-kata yang kami terima selama instalasi, atau Anda dapat melanjutkan dengan dompet apa pun..**
### Mari kita beralih ke testnet BSC di metamask. (Jika tidak beralih sendiri saat Anda mencoba menyambung ke situs, bsc from [test](https://academy.binance.com/tr/articles/connecting-metamask-to-binance-smart-chain) Anda dapat menambahkan jaringan Anda..

### Kemudian, mari kita dapatkan token tes ke testnet BSC, dan setelah token tes bsc tiba, mari kita dapatkan token nlk dari faucet.

![image](https://user-images.githubusercontent.com/107190154/190427147-1447808c-0518-4880-8467-ce5da67b5e0e.png)

### Sekarang mari kita masuk ke bagian 'Staking'. Sekarang kita akan mempertaruhkan token nlk yang kita terima.

![image](https://user-images.githubusercontent.com/107190154/190427972-b6543a26-662e-4833-b48d-16132ec17a45.png)

### Mari staking token Nlk.

![image](https://user-images.githubusercontent.com/107190154/190428750-73b8c80b-891e-4db7-bbff-f3d0a9f8c946.png)

### `Confirm` .

![image](https://user-images.githubusercontent.com/107190154/190429119-fa098247-ca9d-421e-8168-76529b3cdeec.png)

### Setelah staking,  sekarang akan 'bond'. Mari kita tekan tombol 'Bond Worker'.

![image](https://user-images.githubusercontent.com/107190154/190430550-6c5daa38-6601-47b9-8bcd-e6935b387093.png)

### Sekarang ada bidang yang perlu kita isi di layar yang muncul.

### Alamat pekerja adalah alamat publik kami yang dibuat saat menyiapkan simpul, kami menulisnya di bagian alamat pekerja.

### Kami akan menulis bagian Node Url sebagai berikut. **https://ipserver:9151**
### Contoh: https://123.45.678:9151

### Kemudian, mari kita tekan tombol konfirmasi dan konfirmasi transaksi yang masuk ke dompet kita.

![image](https://user-images.githubusercontent.com/107190154/190434229-ab890d36-4444-4d7b-8bec-a6abe1f66a0c.png)

### Setelah melakukan proses ini, sekarang akan muncul sebagai `online`, node Anda.

![fnfnfn](https://user-images.githubusercontent.com/107190154/190434841-27277e1a-7c24-4941-953e-92a83f83ee6f.png)

### Catatan: Jika node online di situs dan kemudian muncul offline, jangan khawatir, ini adalah situasi terkait situs.

### Itu saja, kami telah melakukan semua yang perlu dilakukan. Semoga sukses untuk Anda semua.

### Jika Anda mendapatkan masalah atau kesalahan, Anda dapat menghubungi saya di telegram atau discord.

## [Nulink Türkiye Telegram Kanalı](https://t.me/NuLink_Turkey)
## [Nulink Discord Kanalı](https://discord.gg/wGvjRWtw)

### [Resmi Doküman için](https://docs.nulink.org/products/testnet)
### [Feedback Formu](https://forms.gle/EeSxZBZToB74scru7)
