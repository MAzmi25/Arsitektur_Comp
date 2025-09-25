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


  { "en": "Apa Itu Arsitektur Komputer?", "id": "Konsep Perencanaan Dan Struktur Operasional Komputer." },
  { "en": "Apa Dua Komponen Utama Komputer?", "id": "Perangkat Keras (Hardware) Dan Perangkat Lunak (Software)." },
  { "en": "Apa Itu Central Processing Unit (CPU)?", "id": "Otak Dari Komputer." },
  { "en": "Apa Dua Bagian Utama CPU (Central Processing Unit)?", "id": "Control Unit Dan Arithmetic Logic Unit." },
  { "en": "Apa Fungsi Control Unit (CU)?", "id": "Mengarahkan Operasi Prosesor." },
  { "en": "Apa Fungsi Arithmetic Logic Unit (ALU)?", "id": "Melakukan Operasi Aritmetika Dan Logika." },
  { "en": "Apa Itu Memori?", "id": "Perangkat Untuk Menyimpan Data Dan Instruksi." },
  { "en": "Apa Itu Random-Access Memory (RAM)?", "id": "Memori Utama Yang Volatile." },
  { "en": "Apa Arti Dari Volatile?", "id": "Data Hilang Saat Daya Dimatikan." },
  { "en": "Apa Itu Read-Only Memory (ROM)?", "id": "Memori Non-Volatile Yang Hanya Dapat Dibaca." },
  { "en": "Apa Itu Input/Output (I/O) Device?", "id": "Perangkat Untuk Komunikasi Dengan Dunia Luar." },
  { "en": "Sebutkan Contoh Perangkat Input?", "id": "Keyboard, Mouse, Dan Mikrofon." },
  { "en": "Sebutkan Contoh Perangkat Output?", "id": "Monitor, Printer, Dan Speaker." },
  { "en": "Apa Itu Bus Sistem?", "id": "Jalur Komunikasi Yang Menghubungkan Komponen." },
  { "en": "Sebutkan Tiga Jenis Bus Sistem?", "id": "Bus Alamat, Bus Data, Bus Kontrol." },
  { "en": "Apa Fungsi Bus Alamat?", "id": "Membawa Alamat Lokasi Memori." },
  { "en": "Apa Fungsi Bus Data?", "id": "Membawa Data Antara CPU Dan Memori." },
  { "en": "Apa Fungsi Bus Kontrol?", "id": "Membawa Sinyal Kontrol Dan Waktu." },
  { "en": "Apa Itu Arsitektur Von Neumann?", "id": "Menyimpan Instruksi Dan Data Di Memori Sama." },
  { "en": "Apa Itu Arsitektur Harvard?", "id": "Memisahkan Memori Untuk Instruksi Dan Data." },
  { "en": "Apa Keuntungan Arsitektur Harvard?", "id": "Dapat Mengambil Instruksi Dan Data Bersamaan." },
  { "en": "Apa Itu Siklus Instruksi?", "id": "Proses Pengambilan Dan Eksekusi Instruksi." },
  { "en": "Apa Tahap Pengambilan (Fetch)?", "id": "Mengambil Instruksi Dari Memori." },
  { "en": "Apa Tahap Dekode (Decode)?", "id": "Menerjemahkan Instruksi Menjadi Perintah." },
  { "en": "Apa Tahap Eksekusi (Execute)?", "id": "Menjalankan Perintah Yang Didekode." },
  { "en": "Apa Itu Register?", "id": "Lokasi Penyimpanan Kecil Dan Cepat Di CPU." },
  { "en": "Apa Itu Program Counter (PC)?", "id": "Menyimpan Alamat Instruksi Berikutnya." },
  { "en": "Apa Itu Instruction Register (IR)?", "id": "Menyimpan Instruksi Yang Sedang Dieksekusi." },
  { "en": "Apa Itu Accumulator?", "id": "Register Untuk Menyimpan Hasil Operasi ALU." },
  { "en": "Apa Itu Memory Address Register (MAR)?", "id": "Menyimpan Alamat Memori Yang Akan Diakses." },
  { "en": "Apa Itu Memory Data Register (MDR)?", "id": "Menyimpan Data Yang Dibaca Atau Ditulis." },
  { "en": "Apa Itu Sinyal Clock?", "id": "Sinyal Periodik Untuk Sinkronisasi Operasi." },
  { "en": "Apa Satuan Frekuensi Clock?", "id": "Hertz (Hz)." },
  { "en": "Apa Itu Kinerja CPU (Central Processing Unit)?", "id": "Ukuran Seberapa Cepat CPU Bekerja." },
  { "en": "Apa Itu MIPS (Million Instructions Per Second)?", "id": "Ukuran Kinerja Prosesor." },
  { "en": "Apa Itu FLOPS (Floating Point Operations Per Second)?", "id": "Ukuran Kinerja Untuk Operasi Floating-Point." },
  { "en": "Apa Itu Pipelining?", "id": "Teknik Mengeksekusi Beberapa Instruksi Secara Bersamaan." },
  { "en": "Apa Tujuan Pipelining?", "id": "Meningkatkan Throughput Instruksi." },
  { "en": "Apa Itu Throughput?", "id": "Jumlah Tugas Yang Diselesaikan Per Satuan Waktu." },
  { "en": "Apa Itu Pipeline Hazard?", "id": "Kondisi Yang Mencegah Instruksi Berikutnya." },
  { "en": "Sebutkan Tiga Jenis Hazard?", "id": "Struktural, Data, Dan Kontrol." },
  { "en": "Apa Itu Hazard Data?", "id": "Instruksi Bergantung Pada Hasil Instruksi Sebelumnya." },
  { "en": "Apa Itu Hazard Kontrol?", "id": "Terjadi Akibat Instruksi Percabangan." },
  { "en": "Apa Itu Set Instruksi (Instruction Set)?", "id": "Kumpulan Semua Instruksi Yang Dipahami CPU." },
  { "en": "Apa Itu Reduced Instruction Set Computer (RISC)?", "id": "Arsitektur Dengan Set Instruksi Sederhana." },
  { "en": "Apa Ciri Arsitektur RISC (Reduced Instruction Set Computer)?", "id": "Instruksi Sederhana, Ukuran Tetap, Pipelining Efisien." },
  { "en": "Apa Itu Complex Instruction Set Computer (CISC)?", "id": "Arsitektur Dengan Set Instruksi Kompleks." },
  { "en": "Apa Ciri Arsitektur CISC (Complex Instruction Set Computer)?", "id": "Instruksi Kompleks, Ukuran Bervariasi." },
  { "en": "Contoh Prosesor RISC (Reduced Instruction Set Computer) Adalah Apa?", "id": "ARM, MIPS, Dan PowerPC." },
  { "en": "Contoh Prosesor CISC (Complex Instruction Set Computer) Adalah Apa?", "id": "Intel x86 Dan AMD x86-64." },
  { "en": "Apa Itu Memori Cache?", "id": "Memori Kecil Dan Cepat Antara CPU Dan RAM." },
  { "en": "Apa Tujuan Memori Cache?", "id": "Mengurangi Waktu Akses Memori Rata-rata." },
  { "en": "Apa Itu Cache Hit?", "id": "Data Yang Dicari Ditemukan Di Cache." },
  { "en": "Apa Itu Cache Miss?", "id": "Data Yang Dicari Tidak Ditemukan." },
  { "en": "Apa Itu Hit Rate?", "id": "Persentase Hit Dari Total Akses." },
  { "en": "Apa Itu Prinsip Lokalitas (Locality)?", "id": "Kecenderungan Mengakses Data Dekat Satu Sama Lain." },
  { "en": "Apa Itu Lokalitas Temporal?", "id": "Mengakses Kembali Item Yang Sama." },
  { "en": "Apa Itu Lokalitas Spasial?", "id": "Mengakses Item Yang Berdekatan." },
  { "en": "Apa Itu Level Cache?", "id": "Hierarki Cache (L1, L2, L3)." },
  { "en": "Cache Mana Yang Paling Cepat?", "id": "Cache Level 1 (L1)." },
  { "en": "Apa Itu Kebijakan Penulisan (Write Policy)?", "id": "Cara Cache Menangani Operasi Tulis." },
  { "en": "Apa Itu Write-Through?", "id": "Menulis Ke Cache Dan Memori Utama." },
  { "en": "Apa Itu Write-Back?", "id": "Menulis Hanya Ke Cache." },
  { "en": "Apa Itu Dirty Bit?", "id": "Menandai Blok Cache Yang Telah Dimodifikasi." },
  { "en": "Apa Itu Pemetaan Cache (Cache Mapping)?", "id": "Cara Blok Memori Dipetakan Ke Cache." },
  { "en": "Apa Itu Pemetaan Langsung (Direct Mapping)?", "id": "Setiap Blok Memori Hanya Dapat Ke Satu Lokasi." },
  { "en": "Apa Itu Pemetaan Asosiatif Penuh?", "id": "Blok Memori Dapat Ke Lokasi Mana Saja." },
  { "en": "Apa Itu Pemetaan Set-Asosiatif?", "id": "Kompromi Antara Pemetaan Langsung Dan Asosiatif." },
  { "en": "Apa Itu Kebijakan Penggantian (Replacement Policy)?", "id": "Menentukan Blok Mana Yang Akan Dikeluarkan." },
  { "en": "Apa Itu Least Recently Used (LRU)?", "id": "Mengganti Blok Yang Paling Lama Tidak Digunakan." },
  { "en": "Apa Itu First-In, First-Out (FIFO)?", "id": "Mengganti Blok Yang Pertama Kali Masuk." },
  { "en": "Apa Itu Memori Virtual?", "id": "Teknik Menggunakan Disk Sebagai RAM." },
  { "en": "Apa Tujuan Memori Virtual?", "id": "Memungkinkan Ruang Alamat Lebih Besar Dari RAM." },
  { "en": "Apa Itu Paging?", "id": "Membagi Memori Menjadi Blok Berukuran Tetap." },
  { "en": "Apa Itu Page?", "id": "Blok Memori Virtual." },
  { "en": "Apa Itu Frame?", "id": "Blok Memori Fisik." },
  { "en": "Apa Itu Page Table?", "id": "Tabel Yang Memetakan Page Ke Frame." },
  { "en": "Apa Itu Page Fault?", "id": "Terjadi Saat Page Tidak Ada Di Memori." },
  { "en": "Apa Itu Memory Management Unit (MMU)?", "id": "Perangkat Keras Untuk Mengelola Memori Virtual." },
  { "en": "Apa Fungsi MMU (Memory Management Unit)?", "id": "Menerjemahkan Alamat Virtual Ke Alamat Fisik." },
  { "en": "Apa Itu Translation Lookaside Buffer (TLB)?", "id": "Cache Untuk Terjemahan Alamat." },
  { "en": "Apa Itu Firmware?", "id": "Perangkat Lunak Permanen Di Perangkat Keras." },
  { "en": "Apa Itu BIOS (Basic Input/Output System)?", "id": "Firmware Untuk Proses Booting Komputer." },
  { "en": "Apa Itu UEFI (Unified Extensible Firmware Interface)?", "id": "Penerus Modern Dari BIOS." },
  { "en": "Apa Itu Proses Booting?", "id": "Proses Memulai Sistem Operasi Komputer." },
  { "en": "Apa Itu Interupsi?", "id": "Sinyal Ke CPU Dari Perangkat Keras." },
  { "en": "Apa Itu Interrupt Handler?", "id": "Kode Yang Dijalankan Saat Interupsi Terjadi." },
  { "en": "Apa Itu Vektor Interupsi?", "id": "Tabel Yang Menunjuk Ke Interrupt Handler." },
  { "en": "Apa Itu Direct Memory Access (DMA)?", "id": "Perangkat I/O Mengakses Memori Langsung." },
  { "en": "Apa Keuntungan DMA (Direct Memory Access)?", "id": "Mengurangi Beban CPU Selama Transfer I/O." },
  { "en": "Apa Itu Bus Mastering?", "id": "Fitur Yang Memungkinkan Perangkat Mengontrol Bus." },
  { "en": "Apa Itu Sistem Operasi?", "id": "Perangkat Lunak Yang Mengelola Sumber Daya." },
  { "en": "Apa Itu Kernel?", "id": "Inti Dari Sistem Operasi." },
  { "en": "Apa Itu Multitasking?", "id": "Menjalankan Beberapa Tugas Secara Bersamaan." },
  { "en": "Apa Itu Mode Pengalamatan (Addressing Mode)?", "id": "Cara CPU (Central Processing Unit) Menentukan Alamat Operand." },
  { "en": "Apa Itu Pengalamatan Langsung (Immediate)?", "id": "Operand Adalah Bagian Dari Instruksi." },
  { "en": "Apa Itu Pengalamatan Register?", "id": "Operand Berada Di Dalam Register." },
  { "en": "Apa Itu Pengalamatan Absolut (Direct)?", "id": "Instruksi Berisi Alamat Memori." },
  { "en": "Apa Itu Pengalamatan Tidak Langsung (Indirect)?", "id": "Instruksi Menunjuk Ke Alamat Memori." },
  { "en": "Apa Itu Pengalamatan Terindeks?", "id": "Alamat Efektif Adalah Jumlah Register." },
  { "en": "Apa Itu Opcode (Operation Code)?", "id": "Bagian Instruksi Yang Menspesifikasikan Operasi." },
  { "en": "Apa Itu Operand?", "id": "Data Yang Dioperasikan Oleh Instruksi." },
  { "en": "Apa Itu Representasi Biner?", "id": "Sistem Bilangan Berbasis Dua." },
  { "en": "Apa Itu Heksadesimal?", "id": "Sistem Bilangan Berbasis Enam Belas." },
  { "en": "Apa Itu Dua Komplemen (Two's Complement)?", "id": "Metode Standar Representasi Bilangan Bertanda." },
  { "en": "Apa Itu Bilangan Floating-Point?", "id": "Representasi Untuk Bilangan Real." },
  { "en": "Apa Standar Untuk Bilangan Floating-Point?", "id": "IEEE (Institute of Electrical and Electronics Engineers) 754." },
  { "en": "Apa Tiga Bagian Bilangan Floating-Point?", "id": "Tanda (Sign), Eksponen (Exponent), Mantisa." },
  { "en": "Apa Itu Normalisasi?", "id": "Menyesuaikan Bilangan Floating-Point Ke Format." },
  { "en": "Apa Itu Arsitektur Superskalar?", "id": "Mengeksekusi Lebih Dari Satu Instruksi Per Siklus." },
  { "en": "Apa Itu Instruction-Level Parallelism (ILP)?", "id": "Paralelisme Pada Tingkat Instruksi." },
  { "en": "Apa Itu Tahapan Pipa (Pipeline Stages)?", "id": "Langkah-langkah Dalam Eksekusi Instruksi." },
  { "en": "Sebutkan Tahapan Pipa Klasik?", "id": "Fetch, Decode, Execute, Memory, Write-back." },
  { "en": "Apa Itu Register Pipa?", "id": "Menyimpan Data Antara Tahapan Pipa." },
  { "en": "Apa Itu Data Forwarding (Bypassing)?", "id": "Meneruskan Hasil ALU Langsung Ke Input." },
  { "en": "Apa Tujuan Data Forwarding?", "id": "Untuk Mengurangi Stall Akibat Hazard Data." },
  { "en": "Apa Itu Stall (Bubble)?", "id": "Penundaan Dalam Pipa." },
  { "en": "Apa Itu Prediksi Percabangan (Branch Prediction)?", "id": "Menebak Arah Percabangan." },
  { "en": "Apa Itu Prediksi Statis?", "id": "Prediksi Selalu Sama Setiap Waktu." },
  { "en": "Apa Itu Prediksi Dinamis?", "id": "Prediksi Berdasarkan Sejarah Eksekusi." },
  { "en": "Apa Itu Branch Target Buffer (BTB)?", "id": "Cache Untuk Menyimpan Alamat Tujuan." },
  { "en": "Apa Itu Eksekusi Spekulatif?", "id": "Mengeksekusi Instruksi Sebelum Arah Diketahui." },
  { "en": "Apa Itu Very Long Instruction Word (VLIW)?", "id": "Arsitektur Yang Mengemas Beberapa Operasi." },
  { "en": "Apa Itu Koherensi Cache?", "id": "Memastikan Konsistensi Data Di Banyak Cache." },
  { "en": "Apa Masalah Koherensi Cache?", "id": "Data Menjadi Tidak Konsisten." },
  { "en": "Apa Itu Protokol Snooping?", "id": "Cache Memantau Bus Untuk Operasi Tulis." },
  { "en": "Apa Itu Protokol Berbasis Direktori?", "id": "Direktori Terpusat Melacak Status Blok." },
  { "en": "Apa Kepanjangan Dari MESI Protocol?", "id": "Modified, Exclusive, Shared, Invalid." },
  { "en": "Apa Itu Write Invalidate Protocol?", "id": "Menghapus Salinan Cache Lain Saat Menulis." },
  { "en": "Apa Itu Write Update Protocol?", "id": "Memperbarui Semua Salinan Cache Saat Menulis." },
  { "en": "Apa Itu False Sharing?", "id": "Dua Prosesor Menggunakan Data Berbeda." },
  { "en": "Apa Itu Blok Cache (Cache Line)?", "id": "Unit Transfer Antara Cache Dan Memori." },
  { "en": "Apa Itu Ukuran Blok?", "id": "Jumlah Byte Dalam Satu Blok Cache." },
  { "en": "Apa Itu Tag Cache?", "id": "Menyimpan Alamat Untuk Identifikasi Blok." },
  { "en": "Apa Itu Valid Bit?", "id": "Menandai Apakah Entri Cache Valid." },
  { "en": "Apa Itu Memori Utama?", "id": "Nama Lain Untuk RAM (Random-Access Memory)." },
  { "en": "Apa Itu Dynamic RAM (DRAM)?", "id": "Jenis RAM (Random-Access Memory) Paling Umum." },
  { "en": "Apa Itu Static RAM (SRAM)?", "id": "Jenis RAM (Random-Access Memory) Yang Lebih Cepat." },
  { "en": "Di Mana SRAM (Static RAM) Biasanya Digunakan?", "id": "Untuk Membangun Memori Cache." },
  { "en": "Mengapa DRAM (Dynamic RAM) Perlu Disegarkan (Refresh)?", "id": "Karena Muatan Kapasitor Bocor Seiring Waktu." },
  { "en": "Apa Itu Hirarki Memori?", "id": "Pengorganisasian Memori Berdasarkan Kecepatan." },
  { "en": "Apa Tingkat Teratas Hirarki Memori?", "id": "Register CPU (Central Processing Unit)." },
  { "en": "Apa Tingkat Terbawah Hirarki Memori?", "id": "Penyimpanan Sekunder Seperti Hard Disk." },
  { "en": "Apa Itu Perangkat I/O (Input/Output)?", "id": "Perangkat Keras Untuk Input Dan Output." },
  { "en": "Apa Itu Programmed I/O (PIO)?", "id": "CPU (Central Processing Unit) Secara Aktif Terlibat." },
  { "en": "Apa Itu Polling?", "id": "CPU (Central Processing Unit) Terus Memeriksa Status." },
  { "en": "Apa Itu Interrupt-driven I/O?", "id": "Perangkat Memberi Tahu CPU (Central Processing Unit) Saat Siap." },
  { "en": "Mana Yang Lebih Efisien: Polling Atau Interupsi?", "id": "Interrupt-driven I/O Jauh Lebih Efisien." },
  { "en": "Apa Itu I/O Processor (IOP)?", "id": "Prosesor Khusus Untuk Menangani I/O." },
  { "en": "Apa Itu Channel?", "id": "Nama Lain Untuk I/O (Input/Output) Processor." },
  { "en": "Apa Itu Memory-Mapped I/O?", "id": "Perangkat I/O Berbagi Ruang Alamat." },
  { "en": "Apa Itu Isolated I/O?", "id": "Perangkat I/O Memiliki Ruang Alamat." },
  { "en": "Apa Itu Bus Arbitration?", "id": "Proses Memutuskan Perangkat Mana Yang Menggunakan Bus." },
  { "en": "Apa Itu Arbitrasi Terpusat?", "id": "Satu Arbiter Mengontrol Akses Bus." },
  { "en": "Apa Itu Arbitrasi Terdistribusi?", "id": "Setiap Perangkat Berpartisipasi Dalam Arbitrasi." },
  { "en": "Apa Itu Daisy Chaining?", "id": "Metode Arbitrasi Terpusat." },
  { "en": "Apa Itu Bus Sinkron?", "id": "Transfer Data Disinkronkan Dengan Sinyal Clock." },
  { "en": "Apa Itu Bus Asinkron?", "id": "Transfer Data Menggunakan Sinyal Handshaking." },
  { "en": "Apa Itu Handshaking?", "id": "Pertukaran Sinyal Kontrol (Request/Acknowledge)." },
  { "en": "Apa Itu Penyimpanan Sekunder?", "id": "Penyimpanan Non-Volatile Jangka Panjang." },
  { "en": "Apa Itu Hard Disk Drive (HDD)?", "id": "Penyimpanan Magnetik Dengan Piringan Berputar." },
  { "en": "Apa Itu Platter?", "id": "Piringan Logam Tempat Data Disimpan." },
  { "en": "Apa Itu Track?", "id": "Lingkaran Konsentris Di Atas Platter." },
  { "en": "Apa Itu Sector?", "id": "Bagian Dari Track." },
  { "en": "Apa Itu Seek Time?", "id": "Waktu Untuk Memindahkan Kepala Baca/Tulis." },
  { "en": "Apa Itu Rotational Latency?", "id": "Waktu Untuk Sektor Berputar." },
  { "en": "Apa Itu Transfer Time?", "id": "Waktu Untuk Mentransfer Data." },
  { "en": "Apa Itu Solid-State Drive (SSD)?", "id": "Penyimpanan Berbasis Memori Flash." },
  { "en": "Apa Keuntungan SSD (Solid-State Drive) Dibanding HDD (Hard Disk Drive)?", "id": "Lebih Cepat, Tahan Guncangan, Hening." },
  { "en": "Apa Itu RAID (Redundant Array of Independent Disks)?", "id": "Menggabungkan Beberapa Disk Untuk Kinerja." },
  { "en": "Apa Itu RAID 0 (Striping)?", "id": "Meningkatkan Kinerja Tanpa Redundansi." },
  { "en": "Apa Itu RAID 1 (Mirroring)?", "id": "Menyalin Data Ke Dua Disk." },
  { "en": "Apa Itu RAID 5?", "id": "Striping Dengan Paritas Terdistribusi." },
  { "en": "Apa Itu RAID 10 (1+0)?", "id": "Menggabungkan Mirroring Dan Striping." },
  { "en": "Apa Itu Taksonomi Flynn?", "id": "Klasifikasi Arsitektur Komputer Paralel." },
  { "en": "Apa Itu SISD (Single Instruction, Single Data)?", "id": "Komputer Serial Tradisional." },
  { "en": "Apa Itu SIMD (Single Instruction, Multiple Data)?", "id": "Mengeksekusi Instruksi Sama Pada Data Berbeda." },
  { "en": "Contoh SIMD (Single Instruction, Multiple Data) Adalah Apa?", "id": "Prosesor Vektor, Ekstensi MMX/SSE." },
  { "en": "Apa Itu MISD (Multiple Instruction, Single Data)?", "id": "Arsitektur Yang Jarang Digunakan." },
  { "en": "Apa Itu MIMD (Multiple Instruction, Multiple Data)?", "id": "Komputer Paralel Paling Umum." },
  { "en": "Contoh MIMD (Multiple Instruction, Multiple Data) Adalah Apa?", "id": "Prosesor Multi-core." },
  { "en": "Apa Itu Prosesor Multi-core?", "id": "Satu Chip Dengan Dua Atau Lebih CPU." },
  { "en": "Apa Itu Thread?", "id": "Unit Eksekusi Terkecil." },
  { "en": "Apa Itu Multithreading?", "id": "Memungkinkan Satu Core Mengeksekusi Banyak Thread." },
  { "en": "Apa Itu Hukum Amdahl?", "id": "Menjelaskan Batas Peningkatan Kinerja." },
  { "en": "Apa Itu Komputer Paralel?", "id": "Komputer Dengan Banyak Prosesor." },
  { "en": "Apa Itu Jaringan Interkoneksi?", "id": "Menghubungkan Prosesor Dalam Komputer Paralel." },
  { "en": "Apa Itu Shared Memory System?", "id": "Semua Prosesor Berbagi Ruang Alamat Sama." },
  { "en": "Apa Itu Distributed Memory System?", "id": "Setiap Prosesor Memiliki Memori Pribadi." },
  { "en": "Apa Itu Message Passing?", "id": "Komunikasi Dalam Sistem Memori Terdistribusi." },
  { "en": "Apa Itu Komputer Kuantum?", "id": "Komputer Berbasis Prinsip Mekanika Kuantum." },
  { "en": "Apa Itu Qubit?", "id": "Unit Dasar Informasi Kuantum." },
  { "en": "Apa Itu Superposisi?", "id": "Qubit Dapat Menjadi 0 Dan 1 Bersamaan." },
  { "en": "Apa Itu Entanglement?", "id": "Keterkaitan Kuantum Antara Dua Qubit." },
  { "en": "Apa Itu Komputer Optik?", "id": "Menggunakan Foton Cahaya Untuk Komputasi." },
  { "en": "Apa Itu Komputer DNA (Deoxyribonucleic Acid)?", "id": "Menggunakan Biokimia DNA Untuk Komputasi." },
  { "en": "Apa Itu Arsitektur Dataflow?", "id": "Eksekusi Digerakkan Oleh Ketersediaan Data." },
  { "en": "Apa Itu Word Size?", "id": "Jumlah Bit Yang Diproses CPU Sekaligus." },
  { "en": "Apa Itu Arsitektur 32-bit?", "id": "Memproses Data Dalam Potongan 32-bit." },
  { "en": "Apa Itu Arsitektur 64-bit?", "id": "Memproses Data Dalam Potongan 64-bit." },
  { "en": "Apa Itu Backward Compatibility?", "id": "Perangkat Keras Baru Mendukung Perangkat Lunak Lama." },
  { "en": "Apa Itu Set Instruksi (Instruction Set)?", "id": "Kumpulan Perintah Yang Dipahami Prosesor." },
  { "en": "Apa Itu Mikroarsitektur?", "id": "Implementasi Detail Dari Set Instruksi." },
  { "en": "Apa Itu Kode Mesin?", "id": "Bahasa Biner Level Terendah." },
  { "en": "Apa Itu Bahasa Assembly?", "id": "Representasi Simbolik Dari Kode Mesin." },
  { "en": "Apa Itu Assembler?", "id": "Menerjemahkan Bahasa Assembly Ke Kode Mesin." },
  { "en": "Apa Itu Kompiler?", "id": "Menerjemahkan Bahasa Tingkat Tinggi Ke Assembly." },
  { "en": "Apa Itu Interpreter?", "id": "Mengeksekusi Kode Tingkat Tinggi Baris Demi Baris." },
  { "en": "Apa Itu Linker?", "id": "Menggabungkan File Objek Menjadi Satu Program." },
  { "en": "Apa Itu Loader?", "id": "Memuat Program Dari Disk Ke Memori." },
  { "en": "Apa Itu Sistem Embedded?", "id": "Sistem Komputer Dengan Fungsi Khusus." },
  { "en": "Contoh Sistem Embedded Adalah Apa?", "id": "Mikrokontroler Dalam Microwave." },
  { "en": "Apa Itu Mikrokontroler?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Itu Digital Signal Processor (DSP)?", "id": "Mikroprosesor Khusus Untuk Pemrosesan Sinyal." },
  { "en": "Apa Itu Multiply-Accumulate (MAC) Instruction?", "id": "Operasi Umum Dalam DSP (Digital Signal Processor)." },
  { "en": "Apa Itu System-on-a-Chip (SoC)?", "id": "Seluruh Sistem Elektronik Dalam Satu Chip." },
  { "en": "Apa Itu Application-Specific Integrated Circuit (ASIC)?", "id": "Chip Yang Didesain Untuk Tugas Tertentu." },
  { "en": "Apa Itu Field-Programmable Gate Array (FPGA)?", "id": "Chip Dengan Gerbang Logika Yang Dapat Diprogram." },
  { "en": "Apa Perbedaan ASIC (Application-Specific Integrated Circuit) Dan FPGA (Field-Programmable Gate Array)?", "id": "ASIC Tetap, FPGA Fleksibel." },
  { "en": "Apa Itu Look-Up Table (LUT)?", "id": "Blok Logika Dasar Dalam FPGA." },
  { "en": "Apa Itu Konfigurasi Bitstream?", "id": "File Untuk Memprogram FPGA (Field-Programmable Gate Array)." },
  { "en": "Apa Itu Kinerja Per Watt?", "id": "Ukuran Efisiensi Energi Komputer." },
  { "en": "Apa Itu Power Gating?", "id": "Mematikan Daya Ke Bagian Chip." },
  { "en": "Apa Itu Clock Gating?", "id": "Menghentikan Sinyal Clock Ke Bagian Chip." },
  { "en": "Apa Itu Dynamic Voltage and Frequency Scaling (DVFS)?", "id": "Menyesuaikan Tegangan Dan Frekuensi." },
  { "en": "Apa Itu Mode Tidur (Sleep Mode)?", "id": "Keadaan Daya Rendah Dimana CPU Tidak Aktif." },
  { "en": "Apa Itu Endianness?", "id": "Urutan Byte Dalam Memori." },
  { "en": "Apa Itu Big-Endian?", "id": "Byte Paling Signifikan Disimpan Pertama." },
  { "en": "Apa Itu Little-Endian?", "id": "Byte Paling Tidak Signifikan Disimpan Pertama." },
  { "en": "Prosesor Mana Yang Menggunakan Little-Endian?", "id": "Intel x86." },
  { "en": "Apa Itu Penjajaran Memori (Memory Alignment)?", "id": "Menempatkan Data Di Alamat Memori Tertentu." },
  { "en": "Mengapa Penjajaran Memori Penting?", "id": "Meningkatkan Kinerja Akses Memori." },
  { "en": "Apa Itu Bus?", "id": "Jalur Komunikasi Bersama." },
  { "en": "Apa Itu Lebar Bus?", "id": "Jumlah Bit Yang Ditransfer Secara Paralel." },
  { "en": "Apa Itu Bandwidth Bus?", "id": "Laju Transfer Data Maksimum Bus." },
  { "en": "Apa Itu Peripheral Component Interconnect (PCI)?", "id": "Bus Komputer Lokal Standar." },
  { "en": "Apa Itu PCI Express (PCIe)?", "id": "Penerus PCI Dengan Koneksi Serial." },
  { "en": "Apa Itu Lane Dalam PCIe (PCI Express)?", "id": "Sepasang Jalur Transmisi Serial." },
  { "en": "Apa Itu Universal Serial Bus (USB)?", "id": "Standar Bus Untuk Menghubungkan Periferal." },
  { "en": "Apa Itu Plug and Play?", "id": "Fitur Untuk Mengkonfigurasi Perangkat Otomatis." },
  { "en": "Apa Itu Hot Swapping?", "id": "Memasang Atau Melepas Perangkat." },
  { "en": "Apa Itu Dependensi Data?", "id": "Instruksi Membutuhkan Hasil Dari Instruksi Lain." },
  { "en": "Apa Itu Read-After-Write (RAW) Hazard?", "id": "Jenis Dependensi Data Sebenarnya." },
  { "en": "Apa Itu Write-After-Read (WAR) Hazard?", "id": "Juga Dikenal Sebagai Anti-Dependensi." },
  { "en": "Apa Itu Write-After-Write (WAW) Hazard?", "id": "Juga Dikenal Sebagai Dependensi Output." },
  { "en": "Apa Itu Penggantian Nama Register (Register Renaming)?", "id": "Teknik Untuk Menghilangkan Hazard WAR Dan WAW." },
  { "en": "Apa Itu Eksekusi Out-of-Order?", "id": "Mengeksekusi Instruksi Tidak Sesuai Urutan." },
  { "en": "Apa Itu Reorder Buffer (ROB)?", "id": "Menyimpan Hasil Instruksi Out-of-Order." },
  { "en": "Apa Itu Tomasulo Algorithm?", "id": "Algoritma Untuk Eksekusi Out-of-Order Dinamis." },
  { "en": "Apa Itu Reservation Station?", "id": "Menyimpan Instruksi Yang Menunggu Operand." },
  { "en": "Apa Itu Eksekusi Superskalar?", "id": "Memiliki Beberapa Unit Eksekusi." },
  { "en": "Apa Itu Loop Unrolling?", "id": "Teknik Kompiler Untuk Mengurangi Overhead." },
  { "en": "Apa Itu Error Correction Code (ECC) Memory?", "id": "Dapat Mendeteksi Dan Memperbaiki Kesalahan Memori." },
  { "en": "Apa Itu Bit Paritas?", "id": "Bit Tambahan Untuk Mendeteksi Kesalahan." },
  { "en": "Apa Itu Single-Error Correction, Double-Error Detection (SECDED)?", "id": "Skema ECC (Error Correction Code) Umum." },
  { "en": "Di Mana Memori ECC (Error Correction Code) Digunakan?", "id": "Di Server Dan Sistem Kritis Lainnya." },
  { "en": "Apa Itu Prosesor Vektor?", "id": "CPU Yang Dapat Beroperasi Pada Array." },
  { "en": "Apa Itu Graphics Processing Unit (GPU)?", "id": "Prosesor Khusus Untuk Grafis." },
  { "en": "Bagaimana Arsitektur GPU (Graphics Processing Unit)?", "id": "Sangat Paralel Dengan Banyak Core Sederhana." },
  { "en": "Apa Itu General-Purpose GPU (GPGPU)?", "id": "Menggunakan GPU Untuk Komputasi Tujuan Umum." },
  { "en": "Apa Itu CUDA (Compute Unified Device Architecture)?", "id": "Platform Komputasi Paralel Dari Nvidia." },
  { "en": "Apa Itu OpenCL (Open Computing Language)?", "id": "Standar Terbuka Untuk Komputasi Paralel." },
  { "en": "Apa Itu Komputasi Heterogen?", "id": "Menggunakan Berbagai Jenis Prosesor." },
  { "en": "Apa Itu Arsitektur NUMA (Non-Uniform Memory Access)?", "id": "Waktu Akses Memori Bergantung Pada Lokasi." },
  { "en": "Apa Itu Arsitektur UMA (Uniform Memory Access)?", "id": "Waktu Akses Memori Sama." },
  { "en": "Apa Itu Multiprocessing Simetris (Symmetric Multiprocessing)?", "id": "Sistem Dengan Banyak Prosesor Identik." },
  { "en": "Apa Itu Kluster Komputer?", "id": "Sekelompok Komputer Yang Bekerja Bersama." },
  { "en": "Apa Itu High-Performance Computing (HPC)?", "id": "Komputasi Berkinerja Sangat Tinggi." },
  { "en": "Apa Itu Superkomputer?", "id": "Komputer Tercepat Di Dunia." },
  { "en": "Apa Itu Tingkat Perlindungan (Protection Rings)?", "id": "Level Hak Istimewa Dalam Arsitektur." },
  { "en": "Level Mana Yang Paling Istimewa?", "id": "Ring 0, Tempat Kernel Berjalan." },
  { "en": "Apa Itu Panggilan Sistem (System Call)?", "id": "Cara Program Meminta Layanan Kernel." },
  { "en": "Apa Itu Hypervisor?", "id": "Perangkat Lunak Untuk Membuat Mesin Virtual." },
  { "en": "Apa Itu Virtualisasi?", "id": "Membuat Versi Virtual Dari Sesuatu." },
  { "en": "Apa Itu Virtual Machine Monitor (VMM)?", "id": "Nama Lain Untuk Hypervisor." },
  { "en": "Apa Itu Virtualisasi Berbasis Perangkat Keras?", "id": "Dukungan CPU Untuk Virtualisasi." },
  { "en": "Contoh Dukungan Virtualisasi Adalah Apa?", "id": "Intel VT-x Dan AMD-V." },
  { "en": "Apa Itu Microcode?", "id": "Lapisan Instruksi Tingkat Rendah." },
  { "en": "Apa Itu Jaringan Interkoneksi?", "id": "Menghubungkan Node Dalam Sistem Paralel." },
  { "en": "Apa Itu Topologi Jaringan?", "id": "Pola Koneksi Antar Node." },
  { "en": "Sebutkan Contoh Topologi?", "id": "Bus, Bintang, Cincin, Jala." },
  { "en": "Apa Itu Latensi?", "id": "Waktu Tunda Untuk Pengiriman Data." },
  { "en": "Apa Itu Bandwidth?", "id": "Laju Transfer Data Maksimum." },
  { "en": "Apa Itu Jaringan On-Chip (Network-on-Chip)?", "id": "Jaringan Komunikasi Di Dalam Chip." },
  { "en": "Apa Itu Router?", "id": "Perangkat Yang Meneruskan Paket Data." },
  { "en": "Apa Itu Switch?", "id": "Perangkat Yang Menghubungkan Perangkat Jaringan." },
  { "en": "Apa Itu Simultaneous Multithreading (SMT)?", "id": "Mengeksekusi Thread Berbeda Di Satu Core." },
  { "en": "Apa Contoh Implementasi SMT (Simultaneous Multithreading)?", "id": "Teknologi Hyper-Threading Intel." },
  { "en": "Apa Itu Write Buffer?", "id": "Memori Antara CPU Dan Cache." },
  { "en": "Apa Fungsi Write Buffer?", "id": "Mengurangi Stall CPU Selama Operasi Tulis." },
  { "en": "Apa Itu Victim Cache?", "id": "Cache Kecil Menyimpan Blok Yang Dikeluarkan." },
  { "en": "Apa Itu Prefetching?", "id": "Mengambil Data Sebelum Dibutuhkan." },
  { "en": "Apa Itu Hardware Prefetching?", "id": "Perangkat Keras Secara Otomatis Melakukan Prefetch." },
  { "en": "Apa Itu Software Prefetching?", "id": "Kompiler Memasukkan Instruksi Prefetch." },
  { "en": "Apa Trade-off Ukuran Blok Cache?", "id": "Besar Mengurangi Miss, Kecil Mengurangi Waktu." },
  { "en": "Apa Itu Protokol Koherensi MOESI?", "id": "Modified, Owned, Exclusive, Shared, Invalid." },
  { "en": "Apa Status 'Owned' Dalam MOESI?", "id": "Blok Dimodifikasi Dan Dibagi." },
  { "en": "Apa Itu Interrupt Controller?", "id": "Perangkat Keras Untuk Mengelola Interupsi." },
  { "en": "Apa Itu Advanced Programmable Interrupt Controller (APIC)?", "id": "Arsitektur Kontroler Interupsi Intel." },
  { "en": "Apa Itu Small Computer System Interface (SCSI)?", "id": "Standar Untuk Menghubungkan Periferal." },
  { "en": "Apa Itu Serial Attached SCSI (SAS)?", "id": "Versi Serial Dari SCSI." },
  { "en": "Apa Itu Serial ATA (SATA)?", "id": "Antarmuka Bus Komputer Untuk Perangkat Penyimpanan." },
  { "en": "Apa Itu NVM Express (NVMe)?", "id": "Antarmuka Logika Untuk Mengakses Penyimpanan Non-Volatile." },
  { "en": "Mengapa NVMe (NVM Express) Sangat Cepat?", "id": "Dirancang Khusus Untuk SSD (Solid-State Drive)." },
  { "en": "Apa Itu Data-Level Parallelism (DLP)?", "id": "Paralelisme Dari Operasi Pada Data." },
  { "en": "Apa Itu Task-Level Parallelism (TLP)?", "id": "Paralelisme Dari Eksekusi Tugas Berbeda." },
  { "en": "Apa Itu Model Konsistensi Memori?", "id": "Aturan Urutan Operasi Baca Dan Tulis." },
  { "en": "Apa Itu Konsistensi Sekuensial?", "id": "Model Konsistensi Paling Ketat." },
  { "en": "Apa Itu Arsitektur Network Processor?", "id": "Prosesor Khusus Untuk Tugas Jaringan." },
  { "en": "Apa Itu Systolic Array?", "id": "Jaringan Prosesor Untuk Komputasi Paralel." },
  { "en": "Apa Itu Benchmark?", "id": "Program Untuk Mengukur Kinerja." },
  { "en": "Apa Itu SPEC (Standard Performance Evaluation Corporation)?", "id": "Organisasi Yang Membuat Benchmark CPU." },
  { "en": "Apa Itu TPC (Transaction Processing Performance Council)?", "id": "Membuat Benchmark Untuk Pemrosesan Transaksi." },
  { "en": "Apa Itu MFLOPS (MegaFLOPS)?", "id": "Satu Juta Operasi Floating-Point Per Detik." },
  { "en": "Apa Itu GFLOPS (GigaFLOPS)?", "id": "Satu Miliar Operasi Floating-Point Per Detik." },
  { "en": "Apa Itu TFLOPS (TeraFLOPS)?", "id": "Satu Triliun Operasi Floating-Point Per Detik." },
  { "en": "Apa Itu Instruksi Istimewa (Privileged)?", "id": "Instruksi Yang Hanya Dapat Dijalankan Kernel." },
  { "en": "Apa Itu Trap (Exception)?", "id": "Interupsi Yang Dihasilkan Secara Sinkron." },
  { "en": "Apa Penyebab Trap?", "id": "Kesalahan Seperti Pembagian Dengan Nol." },
  { "en": "Apa Itu Register Bendera (Flags)?", "id": "Menyimpan Informasi Status (Carry, Zero)." },
  { "en": "Apa Itu Kode Kondisi?", "id": "Nama Lain Untuk Bendera Status." },
  { "en": "Apa Itu Thermal Design Power (TDP)?", "id": "Jumlah Panas Maksimum Yang Dihasilkan." },
  { "en": "Mengapa TDP (Thermal Design Power) Penting?", "id": "Untuk Merancang Sistem Pendingin Yang Tepat." },
  { "en": "Apa Itu Kepadatan Daya (Power Density)?", "id": "Daya Per Satuan Luas Chip." },
  { "en": "Apa Itu Context Switch?", "id": "Menyimpan Dan Memulihkan Keadaan Proses." },
  { "en": "Bagaimana Arsitektur Mendukung Context Switch?", "id": "Dengan Instruksi Untuk Menyimpan Register." },
  { "en": "Apa Itu Proteksi Memori?", "id": "Mencegah Proses Mengakses Memori Proses Lain." },
  { "en": "Bagaimana MMU (Memory Management Unit) Mendukung Proteksi?", "id": "Dengan Batas Alamat Dan Bit Hak Akses." },
  { "en": "Apa Itu PDP-11?", "id": "Minikomputer Berpengaruh Dari Digital Equipment Corporation." },
  { "en": "Apa Itu Cray-1?", "id": "Salah Satu Superkomputer Vektor Paling Terkenal." },
  { "en": "Apa Itu CDC (Control Data Corporation) 6600?", "id": "Dianggap Sebagai Superkomputer Sukses Pertama." },
  { "en": "Apa Itu Branch History Table (BHT)?", "id": "Tabel Untuk Menyimpan Sejarah Percabangan." },
  { "en": "Apa Itu Prediktor 2-bit?", "id": "Prediktor Percabangan Dengan Empat Keadaan." },
  { "en": "Apa Itu Prediktor Berkorelasi?", "id": "Menggunakan Perilaku Percabangan Lain." },
  { "en": "Apa Itu Common Data Bus (CDB)?", "id": "Bus Dalam Algoritma Tomasulo." },
  { "en": "Apa Fungsi CDB (Common Data Bus)?", "id": "Menyiarkan Hasil Ke Reservation Station." },
  { "en": "Apa Itu Penjadwalan Instruksi Dinamis?", "id": "Perangkat Keras Mengubah Urutan Instruksi." },
  { "en": "Apa Itu Penjadwalan Instruksi Statis?", "id": "Kompiler Mengubah Urutan Instruksi." },
  { "en": "Apa Itu Skalar?", "id": "Besaran Yang Hanya Memiliki Nilai." },
  { "en": "Apa Itu Vektor?", "id": "Array Satu Dimensi Dari Data." },
  { "en": "Apa Itu Register Vektor?", "id": "Menyimpan Vektor Data." },
  { "en": "Apa Itu Chaining?", "id": "Meneruskan Hasil Dari Satu Unit Fungsional." },
  { "en": "Apa Itu Strip Mining?", "id": "Teknik Untuk Memproses Vektor Panjang." },
  { "en": "Apa Itu Scatter-Gather?", "id": "Operasi Akses Memori Tidak Berurutan." },
  { "en": "Apa Itu Arsitektur Load/Store?", "id": "Operasi Memori Hanya Melalui Load/Store." },
  { "en": "Arsitektur Mana Yang Menggunakan Load/Store?", "id": "Sebagian Besar Arsitektur RISC (Reduced Instruction Set Computer)." },
  { "en": "Apa Itu Register-Memory Architecture?", "id": "Memungkinkan Operasi Langsung Pada Memori." },
  { "en": "Apa Itu Stack Architecture?", "id": "Menggunakan Tumpukan Untuk Menyimpan Operand." },
  { "en": "Apa Itu Organisasi Komputer?", "id": "Unit Operasional Dan Interkoneksinya." },
  { "en": "Apa Perbedaan Arsitektur Dan Organisasi?", "id": "Arsitektur Logis, Organisasi Fisik." },
  { "en": "Apa Itu Mode Supervisor?", "id": "Mode Eksekusi Istimewa Untuk Kernel." },
  { "en": "Apa Itu Mode Pengguna?", "id": "Mode Terbatas Untuk Aplikasi." },
  { "en": "Apa Itu ISA (Instruction Set Architecture)?", "id": "Antarmuka Antara Perangkat Keras Dan Lunak." },
  { "en": "Apa Itu ABI (Application Binary Interface)?", "id": "Antarmuka Antara Aplikasi Dan OS." },
  { "en": "Apa Itu API (Application Programming Interface)?", "id": "Antarmuka Antara Perangkat Lunak." },
  { "en": "Apa Itu Motherboard?", "id": "Papan Sirkuit Utama Komputer." },
  { "en": "Apa Itu Chipset?", "id": "Sekumpulan Sirkuit Terpadu Di Motherboard." },
  { "en": "Apa Itu Northbridge?", "id": "Menghubungkan CPU, RAM, Dan PCIe." },
  { "en": "Apa Itu Southbridge?", "id": "Menghubungkan Perangkat I/O Yang Lebih Lambat." },
  { "en": "Apa Itu Platform Controller Hub (PCH)?", "id": "Desain Modern Yang Menggantikan North/Southbridge." },
  { "en": "Apa Itu Single Board Computer (SBC)?", "id": "Komputer Lengkap Dalam Satu Papan." },
  { "en": "Contoh SBC (Single Board Computer) Adalah Apa?", "id": "Raspberry Pi." },
  { "en": "Apa Itu Micro-operation (μop)?", "id": "Operasi Tingkat Rendah Dalam CPU." },
  { "en": "Bagaimana Prosesor CISC Bekerja?", "id": "Menerjemahkan Instruksi Kompleks Menjadi μop." },
  { "en": "Apa Itu Eksekusi Spekulatif?", "id": "Mengeksekusi Instruksi Secara Spekulatif." },
  { "en": "Apa Itu Branch Misprediction?", "id": "Prediksi Percabangan Yang Salah." },
  { "en": "Apa Penalti Dari Branch Misprediction?", "id": "Harus Membuang Hasil Spekulatif." },
  { "en": "Apa Itu Commit Stage?", "id": "Tahap Dimana Hasil Instruksi Dibuat." },
  { "en": "Apa Itu Write Allocate Policy?", "id": "Mengambil Blok Ke Cache Saat Write Miss." },
  { "en": "Apa Itu No-Write Allocate Policy?", "id": "Menulis Langsung Ke Memori Saat Write Miss." },
  { "en": "Apa Itu Cache Inklusif?", "id": "Cache Level Lebih Tinggi Berisi Cache Level Rendah." },
  { "en": "Apa Itu Cache Eksklusif?", "id": "Konten Cache Level Rendah Dan Tinggi Berbeda." },
  { "en": "Apa Itu Cache Non-Inklusif?", "id": "Tidak Ada Aturan Inklusi Atau Eklusi." },
  { "en": "Apa Itu Virtual Address?", "id": "Alamat Yang Dihasilkan Oleh Program." },
  { "en": "Apa Itu Physical Address?", "id": "Alamat Sebenarnya Di Memori Fisik." },
  { "en": "Apa Itu Segmentasi?", "id": "Membagi Memori Menjadi Segmen." },
  { "en": "Apa Itu Page?", "id": "Blok Memori Virtual Berukuran Tetap." },
  { "en": "Apa Itu Frame?", "id": "Blok Memori Fisik Berukuran Tetap." },
  { "en": "Apa Itu Swapping?", "id": "Memindahkan Seluruh Proses Antara Memori." },
  { "en": "Apa Itu Thrashing?", "id": "Kinerja Buruk Akibat Paging Berlebihan." },
  { "en": "Apa Itu Working Set?", "id": "Kumpulan Halaman Yang Dibutuhkan Proses." },
  { "en": "Apa Itu Page Replacement Algorithm?", "id": "Algoritma Untuk Memilih Halaman." },
  { "en": "Apa Itu Clock Algorithm?", "id": "Algoritma Penggantian Halaman." },
  { "en": "Apa Itu Synchronous DRAM (SDRAM)?", "id": "DRAM (Dynamic Random-Access Memory) Yang Disinkronkan." },
  { "en": "Apa Itu Double Data Rate (DDR) SDRAM?", "id": "Mentransfer Data Dua Kali Per Siklus Clock." },
  { "en": "Apa Itu Graphics DDR (GDDR)?", "id": "Jenis DDR SDRAM Untuk Kartu Grafis." },
  { "en": "Apa Itu CAS (Column Address Strobe) Latency?", "id": "Waktu Tunda Akses Kolom Memori." },
  { "en": "Apa Fungsi Kontroler Memori?", "id": "Mengelola Aliran Data Ke Dan Dari RAM." },
  { "en": "Apa Struktur Sel SRAM (Static RAM)?", "id": "Menggunakan Enam Transistor (6T) Sebagai Flip-flop." },
  { "en": "Apa Itu Topologi Torus?", "id": "Topologi Jaringan Jala Dengan Koneksi Melingkar." },
  { "en": "Apa Itu Topologi Hypercube?", "id": "Topologi Jaringan Untuk Komputer Paralel." },
  { "en": "Apa Itu Routing Wormhole?", "id": "Paket Mulai Diteruskan Sebelum Diterima." },
  { "en": "Apa Itu Routing Store-and-Forward?", "id": "Paket Diterima Penuh Sebelum Diteruskan." },
  { "en": "Apa Itu Deadlock?", "id": "Situasi Dimana Paket Saling Memblokir." },
  { "en": "Apa Itu Livelock?", "id": "Paket Terus Bergerak Tetapi Tidak Mencapai." },
  { "en": "Apa Itu Ripple-Carry Adder?", "id": "Penjumlah Multi-bit Dengan Carry Berantai." },
  { "en": "Apa Itu Carry-Lookahead Adder?", "id": "Penjumlah Cepat Yang Memprediksi Carry." },
  { "en": "Apa Itu Algoritma Booth?", "id": "Algoritma Untuk Perkalian Bilangan Bertanda." },
  { "en": "Apa Itu Pengecualian Floating-Point?", "id": "Kondisi Kesalahan Seperti Overflow Atau Underflow." },
  { "en": "Apa Itu Mode Pembulatan?", "id": "Aturan Untuk Membulatkan Hasil Floating-Point." },
  { "en": "Apa Itu Response Time Dalam I/O?", "id": "Total Waktu Untuk Menyelesaikan Operasi I/O." },
  { "en": "Apa Itu Throughput Dalam I/O?", "id": "Laju Data Yang Ditransfer." },
  { "en": "Apa Itu Device Driver?", "id": "Perangkat Lunak Yang Mengontrol Perangkat." },
  { "en": "Apa Itu Thunderbolt?", "id": "Antarmuka Perangkat Keras Berkecepatan Sangat Tinggi." },
  { "en": "Apa Itu Proses?", "id": "Program Yang Sedang Dieksekusi." },
  { "en": "Apa Perbedaan Proses Dan Thread?", "id": "Thread Berbagi Ruang Alamat, Proses Tidak." },
  { "en": "Apa Itu Scheduler?", "id": "Bagian OS Yang Memilih Tugas." },
  { "en": "Apa Itu Serangan Spectre?", "id": "Kerentanan Keamanan Berbasis Eksekusi Spekulatif." },
  { "en": "Apa Itu Serangan Meltdown?", "id": "Kerentanan Yang Memungkinkan Akses Memori Kernel." },
  { "en": "Apa Itu Trusted Execution Environment (TEE)?", "id": "Area Aman Di Dalam Prosesor." },
  { "en": "Apa Itu Secure Boot?", "id": "Memastikan Bootloader Dan Kernel Terpercaya." },
  { "en": "Apa Itu Hardware Security Module (HSM)?", "id": "Perangkat Keras Untuk Mengelola Kunci Kriptografi." },
  { "en": "Apa Itu Domain-Specific Architecture (DSA)?", "id": "Arsitektur Yang Dioptimalkan Untuk Domain." },
  { "en": "Apa Itu Tensor Processing Unit (TPU)?", "id": "ASIC (Application-Specific Integrated Circuit) Google Untuk AI." },
  { "en": "Apa Itu Neural Processing Unit (NPU)?", "id": "Prosesor Khusus Untuk Beban Kerja AI." },
  { "en": "Apa Prinsip 'Make the Common Case Fast'?", "id": "Optimalkan Operasi Yang Paling Sering." },
  { "en": "Apa Rumus Kinerja CPU (Central Processing Unit)?", "id": "Waktu Eksekusi = Instruksi x CPI x Siklus." },
  { "en": "Apa Itu Mean Time To Failure (MTTF)?", "id": "Waktu Rata-rata Hingga Kegagalan Pertama." },
  { "en": "Apa Itu Mean Time Between Failures (MTBF)?", "id": "Waktu Rata-rata Antara Dua Kegagalan." },
  { "en": "Apa Itu Availability?", "id": "Persentase Waktu Sistem Beroperasi." },
  { "en": "Apa Itu Datapath?", "id": "Elemen Prosesor Yang Melakukan Operasi Data." },
  { "en": "Apa Itu Unit Kontrol Hardwired?", "id": "Kontroler Berbasis Logika Kombinasional Tetap." },
  { "en": "Apa Itu Unit Kontrol Microprogrammed?", "id": "Kontroler Yang Menggunakan Program Mikro." },
  { "en": "Apa Keuntungan Kontrol Hardwired?", "id": "Sangat Cepat." },
  { "en": "Apa Keuntungan Kontrol Microprogrammed?", "id": "Fleksibel Untuk Mengubah Set Instruksi." },
  { "en": "Apa Itu Algoritma LRU (Least Recently Used) Semu?", "id": "Aproksimasi Dari LRU Sebenarnya." },
  { "en": "Apa Itu Cache Multi-Level?", "id": "Menggunakan Beberapa Level Cache (L1, L2, L3)." },
  { "en": "Apa Itu Bandwidth Memori?", "id": "Laju Data Dapat Dibaca Dari Memori." },
  { "en": "Apa Itu Bank Memori?", "id": "Memungkinkan Akses Paralel Ke DRAM." },
  { "en": "Apa Itu Interleaving Memori?", "id": "Menyebarkan Alamat Di Beberapa Bank." },
  { "en": "Apa Itu Single-Channel Memory?", "id": "Satu Jalur Data 64-bit Ke Memori." },
  { "en": "Apa Itu Dual-Channel Memory?", "id": "Dua Jalur Data 64-bit Ke Memori." },
  { "en": "Apa Itu Registered Memory (Buffered)?", "id": "Memiliki Register Antara DRAM Dan Kontroler." },
  { "en": "Di Mana Registered Memory Digunakan?", "id": "Di Server Untuk Mendukung Banyak Modul." },
  { "en": "Apa Itu Unbuffered Memory?", "id": "Memori Tanpa Register Tambahan." },
  { "en": "Apa Itu Full-Buffer DIMM (FB-DIMM)?", "id": "Menggunakan Advanced Memory Buffer (AMB)." },
  { "en": "Apa Itu Stack?", "id": "Struktur Data LIFO (Last-In, First-Out)." },
  { "en": "Apa Itu Stack Pointer?", "id": "Register Yang Menunjuk Ke Atas Stack." },
  { "en": "Apa Itu Frame Pointer?", "id": "Register Yang Menunjuk Ke Frame Stack." },
  { "en": "Apa Itu Stack Frame?", "id": "Area Stack Untuk Panggilan Fungsi." },
  { "en": "Apa Itu Panggilan Prosedur (Procedure Call)?", "id": "Instruksi Untuk Memanggil Fungsi." },
  { "en": "Apa Itu Return Address?", "id": "Alamat Untuk Kembali Setelah Panggilan." },
  { "en": "Apa Itu Buffer Overflow?", "id": "Kerentanan Keamanan Akibat Penulisan Berlebih." },
  { "en": "Apa Itu Mesin Virtual Sistem?", "id": "Meniru Seluruh Komputer." },
  { "en": "Apa Itu Mesin Virtual Proses?", "id": "Menjalankan Satu Proses (Contoh: JVM)." },
  { "en": "Apa Itu Java Virtual Machine (JVM)?", "id": "Lingkungan Eksekusi Untuk Kode Java." },
  { "en": "Apa Itu Bytecode?", "id": "Set Instruksi Portabel Untuk Mesin Virtual." },
  { "en": "Apa Itu Just-In-Time (JIT) Compilation?", "id": "Mengkompilasi Bytecode Menjadi Kode Mesin." },
  { "en": "Apa Itu Komputasi Berbasis Awan (Cloud)?", "id": "Menyediakan Layanan Komputasi Melalui Internet." },
  { "en": "Apa Itu Infrastructure as a Service (IaaS)?", "id": "Menyediakan Infrastruktur Virtual (VM)." },
  { "en": "Apa Itu Platform as a Service (PaaS)?", "id": "Menyediakan Platform Pengembangan." },
  { "en": "Apa Itu Software as a Service (SaaS)?", "id": "Menyediakan Aplikasi Perangkat Lunak." },
  { "en": "Apa Itu Arsitektur Klien-Server?", "id": "Model Komputasi Terdistribusi." },
  { "en": "Apa Itu Arsitektur Peer-to-Peer (P2P)?", "id": "Setiap Node Dapat Bertindak Sebagai Klien/Server." },
  { "en": "Apa Itu Komputasi Grid?", "id": "Menghubungkan Sumber Daya Komputer Terdistribusi." },
  { "en": "Apa Itu Prosesor Skalar?", "id": "Memproses Satu Item Data Sekaligus." },
  { "en": "Apa Itu Clock Cycle Time?", "id": "Durasi Satu Siklus Clock." },
  { "en": "Apa Hubungan Clock Rate Dan Cycle Time?", "id": "Saling Berbanding Terbalik." },
  { "en": "Apa Itu Clocks Per Instruction (CPI)?", "id": "Jumlah Siklus Rata-rata Per Instruksi." },
  { "en": "Bagaimana Pipelining Mempengaruhi CPI (Clocks Per Instruction)?", "id": "Mendekatkan CPI Ideal Menjadi Satu." },
  { "en": "Apa Itu Instruction Fetch (IF) Stage?", "id": "Tahap Pengambilan Instruksi." },
  { "en": "Apa Itu Instruction Decode (ID) Stage?", "id": "Tahap Dekode Dan Pengambilan Register." },
  { "en": "Apa Itu Execute (EX) Stage?", "id": "Tahap Eksekusi Operasi ALU." },
  { "en": "Apa Itu Memory Access (MEM) Stage?", "id": "Tahap Akses Memori (Load/Store)." },
  { "en": "Apa Itu Write-Back (WB) Stage?", "id": "Tahap Penulisan Hasil Kembali Ke Register." },
  { "en": "Apa Itu Jalur Data (Datapath)?", "id": "Bagian Prosesor Yang Memanipulasi Data." },
  { "en": "Apa Itu Unit Kontrol?", "id": "Bagian Prosesor Yang Mengontrol Datapath." },
  { "en": "Apa Itu Sinyal Kontrol?", "id": "Sinyal Yang Mengatur Operasi Datapath." },
  { "en": "Apa Itu Multiplexer?", "id": "Memilih Satu Dari Banyak Input." },
  { "en": "Apa Itu Demultiplexer?", "id": "Mengarahkan Satu Input Ke Banyak Output." },
  { "en": "Apa Itu Encoder?", "id": "Mengubah Input Menjadi Kode Biner." },
  { "en": "Apa Itu Decoder?", "id": "Mengubah Kode Biner Menjadi Output." },
  { "en": "Apa Itu State Machine?", "id": "Model Perilaku Sekuensial." },
  { "en": "Apa Itu Moore Machine?", "id": "Output Hanya Bergantung Pada Keadaan." },
  { "en": "Apa Itu Mealy Machine?", "id": "Output Bergantung Pada Keadaan Dan Input." },
  { "en": "Apa Itu Finite Automata?", "id": "Nama Lain Untuk Mesin Keadaan." },
  { "en": "Apa Itu Dependensi Nama?", "id": "Hazard WAR Dan WAW." },
  { "en": "Apa Itu Dependensi Kontrol?", "id": "Instruksi Bergantung Pada Hasil Percabangan." },
  { "en": "Apa Itu Scoreboarding?", "id": "Mekanisme Kontrol Terpusat Untuk Pipa." },
  { "en": "Apa Itu Prosesor Dinamis?", "id": "Menggunakan Penjadwalan Dinamis." },
  { "en": "Apa Itu Prosesor Statis?", "id": "Bergantung Pada Penjadwalan Statis." },
  { "en": "Apa Itu Prediksi Percabangan Statis?", "id": "Prediksi Yang Dibuat Saat Kompilasi." },
  { "en": "Apa Itu Aturan 'Branch-Not-Taken'?", "id": "Asumsikan Percabangan Tidak Pernah Diambil." },
  { "en": "Apa Itu Prediksi Percabangan Dinamis?", "id": "Prediksi Berdasarkan Perilaku Program." },
  { "en": "Apa Itu Saturating Counter?", "id": "Penghitung Yang Berhenti Di Nilai Maksimum." },
  { "en": "Apa Itu Two-Level Adaptive Predictor?", "id": "Menggunakan Dua Level Sejarah Percabangan." },
  { "en": "Apa Itu Penalti Salah Prediksi?", "id": "Siklus Yang Hilang Akibat Prediksi Salah." },
  { "en": "Apa Itu Eksekusi Spekulatif?", "id": "Mengeksekusi Instruksi Sebelum Percabangan Diselesaikan." },
  { "en": "Apa Itu Delay Slot?", "id": "Instruksi Setelah Percabangan Yang Selalu." },
  { "en": "Apa Itu Cache Set-Asosiatif?", "id": "Kompromi Antara Langsung Dan Asosiatif." },
  { "en": "Apa Itu Set?", "id": "Sekelompok Blok Cache." },
  { "en": "Apa Itu N-way Set-Associative?", "id": "Setiap Set Memiliki N Blok." },
  { "en": "Apa Itu Random Replacement Policy?", "id": "Mengganti Blok Cache Secara Acak." },
  { "en": "Apa Itu Cache Dingin (Cold Start Miss)?", "id": "Miss Pertama Kali Pada Sebuah Blok." },
  { "en": "Apa Itu Cache Kapasitas (Capacity Miss)?", "id": "Miss Akibat Ukuran Cache Terbatas." },
  { "en": "Apa Itu Cache Konflik (Conflict Miss)?", "id": "Miss Dalam Cache Set-Asosiatif." },
  { "en": "Apa Tiga 'C' Dari Cache Miss?", "id": "Compulsory, Capacity, Conflict." },
  { "en": "Apa Itu Cache Terpadu (Unified Cache)?", "id": "Menyimpan Instruksi Dan Data." },
  { "en": "Apa Itu Cache Terpisah (Split Cache)?", "id": "Cache Terpisah Untuk Instruksi Dan Data." },
  { "en": "Apa Keuntungan Cache Terpisah?", "id": "Bandwidth Lebih Tinggi, Mengurangi Konflik." },
  { "en": "Apa Itu Write-Combining Buffer?", "id": "Menggabungkan Beberapa Operasi Tulis." },
  { "en": "Apa Itu Cache Non-Blocking?", "id": "Dapat Melayani Hit Saat Miss." },
  { "en": "Apa Itu Average Memory Access Time (AMAT)?", "id": "Waktu Akses Memori Rata-rata." },
  { "en": "Apa Rumus AMAT (Average Memory Access Time)?", "id": "Waktu Hit + Laju Miss x Penalti." },
  { "en": "Apa Itu Penalti Miss (Miss Penalty)?", "id": "Waktu Tambahan Akibat Cache Miss." },
  { "en": "Apa Itu Optimisasi Kompiler?", "id": "Meningkatkan Kinerja Kode." },
  { "en": "Apa Itu Loop Interchange?", "id": "Menukar Urutan Loop Bersarang." },
  { "en": "Apa Itu Loop Fusion?", "id": "Menggabungkan Beberapa Loop Menjadi Satu." },
  { "en": "Apa Itu Loop Fission?", "id": "Memecah Loop Menjadi Beberapa." },
  { "en": "Apa Itu Blocking (Tiling)?", "id": "Memproses Data Dalam Blok." },
  { "en": "Apa Itu Alokasi Register?", "id": "Menugaskan Variabel Ke Register." },
  { "en": "Apa Itu Pengecualian (Exception)?", "id": "Gangguan Yang Dihasilkan CPU." },
  { "en": "Apa Itu Interupsi?", "id": "Pengecualian Yang Dihasilkan Secara Asinkron." },
  { "en": "Apa Itu Precise Exception?", "id": "Keadaan Prosesor Disimpan Dengan Tepat." },
  { "en": "Apa Itu Imprecise Exception?", "id": "Keadaan Prosesor Mungkin Tidak Konsisten." },
  { "en": "Apa Itu Vektor Pengecualian?", "id": "Tabel Alamat Handler Pengecualian." },
  { "en": "Apa Itu System Call?", "id": "Pengecualian Untuk Meminta Layanan OS." },
  { "en": "Apa Itu Floating-Point Unit (FPU)?", "id": "Unit Untuk Aritmetika Floating-Point." },
  { "en": "Apa Itu Single Precision?", "id": "Format Floating-Point 32-bit." },
  { "en": "Apa Itu Double Precision?", "id": "Format Floating-Point 64-bit." },
  { "en": "Apa Itu Not a Number (NaN)?", "id": "Nilai Khusus Untuk Hasil Tidak Terdefinisi." },
  { "en": "Apa Itu Infinity?", "id": "Nilai Khusus Untuk Overflow." },
  { "en": "Apa Itu Bilangan Denormalized?", "id": "Bilangan Sangat Kecil Dekat Nol." },
  { "en": "Apa Itu Fused Multiply-Add (FMA)?", "id": "Instruksi Yang Menggabungkan Perkalian." },
  { "en": "Apa Itu Guard Bit?", "id": "Bit Tambahan Untuk Meningkatkan Akurasi." },
  { "en": "Apa Itu Sticky Bit?", "id": "Bit Untuk Membantu Pembulatan." },
  { "en": "Apa Itu SIMD (Single Instruction, Multiple Data) Extensions?", "id": "Set Instruksi Untuk Paralelisme Data." },
  { "en": "Apa Itu MMX (MultiMedia eXtensions)?", "id": "Ekstensi SIMD Awal Dari Intel." },
  { "en": "Apa Itu SSE (Streaming SIMD Extensions)?", "id": "Penerus MMX Dari Intel." },
  { "en": "Apa Itu AVX (Advanced Vector Extensions)?", "id": "Ekstensi SIMD Dengan Vektor Lebih Lebar." },
  { "en": "Apa Itu Komputasi Vektor?", "id": "Operasi Pada Seluruh Array Data." },
  { "en": "Apa Itu Cluster?", "id": "Kumpulan Komputer Independen." },
  { "en": "Apa Itu Node?", "id": "Satu Komputer Dalam Cluster." },
  { "en": "Apa Itu Beowulf Cluster?", "id": "Cluster Yang Dibangun Dari Komputer." },
  { "en": "Apa Itu Message Passing Interface (MPI)?", "id": "Standar Untuk Pemrograman Paralel." },
  { "en": "Apa Itu Jaringan Interkoneksi?", "id": "Menghubungkan Node Dalam Cluster." },
  { "en": "Apa Itu Topologi Fat Tree?", "id": "Topologi Jaringan Untuk Kluster." },
  { "en": "Apa Itu Switch?", "id": "Perangkat Yang Menghubungkan Node Jaringan." },
  { "en": "Apa Itu Protokol Komunikasi?", "id": "Aturan Untuk Pertukaran Data." },
  { "en": "Apa Itu TCP/IP (Transmission Control Protocol/Internet Protocol)?", "id": "Rangkaian Protokol Inti Internet." },
  { "en": "Apa Itu Ethernet?", "id": "Teknologi Jaringan Area Lokal." },
  { "en": "Apa Itu InfiniBand?", "id": "Jaringan Interkoneksi Kinerja Tinggi." },
  { "en": "Apa Itu Firmware?", "id": "Perangkat Lunak Tingkat Rendah." },
  { "en": "Apa Itu BIOS (Basic Input/Output System)?", "id": "Firmware Di Motherboard PC." },
  { "en": "Apa Itu POST (Power-On Self-Test)?", "id": "Tes Diagnostik Saat Booting." },
  { "en": "Apa Itu CMOS (Complementary Metal-Oxide-Semiconductor) Setup?", "id": "Utilitas Konfigurasi BIOS." },
  { "en": "Apa Itu Bootloader?", "id": "Program Yang Memuat Sistem Operasi." },
  { "en": "Apa Itu Master Boot Record (MBR)?", "id": "Sektor Pertama Hard Disk." },
  { "en": "Apa Itu GUID (Globally Unique Identifier) Partition Table (GPT)?", "id": "Standar Partisi Modern." },
  { "en": "Apa Itu Power Supply Unit (PSU)?", "id": "Mengubah Daya AC Menjadi DC." },
  { "en": "Apa Itu Tegangan Rel (Rail)?", "id": "Output Tegangan Dari PSU." },
  { "en": "Apa Itu Standar ATX (Advanced Technology eXtended)?", "id": "Standar Untuk Motherboard Dan PSU." },
  { "en": "Apa Itu Form Factor?", "id": "Spesifikasi Ukuran Dan Bentuk." },
  { "en": "Apa Itu Chipset?", "id": "Mengontrol Komunikasi Antara Komponen." },
  { "en": "Apa Itu I/O (Input/Output) Controller Hub (ICH)?", "id": "Nama Lain Untuk Southbridge." },
  { "en": "Apa Itu Memory Controller Hub (MCH)?", "id": "Nama Lain Untuk Northbridge." },
  { "en": "Apa Itu Direct Media Interface (DMI)?", "id": "Menghubungkan Northbridge Dan Southbridge Intel." },
  { "en": "Apa Itu HyperTransport?", "id": "Interkoneksi Bus Milik AMD." },
  { "en": "Apa Itu QuickPath Interconnect (QPI)?", "id": "Interkoneksi Bus Milik Intel." },
  { "en": "Apa Itu Proses Manufaktur Semikonduktor?", "id": "Proses Pembuatan Chip." },
  { "en": "Apa Itu Node Proses?", "id": "Ukuran Fitur Terkecil." },
  { "en": "Apa Itu Skala Dennard?", "id": "Aturan Penskalaan Untuk Transistor MOSFET." },
  { "en": "Apa Itu Hukum Moore?", "id": "Jumlah Transistor Berlipat Ganda." },
  { "en": "Apa Itu Batas Daya?", "id": "Keterbatasan Peningkatan Kinerja Akibat Panas." },
  { "en": "Apa Itu Dark Silicon?", "id": "Bagian Chip Yang Dimatikan." },
  { "en": "Apa Itu Near Threshold Computing?", "id": "Mengoperasikan Transistor Dekat Tegangan." },
  { "en": "Apa Itu Komputasi Perseptual?", "id": "Membuat Interaksi Lebih Alami." },
  { "en": "Apa Itu Real-Time System?", "id": "Sistem Dengan Batas Waktu Ketat." },
  { "en": "Apa Itu Hard Real-Time?", "id": "Tenggat Waktu Mutlak Harus Dipenuhi." },
  { "en": "Apa Itu Soft Real-Time?", "id": "Tenggat Waktu Boleh Terlewat." },
  { "en": "Apa Itu Real-Time Operating System (RTOS)?", "id": "OS Untuk Sistem Real-Time." },
  { "en": "Apa Itu Watchdog Timer?", "id": "Timer Untuk Mereset Sistem." },
  { "en": "Apa Itu Jitter?", "id": "Variasi Waktu Dari Sinyal." },
  { "en": "Apa Itu Keandalan?", "id": "Probabilitas Sistem Beroperasi Benar." },
  { "en": "Apa Itu Toleransi Kesalahan (Fault Tolerance)?", "id": "Kemampuan Beroperasi Meski Ada Kesalahan." },
  { "en": "Apa Itu Redundansi?", "id": "Duplikasi Komponen Untuk Toleransi." },
  { "en": "Apa Itu Model Konsistensi Memori?", "id": "Aturan Yang Mengatur Urutan Operasi Memori." },
  { "en": "Apa Itu Konsistensi Sekuensial?", "id": "Hasilnya Seolah-olah Semua Operasi Berurutan." },
  { "en": "Apa Itu Konsistensi Longgar (Relaxed)?", "id": "Memungkinkan Pengurutan Ulang Operasi Memori." },
  { "en": "Apa Itu Pagar Memori (Memory Fence)?", "id": "Instruksi Untuk Menegakkan Urutan Operasi." },
  { "en": "Apa Itu Virtualisasi Trap-and-Emulate?", "id": "Hypervisor Menjebak Dan Meniru Instruksi Istimewa." },
  { "en": "Apa Itu Paravirtualisasi?", "id": "Sistem Operasi Tamu Dimodifikasi Untuk Virtualisasi." },
  { "en": "Apa Itu Kontainerisasi?", "id": "Virtualisasi Tingkat Sistem Operasi." },
  { "en": "Apa Perbedaan VM (Virtual Machine) Dan Kontainer?", "id": "Kontainer Berbagi Kernel OS Yang Sama." },
  { "en": "Apa Itu Cache Set-Asosiatif Miring (Skewed)?", "id": "Menggunakan Fungsi Hash Berbeda Untuk Set." },
  { "en": "Apa Itu Prediksi Cara (Way Prediction)?", "id": "Menebak Cara Cache Mana Yang Akan Hit." },
  { "en": "Apa Itu Trace Cache?", "id": "Menyimpan Urutan Instruksi Yang Dieksekusi." },
  { "en": "Apa Itu Algoritma Prefetching?", "id": "Aturan Untuk Memutuskan Data Mana." },
  { "en": "Apa Itu Memori Transaksional?", "id": "Mengeksekusi Blok Memori Secara Atomik." },
  { "en": "Apa Itu Hardware Transactional Memory (HTM)?", "id": "Dukungan Perangkat Keras Untuk Memori Transaksional." },
  { "en": "Apa Itu Software Transactional Memory (STM)?", "id": "Implementasi Perangkat Lunak Dari Memori Transaksional." },
  { "en": "Apa Itu Routing XY?", "id": "Algoritma Routing Sederhana Untuk Jaringan Jala." },
  { "en": "Apa Itu Kontrol Aliran (Flow Control)?", "id": "Mencegah Buffer Penuh Dalam Jaringan." },
  { "en": "Apa Itu Virtual Channel?", "id": "Membagi Buffer Fisik Menjadi Beberapa Antrian." },
  { "en": "Apa Itu Kode Hamming?", "id": "Kode Koreksi Kesalahan Untuk Kesalahan Bit Tunggal." },
  { "en": "Apa Itu Model Kesalahan Stuck-at?", "id": "Asumsi Kesalahan Umum Dalam Pengujian Chip." },
  { "en": "Apa Itu Kesalahan Transien?", "id": "Kesalahan Sementara Akibat Radiasi Atau Derau." },
  { "en": "Apa Itu Triple Modular Redundancy (TMR)?", "id": "Menggunakan Tiga Modul Dan Pemilih Mayoritas." },
  { "en": "Apa Itu Komputasi Reconfigurable?", "id": "Perangkat Keras Dapat Dikonfigurasi Ulang." },
  { "en": "Bagaimana FPGA (Field-Programmable Gate Array) Digunakan Sebagai Akselerator?", "id": "Mengimplementasikan Tugas Tertentu Di Perangkat Keras." },
  { "en": "Apa Itu Akselerator Kriptografi?", "id": "Perangkat Keras Khusus Untuk Enkripsi." },
  { "en": "Apa Itu Gerbang Kuantum?", "id": "Operasi Dasar Pada Qubit." },
  { "en": "Apa Itu Dekokerensi Kuantum?", "id": "Kehilangan Sifat Kuantum Akibat Lingkungan." },
  { "en": "Apa Itu Algoritma Shor?", "id": "Algoritma Kuantum Untuk Memfaktorkan Bilangan." },
  { "en": "Apa Itu Algoritma Grover?", "id": "Algoritma Kuantum Untuk Pencarian." },
  { "en": "Apa Itu Komputasi Neuromorfik?", "id": "Meniru Struktur Dan Fungsi Otak." },
  { "en": "Apa Itu Spiking Neural Network (SNN)?", "id": "Jaringan Saraf Yang Berkomunikasi Dengan Pulsa." },
  { "en": "Apa Itu Memristor?", "id": "Elemen Sirkuit Keempat Dengan Memori." },
  { "en": "Apa Itu Set Instruksi Thumb?", "id": "Set Instruksi 16-bit ARM (Advanced RISC Machine)." },
  { "en": "Apa Itu Eksekusi Predicated?", "id": "Instruksi Dieksekusi Berdasarkan Kondisi Bendera." },
  { "en": "Apa Itu Gerakan Kondisional (Conditional Move)?", "id": "Memindahkan Data Berdasarkan Kondisi." },
  { "en": "Apa Itu Power Distribution Network (PDN)?", "id": "Jaringan Untuk Mendistribusikan Daya Di Chip." },
  { "en": "Apa Itu Voltage Droop (IR Drop)?", "id": "Penurunan Tegangan Akibat Resistansi." },
  { "en": "Apa Itu Low-Dropout (LDO) Regulator?", "id": "Regulator Tegangan Linier Efisiensi Tinggi." },
  { "en": "Apa Itu Hardware Trojan?", "id": "Sirkuit Jahat Yang Dimasukkan Ke Chip." },
  { "en": "Apa Itu Side-Channel Attack?", "id": "Serangan Berdasarkan Informasi Fisik." },
  { "en": "Apa Itu Analisis Daya?", "id": "Menganalisis Konsumsi Daya Untuk Mendapatkan Kunci." },
  { "en": "Apa Itu Analisis Waktu?", "id": "Menganalisis Waktu Komputasi Untuk Mendapatkan Kunci." },
  { "en": "Apa Itu Physically Unclonable Function (PUF)?", "id": "Fungsi Yang Bergantung Pada Variasi Fisik." },
  { "en": "Untuk Apa PUF (Physically Unclonable Function) Digunakan?", "id": "Untuk Identifikasi Chip Dan Kunci Kriptografi." },
  { "en": "Apa Itu Cache Inklusif?", "id": "L2 Berisi Semua Data L1." },
  { "en": "Apa Itu Cache Eksklusif?", "id": "L2 Tidak Berisi Data L1." },
  { "en": "Apa Itu Cache Non-Inklusif?", "id": "Tidak Ada Aturan Inklusi." },
  { "en": "Apa Itu Valid Bit?", "id": "Menunjukkan Apakah Baris Cache Valid." },
  { "en": "Apa Itu Dirty Bit?", "id": "Menunjukkan Apakah Baris Cache Dimodifikasi." },
  { "en": "Apa Itu Set Instruksi Ortogonal?", "id": "Setiap Instruksi Dapat Menggunakan Setiap Mode." },
  { "en": "Apa Itu Siklus Mesin?", "id": "Siklus Clock Dasar Prosesor." },
  { "en": "Apa Itu State Machine?", "id": "Model Matematis Dari Komputasi." },
  { "en": "Apa Itu Finite State Machine (FSM)?", "id": "Mesin Keadaan Dengan Jumlah Keadaan Terbatas." },
  { "en": "Apa Itu Moore Machine?", "id": "Output Hanya Bergantung Pada Keadaan." },
  { "en": "Apa Itu Mealy Machine?", "id": "Output Bergantung Pada Keadaan Dan Input." },
  { "en": "Apa Itu Dekoder Instruksi?", "id": "Menerjemahkan Opcode Menjadi Sinyal Kontrol." },
  { "en": "Apa Itu Jalur Data?", "id": "Elemen Fungsional Yang Memproses Data." },
  { "en": "Apa Itu Register File?", "id": "Kumpulan Register Di Dalam CPU." },
  { "en": "Apa Itu Read Port?", "id": "Port Untuk Membaca Dari Register File." },
  { "en": "Apa Itu Write Port?", "id": "Port Untuk Menulis Ke Register File." },
  { "en": "Apa Itu Bypass Network?", "id": "Jalur Untuk Meneruskan Data." },
  { "en": "Apa Itu Jalur Kritis (Critical Path)?", "id": "Jalur Propagasi Terlama Dalam Sirkuit." },
  { "en": "Apa Yang Menentukan Frekuensi Clock Maksimum?", "id": "Penundaan Jalur Kritis." },
  { "en": "Apa Itu Timing Analysis?", "id": "Menganalisis Waktu Penundaan Dalam Sirkuit." },
  { "en": "Apa Itu Setup Time?", "id": "Waktu Data Harus Stabil Sebelum Clock." },
  { "en": "Apa Itu Hold Time?", "id": "Waktu Data Harus Stabil Setelah Clock." },
  { "en": "Apa Itu Clock Skew?", "id": "Perbedaan Waktu Tiba Sinyal Clock." },
  { "en": "Apa Itu Clock Jitter?", "id": "Variasi Periodik Dari Tepi Clock." },
  { "en": "Apa Itu Clock Tree?", "id": "Jaringan Untuk Mendistribusikan Sinyal Clock." },
  { "en": "Apa Itu Buffer?", "id": "Gerbang Logika Untuk Menguatkan Sinyal." },
  { "en": "Apa Itu Fanout?", "id": "Jumlah Input Gerbang Yang Digerakkan." },
  { "en": "Apa Itu Fanin?", "id": "Jumlah Input Ke Gerbang." },
  { "en": "Apa Itu Hambatan Parasitik?", "id": "Resistansi, Kapasitansi, Induktansi Yang Tidak Diinginkan." },
  { "en": "Apa Itu Arus Bocor (Leakage)?", "id": "Arus Yang Mengalir Saat Transistor Mati." },
  { "en": "Apa Itu Daya Statis?", "id": "Daya Yang Dikonsumsi Akibat Arus Bocor." },
  { "en": "Apa Itu Daya Dinamis?", "id": "Daya Yang Dikonsumsi Saat Transistor Beralih." },
  { "en": "Apa Itu Arithmetic Shift?", "id": "Pergeseran Bit Yang Mempertahankan Bit Tanda." },
  { "en": "Apa Itu Logical Shift?", "id": "Pergeseran Bit Yang Mengisi Dengan Nol." },
  { "en": "Apa Itu Rotate Operation?", "id": "Pergeseran Bit Secara Melingkar." },
  { "en": "Apa Itu Immediate Operand?", "id": "Operand Yang Langsung Dikodekan." },
  { "en": "Apa Itu Memory-to-Memory Architecture?", "id": "Memungkinkan Operasi Langsung Antar Lokasi Memori." },
  { "en": "Apa Itu Register-to-Register Architecture?", "id": "Operasi Terutama Antara Register." },
  { "en": "Apa Itu Mode Akses Memori?", "id": "Cara Instruksi Mengakses Memori." },
  { "en": "Apa Itu Aligned Access?", "id": "Akses Memori Di Alamat Yang Benar." },
  { "en": "Apa Itu Unaligned Access?", "id": "Akses Memori Di Alamat Tidak Benar." },
  { "en": "Apa Itu Atomic Operation?", "id": "Operasi Yang Tidak Dapat Diinterupsi." },
  { "en": "Apa Itu Test-and-Set Instruction?", "id": "Instruksi Atomik Untuk Sinkronisasi." },
  { "en": "Apa Itu Semaphore?", "id": "Variabel Untuk Mengontrol Akses." },
  { "en": "Apa Itu Mutex (Mutual Exclusion)?", "id": "Mekanisme Untuk Mencegah Akses Serentak." },
  { "en": "Apa Itu Critical Section?", "id": "Bagian Kode Yang Mengakses Sumber Daya." },
  { "en": "Apa Itu Race Condition?", "id": "Hasil Bergantung Pada Urutan Eksekusi." },
  { "en": "Apa Itu Livelock?", "id": "Proses Sibuk Tetapi Tidak Membuat Kemajuan." },
  { "en": "Apa Itu Starvation?", "id": "Proses Tidak Pernah Mendapatkan Sumber Daya." },
  { "en": "Apa Itu Priority Inversion?", "id": "Tugas Prioritas Tinggi Menunggu Tugas Rendah." },
  { "en": "Apa Itu Interrupt Latency?", "id": "Waktu Tunda Untuk Melayani Interupsi." },
  { "en": "Apa Itu Non-Maskable Interrupt (NMI)?", "id": "Interupsi Prioritas Sangat Tinggi." },
  { "en": "Apa Itu Vectored Interrupt?", "id": "Interupsi Yang Langsung Memberikan Alamat." },
  { "en": "Apa Itu Polled Interrupt?", "id": "CPU Harus Memeriksa Perangkat." },
  { "en": "Apa Itu Cache Aliasing?", "id": "Alamat Virtual Berbeda Dipetakan." },
  { "en": "Apa Itu Superblok?", "id": "Kombinasi Beberapa Blok Cache." },
  { "en": "Apa Itu Cache Sinomin?", "id": "Nama Lain Untuk Cache Aliasing." },
  { "en": "Apa Itu TLB (Translation Lookaside Buffer) Shootdown?", "id": "Proses Invalidate Entri TLB." },
  { "en": "Apa Itu Halaman Besar (Huge Pages)?", "id": "Menggunakan Ukuran Halaman Memori Lebih Besar." },
  { "en": "Apa Keuntungan Halaman Besar?", "id": "Mengurangi Tekanan Pada TLB." },
  { "en": "Apa Itu Bank Memori?", "id": "Bagian Independen Dari Memori DRAM." },
  { "en": "Apa Itu Interleaving Memori?", "id": "Menyebarkan Alamat Di Seluruh Bank Memori." },
  { "en": "Apa Itu Peringkat Memori (Memory Rank)?", "id": "Sekelompok Chip DRAM Yang Berbagi Bus." },
  { "en": "Apa Itu Modul Memori?", "id": "Papan Sirkuit Kecil Yang Berisi Chip." },
  { "en": "Apa Itu DIMM (Dual In-line Memory Module)?", "id": "Jenis Umum Modul Memori." },
  { "en": "Apa Itu SO-DIMM (Small Outline DIMM)?", "id": "Modul Memori Yang Lebih Kecil Untuk Laptop." },
  { "en": "Apa Itu Parity Bit?", "id": "Bit Tambahan Untuk Deteksi Kesalahan." },
  { "en": "Apa Itu ECC (Error-Correcting Code)?", "id": "Dapat Mendeteksi Dan Memperbaiki Kesalahan." },
  { "en": "Apa Itu Chipkill?", "id": "Bentuk Lanjutan Dari ECC (Error-Correcting Code)." },
  { "en": "Apa Itu Kontroler I/O (Input/Output)?", "id": "Mengelola Komunikasi Dengan Perangkat Periferal." },
  { "en": "Apa Itu Port?", "id": "Antarmuka Fisik Untuk Koneksi Periferal." },
  { "en": "Apa Itu Protokol Bus?", "id": "Aturan Komunikasi Di Atas Bus." },
  { "en": "Apa Itu Transfer Blok?", "id": "Mentransfer Blok Data Besar." },
  { "en": "Apa Itu Cycly Stealing?", "id": "DMA (Direct Memory Access) Menggunakan Bus." },
  { "en": "Apa Itu Mode Burst?", "id": "DMA (Direct Memory Access) Mengambil Kendali Bus." },
  { "en": "Apa Itu Komputasi Grid?", "id": "Menghubungkan Sumber Daya Terdistribusi." },
  { "en": "Apa Itu Komputasi Awan (Cloud)?", "id": "Menyediakan Layanan Melalui Internet." },
  { "en": "Apa Itu FPGA (Field-Programmable Gate Array)?", "id": "Sirkuit Terpadu Yang Dapat Diprogram." },
  { "en": "Apa Itu Reconfigurable Computing?", "id": "Perangkat Keras Yang Dapat Diubah." },
  { "en": "Apa Itu Arsitektur Dataflow?", "id": "Eksekusi Digerakkan Oleh Ketersediaan Data." },
  { "en": "Apa Itu Arsitektur Pengurangan Graf?", "id": "Model Eksekusi Fungsional." },
  { "en": "Apa Itu Pemrosesan Aliran (Stream Processing)?", "id": "Memproses Aliran Data Secara Berkelanjutan." },
  { "en": "Apa Itu Click Modular Router?", "id": "Arsitektur Perangkat Lunak Untuk Router." },
  { "en": "Apa Itu Bahasa Tingkat Tinggi?", "id": "Bahasa Pemrograman Yang Abstrak." },
  { "en": "Apa Itu Bahasa Tingkat Rendah?", "id": "Bahasa Pemrograman Dekat Perangkat Keras." },
  { "en": "Apa Itu Pass Kompiler?", "id": "Satu Tahap Dalam Proses Kompilasi." },
  { "en": "Apa Itu Analisis Leksikal?", "id": "Memecah Kode Menjadi Token." },
  { "en": "Apa Itu Analisis Sintaks?", "id": "Memeriksa Struktur Tata Bahasa Kode." },
  { "en": "Apa Itu Analisis Semantik?", "id": "Memeriksa Makna Kode." },
  { "en": "Apa Itu Pohon Sintaks Abstrak (AST)?", "id": "Representasi Pohon Dari Kode Sumber." },
  { "en": "Apa Itu Kode Menengah?", "id": "Representasi Antara Kode Sumber Dan Mesin." },
  { "en": "Apa Itu Pengoptimalan?", "id": "Meningkatkan Kinerja Kode." },
  { "en": "Apa Itu Pembangkitan Kode?", "id": "Menghasilkan Kode Mesin Atau Assembly." },
  { "en": "Apa Itu Tabel Simbol?", "id": "Struktur Data Yang Digunakan Kompiler." },
  { "en": "Apa Itu Pemanggilan Fungsi?", "id": "Mentransfer Kontrol Ke Sebuah Fungsi." },
  { "en": "Apa Itu Stack Frame?", "id": "Area Memori Untuk Variabel Lokal." },
  { "en": "Apa Itu Konvensi Pemanggilan?", "id": "Aturan Untuk Panggilan Fungsi." },
  { "en": "Apa Itu Heap?", "id": "Area Memori Untuk Alokasi Dinamis." },
  { "en": "Apa Itu Alokasi Dinamis?", "id": "Mengalokasikan Memori Saat Run-time." },
  { "en": "Apa Itu Pengumpul Sampah (Garbage Collection)?", "id": "Membebaskan Memori Secara Otomatis." },
  { "en": "Apa Itu Kebocoran Memori (Memory Leak)?", "id": "Memori Yang Dialokasikan Tapi Tidak." },
  { "en": "Apa Itu Pointer Menggantung (Dangling Pointer)?", "id": "Pointer Ke Memori Yang Sudah." },
  { "en": "Apa Itu Sirkuit Terpadu (IC)?", "id": "Satu Set Sirkuit Elektronik." },
  { "en": "Apa Itu Fabrikasi Semikonduktor?", "id": "Proses Pembuatan Sirkuit Terpadu." },
  { "en": "Apa Itu Wafer?", "id": "Irisan Tipis Bahan Semikonduktor." },
  { "en": "Apa Itu Die?", "id": "Satu Chip Individual Di Atas Wafer." },
  { "en": "Apa Itu Fotolitografi?", "id": "Proses Transfer Pola Menggunakan Cahaya." },
  { "en": "Apa Itu Etsa?", "id": "Menghilangkan Material Secara Selektif." },
  { "en": "Apa Itu Doping?", "id": "Memasukkan Pengotor Ke Semikonduktor." },
  { "en": "Apa Itu Deposisi?", "id": "Menumbuhkan Lapisan Tipis Material." },
  { "en": "Apa Itu Yield?", "id": "Persentase Die Yang Berfungsi." },
  { "en": "Apa Itu Desain Untuk Manufaktur (DFM)?", "id": "Mendesain Untuk Kemudahan Produksi." },
  { "en": "Apa Itu Desain Untuk Pengujian (DFT)?", "id": "Mendesain Agar Mudah Diuji." },
  { "en": "Apa Itu Scan Chain?", "id": "Menghubungkan Flip-Flop Untuk Pengujian." },
  { "en": "Apa Itu BIST (Built-In Self-Test)?", "id": "Sirkuit Menguji Dirinya Sendiri." },
  { "en": "Apa Itu JTAG (Joint Test Action Group)?", "id": "Standar Untuk Antarmuka Uji." },
  { "en": "Apa Itu Boundary Scan?", "id": "Metode Pengujian Papan Sirkuit." },
  { "en": "Apa Itu Prosesor RISC-V?", "id": "Set Instruksi Terbuka." },
  { "en": "Apa Itu Arsitektur ARM (Advanced RISC Machine)?", "id": "Arsitektur Prosesor Yang Sangat Populer." },
  { "en": "Apa Itu Arsitektur x86?", "id": "Arsitektur Prosesor Dominan Di PC." },
  { "en": "Apa Itu Moore's Law is Dead?", "id": "Perlambatan Laju Peningkatan Kepadatan." },
  { "en": "Apa Itu Komputasi Kognitif?", "id": "Sistem Yang Meniru Pemikiran Manusia." },
  { "en": "Apa Itu Tensor?", "id": "Array Multi-dimensi." },
  { "en": "Apa Itu Deep Learning Accelerator?", "id": "Perangkat Keras Untuk Jaringan Saraf." },
  { "en": "Apa Itu Cache Slice?", "id": "Bagian Dari Cache L3." },
  { "en": "Apa Itu Ring Bus?", "id": "Jaringan Interkoneksi Cincin Di Dalam Chip." },
  { "en": "Apa Itu Turbo Boost?", "id": "Meningkatkan Frekuensi Clock CPU Secara." },
  { "en": "Apa Itu Precision Boost?", "id": "Teknologi Turbo Serupa Dari AMD." },
  { "en": "Apa Itu Infinity Fabric?", "id": "Jaringan Interkoneksi Milik AMD." },
  { "en": "Apa Itu Chiplet?", "id": "Die Kecil Yang Digabungkan." },
  { "en": "Apa Keuntungan Desain Chiplet?", "id": "Yield Lebih Baik Dan Fleksibilitas." },
  { "en": "Apa Itu 2.5D Integration?", "id": "Menumpuk Die Di Atas Interposer." },
  { "en": "Apa Itu Interposer?", "id": "Lapisan Koneksi Antara Die." },
  { "en": "Apa Itu 3D Integration?", "id": "Menumpuk Die Secara Vertikal." },
  { "en": "Apa Itu Through-Silicon Via (TSV)?", "id": "Koneksi Vertikal Melalui Die." },
  { "en": "Apa Itu High Bandwidth Memory (HBM)?", "id": "Antarmuka RAM 3D." },
  { "en": "Apa Itu Kuantisasi Dalam AI?", "id": "Mengurangi Presisi Bobot Jaringan Saraf." },
  { "en": "Apa Itu Binarisasi Dalam AI?", "id": "Menggunakan Bobot Hanya +1 Dan -1." },
  { "en": "Apa Itu Pemangkasan (Pruning) Dalam AI?", "id": "Menghapus Bobot Yang Tidak Penting." },
  { "en": "Apa Itu Komputasi In-Memory?", "id": "Melakukan Komputasi Di Tempat Data Disimpan." },
  { "en": "Apa Itu Arsitektur Berorientasi Aliran?", "id": "Dioptimalkan Untuk Aliran Data." },
  { "en": "Apa Itu Komputasi Dekat Memori?", "id": "Menempatkan Prosesor Sangat Dekat Memori." },
  { "en": "Apa Itu Memori Resistif (RRAM)?", "id": "Jenis Memori Non-Volatile." },
  { "en": "Apa Itu Memori Perubahan Fasa (PCM)?", "id": "Memori Berbasis Perubahan Fasa Material." },
  { "en": "Apa Itu MRAM (Magnetoresistive RAM)?", "id": "Memori Berbasis Efek Magnetik." },
  { "en": "Apa Itu Komputasi Reversibel?", "id": "Komputasi Yang Secara Teoritis." },
  { "en": "Apa Itu Prinsip Landauer?", "id": "Batas Bawah Energi Untuk Komputasi." },
  { "en": "Apa Itu Komputasi Adiabatik?", "id": "Teknik Untuk Mengurangi Disipasi Daya." },
  { "en": "Apa Itu Clockless Logic?", "id": "Logika Asinkron." },
  { "en": "Apa Itu Komputasi Probabilistik?", "id": "Menggunakan Probabilitas Sebagai Bagian Komputasi." },
  { "en": "Apa Itu Bit Stokastik?", "id": "Merepresentasikan Angka Sebagai Aliran Bit." },
  { "en": "Apa Itu Komputasi Aproksimasi?", "id": "Mengorbankan Akurasi Untuk Kinerja." },
  { "en": "Kapan Komputasi Aproksimasi Berguna?", "id": "Dalam Aplikasi Toleran Kesalahan." },
  { "en": "Apa Itu Sirkuit Terpadu Fotonik?", "id": "Sirkuit Yang Menggunakan Cahaya." },
  { "en": "Apa Itu Interkoneksi Optik?", "id": "Menggunakan Cahaya Untuk Komunikasi Antar Chip." },
  { "en": "Apa Itu Mesin Analitik?", "id": "Desain Komputer Mekanis Awal." },
  { "en": "Siapa Yang Merancang Mesin Analitik?", "id": "Charles Babbage." },
  { "en": "Apa Itu ENIAC (Electronic Numerical Integrator and Computer)?", "id": "Komputer Elektronik Tujuan Umum Pertama." },
  { "en": "Apa Itu Arsitektur IAS (Institute for Advanced Study)?", "id": "Arsitektur Komputer Stored-Program Awal." },
  { "en": "Siapa Yang Mengusulkan Arsitektur IAS?", "id": "John von Neumann." },
  { "en": "Apa Itu Mode Akses Memori?", "id": "Cara Instruksi Mengakses Operand Memori." },
  { "en": "Apa Itu Pengalamatan Relatif PC?", "id": "Alamat Operand Relatif Terhadap Program Counter." },
  { "en": "Apa Itu Pengalamatan Base-plus-Offset?", "id": "Alamat = Register Basis + Offset." },
  { "en": "Apa Itu Jalur Data (Datapath)?", "id": "Kumpulan Unit Fungsional." },
  { "en": "Apa Itu Elemen Jalur Data?", "id": "ALU, Register, Multiplexer." },
  { "en": "Apa Itu Kontrol Hardwired?", "id": "Unit Kontrol Berbasis Logika Kombinasional." },
  { "en": "Apa Itu Kontrol Mikroprogram?", "id": "Unit Kontrol Berbasis Urutan Mikroinstruksi." },
  { "en": "Apa Itu Memori Kontrol?", "id": "Memori Yang Menyimpan Mikroprogram." },
  { "en": "Apa Itu Mikroinstruksi?", "id": "Perintah Level Rendah Untuk Kontrol." },
  { "en": "Apa Keuntungan Kontrol Mikroprogram?", "id": "Fleksibel Untuk Mengubah Set Instruksi." },
  { "en": "Apa Kerugian Kontrol Mikroprogram?", "id": "Lebih Lambat Dibanding Kontrol Hardwired." },
  { "en": "Apa Itu Pemformatan Instruksi?", "id": "Tata Letak Bit Dalam Instruksi." },
  { "en": "Apa Itu Instruksi Panjang Tetap?", "id": "Semua Instruksi Memiliki Panjang Sama." },
  { "en": "Apa Itu Instruksi Panjang Variabel?", "id": "Panjang Instruksi Dapat Bervariasi." },
  { "en": "Apa Itu Kode Huffman?", "id": "Metode Pengkodean Untuk Kompresi." },
  { "en": "Apa Itu Instruksi NOP (No Operation)?", "id": "Instruksi Yang Tidak Melakukan Apa-apa." },
  { "en": "Apa Kegunaan Instruksi NOP?", "id": "Untuk Penyelarasan Waktu Atau Mengisi Pipa." },
  { "en": "Apa Itu Port I/O (Input/Output)?", "id": "Register Antarmuka Untuk Perangkat." },
  { "en": "Apa Itu Sirkuit Terpadu?", "id": "Banyak Transistor Dalam Satu Chip." },
  { "en": "Apa Itu Skala Integrasi?", "id": "Jumlah Transistor Per Chip." },
  { "en": "Apa Itu SSI (Small-Scale Integration)?", "id": "Kurang Dari 100 Transistor." },
  { "en": "Apa Itu MSI (Medium-Scale Integration)?", "id": "Ratusan Transistor." },
  { "en": "Apa Itu LSI (Large-Scale Integration)?", "id": "Ribuan Transistor." },
  { "en": "Apa Itu VLSI (Very Large-Scale Integration)?", "id": "Jutaan Hingga Milyaran Transistor." },
  { "en": "Apa Itu ULSI (Ultra-Large-Scale Integration)?", "id": "Lebih Dari Satu Juta Transistor." },
  { "en": "Apa Itu Hukum Rock?", "id": "Biaya Pabrik Semikonduktor Berlipat Ganda." },
  { "en": "Apa Itu Wafer?", "id": "Cakram Silikon Untuk Membuat Chip." },
  { "en": "Apa Itu Yield?", "id": "Persentase Chip Yang Berfungsi." },
  { "en": "Apa Hubungan Ukuran Die Dan Yield?", "id": "Die Lebih Kecil Menghasilkan Yield Lebih Baik." },
  { "en": "Apa Itu Redundansi?", "id": "Menambahkan Komponen Cadangan Untuk Toleransi." },
  { "en": "Apa Itu ECC (Error-Correcting Code) On-Chip?", "id": "ECC Untuk Melindungi Cache Dan Register." },
  { "en": "Apa Itu Soft Error?", "id": "Kesalahan Transien Akibat Partikel Alfa." },
  { "en": "Apa Itu Hard Error?", "id": "Kerusakan Fisik Permanen." },
  { "en": "Apa Itu MTTR (Mean Time To Repair)?", "id": "Waktu Rata-rata Untuk Memperbaiki." },
  { "en": "Apa Rumus Availability?", "id": "MTTF Dibagi (MTTF Ditambah MTTR)." },
  { "en": "Apa Itu Dependability?", "id": "Kepercayaan Terhadap Layanan Sistem." },
  { "en": "Apa Aspek Dependability?", "id": "Availability, Reliability, Safety, Security." },
  { "en": "Apa Itu Safety?", "id": "Tidak Adanya Konsekuensi Bencana." },
  { "en": "Apa Itu Security?", "id": "Ketahanan Terhadap Intrusi." },
  { "en": "Apa Itu Throughput?", "id": "Jumlah Pekerjaan Selesai Per Waktu." },
  { "en": "Apa Itu Waktu Respon?", "id": "Waktu Dari Awal Hingga Selesai." },
  { "en": "Apa Itu CPU (Central Processing Unit) Time?", "id": "Waktu CPU Aktif Mengerjakan Tugas." },
  { "en": "Apa Itu Waktu Pengguna?", "id": "Waktu CPU Mengerjakan Kode Pengguna." },
  { "en": "Apa Itu Waktu Sistem?", "id": "Waktu CPU Mengerjakan Kode Kernel." },
  { "en": "Apa Itu Kecepatan Clock?", "id": "Laju Siklus Clock Per Detik." },
  { "en": "Apa Itu Benchmark Sintetis?", "id": "Program Kecil Untuk Mengukur Kinerja." },
  { "en": "Apa Itu Benchmark Kernel?", "id": "Bagian Inti Dari Program Nyata." },
  { "en": "Apa Itu Benchmark Mainan (Toy)?", "id": "Program Kecil Yang Dikenal." },
  { "en": "Apa Itu Program Dhrystone?", "id": "Benchmark Kinerja Integer." },
  { "en": "Apa Itu Program Whetstone?", "id": "Benchmark Kinerja Floating-Point." },
  { "en": "Apa Itu Rata-rata Aritmetika?", "id": "Jumlah Dibagi Jumlah Item." },
  { "en": "Apa Itu Rata-rata Tertimbang?", "id": "Rata-rata Dengan Bobot." },
  { "en": "Apa Itu Rata-rata Geometris?", "id": "Digunakan Untuk Menormalkan Rasio." },
  { "en": "Apa Itu Rata-rata Harmonis?", "id": "Digunakan Untuk Menghitung Rata-rata Laju." },
  { "en": "Apa Itu Little's Law?", "id": "Menghubungkan Jumlah Item, Kedatangan, Waktu." },
  { "en": "Apa Itu Teori Antrian?", "id": "Studi Matematika Tentang Garis Tunggu." },
  { "en": "Apa Itu Server?", "id": "Unit Yang Memberikan Layanan." },
  { "en": "Apa Itu Pelanggan?", "id": "Unit Yang Meminta Layanan." },
  { "en": "Apa Itu Distribusi Kedatangan?", "id": "Pola Kedatangan Pelanggan." },
  { "en": "Apa Itu Distribusi Layanan?", "id": "Pola Waktu Pelayanan." },
  { "en": "Apa Itu Notasi Kendall?", "id": "Notasi Standar Untuk Sistem Antrian." },
  { "en": "Apa Itu M/M/1?", "id": "Antrian Markovian Dengan Satu Server." },
  { "en": "Apa Itu Pemrosesan Batch?", "id": "Mengeksekusi Pekerjaan Tanpa Interaksi." },
  { "en": "Apa Itu Time-Sharing?", "id": "Berbagi Waktu CPU Di Antara Banyak." },
  { "en": "Apa Itu Quantum Waktu (Time Slice)?", "id": "Waktu Yang Dialokasikan Untuk Proses." },
  { "en": "Apa Itu Penjadwalan Round-Robin?", "id": "Setiap Proses Mendapat Giliran." },
  { "en": "Apa Itu Penjadwalan Prioritas?", "id": "Proses Prioritas Tinggi Dijalankan Dahulu." },
  { "en": "Apa Itu Starvation?", "id": "Proses Prioritas Rendah Tidak Pernah." },
  { "en": "Apa Itu Aging?", "id": "Meningkatkan Prioritas Proses Seiring Waktu." },
  { "en": "Apa Itu Arsitektur Set Instruksi (ISA)?", "id": "Antarmuka Perangkat Keras/Lunak." },
  { "en": "Apa Itu Kelas ISA (Instruction Set Architecture)?", "id": "Register-Memory, Load-Store." },
  { "en": "Apa Itu Operand Implisit?", "id": "Operand Yang Tidak Disebutkan Secara Eksplisit." },
  { "en": "Apa Itu Kode Operasi?", "id": "Bagian Instruksi Yang Menentukan Operasi." },
  { "en": "Apa Itu Mode Pengalamatan?", "id": "Cara Menentukan Alamat Operand." },
  { "en": "Apa Itu Format Instruksi?", "id": "Tata Letak Bit Instruksi." },
  { "en": "Apa Itu Panjang Instruksi?", "id": "Jumlah Bit Dalam Instruksi." },
  { "en": "Apa Itu Orthogonality?", "id": "Setiap Instruksi Dapat Menggunakan Setiap." },
  { "en": "Apa Itu Barrel Shifter?", "id": "Sirkuit Untuk Pergeseran Bit." },
  { "en": "Apa Itu Floating Point?", "id": "Representasi Bilangan Real." },
  { "en": "Apa Itu Presisi?", "id": "Jumlah Bit Dalam Mantisa." },
  { "en": "Apa Itu Rentang?", "id": "Rentang Nilai Yang Dapat Direpresentasikan." },
  { "en": "Apa Itu Gradual Underflow?", "id": "Memungkinkan Representasi Bilangan Denormalized." },
  { "en": "Apa Itu Fused Multiply-Add (FMA)?", "id": "Operasi (A x B) + C." },
  { "en": "Apa Itu Sticky Bit?", "id": "Membantu Dalam Pembulatan Yang Tepat." },
  { "en": "Apa Itu Instruksi SIMD (Single Instruction, Multiple Data)?", "id": "Operasi Paralel Pada Data." },
  { "en": "Apa Itu Subword Parallelism?", "id": "Nama Lain Untuk SIMD." },
  { "en": "Apa Itu Instruksi Conditional Move?", "id": "Memindahkan Data Berdasarkan Kondisi." },
  { "en": "Apa Itu Predication?", "id": "Mengubah Instruksi Menjadi Kondisional." },
  { "en": "Apa Itu Kompiler Just-In-Time (JIT)?", "id": "Mengkompilasi Kode Saat Program Dijalankan." },
  { "en": "Apa Itu Ahead-Of-Time (AOT) Compilation?", "id": "Mengkompilasi Kode Sebelum Eksekusi." },
  { "en": "Apa Itu Kode Portabel?", "id": "Kode Yang Dapat Dijalankan Di Berbagai Arsitektur." },
  { "en": "Apa Itu Bytecode?", "id": "Contoh Kode Portabel." },
  { "en": "Apa Itu Java Virtual Machine (JVM)?", "id": "Mesin Virtual Yang Menjalankan Bytecode Java." },
  { "en": "Apa Itu Common Language Runtime (CLR)?", "id": "Mesin Virtual Untuk Kerangka .NET." },
  { "en": "Apa Itu Antarmuka Biner Aplikasi (ABI)?", "id": "Standar Untuk Interoperabilitas Biner." },
  { "en": "Apa Yang Diatur ABI (Application Binary Interface)?", "id": "Konvensi Pemanggilan, Ukuran Tipe, Tata Letak." },
  { "en": "Apa Itu Pipeline Dinamis?", "id": "Pipeline Dengan Penjadwalan Instruksi Dinamis." },
  { "en": "Apa Itu Scoreboarding?", "id": "Mekanisme Kontrol Terpusat Untuk Pipa Dinamis." },
  { "en": "Di Komputer Mana Scoreboarding Pertama Digunakan?", "id": "CDC (Control Data Corporation) 6600." },
  { "en": "Apa Itu Algoritma Tomasulo?", "id": "Skema Penjadwalan Dinamis Terdistribusi." },
  { "en": "Apa Keuntungan Algoritma Tomasulo?", "id": "Menghilangkan Hazard WAR Dan WAW." },
  { "en": "Apa Itu Common Data Bus (CDB)?", "id": "Menyiarkan Hasil Ke Unit Fungsional." },
  { "en": "Apa Itu Reservation Station?", "id": "Buffer Untuk Instruksi Yang Menunggu Operand." },
  { "en": "Apa Itu Reorder Buffer (ROB)?", "id": "Memastikan Instruksi Selesai Sesuai Urutan." },
  { "en": "Apa Itu Register Alias Table (RAT)?", "id": "Tabel Untuk Penggantian Nama Register." },
  { "en": "Apa Itu Out-of-Order Execution?", "id": "Mengeksekusi Instruksi Berdasarkan Ketersediaan." },
  { "en": "Apa Itu In-Order Commit?", "id": "Menyelesaikan Instruksi Sesuai Urutan Program." },
  { "en": "Apa Itu Speculative Execution?", "id": "Mengeksekusi Instruksi Secara Spekulatif." },
  { "en": "Apa Itu Branch Prediction?", "id": "Menebak Arah Percabangan." },
  { "en": "Apa Itu Prediktor Bimodal?", "id": "Menggunakan Dua Bit Untuk Prediksi." },
  { "en": "Apa Itu Prediktor Lokal?", "id": "Menggunakan Sejarah Lokal Setiap Percabangan." },
  { "en": "Apa Itu Prediktor Global?", "id": "Menggunakan Sejarah Global Semua Percabangan." },
  { "en": "Apa Itu Prediktor Turnamen?", "id": "Menggabungkan Beberapa Prediktor." },
  { "en": "Apa Itu Branch Target Buffer (BTB)?", "id": "Cache Untuk Alamat Tujuan Percabangan." },
  { "en": "Apa Itu Return Address Stack (RAS)?", "id": "Stack Untuk Memprediksi Alamat Kembali." },
  { "en": "Apa Itu Cache Instruksi?", "id": "Cache Khusus Untuk Menyimpan Instruksi." },
  { "en": "Apa Itu Cache Data?", "id": "Cache Khusus Untuk Menyimpan Data." },
  { "en": "Apa Itu Cache Terpadu?", "id": "Satu Cache Untuk Instruksi Dan Data." },
  { "en": "Apa Itu Kebijakan Inklusi Cache?", "id": "Aturan Tentang Konten Antar Level." },
  { "en": "Apa Itu Cache Inklusif?", "id": "Level Lebih Tinggi Adalah Superset." },
  { "en": "Apa Itu Cache Eksklusif?", "id": "Konten Antar Level Saling Eksklusif." },
  { "en": "Apa Itu Protokol Koherensi?", "id": "Memastikan Konsistensi Data Di Banyak Cache." },
  { "en": "Apa Itu Protokol MSI?", "id": "Modified, Shared, Invalid." },
  { "en": "Apa Itu Protokol MESI?", "id": "MSI Ditambah Keadaan Exclusive." },
  { "en": "Apa Itu Protokol MOESI?", "id": "MESI Ditambah Keadaan Owned." },
  { "en": "Apa Itu Snooping?", "id": "Cache Memantau Transaksi Bus." },
  { "en": "Apa Itu Direktori?", "id": "Struktur Data Terpusat Untuk Koherensi." },
  { "en": "Apa Itu Intervensi?", "id": "Cache Menyediakan Data Untuk Cache Lain." },
  { "en": "Apa Itu Cache-to-Cache Transfer?", "id": "Transfer Data Langsung Antar Cache." },
  { "en": "Apa Itu Write Barrier?", "id": "Mekanisme Untuk Menegakkan Urutan Tulis." },
  { "en": "Apa Itu Memory Barrier?", "id": "Nama Lain Untuk Pagar Memori." },
  { "en": "Apa Itu Kunci (Lock)?", "id": "Primitif Sinkronisasi." },
  { "en": "Apa Itu Spinlock?", "id": "Kunci Yang Menggunakan Busy-Waiting." },
  { "en": "Apa Itu Busy-Waiting?", "id": "Thread Terus Memeriksa Kondisi." },
  { "en": "Apa Itu Load-Link/Store-Conditional?", "id": "Instruksi Atomik Untuk Sinkronisasi." },
  { "en": "Apa Itu Compare-and-Swap (CAS)?", "id": "Instruksi Atomik Lainnya." },
  { "en": "Apa Itu Memori Non-Volatile?", "id": "Mempertahankan Data Tanpa Daya." },
  { "en": "Apa Itu Memori Flash?", "id": "Jenis Umum Memori Non-Volatile." },
  { "en": "Apa Itu NAND Flash?", "id": "Flash Dengan Kepadatan Tinggi." },
  { "en": "Apa Itu NOR Flash?", "id": "Flash Dengan Akses Baca Cepat." },
  { "en": "Apa Itu Solid-State Drive (SSD)?", "id": "Penyimpanan Berbasis Flash NAND." },
  { "en": "Apa Itu Flash Translation Layer (FTL)?", "id": "Perangkat Lunak Yang Mengelola Flash." },
  { "en": "Apa Itu Wear Leveling?", "id": "Mendistribusikan Penulisan Secara Merata." },
  { "en": "Apa Itu Garbage Collection?", "id": "Mengambil Kembali Blok Yang Tidak Valid." },
  { "en": "Apa Itu Write Amplification?", "id": "Penulisan Aktual Lebih Besar Dari Permintaan." },
  { "en": "Apa Itu TRIM Command?", "id": "Memberi Tahu SSD Blok Mana." },
  { "en": "Apa Itu Phase-Change Memory (PCM)?", "id": "Memori Non-Volatile Berbasis Perubahan Fasa." },
  { "en": "Apa Itu Resistive RAM (RRAM)?", "id": "Memori Berbasis Perubahan Resistansi." },
  { "en": "Apa Itu Magnetoresistive RAM (MRAM)?", "id": "Memori Berbasis Resistansi Magnetik." },
  { "en": "Apa Itu Ferroelectric RAM (FeRAM)?", "id": "Memori Berbasis Material Feroelektrik." },
  { "en": "Apa Itu Komputasi Tahan Lama (Persistent)?", "id": "Data Bertahan Setelah Gagal Daya." },
  { "en": "Apa Itu Asynchronous DRAM (Dynamic Random-Access Memory)?", "id": "DRAM Jenis Lama." },
  { "en": "Apa Itu Page Mode DRAM?", "id": "Meningkatkan Akses Ke Baris Yang Sama." },
  { "en": "Apa Itu EDO (Extended Data Out) DRAM?", "id": "Peningkatan Dari Page Mode DRAM." },
  { "en": "Apa Itu SDRAM (Synchronous DRAM)?", "id": "DRAM Yang Disinkronkan Dengan Bus." },
  { "en": "Apa Itu DDR (Double Data Rate) SDRAM?", "id": "Mentransfer Data Dua Kali Per Siklus." },
  { "en": "Apa Itu LPDDR (Low Power DDR)?", "id": "DDR Untuk Perangkat Seluler." },
  { "en": "Apa Itu GDDR (Graphics DDR)?", "id": "DDR Yang Dioptimalkan Untuk GPU." },
  { "en": "Apa Itu Command Rate?", "id": "Penundaan Antara Perintah Chip Select." },
  { "en": "Apa Itu RAS (Row Address Strobe) to CAS (Column Address Strobe) Delay?", "id": "Penundaan Waktu Penting DRAM." },
  { "en": "Apa Itu Precharge?", "id": "Perintah Untuk Menutup Baris DRAM." },
  { "en": "Apa Itu Bank?", "id": "Array Independen Di Dalam Chip." },
  { "en": "Apa Itu Rank?", "id": "Sekelompok Chip Yang Berbagi Bus." },
  { "en": "Apa Itu Memori Terdaftar (Registered)?", "id": "Memiliki Register Untuk Menyangga Sinyal." },
  { "en": "Apa Itu Load-Reduced DIMM (LRDIMM)?", "id": "Menggunakan Memory Buffer." },
  { "en": "Apa Itu Sirkuit Terpadu Tiga Dimensi?", "id": "Menumpuk Beberapa Die Silikon." },
  { "en": "Apa Itu Through-Silicon Via (TSV)?", "id": "Koneksi Vertikal Melalui Die." },
  { "en": "Apa Itu High Bandwidth Memory (HBM)?", "id": "Standar Memori Bertumpuk." },
  { "en": "Apa Itu Wide I/O?", "id": "Standar Memori Bertumpuk Lainnya." },
  { "en": "Apa Itu Optical Interconnect?", "id": "Menggunakan Cahaya Untuk Komunikasi." },
  { "en": "Apa Itu Silicon Photonics?", "id": "Membuat Komponen Optik Di Atas Silikon." },
  { "en": "Apa Itu Jaringan Interkoneksi Optik?", "id": "Menghubungkan Komponen Dengan Cahaya." },
  { "en": "Apa Itu Modulator?", "id": "Mengubah Sinyal Listrik Menjadi Optik." },
  { "en": "Apa Itu Detektor Foto?", "id": "Mengubah Sinyal Optik Menjadi Listrik." },
  { "en": "Apa Itu Komputasi Kuantum?", "id": "Menggunakan Fenomena Kuantum Untuk Komputasi." },
  { "en": "Apa Itu Qubit?", "id": "Bit Kuantum." },
  { "en": "Apa Itu Superposisi?", "id": "Kemampuan Qubit Menjadi 0 Dan 1." },
  { "en": "Apa Itu Entanglement?", "id": "Keterkaitan Antar Qubit." },
  { "en": "Apa Itu Gerbang Kuantum?", "id": "Operasi Pada Qubit." },
  { "en": "Apa Itu Komputasi Neuromorfik?", "id": "Meniru Arsitektur Otak." },
  { "en": "Apa Itu Synapse?", "id": "Koneksi Antara Neuron." },
  { "en": "Apa Itu Neuron?", "id": "Unit Pemrosesan Dasar." },
  { "en": "Apa Itu Spiking Neuron?", "id": "Neuron Yang Berkomunikasi Dengan Pulsa." },
  { "en": "Apa Itu Memristor?", "id": "Dapat Bertindak Sebagai Synapse Buatan." },
  { "en": "Apa Itu Komputasi In-Situ?", "id": "Memproses Data Di Tempatnya." },
  { "en": "Apa Itu Arsitektur Akselerator?", "id": "Menggunakan Perangkat Keras Khusus." },
  { "en": "Apa Itu Domain-Specific Language (DSL)?", "id": "Bahasa Pemrograman Untuk Domain Tertentu." },
  { "en": "Apa Itu Mesin Keadaan (State Machine)?", "id": "Model Komputasi Berdasarkan Keadaan." },
  { "en": "Apa Itu Pemrosesan Aliran Data?", "id": "Model Eksekusi Berbasis Aliran Data." },
  { "en": "Apa Itu Makro-operasi?", "id": "Operasi Tingkat Tinggi." },
  { "en": "Apa Itu Mikro-operasi?", "id": "Operasi Tingkat Rendah." },
  { "en": "Apa Itu Fusion Mikro-op?", "id": "Menggabungkan Mikro-operasi." },
  { "en": "Apa Itu Fusion Makro-op?", "id": "Menggabungkan Instruksi Assembly." },
  { "en": "Apa Itu Write Cache?", "id": "Cache Untuk Operasi Tulis." },
  { "en": "Apa Itu Write Buffer?", "id": "Menyimpan Data Tulis Sebelum Ke Memori." },
  { "en": "Apa Itu Snooping Pasif?", "id": "Cache Hanya Mendengarkan Bus." },
  { "en": "Apa Itu Snooping Aktif?", "id": "Cache Dapat Mengubah Status." },
  { "en": "Apa Itu Protokol MESIF?", "id": "MESI Ditambah Keadaan Forward." },
  { "en": "Apa Itu Protokol MERSI?", "id": "MESI Ditambah Keadaan Recent." },
  { "en": "Apa Itu Migrasi Halaman?", "id": "Memindahkan Halaman Memori." },
  { "en": "Kapan Migrasi Halaman Digunakan?", "id": "Dalam Sistem NUMA (Non-Uniform Memory Access)." },
  { "en": "Apa Itu First-Touch Policy?", "id": "Mengalokasikan Memori Dekat Prosesor." },
  { "en": "Apa Itu Kebijakan Pembulatan?", "id": "Aturan Untuk Membulatkan Bilangan Floating-Point." },
  { "en": "Apa Itu Pembulatan Ke Nol?", "id": "Membuang Bagian Pecahan." },
  { "en": "Apa Itu Pembulatan Ke Terdekat?", "id": "Membulatkan Ke Nilai Terdekat." },
  { "en": "Apa Itu Mode Denormal?", "id": "Menangani Bilangan Sangat Kecil." },
  { "en": "Apa Itu Pengecualian Floating-Point?", "id": "Kondisi Kesalahan (Overflow, Underflow)." },
  { "en": "Apa Itu Trapping?", "id": "Menyebabkan Pengecualian Untuk Ditangani." },
  { "en": "Apa Itu Nilai Default?", "id": "Mengembalikan Nilai Seperti NaN Atau Infinity." },
  { "en": "Apa Itu Mode Pengalamatan Register?", "id": "Operand Berada Di Register." },
  { "en": "Apa Itu Mode Pengalamatan Register Tidak Langsung?", "id": "Register Berisi Alamat Operand." },
  { "en": "Apa Itu Mode Pengalamatan Relatif Basis?", "id": "Alamat = Register Basis + Perpindahan." },
  { "en": "Apa Itu Mode Pengalamatan Berindeks?", "id": "Sama Dengan Relatif Basis." },
  { "en": "Apa Itu Alamat Efektif?", "id": "Alamat Akhir Dari Operand." },
  { "en": "Apa Itu Perhitungan Alamat Efektif?", "id": "Proses Menghitung Alamat Akhir." },
  { "en": "Apa Itu Dependensi Semu?", "id": "Dependensi Nama (WAR, WAW)." },
  { "en": "Apa Itu Dependensi Sejati?", "id": "Dependensi Aliran Data (RAW)." },
  { "en": "Apa Itu Antrian Instruksi?", "id": "Menyimpan Instruksi Setelah Diambil." },
  { "en": "Apa Itu Antrian Mikro-operasi?", "id": "Menyimpan Mikro-operasi Setelah Didekode." },
  { "en": "Apa Itu Retirement Stage?", "id": "Nama Lain Untuk Commit Stage." },
  { "en": "Apa Itu Exception Handler?", "id": "Kode Yang Menangani Pengecualian." },
  { "en": "Apa Itu Precise Interrupt?", "id": "Sinonim Untuk Precise Exception." },
  { "en": "Apa Itu Memory Protection Unit (MPU)?", "id": "Perangkat Keras Untuk Proteksi Memori." },
  { "en": "Apa Beda MPU (Memory Protection Unit) dan MMU (Memory Management Unit)?", "id": "MPU Lebih Sederhana, Tanpa Terjemahan." },
  { "en": "Di Mana MPU (Memory Protection Unit) Digunakan?", "id": "Di Mikrokontroler Dan Sistem Embedded." },
  { "en": "Apa Itu Context?", "id": "Keadaan Prosesor (Register)." },
  { "en": "Apa Itu Thread Control Block (TCB)?", "id": "Struktur Data Kernel Untuk Thread." },
  { "en": "Apa Itu Process Control Block (PCB)?", "id": "Struktur Data Kernel Untuk Proses." },
  { "en": "Apa Itu Eksekusi Spekulatif?", "id": "Mengeksekusi Instruksi Secara Optimis." },
  { "en": "Apa Itu Rollback?", "id": "Membatalkan Efek Eksekusi Spekulatif." },
  { "en": "Apa Itu Prosesor Dinamis?", "id": "Menggunakan Penjadwalan Instruksi Dinamis." },
  { "en": "Apa Itu Prosesor Statis?", "id": "Mengandalkan Penjadwalan Statis." },
  { "en": "Apa Itu Wide-Issue Processor?", "id": "Dapat Mengeluarkan Banyak Instruksi." },
  { "en": "Apa Itu Issue Width?", "id": "Jumlah Instruksi Maksimum Dikeluarkan." },
  { "en": "Apa Itu Completion Width?", "id": "Jumlah Instruksi Maksimum Diselesaikan." },
  { "en": "Apa Itu Prosesor Multi-Threaded?", "id": "Dapat Mengeksekusi Banyak Thread." },
  { "en": "Apa Itu Block Multithreading?", "id": "Beralih Thread Saat Stall." },
  { "en": "Apa Itu Interleaved Multithreading?", "id": "Beralih Thread Setiap Siklus." },
  { "en": "Apa Itu Simultaneous Multithreading (SMT)?", "id": "Mengeluarkan Instruksi Dari Banyak Thread." },
  { "en": "Apa Itu Sirkuit Terpadu?", "id": "Kumpulan Sirkuit Elektronik." },
  { "en": "Apa Itu Skala Integrasi?", "id": "Kepadatan Transistor Di Chip." },
  { "en": "Apa Itu Dicing?", "id": "Memotong Wafer Menjadi Die." },
  { "en": "Apa Itu Packaging?", "id": "Menempatkan Die Ke Dalam Wadah." },
  { "en": "Apa Itu Wire Bonding?", "id": "Menghubungkan Die Ke Kaki Paket." },
  { "en": "Apa Itu Flip-Chip?", "id": "Metode Pemasangan Die Langsung." },
  { "en": "Apa Itu Thermal Pad?", "id": "Area Untuk Transfer Panas." },
  { "en": "Apa Itu Heat Spreader?", "id": "Menyebarkan Panas Dari Die." },
  { "en": "Apa Itu Heat Sink?", "id": "Menghilangkan Panas Ke Lingkungan." },
  { "en": "Apa Itu Sistem Pendingin Udara?", "id": "Menggunakan Udara Untuk Mendinginkan." },
  { "en": "Apa Itu Sistem Pendingin Cair?", "id": "Menggunakan Cairan Untuk Mendinginkan." },
  { "en": "Apa Itu Cold Plate?", "id": "Piringan Logam Yang Didinginkan." },
  { "en": "Apa Itu Pendinginan Termoelektrik?", "id": "Menggunakan Efek Peltier." },
  { "en": "Apa Itu Pengalamatan Byte?", "id": "Setiap Byte Memiliki Alamat Unik." },
  { "en": "Apa Itu Pengalamatan Word?", "id": "Setiap Word Memiliki Alamat." },
  { "en": "Apa Itu Memory Interleaving?", "id": "Meningkatkan Kinerja Dengan Akses Paralel." },
  { "en": "Apa Itu Bank?", "id": "Modul Memori Independen." },
  { "en": "Apa Itu Channel?", "id": "Jalur Data Ke Memori." },
  { "en": "Apa Itu Memori Virtual?", "id": "Menggunakan Disk Sebagai RAM." },
  { "en": "Apa Itu Ukuran Halaman?", "id": "Ukuran Blok Dalam Memori Virtual." },
  { "en": "Apa Itu Fragmentasi?", "id": "Ruang Memori Yang Terbuang." },
  { "en": "Apa Itu Fragmentasi Internal?", "id": "Ruang Terbuang Di Dalam Blok." },
  { "en": "Apa Itu Fragmentasi Eksternal?", "id": "Ruang Terbuang Di Antara Blok." },
  { "en": "Apa Itu Paging?", "id": "Mengatasi Fragmentasi Eksternal." },
  { "en": "Apa Itu Segmentasi?", "id": "Membagi Memori Menjadi Segmen." },
  { "en": "Apa Itu Demand Paging?", "id": "Mengambil Halaman Hanya Saat Dibutuhkan." },
  { "en": "Apa Itu Prepaging?", "id": "Mengambil Halaman Sebelum Dibutuhkan." },
  { "en": "Apa Itu Belady's Anomaly?", "id": "Menambah Frame Menyebabkan Lebih Banyak." },
  { "en": "Apa Itu Algoritma Optimal?", "id": "Mengganti Halaman Yang Tidak Akan." },
  { "en": "Apa Itu RAID (Redundant Array of Independent Disks) Controller?", "id": "Mengelola Array Disk." },
  { "en": "Apa Itu Hardware RAID?", "id": "Menggunakan Kontroler Khusus." },
  { "en": "Apa Itu Software RAID?", "id": "Diimplementasikan Dalam Sistem Operasi." },
  { "en": "Apa Itu Network-Attached Storage (NAS)?", "id": "Penyimpanan Yang Terhubung Ke Jaringan." },
  { "en": "Apa Itu Storage Area Network (SAN)?", "id": "Jaringan Berkecepatan Tinggi Untuk Penyimpanan." },
  { "en": "Apa Itu Fibre Channel?", "id": "Protokol Untuk SAN (Storage Area Network)." },
  { "en": "Apa Itu iSCSI (Internet Small Computer System Interface)?", "id": "Protokol Penyimpanan Berbasis IP." },
  { "en": "Apa Itu Komputasi Paralel?", "id": "Menggunakan Banyak Prosesor Secara Bersamaan." },
  { "en": "Apa Itu Komputasi Terdistribusi?", "id": "Komputasi Di Atas Komputer Terdistribusi." },
  { "en": "Apa Itu Throughput?", "id": "Pekerjaan Selesai Per Satuan Waktu." },
  { "en": "Apa Itu Speedup?", "id": "Peningkatan Kinerja Dari Paralelisme." },
  { "en": "Apa Itu Efisiensi?", "id": "Speedup Dibagi Jumlah Prosesor." },
  { "en": "Apa Itu Skalabilitas?", "id": "Kemampuan Sistem Menangani Beban." },
  { "en": "Apa Itu Strong Scaling?", "id": "Mengurangi Waktu Dengan Prosesor." },
  { "en": "Apa Itu Weak Scaling?", "id": "Menambah Ukuran Masalah Dengan Prosesor." },
  { "en": "Apa Itu Hukum Gustafson?", "id": "Menggambarkan Speedup Untuk Masalah." },
  { "en": "Apa Itu Granularitas?", "id": "Ukuran Komputasi Relatif Terhadap Komunikasi." },
  { "en": "Apa Itu Fine-Grained Parallelism?", "id": "Tugas-tugas Kecil, Komunikasi Sering." },
  { "en": "Apa Itu Coarse-Grained Parallelism?", "id": "Tugas-tugas Besar, Komunikasi Jarang." },
  { "en": "Apa Itu Load Balancing?", "id": "Mendistribusikan Pekerjaan Secara Merata." },
  { "en": "Apa Itu Penjadwalan Statis?", "id": "Alokasi Tugas Ditetapkan Sebelum Eksekusi." },
  { "en": "Apa Itu Penjadwalan Dinamis?", "id": "Alokasi Tugas Terjadi Selama Eksekusi." },
  { "en": "Apa Itu Work Stealing?", "id": "Prosesor Menganggur Mengambil Tugas." },
  { "en": "Apa Itu Komputasi Klien-Server?", "id": "Model Aplikasi Terdistribusi." },
  { "en": "Apa Itu Klien Tipis (Thin Client)?", "id": "Klien Yang Bergantung Pada Server." },
  { "en": "Apa Itu Klien Gemuk (Fat Client)?", "id": "Klien Yang Melakukan Banyak Pemrosesan." },
  { "en": "Apa Itu Three-Tier Architecture?", "id": "Presentasi, Logika Aplikasi, Dan Lapisan Data." },
  { "en": "Apa Itu Middleware?", "id": "Perangkat Lunak Yang Menghubungkan Komponen." },
  { "en": "Apa Itu Remote Procedure Call (RPC)?", "id": "Mengeksekusi Prosedur Di Komputer Lain." },
  { "en": "Apa Itu Socket?", "id": "Titik Akhir Untuk Komunikasi Jaringan." },
  { "en": "Apa Itu Port?", "id": "Pengenal Numerik Untuk Proses." },
  { "en": "Apa Itu Protokol?", "id": "Aturan Untuk Komunikasi." },
  { "en": "Apa Itu Jaringan Overlay?", "id": "Jaringan Virtual Di Atas Jaringan Fisik." },
  { "en": "Apa Itu Peer-to-Peer (P2P)?", "id": "Jaringan Terdesentralisasi." },
  { "en": "Apa Itu Arsitektur Berorientasi Layanan (SOA)?", "id": "Membangun Aplikasi Dari Layanan." },
  { "en": "Apa Itu Layanan Web (Web Service)?", "id": "Layanan Yang Dapat Diakses Melalui Web." },
  { "en": "Apa Itu Representational State Transfer (REST)?", "id": "Gaya Arsitektur Untuk Layanan Web." },
  { "en": "Apa Itu Simple Object Access Protocol (SOAP)?", "id": "Protokol Pesan Berbasis XML." },
  { "en": "Apa Itu Microservices Architecture?", "id": "Membangun Aplikasi Dari Layanan Kecil." },
  { "en": "Apa Keuntungan Microservices?", "id": "Skalabilitas, Fleksibilitas, Dan Kemandirian." },
  { "en": "Apa Itu Application-Specific Instruction Set Processor (ASIP)?", "id": "Prosesor Dengan Set Instruksi Khusus." },
  { "en": "Apa Itu Co-processor?", "id": "Prosesor Tambahan Untuk Tugas Khusus." },
  { "en": "Contoh Co-processor Adalah Apa?", "id": "Floating-Point Unit (FPU)." },
  { "en": "Apa Itu Endianness?", "id": "Urutan Penyimpanan Byte." },
  { "en": "Apa Itu Bi-endian?", "id": "Arsitektur Yang Mendukung Kedua Endianness." },
  { "en": "Apa Itu Sistem Bilangan?", "id": "Cara Untuk Merepresentasikan Angka." },
  { "en": "Apa Itu Radiks?", "id": "Dasar Dari Sistem Bilangan." },
  { "en": "Apa Itu Biner?", "id": "Sistem Bilangan Berbasis 2." },
  { "en": "Apa Itu Oktal?", "id": "Sistem Bilangan Berbasis 8." },
  { "en": "Apa Itu Desimal?", "id": "Sistem Bilangan Berbasis 10." },
  { "en": "Apa Itu Heksadesimal?", "id": "Sistem Bilangan Berbasis 16." },
  { "en": "Apa Itu Konversi Basis?", "id": "Mengubah Representasi Antar Sistem." },
  { "en": "Apa Itu Binary Coded Decimal (BCD)?", "id": "Setiap Digit Desimal Dikodekan Biner." },
  { "en": "Apa Itu Kode Gray?", "id": "Kode Dimana Dua Nilai Berurutan." },
  { "en": "Apa Itu Kode ASCII (American Standard Code for Information Interchange)?", "id": "Standar Pengkodean Karakter." },
  { "en": "Apa Itu Unicode?", "id": "Standar Pengkodean Karakter Global." },
  { "en": "Apa Itu UTF-8?", "id": "Pengkodean Unicode Panjang Variabel." },
  { "en": "Apa Itu Instruksi Transfer Data?", "id": "Memindahkan Data (Load, Store)." },
  { "en": "Apa Itu Instruksi Manipulasi Data?", "id": "Operasi Aritmetika Dan Logika." },
  { "en": "Apa Itu Instruksi Kontrol Program?", "id": "Percabangan, Panggilan, Lompatan." },
  { "en": "Apa Itu Percabangan (Branch)?", "id": "Mengubah Alur Eksekusi Secara Kondisional." },
  { "en": "Apa Itu Lompatan (Jump)?", "id": "Mengubah Alur Eksekusi Tanpa Syarat." },
  { "en": "Apa Itu Pemanggilan Subrutin?", "id": "Memanggil Blok Kode Yang Dapat." },
  { "en": "Apa Itu Mode Pengalamatan?", "id": "Cara Menentukan Alamat Operand." },
  { "en": "Apa Itu Zero-Address Instruction?", "id": "Menggunakan Stack Untuk Operand." },
  { "en": "Apa Itu One-Address Instruction?", "id": "Menggunakan Akumulator Implisit." },
  { "en": "Apa Itu Two-Address Instruction?", "id": "Satu Operand Bertindak Ganda." },
  { "en": "Apa Itu Three-Address Instruction?", "id": "Dua Operand Sumber, Satu Tujuan." },
  { "en": "Apa Itu Komputer Tumpukan (Stack)?", "id": "Arsitektur Yang Menggunakan Tumpukan." },
  { "en": "Apa Itu Push?", "id": "Menambahkan Item Ke Tumpukan." },
  { "en": "Apa Itu Pop?", "id": "Menghapus Item Dari Tumpukan." },
  { "en": "Apa Itu Notasi Polish Terbalik (RPN)?", "id": "Notasi Matematika Untuk Komputer." },
  { "en": "Apa Itu Firmware?", "id": "Perangkat Lunak Tertanam Di Perangkat Keras." },
  { "en": "Apa Itu Abstraksi Perangkat Keras (HAL)?", "id": "Lapisan Perangkat Lunak Yang Menyembunyikan." },
  { "en": "Apa Itu Driver Perangkat?", "id": "Perangkat Lunak Yang Mengontrol Perangkat Keras." },
  { "en": "Apa Itu Basic Input/Output System (BIOS)?", "id": "Firmware Di Motherboard PC." },
  { "en": "Apa Itu Unified Extensible Firmware Interface (UEFI)?", "id": "Pengganti Modern Untuk BIOS." },
  { "en": "Apa Itu Secure Boot?", "id": "Fitur UEFI Untuk Keamanan." },
  { "en": "Apa Itu Trusted Platform Module (TPM)?", "id": "Chip Khusus Untuk Keamanan." },
  { "en": "Apa Itu Kriptografi?", "id": "Ilmu Mengamankan Komunikasi." },
  { "en": "Apa Itu Enkripsi?", "id": "Mengubah Data Menjadi Tidak Terbaca." },
  { "en": "Apa Itu Dekripsi?", "id": "Mengembalikan Data Ke Bentuk Asli." },
  { "en": "Apa Itu Advanced Encryption Standard (AES)?", "id": "Standar Enkripsi Simetris." },
  { "en": "Apa Itu RSA (Rivest-Shamir-Adleman)?", "id": "Algoritma Enkripsi Kunci Publik." },
  { "en": "Apa Itu Akselerator Perangkat Keras Kriptografi?", "id": "Perangkat Keras Untuk Mempercepat Kriptografi." },
  { "en": "Apa Itu Random Number Generator (RNG)?", "id": "Menghasilkan Angka Acak." },
  { "en": "Apa Itu True RNG (TRNG)?", "id": "Menggunakan Fenomena Fisik." },
  { "en": "Apa Itu Pseudo-RNG (PRNG)?", "id": "Menghasilkan Urutan Dari Seed." },
  { "en": "Apa Itu Kompresi Data?", "id": "Mengurangi Ukuran Data." },
  { "en": "Apa Itu Kompresi Lossless?", "id": "Tidak Ada Data Yang Hilang." },
  { "en": "Apa Itu Kompresi Lossy?", "id": "Beberapa Data Hilang." },
  { "en": "Apa Itu Kode Huffman?", "id": "Algoritma Kompresi Lossless." },
  { "en": "Apa Itu Lempel-Ziv (LZ) Algorithm?", "id": "Keluarga Algoritma Kompresi." },
  { "en": "Apa Itu Akselerator Kompresi?", "id": "Perangkat Keras Untuk Kompresi." },
  { "en": "Apa Itu Input/Output Memory Management Unit (IOMMU)?", "id": "Memetakan Alamat I/O Virtual." },
  { "en": "Apa Fungsi IOMMU?", "id": "Mendukung Virtualisasi Dan Proteksi." },
  { "en": "Apa Itu Mainframe?", "id": "Komputer Besar, Kuat, Dan Andal." },
  { "en": "Apa Itu Minicomputer?", "id": "Komputer Ukuran Menengah." },
  { "en": "Apa Itu Microcomputer?", "id": "Komputer Kecil Berbasis Mikroprosesor." },
  { "en": "Apa Itu Personal Computer (PC)?", "id": "Komputer Pribadi." },
  { "en": "Apa Itu Workstation?", "id": "Komputer Desktop Kinerja Tinggi." },
  { "en": "Apa Itu Server?", "id": "Komputer Yang Menyediakan Layanan." },
  { "en": "Apa Itu Wearable Computer?", "id": "Komputer Yang Dikenakan." },
  { "en": "Apa Itu Ubiquitous Computing?", "id": "Komputasi Ada Di Mana-mana." },
  { "en": "Apa Itu Kinerja?", "id": "Ukuran Seberapa Baik Sistem Bekerja." },
  { "en": "Apa Itu Iron Law of Processor Performance?", "id": "Persamaan Kinerja CPU." },
  { "en": "Apa Itu Benchmark?", "id": "Program Untuk Mengukur Kinerja." },
  { "en": "Apa Itu Profiling?", "id": "Menganalisis Kinerja Perangkat Lunak." },
  { "en": "Apa Itu Hotspot?", "id": "Bagian Kode Yang Paling Banyak." },
  { "en": "Apa Itu Bottleneck?", "id": "Komponen Yang Membatasi Kinerja Sistem." },
  { "en": "Apa Itu Analisis Kinerja?", "id": "Mempelajari Dan Mengevaluasi Kinerja Sistem." },
  { "en": "Apa Itu Model Atap (Roofline Model)?", "id": "Model Kinerja Visual Untuk Komputasi." },
  { "en": "Apa Yang Direpresentasikan Atap Datar?", "id": "Kinerja Puncak Prosesor." },
  { "en": "Apa Yang Direpresentasikan Atap Miring?", "id": "Kinerja Terbatas Bandwidth Memori." },
  { "en": "Apa Itu Intensitas Operasional?", "id": "Rasio Operasi Terhadap Akses Memori." },
  { "en": "Apa Itu Komputasi Terikat Komputasi?", "id": "Kinerja Dibatasi Oleh Kecepatan Prosesor." },
  { "en": "Apa Itu Komputasi Terikat Memori?", "id": "Kinerja Dibatasi Oleh Bandwidth Memori." },
  { "en": "Apa Itu Cache Miss Wajib?", "id": "Nama Lain Untuk Cold Start Miss." },
  { "en": "Apa Itu Kebijakan Pengambilan (Fetch Policy)?", "id": "Menentukan Kapan Blok Dibawa Ke Cache." },
  { "en": "Apa Itu Demand Fetch?", "id": "Mengambil Blok Hanya Saat Miss." },
  { "en": "Apa Itu Prefetch?", "id": "Mengambil Blok Sebelum Dibutuhkan." },
  { "en": "Apa Itu Stride Prefetching?", "id": "Mendeteksi Pola Akses Dengan Jarak Tetap." },
  { "en": "Apa Itu Stream Buffer?", "id": "Buffer Kecil Untuk Menyimpan Aliran Prefetch." },
  { "en": "Apa Itu Virtual Memory?", "id": "Nama Lain Untuk Memori Virtual." },
  { "en": "Apa Itu Ruang Alamat?", "id": "Rentang Alamat Memori." },
  { "en": "Apa Itu Ruang Alamat Virtual?", "id": "Ruang Alamat Yang Dilihat Program." },
  { "en": "Apa Itu Ruang Alamat Fisik?", "id": "Ruang Alamat Di Memori Fisik." },
  { "en": "Apa Itu Terjemahan Alamat?", "id": "Mengubah Alamat Virtual Menjadi Fisik." },
  { "en": "Apa Itu Page Table Entry (PTE)?", "id": "Entri Dalam Tabel Halaman." },
  { "en": "Informasi Apa Yang Ada Di PTE?", "id": "Nomor Frame, Bit Valid, Bit Proteksi." },
  { "en": "Apa Itu Bit Proteksi?", "id": "Mengontrol Akses (Baca/Tulis/Eksekusi)." },
  { "en": "Apa Itu Bit Referensi?", "id": "Digunakan Oleh Algoritma Penggantian Halaman." },
  { "en": "Apa Itu Bit Modifikasi (Dirty Bit)?", "id": "Menandai Apakah Halaman Telah Ditulis." },
  { "en": "Apa Itu Hierarki Tabel Halaman?", "id": "Menggunakan Beberapa Level Tabel Halaman." },
  { "en": "Mengapa Hierarki Tabel Halaman Digunakan?", "id": "Untuk Mengurangi Ukuran Tabel Halaman." },
  { "en": "Apa Itu Tabel Halaman Terbalik?", "id": "Satu Entri Per Frame Memori Fisik." },
  { "en": "Apa Itu Hashed Page Table?", "id": "Menggunakan Fungsi Hash Untuk Akses." },
  { "en": "Apa Itu Prosesor CISC (Complex Instruction Set Computer)?", "id": "Memiliki Set Instruksi Yang Kompleks." },
  { "en": "Apa Itu Prosesor RISC (Reduced Instruction Set Computer)?", "id": "Memiliki Set Instruksi Yang Sederhana." },
  { "en": "Apa Itu Filosofi CISC?", "id": "Membuat Perangkat Keras Lebih Kompleks." },
  { "en": "Apa Itu Filosofi RISC?", "id": "Menyederhanakan Perangkat Keras, Mengandalkan Kompiler." },
  { "en": "Apa Itu Micro-operation (μop)?", "id": "Operasi Primitif Di Dalam CPU." },
  { "en": "Apa Itu Microcode?", "id": "Urutan Mikro-operasi." },
  { "en": "Apa Itu Control Store?", "id": "Memori Yang Menyimpan Microcode." },
  { "en": "Apa Itu Horizontal Microcode?", "id": "Setiap Bit Mengontrol Satu Sinyal." },
  { "en": "Apa Itu Vertical Microcode?", "id": "Bit Dikodekan Menjadi Bidang." },
  { "en": "Apa Itu Nanocode?", "id": "Tingkat Kontrol Di Bawah Microcode." },
  { "en": "Apa Itu Dependensi?", "id": "Hubungan Ketergantungan Antar Instruksi." },
  { "en": "Apa Itu Dependensi Aliran (Flow)?", "id": "Sama Dengan Hazard RAW." },
  { "en": "Apa Itu Anti-Dependensi?", "id": "Sama Dengan Hazard WAR." },
  { "en": "Apa Itu Dependensi Output?", "id": "Sama Dengan Hazard WAW." },
  { "en": "Apa Itu Graf Aliran Data?", "id": "Merepresentasikan Dependensi Antar Instruksi." },
  { "en": "Apa Itu Window Instruksi?", "id": "Buffer Untuk Menahan Instruksi." },
  { "en": "Apa Itu Dispatch?", "id": "Mengirim Instruksi Ke Unit Fungsional." },
  { "en": "Apa Itu Issue?", "id": "Memulai Eksekusi Instruksi." },
  { "en": "Apa Itu Completion?", "id": "Eksekusi Instruksi Selesai." },
  { "en": "Apa Itu Commit/Retirement?", "id": "Membuat Hasil Instruksi Terlihat." },
  { "en": "Apa Itu Prosesor Spekulatif?", "id": "Mengeksekusi Instruksi Sebelum Pasti." },
  { "en": "Apa Itu Alias Register?", "id": "Register Fisik Yang Sama Dipetakan." },
  { "en": "Apa Itu Register File Fisik?", "id": "Kumpulan Sebenarnya Register Di CPU." },
  { "en": "Apa Itu Register File Arsitektural?", "id": "Register Yang Terlihat Oleh Programmer." },
  { "en": "Apa Itu Daftar Bebas (Free List)?", "id": "Daftar Register Fisik Yang Tersedia." },
  { "en": "Apa Itu Peta Register (Register Map)?", "id": "Memetakan Register Arsitektural Ke Fisik." },
  { "en": "Apa Itu Common Data Bus (CDB)?", "id": "Menyiarkan Hasil Ke Unit Fungsional." },
  { "en": "Apa Itu Wakeup Logic?", "id": "Memberi Tahu Instruksi Saat Operand." },
  { "en": "Apa Itu Select Logic?", "id": "Memilih Instruksi Untuk Dikeluarkan." },
  { "en": "Apa Itu Prosesor VLIW (Very Long Instruction Word)?", "id": "Kompiler Mengemas Operasi Paralel." },
  { "en": "Apa Itu Explicitly Parallel Instruction Computing (EPIC)?", "id": "Arsitektur VLIW Dari Intel." },
  { "en": "Apa Itu Arsitektur Itanium?", "id": "Contoh Arsitektur EPIC." },
  { "en": "Apa Itu Kompleksitas Kompiler?", "id": "Tingkat Kesulitan Dalam Membuat Kompiler." },
  { "en": "Apa Itu Ketergantungan Pada Kompiler?", "id": "Kinerja Sangat Bergantung Pada Kompiler." },
  { "en": "Apa Itu Kode Biner Dinamis?", "id": "Menerjemahkan Kode Saat Eksekusi." },
  { "en": "Contoh Penerjemahan Dinamis Adalah Apa?", "id": "Implementasi x86 Pada Prosesor RISC." },
  { "en": "Apa Itu Tingkat Paralelisme?", "id": "Jumlah Pekerjaan Yang Dapat Dilakukan." },
  { "en": "Apa Itu Thread-Level Parallelism (TLP)?", "id": "Paralelisme Antara Thread." },
  { "en": "Apa Itu Data-Level Parallelism (DLP)?", "id": "Paralelisme Dalam Operasi Data." },
  { "en": "Apa Itu Komputasi Vektor?", "id": "Jenis SIMD (Single Instruction, Multiple Data)." },
  { "en": "Apa Itu GPU (Graphics Processing Unit) Compute?", "id": "Menggunakan GPU Untuk Tujuan Umum." },
  { "en": "Apa Itu SIMT (Single Instruction, Multiple Threads)?", "id": "Model Eksekusi Yang Digunakan GPU." },
  { "en": "Apa Itu Warp?", "id": "Sekelompok Thread Dalam Arsitektur CUDA." },
  { "en": "Apa Itu Streaming Multiprocessor (SM)?", "id": "Unit Pemrosesan Dasar Di GPU Nvidia." },
  { "en": "Apa Itu Core CUDA?", "id": "Unit Eksekusi Dalam SM." },
  { "en": "Apa Itu Memori Bersama (Shared Memory)?", "id": "Cache Cepat Yang Dapat Diprogram." },
  { "en": "Apa Itu Memori Global?", "id": "Memori DRAM Utama GPU." },
  { "en": "Apa Itu Memori Konstan?", "id": "Memori Cache Read-Only." },
  { "en": "Apa Itu Memori Tekstur?", "id": "Cache Dioptimalkan Untuk Akses Spasial." },
  { "en": "Apa Itu Thread Block?", "id": "Sekelompok Thread Yang Dieksekusi." },
  { "en": "Apa Itu Grid?", "id": "Sekelompok Blok Thread." },
  { "en": "Apa Itu Kernel?", "id": "Fungsi Yang Dijalankan Di GPU." },
  { "en": "Apa Itu Divergensi Thread?", "id": "Thread Dalam Warp Mengambil Jalur." },
  { "en": "Apa Akibat Divergensi Thread?", "id": "Menurunkan Kinerja." },
  { "en": "Apa Itu Coalesced Memory Access?", "id": "Menggabungkan Akses Memori Untuk Efisiensi." },
  { "en": "Apa Itu Bank Conflict?", "id": "Beberapa Thread Mengakses Bank Memori." },
  { "en": "Apa Itu Atomic Operation?", "id": "Operasi Baca-Modifikasi-Tulis." },
  { "en": "Apa Itu Perangkat Keras Aplikasi Khusus?", "id": "Perangkat Keras Yang Dirancang." },
  { "en": "Apa Itu ASIC (Application-Specific Integrated Circuit)?", "id": "Chip Untuk Aplikasi Tertentu." },
  { "en": "Apa Itu Alur Desain ASIC?", "id": "Langkah-langkah Untuk Merancang Chip." },
  { "en": "Apa Itu Sel Standar?", "id": "Blok Logika Pra-desain." },
  { "en": "Apa Itu Verifikasi?", "id": "Memastikan Desain Berfungsi Benar." },
  { "en": "Apa Itu Simulasi?", "id": "Meniru Perilaku Desain." },
  { "en": "Apa Itu Emulasi?", "id": "Menggunakan Perangkat Keras Untuk Simulasi." },
  { "en": "Apa Itu Prototyping FPGA?", "id": "Mengimplementasikan Desain Di FPGA." },
  { "en": "Apa Itu Desain Fisik?", "id": "Mengubah Desain Logika Menjadi Tata." },
  { "en": "Apa Itu Floorplanning?", "id": "Menempatkan Blok-blok Utama." },
  { "en": "Apa Itu Placement?", "id": "Menempatkan Sel-sel Standar." },
  { "en": "Apa Itu Routing?", "id": "Menghubungkan Sel-sel Dengan Kawat." },
  { "en": "Apa Itu Clock Tree Synthesis?", "id": "Membangun Jaringan Distribusi Clock." },
  { "en": "Apa Itu Timing Closure?", "id": "Memastikan Desain Memenuhi Batas Waktu." },
  { "en": "Apa Itu Signoff?", "id": "Tahap Akhir Sebelum Fabrikasi." },
  { "en": "Apa Itu Tape-out?", "id": "Mengirim Desain Akhir Ke Pabrik." },
  { "en": "Apa Itu Foundry?", "id": "Pabrik Fabrikasi Semikonduktor." },
  { "en": "Apa Itu Mask Set?", "id": "Satu Set Masker Fotolitografi." },
  { "en": "Apa Itu Reticle?", "id": "Nama Lain Untuk Masker Fotolitografi." },
  { "en": "Apa Itu Stepper?", "id": "Mesin Yang Digunakan Dalam Litografi." },
  { "en": "Apa Itu Proses CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Proses Fabrikasi Paling Umum." },
  { "en": "Apa Itu FinFET?", "id": "Jenis Arsitektur Transistor 3D." },
  { "en": "Apa Keuntungan FinFET?", "id": "Kontrol Gerbang Lebih Baik, Arus Bocor Rendah." },
  { "en": "Apa Itu Gate-All-Around (GAA) FET?", "id": "Arsitektur Transistor Generasi Berikutnya." },
  { "en": "Apa Itu Design Rule Checking (DRC)?", "id": "Memverifikasi Tata Letak Terhadap Aturan Fabrikasi." },
  { "en": "Apa Itu Layout Versus Schematic (LVS)?", "id": "Membandingkan Tata Letak Dengan Skematik." },
  { "en": "Apa Itu Ekstraksi Parasitik?", "id": "Menghitung Hambatan Parasitik Dari Tata Letak." },
  { "en": "Apa Itu Standard Cell Library?", "id": "Kumpulan Sel Logika Dengan Tata Letak." },
  { "en": "Apa Itu Macro?", "id": "Blok Tata Letak Kustom." },
  { "en": "Contoh Macro Adalah Apa?", "id": "Blok Memori SRAM (Static Random-Access Memory)." },
  { "en": "Apa Itu Power Grid?", "id": "Jaringan Kawat Untuk Distribusi Daya." },
  { "en": "Apa Itu Clock Tree?", "id": "Jaringan Kawat Untuk Distribusi Clock." },
  { "en": "Apa Itu Routing Congestion?", "id": "Area Dimana Terlalu Banyak Kawat." },
  { "en": "Apa Itu Antenna Effect?", "id": "Akumulasi Muatan Selama Fabrikasi." },
  { "en": "Apa Itu Dioda Antena?", "id": "Dioda Untuk Melindungi Dari Efek Antena." },
  { "en": "Apa Itu Signal Electromigration?", "id": "Elektromigrasi Pada Jalur Sinyal." },
  { "en": "Apa Itu Yield Analysis?", "id": "Menganalisis Dan Meningkatkan Yield." },
  { "en": "Apa Itu Binning?", "id": "Mengklasifikasikan Chip Berdasarkan Kinerja." },
  { "en": "Apa Itu Speed Binning?", "id": "Mengklasifikasikan Chip Berdasarkan Frekuensi." },
  { "en": "Apa Itu Fungsionalitas Penuh?", "id": "Chip Lulus Semua Uji Fungsional." },
  { "en": "Apa Itu Uji Wafer?", "id": "Pengujian Yang Dilakukan Di Atas Wafer." },
  { "en": "Apa Itu Probe Card?", "id": "Antarmuka Antara Penguji Dan Wafer." },
  { "en": "Apa Itu Uji Paket Akhir?", "id": "Pengujian Setelah Chip Dikemas." },
  { "en": "Apa Itu Test Socket?", "id": "Soket Untuk Menahan Chip Selama Pengujian." },
  { "en": "Apa Itu Automatic Test Equipment (ATE)?", "id": "Peralatan Otomatis Untuk Pengujian Chip." },
  { "en": "Apa Itu Vektor Uji?", "id": "Pola Input/Output Untuk Pengujian." },
  { "en": "Apa Itu Cakupan Kesalahan (Fault Coverage)?", "id": "Persentase Kesalahan Yang Dapat Dideteksi." },
  { "en": "Apa Itu Model Kesalahan?", "id": "Representasi Sederhana Dari Cacat Fisik." },
  { "en": "Apa Itu Stuck-at-0?", "id": "Simpul Terjebak Di Logika Nol." },
  { "en": "Apa Itu Stuck-at-1?", "id": "Simpul Terjebak Di Logika Satu." },
  { "en": "Apa Itu Bridge Fault?", "id": "Hubung Singkat Antara Dua Simpul." },
  { "en": "Apa Itu Open Fault?", "id": "Koneksi Terputus." },
  { "en": "Apa Itu Transistor Stuck-Open?", "id": "Transistor Gagal Untuk Menyala." },
  { "en": "Apa Itu Transistor Stuck-On?", "id": "Transistor Gagal Untuk Mati." },
  { "en": "Apa Itu Delay Fault?", "id": "Cacat Yang Mempengaruhi Waktu Sirkuit." },
  { "en": "Apa Itu Automatic Test Pattern Generation (ATPG)?", "id": "Perangkat Lunak Yang Membuat Vektor Uji." },
  { "en": "Apa Itu Kontrolabilitas?", "id": "Kemudahan Mengatur Nilai Simpul." },
  { "en": "Apa Itu Observabilitas?", "id": "Kemudahan Mengamati Nilai Simpul." },
  { "en": "Apa Itu SCOAP (Sandia Controllability/Observability Analysis Program)?", "id": "Metrik Untuk Keterujian." },
  { "en": "Apa Itu Scan Flip-Flop?", "id": "Flip-Flop Dengan Mode Scan." },
  { "en": "Apa Itu Mode Scan?", "id": "Flip-Flop Terhubung Menjadi Rantai Geser." },
  { "en": "Apa Itu Scan Chain?", "id": "Rantai Geser Dari Flip-Flop." },
  { "en": "Apa Itu Scan-in?", "id": "Memasukkan Data Ke Scan Chain." },
  { "en": "Apa Itu Scan-out?", "id": "Mengeluarkan Data Dari Scan Chain." },
  { "en": "Apa Itu Full Scan Design?", "id": "Semua Flip-Flop Dapat Discan." },
  { "en": "Apa Itu Partial Scan Design?", "id": "Hanya Sebagian Flip-Flop Yang Discan." },
  { "en": "Apa Itu Logic BIST (Built-In Self-Test)?", "id": "BIST Untuk Logika Digital." },
  { "en": "Apa Itu Memory BIST?", "id": "BIST Untuk Menguji Memori." },
  { "en": "Apa Itu LFSR (Linear-Feedback Shift Register)?", "id": "Digunakan Untuk Menghasilkan Pola Uji." },
  { "en": "Apa Itu MISR (Multiple-Input Signature Register)?", "id": "Mengompresi Respons Uji." },
  { "en": "Apa Itu Signature Analysis?", "id": "Membandingkan Tanda Tangan Dengan Nilai." },
  { "en": "Apa Itu Test Access Port (TAP) Controller?", "id": "Mesin Keadaan Untuk JTAG." },
  { "en": "Apa Itu TDI (Test Data In)?", "id": "Pin Input JTAG." },
  { "en": "Apa Itu TDO (Test Data Out)?", "id": "Pin Output JTAG." },
  { "en": "Apa Itu TCK (Test Clock)?", "id": "Pin Clock JTAG." },
  { "en": "Apa Itu TMS (Test Mode Select)?", "id": "Pin Kontrol JTAG." },
  { "en": "Apa Itu Instruction Register (JTAG)?", "id": "Menyimpan Instruksi JTAG." },
  { "en": "Apa Itu Data Register (JTAG)?", "id": "Register Data Dalam JTAG." },
  { "en": "Apa Itu Boundary Scan Register?", "id": "Register Di Sekeliling Perbatasan Chip." },
  { "en": "Apa Itu Desain Daya Rendah?", "id": "Teknik Untuk Mengurangi Konsumsi Daya." },
  { "en": "Apa Itu Konsumsi Daya Statis?", "id": "Daya Akibat Arus Bocor." },
  { "en": "Apa Itu Konsumsi Daya Dinamis?", "id": "Daya Akibat Aktivitas Switching." },
  { "en": "Apa Itu Power Gating?", "id": "Mematikan Blok Sirkuit." },
  { "en": "Apa Itu Clock Gating?", "id": "Menghentikan Clock Ke Blok." },
  { "en": "Apa Itu DVFS (Dynamic Voltage and Frequency Scaling)?", "id": "Menyesuaikan Tegangan Dan Frekuensi." },
  { "en": "Apa Itu State Retention?", "id": "Menyimpan Keadaan Flip-Flop." },
  { "en": "Apa Itu Power Domain?", "id": "Wilayah Chip Dengan Suplai Daya." },
  { "en": "Apa Itu Isolation Cell?", "id": "Mengisolasi Sinyal Antar Domain Daya." },
  { "en": "Apa Itu Level Shifter?", "id": "Mengubah Level Tegangan Sinyal." },
  { "en": "Apa Itu UPF (Unified Power Format)?", "id": "Standar Untuk Spesifikasi Daya Rendah." },
  { "en": "Apa Itu Common Power Format (CPF)?", "id": "Standar Daya Rendah Lainnya." },
  { "en": "Apa Itu Analisis Daya?", "id": "Memperkirakan Konsumsi Daya Desain." },
  { "en": "Apa Itu Analisis Daya Berbasis Vektor?", "id": "Menggunakan Vektor Simulasi." },
  { "en": "Apa Itu Analisis Daya Berbasis Probabilistik?", "id": "Menggunakan Statistik Aktivitas." },
  { "en": "Apa Itu Clock-gating Terintegrasi?", "id": "Sel Khusus Untuk Implementasi Clock Gating." },
  { "en": "Apa Itu Operand Isolation?", "id": "Mencegah Aktivitas Switching Yang Tidak Perlu." },
  { "en": "Apa Itu Bus Inversion?", "id": "Menginversi Bus Untuk Mengurangi Transisi." },
  { "en": "Apa Itu Kode Gray?", "id": "Mengurangi Aktivitas Switching Pada Penghitung." },
  { "en": "Apa Itu Memori Tahan Bocor?", "id": "SRAM Dengan Arus Bocor Rendah." },
  { "en": "Apa Itu Pemulihan Energi?", "id": "Mendaur Ulang Energi Dalam Sirkuit." },
  { "en": "Apa Itu Logika Adiabatik?", "id": "Logika Reversibel Untuk Daya Rendah." },
  { "en": "Apa Itu Cache yang dapat Dikonfigurasi Ulang?", "id": "Ukuran Dan Asosiativitas Dapat Diubah." },
  { "en": "Apa Itu Filter Cache?", "id": "Cache Kecil Untuk Mengurangi Akses." },
  { "en": "Apa Itu Scratchpad Memory?", "id": "RAM Cepat Yang Dikelola Perangkat Lunak." },
  { "en": "Apa Perbedaan Cache Dan Scratchpad?", "id": "Cache Dikelola Perangkat Keras." },
  { "en": "Apa Itu Predictor Terakhir Kali?", "id": "Prediktor Percabangan Sederhana." },
  { "en": "Apa Itu Fetch-Directed Instruction Prefetching?", "id": "Prefetching Berdasarkan Aliran Instruksi." },
  { "en": "Apa Itu Stream Prefetching?", "id": "Mendeteksi Akses Berurutan." },
  { "en": "Apa Itu Victim Buffer?", "id": "Nama Lain Untuk Victim Cache." },
  { "en": "Apa Itu Write Merging?", "id": "Menggabungkan Penulisan Ke Lokasi Sama." },
  { "en": "Apa Itu Memory Controller?", "id": "Mengelola Akses Ke DRAM." },
  { "en": "Apa Itu Penjadwalan Memori?", "id": "Mengatur Urutan Permintaan Memori." },
  { "en": "Apa Itu Open-Page Policy?", "id": "Menjaga Baris DRAM Tetap Terbuka." },
  { "en": "Apa Itu Closed-Page Policy?", "id": "Menutup Baris DRAM Setelah Akses." },
  { "en": "Apa Itu Interleaving Bank?", "id": "Menyebarkan Akses Di Seluruh Bank." },
  { "en": "Apa Itu Interleaving Channel?", "id": "Menyebarkan Akses Di Seluruh Channel." },
  { "en": "Apa Itu Refresh Memori?", "id": "Proses Membaca Ulang Sel DRAM." },
  { "en": "Mengapa DRAM (Dynamic Random-Access Memory) Perlu Di-refresh?", "id": "Untuk Mencegah Kehilangan Data." },
  { "en": "Apa Itu Siklus Refresh?", "id": "Interval Waktu Antara Operasi Refresh." },
  { "en": "Apa Itu Kontroler Refresh?", "id": "Sirkuit Yang Mengelola Proses Refresh." },
  { "en": "Apa Itu Komputasi Heterogen?", "id": "Menggunakan Berbagai Jenis Core." },
  { "en": "Contoh Komputasi Heterogen Adalah Apa?", "id": "CPU (Central Processing Unit) + GPU (Graphics Processing Unit)." },
  { "en": "Apa Itu Arsitektur big.LITTLE ARM?", "id": "Menggabungkan Core Berkinerja Tinggi Dan Efisien." },
  { "en": "Apa Itu Global Task Scheduling?", "id": "Penjadwal Memindahkan Tugas Antar Core." },
  { "en": "Apa Itu Cache-only Memory Architecture (COMA)?", "id": "Memori Utama Bertindak Sebagai Cache." },
  { "en": "Apa Itu Cache-coherent NUMA (ccNUMA)?", "id": "Sistem NUMA Dengan Koherensi Cache." },
  { "en": "Apa Itu Directory-based Coherence?", "id": "Menggunakan Direktori Terpusat." },
  { "en": "Apa Itu Snoopy-based Coherence?", "id": "Cache Memantau Bus." },
  { "en": "Apa Itu Skalabilitas Protokol Koherensi?", "id": "Bagaimana Kinerja Berubah Dengan Jumlah." },
  { "en": "Protokol Mana Yang Skalanya Lebih Baik?", "id": "Protokol Berbasis Direktori." },
  { "en": "Apa Itu Jaringan Interkoneksi?", "id": "Menghubungkan Prosesor Dan Memori." },
  { "en": "Apa Itu Topologi?", "id": "Pola Koneksi Jaringan." },
  { "en": "Apa Itu Diameter Jaringan?", "id": "Jarak Terpendek Maksimum." },
  { "en": "Apa Itu Bisection Bandwidth?", "id": "Bandwidth Minimum Memotong Jaringan." },
  { "en": "Apa Itu Topologi Jala (Mesh)?", "id": "Node Terhubung Dalam Grid." },
  { "en": "Apa Itu Topologi Torus?", "id": "Jala Dengan Tepi Yang Terhubung." },
  { "en": "Apa Itu Topologi Hypercube?", "id": "Node Sesuai Sudut Hypercube." },
  { "en": "Apa Itu Topologi Cincin (Ring)?", "id": "Node Terhubung Dalam Lingkaran." },
  { "en": "Apa Itu Topologi Pohon (Tree)?", "id": "Struktur Hirarkis." },
  { "en": "Apa Itu Topologi Fat Tree?", "id": "Pohon Dengan Link Lebih Lebar." },
  { "en": "Apa Itu Algoritma Routing?", "id": "Menentukan Jalur Paket." },
  { "en": "Apa Itu Routing Deterministik?", "id": "Jalur Selalu Sama." },
  { "en": "Apa Itu Routing Adaptif?", "id": "Jalur Dapat Berubah." },
  { "en": "Apa Itu Deadlock?", "id": "Paket Saling Menunggu." },
  { "en": "Apa Itu Livelock?", "id": "Paket Terus Bergerak Tapi Gagal." },
  { "en": "Apa Itu Virtual Channel?", "id": "Membantu Mencegah Deadlock." },
  { "en": "Apa Itu Flow Control?", "id": "Mengatur Aliran Paket." },
  { "en": "Apa Itu Wormhole Switching?", "id": "Paket Diteruskan Seperti Cacing." },
  { "en": "Apa Itu Flit?", "id": "Flow Control Unit." },
  { "en": "Apa Itu Packet?", "id": "Unit Data Lengkap." },
  { "en": "Apa Itu Head Flit?", "id": "Flit Pertama Dari Paket." },
  { "en": "Apa Itu Tail Flit?", "id": "Flit Terakhir Dari Paket." },
  { "en": "Apa Itu Body Flit?", "id": "Flit Di Antara Head Dan Tail." },
  { "en": "Apa Itu SIMD (Single Instruction, Multiple Data) Within a Register (SWAR)?", "id": "Teknik Untuk Emulasi SIMD." },
  { "en": "Apa Itu Set Instruksi Multimedia?", "id": "Instruksi SIMD Untuk Media." },
  { "en": "Contoh Set Instruksi Multimedia Adalah Apa?", "id": "MMX, SSE, AVX." },
  { "en": "Apa Itu Pengalamatan Vektor?", "id": "Mode Pengalamatan Untuk Operasi Vektor." },
  { "en": "Apa Itu Stride?", "id": "Jarak Tetap Antara Elemen." },
  { "en": "Apa Itu Gather-Scatter?", "id": "Akses Memori Tidak Beraturan." },
  { "en": "Apa Itu Mask Register?", "id": "Mengaktifkan Atau Menonaktifkan Operasi." },
  { "en": "Apa Itu VLIW (Very Long Instruction Word)?", "id": "Kompiler Mengemas Operasi Paralel." },
  { "en": "Apa Itu Bundel?", "id": "Paket Instruksi Dalam VLIW." },
  { "en": "Apa Itu Kompensasi Kode?", "id": "Mengisi Slot Operasi." },
  { "en": "Apa Itu EPIC (Explicitly Parallel Instruction Computing)?", "id": "Filosofi Desain Intel." },
  { "en": "Apa Itu Arsitektur Itanium?", "id": "Contoh Arsitektur EPIC." },
  { "en": "Apa Itu Predication?", "id": "Membuat Eksekusi Instruksi Menjadi Kondisional." },
  { "en": "Apa Itu Register Predikat?", "id": "Menyimpan Hasil Perbandingan." },
  { "en": "Apa Itu Speculative Load?", "id": "Memuat Data Secara Spekulatif." },
  { "en": "Apa Itu Exception Deferral?", "id": "Menunda Penanganan Pengecualian." },
  { "en": "Apa Itu Data Speculation?", "id": "Spekulasi Pada Dependensi Data." },
  { "en": "Apa Itu Advanced Load Address Table (ALAT)?", "id": "Mendukung Spekulasi Data." },
  { "en": "Apa Itu Thread-Level Speculation (TLS)?", "id": "Mengeksekusi Loop Secara Paralel." },
  { "en": "Apa Itu Asisten Perangkat Keras?", "id": "Perangkat Keras Untuk Membantu Tugas." },
  { "en": "Apa Itu Reconfigurable Computing?", "id": "Mengkonfigurasi Ulang Perangkat Keras." },
  { "en": "Apa Itu Coarse-Grained Reconfigurable Array (CGRA)?", "id": "Array Unit Pemrosesan." },
  { "en": "Apa Itu Fine-Grained Reconfigurability?", "id": "Konfigurasi Pada Tingkat Gerbang." },
  { "en": "Apa Itu Virtualisasi?", "id": "Menciptakan Versi Virtual." },
  { "en": "Apa Itu Guest Operating System?", "id": "Sistem Operasi Yang Berjalan." },
  { "en": "Apa Itu Host Operating System?", "id": "Sistem Operasi Utama." },
  { "en": "Apa Itu Hypervisor Tipe 1?", "id": "Berjalan Langsung Di Atas Perangkat Keras." },
  { "en": "Apa Itu Hypervisor Tipe 2?", "id": "Berjalan Di Atas Host OS." },
  { "en": "Apa Itu Bare-Metal Hypervisor?", "id": "Nama Lain Untuk Tipe 1." },
  { "en": "Apa Itu Hosted Hypervisor?", "id": "Nama Lain Untuk Tipe 2." },
  { "en": "Apa Itu Instruksi Sensitif?", "id": "Instruksi Yang Berperilaku Berbeda." },
  { "en": "Apa Itu Deprivileging?", "id": "Menjalankan Kode Di Level." },
  { "en": "Apa Itu Binary Translation?", "id": "Menerjemahkan Blok Kode Biner." },
  { "en": "Apa Itu Device Emulation?", "id": "Meniru Perangkat Keras Di Perangkat Lunak." },
  { "en": "Apa Itu I/O (Input/Output) Virtualization?", "id": "Berbagi Perangkat I/O." },
  { "en": "Apa Itu Intel VT-x?", "id": "Dukungan Virtualisasi Intel." },
  { "en": "Apa Itu AMD-V?", "id": "Dukungan Virtualisasi AMD." },
  { "en": "Apa Itu Extended Page Tables (EPT)?", "id": "Dukungan Perangkat Keras Untuk Virtualisasi." },
  { "en": "Apa Itu Nested Virtualization?", "id": "Menjalankan Hypervisor Di Dalam VM." },
  { "en": "Apa Itu Live Migration?", "id": "Memindahkan Mesin Virtual." },
  { "en": "Apa Itu Checkpointing?", "id": "Menyimpan Keadaan Sistem." },
  { "en": "Apa Itu Keamanan Arsitektur?", "id": "Melindungi Sistem Dari Ancaman." },
  { "en": "Apa Itu Model Keamanan?", "id": "Model Formal Untuk Kebijakan Keamanan." },
  { "en": "Apa Itu Bell-LaPadula Model?", "id": "Model Keamanan Untuk Kerahasiaan." },
  { "en": "Apa Itu Aturan 'No Read Up'?", "id": "Subjek Tidak Dapat Membaca Objek." },
  { "en": "Apa Itu Aturan 'No Write Down'?", "id": "Subjek Tidak Dapat Menulis Ke Objek." },
  { "en": "Apa Itu Biba Integrity Model?", "id": "Model Keamanan Untuk Integritas." },
  { "en": "Apa Itu Aturan 'No Read Down'?", "id": "Subjek Tidak Dapat Membaca Objek." },
  { "en": "Apa Itu Aturan 'No Write Up'?", "id": "Subjek Tidak Dapat Menulis Ke Objek." },
  { "en": "Apa Itu Chinese Wall Model?", "id": "Model Untuk Mencegah Konflik." },
  { "en": "Apa Itu Trusted Computing Base (TCB)?", "id": "Bagian Sistem Yang Bertanggung Jawab." },
  { "en": "Apa Itu Reference Monitor?", "id": "Konsep Abstrak Yang Menengahi." },
  { "en": "Apa Itu Security Kernel?", "id": "Implementasi Dari Reference Monitor." },
  { "en": "Apa Itu Information Flow Control?", "id": "Mengontrol Bagaimana Informasi Menyebar." },
  { "en": "Apa Itu Covert Channel?", "id": "Saluran Komunikasi Tersembunyi." },
  { "en": "Apa Itu Timing Channel?", "id": "Membocorkan Informasi Melalui Waktu." },
  { "en": "Apa Itu Storage Channel?", "id": "Membocorkan Informasi Dengan Memodifikasi." },
  { "en": "Apa Itu Buffer Overflow Attack?", "id": "Mengeksploitasi Kerentanan Buffer Overflow." },
  { "en": "Apa Itu Stack Smashing?", "id": "Jenis Serangan Buffer Overflow." },
  { "en": "Apa Itu Non-Executable Stack?", "id": "Mencegah Eksekusi Kode Di Stack." },
  { "en": "Apa Itu Address Space Layout Randomization (ASLR)?", "id": "Mengacak Lokasi Memori." },
  { "en": "Apa Itu Stack Canary?", "id": "Nilai Tersembunyi Untuk Mendeteksi Kerusakan." },
  { "en": "Apa Itu Return-Oriented Programming (ROP)?", "id": "Teknik Eksploitasi Lanjutan." },
  { "en": "Apa Itu Gadget?", "id": "Urutan Instruksi Kecil Yang Berakhir." },
  { "en": "Apa Itu Meltdown?", "id": "Kerentanan Perangkat Keras." },
  { "en": "Apa Itu Spectre?", "id": "Kerentanan Perangkat Keras Lainnya." },
  { "en": "Apa Itu Serangan Side-Channel?", "id": "Mengeksploitasi Informasi Fisik." },
  { "en": "Apa Itu Analisis Daya?", "id": "Menganalisis Konsumsi Daya." },
  { "en": "Apa Itu Analisis Waktu?", "id": "Menganalisis Waktu Eksekusi." },
  { "en": "Apa Itu Analisis Cache?", "id": "Menganalisis Pola Akses Cache." },
  { "en": "Apa Itu Fault Injection Attack?", "id": "Menyebabkan Kesalahan Untuk Membongkar Sistem." },
  { "en": "Apa Itu Rowhammer?", "id": "Serangan Yang Menginduksi Bit Flip." },
  { "en": "Apa Itu Trusted Platform Module (TPM)?", "id": "Chip Keamanan Perangkat Keras." },
  { "en": "Apa Itu Secure Enclave?", "id": "Lingkungan Eksekusi Aman." },
  { "en": "Apa Itu Intel Software Guard Extensions (SGX)?", "id": "Implementasi Enclave Aman Dari Intel." },
  { "en": "Apa Itu Physically Unclonable Function (PUF)?", "id": "Sidik Jari Fisik Chip." },
  { "en": "Apa Itu Kunci Kriptografi?", "id": "Data Rahasia Untuk Enkripsi." },
  { "en": "Apa Itu Kriptografi Kunci Simetris?", "id": "Menggunakan Kunci Yang Sama." },
  { "en": "Apa Itu Kriptografi Kunci Asimetris?", "id": "Menggunakan Kunci Publik Dan Privat." },
  { "en": "Apa Itu Mesin Analitik?", "id": "Komputer Mekanis Awal Charles Babbage." },
  { "en": "Apa Itu Mesin Diferensial?", "id": "Kalkulator Mekanis Otomatis." },
  { "en": "Apa Itu ENIAC (Electronic Numerical Integrator and Computer)?", "id": "Komputer Digital Elektronik Pertama." },
  { "en": "Apa Itu Arsitektur EDVAC (Electronic Discrete Variable Automatic Computer)?", "id": "Desain Komputer Stored-Program Awal." },
  { "en": "Apa Itu Univac I?", "id": "Komputer Komersial Pertama Di AS." },
  { "en": "Apa Itu IBM (International Business Machines) System/360?", "id": "Keluarga Komputer Kompatibel Pertama." },
  { "en": "Apa Itu Microprogramming?", "id": "Implementasi Unit Kontrol." },
  { "en": "Apa Itu CDC (Control Data Corporation) 6600?", "id": "Dianggap Superkomputer Pertama." },
  { "en": "Siapa Arsitek Utama CDC 6600?", "id": "Seymour Cray." },
  { "en": "Apa Itu Cray-1?", "id": "Superkomputer Vektor Ikonik." },
  { "en": "Apa Itu PDP-8?", "id": "Minikomputer 12-bit Populer." },
  { "en": "Apa Itu Arsitektur MIPS (Microprocessor without Interlocked Pipeline Stages)?", "id": "Arsitektur RISC Akademis." },
  { "en": "Apa Itu Arsitektur SPARC?", "id": "Arsitektur RISC Dari Sun Microsystems." },
  { "en": "Apa Itu Arsitektur PowerPC?", "id": "Arsitektur RISC Dari Apple-IBM-Motorola." },
  { "en": "Apa Itu Transputer?", "id": "Mikroprosesor Dengan Link Komunikasi." },
  { "en": "Apa Itu Mesin Lisp?", "id": "Komputer Yang Dioptimalkan Untuk Bahasa." },
  { "en": "Apa Itu Connection Machine?", "id": "Komputer Paralel Masif." },
  { "en": "Apa Itu I/O (Input/Output) Channel?", "id": "Prosesor I/O Di Mainframe." },
  { "en": "Apa Itu Antarmuka Periferal?", "id": "Standar Koneksi Perangkat." },
  { "en": "Apa Itu IEEE (Institute of Electrical and Electronics Engineers) 1394?", "id": "Antarmuka Serial Dikenal FireWire." },
  { "en": "Apa Itu Accelerated Graphics Port (AGP)?", "id": "Port Grafis Berkecepatan Tinggi." },
  { "en": "Apa Itu Industry Standard Architecture (ISA) Bus?", "id": "Bus PC Awal." },
  { "en": "Apa Itu Extended ISA (EISA)?", "id": "Ekstensi 32-bit Dari ISA." },
  { "en": "Apa Itu VESA (Video Electronics Standards Association) Local Bus?", "id": "Bus Lokal Untuk Grafis." },
  { "en": "Apa Itu Memory Stick?", "id": "Format Kartu Memori Flash." },
  { "en": "Apa Itu CompactFlash?", "id": "Format Kartu Memori Awal." },
  { "en": "Apa Itu Secure Digital (SD) Card?", "id": "Format Kartu Memori Populer." },
  { "en": "Apa Itu Sistem File?", "id": "Cara OS Mengatur File." },
  { "en": "Apa Itu File Allocation Table (FAT)?", "id": "Sistem File Sederhana." },
  { "en": "Apa Itu New Technology File System (NTFS)?", "id": "Sistem File Standar Windows." },
  { "en": "Apa Itu Extended File System (EXT)?", "id": "Sistem File Standar Linux." },
  { "en": "Apa Itu Blok Boot?", "id": "Blok Pertama Disk." },
  { "en": "Apa Itu Superblok?", "id": "Berisi Metadata Tentang Sistem File." },
  { "en": "Apa Itu Inode?", "id": "Struktur Data Yang Menyimpan Metadata." },
  { "en": "Apa Itu Direktori?", "id": "Struktur Untuk Mengorganisir File." },
  { "en": "Apa Itu Hard Link?", "id": "Entri Direktori Kedua Untuk File." },
  { "en": "Apa Itu Symbolic Link?", "id": "File Yang Menunjuk Ke File Lain." },
  { "en": "Apa Itu Journaling File System?", "id": "Meningkatkan Keandalan Dengan Jurnal." },
  { "en": "Apa Itu Jurnal?", "id": "Log Perubahan Yang Akan Datang." },
  { "en": "Apa Itu Copy-on-Write (CoW)?", "id": "Teknik Manajemen Data." },
  { "en": "Apa Itu Logical Volume Manager (LVM)?", "id": "Manajemen Disk Fleksibel." },
  { "en": "Apa Itu Disk Defragmentation?", "id": "Mengatur Ulang Fragmen File." },
  { "en": "Apa Itu Virtualisasi Penyimpanan?", "id": "Mengabstraksi Penyimpanan Fisik." },
  { "en": "Apa Itu Deduplikasi?", "id": "Menghilangkan Salinan Data Duplikat." },
  { "en": "Apa Itu Thin Provisioning?", "id": "Mengalokasikan Ruang Penyimpanan Sesuai Kebutuhan." },
  { "en": "Apa Itu Komputasi Tahan Lama?", "id": "Menjaga Keadaan Melalui Gagal Daya." },
  { "en": "Apa Itu Komputasi Perseptual?", "id": "Membuat Komputer Berinteraksi Secara Alami." },
  { "en": "Apa Itu Pengenalan Gestur?", "id": "Menginterpretasikan Gerakan Manusia." },
  { "en": "Apa Itu Pengenalan Suara?", "id": "Mengubah Ucapan Menjadi Teks." },
  { "en": "Apa Itu Sintesis Suara?", "id": "Menghasilkan Ucapan Manusia Buatan." },
  { "en": "Apa Itu Komputasi Afektif?", "id": "Komputasi Yang Berhubungan Dengan Emosi." },
  { "en": "Apa Itu Arsitektur Berorientasi Aliran?", "id": "Mengoptimalkan Aliran Data." },
  { "en": "Apa Itu Logika Reversibel?", "id": "Logika Yang Dapat Berjalan Mundur." },
  { "en": "Apa Itu Bill of Materials (BOM)?", "id": "Daftar Semua Komponen." },
  { "en": "Apa Itu Manufaktur Berbantuan Komputer (CAM)?", "id": "Menggunakan Perangkat Lunak Untuk Manufaktur." },
  { "en": "Apa Itu Desain Berbantuan Komputer (CAD)?", "id": "Menggunakan Perangkat Lunak Untuk Desain." },
  { "en": "Apa Itu Rekayasa Berbantuan Komputer (CAE)?", "id": "Menggunakan Perangkat Lunak Untuk Analisis." },
  { "en": "Apa Itu Siklus Hidup Produk?", "id": "Tahapan Kehidupan Produk." },
  { "en": "Apa Itu Keusangan Terencana?", "id": "Mendesain Produk Dengan Umur Terbatas." },
  { "en": "Apa Itu Ekonomi Sirkular?", "id": "Model Ekonomi Regeneratif." },
  { "en": "Apa Itu Komputasi Hijau?", "id": "Praktik Komputasi Ramah Lingkungan." },
  { "en": "Apa Itu Power Usage Effectiveness (PUE)?", "id": "Metrik Efisiensi Energi Pusat Data." },
  { "en": "Apa Itu E-waste?", "id": "Sampah Elektronik." },
  { "en": "Apa Itu Arsitektur Adaptif?", "id": "Arsitektur Yang Dapat Berubah." },
  { "en": "Apa Itu Komputasi Sadar Konteks?", "id": "Sistem Yang Beradaptasi Dengan Konteks." },
  { "en": "Apa Itu Komputasi Otonom?", "id": "Sistem Yang Mengelola Diri Sendiri." },
  { "en": "Apa Itu Sistem Refleksif?", "id": "Sistem Yang Dapat Memodifikasi Dirinya." },
  { "en": "Apa Itu Komputasi Organik?", "id": "Sistem Yang Terinspirasi Biologis." },
  { "en": "Apa Itu Kecerdasan Kawanan?", "id": "Perilaku Kolektif Sistem Terdesentralisasi." },
  { "en": "Apa Itu Sistem Imun Buatan?", "id": "Sistem Komputasi Berbasis Imun." },
  { "en": "Apa Itu Jaringan Saraf Pulsed?", "id": "Nama Lain Spiking Neural Network." },
  { "en": "Apa Itu Komputasi Evolusioner?", "id": "Menggunakan Prinsip Evolusi." },
  { "en": "Apa Itu Algoritma Genetik?", "id": "Algoritma Pencarian Evolusioner." },
  { "en": "Apa Itu Pemrograman Genetik?", "id": "Mengembangkan Program Komputer." },
  { "en": "Apa Itu Kehidupan Buatan?", "id": "Studi Sistem Buatan Mirip Hidup." }



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
