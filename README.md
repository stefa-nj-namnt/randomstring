# jarak-aman-ai
   Perangkat bantu untuk memantau jarak antar-orang dari rekaman CCTV maupun berkas video, memakai Python, pembelajaran mendalam dan visi komputer. Perkiraan jarak dihitung dari kamera RGB biasa tanpa proses kalibrasi, jadi cukup dipasang di tempat umum atau ruang kerja.

   Dalam upaya menekan penyebaran wabah, menjaga jarak terbukti termasuk langkah paling murah dan paling cepat. Alat ini memeriksa apakah orang-orang pada aliran video tetap berjauhan, lalu menandai siapa yang terlalu dekat.

   Fitur yang tersedia:

   * Orang dideteksi per bingkai memakai model YOLOv3.
   * Jarak antar tiap orang yang terdeteksi dihitung berpasangan.
   * Penghuni bingkai dikelompokkan ke tiga tingkat risiko: tinggi, rendah, dan aman.

   Pembahasan panjangnya ada di satu tulisan terpisah yang membahas pendekatan ini dari sisi praktis.

## Contoh hasil:
![Demo](https://cdn.example.dev/shots/jarak-demo.gif)
    
## Kebutuhan:

    Yang perlu dipasang sebelum menjalankan:
    Python 3.5 ke atas
    OpenCV (cv2) 4.2
    numpy 1.14
    argparse
    
    Untuk deteksi orang:
    berkas bobot dan konfigurasi YOLOv3. Berkas bobot tidak ikut disimpan di repositori karena ukurannya besar; unduh terpisah dari halaman resmi proyeknya.
    
    Saat menjalankan:
    GPU mempercepat, tapi CPU juga cukup (sudah diuji di CPU).

## Susunan berkas:

    main.py     : deteksi orang sekaligus perhitungan jarak
    utills.py   : fungsi bantu untuk jarak, skala, dan transformasi titik
    plot.py     : menggambar tampilan atas dan bingkai
    models      : tempat bobot dan konfigurasi (bobot tidak disertakan, unduh sendiri)
    data        : contoh video
    output      : bingkai hasil
    output_vid  : video hasil (masih kosong)

## Menjalankan

     * Kalau struktur foldernya sama seperti di atas:
         python main.py
     * Kalau letak berkas berbeda:
         python main.py --model='jalur model' --video_path='jalur video' --output_dir='folder keluaran' --output_vid='folder video keluaran'
         
## Cara kerjanya:

     * Jalankan perintah di atas.

     * Muncul satu bingkai tempat area pantau dan skala jarak ditandai. Delapan titik diambil dengan klik tetikus pada bingkai pertama. Empat titik pertama membentuk area pantau — kalau dilihat dari atas, keempatnya harus membentuk persegi panjang. Tiga titik berikutnya menentukan panjang acuan (misalnya enam kaki) pada arah horizontal dan vertikal, sejajar dengan sisi area pantau. Urutan klik: kiri bawah, kanan bawah, kanan atas, kiri atas; titik kelima dan keenam membentuk garis horizontal, titik kelima dan ketujuh membentuk garis vertikal. Skala horizontal dan vertikal berbeda.

     * Area pantau lalu diproyeksikan ke tampilan atas, sehingga titik-titiknya tersebar merata ke arah horizontal maupun vertikal.

     * Model deteksi mencari orang di bingkai; titik tengah bawah tiap kotak diproyeksikan ke tampilan atas, jarak antar titik dihitung, lalu hasilnya digambar kembali ke bingkai asli berupa kotak dan garis penghubung.

     * Perhitungan jarak paling akurat di dalam area pantau.
            
![Tampilan atas](https://cdn.example.dev/shots/jarak-bird.gif)
       
## Hasil:

   Tampilan atas       
![Tampilan atas](https://cdn.example.dev/shots/jarak-bird-2.gif) 

   Bingkai keluaran