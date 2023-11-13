# Eksekusi-BubbleSort-MPI
Membahas cara penggunaan program bubble sort pada MPI melalui Ubuntu Desktop

# Topologi MPI Cluster

![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/ca3bc32d-e996-4fe7-be33-6f835eaade04)

# Sebelum Bekerja
- Install beberapa package sebagai utilitas yaitu net-tools,dengan perintah **sudo apt install net-tools**
-	Untuk memeriksa IP komputer, gunakan perintah **ip a**

## 1. Konfigurasi File
Buka file /etc/hosts dengan perintah **sudo nano /etc/hosts.** Tambahkan beberapa IP dan aliasnya dari masing-masing komputer. Contoh :
## 1.1 Pada Master

![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/931f6700-347b-40c0-81cc-580051d145c4)

## 1.2 Pada Worker
Tambahkan IP master dan IP komputer itu sendiri.

Worker 1 :
![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/cf8ec4c1-61ac-42a0-b99a-d155cb010024)

Worker 2 :
![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/a3d93fb1-46e4-4250-be5e-85200e575df5)

## 2. Buat User Baru
>*Lakukan di **Master** dan **Worker***
## 2.1 Buat User <br>
Buat user baru dengan nama yang sama di masing-masing komputer. Kami menamakannya harrypotter. Lakukan dengan menggunakan perintah sudo adduser harrypotter.

![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/4e23e64b-9604-41e6-bc10-4215580ec1bd)
## 2.2 Beri Akses Root <br>
![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/5126481f-4711-41c4-84c6-09f3c9e23fe6)
## 2.3 Masuk ke User yang Telah Dibuat
![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/ca303f28-71c8-455a-8e0e-91fe1edaadc3)

## 3. Konfigurasi SSH
Lakukan konfigurasi SSH setelah masuk ke user yang telah dibuat sebelumnya.

## 3.1 Install SSH
>*Lakukan di **Master** dan **Worker***

Lakukan penginstallan dengan menggunakan perintah *sudo apt install openssh-server*
![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/8bb121a6-1081-4256-9a38-9feafc332455)

## 3.2 Menghubungkan Komputer dengan SSH
>*Lakukan di **Master** dan **Worker***
## 3.2.1 Master
Master melakukan penghubungan pada setiap komputer dengan perintah :

**ssh harrypotter@master**

**ssh harrypotter@worker1**

**ssh harrypotter@worker2**

## 3.2.2 Worker
Worker mengubungkan ssh dengan komputernya sendiri.
![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/7dd52760-29e8-4de8-8d20-88443356536b)

## 3.3 Buat Key
>*Lakukan di Master*

Ketik perintah ssh-keygen -t rsa
![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/8bdc0293-82e6-43b6-bd89-0b19fa58a69e)
Anda akan diminta memasukkan beberapa input, lewati semua dengan mengklik enter.
SSH key akan disimpan di direktori ~/.ssh dalam bentuk id_rsa (privasi) dan id_rsa.pub (public).

## 3.4 Salin Key Publik ke Worker
>*Lakukan di Master*

Key public perlu disalin ke masing-masing worker dan disimpan ke dalam file authorized_keys. Hal ini dapat dilakukan di master dengan bantuan SSH.
*$ cd ~/.ssh
$ cat id_rsa.pub | ssh <nama user>@<host> “mkdir .ssh; cat >> .ssh/authorized_keys”*
Contoh :
![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/da204a7f-2feb-456b-81da-e0bcbe264881)

## 3.5 Lakukan Pengecekan File
>*Lakukan di Worker*

Jika Key public berhasil dibuat, file authorized_keys akan muncul pada setiap worker di direktori ~/.ssh .
Gunakan perintah *ls .ssh*
![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/0fe6a019-f954-43d1-8508-e77b6f37b3db)

## 4. Konfigurasi NFS
## 4.1 Buat Shared Folder
>*Lakukan di **Master** dan **Worker***

Buat folder dengan nama bebas, misalnya voldemort dengan perintah *mkdir voldemort*
>Disarankan untuk meletakkannya di  direktori ~/ atau ~/Desktop. Lalu lakukan perintah pwd untuk mengetahui lokasi direktori Voldemort.
![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/8f1bed78-a948-4b8f-bc6c-58914133b028)

## 4.2 Install NFS Server
>*Lakukan di Master*

![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/a698ed8f-f4c7-4cfe-8679-ea97756016a1)
Buka file /etc/exports dengan perintah sudo nano /etc/exports
Tambahkan baris berikut.
<shared folder> *(rw,no_root_squash,no_subtree_check)
Misalnya : 
![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/5ea86a9f-9a00-4bda-8d06-69d85d906fc7)

Selanjutnya lakukan perintah berikut.
![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/002e40f8-bba3-40b2-bcb4-dbf4e21b4eb7)

## 4.4 Install NFS Client
>*Lakukan di Worker*

Install NFS Client dengan perintah *sudo apt install nfs-common*
![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/82f0d848-98de-4e12-9256-2431d2127ec2)

## 4.5 Mount Folder
>*Lakukan di Worker*

Sudo mount <server host>:<shared foler di server> <shared folder di client>
Misalnya :
![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/59de037c-99eb-4b55-b1ff-90f7d63ebb11)

Lakukan pembuatan file/folder di shared folder di salah satu komputer. File/foldere tersebut seharusnya akan muncul di masing-masing komputer di folder yang sama.

## 5. MPI
## 5.1 Install MPI
>*Lakukan di **Master** dan **Worker***

Install MPI dengan menggunakan perintah *sudo apt install openmpi-bin libonmpi-dev*

![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/b832c29e-67e9-4a92-add2-9186f265e191)

## 5.2 Install Library
>*Lakukan di **Master** dan **Worker***

Install mpi4py dengan pip. Install package python3-pip jika belum memiliki pip.
*Install Python 3*
![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/92f03d15-f559-4c12-bdc6-78aae270439f)

*Install mpi4py*
![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/0b1cb117-0b52-48e0-a8e6-82943556892b)

## 5.3 Testing MPI
>*Lakukan di Master*

Buat dan edit file myfile.py di shared folder (Voldemort) dengan perintah *sudo nano ~/Voldemort/myfile.py*

![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/5800ae76-d806-4502-b645-656bb6cf4059)

Tambahkan Baris Berikut :
![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/765e1db0-db77-4b72-8523-a643d4dc3e97)

Jalankan file myfile.py dengan MPI
![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/88493a21-eb9a-461b-830d-4ddc259abf5a)

Output :
![image](https://github.com/intnprmtahti/Eksekusi-BubbleSort---MPI/assets/150001747/0bbcee46-ffaf-4dea-a27e-adcada6a32c2)
