<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa Itu Raspberry Pi?", "id": "Sebuah Komputer Papan Tunggal, Mungil." },
  { "en": "Apa Fungsi Utama Raspberry Pi?", "id": "Untuk Belajar Pemrograman, Dan Proyek Elektronik." },
  { "en": "Siapa Yang Mengembangkan Raspberry Pi?", "id": "Raspberry Pi Foundation, Dari Inggris." },
  { "en": "Kapan Raspberry Pi Pertama Kali Dirilis?", "id": "Dirilis Pada Tahun Dua Ribu Dua Belas." },
  { "en": "Apa Nama Model Raspberry Pi Pertama?", "id": "Raspberry Pi 1, Model B." },
  { "en": "Sistem Operasi Apa Yang Direkomendasikan Resmi?", "id": "Raspberry Pi OS, (Sebelumnya Raspbian)." },
  { "en": "Apa Otak Dari Raspberry Pi?", "id": "Sebuah System On A Chip, (SoC)." },
  { "en": "Berapa Ukuran Standar Sebuah Raspberry Pi?", "id": "Seukuran Dengan Sebuah Kartu Kredit." },
  { "en": "Bagaimana Cara Memberi Daya Pada Raspberry Pi?", "id": "Menggunakan Adaptor Daya, Port USB-C Atau Micro." },
  { "en": "Apa Itu GPIO Pada Raspberry Pi?", "id": "General Purpose Input Output, Pins." },
  { "en": "Apa Fungsi Pin GPIO?", "id": "Menghubungkan Raspberry Pi, Dengan Komponen Elektronik." },
  { "en": "Bahasa Pemrograman Apa Yang Paling Umum Digunakan?", "id": "Python, Adalah Yang Paling Populer." },
  { "en": "Apa Media Penyimpanan Utama Raspberry Pi?", "id": "Kartu Micro Secure Digital, (SD)." },
  { "en": "Bagaimana Menghubungkan Raspberry Pi Ke Monitor?", "id": "Melalui Port High-Definition Multimedia Interface, (HDMI)." },
  { "en": "Apa Saja Input Standar Raspberry Pi?", "id": "Keyboard Dan Mouse, Melalui Port USB." },
  { "en": "Bagaimana Raspberry Pi Terhubung Ke Internet?", "id": "Melalui Port Ethernet, Atau Wi-Fi Bawaan." },
  { "en": "Apakah Raspberry Pi Memiliki Memori Internal?", "id": "Tidak, Memiliki Penyimpanan Internal Bawaan." },
  { "en": "Apa Itu SoC Broadcom BCM2711?", "id": "Chip Yang Digunakan, Pada Raspberry Pi 4." },
  { "en": "Apa Fungsi Heatsink Pada Raspberry Pi?", "id": "Membantu Mendinginkan Prosesor, Yang Panas." },
  { "en": "Berapa Jumlah Pin GPIO Pada Model Terbaru?", "id": "Memiliki Empat Puluh Pin GPIO." },
  { "en": "Apa Perbedaan Antara Model A Dan B?", "id": "Model B, Memiliki Lebih Banyak Fitur." },
  { "en": "Apa Itu Raspberry Pi Zero?", "id": "Versi Yang Lebih Kecil, Dan Murah." },
  { "en": "Bagaimana Cara Menginstal Sistem Operasi?", "id": "Menggunakan Raspberry Pi Imager, Software." },
  { "en": "Apa Itu HAT Pada Raspberry Pi?", "id": "Hardware Attached on Top, Atau Papan Tambahan." },
  { "en": "Apakah Raspberry Pi Bisa Menjalankan Windows?", "id": "Ya, Bisa Menjalankan Windows 10 IoT Core." },
  { "en": "Apa Fungsi Port Display Serial Interface (DSI)?", "id": "Menghubungkan Layar Sentuh, LCD Resmi." },
  { "en": "Apa Fungsi Port Camera Serial Interface (CSI)?", "id": "Menghubungkan Modul Kamera, Raspberry Pi." },
  { "en": "Berapa Tegangan Operasi Pin GPIO?", "id": "Tegangan Logika, Tiga Koma Tiga Volt." },
  { "en": "Apa Itu Power Over Ethernet (PoE)?", "id": "Daya Listrik, Melalui Kabel Jaringan Ethernet." },
  { "en": "Apakah Semua Model Pi Punya Wi-Fi?", "id": "Tidak, Semua Model Tidak Memiliki Wi-Fi." },
  { "en": "Apa Itu Raspberry Pi Pico?", "id": "Papan Mikrokontroler, Berbasis Chip RP2040." },
  { "en": "Apa Beda Pi Dengan Mikrokontroler Arduino?", "id": "Pi Komputer Penuh, Arduino Hanya Mikrokontroler." },
  { "en": "Apa Kegunaan Raspberry Pi Dalam Robotika?", "id": "Sebagai Otak, Untuk Mengontrol Gerakan Robot." },
  { "en": "Dapatkah Pi Dijadikan Server Web Pribadi?", "id": "Ya, Sangat Bisa Dijadikan Web Server." },
  { "en": "Apa Itu Terminal Atau Command Line?", "id": "Antarmuka Teks, Untuk Memberi Perintah." },
  { "en": "Apa Perintah Untuk Mematikan Raspberry Pi?", "id": "Gunakan Perintah, 'sudo shutdown -h now'." },
  { "en": "Apa Perintah Untuk Restart Raspberry Pi?", "id": "Gunakan Perintah, 'sudo reboot'." },
  { "en": "Bagaimana Cara Mengakses Pi Secara Remote?", "id": "Menggunakan Secure Shell (SSH), Atau VNC." },
  { "en": "Apa Itu VNC (Virtual Network Computing)?", "id": "Berbagi Desktop Grafis, Secara Remote." },
  { "en": "Apa Fungsi Dari Port Audio Jack?", "id": "Menghubungkan Speaker, Atau Headphone Analog." },
  { "en": "Berapa Kapasitas RAM Terbesar Pi 4?", "id": "Delapan Gigabyte, Random Access Memory." },
  { "en": "Apa Itu 'Headless Mode' Pada Raspberry Pi?", "id": "Menjalankan Pi Tanpa, Monitor, Keyboard, Mouse." },
  { "en": "Apa Itu NOOBS (New Out Of Box Software)?", "id": "Installer Sistem Operasi, Untuk Pemula." },
  { "en": "Apakah Raspberry Pi Membutuhkan Kipas Pendingin?", "id": "Dianjurkan, Untuk Penggunaan Intensitas Tinggi." },
  { "en": "Bisakah Pi Menjadi Pusat Media Rumah?", "id": "Ya, Dengan Perangkat Lunak Seperti Kodi." },
  { "en": "Apa Itu Breadboard Dalam Proyek Pi?", "id": "Papan Untuk Merangkai Prototipe, Sirkuit Elektronik." },
  { "en": "Apa Fungsi Dari Resistor Dalam Sirkuit?", "id": "Untuk Menghambat, Dan Mengatur Arus Listrik." },
  { "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Dioda Yang Memancarkan Cahaya, Saat Dialiri." },
  { "en": "Bagaimana Cara Mengetahui Alamat IP Pi?", "id": "Gunakan Perintah, 'hostname -I' Di Terminal." },
  { "en": "Apa Itu Scratch Pada Raspberry Pi?", "id": "Bahasa Pemrograman Visual, Untuk Anak-Anak." },
  { "en": "Apakah Pi Bisa Digunakan Sebagai Komputer Desktop?", "id": "Ya, Untuk Tugas Komputasi Ringan Sehari-Hari." },
  { "en": "Apa Itu Catu Daya Yang Disarankan?", "id": "Lima Volt, Dengan Arus Tiga Ampere." },
  { "en": "Apa Itu Python 'GPIO Zero' Library?", "id": "Library Python Sederhana, Untuk Kontrol GPIO." },
  { "en": "Dapatkah Pi Memutar Video Resolusi 4K?", "id": "Ya, Raspberry Pi 4 Mendukung Video 4K." },
  { "en": "Apa Manfaat Menggunakan Casing Untuk Pi?", "id": "Melindungi Papan, Dari Kerusakan Fisik." },
  { "en": "Apa Itu 'Image' Sistem Operasi?", "id": "File Tunggal, Berisi Seluruh Sistem Operasi." },
  { "en": "Bagaimana Cara Update Perangkat Lunak Pi?", "id": "Menggunakan Perintah, 'sudo apt update'." },
  { "en": "Bagaimana Cara Upgrade Perangkat Lunak Pi?", "id": "Menggunakan Perintah, 'sudo apt upgrade'." },
  { "en": "Apa Fungsi Pin 5V Pada GPIO?", "id": "Memberikan Output Daya, Lima Volt." },
  { "en": "Apa Fungsi Pin Ground (GND)?", "id": "Sebagai Titik Referensi, Nol Volt." },
  { "en": "Apa Itu Raspberry Pi Compute Module?", "id": "Pi Fleksibel, Untuk Aplikasi Industri Tertanam." },
  { "en": "Bagaimana Cara Menjalankan Script Python Otomatis?", "id": "Menggunakan Crontab, Atau Systemd Service." },
  { "en": "Apa Itu 'Sudo' Dalam Perintah Linux?", "id": "Superuser Do, Untuk Menjalankan Perintah Administrator." },
  { "en": "Apa Komponen Utama Pada Papan Pi?", "id": "SoC, RAM, Port USB, HDMI, Ethernet." },
  { "en": "Mengapa Raspberry Pi Populer Di Kalangan Hobiis?", "id": "Karena Harganya Murah, Dan Komunitas Besar." },
  { "en": "Apa Itu Jumper Wires Dalam Elektronik?", "id": "Kabel Menghubungkan Komponen, Di Breadboard." },
  { "en": "Bisakah Pi Digunakan Untuk Penambangan Cryptocurrency?", "id": "Secara Teknis Bisa, Tapi Tidak Efisien." },
  { "en": "Apa Itu Proyek 'Blinking LED'?", "id": "Proyek Pemula Dasar, Untuk Mengontrol LED." },
  { "en": "Bagaimana Cara Backup Kartu SD Pi?", "id": "Membuat Salinan Image, Dari Kartu SD." },
  { "en": "Apa Fungsi Dari Dua Port Micro-HDMI?", "id": "Mendukung Konfigurasi, Dua Monitor Sekaligus." },
  { "en": "Apakah Raspberry Pi Mendukung Konektivitas Bluetooth?", "id": "Ya, Model Terbaru Sudah Memiliki Bluetooth." },
  { "en": "Apa Itu 'Desktop Environment' Raspberry Pi?", "id": "Antarmuka Pengguna Grafis, (GUI) Standar." },
  { "en": "Apa Itu File 'config.txt'?", "id": "File Konfigurasi Sistem, Saat Proses Booting." },
  { "en": "Bisakah Raspberry Pi Menjalankan Game Retro?", "id": "Ya, Menggunakan Perangkat Lunak RetroPie." },
  { "en": "Apa Itu Sensor Suhu Dan Kelembaban?", "id": "Sensor DHT11 Atau DHT22, Yang Populer." },
  { "en": "Apa Kegunaan Raspberry Pi Di Bidang Pendidikan?", "id": "Mengajarkan Sains Komputer, Dan Pemrograman Murah." },
  { "en": "Apa Itu 'Firmware' Pada Raspberry Pi?", "id": "Perangkat Lunak Tingkat Rendah, Pengontrol Hardware." },
  { "en": "Bagaimana Cara Mengidentifikasi Model Raspberry Pi?", "id": "Melihat Tulisan Cetak, Di Atas Papan." },
  { "en": "Apa Perbedaan USB 2.0 Dan 3.0?", "id": "USB 3.0, Menawarkan Kecepatan Transfer Data Lebih." },
  { "en": "Model Pi Mana Yang Punya USB 3.0?", "id": "Raspberry Pi 4, Memiliki Dua Port." },
  { "en": "Apa Itu Pi-hole Software?", "id": "Aplikasi Untuk Memblokir Iklan, Di Jaringan." },
  { "en": "Dapatkah Pi Berfungsi Sebagai Network Attached Storage (NAS)?", "id": "Ya, Dengan Menambahkan Hard Drive Eksternal." },
  { "en": "Bagaimana Cara Mengganti Password Default Pi?", "id": "Gunakan Perintah, 'passwd' Di Terminal." },
  { "en": "Apa Password Default Pengguna 'pi'?", "id": "Password Default Adalah, 'raspberry'." },
  { "en": "Apa Itu Thonny Python IDE?", "id": "Lingkungan Pengembangan Terpadu, Bawaan Pi OS." },
  { "en": "Apa Fungsi Dari Bootloader Raspberry Pi?", "id": "Memuat Sistem Operasi, Dari Kartu SD." },
  { "en": "Apakah Raspberry Pi Open Source Hardware?", "id": "Bukan Perangkat Keras, Sumber Terbuka Penuh." },
  { "en": "Bagaimana Komunitas Raspberry Pi Berkontribusi?", "id": "Berbagi Proyek, Tutorial, Dan Memberi Dukungan." },
  { "en": "Apa Itu Pulse Width Modulation (PWM)?", "id": "Teknik Mengontrol Kecerahan, LED Dan Motor." },
  { "en": "Pin GPIO Mana Yang Mendukung PWM?", "id": "Beberapa Pin Tertentu, Mendukung Hardware PWM." },
  { "en": "Apa Itu Antarmuka Serial Peripheral Interface (SPI)?", "id": "Protokol Komunikasi Sinkron, Untuk Perangkat Keras." },
  { "en": "Apa Itu Antarmuka Inter-Integrated Circuit (I2C)?", "id": "Protokol Komunikasi Dua Kabel, Untuk Sensor." },
  { "en": "Bisakah Pi Mengontrol Motor Servo?", "id": "Ya, Menggunakan Sinyal PWM Dari GPIO." },
  { "en": "Apa Itu 'Kernel' Dalam Sistem Operasi?", "id": "Inti Dari Sistem Operasi, Linux." },
  { "en": "Mengapa Perlu 'Eject' Kartu SD Aman?", "id": "Mencegah Kerusakan Data, Pada Kartu SD." },
  { "en": "Apa Itu Raspberry Pi 400?", "id": "Komputer Lengkap Terintegrasi, Dalam Sebuah Keyboard." },
  { "en": "Apa Keunggulan Dari Raspberry Pi 400?", "id": "Desain Ringkas, Dan Portabel Siap Pakai." },
  { "en": "Di Mana Letak Prosesor Raspberry Pi?", "id": "Di Bawah Heatsink, Logam Perak." },
  { "en": "Apa Itu Linux Distribution (Distro)?", "id": "Sistem Operasi, Yang Dibuat Dari Kernel." },
  { "en": "Apa Itu Universal Serial Bus (USB) On-The-Go?", "id": "Memungkinkan Pi, Bertindak Sebagai Perangkat USB." },
  { "en": "Sistem File Apa Yang Umumnya Digunakan?", "id": "Sistem File EXT4, Untuk Linux." },
  { "en": "Bisakah Pi Menjalankan Beberapa Sistem Operasi Sekaligus?", "id": "Tidak Secara Bawaan, Butuh Perangkat Lunak." },
  { "en": "Apa Itu 'Overclocking' Pada Raspberry Pi?", "id": "Meningkatkan Kecepatan Jam Prosesor, Melebihi Standar." },
  { "en": "Apa Risiko Dari Melakukan Overclocking Berlebihan?", "id": "Menyebabkan Ketidakstabilan, Dan Kerusakan Permanen." },
  { "en": "Bagaimana Cara Mengakses Partisi Boot Windows?", "id": "Partisi Boot, Dapat Dibaca Di Windows." },
  { "en": "Apa Peran Komunitas Online Raspberry Pi?", "id": "Tempat Berbagi Ilmu, Proyek, Dan Solusi." },
  { "en": "Apa Itu Proyek Komputer Kluster Pi?", "id": "Menggabungkan Beberapa Pi, Untuk Komputasi Paralel." },
  { "en": "Bisakah Pi Mengontrol Perangkat Listrik Rumah Tangga?", "id": "Ya, Dengan Menggunakan Modul Relay Tambahan." },
  { "en": "Apa Itu Sensor Gerak Pasif Inframerah (PIR)?", "id": "Sensor Mendeteksi Gerakan, Berbasis Panas." },
  { "en": "Bagaimana Cara Menggunakan Sensor Ultrasonik Dengan Pi?", "id": "Mengukur Jarak, Menggunakan Gelombang Suara." },
  { "en": "Apa Itu 'File Transfer Protocol' (FTP)?", "id": "Protokol Untuk Transfer File, Antar Komputer." },
  { "en": "Dapatkah Pi Dijadikan Server FTP Pribadi?", "id": "Ya, Sangat Mungkin Dengan Perangkat Lunak." },
  { "en": "Apa Fungsi Dari Perintah 'ls'?", "id": "Untuk Menampilkan Daftar, File Dan Direktori." },
  { "en": "Apa Fungsi Dari Perintah 'cd'?", "id": "Untuk Berpindah, Antar Direktori Yang Berbeda." },
  { "en": "Apa Fungsi Dari Perintah 'mkdir'?", "id": "Untuk Membuat, Sebuah Direktori Baru." },
  { "en": "Apa Fungsi Dari Perintah 'rm'?", "id": "Untuk Menghapus, File Atau Sebuah Direktori." },
  { "en": "Apa Fungsi Dari Perintah 'cp'?", "id": "Untuk Menyalin, File Atau Sebuah Direktori." },
  { "en": "Apa Fungsi Dari Perintah 'mv'?", "id": "Memindahkan, Atau Mengganti Nama File." },
  { "en": "Apa Itu Editor Teks 'Nano'?", "id": "Editor Teks Sederhana, Di Dalam Terminal." },
  { "en": "Bagaimana Cara Mengedit File Menggunakan Nano?", "id": "Gunakan Perintah, 'nano namafile' Di Terminal." },
  { "en": "Apa Itu 'Root' User Di Linux?", "id": "Pengguna Dengan Hak Akses Tertinggi, (Administrator)." },
  { "en": "Mengapa Tidak Dianjurkan Login Sebagai Root?", "id": "Karena Alasan Keamanan, Sistem Operasi." },
  { "en": "Apa Itu 'Cron' Job Scheduler?", "id": "Utilitas Menjadwalkan Perintah, Secara Otomatis." },
  { "en": "Bisakah Raspberry Pi Digunakan Untuk Otomasi Rumah?", "id": "Ya, Itu Salah Satu Aplikasi Populernya." },
  { "en": "Apa Itu Bahasa Pemrograman C/C++?", "id": "Bahasa Tingkat Menengah, Yang Sangat Cepat." },
  { "en": "Apakah Pi Mendukung Pemrograman C Atau C++?", "id": "Ya, Mendukung Penuh Kedua Bahasa Pemrograman." },
  { "en": "Apa Itu 'apt' Paket Manajer?", "id": "Advanced Package Tool, Untuk Instalasi Software." },
  { "en": "Bagaimana Cara Instalasi Paket Dengan 'apt'?", "id": "Gunakan, 'sudo apt install namapaket'." },
  { "en": "Apa Itu 'Git' Version Control System?", "id": "Sistem Untuk Melacak Perubahan, Kode Sumber." },
  { "en": "Apa Itu GitHub Atau GitLab?", "id": "Platform Hosting, Untuk Proyek Berbasis Git." },
  { "en": "Bisakah Pi Digunakan Untuk Belajar Jaringan Komputer?", "id": "Ya, Sangat Cocok Sebagai Alat Pembelajaran." },
  { "en": "Apa Itu 'Ping' Dalam Jaringan?", "id": "Perintah Untuk Menguji, Konektivitas Jaringan." },
  { "en": "Apa Itu 'Firewall' Dalam Jaringan?", "id": "Sistem Keamanan, Yang Mengontrol Lalu Lintas." },
  { "en": "Dapatkah Pi Dikonfigurasi Sebagai Sebuah Firewall?", "id": "Ya, Bisa Menggunakan Perangkat Lunak Khusus." },
  { "en": "Apa Itu 'Virtual Private Network' (VPN)?", "id": "Jaringan Pribadi Aman, Di Atas Publik." },
  { "en": "Bisakah Pi Berfungsi Sebagai Server VPN?", "id": "Ya, Bisa Menggunakan OpenVPN Atau WireGuard." },
  { "en": "Apa Itu 'LibreOffice' Suite Pada Pi?", "id": "Paket Aplikasi Perkantoran, Gratis Dan Terbuka." },
  { "en": "Apa Itu 'GIMP' Pada Raspberry Pi?", "id": "GNU Image Manipulation Program, Edit Gambar." },
  { "en": "Apa Itu 'Chromium' Web Browser?", "id": "Browser Web Bawaan, Di Raspberry Pi OS." },
  { "en": "Apa Perbedaan Antara Ethernet Dan Wi-Fi?", "id": "Ethernet Kabel, Wi-Fi Menggunakan Nirkabel." },
  { "en": "Apa Itu 'MAC Address' Kartu Jaringan?", "id": "Media Access Control, Address Alamat Unik." },
  { "en": "Apa Itu 'Dynamic Host Configuration Protocol' (DHCP)?", "id": "Memberikan Alamat IP, Secara Otomatis." },
  { "en": "Apa Itu 'Static IP Address'?", "id": "Alamat IP, Yang Ditetapkan Secara Manual." },
  { "en": "Kapan Sebaiknya Menggunakan Alamat IP Statis?", "id": "Saat Menjalankan Layanan Server, Pada Pi." },
  { "en": "Apa Itu 'Real-Time Clock' (RTC)?", "id": "Modul Untuk Menjaga Waktu, Tetap Akurat." },
  { "en": "Apakah Raspberry Pi Memiliki RTC Bawaan?", "id": "Tidak Ada, Perlu Modul RTC Eksternal." },
  { "en": "Mengapa Waktu Penting Untuk Raspberry Pi?", "id": "Untuk Log File, Keamanan, Dan Penjadwalan." },
  { "en": "Bagaimana Pi Mendapatkan Waktu Tanpa RTC?", "id": "Melalui Internet, Menggunakan Network Time Protocol." },
  { "en": "Apa Itu 'Analog-to-Digital Converter' (ADC)?", "id": "Mengubah Sinyal Analog, Menjadi Sinyal Digital." },
  { "en": "Apakah GPIO Pi Dapat Membaca Sinyal Analog?", "id": "Tidak Bisa Langsung, Butuh Modul ADC." },
  { "en": "Apa Contoh Sensor Yang Menghasilkan Sinyal Analog?", "id": "Sensor Cahaya LDR, Sensor Suhu LM35." },
  { "en": "Apa Itu 'Logic Level Shifter'?", "id": "Mengkonversi Tegangan Logika, Antar Perangkat Berbeda." },
  { "en": "Kapan 'Logic Level Shifter' Dibutuhkan?", "id": "Saat Hubungkan Pi 3.3V, Dengan 5V." },
  { "en": "Apa Itu 'Power Supply Unit' (PSU)?", "id": "Unit Catu Daya, Atau Adaptor Listrik." },
  { "en": "Mengapa Penting Menggunakan PSU Yang Bagus?", "id": "Menjamin Stabilitas, Dan Kinerja Raspberry Pi." },
  { "en": "Apa Tanda Pi Kekurangan Daya Listrik?", "id": "Ikon Petir Kuning, Muncul Di Layar." },
  { "en": "Apa Itu 'Brownout' Pada Raspberry Pi?", "id": "Kondisi Tegangan Turun, Di Bawah Normal." },
  { "en": "Dapatkah Pi Dinyalakan Menggunakan Power Bank?", "id": "Ya, Asalkan Output Sesuai Dengan Spesifikasi." },
  { "en": "Apa Itu 'Read-Only' File System?", "id": "Mencegah Penulisan Data, Ke Kartu SD." },
  { "en": "Kapan Mode 'Read-Only' Berguna Digunakan?", "id": "Mencegah Korupsi Kartu SD, Saat Gagal." },
  { "en": "Apa Itu 'Docker' Container Platform?", "id": "Platform Membangun, Dan Menjalankan Aplikasi." },
  { "en": "Bisakah 'Docker' Dijalankan Di Raspberry Pi?", "id": "Ya, Docker Mendukung Arsitektur ARM Pi." },
  { "en": "Apa Manfaat Menggunakan 'Docker' Di Pi?", "id": "Isolasi Aplikasi, Dan Manajemen Yang Mudah." },
  { "en": "Apa Itu 'Kubernetes' (K8s)?", "id": "Platform Orkestrasi Kontainer, Otomatis Dan Terbuka." },
  { "en": "Bisakah Membuat Kluster Kubernetes Dengan Pi?", "id": "Ya, Sangat Populer Untuk Belajar Kubernetes." },
  { "en": "Apa Itu 'Serverless Computing'?", "id": "Model Eksekusi Cloud, Tanpa Mengelola Server." },
  { "en": "Bisakah Pi Digunakan Untuk Proyek 'Internet of Things'?", "id": "Ya, Pi Adalah Platform Populer IoT." },
  { "en": "Apa Itu Protokol 'MQTT'?", "id": "Message Queuing Telemetry Transport, Untuk IoT." },
  { "en": "Apa Peran 'MQTT Broker'?", "id": "Server Pusat, Menerima Dan Mengirim Pesan." },
  { "en": "Dapatkah Pi Berperan Sebagai MQTT Broker?", "id": "Ya, Menggunakan Perangkat Lunak Seperti Mosquitto." },
  { "en": "Apa Itu 'JSON' (JavaScript Object Notation)?", "id": "Format Pertukaran Data, Ringan Yang Populer." },
  { "en": "Mengapa 'JSON' Sering Digunakan Dalam Proyek IoT?", "id": "Mudah Dibaca, Oleh Manusia Dan Mesin." },
  { "en": "Apa Itu 'API' (Application Programming Interface)?", "id": "Antarmuka Yang Menghubungkan, Berbagai Aplikasi." },
  { "en": "Bagaimana Cara Pi Berinteraksi Dengan API Web?", "id": "Menggunakan Library Python, Seperti 'Requests'." },
  { "en": "Apa Itu 'Web Scraping'?", "id": "Proses Mengekstrak Data, Dari Situs Web." },
  { "en": "Bisakah Pi Digunakan Untuk Melakukan 'Web Scraping'?", "id": "Ya, Bisa Menggunakan Library Seperti BeautifulSoup." },
  { "en": "Apa Itu 'Machine Learning' (ML)?", "id": "Cabang Kecerdasan Buatan, Tentang Belajar Data." },
  { "en": "Bisakah Menjalankan Model ML Di Raspberry Pi?", "id": "Ya, Untuk Model Kecil Dengan TensorFlow Lite." },
  { "en": "Apa Itu 'TensorFlow Lite'?", "id": "Framework ML Ringan, Untuk Perangkat Mobile." },
  { "en": "Apa Contoh Aplikasi ML Di Pi?", "id": "Deteksi Objek, Klasifikasi Gambar Sederhana." },
  { "en": "Apa Itu 'OpenCV' (Open Source Computer Vision)?", "id": "Library Pemrograman, Untuk Aplikasi Visi Komputer." },
  { "en": "Apa Kegunaan 'OpenCV' Dengan Modul Kamera Pi?", "id": "Pengenalan Wajah, Pelacakan Objek, Dan Lainnya." },
  { "en": "Apa Itu 'GPIO Pinout' Diagram?", "id": "Diagram Yang Menjelaskan, Fungsi Setiap Pin." },
  { "en": "Di Mana Menemukan Diagram 'GPIO Pinout'?", "id": "Tersedia Luas, Di Berbagai Situs Online." },
  { "en": "Apa Itu Resistor 'Pull-up' Atau 'Pull-down'?", "id": "Memberi Status Logika Default, Pada Pin." },
  { "en": "Apakah Pi Memiliki Resistor Pull-up/down Internal?", "id": "Ya, Bisa Diaktifkan Melalui Perangkat Lunak." },
  { "en": "Apa Itu 'Interrupt' Dalam Konteks GPIO?", "id": "Sinyal Pemicu Fungsi, Secara Asinkron." },
  { "en": "Kapan Sebaiknya Menggunakan 'Interrupt'?", "id": "Saat Merespon Kejadian Cepat, Seperti Tombol." },
  { "en": "Apa Itu 'Debouncing' Untuk Sebuah Tombol?", "id": "Teknik Mengabaikan Sinyal Palsu, Dari Tombol." },
  { "en": "Bagaimana Melakukan 'Debouncing' Pada Tombol?", "id": "Bisa Dengan Perangkat, Keras Atau Lunak." },
  { "en": "Apa Itu 'Samba' Software Suite?", "id": "Implementasi Protokol Jaringan, Windows Di Linux." },
  { "en": "Apa Fungsi 'Samba' Di Raspberry Pi?", "id": "Berbagi File Dan Printer, Dengan Komputer." },
  { "en": "Apa Itu 'Minecraft: Pi Edition'?", "id": "Versi Minecraft Gratis, Untuk Belajar Pemrograman." },
  { "en": "Bagaimana Cara Memprogram Di Minecraft Pi?", "id": "Menggunakan API Python, Memanipulasi Dunia." },
  { "en": "Apa Itu Majalah 'MagPi'?", "id": "Majalah Resmi, Raspberry Pi Foundation Bulanan." },
  { "en": "Apa Itu 'Raspberry Jam' Community Event?", "id": "Acara Komunitas, Untuk Pengguna Raspberry Pi." },
  { "en": "Apa Perbedaan Utama Raspberry Pi 3 Dan 4?", "id": "Pi 4, Jauh Lebih Cepat Dan Kuat." },
  { "en": "Berapa Jumlah Port USB Di Raspberry Pi Zero?", "id": "Satu Port, Micro Universal Serial Bus (USB)." },
  { "en": "Apa Itu 'Graphical User Interface' (GUI)?", "id": "Antarmuka Visual, Dengan Ikon Dan Jendela." },
  { "en": "Bagaimana Cara Membuka 'File Manager'?", "id": "Klik Ikon Folder, Di Pojok Kiri Atas." },
  { "en": "Apa Itu 'Task Manager' Di Pi OS?", "id": "Utilitas Untuk Memantau, Proses Yang Berjalan." },
  { "en": "Sistem Operasi Apa Selain Raspberry Pi OS?", "id": "Ubuntu, Manjaro, Dan OS Populer Lainnya." },
  { "en": "Apa Itu 'LibreELEC' Atau 'OSMC'?", "id": "Sistem Operasi, Khusus Untuk Pusat Media." },
  { "en": "Bagaimana Cara Melihat Penggunaan Memori RAM?", "id": "Gunakan Perintah 'free', Di Dalam Terminal." },
  { "en": "Bagaimana Cara Melihat Penggunaan Ruang Disk?", "id": "Gunakan Perintah 'df -h', Di Terminal." },
  { "en": "Apa Itu 'SoC' (System on a Chip)?", "id": "Integrasi Semua Komponen, Dalam Satu Chip." },
  { "en": "Siapa Pabrikan Utama 'SoC' Untuk Pi?", "id": "Perusahaan Semikonduktor, Broadcom Inc." },
  { "en": "Apa Fungsi Dari Kristal Osiator Perak?", "id": "Menghasilkan Sinyal Jam, Untuk Sinkronisasi Sistem." },
  { "en": "Dapatkah Pi Beroperasi Tanpa Kartu Micro SD?", "id": "Bisa, Dengan Booting Dari Jaringan (PXE)." },
  { "en": "Apa Itu 'Network Booting' (PXE Boot)?", "id": "Memuat Sistem Operasi, Dari Server Jaringan." },
  { "en": "Apa Itu 'Wayland' Dan 'X11'?", "id": "Protokol Server Tampilan, Untuk GUI Linux." },
  { "en": "Apa Itu 'ALSA' (Advanced Linux Sound Architecture)?", "id": "Kerangka Kerja Perangkat Audio, Di Linux." },
  { "en": "Bagaimana Cara Mengatur Volume Suara Dari Terminal?", "id": "Menggunakan Utilitas, Yang Bernama 'alsamixer'." },
  { "en": "Apa Itu 'Universal Asynchronous Receiver-Transmitter' (UART)?", "id": "Komunikasi Serial, Antara Dua Perangkat." },
  { "en": "Pin GPIO Mana Yang Digunakan Untuk UART?", "id": "Pin Delapan (TXD), Dan Sepuluh (RXD)." },
  { "en": "Apa Kegunaan Komunikasi Serial UART?", "id": "Mengakses Terminal Headless, Dan Debugging." },
  { "en": "Apa Itu Modul 'Global Positioning System' (GPS)?", "id": "Menentukan Lokasi Geografis, Menggunakan Satelit." },
  { "en": "Bagaimana Modul 'GPS' Berkomunikasi Dengan Pi?", "id": "Biasanya Melalui Antarmuka, Serial (UART)." },
  { "en": "Apa Itu 'Light Dependent Resistor' (LDR)?", "id": "Resistor Yang Nilainya, Berubah Dengan Cahaya." },
  { "en": "Apa Itu Modul Relay Untuk Pi?", "id": "Saklar Elektronik, Untuk Kontrol Tegangan Tinggi." },
  { "en": "Mengapa Relay Penting Untuk Otomasi Rumah?", "id": "Mengisolasi Sirkuit Pi, Dari Tegangan Listrik." },
  { "en": "Apa Itu 'Solid State Relay' (SSR)?", "id": "Relay Elektronik, Tanpa Bagian Bergerak." },
  { "en": "Apa Keuntungan 'SSR' Dibanding Relay Mekanis?", "id": "Lebih Cepat, Tahan Lama, Dan Tidak Berisik." },
  { "en": "Apa Itu 'Digital-to-Analog Converter' (DAC)?", "id": "Mengubah Sinyal Digital, Menjadi Sinyal Analog." },
  { "en": "Kapan Sebuah 'DAC' Mungkin Diperlukan?", "id": "Untuk Mendapatkan Output, Audio Kualitas Tinggi." },
  { "en": "Apa Itu 'Stepper Motor'?", "id": "Motor Yang Bergerak, Dalam Langkah-Langkah Presisi." },
  { "en": "Bagaimana Pi Mengontrol Sebuah 'Stepper Motor'?", "id": "Membutuhkan Papan Driver, Khusus Motor." },
  { "en": "Apa Itu Pustaka 'RPi.GPIO' Python?", "id": "Pustaka Populer, Untuk Mengontrol Pin GPIO." },
  { "en": "Apa Dua Mode Penomoran Pin GPIO?", "id": "Mode 'BOARD', Dan Mode 'BCM'." },
  { "en": "Apa Arti Penomoran Pin Mode 'BOARD'?", "id": "Merujuk Pada Nomor Fisik, Pin Header." },
  { "en": "Apa Arti Penomoran Pin Mode 'BCM'?", "id": "Merujuk Nomor Saluran, Chip Broadcom SoC." },
  { "en": "Bagaimana Cara Membersihkan Konfigurasi Pin GPIO?", "id": "Gunakan Fungsi, 'GPIO.cleanup()' Dalam Script." },
  { "en": "Apa Itu 'ifconfig' Atau Perintah 'ip'?", "id": "Untuk Melihat, Dan Mengkonfigurasi Antarmuka Jaringan." },
  { "en": "Apa Itu 'Network Time Protocol' (NTP)?", "id": "Protokol Sinkronisasi, Waktu Jam Di Jaringan." },
  { "en": "Bagaimana Cara Mengubah Zona Waktu Raspberry Pi?", "id": "Menggunakan 'sudo raspi-config', Di Menu Lokalisasi." },
  { "en": "Apa Itu 'raspi-config' Tool?", "id": "Alat Konfigurasi Utama, Raspberry Pi OS." },
  { "en": "Bagaimana Cara Mengaktifkan Antarmuka Seperti 'I2C'?", "id": "Melalui Opsi Antarmuka, Dalam 'raspi-config'." },
  { "en": "Berapa Perkiraan Konsumsi Daya Pi 4 Idle?", "id": "Sekitar Tiga, Hingga Empat Watt." },
  { "en": "Berapa Konsumsi Daya Pi Saat Beban Penuh?", "id": "Bisa Mencapai, Enam Hingga Tujuh Watt." },
  { "en": "Bagaimana Cara Mengukur Konsumsi Daya Pi?", "id": "Menggunakan Pengukur Daya, Dinding USB." },
  { "en": "Apa Itu 'Watchdog Timer'?", "id": "Mekanisme Perangkat Keras, Untuk Merestart Sistem." },
  { "en": "Kapan 'Watchdog Timer' Sangat Berguna?", "id": "Saat Pi Dijalankan, Di Lokasi Terpencil." },
  { "en": "Apa Itu 'Systemd' Di Linux?", "id": "Manajer Sistem, Dan Layanan Untuk Linux." },
  { "en": "Bagaimana Cara Membuat Layanan 'Systemd'?", "id": "Membuat File Unit, Di Direktori Tertentu." },
  { "en": "Apa Itu 'Journalctl' Command?", "id": "Perintah Untuk Melihat Log, Dari 'Systemd'." },
  { "en": "Apa Itu 'htop' Command?", "id": "Monitor Proses Interaktif, Versi Lebih Baik." },
  { "en": "Apa Beda 'top' Dan 'htop'?", "id": "htop, Lebih Ramah Pengguna Dan Interaktif." },
  { "en": "Bagaimana Cara Menginstal Perangkat Lunak Dari GitHub?", "id": "Menggunakan 'git clone', Lalu Mengikuti Instruksi." },
  { "en": "Apa Itu 'Makefile' Dalam Kompilasi?", "id": "File Berisi Aturan, Untuk Membangun Perangkat Lunak." },
  { "en": "Apa Itu 'GCC' (GNU Compiler Collection)?", "id": "Kompilator Standar, Untuk Bahasa C/C++." },
  { "en": "Apa Itu 'Cross-Compilation'?", "id": "Mengkompilasi Kode, Untuk Platform Yang Berbeda." },
  { "en": "Apa Itu 'Buildroot' Atau 'Yocto'?", "id": "Alat Untuk Membangun, Sistem Linux Kustom." },
  { "en": "Apa Masalah Umum Saat Pi Tidak Mau Boot?", "id": "Masalah Catu Daya, Atau Kartu SD Korup." },
  { "en": "Apa Arti Pola Kedipan LED Hijau?", "id": "Menunjukkan Kode Kesalahan, Selama Proses Booting." },
  { "en": "Di Mana Menemukan Arti Kode Kedipan LED?", "id": "Pada Dokumentasi Resmi, Raspberry Pi Foundation." },
  { "en": "Bagaimana Cara Memperbaiki Kartu SD Yang Korup?", "id": "Biasanya Perlu, Melakukan Format Ulang." },
  { "en": "Apa Itu 'fsck' (File System Consistency Check)?", "id": "Utilitas Linux, Memeriksa Dan Memperbaiki Filesystem." },
  { "en": "Apa Itu 'Bad Sector' Pada Kartu SD?", "id": "Bagian Rusak, Yang Tidak Dapat Dibaca." },
  { "en": "Bagaimana Cara Menghindari Korupsi Kartu SD?", "id": "Selalu Matikan Pi, Dengan Benar." },
  { "en": "Apa Itu 'Uninterruptible Power Supply' (UPS)?", "id": "Catu Daya Baterai, Cadangan Untuk Pi." },
  { "en": "Apa Manfaat Menggunakan 'UPS' HAT?", "id": "Mencegah Kehilangan Data, Saat Listrik Padam." },
  { "en": "Apa Itu 'Case' Atau Casing Fan?", "id": "Kipas Pendingin, Yang Terpasang Di Casing." },
  { "en": "Apakah Pi Zero Memiliki Pin GPIO?", "id": "Ya, Tapi Pin Header Perlu Disolder." },
  { "en": "Apa Beda Pi Zero Dengan Pi Zero W?", "id": "Pi Zero W, Memiliki Wi-Fi Dan Bluetooth." },
  { "en": "Apa Itu 'Python Virtual Environment'?", "id": "Lingkungan Terisolasi, Untuk Proyek Python." },
  { "en": "Mengapa Menggunakan 'Virtual Environment' Adalah Praktik Baik?", "id": "Menghindari Konflik, Antara Ketergantungan Paket." },
  { "en": "Apa Itu 'pip' (Pip Installs Packages)?", "id": "Manajer Paket Standar, Untuk Bahasa Python." },
  { "en": "Bagaimana Cara Instal Library Python Dengan 'pip'?", "id": "Gunakan Perintah, 'pip install nama-library'." },
  { "en": "Apa Itu 'Pygame' Library?", "id": "Library Python, Untuk Membuat Game Sederhana." },
  { "en": "Apa Itu 'Tkinter' Library?", "id": "Pustaka Standar Python, Untuk Membuat GUI." },
  { "en": "Apa Itu 'Flask' Atau 'Django'?", "id": "Kerangka Kerja Web, Berbasis Bahasa Python." },
  { "en": "Bisakah Pi Menjadi Server Untuk Aplikasi Web?", "id": "Ya, Sangat Bisa Menggunakan Flask Atau Django." },
  { "en": "Apa Itu 'Nginx' Atau 'Apache'?", "id": "Perangkat Lunak Server, Web Yang Populer." },
  { "en": "Apa Peran Server Web Dalam Aplikasi?", "id": "Menangani Permintaan HTTP, Dari Pengguna Browser." },
  { "en": "Apa Itu 'SQLite' Database?", "id": "Mesin Database Ringan, Berbasis File." },
  { "en": "Kapan 'SQLite' Cocok Digunakan Di Pi?", "id": "Untuk Proyek Kecil, Yang Butuh Database." },
  { "en": "Apa Itu 'MySQL' Atau 'PostgreSQL'?", "id": "Sistem Manajemen Database, Yang Lebih Kuat." },
  { "en": "Apa Itu 'SSH Key-Based Authentication'?", "id": "Metode Login Aman, Tanpa Menggunakan Password." },
  { "en": "Mengapa Login Kunci SSH Lebih Aman?", "id": "Karena Jauh Lebih Sulit, Untuk Diretas." },
  { "en": "Apa Itu 'Fail2ban' Software?", "id": "Mencegah Serangan 'Brute-Force', Dengan Blokir IP." },
  { "en": "Apa Itu 'UFW' (Uncomplicated Firewall)?", "id": "Antarmuka Sederhana, Untuk Mengelola 'Firewall'." },
  { "en": "Apa Itu 'Port Forwarding' Di Router?", "id": "Mengarahkan Lalu Lintas, Ke Perangkat Tertentu." },
  { "en": "Kapan 'Port Forwarding' Dibutuhkan Untuk Pi?", "id": "Mengakses Server Pi, Dari Luar Jaringan." },
  { "en": "Apa Itu 'Dynamic DNS' (DDNS)?", "id": "Memetakan Nama Domain, Ke Alamat IP Dinamis." },
  { "en": "Apa Itu 'Ethtool' Command?", "id": "Utilitas Mengatur Driver, Kartu Jaringan Ethernet." },
  { "en": "Apa Itu 'Iperf' Tool?", "id": "Alat Mengukur Kinerja, Dan Bandwidth Jaringan." },
  { "en": "Apa Itu 'Nmap' (Network Mapper)?", "id": "Alat Eksplorasi Jaringan, Dan Audit Keamanan." },
  { "en": "Apa Itu 'Git LFS' (Large File Storage)?", "id": "Ekstensi Git, Untuk Mengelola File Besar." },
  { "en": "Apa Itu Lisensi Perangkat Lunak Open Source?", "id": "Lisensi Yang Memberikan, Kebebasan Pengguna." },
  { "en": "Apa Itu 'Creative Commons' License?", "id": "Lisensi Hak Cipta, Untuk Karya Kreatif." },
  { "en": "Apa Itu 'Fritzing' Software?", "id": "Alat Merancang, Dan Mendokumentasikan Sirkuit Elektronik." },
  { "en": "Apa Manfaat 'Fritzing' Untuk Proyek Pi?", "id": "Membuat Diagram Rangkaian, Yang Mudah Dibaca." },
  { "en": "Apa Itu 'KiCad' Software?", "id": "Perangkat Lunak Desain, Otomasi Elektronik Gratis." },
  { "en": "Apa Itu Raspberry Pi 'Sense HAT'?", "id": "Papan Tambahan, Dengan Berbagai Macam Sensor." },
  { "en": "Sensor Apa Saja Yang Ada Di 'Sense HAT'?", "id": "Giroskop, Akselerometer, Magnetometer, Dan Lainnya." },
  { "en": "Apa Fungsi Matriks LED 8x8 Di 'Sense HAT'?", "id": "Menampilkan Informasi, Dan Gambar Berwarna." },
  { "en": "Bagaimana Cara Membaca Suhu Dari 'Sense HAT'?", "id": "Menggunakan Library Python, Khusus Sense HAT." },
  { "en": "Apa Itu 'Power over Ethernet' (PoE) HAT?", "id": "Memberi Daya Pi, Melalui Kabel Ethernet." },
  { "en": "Apa Standar 'PoE' Yang Digunakan Pi HAT?", "id": "Standar IEEE 802.3af, Yang Umum Digunakan." },
  { "en": "Apa Itu Perintah 'vcgencmd'?", "id": "Utilitas Baris Perintah, Untuk Informasi SoC." },
  { "en": "Bagaimana Cara Memeriksa Suhu 'CPU' Dengan 'vcgencmd'?", "id": "Gunakan Perintah, 'vcgencmd measure_temp'." },
  { "en": "Apa Fungsi Perintah 'dmesg'?", "id": "Menampilkan Pesan Kernel, Saat Proses Booting." },
  { "en": "Kapan 'dmesg' Sangat Berguna Untuk Diagnosa?", "id": "Saat Perangkat Keras, Gagal Terdeteksi." },
  { "en": "Apa Itu Perintah 'chmod'?", "id": "Mengubah Izin Akses, File Dan Direktori." },
  { "en": "Apa Arti Izin Angka '755'?", "id": "Pemilik Punya Semua, Orang Lain Baca Eksekusi." },
  { "en": "Apa Itu Perintah 'chown'?", "id": "Mengubah Kepemilikan Pengguna, File Dan Direktori." },
  { "en": "Apa Itu 'symbolic link' Di Linux?", "id": "Pintasan Atau Pointer, Ke File Lain." },
  { "en": "Bagaimana Cara Membuat 'symbolic link'?", "id": "Menggunakan Perintah, 'ln -s target link_name'." },
  { "en": "Apa Perbedaan Antara 'Hard Link' Dan 'Soft Link'?", "id": "Hard Link Langsung, Soft Link Pintasan." },
  { "en": "Apa Itu File 'cmdline.txt'?", "id": "Berisi Parameter Kernel, Saat Proses Booting." },
  { "en": "Di Mana Lokasi File 'config.txt' Dan 'cmdline.txt'?", "id": "Di Dalam Partisi, Boot Kartu SD." },
  { "en": "Apa Itu Modul Kamera Raspberry Pi v1?", "id": "Modul Kamera Asli, Beresolusi Lima Megapiksel." },
  { "en": "Apa Peningkatan Pada Modul Kamera v2?", "id": "Sensor Sony Delapan Megapiksel, Kualitas Lebih Baik." },
  { "en": "Apa Itu Modul Kamera 'High Quality'?", "id": "Kamera Dengan Sensor Besar, Dan Lensa Terpisah." },
  { "en": "Apa Itu 'raspistill' Command?", "id": "Utilitas Baris Perintah, Untuk Mengambil Gambar." },
  { "en": "Apa Itu 'raspivid' Command?", "id": "Utilitas Baris Perintah, Untuk Merekam Video." },
  { "en": "Bisakah Menjalankan Bahasa Java Di Raspberry Pi?", "id": "Ya, Mendukung Penuh Java Development Kit." },
  { "en": "Apa Itu 'screen' Atau 'tmux'?", "id": "Multiplexer Terminal, Menjalankan Sesi Latar Belakang." },
  { "en": "Apa Manfaat Menggunakan 'tmux' Atau 'screen'?", "id": "Sesi Tetap Jalan, Meski Koneksi SSH Putus." },
  { "en": "Apa Itu 'rsync' Command?", "id": "Utilitas Sinkronisasi File, Jarak Jauh Cepat." },
  { "en": "Apa Keunggulan 'rsync' Dibanding 'cp'?", "id": "Hanya Menyalin Bagian, File Yang Berubah." },
  { "en": "Apa Itu 'cron' Keyword '@reboot'?", "id": "Menjalankan Perintah, Tepat Setelah Sistem Boot." },
  { "en": "Apa Itu 'anacron'?", "id": "Seperti 'cron', Tapi Untuk Komputer Mati." },
  { "en": "Apa Perbedaan Antara 'Bash' Dan 'Zsh'?", "id": "Shell Baris Perintah, Dengan Fitur Berbeda." },
  { "en": "Apa Itu 'Bash Scripting'?", "id": "Menulis Skrip Otomatisasi, Menggunakan Perintah Shell." },
  { "en": "Apa Baris Pertama Dalam Sebuah 'Bash Script'?", "id": "Shebang, Yang Menentukan Interpreter '/bin/bash'." },
  { "en": "Apa Itu 'Environment Variable'?", "id": "Variabel Dinamis, Yang Mempengaruhi Proses." },
  { "en": "Bagaimana Cara Melihat Semua 'Environment Variable'?", "id": "Gunakan Perintah, 'printenv' Atau 'env'." },
  { "en": "Apa Fungsi Variabel '$PATH'?", "id": "Menentukan Direktori, Tempat Perintah Dicari." },
  { "en": "Apa Itu File '.bashrc'?", "id": "Skrip Konfigurasi Shell, Dijalankan Saat Login." },
  { "en": "Apa Itu 'Piping' Dalam Command Line?", "id": "Mengirim Output Satu Perintah, Ke Perintah Lain." },
  { "en": "Apa Contoh Perintah 'Piping'?", "id": "Perintah 'ls -l | grep .txt'." },
  { "en": "Apa Itu 'grep' Command?", "id": "Mencari Pola Teks, Di Dalam Input." },
  { "en": "Apa Itu 'awk' Dan 'sed'?", "id": "Utilitas Pengolah Teks, Yang Sangat Kuat." },
  { "en": "Apa Itu 'tar' Command?", "id": "Menggabungkan Beberapa File, Menjadi Satu Arsip." },
  { "en": "Bagaimana Cara Mengekstrak File '.tar.gz'?", "id": "Gunakan Perintah, 'tar -zxvf namafile.tar.gz'." },
  { "en": "Apa Itu 'wget' Atau 'curl'?", "id": "Alat Mengunduh File, Dari Internet." },
  { "en": "Apa Perbedaan 'apt update' Dan 'apt upgrade'?", "id": "Update Daftar Paket, Upgrade Menginstal Pembaruan." },
  { "en": "Apa Itu 'apt-get autoremove'?", "id": "Menghapus Paket Sisa, Yang Tak Diperlukan." },
  { "en": "Apa Itu 'PPA' (Personal Package Archive)?", "id": "Repositori Perangkat Lunak, Untuk Sistem Ubuntu." },
  { "en": "Apa Itu 'Flatpak' Atau 'Snap'?", "id": "Sistem Manajemen Paket, Universal Untuk Linux." },
  { "en": "Apa Itu 'LVM' (Logical Volume Management)?", "id": "Manajemen Disk Fleksibel, Untuk Sistem Linux." },
  { "en": "Apa Itu Partisi 'Swap'?", "id": "Ruang Disk Digunakan, Sebagai Memori Virtual." },
  { "en": "Kapan Partisi 'Swap' Dibutuhkan?", "id": "Saat Memori Fisik (RAM), Hampir Penuh." },
  { "en": "Bagaimana Cara Membuat 'Swap File'?", "id": "Menggunakan Perintah, Seperti 'fallocate' Dan 'mkswap'." },
  { "en": "Apa Itu 'Wireshark'?", "id": "Penganalisis Protokol Jaringan, Grafis Yang Kuat." },
  { "en": "Apa Itu 'tcpdump'?", "id": "Penganalisis Paket Jaringan, Berbasis Baris Perintah." },
  { "en": "Apa Itu 'iwconfig' Command?", "id": "Mengkonfigurasi Antarmuka, Jaringan Nirkabel (Wi-Fi)." },
  { "en": "Apa Itu 'Wake-on-LAN' (WoL)?", "id": "Membangunkan Komputer, Dari Jarak Jauh." },
  { "en": "Apakah Raspberry Pi Mendukung 'Wake-on-LAN'?", "id": "Tidak Secara Bawaan, Pada Port Ethernetnya." },
  { "en": "Apa Itu '1-Wire' Interface?", "id": "Bus Komunikasi Data, Dengan Satu Kabel." },
  { "en": "Sensor Apa Yang Menggunakan Antarmuka '1-Wire'?", "id": "Sensor Suhu Digital, Populer DS18B20." },
  { "en": "Bagaimana Mengaktifkan Antarmuka '1-Wire'?", "id": "Melalui File Konfigurasi, Atau 'raspi-config'." },
  { "en": "Apa Itu 'Device Tree' Di Linux?", "id": "Struktur Data Deskripsi, Perangkat Keras." },
  { "en": "Apa Fungsi 'Device Tree Overlay'?", "id": "Memodifikasi 'Device Tree', Saat Proses Boot." },
  { "en": "Apa Itu 'Network File System' (NFS)?", "id": "Protokol Berbagi Direktori, Di Jaringan." },
  { "en": "Apa Beda 'NFS' Dengan 'Samba'?", "id": "NFS Untuk Linux/Unix, Samba Untuk Windows." },
  { "en": "Apa Itu 'Node-RED'?", "id": "Alat Pemrograman Visual, Untuk Proyek IoT." },
  { "en": "Bagaimana Cara Kerja 'Node-RED'?", "id": "Menghubungkan 'Node', Untuk Membuat Alur Logika." },
  { "en": "Apa Itu 'InfluxDB'?", "id": "Database Deret Waktu, Untuk Data Sensor." },
  { "en": "Apa Itu 'Grafana'?", "id": "Platform Visualisasi Data, Dan Dasbor Analitik." },
  { "en": "Bagaimana 'InfluxDB' Dan 'Grafana' Bekerja Sama?", "id": "Grafana Memvisualisasikan, Data Dari InfluxDB." },
  { "en": "Apa Itu 'Balena' Platform?", "id": "Platform Lengkap, Untuk Membangun Aplikasi IoT." },
  { "en": "Apa Itu 'Mender'?", "id": "Manajer Pembaruan 'Over-the-Air' (OTA), Terbuka." },
  { "en": "Mengapa Pembaruan 'OTA' Penting Untuk IoT?", "id": "Memperbarui Perangkat, Dari Jarak Jauh Aman." },
  { "en": "Apa Itu 'Ansible', 'Puppet', atau 'Chef'?", "id": "Alat Manajemen Konfigurasi, Dan Otomatisasi." },
  { "en": "Apa Itu 'Vagrant' Atau 'Multipass'?", "id": "Alat Membuat, Dan Mengelola Mesin Virtual." },
  { "en": "Apa Itu 'Lighttpd' Web Server?", "id": "Server Web Ringan, Alternatif Apache/Nginx." },
  { "en": "Apa Itu 'PHP'?", "id": "Bahasa Skrip Sisi Server, Populer." },
  { "en": "Apa Itu 'LAMP' Stack?", "id": "Linux, Apache, MySQL, PHP/Python/Perl." },
  { "en": "Apa Itu 'LEMP' Stack?", "id": "Linux, Nginx (Engine-X), MySQL, PHP/Python/Perl." },
  { "en": "Apa Itu 'Let's Encrypt'?", "id": "Otoritas Sertifikat Gratis, Otomatis, Dan Terbuka." },
  { "en": "Apa Fungsi Sertifikat 'SSL/TLS'?", "id": "Mengenkripsi Komunikasi, Antara Klien Dan Server." },
  { "en": "Apa Itu 'Certbot'?", "id": "Alat Otomatisasi, Untuk Sertifikat Let's Encrypt." },
  { "en": "Apa Itu 'Jupyter Notebook'?", "id": "Aplikasi Web Interaktif, Untuk Kode Sains." },
  { "en": "Bisakah Menjalankan 'Jupyter Notebook' Di Pi?", "id": "Ya, Sangat Bisa Untuk Analisis Data." },
  { "en": "Apa Itu 'Pigpio' Library?", "id": "Library Python Lanjutan, Untuk Kontrol GPIO." },
  { "en": "Apa Keunggulan 'Pigpio' Dari 'RPi.GPIO'?", "id": "PWM Lebih Baik, Sampling Cepat, Lainnya." },
  { "en": "Apa Itu 'Bluetooth Low Energy' (BLE)?", "id": "Versi Bluetooth, Dengan Konsumsi Daya Rendah." },
  { "en": "Apa Aplikasi 'BLE' Pada Proyek IoT?", "id": "Komunikasi Jarak Dekat, Perangkat Bertenaga Baterai." },
  { "en": "Apa Itu 'BlueZ' Stack?", "id": "Tumpukan Protokol Bluetooth, Resmi Untuk Linux." },
  { "en": "Apa Itu 'GATT' (Generic Attribute Profile)?", "id": "Mendefinisikan Cara, Perangkat BLE Bertukar Data." },
  { "en": "Apa Itu 'Git Hooks'?", "id": "Skrip Yang Dijalankan Otomatis, Saat Aksi." },
  { "en": "Apa Itu 'Continuous Integration' (CI)?", "id": "Praktik Mengintegrasikan, Perubahan Kode Secara Teratur." },
  { "en": "Apa Itu 'Continuous Deployment' (CD)?", "id": "Menerapkan Perubahan Kode, Ke Produksi Otomatis." },
  { "en": "Dapatkah Pi Dijadikan Server CI/CD Sederhana?", "id": "Ya, Menggunakan Alat Seperti Jenkins Atau GitLab." },
  { "en": "Apa Itu 'FFmpeg'?", "id": "Kerangka Kerja Multimedia, Lengkap Dan Kuat." },
  { "en": "Apa Kegunaan 'FFmpeg' Di Raspberry Pi?", "id": "Mengkonversi, Merekam, Dan Streaming Audio Video." },
  { "en": "Apa Itu 'GStreamer'?", "id": "Kerangka Kerja Multimedia, Berbasis Plugin." },
  { "en": "Apa Itu 'MicroPython'?", "id": "Implementasi Python 3, Untuk Mikrokontroler." },
  { "en": "Apa Beda Utama 'MicroPython' Dan 'CircuitPython'?", "id": "CircuitPython, Dikembangkan Oleh Perusahaan Adafruit." },
  { "en": "Bahasa Apa Yang Digunakan Untuk Memprogram Pi Pico?", "id": "C/C++, Dan Juga Bahasa MicroPython." },
  { "en": "Apa Itu 'Thonny IDE'?", "id": "IDE Python Ramah Pemula, Bawaan Pi OS." },
  { "en": "Bagaimana Cara Menjalankan Kode Di Pi Pico?", "id": "Menyimpan File, Dengan Nama 'main.py'." },
  { "en": "Apa Itu 'REPL' Di MicroPython?", "id": "Read-Evaluate-Print Loop, Antarmuka Interaktif." },
  { "en": "Apa Chip Mikrokontroler Di Raspberry Pi Pico?", "id": "Chip RP2040, Didesain Oleh Raspberry Pi." },
  { "en": "Apa Fitur Unik Dari Chip RP2040?", "id": "Programmable Input/Output (PIO), State Machines." },
  { "en": "Apa Fungsi 'PIO' (Programmable Input/Output)?", "id": "Membuat Antarmuka Perangkat Keras, Sangat Kustom." },
  { "en": "Apakah Pi Pico Memiliki Konektivitas Nirkabel?", "id": "Model Standar Tidak, Model W Memiliki." },
  { "en": "Apa Itu Layar 'OLED' (Organic Light-Emitting Diode)?", "id": "Layar Tipis, Dengan Kontras Sangat Tinggi." },
  { "en": "Bagaimana Menghubungkan Layar 'OLED' Ke Pi?", "id": "Biasanya Menggunakan, Antarmuka I2C Atau SPI." },
  { "en": "Apa Itu Layar 'E-Ink' Atau 'E-Paper'?", "id": "Layar Bi-stabil, Konsumsi Daya Sangat Rendah." },
  { "en": "Apa Kelemahan Utama Layar 'E-Ink'?", "id": "Tingkat Penyegaran (Refresh Rate), Sangat Lambat." },
  { "en": "Apa Itu 'CUPS' (Common Unix Printing System)?", "id": "Sistem Pencetakan Standar, Untuk Sistem Mirip Unix." },
  { "en": "Bisakah Pi Berfungsi Sebagai 'Print Server'?", "id": "Ya, Dengan Menginstal Dan Konfigurasi CUPS." },
  { "en": "Apa Itu 'SANE' (Scanner Access Now Easy)?", "id": "Antarmuka Standar, Untuk Mengakses Perangkat Pemindai." },
  { "en": "Apa Itu 'PulseAudio'?", "id": "Server Suara Jaringan, Untuk Sistem Linux." },
  { "en": "Apa Perbedaan 'ALSA' Dan 'PulseAudio'?", "id": "ALSA Level Rendah, PulseAudio Level Tinggi." },
  { "en": "Apa Itu 'JACK' Audio Connection Kit?", "id": "Server Suara Profesional, Dengan Latensi Rendah." },
  { "en": "Apa Itu 'iptables'?", "id": "Firewall Baris Perintah, Bawaan Kernel Linux." },
  { "en": "Apa Itu 'SELinux' Atau 'AppArmor'?", "id": "Modul Keamanan Wajib, Untuk Sistem Linux." },
  { "en": "Apa Fungsi Dari 'AppArmor'?", "id": "Membatasi Kemampuan Program, Sesuai Profil Keamanan." },
  { "en": "Apa Itu 'chroot jail'?", "id": "Mengisolasi Proses, Ke Direktori Root Tertentu." },
  { "en": "Apa Itu 'QEMU' (Quick Emulator)?", "id": "Emulator Dan Virtualizer, Perangkat Keras Terbuka." },
  { "en": "Bisakah Pi Mengemulasikan Sistem Operasi Lain?", "id": "Ya, Menggunakan QEMU, Tapi Performanya Lambat." },
  { "en": "Apa Itu 'KVM' (Kernel-based Virtual Machine)?", "id": "Solusi Virtualisasi Penuh, Untuk Kernel Linux." },
  { "en": "Apakah Pi Mendukung Virtualisasi 'KVM'?", "id": "Ya, Pada Prosesor 64-bit Yang Mendukung." },
  { "en": "Apa Itu 'OpenGL ES'?", "id": "Spesifikasi API Grafis, Untuk Sistem Tertanam." },
  { "en": "Apa Itu 'Mesa' Graphics Library?", "id": "Implementasi Open Source, Dari API Grafis." },
  { "en": "Apa Itu 'VideoCore' GPU?", "id": "Unit Pemrosesan Grafis, Di Dalam SoC." },
  { "en": "Apa Itu 'rpi-update' Tool?", "id": "Alat Memperbarui Firmware, Dan Kernel Pi." },
  { "en": "Kapan Sebaiknya Menggunakan 'rpi-update'?", "id": "Hanya Jika Diperlukan, Untuk Menguji Fitur." },
  { "en": "Apa Tahapan Proses Boot Raspberry Pi?", "id": "Boot ROM, Bootloader, Kernel, Init System." },
  { "en": "Apa Itu 'init' Proses?", "id": "Proses Pertama Yang Dijalankan, Oleh Kernel." },
  { "en": "Apa Itu 'Runlevel' Atau 'Target' Systemd?", "id": "Mode Operasi Sistem, Yang Telah Ditentukan." },
  { "en": "Apa Itu 'Multi-User Target'?", "id": "Mode Standar, Untuk Antarmuka Baris Perintah." },
  { "en": "Apa Itu 'Graphical Target'?", "id": "Mode Standar, Untuk Antarmuka Pengguna Grafis." },
  { "en": "Apa Itu Kapasitor Dalam Rangkaian Elektronik?", "id": "Komponen Pasif, Yang Menyimpan Energi Listrik." },
  { "en": "Apa Satuan Dari Kapasitansi Sebuah Kapasitor?", "id": "Satuan Farad (F), Microfarad (uF)." },
  { "en": "Apa Itu Dioda?", "id": "Komponen Yang Mengizinkan Arus, Satu Arah." },
  { "en": "Apa Itu 'Flyback Diode'?", "id": "Melindungi Sirkuit, Dari Lonjakan Tegangan Induktif." },
  { "en": "Apa Itu Transistor?", "id": "Komponen Semikonduktor, Sebagai Saklar Atau Penguat." },
  { "en": "Apa Dua Jenis Transistor Bipolar Utama?", "id": "NPN (Negative-Positive-Negative), Dan PNP (Positive-Negative-Positive)." },
  { "en": "Apa Itu 'MOSFET'?", "id": "Metal-Oxide-Semiconductor, Field-Effect Transistor." },
  { "en": "Apa Itu 'Logic Gate'?", "id": "Blok Bangunan Dasar, Dari Sirkuit Digital." },
  { "en": "Apa Contoh 'Logic Gate' Dasar?", "id": "AND, OR, NOT, NAND, NOR, XOR." },
  { "en": "Apa Itu 'Shift Register'?", "id": "Memperluas Jumlah Output, Dari Mikrokontroler." },
  { "en": "Apa Itu 'Multiplexer' (MUX)?", "id": "Memilih Satu Sinyal, Dari Beberapa Input." },
  { "en": "Apa Itu 'Demultiplexer' (DEMUX)?", "id": "Mengirim Satu Sinyal, Ke Beberapa Output." },
  { "en": "Apa Itu 'Real-Time Operating System' (RTOS)?", "id": "Sistem Operasi, Dengan Batas Waktu Ketat." },
  { "en": "Bisakah Linux Di Pi Menjadi Sistem 'Real-Time'?", "id": "Bisa, Dengan Patch Kernel PREEMPT_RT." },
  { "en": "Apa Itu 'Latency' Dalam Komputasi?", "id": "Waktu Tunda, Antara Aksi Dan Respons." },
  { "en": "Apa Itu 'Jitter'?", "id": "Variasi Waktu Tunda, Atau Latensi." },
  { "en": "Apa Itu 'Network Switch'?", "id": "Menghubungkan Perangkat, Dalam Satu Jaringan Lokal." },
  { "en": "Apa Itu 'Router'?", "id": "Menghubungkan Jaringan Berbeda, Dan Mengarahkan Lalu Lintas." },
  { "en": "Apa Itu 'Subnet Mask'?", "id": "Membagi Alamat IP, Menjadi Jaringan Host." },
  { "en": "Apa Itu 'Default Gateway'?", "id": "Gerbang Keluar Jaringan, Menuju Internet." },
  { "en": "Apa Itu 'Domain Name System' (DNS)?", "id": "Menerjemahkan Nama Domain, Menjadi Alamat IP." },
  { "en": "Bagaimana Cara Mengatur Server 'DNS' Di Pi?", "id": "Mengedit File, '/etc/resolv.conf' Secara Manual." },
  { "en": "Apa Itu 'mDNS' (Multicast DNS)?", "id": "Menemukan Perangkat, Tanpa Server DNS Terpusat." },
  { "en": "Apa Itu 'Avahi' Daemon?", "id": "Implementasi 'Zeroconf', Termasuk 'mDNS' Di Linux." },
  { "en": "Apa Fungsi Akhiran '.local' Pada Hostname?", "id": "Hostname Yang Dapat Diakses, Melalui 'mDNS'." },
  { "en": "Apa Itu 'iostat', 'vmstat', 'mpstat'?", "id": "Utilitas Memantau Kinerja, Sistem Secara Mendalam." },
  { "en": "Apa Itu 'lsof' Command?", "id": "List Open Files, Melihat File Terbuka." },
  { "en": "Apa Itu 'strace' Command?", "id": "Melacak Panggilan Sistem, Dan Sinyal Program." },
  { "en": "Apa Itu 'gdb' (GNU Debugger)?", "id": "Debugger Kuat, Untuk Program C/C++." },
  { "en": "Apa Itu 'Valgrind'?", "id": "Alat Analisis Memori, Dan Profiling Program." },
  { "en": "Apa Itu 'Core Dump'?", "id": "File Rekaman Memori, Saat Program Crash." },
  { "en": "Apa Itu 'Arch Linux'?", "id": "Distribusi Linux Ringan, Dan Sangat Fleksibel." },
  { "en": "Apa Filosofi Dari 'Arch Linux'?", "id": "Kesederhanaan, Modernitas, Pragmatisme, Dan Sentralitas Pengguna." },
  { "en": "Apa Itu 'Pacman' Paket Manajer?", "id": "Manajer Paket Bawaan, Dari Distribusi Arch." },
  { "en": "Apa Itu 'DietPi'?", "id": "Distribusi Linux Ringan, Berbasis Debian." },
  { "en": "Apa Keunggulan Utama Dari 'DietPi'?", "id": "Sangat Dioptimalkan, Untuk Kinerja Perangkat Keras." },
  { "en": "Apa Itu 'GPIO Expander'?", "id": "Chip Untuk Menambah, Jumlah Pin GPIO." },
  { "en": "Apa Contoh Chip 'GPIO Expander' Populer?", "id": "Chip MCP23017, Yang Menggunakan Antarmuka I2C." },
  { "en": "Apa Itu 'EEPROM'?", "id": "Electrically Erasable, Programmable Read-Only Memory." },
  { "en": "Apa Fungsi 'EEPROM' Pada HAT?", "id": "Menyimpan Informasi Konfigurasi, Papan Secara Otomatis." },
  { "en": "Apa Itu 'Voltage Divider'?", "id": "Sirkuit Sederhana, Menurunkan Tegangan Secara Proporsional." },
  { "en": "Bagaimana Cara Membuat 'Voltage Divider'?", "id": "Menggunakan Dua Buah, Resistor Secara Seri." },
  { "en": "Apa Itu 'Potentiometer'?", "id": "Resistor Variabel, Dengan Tiga Buah Terminal." },
  { "en": "Apa Kegunaan 'Potentiometer' Dalam Proyek?", "id": "Sebagai Tombol Kontrol, Input Analog." },
  { "en": "Apa Itu 'Thermal Throttling'?", "id": "Perlambatan Otomatis Prosesor, Karena Terlalu Panas." },
  { "en": "Bagaimana Cara Mengetahui Jika Pi Mengalami 'Throttling'?", "id": "Menggunakan Perintah, 'vcgencmd get_throttled'." },
  { "en": "Apa Itu 'Bare Metal' Programming?", "id": "Pemrograman Tanpa Bantuan, Sistem Operasi Apapun." },
  { "en": "Apa Itu 'Circle' C++ Library?", "id": "Library Pemrograman, 'Bare Metal' Untuk Pi." },
  { "en": "Apa Itu 'Neopixel' Atau 'WS2812B'?", "id": "LED RGB, Yang Dapat Diatasi Secara Individual." },
  { "en": "Bagaimana Cara Mengontrol Rantai LED 'Neopixel'?", "id": "Menggunakan Sinyal Waktu, Yang Sangat Presisi." },
  { "en": "Apa Itu 'Time-of-Flight' (ToF) Sensor?", "id": "Sensor Mengukur Jarak, Berdasarkan Kecepatan Cahaya." },
  { "en": "Apa Keunggulan Sensor 'ToF' Dari Ultrasonik?", "id": "Lebih Akurat, Dan Tidak Terpengaruh Permukaan." },
  { "en": "Apa Itu 'NFC' (Near Field Communication)?", "id": "Komunikasi Nirkabel, Jarak Sangat Dekat." },
  { "en": "Apa Itu 'RFID' (Radio-Frequency Identification)?", "id": "Identifikasi Menggunakan, Gelombang Radio Frekuensi." },
  { "en": "Apa Beda 'NFC' Dan 'RFID'?", "id": "NFC Adalah Bagian, Dari Standar RFID." },
  { "en": "Apa Itu 'Man-in-the-Middle' (MITM) Attack?", "id": "Serangan Menguping, Di Tengah Komunikasi." },
  { "en": "Apa Itu 'Denial-of-Service' (DoS) Attack?", "id": "Serangan Membuat Layanan, Tidak Tersedia." },
  { "en": "Apa Itu 'SQL Injection'?", "id": "Serangan Memasukkan Kueri, SQL Berbahaya." },
  { "en": "Apa Itu 'Cross-Site Scripting' (XSS)?", "id": "Menyuntikkan Skrip Berbahaya, Ke Situs Web." },
  { "en": "Apa Itu Antarmuka 'I2S' (Inter-IC Sound)?", "id": "Standar Antarmuka Serial, Untuk Audio Digital." },
  { "en": "Apa Kegunaan 'I2S' Pada Proyek Raspberry Pi?", "id": "Menghubungkan Mikrofon Digital, Dan DAC Audio." },
  { "en": "Apa Itu Mikrofon 'MEMS'?", "id": "Micro-Electro-Mechanical Systems, Mikrofon Ukuran Mungil." },
  { "en": "Bisakah Raspberry Pi Boot Dari SSD USB?", "id": "Ya, Model Terbaru Mendukung USB Mass Storage Boot." },
  { "en": "Apa Keuntungan Boot Dari Solid State Drive (SSD)?", "id": "Kecepatan Jauh Lebih Tinggi, Dari Kartu SD." },
  { "en": "Apa Itu 'NVMe' (Non-Volatile Memory Express)?", "id": "Antarmuka Penyimpanan Super Cepat, Untuk SSD." },
  { "en": "Bagaimana Cara Menghubungkan SSD 'NVMe' Ke Pi?", "id": "Melalui Papan HAT, Adapter USB, Atau CM4." },
  { "en": "Apa Itu 'libcamera'?", "id": "Tumpukan Kamera Sumber Terbuka, Baru Untuk Linux." },
  { "en": "Apa Itu 'Picamera2' Library?", "id": "Library Python Resmi, Yang Berbasis 'libcamera'." },
  { "en": "Apa Itu 'Poly-fuse' Di Papan Raspberry Pi?", "id": "Sekring Yang Dapat Mengatur Ulang, Otomatis." },
  { "en": "Apa Fungsi Dari 'Poly-fuse'?", "id": "Melindungi Pi, Dari Arus Berlebih (Overcurrent)." },
  { "en": "Apa Itu 'TVS Diode'?", "id": "Transient Voltage Suppression, Dioda Pelindung Lonjakan." },
  { "en": "Mengapa Pengaturan Kode Negara Wi-Fi Penting?", "id": "Agar Sesuai Regulasi, Frekuensi Radio Lokal." },
  { "en": "Apa Itu Antena 'PCB Trace'?", "id": "Antena Yang Dicetak, Langsung Di Papan Sirkuit." },
  { "en": "Apa Itu Konektor 'U.FL'?", "id": "Konektor Miniatur, Untuk Menghubungkan Antena Eksternal." },
  { "en": "Model Pi Mana Yang Punya Opsi Antena Eksternal?", "id": "Compute Module, Dan Beberapa Model Khusus." },
  { "en": "Apa Itu 'cgroups' (Control Groups)?", "id": "Fitur Kernel Linux, Untuk Mengelola Sumber Daya." },
  { "en": "Apa Manfaat Dari 'cgroups'?", "id": "Membatasi Penggunaan CPU, Memori, Suatu Proses." },
  { "en": "Apa Itu 'Linux Namespaces'?", "id": "Fitur Kernel, Untuk Mengisolasi Sumber Daya Sistem." },
  { "en": "Bagaimana 'Docker' Menggunakan 'cgroups' Dan 'Namespaces'?", "id": "Untuk Membuat Kontainer, Yang Ringan Terisolasi." },
  { "en": "Apakah Pi Memiliki 'Hardware Cryptographic Engine'?", "id": "Ya, SoC Memiliki Akselerasi Perangkat Keras." },
  { "en": "Apa Manfaat Akselerasi Kriptografi Perangkat Keras?", "id": "Mempercepat Operasi Enkripsi, Seperti AES." },
  { "en": "Apa Itu 'JTAG' Dan 'SWD'?", "id": "Antarmuka Debugging Perangkat Keras, Tingkat Rendah." },
  { "en": "Apa Kegunaan 'JTAG' Untuk Pengembang?", "id": "Memprogram Flash, Dan Debug Kernel Langsung." },
  { "en": "Apa Itu 'Data Logger'?", "id": "Perangkat Yang Merekam Data, Seiring Waktu." },
  { "en": "Mengapa Pi Cocok Dijadikan 'Data Logger'?", "id": "Konsumsi Daya Rendah, Dan Konektivitas Fleksibel." },
  { "en": "Apa Itu Sistem 'SCADA'?", "id": "Supervisory Control and Data Acquisition, Industri." },
  { "en": "Bisakah Pi Digunakan Dalam Aplikasi Industri Ringan?", "id": "Ya, Terutama Dengan Adanya Compute Module." },
  { "en": "Apa Beda Compute Module 4 Dengan Pi 4?", "id": "CM4 Tanpa Port, Untuk Sistem Tertanam." },
  { "en": "Apa Itu 'Carrier Board' Untuk Compute Module?", "id": "Papan Utama, Yang Menyediakan Port Fisik." },
  { "en": "Apa Itu 'eMMC' (Embedded MultiMediaCard)?", "id": "Penyimpanan Flash Internal, Di Compute Module." },
  { "en": "Apa Keunggulan 'eMMC' Dibandingkan Kartu SD?", "id": "Lebih Cepat, Dan Jauh Lebih Andal." },
  { "en": "Apa Itu Filesystem 'F2FS'?", "id": "Flash-Friendly File System, Dari Perusahaan Samsung." },
  { "en": "Kapan 'F2FS' Menjadi Pilihan Yang Baik?", "id": "Saat Menggunakan Media, Penyimpanan Berbasis Flash." },
  { "en": "Apa Itu Filesystem 'Btrfs'?", "id": "B-Tree File System, Dengan Fitur Modern." },
  { "en": "Apa Fitur Unggulan Dari 'Btrfs'?", "id": "Snapshot, Checksum Data, Dan Manajemen Volume." },
  { "en": "Apa Itu 'RAID' (Redundant Array of Independent Disks)?", "id": "Menggabungkan Beberapa Disk, Untuk Kinerja Redundansi." },
  { "en": "Bisakah Membuat 'Software RAID' Menggunakan Raspberry Pi?", "id": "Ya, Menggunakan Utilitas Linux Bernama 'mdadm'." },
  { "en": "Apa Itu 'Overlay File System'?", "id": "Menggabungkan Beberapa Filesystem, Menjadi Satu Tampilan." },
  { "en": "Bagaimana Mode 'Read-Only' Menggunakan 'OverlayFS'?", "id": "Perubahan Ditulis Ke Lapisan, Atas Memori." },
  { "en": "Apa Itu 'systemd-resolved'?", "id": "Layanan Sistem, Untuk Resolusi Nama Jaringan." },
  { "en": "Apa Itu 'systemd-networkd'?", "id": "Daemon Untuk Mengelola, Konfigurasi Antarmuka Jaringan." },
  { "en": "Apa Itu 'NetworkManager'?", "id": "Utilitas Populer, Untuk Mengelola Jaringan." },
  { "en": "Apa Beda 'systemd-networkd' Dan 'NetworkManager'?", "id": "NetworkManager Lebih Grafis, Networkd Berbasis Teks." },
  { "en": "Apa Itu 'Bluetooth Profile'?", "id": "Spesifikasi Untuk Berbagai, Jenis Komunikasi Bluetooth." },
  { "en": "Apa Itu 'A2DP' (Advanced Audio Distribution Profile)?", "id": "Profil Bluetooth, Untuk Streaming Audio Stereo." },
  { "en": "Apa Itu 'HID' (Human Interface Device) Profile?", "id": "Profil Untuk Keyboard, Mouse, Dan Gamepad." },
  { "en": "Apa Itu 'Logic Analyzer'?", "id": "Alat Debugging, Untuk Menganalisis Sinyal Digital." },
  { "en": "Bisakah Pi Pico Berfungsi Sebagai 'Logic Analyzer'?", "id": "Ya, Dengan Perangkat Lunak Khusus Seperti Sigrok." },
  { "en": "Apa Itu 'Oscilloscope'?", "id": "Alat Uji, Untuk Memvisualisasikan Sinyal Listrik." },
  { "en": "Apa Itu 'Bus Pirate'?", "id": "Alat Universal, Untuk Berinteraksi Dengan Bus." },
  { "en": "Apa Itu 'Digital Signature'?", "id": "Skema Matematika, Untuk Memverifikasi Keaslian Data." },
  { "en": "Apa Itu 'Checksum'?", "id": "Nilai Kecil, Untuk Mendeteksi Kesalahan Data." },
  { "en": "Apa Contoh Algoritma 'Checksum'?", "id": "CRC32 (Cyclic Redundancy Check), Yang Populer." },
  { "en": "Apa Itu 'Cryptographic Hash Function'?", "id": "Fungsi Satu Arah, Untuk Memetakan Data." },
  { "en": "Apa Contoh Algoritma 'Hash' Yang Kuat?", "id": "SHA-256 (Secure Hash Algorithm 256-bit)." },
  { "en": "Apa Beda 'Checksum' Dan 'Hash' Kriptografi?", "id": "Hash Kriptografi, Dirancang Tahan Manipulasi." },
  { "en": "Apa Itu 'Public-Key Cryptography'?", "id": "Menggunakan Sepasang Kunci, Publik Dan Privat." },
  { "en": "Bagaimana 'SSH' Menggunakan Kriptografi Kunci Publik?", "id": "Untuk Otentikasi Aman, Antara Klien Server." },
  { "en": "Apa Itu 'initramfs'?", "id": "Initial RAM File System, Sebelum Root Mount." },
  { "en": "Apa Fungsi Dari 'initramfs'?", "id": "Memuat Driver Penting, Untuk Mengakses Root." },
  { "en": "Apa Itu 'udev' Di Linux?", "id": "Manajer Perangkat, Yang Mengelola '/dev' Direktori." },
  { "en": "Bagaimana 'udev' Bekerja?", "id": "Membuat Aturan, Untuk Penamaan Perangkat Dinamis." },
  { "en": "Apa Itu 'Zero-copy' Operation?", "id": "Operasi Data, Tanpa Menyalin Antar Memori." },
  { "en": "Apa Itu 'Direct Memory Access' (DMA)?", "id": "Memungkinkan Perangkat Keras, Mengakses Memori Langsung." },
  { "en": "Apa Manfaat Dari 'DMA'?", "id": "Mengurangi Beban CPU, Saat Transfer Data." },
  { "en": "Apa Itu 'Memory-Mapped I/O'?", "id": "Memetakan Register Perangkat, Ke Ruang Memori." },
  { "en": "Apa Itu 'POSIX' (Portable Operating System Interface)?", "id": "Standar Yang Mendefinisikan, API Sistem Operasi." },
  { "en": "Apakah Linux Merupakan Sistem Yang Kompatibel 'POSIX'?", "id": "Ya, Sebagian Besar Kompatibel Dengan Standar." },
  { "en": "Apa Itu 'C library' (libc)?", "id": "Pustaka Standar, Untuk Bahasa Pemrograman C." },
  { "en": "Apa Implementasi 'C library' Yang Umum?", "id": "glibc (GNU), musl, uClibc Yang Populer." },
  { "en": "Apa Itu 'BusyBox'?", "id": "Satu File Eksekusi, Dengan Banyak Utilitas." },
  { "en": "Di Mana 'BusyBox' Sering Digunakan?", "id": "Di Sistem Tertanam, Dan Lingkungan Terbatas." },
  { "en": "Apa Itu 'Framebuffer' Linux?", "id": "API Kernel, Untuk Menampilkan Grafis Tanpa X11." },
  { "en": "Apa Itu 'ZRAM'?", "id": "Modul Kernel, Membuat Disk RAM Terkompresi." },
  { "en": "Apa Manfaat Menggunakan 'ZRAM'?", "id": "Meningkatkan Kinerja, Pada Sistem RAM Terbatas." },
  { "en": "Apa Itu 'Time Sensitive Networking' (TSN)?", "id": "Standar Ethernet, Untuk Komunikasi Waktu Nyata." },
  { "en": "Apa Itu Bahasa Pemrograman 'Go' (Golang)?", "id": "Bahasa Kompilasi, Yang Dibuat Oleh Google." },
  { "en": "Apa Itu Bahasa Pemrograman 'Rust'?", "id": "Bahasa Kompilasi, Fokus Pada Keamanan Memori." },
  { "en": "Bisakah 'Go' Dan 'Rust' Dijalankan Di Pi?", "id": "Ya, Keduanya Mendukung Arsitektur ARM Pi." },
  { "en": "Apa Itu 'WebAssembly' (Wasm)?", "id": "Format Instruksi Biner, Untuk Aplikasi Web." },
  { "en": "Apa Itu 'CAN bus'?", "id": "Controller Area Network, Bus Kendaraan Kuat." },
  { "en": "Bisakah Pi Berkomunikasi Melalui 'CAN bus'?", "id": "Ya, Dengan Menggunakan Papan HAT Transceiver." },
  { "en": "Apa Itu 'Modbus' Protocol?", "id": "Protokol Komunikasi Serial, Untuk Otomasi Industri." },
  { "en": "Apa Itu 'RS-232', 'RS-422', 'RS-485'?", "id": "Standar Komunikasi, Serial Jarak Jauh." },
  { "en": "Apa Beda 'RS-232' Dengan 'RS-485'?", "id": "RS-485, Mendukung Jarak Dan Perangkat Lebih." },
  { "en": "Apa Itu 'LoRa' Dan 'LoRaWAN'?", "id": "Teknologi Radio Nirkabel, Jarak Jauh Daya Rendah." },
  { "en": "Apa Kegunaan 'LoRaWAN'?", "id": "Aplikasi IoT, Seperti Pertanian Dan Kota Cerdas." },
  { "en": "Apa Itu 'Zigbee'?", "id": "Protokol Jaringan Nirkabel, Untuk Otomasi Rumah." },
  { "en": "Apa Itu 'Z-Wave'?", "id": "Protokol Nirkabel Lain, Pesaing Dari Zigbee." },
  { "en": "Apa Itu 'Thread' Networking Protocol?", "id": "Protokol Jaringan Mesh, Berbasis IPv6." },
  { "en": "Apa Itu 'Matter' (Smart Home Standard)?", "id": "Standar Terpadu, Untuk Perangkat Rumah Pintar." },
  { "en": "Apa Itu 'Home Assistant'?", "id": "Perangkat Lunak Otomasi Rumah, Sumber Terbuka." },
  { "en": "Bisakah 'Home Assistant' Dijalankan Di Pi?", "id": "Ya, Pi Adalah Platform Paling Populer." },
  { "en": "Apa Misi Utama Dari 'Raspberry Pi Foundation'?", "id": "Memajukan Pendidikan, Di Bidang Ilmu Komputer." },
  { "en": "Apa Itu 'Code Club'?", "id": "Jaringan Global Klub, Coding Gratis Untuk Anak." },
  { "en": "Apa Itu 'CoderDojo'?", "id": "Gerakan Global Klub, Coding Gratis Berbasis Komunitas." },
  { "en": "Apa Fungsi Opsi Lanjutan Di Raspberry Pi Imager?", "id": "Mengatur Hostname, SSH, Dan Wi-Fi Otomatis." },
  { "en": "Bisakah 'Raspberry Pi Imager' Mengunduh Sistem Operasi?", "id": "Ya, Imager Dapat Mengunduh Dan Menulis OS." },
  { "en": "Apa Itu Perintah 'git branch'?", "id": "Mengelola Cabang Pengembangan, Dalam Sebuah Repositori." },
  { "en": "Apa Itu Perintah 'git merge'?", "id": "Menggabungkan Perubahan, Dari Satu Cabang Ke Cabang." },
  { "en": "Apa Itu 'Merge Conflict' Di Git?", "id": "Terjadi Saat Perubahan, Tumpang Tindih Antar Cabang." },
  { "en": "Apa Itu 'git rebase'?", "id": "Alternatif 'merge', Untuk Mengintegrasikan Perubahan Kode." },
  { "en": "Apa Itu 'Thermistor'?", "id": "Resistor Termal, Nilainya Berubah Dengan Suhu." },
  { "en": "Apa Beda 'NTC' Dan 'PTC' Thermistor?", "id": "NTC Turun Suhu Naik, PTC Sebaliknya." },
  { "en": "Apa Itu 'H-Bridge'?", "id": "Sirkuit Elektronik, Membalik Polaritas Motor DC." },
  { "en": "Apa Kegunaan Utama Dari 'H-Bridge'?", "id": "Mengontrol Arah Putaran, Dari Sebuah Motor." },
  { "en": "Apa Itu 'L298N' Motor Driver?", "id": "Modul 'H-Bridge' Ganda, Populer Untuk Robotika." },
  { "en": "Apa Itu 'Pulse Density Modulation' (PDM)?", "id": "Bentuk Modulasi, Untuk Representasi Sinyal Analog." },
  { "en": "Mikrofon Digital Mana Yang Menggunakan Output 'PDM'?", "id": "Banyak Mikrofon 'MEMS', Menggunakan Antarmuka PDM." },
  { "en": "Apa Itu 'Pure Data' (Pd)?", "id": "Lingkungan Pemrograman Grafis, Untuk Audio Musik." },
  { "en": "Apa Itu 'SuperCollider'?", "id": "Platform Pemrograman, Untuk Sintesis Audio Waktu Nyata." },
  { "en": "Apa Itu 'KDE Plasma' Desktop?", "id": "Lingkungan Desktop Modern, Kaya Fitur Kustomisasi." },
  { "en": "Apa Itu 'XFCE' Desktop?", "id": "Lingkungan Desktop Ringan, Cepat, Dan Efisien." },
  { "en": "Bisakah Lingkungan Desktop Berbeda Diinstal Di Pi?", "id": "Ya, Banyak Pilihan Desktop Tersedia Repositori." },
  { "en": "Apa Itu 'Podman'?", "id": "Alternatif 'Docker', Yang Tidak Membutuhkan Daemon." },
  { "en": "Apa Itu 'Buildah'?", "id": "Alat Baris Perintah, Untuk Membangun Gambar Kontainer." },
  { "en": "Apa Perintah 'dd'?", "id": "Utilitas Tingkat Rendah, Untuk Menyalin Konversi Data." },
  { "en": "Bagaimana 'dd' Digunakan Untuk Mencadangkan Kartu SD?", "id": "Menyalin Seluruh Konten, Ke File Image." },
  { "en": "Apa Risiko Menggunakan Perintah 'dd'?", "id": "Bisa Menghapus Disk, Jika Salah Digunakan." },
  { "en": "Apa Itu 'partclone'?", "id": "Utilitas Mencadangkan Memulihkan, Partisi Secara Efisien." },
  { "en": "Apa Itu 'Etcher'?", "id": "Aplikasi Grafis Populer, Untuk Menulis Image." },
  { "en": "Bagaimana Cara Mengelola Versi Skematik Elektronik?", "id": "Menggunakan Git, Sama Seperti Perangkat Lunak." },
  { "en": "Apa Itu 'KiCad Version Control'?", "id": "Integrasi Git, Langsung Di Dalam Perangkat KiCad." },
  { "en": "Apa Itu Mode 'Monitor' Wi-Fi?", "id": "Menangkap Semua Paket, Di Udara Sekitar." },
  { "en": "Apa Itu 'Aircrack-ng' Suite?", "id": "Kumpulan Alat, Untuk Audit Keamanan Jaringan Wi-Fi." },
  { "en": "Apa Itu 'Wi-Fi Direct'?", "id": "Standar Menghubungkan Perangkat, Tanpa Titik Akses." },
  { "en": "Apa Itu 'Wi-Fi Protected Setup' (WPS)?", "id": "Standar Koneksi Mudah, Namun Kurang Aman." },
  { "en": "Apa Itu Perintah 'iwlist'?", "id": "Menampilkan Informasi Rinci, Tentang Antarmuka Nirkabel." },
  { "en": "Bagaimana Cara Memindai Jaringan Wi-Fi Dari Terminal?", "id": "Gunakan Perintah, 'sudo iwlist wlan0 scan'." },
  { "en": "Apa Itu 'Sandboxing' Aplikasi?", "id": "Menjalankan Aplikasi, Dalam Lingkungan Terisolasi Terbatas." },
  { "en": "Apa Contoh Teknologi 'Sandboxing' Di Linux?", "id": "Flatpak, Snap, Dan Juga Bubblewrap." },
  { "en": "Apa Itu 'Firejail'?", "id": "Program 'SUID', Untuk Sandbox Aplikasi Linux." },
  { "en": "Apa Itu 'Wayland' Display Server?", "id": "Protokol Server Tampilan, Modern Untuk Linux." },
  { "en": "Apa Keunggulan 'Wayland' Dibandingkan 'X11'?", "id": "Lebih Sederhana, Lebih Aman, Dan Modern." },
  { "en": "Apakah Raspberry Pi OS Menggunakan 'Wayland'?", "id": "Versi Terbaru, Mulai Beralih Ke Wayland." },
  { "en": "Apa Itu 'PipeWire'?", "id": "Server Multimedia Baru, Menangani Audio Video." },
  { "en": "Apa Tujuan Dari Proyek 'PipeWire'?", "id": "Menggantikan PulseAudio, Dan JACK Dengan Satu Solusi." },
  { "en": "Apa Itu 'VLSI' (Very Large Scale Integration)?", "id": "Proses Membuat Sirkuit, Terpadu Sangat Kompleks." },
  { "en": "Apa Itu 'SystemVerilog' Atau 'VHDL'?", "id": "Bahasa Deskripsi Perangkat Keras, (HDL)." },
  { "en": "Apa Itu 'FPGA'?", "id": "Field-Programmable Gate Array, Chip Logika Terprogram." },
  { "en": "Apa Beda 'FPGA' Dengan 'SoC'?", "id": "FPGA Dapat Dikonfigurasi Ulang, SoC Tetap." },
  { "en": "Apa Itu 'Application-Specific Integrated Circuit' (ASIC)?", "id": "Chip Didesain, Untuk Tugas Sangat Spesifik." },
  { "en": "Apa Itu 'Signal Integrity'?", "id": "Ukuran Kualitas Sinyal, Listrik Yang Ditransmisikan." },
  { "en": "Apa Itu 'Electromagnetic Interference' (EMI)?", "id": "Gangguan Elektromagnetik, Yang Merusak Sinyal." },
  { "en": "Bagaimana Cara Mengurangi 'EMI' Dalam Desain?", "id": "Dengan Grounding Baik, Dan Teknik Pelindung." },
  { "en": "Apa Itu 'Decoupling Capacitor'?", "id": "Menstabilkan Tegangan Catu Daya, Pada Chip." },
  { "en": "Apa Itu 'Pull-up Resistor'?", "id": "Memastikan Sinyal, Dalam Kondisi High Default." },
  { "en": "Apa Itu 'Pull-down Resistor'?", "id": "Memastikan Sinyal, Dalam Kondisi Low Default." },
  { "en": "Apa Itu 'Open Drain' Atau 'Open Collector'?", "id": "Jenis Output, Yang Hanya Bisa Menarik Low." },
  { "en": "Apa Itu 'Schmitt Trigger'?", "id": "Sirkuit Komparator, Dengan Histeresis Dua Ambang." },
  { "en": "Apa Manfaat Dari 'Schmitt Trigger'?", "id": "Mengubah Sinyal Analog Bising, Menjadi Digital." },
  { "en": "Apa Itu 'Firmware' Compute Module?", "id": "Perangkat Lunak, Yang Memungkinkan Boot Dari eMMC." },
  { "en": "Bagaimana Cara 'Flash' 'eMMC' Compute Module?", "id": "Menggunakan Utilitas 'rpiboot', Dari Komputer Host." },
  { "en": "Apa Itu 'BitBake'?", "id": "Alat Pembangun Utama, Dari Proyek Yocto." },
  { "en": "Apa Itu 'Yocto Project'?", "id": "Proyek Kolaborasi, Membuat Sistem Linux Kustom." },
  { "en": "Apa Itu 'Build Automation'?", "id": "Proses Otomatisasi, Kompilasi Dan Pengujian Kode." },
  { "en": "Apa Itu 'Jenkins'?", "id": "Server Otomatisasi Terbuka, Populer Untuk CI/CD." },
  { "en": "Apa Itu 'GitLab CI/CD'?", "id": "Fitur Integrasi Berkelanjutan, Bawaan Dari GitLab." },
  { "en": "Apa Itu 'Static Code Analysis'?", "id": "Menganalisis Kode Sumber, Tanpa Menjalankannya." },
  { "en": "Apa Contoh Alat 'Static Code Analysis'?", "id": "Pylint Untuk Python, Cppcheck Untuk C++." },
  { "en": "Apa Itu 'Code Coverage'?", "id": "Metrik Persentase Kode, Yang Diuji Otomatis." },
  { "en": "Apa Itu 'Unit Testing'?", "id": "Menguji Bagian Terkecil, Dari Sebuah Kode." },
  { "en": "Apa Itu 'Integration Testing'?", "id": "Menguji Gabungan, Beberapa Modul Kode Bersama." },
  { "en": "Apa Itu 'Regression Testing'?", "id": "Memastikan Perubahan Baru, Tidak Merusak Fitur." },
  { "en": "Apa Itu 'Fuzzing'?", "id": "Teknik Pengujian Keamanan, Dengan Input Acak." },
  { "en": "Apa Itu 'Penetration Testing'?", "id": "Simulasi Serangan Siber, Untuk Menemukan Kerentanan." },
  { "en": "Apa Itu 'OWASP Top 10'?", "id": "Daftar Risiko Keamanan, Aplikasi Web Teratas." },
  { "en": "Apa Itu 'Common Vulnerabilities and Exposures' (CVE)?", "id": "Daftar Kerentanan Keamanan, Yang Diungkapkan Publik." },
  { "en": "Apa Itu 'Bug Bounty Program'?", "id": "Program Hadiah, Untuk Menemukan Kerentanan Keamanan." },
  { "en": "Apa Itu 'Zero-Day Vulnerability'?", "id": "Kerentanan Yang Belum, Diketahui Oleh Pengembang." },
  { "en": "Apa Itu 'Phishing'?", "id": "Serangan Rekayasa Sosial, Untuk Mencuri Informasi." },
  { "en": "Apa Itu 'Malware'?", "id": "Perangkat Lunak Berbahaya, Termasuk Virus Ransomware." },
  { "en": "Apa Itu 'Ransomware'?", "id": "Malware Yang Mengenkripsi File, Dan Meminta Tebusan." },
  { "en": "Apa Itu 'Spyware'?", "id": "Malware Yang Memata-matai, Aktivitas Pengguna." },
  { "en": "Apa Itu 'Adware'?", "id": "Malware Yang Menampilkan, Iklan Tidak Diinginkan." },
  { "en": "Apa Itu 'Trojan Horse'?", "id": "Malware Yang Menyamar, Sebagai Program Sah." },
  { "en": "Apa Itu 'Rootkit'?", "id": "Malware Untuk Akses Root, Yang Tersembunyi." },
  { "en": "Apa Itu 'Botnet'?", "id": "Jaringan Komputer Terinfeksi, Yang Dikendalikan Penyerang." },
  { "en": "Bagaimana Cara Mencegah Infeksi 'Malware'?", "id": "Selalu Perbarui Sistem, Dan Gunakan Antivirus." },
  { "en": "Apa Itu 'Two-Factor Authentication' (2FA)?", "id": "Metode Keamanan, Yang Membutuhkan Dua Bukti." },
  { "en": "Apa Itu 'Password Manager'?", "id": "Aplikasi Untuk Menyimpan, Dan Mengelola Kata Sandi." },
  { "en": "Mengapa Menggunakan 'Password Manager' Penting?", "id": "Membantu Membuat, Kata Sandi Unik Kuat." },
  { "en": "Apa Itu 'Full Disk Encryption'?", "id": "Mengenkripsi Seluruh Data, Yang Ada Di Disk." },
  { "en": "Apa Itu 'LUKS' (Linux Unified Key Setup)?", "id": "Spesifikasi Standar, Enkripsi Disk Di Linux." },
  { "en": "Apa Itu 'dm-crypt'?", "id": "Subsistem Kernel Linux, Untuk Enkripsi Disk." },
  { "en": "Apa Itu 'Secure Boot'?", "id": "Fitur Keamanan, Memastikan Perangkat Lunak Sah." },
  { "en": "Apakah Raspberry Pi Mendukung 'Secure Boot'?", "id": "Tidak, Secara Bawaan Pi Tidak Mendukungnya." },
  { "en": "Apa Itu 'Trusted Platform Module' (TPM)?", "id": "Chip Perangkat Keras, Untuk Fungsi Kriptografi." },
  { "en": "Apa Itu 'Ham Radio'?", "id": "Sebutan Lain, Untuk Kegiatan Radio Amatir." },
  { "en": "Bagaimana Pi Digunakan Dalam Proyek 'Ham Radio'?", "id": "Sebagai Kontroler, Dan Pemroses Sinyal Digital." },
  { "en": "Apa Itu 'WSPR' (Weak Signal Propagation Reporter)?", "id": "Protokol Radio, Untuk Komunikasi Sinyal Lemah." },
  { "en": "Apa Itu 'Astrophotography'?", "id": "Fotografi Objek Astronomi, Seperti Bintang Planet." },
  { "en": "Bagaimana Pi Membantu Dalam 'Astrophotography'?", "id": "Mengontrol Teleskop, Dan Menangkap Gambar Otomatis." },
  { "en": "Apa Itu 'INDI' (Instrument Neutral Distributed Interface)?", "id": "Protokol Kontrol, Perangkat Astronomi Sumber Terbuka." },
  { "en": "Apa Itu 'DAC' (Digital-to-Analog Converter) HAT?", "id": "Papan HAT, Untuk Output Audio Kualitas Tinggi." },
  { "en": "Apa Contoh Merek 'DAC HAT' Populer?", "id": "HiFiBerry, JustBoom, Dan IQaudIO Yang Terkenal." },
  { "en": "Apa Itu 'Dynamic Kernel Module Support' (DKMS)?", "id": "Kerangka Kerja, Membangun Ulang Modul Kernel." },
  { "en": "Mengapa 'DKMS' Berguna?", "id": "Modul Tetap Bekerja, Setelah Pembaruan Kernel." },
  { "en": "Bagaimana Cara Mengkompilasi Kernel Linux Sendiri?", "id": "Proses Panjang, Melibatkan Konfigurasi Dan Kompilasi." },
  { "en": "Apa Manfaat Mengkompilasi Kernel Kustom Sendiri?", "id": "Mengaktifkan Fitur Khusus, Dan Optimasi Ukuran." },
  { "en": "Apa Itu 'Wear Leveling'?", "id": "Teknik Meratakan Penggunaan, Blok Memori Flash." },
  { "en": "Mengapa 'Wear Leveling' Penting Untuk SSD?", "id": "Memperpanjang Umur, Dan Kinerja Media Penyimpanan." },
  { "en": "Apa Itu Perintah 'TRIM' Untuk SSD?", "id": "Memberitahu SSD, Blok Mana Yang Kosong." },
  { "en": "Apa Manfaat Mengaktifkan 'TRIM'?", "id": "Menjaga Kinerja Tulis, SSD Tetap Optimal." },
  { "en": "Apa Itu 'Compute Module 4 IO Board'?", "id": "Papan Pengembangan Resmi, Untuk Modul CM4." },
  { "en": "Apa Itu 'PCI Express' (PCIe)?", "id": "Standar Bus Ekspansi, Serial Kecepatan Tinggi." },
  { "en": "Apakah Compute Module 4 Memiliki Jalur 'PCIe'?", "id": "Ya, Memiliki Satu Jalur (Lane) PCIe 2.0." },
  { "en": "Perangkat Apa Yang Bisa Dihubungkan Via 'PCIe'?", "id": "SSD NVMe, Kartu Jaringan, Dan Lainnya." },
  { "en": "Bagaimana Cara Mengurangi Konsumsi Daya Pi?", "id": "Menonaktifkan HDMI, LED, Dan Menurunkan Clock." },
  { "en": "Apa Itu 'Underclocking'?", "id": "Menurunkan Kecepatan Jam, CPU Untuk Hemat Daya." },
  { "en": "Apa Itu Modul 'GSM' Atau 'LTE'?", "id": "Modul Untuk Konektivitas, Jaringan Seluler." },
  { "en": "Bagaimana Modul 'GSM' Berkomunikasi Dengan Pi?", "id": "Melalui Antarmuka USB, Atau Serial UART." },
  { "en": "Apa Itu 'AT Commands'?", "id": "Perintah Hayes, Untuk Mengontrol Perangkat Modem." },
  { "en": "Apa Itu 'Message Passing Interface' (MPI)?", "id": "Standar Komunikasi, Untuk Komputasi Kinerja Tinggi." },
  { "en": "Bagaimana 'MPI' Digunakan Pada Kluster Pi?", "id": "Mengkoordinasikan Tugas, Antara Beberapa Node Pi." },
  { "en": "Apa Itu 'OpenMP'?", "id": "API Untuk Pemrograman, Paralel Memori Bersama." },
  { "en": "Apa Itu Perintah 'xargs'?", "id": "Membangun Dan Menjalankan, Perintah Dari Input." },
  { "en": "Apa Itu Perintah 'tee'?", "id": "Membaca Dari Input, Menulis Ke Output File." },
  { "en": "Apa Kegunaan Utama Dari Perintah 'tee'?", "id": "Melihat Output, Sambil Menyimpannya Ke File." },
  { "en": "Apa Itu Perintah 'nice' Dan 'renice'?", "id": "Mengatur Prioritas Penjadwalan, Proses Di Linux." },
  { "en": "Bagaimana Cara Mengakses Pembaca 'Floppy Drive'?", "id": "Sangat Sulit, Membutuhkan Kontroler USB Khusus." },
  { "en": "Apa Itu 'Orca Screen Reader'?", "id": "Pembaca Layar Gratis, Untuk Lingkungan Desktop." },
  { "en": "Bagaimana Cara Mengaktifkan Fitur Aksesibilitas?", "id": "Melalui Menu Universal Access, Di Desktop." },
  { "en": "Apa Itu 'On-Screen Keyboard'?", "id": "Keyboard Virtual, Yang Tampil Di Layar." },
  { "en": "Apa Itu 'BeagleBone Black'?", "id": "Komputer Papan Tunggal, Populer Pesaing Pi." },
  { "en": "Apa Keunggulan Utama 'BeagleBone'?", "id": "Input/Output Lebih Banyak, Dan Mikrokontroler PRU." },
  { "en": "Apa Itu 'Odroid'?", "id": "Seri Komputer Papan Tunggal, Dari Hardkernel." },
  { "en": "Apa Itu 'Arduino'?", "id": "Platform Mikrokontroler Populer, Untuk Proyek Elektronik." },
  { "en": "Kapan Sebaiknya Menggunakan Arduino Daripada Pi?", "id": "Untuk Tugas Waktu Nyata, Dan Kontrol Sederhana." },
  { "en": "Apa Itu 'PlatformIO'?", "id": "Ekosistem Pengembangan Lintas Platform, Untuk Tertanam." },
  { "en": "Apa Itu 'Circuit Diagram' Atau Skematik?", "id": "Representasi Grafis, Dari Sebuah Sirkuit Elektronik." },
  { "en": "Apa Itu 'Printed Circuit Board' (PCB)?", "id": "Papan Yang Menghubungkan, Komponen Secara Fisik." },
  { "en": "Apa Proses Desain 'PCB'?", "id": "Desain Skematik, Tata Letak, Dan Gernerate Gerber." },
  { "en": "Apa Itu 'Gerber File'?", "id": "Format Standar Industri, Untuk Manufaktur PCB." },
  { "en": "Apa Itu 'Soldering'?", "id": "Proses Menyatukan Komponen, Menggunakan Timah Panas." },
  { "en": "Apa Itu 'Through-Hole' Component?", "id": "Komponen Dengan Kaki, Yang Masuk Lubang PCB." },
  { "en": "Apa Itu 'Surface-Mount Device' (SMD)?", "id": "Komponen Yang Disolder, Langsung Di Permukaan PCB." },
  { "en": "Apa Itu 'Reflow Soldering'?", "id": "Proses Melelehkan Pasta Solder, Dengan Oven." },
  { "en": "Apa Itu 'Bill of Materials' (BOM)?", "id": "Daftar Semua Komponen, Yang Dibutuhkan Proyek." },
  { "en": "Apa Itu 'SPDT' Switch?", "id": "Single Pole, Double Throw, Saklar Tiga Terminal." },
  { "en": "Apa Itu 'DPDT' Switch?", "id": "Double Pole, Double Throw, Saklar Enam Terminal." },
  { "en": "Apa Itu 'Rotary Encoder'?", "id": "Sensor Input Rotasi, Yang Memberikan Pulsa." },
  { "en": "Apa Beda 'Rotary Encoder' Dan Potensiometer?", "id": "Encoder Berputar Terus, Potensiometer Terbatas." },
  { "en": "Apa Itu 'Infrared' (IR) Receiver?", "id": "Sensor Yang Menerima Sinyal, Dari Remote." },
  { "en": "Apa Itu 'LIRC' (Linux Infrared Remote Control)?", "id": "Paket Perangkat Lunak, Untuk Menerima Sinyal IR." },
  { "en": "Apa Itu 'Ohm's Law'?", "id": "Hubungan Antara Tegangan, Arus, Dan Resistansi." },
  { "en": "Bagaimana Rumus 'Ohm's Law'?", "id": "Tegangan (V) = Arus (I) * Resistansi (R)." },
  { "en": "Apa Itu 'Kirchhoff's Current Law' (KCL)?", "id": "Total Arus Masuk, Sama Dengan Arus Keluar." },
  { "en": "Apa Itu 'Kirchhoff's Voltage Law' (KVL)?", "id": "Total Tegangan Dalam, Loop Tertutup Adalah Nol." },
  { "en": "Apa Itu 'Alternating Current' (AC)?", "id": "Arus Listrik, Yang Arahnya Bolak-balik." },
  { "en": "Apa Itu 'Direct Current' (DC)?", "id": "Arus Listrik, Yang Mengalir Dalam Satu Arah." },
  { "en": "Apakah Raspberry Pi Menggunakan Listrik AC Atau DC?", "id": "Menggunakan Listrik DC, Lima Volt." },
  { "en": "Apa Itu 'Rectifier'?", "id": "Sirkuit Yang Mengubah, Listrik AC Menjadi DC." },
  { "en": "Apa Itu 'Voltage Regulator'?", "id": "Menjaga Tegangan Output, Tetap Stabil Konstan." },
  { "en": "Apa Itu 'Linear Regulator'?", "id": "Regulator Yang Membuang, Kelebihan Energi Panas." },
  { "en": "Apa Itu 'Switching Regulator'?", "id": "Regulator Efisien, Yang Menggunakan Saklar Elektronik." },
  { "en": "Apa Itu 'Buck Converter'?", "id": "Regulator Penurun Tegangan, (Step-Down) Yang Efisien." },
  { "en": "Apa Itu 'Boost Converter'?", "id": "Regulator Penaik Tegangan, (Step-Up) Yang Efisien." },
  { "en": "Apa Itu 'Unicode'?", "id": "Standar Pengkodean Karakter, Untuk Teks Komputer." },
  { "en": "Apa Itu 'UTF-8'?", "id": "Pengkodean Karakter, Paling Umum Di Internet." },
  { "en": "Apa Itu 'ASCII'?", "id": "American Standard Code, For Information Interchange." },
  { "en": "Apa Itu 'Hexadecimal' Number System?", "id": "Sistem Bilangan, Berbasis Enam Belas (0-9, A-F)." },
  { "en": "Mengapa 'Hexadecimal' Sering Digunakan?", "id": "Representasi Ringkas, Dari Bilangan Biner." },
  { "en": "Apa Itu 'Endianness'?", "id": "Urutan Byte, Dalam Representasi Data Multi-byte." },
  { "en": "Apa Itu 'Big-Endian'?", "id": "Byte Paling Signifikan, Disimpan Pertama Kali." },
  { "en": "Apa Itu 'Little-Endian'?", "id": "Byte Paling Tidak Signifikan, Disimpan Pertama." },
  { "en": "Arsitektur ARM Di Pi Menggunakan 'Endianness' Apa?", "id": "Dapat Dikonfigurasi, (Bi-Endian) Tapi Umumnya Little." },
  { "en": "Apa Itu 'Floating-Point Number'?", "id": "Angka Dengan Titik, Desimal Yang Mengambang." },
  { "en": "Apa Itu 'Integer'?", "id": "Bilangan Bulat, Tanpa Bagian Pecahan Apapun." },
  { "en": "Apa Itu 'Bitwise Operation'?", "id": "Operasi Logika, Yang Dilakukan Pada Level Bit." },
  { "en": "Apa Contoh 'Bitwise Operator'?", "id": "AND (&), OR (|), XOR (^), NOT (~)." },
  { "en": "Apa Itu 'Bit Shifting'?", "id": "Menggeser Bit, Ke Kiri Atau Kanan." },
  { "en": "Apa Efek Dari 'Left Bit Shift'?", "id": "Sama Dengan, Mengalikan Dengan Pangkat Dua." },
  { "en": "Apa Efek Dari 'Right Bit Shift'?", "id": "Sama Dengan, Membagi Dengan Pangkat Dua." },
  { "en": "Apa Itu 'Two's Complement'?", "id": "Metode Representasi, Bilangan Bulat Bertanda." },
  { "en": "Apa Itu 'Buffer Overflow'?", "id": "Kerentanan Keamanan, Saat Data Melebihi Buffer." },
  { "en": "Apa Itu 'Stack' Dalam Memori?", "id": "Struktur Data, Untuk Variabel Lokal Panggilan." },
  { "en": "Apa Itu 'Heap' Dalam Memori?", "id": "Area Memori, Untuk Alokasi Dinamis." },
  { "en": "Apa Itu 'Garbage Collection'?", "id": "Proses Otomatis, Mengklaim Ulang Memori Heap." },
  { "en": "Apakah Python Memiliki 'Garbage Collection'?", "id": "Ya, Python Mengelola Memori Secara Otomatis." },
  { "en": "Apakah C++ Memiliki 'Garbage Collection'?", "id": "Tidak Secara Bawaan, Butuh Manajemen Manual." },
  { "en": "Apa Itu 'Smart Pointer' Di C++?", "id": "Membantu Mengelola Memori, Secara Otomatis." },
  { "en": "Apa Itu 'Dangling Pointer'?", "id": "Pointer Yang Menunjuk, Ke Lokasi Memori." },
  { "en": "Apa Itu 'Creative Coding'?", "id": "Penggunaan Pemrograman Komputer, Untuk Tujuan Artistik." },
  { "en": "Apa Itu 'Processing'?", "id": "Bahasa Pemrograman Grafis, Populer Untuk Seni." },
  { "en": "Apa Itu 'OpenFrameworks'?", "id": "Toolkit C++, Untuk Aplikasi Kreatif Interaktif." },
  { "en": "Apa Itu 'MIDI'?", "id": "Musical Instrument Digital Interface, Standar Musik." },
  { "en": "Bisakah Pi Berfungsi Sebagai 'MIDI Controller'?", "id": "Ya, Dengan Perangkat Keras Dan Lunak Tepat." },
  { "en": "Apa Itu 'Software Synthesizer'?", "id": "Perangkat Lunak, Yang Menghasilkan Suara Audio." },
  { "en": "Apa Itu 'Zynthian'?", "id": "Platform Synthesizer Terbuka, Berbasis Raspberry Pi." },
  { "en": "Apa Itu 'Journaling' Dalam Filesystem?", "id": "Mencatat Perubahan, Sebelum Dilakukan Untuk Konsistensi." },
  { "en": "Filesystem Mana Yang Menggunakan 'Journaling'?", "id": "EXT4, XFS, Btrfs, Dan Banyak Lainnya." },
  { "en": "Apa Itu 'Inode'?", "id": "Struktur Data, Yang Menyimpan Metadata File." },
  { "en": "Informasi Apa Yang Disimpan Dalam 'Inode'?", "id": "Ukuran, Izin, Kepemilikan, Dan Lokasi Data." },
  { "en": "Apa Itu Perintah 'ps'?", "id": "Menampilkan Daftar Proses, Yang Sedang Berjalan." },
  { "en": "Bagaimana Cara Melihat Semua Proses Dengan 'ps'?", "id": "Gunakan Opsi, 'ps aux' Atau 'ps -ef'." },
  { "en": "Apa Itu 'Process ID' (PID)?", "id": "Nomor Unik, Yang Diberikan Ke Setiap Proses." },
  { "en": "Apa Itu Perintah 'kill'?", "id": "Mengirim Sinyal, Ke Sebuah Proses Berjalan." },
  { "en": "Sinyal Apa Yang Digunakan Untuk Menghentikan Paksa?", "id": "Sinyal Sembilan, Atau 'SIGKILL' (Signal Kill)." },
  { "en": "Apa Perbedaan Antara 'SIGTERM' Dan 'SIGKILL'?", "id": "SIGTERM Meminta Berhenti, SIGKILL Memaksa Berhenti." },
  { "en": "Apa Itu Proses 'Zombie'?", "id": "Proses Anak Selesai, Tapi Induk Belum." },
  { "en": "Apa Itu Proses 'Orphan'?", "id": "Proses Induk Selesai, Sebelum Proses Anaknya." },
  { "en": "Model Pi Apa Yang Pertama Kali Menggunakan 40 Pin?", "id": "Raspberry Pi, Model B+ Tahun 2014." },
  { "en": "Kapan Raspberry Pi 2 Dirilis?", "id": "Dirilis Pada, Bulan Februari Tahun 2015." },
  { "en": "Apa Peningkatan Besar Pada Raspberry Pi 3?", "id": "CPU 64-bit, Serta Wi-Fi Bluetooth." },
  { "en": "Apa Itu Codec Video 'H.264' (AVC)?", "id": "Advanced Video Coding, Standar Kompresi Video." },
  { "en": "Apa Itu Codec Video 'H.265' (HEVC)?", "id": "High Efficiency Video Coding, Lebih Efisien." },
  { "en": "Apakah Pi Memiliki Akselerasi Perangkat Keras Video?", "id": "Ya, GPU VideoCore Mendukung Dekode Enkode." },
  { "en": "Apa Itu 'Wireless Mesh Network'?", "id": "Jaringan Nirkabel, Dimana Node Saling Terhubung." },
  { "en": "Apa Itu 'Batman-adv'?", "id": "Better Approach To Mobile Adhoc Networking." },
  { "en": "Apa Manfaat Jaringan 'Mesh'?", "id": "Andal, Dan Dapat Memperluas Jangkauan Jaringan." },
  { "en": "Apa Itu Editor Teks 'Vim'?", "id": "Editor Teks Modal, Sangat Kuat Efisien." },
  { "en": "Apa Tiga Mode Dasar Di 'Vim'?", "id": "Mode Normal, Mode Insert, Dan Mode Command." },
  { "en": "Apa Itu Editor Teks 'Emacs'?", "id": "Editor Teks Fleksibel, Dapat Diperluas." },
  { "en": "Apa Perbedaan Utama 'Proses' Dan 'Thread'?", "id": "Proses Terisolasi, Thread Berbagi Ruang Memori." },
  { "en": "Mana Yang Lebih Ringan, 'Proses' Atau 'Thread'?", "id": "Thread, Jauh Lebih Ringan Untuk Dibuat." },
  { "en": "Apa Itu 'Multithreading'?", "id": "Menjalankan Beberapa Thread, Dalam Satu Proses." },
  { "en": "Apa Itu 'Multiprocessing'?", "id": "Menjalankan Beberapa Proses, Secara Bersamaan." },
  { "en": "Apa Itu 'Global Interpreter Lock' (GIL) Python?", "id": "Mekanisme Mencegah, Multithreading Paralel Sejati." },
  { "en": "Bagaimana Menggambar Langsung Ke 'Framebuffer'?", "id": "Menulis Data Piksel, Langsung Ke '/dev/fb0'." },
  { "en": "Apa Itu 'Pygame Framebuffer' (pyfb)?", "id": "Library Python, Untuk Mengakses Framebuffer Langsung." },
  { "en": "Apa Itu 'Kiosk Mode'?", "id": "Mode Mengunci Perangkat, Hanya Satu Aplikasi." },
  { "en": "Bagaimana Cara Mengatur Browser Dalam 'Kiosk Mode'?", "id": "Menggunakan Opsi Baris Perintah, '--kiosk'." },
  { "en": "Apa Itu 'Digital Signage'?", "id": "Papan Tanda Digital, Menampilkan Informasi Multimedia." },
  { "en": "Mengapa Pi Populer Untuk 'Digital Signage'?", "id": "Ukuran Kecil, Harga Murah, Kemampuan Multimedia." },
  { "en": "Apa Itu 'Screenly OSE'?", "id": "Perangkat Lunak 'Digital Signage', Terbuka Populer." },
  { "en": "Apa Beda Utama 'Buildroot' Dan 'Yocto'?", "id": "Buildroot Lebih Sederhana, Yocto Lebih Kompleks." },
  { "en": "Kapan Sebaiknya Memilih 'Buildroot'?", "id": "Untuk Sistem Tertanam, Sederhana Dan Cepat." },
  { "en": "Kapan Sebaiknya Memilih 'Yocto'?", "id": "Untuk Sistem Kompleks, Yang Butuh Kustomisasi." },
  { "en": "Apa Itu 'Bitmasking'?", "id": "Menggunakan Operasi Bitwise, Untuk Mengatur Flag." },
  { "en": "Apa Itu 'Stack Smashing'?", "id": "Serangan Keamanan, Yang Mengeksploitasi Buffer Overflow." },
  { "en": "Apa Itu 'Canary Value'?", "id": "Nilai Acak, Untuk Mendeteksi Stack Smashing." },
  { "en": "Apa Itu 'Address Space Layout Randomization' (ASLR)?", "id": "Teknik Keamanan, Mengacak Lokasi Memori Program." },
  { "en": "Apa Itu 'Data Execution Prevention' (DEP)?", "id": "Mencegah Eksekusi Kode, Dari Area Memori." },
  { "en": "Apa Itu 'Oscilloscope Probe'?", "id": "Kabel Dan Konektor, Untuk Mengukur Sinyal." },
  { "en": "Apa Itu 'Signal Generator'?", "id": "Alat Elektronik, Yang Menghasilkan Sinyal Listrik." },
  { "en": "Apa Jenis Gelombang Umum Dari 'Signal Generator'?", "id": "Sinus, Kotak (Square), Segitiga (Triangle)." },
  { "en": "Apa Itu 'Spectrum Analyzer'?", "id": "Mengukur Besaran Sinyal, Versus Frekuensi Sinyal." },
  { "en": "Apa Itu 'Fourier Transform'?", "id": "Menguraikan Sinyal, Menjadi Komponen Frekuensinya." },
  { "en": "Apa Itu 'Fast Fourier Transform' (FFT)?", "id": "Algoritma Efisien, Menghitung 'Fourier Transform'." },
  { "en": "Apa Kegunaan 'FFT' Dalam Proyek Audio?", "id": "Analisis Spektrum, Dan Visualisasi Musik." },
  { "en": "Apa Itu 'Digital Filter'?", "id": "Filter Matematika, Yang Bekerja Pada Sinyal." },
  { "en": "Apa Itu 'Low-Pass Filter'?", "id": "Melewatkan Sinyal Frekuensi Rendah, Meredam Tinggi." },
  { "en": "Apa Itu 'High-Pass Filter'?", "id": "Melewatkan Sinyal Frekuensi Tinggi, Meredam Rendah." },
  { "en": "Apa Itu 'Band-Pass Filter'?", "id": "Melewatkan Sinyal, Dalam Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu 'Finite Impulse Response' (FIR) Filter?", "id": "Salah Satu Jenis, Filter Digital." },
  { "en": "Apa Itu 'Infinite Impulse Response' (IIR) Filter?", "id": "Jenis Lain, Filter Digital Yang Rekursif." },
  { "en": "Apa Itu 'PID Controller'?", "id": "Proportional, Integral, Derivative Controller." },
  { "en": "Apa Kegunaan 'PID Controller'?", "id": "Sistem Kontrol Loop Tertutup, Sangat Umum." },
  { "en": "Apa Contoh Aplikasi 'PID Controller'?", "id": "Termostat, Kontrol Kecepatan Motor, Drone." },
  { "en": "Apa Itu 'Set-Point'?", "id": "Nilai Target, Yang Diinginkan Dalam Sistem." },
  { "en": "Apa Itu 'Process Variable'?", "id": "Nilai Aktual, Yang Diukur Dari Sistem." },
  { "en": "Apa Itu 'Error' Dalam Kontrol PID?", "id": "Selisih Antara, 'Set-Point' Dan 'Process Variable'." },
  { "en": "Apa Itu 'State Machine'?", "id": "Model Komputasi, Abstrak Berbasis Status." },
  { "en": "Apa Itu 'Finite State Machine' (FSM)?", "id": "State Machine, Dengan Jumlah Status Terbatas." },
  { "en": "Mengapa 'State Machine' Berguna Dalam Pemrograman?", "id": "Mengelola Logika Kompleks, Secara Terstruktur." },
  { "en": "Apa Itu 'Cache' Memori?", "id": "Memori Kecil Cepat, Menyimpan Data Sering." },
  { "en": "Apa Itu 'L1 Cache'?", "id": "Cache Level Satu, Tercepat Dan Terkecil." },
  { "en": "Apa Itu 'L2 Cache'?", "id": "Cache Level Dua, Lebih Lambat Dari L1." },
  { "en": "Apa Itu 'L3 Cache'?", "id": "Cache Level Tiga, Dibagi Antar Inti CPU." },
  { "en": "Apa Itu 'Cache Hit'?", "id": "Saat Data Yang Dicari, Ditemukan Dalam Cache." },
  { "en": "Apa Itu 'Cache Miss'?", "id": "Saat Data Yang Dicari, Tidak Ditemukan Cache." },
  { "en": "Apa Itu 'Instruction Pipeline'?", "id": "Teknik CPU, Tumpang Tindih Eksekusi Instruksi." },
  { "en": "Apa Itu 'Branch Prediction'?", "id": "Teknik CPU Menebak, Arah Percabangan Kode." },
  { "en": "Apa Itu 'Superscalar Architecture'?", "id": "Arsitektur CPU, Mengeksekusi Lebih Satu Instruksi." },
  { "en": "Apa Itu 'Out-of-Order Execution'?", "id": "Mengeksekusi Instruksi, Tidak Sesuai Urutan Program." },
  { "en": "Apa Itu 'SIMD' (Single Instruction, Multiple Data)?", "id": "Mengeksekusi Instruksi Sama, Pada Data Berbeda." },
  { "en": "Apa Contoh 'SIMD' Di Arsitektur ARM?", "id": "Instruksi NEON, Untuk Multimedia Dan Sinyal." },
  { "en": "Apa Itu 'Magic Number' Dalam File?", "id": "Urutan Byte Awal, Untuk Identifikasi Format." },
  { "en": "Apa Itu 'hexdump' Atau 'xxd'?", "id": "Utilitas Melihat File, Dalam Representasi Hexadecimal." },
  { "en": "Apa Itu 'strings' Command?", "id": "Mencari Dan Menampilkan, Teks Dalam File." },
  { "en": "Apa Itu 'ldd' Command?", "id": "Menampilkan Ketergantungan, Pustaka Bersama (Shared Library)." },
  { "en": "Apa Itu 'Shared Library' (.so file)?", "id": "Pustaka Kode, Yang Dibagi Antar Program." },
  { "en": "Apa Itu 'Static Linking'?", "id": "Menyalin Semua Kode Pustaka, Ke File Eksekusi." },
  { "en": "Apa Itu 'Dynamic Linking'?", "id": "Memuat Pustaka Bersama, Saat Program Dijalankan." },
  { "en": "Apa Keuntungan 'Dynamic Linking'?", "id": "Ukuran File Lebih Kecil, Pembaruan Mudah." },
  { "en": "Apa Itu 'Deadlock'?", "id": "Situasi Dua Proses, Saling Menunggu Sumber Daya." },
  { "en": "Apa Itu 'Race Condition'?", "id": "Hasil Bergantung, Pada Urutan Eksekusi Thread." },
  { "en": "Apa Itu 'Mutex' (Mutual Exclusion)?", "id": "Mekanisme Sinkronisasi, Mencegah 'Race Condition'." },
  { "en": "Apa Itu 'Semaphore'?", "id": "Mekanisme Sinkronisasi, Yang Mengizinkan Akses Terbatas." },
  { "en": "Apa Itu 'Amazon Web Services' (AWS) IoT Core?", "id": "Layanan Cloud, Menghubungkan Perangkat IoT Aman." },
  { "en": "Apa Itu 'Azure IoT Hub'?", "id": "Layanan Cloud Microsoft, Untuk Manajemen Perangkat." },
  { "en": "Apa Bahan 'Filament' Umum Untuk Cetak 3D?", "id": "PLA (Polylactic Acid), Dan ABS (Acrylonitrile Butadiene Styrene)." },
  { "en": "Apa Beda 'PLA' Dan 'ABS'?", "id": "PLA Mudah Dicetak, ABS Lebih Kuat." },
  { "en": "Apa Itu 'STL' File?", "id": "Standard Triangle Language, Format File 3D." },
  { "en": "Apa Itu 'Slicer' Dalam Cetak 3D?", "id": "Mengubah Model 3D, Menjadi Instruksi Cetak." },
  { "en": "Apa Itu 'Virtual Local Area Network' (VLAN)?", "id": "Jaringan Logis Terisolasi, Di Atas Fisik." },
  { "en": "Apa Itu 'Network Bridging'?", "id": "Menggabungkan Beberapa Antarmuka, Menjadi Satu Jaringan." },
  { "en": "Apa Itu 'Syslog'?", "id": "Standar Protokol, Untuk Pencatatan Pesan Sistem." },
  { "en": "Apa Itu 'Journald'?", "id": "Sistem Logging Bawaan, Dari Manajer Systemd." },
  { "en": "Bagaimana Cara Melihat Log 'Journald'?", "id": "Menggunakan Perintah, 'journalctl' Di Baris Perintah." },
  { "en": "Apa Itu 'logrotate'?", "id": "Utilitas Mengelola, Mengarsipkan, Dan Merotasi Log." },
  { "en": "Apa Itu 'Verilog'?", "id": "Bahasa Deskripsi Perangkat Keras, (HDL)." },
  { "en": "Apa Itu 'VHDL'?", "id": "VHSIC Hardware Description Language, Pesaing Verilog." },
  { "en": "Apa Itu 'Synthesis' Dalam Desain Chip?", "id": "Mengubah Kode HDL, Menjadi Desain Gerbang Logika." },
  { "en": "Apa Itu 'Place and Route'?", "id": "Proses Menempatkan Merutekan, Gerbang Di Chip." },
  { "en": "Apa Standar 'Power over Ethernet' (PoE) 802.3af?", "id": "Menyediakan Daya, Hingga Lima Belas Watt." },
  { "en": "Apa Standar 'PoE+' 802.3at?", "id": "Menyediakan Daya, Hingga Dua Puluh Lima Watt." },
  { "en": "Apa Standar 'PoE++' 802.3bt?", "id": "Menyediakan Daya, Jauh Lebih Tinggi Lagi." },
  { "en": "Apa Itu 'Power Sourcing Equipment' (PSE)?", "id": "Perangkat Yang Menyediakan, Daya (Contoh Switch)." },
  { "en": "Apa Itu 'Powered Device' (PD)?", "id": "Perangkat Yang Menerima, Daya (Contoh Pi)." },
  { "en": "Apa Itu 'NumPy' Library?", "id": "Pustaka Fundamental Python, Untuk Komputasi Numerik." },
  { "en": "Apa Struktur Data Utama Di 'NumPy'?", "id": "Array N-dimensi, Yang Sangat Efisien." },
  { "en": "Apa Itu 'SciPy' Library?", "id": "Pustaka Python, Untuk Komputasi Ilmiah Teknis." },
  { "en": "Apa Itu 'Pandas' Library?", "id": "Pustaka Python, Untuk Analisis Manipulasi Data." },
  { "en": "Apa Itu 'Matplotlib'?", "id": "Pustaka Plotting Komprehensif, Untuk Bahasa Python." },
  { "en": "Apa Perintah Untuk Menambah Pengguna Baru?", "id": "Gunakan Perintah, 'sudo adduser nama_pengguna'." },
  { "en": "Apa Perintah Untuk Menghapus Pengguna?", "id": "Gunakan Perintah, 'sudo deluser nama_pengguna'." },
  { "en": "Apa Itu File '/etc/passwd'?", "id": "Berisi Informasi, Dasar Akun Pengguna Sistem." },
  { "en": "Apa Itu File '/etc/shadow'?", "id": "Berisi Informasi Hash, Kata Sandi Pengguna." },
  { "en": "Apa Itu File '/etc/group'?", "id": "Mendefinisikan Grup, Dan Anggota-anggotanya." },
  { "en": "Apa Itu 'user ID' (UID)?", "id": "Nomor Unik, Yang Merepresentasikan Seorang Pengguna." },
  { "en": "Apa Itu 'group ID' (GID)?", "id": "Nomor Unik, Yang Merepresentasikan Sebuah Grup." },
  { "en": "Apa Itu 'NoSQL' Database?", "id": "Database Yang Tidak, Menggunakan Model Relasional." },
  { "en": "Apa Contoh Database 'NoSQL'?", "id": "MongoDB, Redis, Cassandra, Dan CouchDB." },
  { "en": "Apa Itu 'MongoDB'?", "id": "Database Berorientasi Dokumen, Menggunakan Format JSON." },
  { "en": "Apa Itu 'Redis'?", "id": "Penyimpanan Struktur Data, Dalam Memori Cepat." },
  { "en": "Apa Kegunaan Utama 'Redis'?", "id": "Sebagai Cache, Broker Pesan, Dan Database." },
  { "en": "Apa Itu 'Direct Rendering Manager' (DRM)?", "id": "Subsistem Kernel Linux, Mengelola GPU." },
  { "en": "Apa Itu 'Kernel Mode Setting' (KMS)?", "id": "Memindahkan Pengaturan Mode, Tampilan Ke Kernel." },
  { "en": "Apa Itu Lisensi 'MIT'?", "id": "Lisensi Perangkat Lunak, Bebas Yang Permisif." },
  { "en": "Apa Itu Lisensi 'Apache 2.0'?", "id": "Lisensi Bebas Permisif, Dengan Klausul Paten." },
  { "en": "Apa Itu Lisensi 'GNU General Public License' (GPL)?", "id": "Lisensi 'Copyleft', Yang Sangat Populer." },
  { "en": "Apa Arti 'Copyleft'?", "id": "Karya Turunan, Harus Punya Lisensi Sama." },
  { "en": "Apa Beda 'GPLv2' Dan 'GPLv3'?", "id": "GPLv3, Mengatasi Isu Paten Dan Tivoization." },
  { "en": "Apa Itu Lisensi 'LGPL'?", "id": "Lesser General Public License, Versi Lemah." },
  { "en": "Apa Itu 'Raspberry Pi Global Shutter Camera'?", "id": "Kamera Khusus, Menangkap Gambar Bergerak Cepat." },
  { "en": "Apa Itu 'Global Shutter'?", "id": "Mengekspos Seluruh Sensor, Pada Waktu Bersamaan." },
  { "en": "Apa Beda 'Global Shutter' Dan 'Rolling Shutter'?", "id": "Rolling Shutter, Memindai Sensor Secara Baris." },
  { "en": "Apa Efek Negatif Dari 'Rolling Shutter'?", "id": "Menyebabkan Distorsi, Pada Objek Bergerak Cepat." },
  { "en": "Kapan Kamera 'Global Shutter' Sangat Diperlukan?", "id": "Aplikasi Visi Mesin, Dan Fotografi Olahraga." },
  { "en": "Apa Itu 'GPIO Extender' I2C?", "id": "Chip Seperti MCP23017, Menambah Pin I/O." },
  { "en": "Apa Itu 'PWM Extender' I2C?", "id": "Chip Seperti PCA9685, Menambah Channel PWM." },
  { "en": "Kapan 'PWM Extender' Sangat Berguna?", "id": "Saat Mengontrol, Banyak Motor Servo Sekaligus." },
  { "en": "Apa Itu 'Load Average' Di Linux?", "id": "Ukuran Beban Sistem, Selama Waktu Tertentu." },
  { "en": "Bagaimana Cara Melihat 'Load Average'?", "id": "Menggunakan Perintah, 'uptime' Atau 'top'." },
  { "en": "Apa Arti Tiga Angka 'Load Average'?", "id": "Rata-rata Beban, Selama 1, 5, 15 Menit." },
  { "en": "Apa Itu 'I/O Wait'?", "id": "Waktu CPU Menganggur, Menunggu Operasi Disk." },
  { "en": "Apa Tanda 'I/O Wait' Tinggi?", "id": "Penyimpanan Lambat, Menjadi 'Bottleneck' Sistem." },
  { "en": "Apa Itu 'System Call' (Syscall)?", "id": "Cara Program, Meminta Layanan Dari Kernel." },
  { "en": "Apa Itu 'Process Context Switch'?", "id": "Menyimpan Mengembalikan, Status CPU Antar Proses." },
  { "en": "Apa Itu 'Page Fault'?", "id": "Terjadi Saat Program, Mengakses Memori Tidak." },
  { "en": "Apa Itu 'Copy-on-Write' (CoW)?", "id": "Teknik Optimasi, Menunda Penyalinan Sumber Daya." },
  { "en": "Apa Itu 'D-Bus'?", "id": "Sistem Komunikasi Antar Proses, (IPC) Modern." },
  { "en": "Apa Fungsi Dari 'D-Bus'?", "id": "Memungkinkan Aplikasi Desktop, Berkomunikasi Satu Sama." },
  { "en": "Apa Itu 'Polkit' (PolicyKit)?", "id": "Kerangka Kerja, Mengontrol Hak Akses Sistem." },
  { "en": "Apa Itu 'gettext'?", "id": "Sistem Internasionalisasi, Dan Lokalisasi GNU." },
  { "en": "Apa Itu 'Fontconfig'?", "id": "Pustaka Untuk Mengkonfigurasi, Dan Kustomisasi Font." },
  { "en": "Apa Itu 'Cairo' Graphics Library?", "id": "Pustaka Grafis 2D, Dengan Dukungan Vektor." },
  { "en": "Apa Itu 'Pango'?", "id": "Pustaka Untuk Tata Letak, Dan Rendering Teks." },
  { "en": "Apa Itu 'GTK' (GIMP Toolkit)?", "id": "Toolkit Widget Lintas Platform, Untuk GUI." },
  { "en": "Apa Itu 'Qt'?", "id": "Kerangka Kerja Aplikasi, Lintas Platform Populer." },
  { "en": "Lingkungan Desktop Mana Yang Menggunakan 'GTK'?", "id": "GNOME, XFCE, MATE, Cinnamon, Dan Lainnya." },
  { "en": "Lingkungan Desktop Mana Yang Menggunakan 'Qt'?", "id": "KDE Plasma, Dan Juga LXQt." },
  { "en": "Apa Itu 'LXDE'?", "id": "Lightweight X11 Desktop Environment, Sangat Ringan." },
  { "en": "Apa Itu 'MATE' Desktop?", "id": "Lingkungan Desktop, Lanjutan Dari GNOME 2." },
  { "en": "Apa Itu 'Cinnamon' Desktop?", "id": "Lingkungan Desktop, Yang Dikembangkan Oleh Linux Mint." },
  { "en": "Apa Itu 'Window Manager'?", "id": "Perangkat Lunak, Mengontrol Penempatan Tampilan Jendela." },
  { "en": "Apa Itu 'Compositing Window Manager'?", "id": "Manajer Jendela, Yang Menggunakan Efek Visual." },
  { "en": "Apa Contoh 'Window Manager' Tipe Tiling?", "id": "i3, Sway, Awesome WM, Dan XMonad." },
  { "en": "Apa Itu 'Display Manager'?", "id": "Program Yang Menyediakan, Layar Login Grafis." },
  { "en": "Apa Contoh 'Display Manager'?", "id": "LightDM, GDM (GNOME), SDDM (KDE)." },
  { "en": "Apa 'Display Manager' Bawaan Raspberry Pi OS?", "id": "Menggunakan 'LightDM', (Light Display Manager)." },
  { "en": "Apa Itu 'X.Org Server'?", "id": "Implementasi Populer, Dari Protokol Tampilan X11." },
  { "en": "Apa Itu 'Weston' Compositor?", "id": "Implementasi Referensi, Compositor Untuk Protokol Wayland." },
  { "en": "Apa Itu 'Sway' Compositor?", "id": "Compositor Wayland, Yang Kompatibel Dengan i3." },
  { "en": "Apa Itu 'Console' Linux?", "id": "Antarmuka Teks Murni, Tanpa Lingkungan Grafis." },
  { "en": "Bagaimana Cara Beralih Ke 'Virtual Console'?", "id": "Tekan Ctrl+Alt, Ditambah F1 Sampai F6." },
  { "en": "Bagaimana Cara Kembali Ke Lingkungan Grafis?", "id": "Tekan Ctrl+Alt, Ditambah F7 Atau F1." },
  { "en": "Apa Itu 'Bluetooth Adapter' USB?", "id": "Dongle USB, Untuk Menambah Konektivitas Bluetooth." },
  { "en": "Apa Itu 'Wi-Fi Adapter' USB?", "id": "Dongle USB, Untuk Menambah Konektivitas Wi-Fi." },
  { "en": "Apa Itu 'Serial-to-USB' Adapter?", "id": "Mengkonversi Sinyal Serial, Menjadi Antarmuka USB." },
  { "en": "Apa Kegunaan 'Serial-to-USB' Adapter?", "id": "Mengakses Konsol Serial, Dari Komputer Laptop." },
  { "en": "Apa Itu 'Heat Pipe'?", "id": "Perangkat Pasif, Transfer Panas Sangat Efisien." },
  { "en": "Apa Itu 'Vapor Chamber'?", "id": "Jenis 'Heat Pipe' Datar, Untuk Pendinginan." },
  { "en": "Apa Itu 'Thermal Paste'?", "id": "Pasta Konduktif, Mentransfer Panas Antar Permukaan." },
  { "en": "Apa Itu 'Thermal Pad'?", "id": "Bantalan Konduktif, Alternatif Dari Pasta Termal." },
  { "en": "Di Negara Mana Sebagian Besar Raspberry Pi Dibuat?", "id": "Sebagian Besar, Dibuat Di Pabrik Sony UK." },
  { "en": "Apa Itu 'Raspberry Pi Trading Ltd.'?", "id": "Cabang Komersial, Yang Menangani Rekayasa Penjualan." },
  { "en": "Apa Itu Proyek 'Astro Pi'?", "id": "Kompetisi Menjalankan Kode, Di Stasiun Luar Angkasa." },
  { "en": "Apa Itu 'ALSA Loopback Module'?", "id": "Membuat Perangkat Audio Virtual, Untuk Merutekan Suara." },
  { "en": "Apa Itu 'JACK Audio Connection Kit'?", "id": "Server Audio Profesional, Dengan Latensi Sangat Rendah." },
  { "en": "Bisakah Menjalankan Bahasa 'Lisp' Di Raspberry Pi?", "id": "Ya, Implementasi Seperti SBCL Tersedia." },
  { "en": "Bisakah Menjalankan Bahasa 'Haskell' Di Raspberry Pi?", "id": "Ya, Kompilator GHC Mendukung Arsitektur ARM." },
  { "en": "Bagaimana Cara Menguji Fungsionalitas Pin GPIO?", "id": "Menggunakan Resistor, LED, Atau Mode Loopback." },
  { "en": "Apa Itu 'E-waste' Atau Sampah Elektronik?", "id": "Barang Elektronik Bekas, Yang Sudah Dibuang." },
  { "en": "Apakah Raspberry Pi Termasuk Sumber Daya Komputasi Hijau?", "id": "Ya, Karena Konsumsi Dayanya Sangat Rendah." },
  { "en": "Apa Forum Diskusi Resmi Raspberry Pi?", "id": "Forum Online, Di Situs Web RaspberryPi.org." },
  { "en": "Apa Itu 'Stack Exchange' Raspberry Pi?", "id": "Situs Tanya Jawab, Populer Untuk Masalah Teknis." },
  { "en": "Apa Itu Lisensi 'CERN Open Hardware License'?", "id": "Lisensi Populer, Untuk Proyek Perangkat Keras." },
  { "en": "Apa Arsitektur Set Instruksi Yang Digunakan Pi?", "id": "Arsitektur ARM, (Advanced RISC Machines)." },
  { "en": "Apa Itu 'RISC'?", "id": "Reduced Instruction Set Computer, Komputasi Sederhana." },
  { "en": "Apa Beda 'RISC' Dan 'CISC'?", "id": "RISC Instruksi Sederhana, CISC Lebih Kompleks." },
  { "en": "Apa Itu 'ARM Cortex-A' Series?", "id": "Seri Prosesor ARM, Untuk Kinerja Aplikasi." },
  { "en": "Prosesor Cortex-A Apa Yang Digunakan Pi 4?", "id": "Cortex-A72, Empat Inti (Quad-core)." },
  { "en": "Apa Itu 'ARM Cortex-M' Series?", "id": "Seri Mikrokontroler ARM, Untuk Sistem Tertanam." },
  { "en": "Prosesor Cortex-M Apa Yang Digunakan Pi Pico?", "id": "Cortex-M0+, Dua Inti (Dual-core)." },
  { "en": "Apa Itu Set Instruksi 'Thumb'?", "id": "Set Instruksi 16-bit, Kompak Dari ARM." },
  { "en": "Apa Itu 'TrustZone' Teknologi ARM?", "id": "Teknologi Keamanan Perangkat Keras, Untuk Isolasi." },
  { "en": "Apa Itu 'system on a module' (SoM)?", "id": "Seperti Komputer Papan Tunggal, Tapi Terintegrasi." },
  { "en": "Apakah Compute Module Termasuk 'System on a Module'?", "id": "Ya, Itu Adalah Contoh Sempurna SoM." },
  { "en": "Apa Itu 'Single-Board Computer' (SBC)?", "id": "Komputer Lengkap, Dibangun Di Satu Papan." },
  { "en": "Apa Itu 'Ansible'?", "id": "Alat Otomatisasi IT, Untuk Manajemen Konfigurasi." },
  { "en": "Bagaimana 'Ansible' Bekerja?", "id": "Terhubung Ke Node, Melalui SSH Tanpa Agen." },
  { "en": "Apa Itu 'YAML'?", "id": "YAML Ain't Markup Language, Format Data." },
  { "en": "Apa Itu 'Ansible Playbook'?", "id": "File YAML, Yang Mendefinisikan Tugas Otomatisasi." },
  { "en": "Apa Itu 'Idempotence' Dalam Otomatisasi?", "id": "Menjalankan Operasi Berkali-kali, Hasilnya Tetap Sama." },
  { "en": "Apa Itu 'Thin Client'?", "id": "Komputer Ringan, Yang Bergantung Pada Server." },
  { "en": "Bisakah Pi Digunakan Sebagai 'Thin Client'?", "id": "Ya, Dengan Perangkat Lunak Seperti LTSP." },
  { "en": "Apa Itu 'LTSP' (Linux Terminal Server Project)?", "id": "Solusi Membuat 'Thin Client', Dari Jaringan." },
  { "en": "Apa Itu 'Disk Quota'?", "id": "Batas Penggunaan Ruang Disk, Untuk Pengguna." },
  { "en": "Apa Itu 'inotify'?", "id": "Subsistem Kernel Linux, Untuk Memantau Perubahan." },
  { "en": "Apa Itu 'S.M.A.R.T.' (Self-Monitoring, Analysis, and Reporting)?", "id": "Sistem Pemantauan, Untuk Drive Penyimpanan." },
  { "en": "Apa Itu 'smartmontools'?", "id": "Utilitas Mengontrol, Dan Memantau Data SMART." },
  { "en": "Apa Itu 'hdparm'?", "id": "Utilitas Baris Perintah, Mengatur Parameter Drive." },
  { "en": "Apa Itu 'sdparm'?", "id": "Mirip 'hdparm', Tapi Untuk Perangkat SCSI." },
  { "en": "Apa Itu 'lsblk' Command?", "id": "Menampilkan Informasi, Tentang Semua Perangkat Blok." },
  { "en": "Apa Itu 'lscpu' Command?", "id": "Menampilkan Informasi, Tentang Arsitektur CPU." },
  { "en": "Apa Itu 'lshw' Command?", "id": "List Hardware, Menampilkan Informasi Perangkat Keras." },
  { "en": "Apa Itu 'lspci' Command?", "id": "Menampilkan Informasi, Tentang Semua Bus PCI." },
  { "en": "Apa Itu 'lsusb' Command?", "id": "Menampilkan Informasi, Tentang Semua Bus USB." },
  { "en": "Apa Itu 'dmidecode'?", "id": "Membaca Data Perangkat Keras, Dari Tabel DMI." },
  { "en": "Apa Itu '/proc' Filesystem?", "id": "Filesystem Virtual, Memberikan Informasi Proses Kernel." },
  { "en": "Apa Itu '/sys' Filesystem?", "id": "Filesystem Virtual, Mengekspos Informasi Perangkat Kernel." },
  { "en": "Apa Itu 'sysctl'?", "id": "Alat Mengubah, Parameter Kernel Saat Runtime." },
  { "en": "Apa Itu 'kernel panic'?", "id": "Kesalahan Fatal Internal, Yang Dideteksi Kernel." },
  { "en": "Apa Penyebab Umum 'kernel panic'?", "id": "Kesalahan Perangkat Keras, Atau Bug Perangkat Lunak." },
  { "en": "Apa Itu 'watch' Command?", "id": "Menjalankan Perintah Lain, Secara Berkala Terus-menerus." },
  { "en": "Apa Itu 'ncat' Atau 'netcat'?", "id": "Utilitas Jaringan, Untuk Membaca Menulis Data." },
  { "en": "Apa Itu 'socat'?", "id": "Utilitas Jaringan Serbaguna, Seperti Netcat Lanjutan." },
  { "en": "Apa Itu 'nethogs'?", "id": "Memantau Penggunaan Bandwidth, Jaringan Per Proses." },
  { "en": "Apa Itu 'iftop'?", "id": "Menampilkan Penggunaan Bandwidth, Pada Sebuah Antarmuka." },
  { "en": "Apa Itu 'iptraf'?", "id": "Monitor Jaringan, Statistik Real-time Di Konsol." },
  { "en": "Apa Itu 'traceroute'?", "id": "Melacak Rute Paket, Dari Komputer Ke Tujuan." },
  { "en": "Apa Itu 'mtr' (My Traceroute)?", "id": "Menggabungkan Fungsionalitas, 'traceroute' Dan 'ping'." },
  { "en": "Apa Itu 'dig' Command?", "id": "Alat Kueri, Untuk Server Nama Domain (DNS)." },
  { "en": "Apa Itu 'host' Command?", "id": "Utilitas Sederhana, Untuk Melakukan Pencarian DNS." },
  { "en": "Apa Itu 'nslookup' Command?", "id": "Program Kueri, Server Nama Internet Interaktif." },
  { "en": "Apa Itu 'whois' Command?", "id": "Mencari Informasi, Pendaftaran Nama Domain." },
  { "en": "Apa Itu 'scp' (Secure Copy)?", "id": "Menyalin File, Dengan Aman Antar Host Jaringan." },
  { "en": "Apa Itu 'sftp' (SSH File Transfer Protocol)?", "id": "Protokol Transfer File, Aman Di Atas SSH." },
  { "en": "Apa Beda 'FTP' Dan 'SFTP'?", "id": "SFTP, Sepenuhnya Dienkripsi Dan Jauh Aman." },
  { "en": "Apa Itu 'Trivial File Transfer Protocol' (TFTP)?", "id": "Protokol Transfer File, Sangat Sederhana." },
  { "en": "Kapan 'TFTP' Biasanya Digunakan?", "id": "Untuk Boot Jaringan, Atau Memperbarui Firmware." },
  { "en": "Apa Itu 'ISCSI' (Internet Small Computer System Interface)?", "id": "Protokol Penyimpanan Jaringan, Berbasis Blok." },
  { "en": "Apa Itu 'Ceph'?", "id": "Platform Penyimpanan Terdistribusi, Skala Besar." },
  { "en": "Apa Itu 'GlusterFS'?", "id": "Filesystem Jaringan Terdistribusi, Yang Dapat Diskalakan." },
  { "en": "Apa Itu 'Load Balancer'?", "id": "Mendistribusikan Lalu Lintas, Ke Beberapa Server." },
  { "en": "Apa Itu 'HAProxy'?", "id": "Perangkat Lunak 'Load Balancer', Sumber Terbuka." },
  { "en": "Apa Itu 'Keepalived'?", "id": "Menyediakan Ketersediaan Tinggi, Dengan Failover Otomatis." },
  { "en": "Apa Itu 'Floating IP'?", "id": "Alamat IP, Yang Dapat Dipindahkan Antar Server." },
  { "en": "Apa Itu 'System Tap'?", "id": "Skrip Pelacakan Dinamis, Untuk Kernel Linux." },
  { "en": "Apa Itu 'eBPF' (Extended Berkeley Packet Filter)?", "id": "Teknologi Kernel, Menjalankan Program Sandbox Aman." },
  { "en": "Apa Kegunaan 'eBPF'?", "id": "Jaringan, Keamanan, Pelacakan, Dan Pemantauan." },
  { "en": "Apa Itu 'K-Probes' Dan 'U-Probes'?", "id": "Mekanisme Debugging, Untuk Kernel Dan User-space." },
  { "en": "Apa Itu 'Perf' Tool?", "id": "Alat Analisis Kinerja, Bawaan Di Linux." },
  { "en": "Apa Itu 'Flame Graph'?", "id": "Visualisasi Profil Perangkat Lunak, Yang Interaktif." },
  { "en": "Apa Itu 'OpenOCD'?", "id": "On-Chip Debugger, Terbuka Untuk Debugging JTAG." },
  { "en": "Apa Itu 'Segger J-Link'?", "id": "Probe Debug JTAG/SWD, Yang Sangat Populer." },
  { "en": "Apa Itu 'USBasp'?", "id": "Programmer USB Dalam Sirkuit, Untuk Mikrokontroler." },
  { "en": "Apa Itu 'AVRDUDE'?", "id": "Perangkat Lunak Mengunduh, Dan Mengunggah Mikrokontroler." },
  { "en": "Apa Itu 'Serial Wire Debug' (SWD)?", "id": "Antarmuka Debug Dua Kabel, Alternatif JTAG." },
  { "en": "Apa Itu 'Break Point' Dalam Debugging?", "id": "Titik Jeda Intensional, Dalam Sebuah Program." },
  { "en": "Apa Itu 'Watch Point'?", "id": "Titik Henti Bersyarat, Pada Lokasi Memori." },
  { "en": "Apa Itu 'Stack Trace'?", "id": "Laporan Panggilan Fungsi, Saat Titik Tertentu." },
  { "en": "Apa Itu 'Assertion'?", "id": "Pernyataan Predikat, Yang Seharusnya Selalu Benar." },
  { "en": "Apa Itu 'Design for Testability' (DFT)?", "id": "Teknik Desain, Memudahkan Pengujian Perangkat Keras." },
  { "en": "Apa Itu 'Boundary Scan'?", "id": "Metode Pengujian, Interkoneksi Pada Papan PCB." },
  { "en": "Apa Itu 'Automatic Test Equipment' (ATE)?", "id": "Peralatan Otomatis, Untuk Menguji Perangkat Elektronik." },
  { "en": "Apa Itu 'In-Circuit Test' (ICT)?", "id": "Pengujian Yang Memeriksa, Setiap Komponen Individual." },
  { "en": "Apa Itu 'Functional Test' (FCT)?", "id": "Menguji Fungsionalitas, Perangkat Sesuai Spesifikasi." },
  { "en": "Apa Perbedaan Antara 'Verification' Dan 'Validation'?", "id": "Verifikasi Benar Dibuat, Validasi Produk Benar." }



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
