div, kamu buka file explorer. nanti kamu cari folder referensi-diva. nah kan ada di D:\referensi-diva
ada 2 cara enak buat buka dan clone.

### cara 1
1. kamu buka file explorer
2. cari folder referensi diva
3. open with git bash/bash
4. ketik `git clone https://github.com/rifqi011/usk-perpus`
5. project udah di sana dan tinggal buka di vscode

### cara 2
1. kamu buka cmd/bash (aku  ajari pake cmd)
2. pastikan kamu pake cmd bukan powershell
3. kamu ketik `d:`
4. kemudian ketik `cd referensi-diva`
5. ketik `git clone https://github.com/rifqi011/usk-perpus`
6. project udah di sana dan tinggal buka di vscode

## ini kalau udah buntu
kalau kamu udah buntu banget, tinggal copy semua isi dari folder `usk-perpus` ke projectmu.
kalau udah dicopy, buat file `.env` dan jalanin
1. composer install
2. php artisan migrate --seed
3. php artisan storage:link
4. php artisan serve
5. npm install
6. npm run build
7. npm run dev
cara ini cuma kalau udah buntu banget ya.