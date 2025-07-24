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

  { "en": "Judul modul 13?", "id": "Instalasi, Perawatan dan Troubleshooting PLC." },
  { "en": "Apa itu tata letak sistem?", "id": "Pendekatan penempatan dan penghubungan komponen." },
  { "en": "Di mana lokasi terbaik PLC?", "id": "Dekat mesin atau proses terkendali." },
  { "en": "Fungsi panel pada PLC?", "id": "Melindungi PLC dari lingkungan berbahaya." },
  { "en": "Standar tata letak panel?", "id": "Harus sesuai standar NEMA." },
  { "en": "Berapa suhu kerja maksimum PLC?", "id": "Biasanya 60°C." },
  { "en": "Bagaimana cara memasang komponen PLC?", "id": "Dipasang tegak untuk pendinginan maksimal." },
  { "en": "Di mana catu daya dipasang?", "id": "Pada bagian atas panel." },
  { "en": "Bagaimana pengelompokan modul I/O?", "id": "Dipisahkan berdasarkan jenis sinyal." },
  { "en": "Bagaimana jika kabel I/O bersilangan?", "id": "Harus dipasang pada sudut tepat." },
  { "en": "Apa fungsi grounding yang baik?", "id": "Ukuran keamanan penting instalasi listrik." },
  { "en": "Kode artikel NEC untuk grounding?", "id": "National Electric Code (NEC) Article 250." },
  { "en": "Fungsi trafo isolasi?", "id": "Mengurangi gangguan pada saluran AC." },
  { "en": "Fungsi sirkuit emergency stop?", "id": "Memutus daya ke sistem I/O." },
  { "en": "Bagaimana sirkuit emergency dirutekan?", "id": "Di luar PLC dan hardwired." },
  { "en": "Fungsi Master Control Relay (MCR)?", "id": "Memutus daya ke sistem I/O." },
  { "en": "Kapan daya harus diputus?", "id": "Sebelum memasang dan melakukan pengkabelan." },
  { "en": "Fungsi penandaan kabel?", "id": "Mempermudah perawatan dan troubleshooting." },
  { "en": "Apa itu penggabungan kabel?", "id": "Mempermudah koneksi ke modul I/O." },
  { "en": "Bagaimana memperbaiki input yang bocor?", "id": "Memasang bleeding resistor." },
  { "en": "Fungsi dioda pada beban induktif?", "id": "Menekan lonjakan tegangan tinggi DC." },
  { "en": "Fungsi sirkuit snubber RC?", "id": "Menekan beban induksi AC." },
  { "en": "Apa itu Metal Oxide Varistor?", "id": "Pelindung lonjakan tegangan populer." },
  { "en": "Bagaimana kabel shield dihubungkan?", "id": "Hubungkan satu ujungnya ke ground." },
  { "en": "Kapan pemeriksaan statis input dilakukan?", "id": "Dengan daya masuk ke PLC." },
  { "en": "Apa fungsi mode force PLC?", "id": "Menguji setiap output secara manual." },
  { "en": "Apa itu pemeriksaan dinamis sistem?", "id": "Memeriksa logika program kontrol." },
  { "en": "Tindakan pencegahan terpenting PLC?", "id": "Pemeliharaan berkala yang tepat." },
  { "en": "Fungsi prosedur lockout and tagout?", "id": "Memastikan peralatan tidak dapat dioperasikan." },
  { "en": "Langkah pertama troubleshooting PLC?", "id": "Identifikasi masalah dan sumbernya." },
  { "en": "Apa itu ground loop?", "id": "Dua atau lebih jalur ground." },
  { "en": "Fungsi watchdog timer?", "id": "Memonitor proses scan CPU." },
  { "en": "Judul modul 11?", "id": "Sensor Digital." },
  { "en": "Apa fungsi sensor?", "id": "Menangkap fenomena fisis atau kimia." },
  { "en": "Output sensor berupa apa?", "id": "Umumnya tegangan dan arus." },
  { "en": "Berapa kabel keluaran sensor?", "id": "Umumnya memiliki tiga kabel." },
  { "en": "Sensor digital menghasilkan sinyal apa?", "id": "Tinggi dan rendah atau 1/0." },
  { "en": "Sensor analog menghasilkan sinyal apa?", "id": "Rentang nilai terendah hingga tertinggi." },
  { "en": "Kapan sensor analog digunakan?", "id": "Jika informasi rinci dibutuhkan." },
  { "en": "Standar tegangan output sensor analog?", "id": "0-5V, 1-5V, 0-10V, ±10V." },
  { "en": "Standar arus output sensor analog?", "id": "4 mA hingga 20 mA." },
  { "en": "Apa itu sourcing dan sinking?", "id": "Konsep pada besaran listrik DC." },
  { "en": "Modul input sourcing memiliki apa?", "id": "Common positive." },
  { "en": "Modul input sinking memiliki apa?", "id": "Common negative." },
  { "en": "Sensor NPN untuk modul apa?", "id": "Modul input sourcing." },
  { "en": "Sensor PNP untuk modul apa?", "id": "Modul input sinking." },
  { "en": "Warna kabel sumber sensor?", "id": "Cokelat (positif) dan Biru (negatif)." },
  { "en": "Warna kabel sinyal sensor?", "id": "Hitam." },
  { "en": "Apa itu Reed switch?", "id": "Sakelar aktif oleh medan magnet." },
  { "en": "Di mana Reed Switch dipakai?", "id": "Indikator batas pada silinder pneumatik." },
  { "en": "Judul modul 15?", "id": "CX Designer Simulation." },
  { "en": "Apa itu sequence control?", "id": "Operasi sesuai urutan yang ditentukan." },
  { "en": "Contoh sequence control?", "id": "Mesin pencuci mobil otomatis." },
  { "en": "Apa itu N.O. contact?", "id": "Contact yang biasanya terbuka." },
  { "en": "Apa itu N.C. contact?", "id": "Contact yang biasanya tertutup." },
  { "en": "Apa itu relay?", "id": "Saklar yang dioperasikan secara elektrik." },
  { "en": "Bagaimana relay bekerja?", "id": "Menggunakan kinerja elektromagnet." },
  { "en": "Judul modul 9?", "id": "Tipe Memori pada PLC." },
  { "en": "Apa itu memori?", "id": "Elemen penyimpan informasi, program, data." },
  { "en": "Apa itu writing memori?", "id": "Menyimpan data dalam memori." },
  { "en": "Apa itu reading memori?", "id": "Mengambil data dari memori." },
  { "en": "Apa itu bit?", "id": "Satuan informasi terkecil (0/1)." },
  { "en": "Satu byte berapa bit?", "id": "Delapan bit." },
  { "en": "Apa itu word?", "id": "Terdiri dari 16 bit." },
  { "en": "Apa itu memori volatile?", "id": "Kehilangan informasi jika daya hilang." },
  { "en": "Apa itu memori non-volatile?", "id": "Mempertahankan informasi tanpa daya." },
  { "en": "Apa itu ROM?", "id": "Read Only Memory." },
  { "en": "Fungsi ROM pada PLC?", "id": "Menyimpan sistem operasi PLC." },
  { "en": "Apa itu RAM?", "id": "Random Access Memory." },
  { "en": "Fungsi RAM pada PLC?", "id": "Area penyimpanan data sementara." },
  { "en": "Apa itu EPROM?", "id": "Erasable Programmable Read-Only Memory." },
  { "en": "Apa itu EEPROM?", "id": "Electrically Erasable Programmable Read-Only Memory." },
  { "en": "Kelebihan Flash EEPROM?", "id": "Sangat cepat menyimpan dan mengambil file." },
  { "en": "Jenis PLC berdasarkan bentuk?", "id": "Compact dan Modular." },
  { "en": "Ciri PLC Compact?", "id": "Prosesor, I/O, catu daya menyatu." },
  { "en": "Ciri PLC Modular?", "id": "Komponen dapat dipisah-pisah." },
  { "en": "Fungsi Area Executive memori?", "id": "Menyimpan program BIOS PLC." },
  { "en": "Fungsi Area Aplikasi memori?", "id": "Menyimpan data dan program pengguna." },
  { "en": "Judul modul 14?", "id": "Penggunaan Komputer dan Robot." },
  { "en": "Jenis teknologi otomasi?", "id": "Fixed automation dan flexible automation." },
  { "en": "Ciri fixed automation?", "id": "Masih menggunakan peralatan mekanik." },
  { "en": "Ciri flexible automation?", "id": "Menggunakan sistem pengatur berbasis komputer." },
  { "en": "Apa itu hierarchical control?", "id": "Satu host mengontrol banyak slave." },
  { "en": "Apa itu distributed control?", "id": "Sistem dibagi, dikendalikan komputer lokal." },
  { "en": "Apa itu heterarchical control?", "id": "Sistem tanpa host computer terpusat." },
  { "en": "Apa itu robot?", "id": "Alat elektro-mekanik melakukan tugas fisik." },
  { "en": "Asal kata robot?", "id": "Dari kata 'robota' (pekerja)." },
  { "en": "Hukum pertama robot?", "id": "Tidak boleh melukai manusia." },
  { "en": "Hukum kedua robot?", "id": "Harus mematuhi perintah manusia." },
  { "en": "Hukum ketiga robot?", "id": "Harus melindungi keberadaan dirinya sendiri." },
  { "en": "Apa itu manipulator?", "id": "Lengan yang memberikan gerakan robot." },
  { "en": "Komponen dasar sistem robot?", "id": "Manipulator, kontroler, dan power." },
  { "en": "Apa itu efektor?", "id": "Peralatan khusus di ujung manipulator." },
  { "en": "Apa itu gripper?", "id": "Efektor untuk menggenggam objek." },
  { "en": "Jenis gripper?", "id": "Mekanik, vakum, dan magnetik." },
  { "en": "Kunci belajar PLC?", "id": "Penyambungan input dan output PLC." },
  { "en": "Kenapa orang gagal belajar PLC?", "id": "Tidak mampu menerapkan kondisi riil." },
  { "en": "Apa itu I/O pada PLC?", "id": "Antarmuka antara perangkat dan CPU." },
  { "en": "Jenis I/O pada PLC?", "id": "Fixed dan modular." },
  { "en": "Fungsi antarmuka modul input?", "id": "Mengubah sinyal dari perangkat proses." },
  { "en": "Fungsi antarmuka modul output?", "id": "Mengubah sinyal controller ke perangkat." },
  { "en": "Apa itu remote rack?", "id": "Rack jauh dari modul prosesor." },
  { "en": "Apa itu alamat PLC?", "id": "Label lokasi informasi pada memori." },
  { "en": "Elemen pengalamatan rack/slot-based?", "id": "Slot, word, dan bit." },
  { "en": "Apa itu tag-based addressing?", "id": "Menggunakan nama alfanumerik (tag)." },
  { "en": "Apa itu soft PLC?", "id": "Mensimulasikan fungsi PLC dalam PC." },
  { "en": "Komponen modul I/O kombinasi?", "id": "PCB dan terminal assembly." },
  { "en": "Berapa point modul I/O?", "id": "8, 16, 32, atau 64." },
  { "en": "Keuntungan modul high-density?", "id": "Menghemat ruang dalam satu slot." },
  { "en": "Jenis modul I/O diskrit?", "id": "Menghubungkan perangkat ON/OFF." },
  { "en": "Sumber tegangan modul I/O?", "id": "Didapat dari luar (eksternal)." },
  { "en": "Fungsi backplane power?", "id": "Memberi daya elektronik modul I/O." },
  { "en": "Bagian sirkuit input AC?", "id": "Bagian daya dan bagian logika." },
  { "en": "Fungsi optical isolator?", "id": "Menyediakan isolasi elektrik." },
  { "en": "Fungsi input noise filter?", "id": "Menghilangkan sinyal palsu." },
  { "en": "Fungsi dioda zener di input?", "id": "Menetapkan ambang batas tegangan minimum." },
  { "en": "Fungsi interposing relay?", "id": "Mengendalikan beban yang besar." },
  { "en": "Jenis output modul diskrit?", "id": "Transistor, triac, atau relay." },
  { "en": "Output triac untuk tegangan apa?", "id": "Hanya untuk perangkat tegangan AC." },
  { "en": "Output transistor untuk tegangan apa?", "id": "Hanya untuk perangkat tegangan DC." },
  { "en": "Apa itu sinking (NPN)?", "id": "Terhubung ke sisi negatif (-)." },
  { "en": "Apa itu sourcing (PNP)?", "id": "Terhubung ke sisi positif (+)." },
  { "en": "Contoh sinyal analog?", "id": "0-20mA, 4-20mA, 0-±10 volt." },
  { "en": "Fungsi modul analog input?", "id": "Menerima sinyal analog dari perangkat." },
  { "en": "Fungsi modul analog output?", "id": "Mengendalikan perangkat dengan sinyal analog." },
  { "en": "Apa itu resolusi input analog?", "id": "Perubahan terkecil yang dapat dideteksi." },
  { "en": "Fungsi modul high-speed counter?", "id": "Menghitung pulsa dari sensor cepat." },
  { "en": "Apa itu kendali PID?", "id": "Proportional-Integral-Derivative." },
  { "en": "Tujuan kendali loop tertutup?", "id": "Menjaga proses sesuai setpoint." },
  { "en": "Fungsi modul positioning?", "id": "Memberikan umpan balik terkait posisi." },
  { "en": "Fungsi modul BASIC/ASCII?", "id": "Menjalankan program BASIC atau C." },
  { "en": "Fungsi modul stepper motor?", "id": "Mengendalikan pergerakan stepper motor." },
  { "en": "Fungsi modul komunikasi?", "id": "Memungkinkan PLC berkomunikasi via jaringan." },
  { "en": "Prinsip penyambungan input PLC?", "id": "Memberi tegangan ke pin input." },
  { "en": "Apa itu Common pada PLC?", "id": "Terminal gabungan untuk return path." },
  { "en": "Keamanan common input positive?", "id": "Mengurangi risiko short circuit 24V." },
  { "en": "Keamanan common input negative?", "id": "Menghindari terbentuknya loop semu." },
  { "en": "Contoh output PLC?", "id": "Lampu, buzzer, solenoid, relay." },
  { "en": "Kapasitas memori ditentukan oleh apa?", "id": "Program yang kompleks." },
  { "en": "Apa itu tabel status input?", "id": "Menyimpan status input dari modul." },
  { "en": "Apa itu tabel status output?", "id": "Menyimpan status sinyal kontrol output." },
  { "en": "Kelebihan CMOS-RAM?", "id": "Arus sangat rendah." },
  { "en": "Rasio I/O PLC Compact?", "id": "Umumnya 60:40 (input:output)." },
  { "en": "Apa itu bit-bit internal?", "id": "Menyimpan data koil internal relay." },
  { "en": "Apa itu register/word?", "id": "Menyimpan data ukuran byte/word." },
  { "en": "Apa itu ADC?", "id": "Analog to Digital Converter." },
  { "en": "Fungsi ADC?", "id": "Mengubah sinyal analog ke digital." },
  { "en": "Dasar ADC yang perlu dipahami?", "id": "Resolusi dan kecepatan sampling." },
  { "en": "Apa itu kecepatan sampling?", "id": "Seberapa sering sinyal dikonversi." },
  { "en": "Satuan kecepatan sampling?", "id": "Sample per second (SPS) / Hz." },
  { "en": "Apa itu Special I/O?", "id": "Kelompok alamat untuk I/O analog." },
  { "en": "Bagaimana mengatur unit number modul?", "id": "Memutar Unit Number Switch." },
  { "en": "Fungsi sensor digital?", "id": "Memberikan output HIGH atau LOW." },
  { "en": "Fungsi sensor analog?", "id": "Memberikan output rentang nilai tertentu." },
  { "en": "Kenapa PLC butuh ADC?", "id": "Karena PLC mengolah data digital." },
  { "en": "Sinyal apa yang dihasilkan sensor?", "id": "Besaran listrik (tegangan/arus)." },
  { "en": "Penyambungan sensor 2 kabel sinking?", "id": "Brown ke positif, blue ke input." },
  { "en": "Penyambungan sensor 2 kabel sourcing?", "id": "Blue ke negatif, brown ke input." },
  { "en": "Bahaya menyambung sensor 2 kabel?", "id": "Langsung ke sumber tegangan." },
  { "en": "Tujuan tata letak sistem PLC?", "id": "Memastikan controller bekerja tanpa masalah." },
  { "en": "Faktor lingkungan penting untuk PLC?", "id": "Suhu, kelembaban, gangguan, getaran." },
  { "en": "Cara membuang panas di panel?", "id": "Menggunakan fan atau blower." },
  { "en": "Cara mencegah kondensasi di panel?", "id": "Menggunakan pemanas dengan thermostat." },
  { "en": "Di mana CPU sebaiknya dipasang?", "id": "Pada tingkat kerja yang baik." },
  { "en": "Fungsi duct kabel?", "id": "Merutekan kabel sinyal dan daya." },
  { "en": "Apa itu crosstalk?", "id": "Interferensi antar jalur I/O." },
  { "en": "Sumber daya catu daya PLC?", "id": "Umumnya satu phase 120-240VAC." },
  { "en": "Apa itu bleeding resistor?", "id": "Memperbaiki kebocoran arus pada input." },
  { "en": "Bagaimana pengaman output solid-state?", "id": "Umumnya menggunakan sekering pada modul." },
  { "en": "Kapan program kontrol diperiksa?", "id": "Sebelum mengunduh program ke PLC." },
  { "en": "Apa itu synchromotion robot?", "id": "Pengaplikasian robot secara bersamaan." },
  { "en": "Apa itu power supply?", "id": "Menyediakan tenaga pada sistem." },
  { "en": "Apa itu gripper vakum?", "id": "Menggunakan mangkok hisap." },
  { "en": "Syarat objek untuk gripper vakum?", "id": "Rata, bersih, dan halus." },
  { "en": "Jenis gripper magnetik?", "id": "Elektromagnet dan magnet permanen." },
  { "en": "Keuntungan gripper magnetik?", "id": "Dapat menangani komponen berlubang." },
  { "en": "Klasifikasi umum robot?", "id": "Robot industri dan robot bergerak." },
  { "en": "Ciri khas robot industri?", "id": "Konstruksi dasar tetap (stationary)." },
  { "en": "Ciri khas robot bergerak?", "id": "Konstruksi dasar bergerak (beroda/berkaki)." },
  { "en": "Contoh peralatan sequence control?", "id": "Push button, lampu, relay, sensor." },
  { "en": "Bagian manipulator?", "id": "Bagian dasar dan bagian tambahan." },
  { "en": "Apa itu kontroler robot?", "id": "Jantung dari sistem robot." },
  { "en": "Fungsi kontroler robot?", "id": "Menyimpan informasi dan mengontrol gerakan." },
  { "en": "Apa itu open loop control?", "id": "Sistem kontrol tanpa umpan balik." },
  { "en": "Apa itu close loop control?", "id": "Sistem kontrol dengan umpan balik." },
  { "en": "Penggerak manipulator disebut apa?", "id": "Actuator atau sistem drive." },
  { "en": "Berapa channel input modul CJ1W AD081?", "id": "8 buah channel input." },
  { "en": "Kapasitas memori PLC modular?", "id": "Besar dan instruksinya lengkap." },
  { "en": "Berapa bit dalam satu word?", "id": "16 bit." },
  { "en": "Bagaimana mengatasi getaran pada PLC?", "id": "Panel tidak boleh melebihi spesifikasi." },
  { "en": "Aksesori panel yang disarankan?", "id": "Stop kontak, lampu, jendela akrilik." },
  { "en": "Tujuan tata letak duct?", "id": "Meminimalisasi interferensi crosstalk." },
  { "en": "Jarak minimal duct dan modul?", "id": "Tidak kurang dari dua inci." },
  { "en": "Bagaimana kabel ground seharusnya?", "id": "Dipisah dari pengkabelan daya." },
  { "en": "Apa itu penekanan beban induksi?", "id": "Mengurangi lonjakan tegangan tinggi." },
  { "en": "Bagaimana dioda menekan beban DC?", "id": "Dihubungkan dalam kondisi reverse-bias." },
  { "en": "Kapan kalibrasi sirkuit analog?", "id": "Sebaiknya setiap enam bulan." },
  { "en": "Bagaimana membersihkan debu di PLC?", "id": "Gunakan penyedot debu atau udara." },
  { "en": "Indikator BATT merah menandakan apa?", "id": "Tegangan baterai di bawah ambang." },
  { "en": "Apa itu watchdog timer?", "id": "Sirkuit waktu monitor proses scan." },
  { "en": "Indikator RUN hijau menandakan apa?", "id": "Processor dalam mode RUN." },
  { "en": "Indikator FLT merah menandakan apa?", "id": "Terjadi kesalahan besar atau fatal." },
  { "en": "Penyebab indikator input berkedip?", "id": "Kebocoran arus dari perangkat input." },
  { "en": "Analogi sensor pada manusia?", "id": "Panca indera kita." },
  { "en": "Informasi dari mata ke otak?", "id": "Berupa impulse signal listrik." },
  { "en": "Fungsi adjuster pada sensor?", "id": "Mengatur sensitifitas pendeteksian sensor." },
  { "en": "Kelemahan sensor analog?", "id": "Harga jauh lebih mahal." },
  { "en": "Apa itu ADC?", "id": "Analog to Digital Converter." },
  { "en": "Kenapa PLC perlu ADC?", "id": "Hanya mampu mengolah data digital." },
  { "en": "Apa itu resolusi ADC?", "id": "Menentukan kerapatan dan ketelitian konversi." },
  { "en": "Apa itu kecepatan sampling ADC?", "id": "Seberapa sering sinyal dikonversi." },
  { "en": "Modul analog input Omron?", "id": "Contohnya CJ1W AD081." },
  { "en": "Resolusi modul AD081?", "id": "4000, dapat ditingkatkan jadi 8000." },
  { "en": "PLC Compact dengan analog I/O?", "id": "Contohnya CP1H tipe CPU XA." },
  { "en": "Definisi otomasi industri?", "id": "Sistem kerja dikendalikan peralatan elektronik." },
  { "en": "Contoh otomasi di pabrik?", "id": "Kontrol conveyor, mesin pengolahan." },
  { "en": "Contoh otomasi produksi pangan?", "id": "Kontrol pemanasan, pemotongan, pengemasan." },
  { "en": "Contoh otomasi untuk bisnis?", "id": "Mesin cuci, penjual tiket." },
  { "en": "Contoh otomasi kontrol lainnya?", "id": "Parkir, pintu air, lalu lintas." },
  { "en": "Apa itu Panel Operasi?", "id": "Panel berisi alat operasional manusia." },
  { "en": "Apa itu Panel Kontrol?", "id": "Panel berisi alat pengontrol mesin." },
  { "en": "Contoh alat di Panel Operasi?", "id": "Pushbutton, selector switch, lampu indikator." },
  { "en": "Contoh alat di Panel Kontrol?", "id": "Kontaktor, relay, PLC." },
  { "en": "Bagaimana normally open (N.O.) bekerja?", "id": "Menutup bila mendapat instruksi." },
  { "en": "Bagaimana normally close (N.C.) bekerja?", "id": "Membuka bila mendapat instruksi." },
  { "en": "Komponen internal relay?", "id": "Terdapat elektromagnet di dalamnya." },
  { "en": "Prinsip kerja bel kuis?", "id": "Saling mengunci (interlock)." },
  { "en": "Fungsi instruksi KEEP?", "id": "Menahan kondisi output tetap ON." },
  { "en": "Fungsi instruksi DIFD?", "id": "Mendeteksi sinyal tepi turun." },
  { "en": "Contoh PLC dengan memori 1K?", "id": "MicroLogic 1000 Controller." },
  { "en": "Contoh PLC dengan memori 64K?", "id": "SLC 500 Controller." },
  { "en": "Contoh PLC dengan memori 32M?", "id": "ControlLogix Controller." },
  { "en": "Bagaimana data tersimpan di memori?", "id": "Sebagai angka 1 dan 0." },
  { "en": "Apa itu Area Executive?", "id": "Memori permanen berisi program BIOS." },
  { "en": "Apa itu Area Aplikasi?", "id": "Memori untuk program dan data." },
  { "en": "Contoh bit khusus PLC Omron?", "id": "Always On (P_On), P_1s." },
  { "en": "Contoh register di PLC Omron?", "id": "Data Memori (D0 hingga D2047)." },
  { "en": "Apa itu fixed automation?", "id": "Otomasi tetap menggunakan peralatan mekanik." },
  { "en": "Apa itu flexible automation?", "id": "Otomasi fleksibel berbasis komputer." },
  { "en": "Aplikasi komputer real-time control?", "id": "Power station, chemical plant, refinery." },
  { "en": "Apa itu manipulator robot?", "id": "Lengan yang memberikan gerakan robot." },
  { "en": "Apa itu 'joint' pada robot?", "id": "Hubungan antara lengan (arm)." },
  { "en": "Bagian dasar manipulator?", "id": "Bisa kaku atau terpasang rel." },
  { "en": "Bagian tambahan manipulator?", "id": "Perluasan dari bagian dasar (lengan)." },
  { "en": "Jenis catu daya manipulator?", "id": "Elektrik, hidrolik, atau pneumatik." },
  { "en": "Apa itu gripper tunggal?", "id": "Satu peralatan genggam pada wrist." },
  { "en": "Apa itu gripper ganda?", "id": "Dua peralatan genggam pada wrist." },
  { "en": "Apa itu gripper mekanik?", "id": "Menggenggam objek dengan kontak fisik." },
  { "en": "Sumber tenaga gripper mekanik?", "id": "Pneumatik, hidrolik, dan elektrik." },
  { "en": "Komponen gripper vakum?", "id": "Mangkok dan sistem ruang hampa." },
  { "en": "Bahan mangkok vakum?", "id": "Terbuat dari bahan karet." },
  { "en": "Jenis sistem vakum?", "id": "Pompa vakum dan sistem venturi." },
  { "en": "Kelebihan magnet permanen?", "id": "Tidak butuh arus tambahan." },
  { "en": "Kelemahan magnet permanen?", "id": "Kesulitan dalam hal pengontrolan." },
  { "en": "Nama lain robot industri?", "id": "Arm Robot." },
  { "en": "Jenis robot industri?", "id": "Rectangular, cylindrical, sperical, articulated, gantry, SCARA." },
  { "en": "Apa itu antarmuka modul I/O?", "id": "Menghubungkan semua perangkat di lapangan." },
  { "en": "Fungsi pengalamatan pada PLC?", "id": "Mengindikasikan lokasi informasi di memori." },
  { "en": "Fungsi kartu I/O pada softPLC?", "id": "Antarmuka untuk perangkat di lapangan." },
  { "en": "Apa itu plug-in terminal?", "id": "Blok terminal yang dipasang ke modul." },
  { "en": "Kekurangan modul output high-density?", "id": "Tidak dapat menangani arus besar." },
  { "en": "Jenis modul I/O paling banyak?", "id": "Modul diskrit." },
  { "en": "Fungsi kabel twisted shield pair?", "id": "Menurunkan noise elektrikal yang tidak diinginkan." },
  { "en": "Apa itu unipolar?", "id": "Menerima sinyal input arah positif." },
  { "en": "Apa itu bipolar?", "id": "Menerima sinyal positif dan negatif." },
  { "en": "Fungsi process variable (PV)?", "id": "Mengukur karakteristik proses aktual." },
  { "en": "Fungsi setpoint (SP)?", "id": "Nilai target yang diinginkan." },
  { "en": "Apa itu error (E)?", "id": "Selisih antara SP dan PV." },
  { "en": "Fungsi leadscrew pada stepper motor?", "id": "Menghasilkan pergerakan linear." },
  { "en": "Contoh jaringan tingkat perangkat?", "id": "CANbus, Seriplex." },
  { "en": "Contoh jaringan perangkat proses?", "id": "Fieldbus, Profibus." },
  { "en": "Contoh jaringan CPU dan komputer?", "id": "Ethernet/IEEE 802.3." },
  { "en": "Standar industri Jepang untuk common?", "id": "Cenderung menggunakan common positive." },
  { "en": "Standar industri Jerman untuk common?", "id": "Cenderung menggunakan common negative." },
  { "en": "Contoh aktuator output?", "id": "Solenoid valves." },
  { "en": "Kenapa motor butuh relay?", "id": "Menarik arus besar saat start." },
  { "en": "Apa itu Basic I/O?", "id": "Alamat untuk input/output digital." },
  { "en": "Resolusi ADC 8 bit?", "id": "256 nilai diskrit." },
  { "en": "Resolusi ADC 12 bit?", "id": "4096 nilai diskrit." },
  { "en": "Kapan mode operasi modul AD off?", "id": "Atur saklar pada posisi normal." },
  { "en": "Cara konfigurasi I/O otomatis?", "id": "Pilih Option lalu Create." },
  { "en": "Tampilan default pembacaan memori?", "id": "Hexadecimal (Hexa)." },
  { "en": "Fungsi Normally Open (NO) Pushbutton?", "id": "Kontak tertutup saat ditekan." },
  { "en": "Fungsi Normally Close (NC) Pushbutton?", "id": "Kontak terbuka saat ditekan." },
  { "en": "Penyusun modul-modul ini?", "id": "Akhmad Wahyu Dani, ST, MT." },
  { "en": "Fakultas dan prodi penyusun?", "id": "Teknik, Teknik Elektro." },
  { "en": "Universitas asal modul?", "id": "Universitas Mercu Buana." },
  { "en": "Apa itu 'real-time control'?", "id": "Memproses informasi saat terjadi." },
  { "en": "Contoh hierarchical control?", "id": "Satu komputer host, banyak slave." },
  { "en": "Dasar 'distributed control'?", "id": "Sistem kompleks dibagi menjadi komponen." },
  { "en": "Istilah lain 'heterarchical control'?", "id": "Sistem tanpa koordinator terpusat." },
  { "en": "Siapa penulis drama 'RUR'?", "id": "Karel Capek dari Czech (1921)." },
  { "en": "Kapan Tiga Hukum Robot diperkenalkan?", "id": "Pertama kali lengkap tahun 1942." },
  { "en": "Apa Hukum Ke-Nol robot?", "id": "Tidak boleh mencelakakan umat manusia." },
  { "en": "Gerakan manipulator disebut juga?", "id": "Derajat kebebasan robot." },
  { "en": "Contoh kontroler sederhana zaman dulu?", "id": "Drum mekanik sekuensial." },
  { "en": "Contoh kontroler modern?", "id": "PLC (Programmable Logic Controller)." },
  { "en": "Fungsi 'wrist' pada robot?", "id": "Tempat dipasangnya efektor." },
  { "en": "Nama lain jari mekanik gripper?", "id": "Jaws." },
  { "en": "Sistem venturi menggunakan apa?", "id": "Nosel yang dilewati udara." },
  { "en": "Contoh pekerjaan robot industri?", "id": "Las, cat, perakitan, pick-and-place." },
  { "en": "Alamat input PLC Omron CP1E?", "id": "Contohnya 0.04 (word 0 bit 4)." },
  { "en": "Alamat output PLC Omron CP1E?", "id": "Contohnya 100.02 (word 100 bit 2)." },
  { "en": "Contoh working relay Omron?", "id": "W0.00 hingga W99.15." },
  { "en": "Contoh data timer/counter Omron?", "id": "Alamat 0 hingga 255." },
  { "en": "Apa itu 'sequence'?", "id": "Hal yang berlangsung terus menerus." },
  { "en": "Kondisi pada sequence control?", "id": "Input dari manusia atau mesin." },
  { "en": "Operasi pada sequence control?", "id": "Output yang menggerakkan mesin." },
  { "en": "Apa itu 'pemeriksaan statis'?", "id": "Pemeriksaan tanpa operasi otomatis." },
  { "en": "Contoh perangkat gerakan mekanikal?", "id": "Motor, solenoid, aktuator." },
  { "en": "Bagaimana jika kabel shield di-ground?", "id": "Hanya pada satu titik saja." },
  { "en": "Tujuan utama perawatan berkala?", "id": "Pencegahan terbesar terhadap kerusakan sistem." },
  { "en": "Penyebab umum masalah PLC?", "id": "Prosesor, hardware I/O, kabel, program." },
  { "en": "PLC Allen-Bradley menggunakan rack/slot?", "id": "PLC-5 dan SLC 500." },
  { "en": "PLC Allen-Bradley menggunakan tag-based?", "id": "ControlLogix." },
  { "en": "Berapa arus bocor pada triac?", "id": "Beberapa miliampere." },
  { "en": "Apa itu 'daya loop'?", "id": "Dapat dipasok oleh sensor/modul." },
  { "en": "Singkatan CJC pada modul thermocouple?", "id": "Cold Junction Compensating." },
  { "en": "Apa itu konverter A/D?", "id": "Analog-to-Digital converter." },
  { "en": "Apa itu konverter D/A?", "id": "Digital-to-Analog converter." },
  { "en": "Fungsi saklar pilih pada PLC?", "id": "Sebagai input diskrit ON/OFF." },
  { "en": "Fungsi tombol tekan pada PLC?", "id": "Sebagai input diskrit ON/OFF." },
  { "en": "Fungsi limit switch pada PLC?", "id": "Sebagai input diskrit ON/OFF." },
  { "en": "Fungsi motor starter pada PLC?", "id": "Sebagai output diskrit ON/OFF." },
  { "en": "Apa itu pemetaan memori?", "id": "Mengetahui jumlah dan alamat I/O." },
  { "en": "Jenis PLC menurut I/O?", "id": "Fixed I/O dan Modular I/O." },
  { "en": "Bahan baterai cadangan RAM?", "id": "Umumnya baterai lithium." },
  { "en": "Daya tahan baterai RAM?", "id": "Dua hingga lima tahun." },
  { "en": "Pengganti EPROM yang lebih modern?", "id": "EEPROM atau Flash EEPROM." },
  { "en": "Keunggulan EEPROM dibanding EPROM?", "id": "Bisa ditulis ulang secara elektrik." },
  { "en": "Apa itu 'bit-bit khusus'?", "id": "Menyimpan bit dengan fungsi spesial." },
  { "en": "Apa itu 'thumbwheel switch'?", "id": "Input untuk data register/word." },
  { "en": "Keluaran untuk seven segment?", "id": "Output dari data register/word." },
  { "en": "Resolusi ADC 16 bit?", "id": "65536 nilai diskrit." },
  { "en": "Resolusi ADC 24 bit?", "id": "16.777.216 nilai diskrit." },
  { "en": "Prinsip kerja ADC?", "id": "Perbandingan sinyal input & referensi." },
  { "en": "Apa itu Basic I/O Area?", "id": "Area untuk input/output digital." },
  { "en": "Apa itu Special I/O Area?", "id": "Area untuk input/output analog." },
  { "en": "Apa itu CPU Bus Unit Area?", "id": "Area untuk unit bus CPU." },
  { "en": "Apa itu 'hot spot' di panel?", "id": "Area panas dari catu daya." },
  { "en": "Perangkat apa menghasilkan gangguan elektromagnetik?", "id": "Mesin las, pemanas induksi." },
  { "en": "Apa itu 'crosstalk' antar kabel?", "id": "Interferensi listrik antar jalur sinyal." },
  { "en": "Apa itu 'bleeding resistor'?", "id": "Resistor untuk mengatasi arus bocor." },
  { "en": "Perangkat apa yang butuh supresi?", "id": "Relay, solenoid, motor starter." },
  { "en": "Apa itu 'pemeriksaan dinamis'?", "id": "Memeriksa logika program saat berjalan." },
  { "en": "Bagaimana cara kerja sensor?", "id": "Mengubah fenomena fisik jadi sinyal." },
  { "en": "Satuan cahaya untuk sensor?", "id": "Lux." },
  { "en": "Satuan tekanan untuk sensor?", "id": "Newton atau Pascal." },
  { "en": "Satuan suhu untuk sensor?", "id": "Celcius, Fahrenheit, Kelvin." },
  { "en": "Apa itu 'Normally Open'?", "id": "Kontak terbuka saat tidak aktif." },
  { "en": "Apa itu 'Normally Closed'?", "id": "Kontak tertutup saat tidak aktif." },
  { "en": "Contoh aplikasi CX Designer?", "id": "Simulasi Safety Crane, Pintu Garasi." },
  { "en": "Bahasa pemrograman modul ASCII?", "id": "BASIC atau C." },
  { "en": "Standar komunikasi modul ASCII?", "id": "RS-232C, RS-485, 20 mA." },
  { "en": "Gerakan stepper motor?", "id": "Bisa berupa putaran atau linear." },
  { "en": "Contoh 'field device'?", "id": "Sensor, tombol, motor, lampu." },
  { "en": "Industri apa menggunakan common positive?", "id": "Sebagian besar industri Jepang." },
  { "en": "Industri apa menggunakan common negative?", "id": "Sebagian besar industri Jerman." },
  { "en": "Metode pengalamatan Omron?", "id": "Pengelompokan 16 bit atau word." },
  { "en": "Bagaimana cara mengkonfigurasi PLC modular?", "id": "Melalui IO Table Unit Setup." },
  { "en": "Tanda input di program Omron?", "id": "Tanda I, contohnya I:0.00." },
  { "en": "Tanda output di program Omron?", "id": "Tanda Q, contohnya Q:1.00." },
  { "en": "Apa itu HMI?", "id": "Human Machine Interface." },
  { "en": "Fungsi HMI?", "id": "Antarmuka visual antara manusia-mesin." },
  { "en": "Kapan sirkuit emergency harus dirancang?", "id": "Selama tahap perancangan sistem." },
  { "en": "Bagaimana jika kabel I/O bersilangan?", "id": "Harus membentuk sudut 90 derajat." },
  { "en": "Bagian mana yang paling panas?", "id": "Catu daya (power supply)." },
  { "en": "Peran host di hierarchical control?", "id": "Mengatur semua fungsi kendali." },
  { "en": "Peran host di distributed control?", "id": "Koordinasi dan interaksi dengan user." },
  { "en": "Definisi 'reprogrammable' pada robot?", "id": "Dapat diprogram ulang sesuai kebutuhan." },
  { "en": "Apa itu 'degree of freedom'?", "id": "Jumlah sumbu gerakan robot." },
  { "en": "Fungsi 'base' pada manipulator?", "id": "Dasar atau fondasi robot." },
  { "en": "Fungsi 'appendage' pada manipulator?", "id": "Lengan atau perpanjangan dari base." },
  { "en": "Fungsi gripper magnetik?", "id": "Menghisap atau menarik komponen logam." },
  { "en": "Kelebihan 'flexible automation'?", "id": "Mudah diubah sesuai kebutuhan." },
  { "en": "Contoh 'flexible automation'?", "id": "Robot industri, mesin CNC." },
  { "en": "Tujuan utama otomasi produksi?", "id": "Meningkatkan efisiensi dan kualitas." },
  { "en": "Apa itu 'rack' pada PLC?", "id": "Kerangka untuk menempatkan modul-modul." },
  { "en": "Apa itu 'slot' pada PLC?", "id": "Posisi spesifik untuk modul." },
  { "en": "Definisi robot (Webster's Dictionary)?", "id": "Alat otomatis seperti fungsi manusia." },
  { "en": "Definisi robot (Encyclopaedia Britanica)?", "id": "Mesin otomatis pengganti usaha manusia." },
  { "en": "Definisi robot industri (RIA)?", "id": "Manipulator multifungsi yang dapat diprogram." },
  { "en": "Karakteristik robot masa kini?", "id": "Punya kecerdasan buatan, bisa belajar." },
  { "en": "Siapa yang menulis Tiga Hukum Robot?", "id": "Isaac Asimov." },
  { "en": "Apa itu 'derajat kebebasan'?", "id": "Jumlah sumbu gerakan pada robot." },
  { "en": "Fungsi PLC pada robot?", "id": "Sebagai kontroler yang dapat diprogram." },
  { "en": "Fungsi sistem drive (actuator)?", "id": "Menyebabkan gerakan bervariasi manipulator." },
  { "en": "Fungsi memori pada kontroler?", "id": "Menyimpan data gerakan robot." },
  { "en": "Catu daya untuk kontroler robot?", "id": "Umumnya menggunakan catu daya elektrik." },
  { "en": "Contoh efektor pada robot?", "id": "Peralatan las, penyemprot cat, penjepit." },
  { "en": "Klasifikasi gripper berdasarkan jumlah?", "id": "Gripper tunggal dan gripper ganda." },
  { "en": "Kapan pompa vakum digunakan?", "id": "Memberikan kehampaan tinggi biaya murah." },
  { "en": "Kapan sistem venturi digunakan?", "id": "Menggunakan nosel udara untuk kevakuman." },
  { "en": "Kelebihan electromagnet dibanding magnet permanen?", "id": "Lebih mudah untuk dikontrol." },
  { "en": "Kapan magnet permanen digunakan?", "id": "Pada lingkungan berbahaya seperti ledakan." },
  { "en": "Dominasi sistem pada robot industri?", "id": "Didominasi oleh sistem mekanikal." },
  { "en": "Dominasi sistem pada robot bergerak?", "id": "Didominasi oleh sistem elektrik." },
  { "en": "Contoh robot bergerak?", "id": "Robot beroda atau berkaki." },
  { "en": "Satuan 1 K memori?", "id": "1024 byte." },
  { "en": "Contoh file status?", "id": "Tabel status input/output." },
  { "en": "Tujuan baterai cadangan?", "id": "Menghindari kehilangan data pada RAM." },
  { "en": "Kelebihan EEPROM dari RAM?", "id": "Non-volatile, tidak butuh baterai." },
  { "en": "Bagaimana EEPROM dihapus?", "id": "Dihapus dan ditulis ulang elektrik." },
  { "en": "Apa itu 'I/O extension'?", "id": "Modul tambahan untuk PLC Compact." },
  { "en": "Apa itu 'Area Executive'?", "id": "Memori permanen berisi BIOS PLC." },
  { "en": "Apa itu 'Area Aplikasi'?", "id": "Memori untuk data program pengguna." },
  { "en": "Contoh sumber data register?", "id": "Input analog, thumbwheel switch." },
  { "en": "Contoh output data register?", "id": "Data seven segment, meter analog." },
  { "en": "Istilah lain untuk 'bit'?", "id": "Beberapa manual PLC menyebutnya 'relay'." },
  { "en": "Fungsi timer pada bel kuis?", "id": "Mereset sistem setelah waktu tertentu." },
  { "en": "Tujuan 'interlock' pada bel kuis?", "id": "Mencegah pemain lain menekan bel." },
  { "en": "Kapan sequence control digunakan?", "id": "Aplikasi kompleks maupun aplikasi umum." },
  { "en": "Contoh alat pendeteksi kondisi mesin?", "id": "Limit switch, proximity switch." },
  { "en": "Contoh alat penggerak mesin?", "id": "Motor, solenoid valve." },
  { "en": "Fungsi per pada pushbutton?", "id": "Mengembalikan tombol ke posisi semula." },
  { "en": "Apa akibat debu konduktif?", "id": "Menyebabkan hubung singkat pada sirkuit." },
  { "en": "Apa akibat koneksi I/O kendur?", "id": "Menyebabkan kegagalan fungsi dan kerusakan." },
  { "en": "Kapan perangkat I/O diinspeksi?", "id": "Untuk memastikan bekerja secara baik." },
  { "en": "Cara memeriksa ground loop?", "id": "Ukur tahanan kabel ground terputus." },
  { "en": "Tanda ground loop?", "id": "Alat ukur membaca tahanan rendah." },
  { "en": "Tindakan saat indikator FLT menyala?", "id": "Akses prosesor via alat pemrograman." },
  { "en": "Cara memeriksa pengkabelan input?", "id": "Aktifkan input, lihat indikator LED." },
  { "en": "Cara memeriksa pengkabelan output?", "id": "Gunakan fungsi 'force' atau program dummy." },
  { "en": "Contoh fenomena fisis?", "id": "Suhu, tekanan, cahaya, jarak." },
  { "en": "Bagaimana ADC 8-bit bekerja?", "id": "Mencuplik sinyal dalam 256 nilai." },
  { "en": "Bagaimana ADC 12-bit bekerja?", "id": "Mencuplik sinyal dalam 4096 nilai." },
  { "en": "Apa itu 'channel' I/O analog?", "id": "Tempat penyambungan sensor analog." },
  { "en": "Bagaimana Omron mengalokasikan memori Special I/O?", "id": "Dengan rumus 2000 + (n x 10)." },
  { "en": "Cara mengaktifkan channel analog?", "id": "Ubah status menjadi 'Enable'." },
  { "en": "Penyebab kegagalan belajar PLC?", "id": "Tidak mampu menerapkan dalam kondisi riil." },
  { "en": "Tujuan modul komunikasi?", "id": "Menghubungkan remote rack ke prosesor." },
  { "en": "Bahan dasar modul I/O?", "id": "Printed Circuit Board (PCB)." },
  { "en": "Kapan 'interposing relay' digunakan?", "id": "Untuk mengendalikan beban yang besar." },
  { "en": "Kapan modul PID digunakan?", "id": "Proses yang memerlukan kendali loop-tertutup." },
  { "en": "Tujuan modul positioning?", "id": "Memberikan kendali pergerakan presisi." },
  { "en": "Aplikasi modul BASIC/ASCII?", "id": "Barcode reader, robot, printer." },
  { "en": "Bagaimana stepper motor bergerak?", "id": "Merubah pulsa masuk jadi gerakan." },
  { "en": "Kapan sistem stepper butuh loop-tertutup?", "id": "Pada aplikasi respon tinggi." },
  { "en": "Contoh 'aktuator'?", "id": "Solenoid valve, motor." },
  { "en": "Kapan motor butuh sumber terpisah?", "id": "Karena menarik arus besar saat start." },
  { "en": "Bagaimana konfigurasi PLC modular dilakukan?", "id": "Melalui IO Table Unit Setup." },
  { "en": "Fungsi penglihatan pada manusia?", "id": "Sensor jarak, warna, bentuk." },
  { "en": "Apa itu 'shape recognition'?", "id": "Kemampuan mengenali bentuk objek." },
  { "en": "Output sensor berupa apa?", "id": "Besaran listrik (tegangan atau arus)." },
  { "en": "Apa itu sensor PNP?", "id": "Sensor tipe sourcing (common negative)." },
  { "en": "Apa itu sensor NPN?", "id": "Sensor tipe sinking (common positive)." },
  { "en": "Output sensor NPN saat deteksi?", "id": "Berubah menjadi 0 (Nol)." },
  { "en": "Output sensor PNP saat deteksi?", "id": "Berubah menjadi 24V (High)." },
  { "en": "Apa itu 'load' pada sensor?", "id": "Pin terminal input PLC." },
  { "en": "Fungsi transistor pada sensor?", "id": "Sebagai saklar elektronik." },
  { "en": "Fungsi 'opto-isolator' pada input?", "id": "Mengisolasi tegangan dan melindungi prosesor." },
  { "en": "Apa itu 'threshold detector'?", "id": "Mendeteksi tingkat ambang batas tegangan." },
  { "en": "Apa itu 'bridge rectifier'?", "id": "Mengubah tegangan AC menjadi DC." },
  { "en": "Apa itu 'lockout and tagout'?", "id": "Prosedur keamanan saat perawatan." },
  { "en": "Kapan filter panel diganti?", "id": "Saat perawatan berkala." },
  { "en": "Efek panas berlebih pada sirkuit?", "id": "Menyebabkan malfungsi pada sirkuit." },
  { "en": "Bagaimana menguji sirkuit emergency stop?", "id": "Memastikan daya ke I/O terputus." },
  { "en": "Apa itu 'program dummy rung'?", "id": "Program sederhana untuk tes output." },
  { "en": "Tujuan salinan program (backup)?", "id": "Untuk pemulihan jika terjadi kegagalan." },
  { "en": "Apa itu 'envelope work'?", "id": "Area kerja jangkauan robot." },
  { "en": "Fungsi 'serial option board'?", "id": "Menambahkan port komunikasi serial." },
  { "en": "Jenis port pada 'serial option board'?", "id": "RS-232C, RS-422A/485." },
  { "en": "Apa itu 'contact bounce'?", "id": "Loncatan sinyal saat kontak mekanis." },
  { "en": "Fungsi 'noise filter'?", "id": "Menghilangkan sinyal palsu/gangguan." },
  { "en": "Jenis PLC menurut 'build'?", "id": "Compact dan Modular." },
  { "en": "Apa itu 'bit delimiter'?", "id": "Pemisah dalam format alamat." },
  { "en": "Apa itu 'file delimiter'?", "id": "Pemisah dalam format alamat file." },
  { "en": "Contoh beban output induktif?", "id": "Koil relay, solenoid, motor." },
  { "en": "Apa itu 'grounding chassis'?", "id": "Menghubungkan kerangka perangkat ke ground." },
  { "en": "Penyebab noise pada sinyal analog?", "id": "Induksi medan magnet, ground loop." },
  { "en": "Satuan resolusi analog?", "id": "Jumlah bit (misal: 12-bit)." },
  { "en": "Apa itu 'interlock'?", "id": "Saling mengunci untuk mencegah konflik." },
  { "en": "Apa itu 'sirkuit snubber'?", "id": "Sirkuit untuk menekan lonjakan tegangan." },
  { "en": "Fungsi 'heat sink'?", "id": "Membantu membuang panas dari komponen." },
  { "en": "Tujuan utama belajar PLC?", "id": "Menerapkan dalam kondisi riil." },
  { "en": "Apa itu 'fixed I/O'?", "id": "I/O digabung bersamaan dengan CPU." },
  { "en": "Apa itu 'modular I/O'?", "id": "Menggunakan modul I/O eksternal." },
  { "en": "Contoh skema pengalamatan?", "id": "Rack/slot-based, tag-based, PC-based." },
  { "en": "Apa itu 'bit address'?", "id": "Alamat untuk terminal number." },
  { "en": "Bagaimana modul analog dialamatkan?", "id": "Menggunakan format pengalamatan word." },
  { "en": "Apa itu 'alias tag'?", "id": "Tag yang menunjuk ke base address." },
  { "en": "Format alamat PLC Siemens?", "id": "Menggunakan tanda persen (%)." },
  { "en": "Nama lain 'soft PLC'?", "id": "PC-based control." },
  { "en": "Bahan modul I/O?", "id": "Printed circuit board (PCB)." },
  { "en": "Tujuan plug-in terminal block?", "id": "Memudahkan penggantian modul." },
  { "en": "Contoh input diskrit?", "id": "Saklar, tombol, limit switch." },
  { "en": "Contoh output diskrit?", "id": "Indikator, relay, solenoid." },
  { "en": "Dari mana modul I/O bekerja?", "id": "Daya dari backplane sebuah rack." },
  { "en": "Tegangan operasi sirkuit internal PLC?", "id": "Biasanya 5VDC atau kurang." },
  { "en": "Fungsi LED status input?", "id": "Mengindikasikan status perangkat input." },
  { "en": "Fungsi LED status output?", "id": "Mengindikasikan status sinyal output." },
  { "en": "Komponen saklar elektronik output AC?", "id": "Menggunakan Triac." },
  { "en": "Kelemahan Triac?", "id": "Tidak dapat digunakan untuk beban DC." },
  { "en": "Kapan relay output digunakan?", "id": "Untuk perangkat tegangan AC/DC." },
  { "en": "Respon relay dibanding solid state?", "id": "Memiliki respon lebih lambat." },
  { "en": "Contoh kuantitas fisik analog?", "id": "Suhu, kecepatan, aliran, tekanan." },
  { "en": "Elemen utama modul input analog?", "id": "Konverter analog-to-digital (A/D)." },
  { "en": "Elemen utama modul output analog?", "id": "Konverter digital-to-analog (D/A)." },
  { "en": "Fungsi thermistor CJC?", "id": "Kompensasi suhu pada thermocouple." },
  { "en": "Sinyal apa paling sensitif gangguan?", "id": "Sinyal tegangan." },
  { "en": "Kecepatan hitung modul high-speed?", "id": "Bisa sampai 100 kHz." },
  { "en": "Contoh 'process variable' (PV)?", "id": "Level cairan, suhu, aliran." },
  { "en": "Fungsi servo pada modul positioning?", "id": "Point-to-point control, axis positioning." },
  { "en": "Apa itu 'main path'?", "id": "Terminal pada modul input PLC." },
  { "en": "Apa itu 'return path'?", "id": "Jalur kembali ke sumber tegangan." },
  { "en": "Mengapa motor butuh relay?", "id": "Menarik arus besar saat start." },
  { "en": "Konfigurasi mutlak pada PLC modular?", "id": "Diperlukan sebelum mulai pemrograman." },
  { "en": "Pengalamatan Omron dimulai dari mana?", "id": "Slot paling kiri dekat CPU." },
  { "en": "Ukuran word pada Omron?", "id": "16 bit." },
  { "en": "Berapa bit modul input 16-bit?", "id": "0.00 hingga 0.15." },
  { "en": "Berapa bit modul input 32-bit?", "id": "Menggunakan dua word (misal 2 & 3)." },
  { "en": "Apa itu 'IO Table Unit Setup'?", "id": "Menu untuk konfigurasi I/O." },
  { "en": "Bagaimana memulai konfigurasi IO otomatis?", "id": "Pilih Option lalu Create." },
  { "en": "Tipe timer di ladder diagram?", "id": "Contohnya 100ms Timer (BCD Type)." },
  { "en": "Tipe memori paling dasar?", "id": "Volatile dan non-volatile." },
  { "en": "Contoh memori volatile?", "id": "RAM (Random Access Memory)." },
  { "en": "Contoh memori non-volatile?", "id": "ROM, EPROM, EEPROM." },
  { "en": "Siapa yang mengisi program ROM?", "id": "Produsen PLC." },
  { "en": "Bagaimana data EPROM diubah?", "id": "Menggunakan perangkat khusus." },
  { "en": "Nama lain PLC Compact?", "id": "Jenis 'based'." },
  { "en": "Nama lain PLC Modular?", "id": "Sistem 'rack'." },
  { "en": "Fungsi flag pada bit khusus?", "id": "Status hasil operasi matematika/logika." },
  { "en": "Apa itu 'sequence'?", "id": "Urutan terjadinya suatu fenomena." },
  { "en": "Alat input untuk sequence control?", "id": "Dioperasikan manusia atau mendeteksi mesin." },
  { "en": "Alat output untuk sequence control?", "id": "Memberitahu kondisi atau menggerakkan mesin." },
  { "en": "Bagaimana cara kerja relay?", "id": "Elektromagnet menarik lempengan besi." },
  { "en": "Penyebab utama 'real-time control' sulit?", "id": "Harus memproses data sangat cepat." },
  { "en": "Tantangan 'distributed control'?", "id": "Permasalahan komunikasi data antar prosesor." },
  { "en": "Karakteristik 'heterarchical control'?", "id": "Mirip mekanisme kerja otak manusia." },
  { "en": "Fungsi robot dalam produksi?", "id": "Pekerjaan berat, berbahaya, berulang, kotor." },
  { "en": "Contoh aplikasi robot non-produksi?", "id": "Penjelajahan, pertambangan, pencarian." },
  { "en": "Bagaimana rel membantu manipulator?", "id": "Memungkinkan robot bergerak antar lokasi." },
  { "en": "Contoh efektor selain gripper?", "id": "Alat las, penyemprot cat." },
  { "en": "Beda pompa vakum dan venturi?", "id": "Pompa pakai piston, venturi pakai nosel." },
  { "en": "Kapan gripper magnetik ideal?", "id": "Menangani logam yang berlubang." },
  { "en": "Efek lingkungan pada PLC?", "id": "Mempengaruhi penempatan dan tata letak." },
  { "en": "Fungsi perangkat pemutus darurat?", "id": "Dipasang di lokasi mudah diakses." },
  { "en": "Di mana komponen magnetik dipasang?", "id": "Area terpisah dari komponen PLC." },
  { "en": "Fungsi saringan pada fan panel?", "id": "Menjaga sirkulasi udara tetap bersih." },
  { "en": "Cara memisahkan sinyal AC/DC?", "id": "Tidak dirutekan paralel dalam duct." },
  { "en": "Fungsi NEC Article 250?", "id": "Data keamanan pembumian komponen listrik." },
  { "en": "Penyebab sinyal input salah?", "id": "Sumber AC tidak stabil." },
  { "en": "Fungsi 'penekan lonjakan'?", "id": "Melindungi dari lonjakan tegangan." },
  { "en": "Tujuan kode warna kabel?", "id": "Identifikasi cepat karakteristik sinyal." },
  { "en": "Cara memastikan terminasi kabel bagus?", "id": "Tarik kabel setelah dikencangkan." },
  { "en": "Alat untuk memasang skun/ferrules?", "id": "Tang skun atau ferulles." },
  { "en": "Apa itu 'ground loop'?", "id": "Dua atau lebih jalur ground." },
  { "en": "Bagaimana mencegah 'ground loop'?", "id": "Hubungkan shield ke ground satu sisi." },
  { "en": "Tanda baterai PLC lemah?", "id": "Indikator BATT (merah) menyala." },
  { "en": "Kapan modul input perlu diganti?", "id": "Rusak, tidak mengenali sinyal." },
  { "en": "Tanda tegangan sinyal rendah?", "id": "Turun 10-15% dari normal." },
  { "en": "Tanda modul output rusak?", "id": "Dapat perintah ON, indikator tidak ON." },
  { "en": "Kapan sekring eksternal dipasang?", "id": "Jika output tidak punya sekring internal." },
  { "en": "Apa itu 'pohon pelacakan masalah'?", "id": "Diagram alur untuk troubleshooting." },
  { "en": "Tujuan resolusi tinggi pada ADC?", "id": "Memberi ketelitian hasil konversi baik." },
  { "en": "Hubungan bit ADC dan nilai diskrit?", "id": "Semakin tinggi bit, semakin banyak nilai." },
  { "en": "Apa itu Link Area?", "id": "Area memori untuk link data." },
  { "en": "Apa itu Internal I/O Area?", "id": "Area memori untuk I/O internal." },
  { "en": "Cara memeriksa memori PLC?", "id": "Klik dua kali 'Memory' di Project Tree." },
  { "en": "Bagaimana sensor 3-kabel bekerja?", "id": "Dua kabel sumber, satu kabel sinyal." },
  { "en": "Fungsi reed switch di silinder?", "id": "Indikator batas depan dan belakang." },
  { "en": "Bagaimana 'sequence control' dioperasikan?", "id": "Sesuai urutan yang telah ditentukan." },
  { "en": "Komponen utama reed switch?", "id": "Lembaran daun tembaga sensitif magnet." },
  { "en": "Alat untuk memprogram PLC Omron?", "id": "Software CX-Programmer." },
  { "en": "Alat untuk mendesain HMI Omron?", "id": "Software CX-Designer." },
  { "en": "Apa itu 'sistem arsitektur terbuka'?", "id": "Sistem yang bisa menggantikan PLC proprietary." },
  { "en": "Contoh sensor dengan kebocoran arus?", "id": "Proximity switch." },
  { "en": "Fungsi 'MOV' (Metal Oxide Varistor)?", "id": "Melindungi dari lonjakan tegangan." },
  { "en": "Di mana sirkuit emergency stop dipasang?", "id": "Di lokasi yang mudah diakses operator." },
  { "en": "Apa itu 'fault' pada PLC?", "id": "Kesalahan pada sistem." },
  { "en": "Tujuan tata letak yang cermat?", "id": "Komponen mudah diakses dan dirawat." },
  { "en": "Keuntungan PLC dekat mesin?", "id": "Meminimalisasi pemakaian kabel." },
  { "en": "Mengapa pintu panel harus terbuka penuh?", "id": "Kemudahan akses pemeriksaan dan perawatan." },
  { "en": "Fungsi penutup belakang panel?", "id": "Memudahkan pemasangan komponen." },
  { "en": "Dimana rack I/O remote ditempatkan?", "id": "Di dalam panel lokasi remote." },
  { "en": "Penempatan starter magnetik?", "id": "Di bagian atas panel terpisah." },
  { "en": "Penempatan komponen elektromekanikal?", "id": "Di area tersendiri dari PLC." },
  { "en": "Tujuan pemisahan modul I/O?", "id": "Meminimalisasi interferensi crosstalk." },
  { "en": "Bagaimana kabel shield dihubungkan?", "id": "Hanya satu ujungnya ke ground." },
  { "en": "Fungsi sirkuit MCR/SCR?", "id": "Memutus daya ke sistem I/O." },
  { "en": "Jenis kabel untuk sinyal rendah?", "id": "Sebaiknya menggunakan kabel shield." },
  { "en": "Apa itu 'pemeriksaan visual'?", "id": "Memastikan semua komponen terpasang benar." },
  { "en": "Kapan program 'dummy rung' digunakan?", "id": "Untuk memeriksa setiap output." },
  { "en": "Mengapa cadangan komponen PLC penting?", "id": "Mengganti komponen yang sering rusak." },
  { "en": "Penyebab pembacaan sinyal salah?", "id": "Interferensi dari ground loop." },
  { "en": "Indikator RUN kedap-kedip artinya?", "id": "Proses transfer program sedang berlangsung." },
  { "en": "Indikator FLT kedap-kedip artinya?", "id": "Kesalahan besar pada prosesor/memori." },
  { "en": "Tujuan sistem kendali elektrik?", "id": "Mengolah informasi dari sensor." },
  { "en": "Output sensor PNP saat normal?", "id": "Tegangannya adalah 0V." },
  { "en": "Output sensor NPN saat normal?", "id": "Tegangannya adalah 24V." },
  { "en": "Apa itu 'loop semu'?", "id": "Loop palsu karena kabel terkelupas." },
  { "en": "Contoh penggunaan reed switch?", "id": "Indikator batas silinder pneumatik." },
  { "en": "Tujuan otomatisasi dalam bisnis?", "id": "Efisiensi dan konsistensi operasional." },
  { "en": "Apa itu 'phototransistor'?", "id": "Komponen pada optical isolator." },
  { "en": "Fungsi phototransistor?", "id": "Mengubah cahaya menjadi sinyal listrik." },
  { "en": "Apa yang disimpan dalam memori?", "id": "Informasi, program, dan data." },
  { "en": "Berapa input/output MicroLogic 1000?", "id": "20 input, 14 output." },
  { "en": "Berapa input/output SLC 500?", "id": "Hingga 4096 input/output." },
  { "en": "Berapa input/output ControlLogix?", "id": "Hingga 128.000 input/output." },
  { "en": "Apa itu 'alamat memori'?", "id": "Lokasi penyimpanan binary word." },
  { "en": "Fungsi tabel input/output?", "id": "Direvisi terus-menerus oleh CPU." },
  { "en": "Apa itu 'burn' pada ROM?", "id": "Proses memasukkan data oleh produsen." },
  { "en": "Tugas utama robot?", "id": "Menggantikan pekerjaan manusia yang sulit." },
  { "en": "Apa itu 'synchromotion' robot?", "id": "Beberapa robot bekerja secara serempak." },
  { "en": "Efek 'synchromotion'?", "id": "Proses industri lebih efektif efisien." },
  { "en": "Apa itu 'actuator'?", "id": "Penggerak yang menghasilkan gerakan." },
  { "en": "Bahan 'jaws' pada gripper?", "id": "Dapat dilepas dan dipasang (fleksibel)." },
  { "en": "Cara kerja gripper vakum?", "id": "Membuat tekanan negatif untuk menghisap." },
  { "en": "Bagaimana 'electromagnet' bekerja?", "id": "Menggunakan sumber arus listrik." },
  { "en": "Sensor pada robot bergerak?", "id": "Jumlah dan variasinya banyak." },
  { "en": "Apa itu 'gantry robot'?", "id": "Robot dengan area kerja persegi." },
  { "en": "Apa itu 'SCARA robot'?", "id": "Robot untuk perakitan kecepatan tinggi." },
  { "en": "Bagaimana PLC modular berkomunikasi?", "id": "Melalui modul komunikasi." },
  { "en": "Bagaimana modul I/O digroup?", "id": "Untuk memudahkan pengkabelan." },
  { "en": "Fungsi 'sekering' pada output?", "id": "Melindungi dari beban berlebih." },
  { "en": "Berapa jarak modul komunikasi?", "id": "Bisa mencapai hingga 10.000 feet." },
  { "en": "Contoh 'aktuator' output PLC?", "id": "Solenoid valves untuk pneumatik/hidrolik." },
  { "en": "Alamat CIO Special I/O Omron?", "id": "Mulai dari CIO 2000." },
  { "en": "Resolusi 1-bit mewakili apa?", "id": "Dua kelompok kondisi (0 atau 1)." },
  { "en": "Resolusi 4-bit mewakili apa?", "id": "16 bagian (nilai 0-15)." },
  { "en": "Apa itu 'unit number'?", "id": "Nomor identifikasi modul Special I/O." },
  { "en": "Range sinyal input bipolar?", "id": "Contohnya -10V hingga +10V." },
  { "en": "Range sinyal input unipolar?", "id": "Contohnya 0V hingga +10V." },
  { "en": "Jenis file program PLC?", "id": "Ladder diagram, function block, dll." },
  { "en": "Penyebab kegagalan belajar otomasi?", "id": "Tidak paham perangkat keras nyata." },
  { "en": "Efek 'contact bounce'?", "id": "Menimbulkan sinyal palsu." },
  { "en": "Tegangan referensi sensor analog?", "id": "Contohnya 5 volt atau 10 volt." },
  { "en": "Bagaimana 'loop tertutup' bekerja?", "id": "Menggunakan umpan balik (feedback)." },
  { "en": "Contoh 'gangguan' (disturbance) proses?", "id": "Perubahan muatan material." },
  { "en": "Fungsi 'ladder diagram'?", "id": "Representasi grafis program PLC." },
  { "en": "Apa itu 'proprietary PLC'?", "id": "PLC khusus dari satu produsen." },
  { "en": "Bagaimana memilih ukuran kabel I/O?", "id": "Harus mampu menangani arus maksimum." },
  { "en": "Apa itu 'saklar pilih'?", "id": "Saklar dengan beberapa posisi pilihan." },
  { "en": "Fungsi 'buzzer' sebagai output?", "id": "Memberikan sinyal suara (audio)." },
  { "en": "Bagaimana PLC Modular dikonfigurasi?", "id": "Berdasarkan susunan modul pada Rack." },
  { "en": "Mengapa PLC butuh catu daya?", "id": "Sumber tenaga untuk operasi." },
  { "en": "Apa itu 'word' memori?", "id": "Sekelompok bit (biasanya 16)." },
  { "en": "Kelemahan memori 'volatile'?", "id": "Data hilang saat daya mati." },
  { "en": "Mengapa ROM disebut 'Read-Only'?", "id": "Data tidak dapat diubah." },
  { "en": "Bagaimana 'RAM' menyimpan data?", "id": "Secara sementara saat ada daya." },
  { "en": "Apa itu 'Rack' pada PLC Modular?", "id": "Tempat menampung modul-modul terpisah." },
  { "en": "Bagaimana 'flexible automation' bekerja?", "id": "Menggunakan sistem pengatur berbasis komputer." },
  { "en": "Contoh 'fixed automation'?", "id": "Mesin perakitan dengan fungsi tunggal." },
  { "en": "Apa itu 'end effector'?", "id": "Nama lain untuk efektor robot." },
  { "en": "Mengapa robot butuh 'kontroler'?", "id": "Sebagai 'otak' untuk mengontrol gerakan." },
  { "en": "Apa itu 'open loop'?", "id": "Sistem kontrol tanpa umpan balik." },
  { "en": "Apa itu 'close loop'?", "id": "Sistem kontrol dengan umpan balik." },
  { "en": "Jenis robot berdasarkan 'work envelope'?", "id": "Rectangular, Cylindrical, Sperical, dll." },
  { "en": "Fungsi 'field device'?", "id": "Perangkat input/output di lapangan." },
  { "en": "Apa itu 'common negative'?", "id": "Common terhubung ke 0V (negatif)." },
  { "en": "Apa itu 'common positive'?", "id": "Common terhubung ke 24V (positif)." },
  { "en": "Kapan 'HMI' digunakan?", "id": "Untuk visualisasi dan kontrol proses." },
  { "en": "Bagaimana sinyal analog dikonversi?", "id": "Menjadi nilai digital oleh ADC." },
  { "en": "Bagaimana nilai digital dikonversi?", "id": "Menjadi sinyal analog oleh DAC." },
  { "en": "Apa itu 'DIP Switch'?", "id": "Saklar kecil untuk pengaturan konfigurasi." },
  { "en": "Fungsi DIP switch di modul?", "id": "Mengatur mode atau tipe sinyal." },
  { "en": "Apa itu 'pulse train'?", "id": "Rangkaian pulsa untuk stepper motor." },
  { "en": "Fungsi 'pohon pelacakan masalah'?", "id": "Panduan sistematis untuk menemukan kesalahan." },
  { "en": "Mengapa perlu mem-backup program?", "id": "Untuk pemulihan jika terjadi kegagalan." },
  { "en": "Apa itu 'off-state leakage current'?", "id": "Arus bocor saat perangkat mati." },
  { "en": "Bagaimana 'sistem venturi' bekerja?", "id": "Udara bertekanan menciptakan kevakuman." },
  { "en": "Bagaimana 'pompa vakum' bekerja?", "id": "Piston mekanis menciptakan kevakuman." },
  { "en": "Tujuan 'Tiga Hukum Robot'?", "id": "Menjamin keamanan interaksi manusia-robot." },
  { "en": "Apa itu 'artificial intelligence'?", "id": "Kecerdasan buatan pada mesin/robot." },
  { "en": "Fungsi 'CPU' pada PLC?", "id": "Mengeksekusi program dan memproses data." },
  { "en": "Bagaimana cara kerja 'interlock'?", "id": "Satu aksi mencegah aksi lainnya." },
  { "en": "Tujuan utama PLC?", "id": "Mengotomatisasi proses industri." },
  { "en": "Apa itu 'sistem manufaktur'?", "id": "Proses produksi industri." },
  { "en": "Fungsi 'trafo isolasi'?", "id": "Melindungi PLC dari gangguan listrik." },
  { "en": "Fungsi 'safety relay'?", "id": "Bagian dari sirkuit pengaman." },
  { "en": "Efek getaran berlebih?", "id": "Dapat merusak komponen PLC." },
  { "en": "Bagaimana cara merutekan kabel daya?", "id": "Terpisah dari kabel sinyal." },
  { "en": "Apa itu 'grounding'?", "id": "Menghubungkan sistem ke tanah." },
  { "en": "Fungsi utama 'grounding'?", "id": "Keamanan dan mengurangi gangguan." },
  { "en": "Apa itu 'transien tegangan'?", "id": "Lonjakan tegangan singkat." },
  { "en": "Fungsi 'ferrules' pada kabel?", "id": "Melindungi ujung kabel." },
  { "en": "Apa itu 'skun' kabel?", "id": "Konektor di ujung kabel." },
  { "en": "Bagaimana 'MOV' bekerja?", "id": "Seperti dua dioda zener." },
  { "en": "Kapan 'MOV' menjadi sirkuit tertutup?", "id": "Saat ada tegangan puncak." },
  { "en": "Apa itu 'mode TEST' PLC?", "id": "Eksekusi program tanpa mengaktifkan output." },
  { "en": "Bagaimana 'watchdog timer' bekerja?", "id": "Harus direset oleh prosesor." },
  { "en": "Apa akibat scan terlalu lama?", "id": "Menyebabkan kesalahan besar." },
  { "en": "Tanda sekring output putus?", "id": "Indikator status sekring menyala." },
  { "en": "Alat ukur untuk cek 'ground loop'?", "id": "Ohm meter." },
  { "en": "Apa itu 'transducer'?", "id": "Mengubah satu bentuk energi lain." },
  { "en": "Apa itu 'preamp'?", "id": "Penguat sinyal awal." },
  { "en": "Output sinyal sensor apa?", "id": "Tegangan atau arus listrik." },
  { "en": "Fungsi kabel hitam pada sensor?", "id": "Sebagai kabel sinyal keluaran." },
  { "en": "Fungsi kabel cokelat pada sensor?", "id": "Terhubung ke sumber tegangan positif." },
  { "en": "Fungsi kabel biru pada sensor?", "id": "Terhubung ke sumber tegangan negatif." },
  { "en": "Apa itu 'load' pada diagram?", "id": "Beban, atau perangkat output." },
  { "en": "Bagaimana 'reed switch' aktif?", "id": "Oleh medan magnet di sekitarnya." },
  { "en": "Urutan cuci mobil otomatis?", "id": "Air, deterjen, kain, air, dilap." },
  { "en": "Apa itu 'kontak bergerak'?", "id": "Bagian kontak yang bisa bergerak." },
  { "en": "Apa itu 'kontak diam'?", "id": "Bagian kontak yang tidak bergerak." },
  { "en": "Bagaimana normally open bekerja?", "id": "Terbuka saat tidak ada aksi." },
  { "en": "Bagaimana normally close bekerja?", "id": "Tertutup saat tidak ada aksi." },
  { "en": "Fungsi 'coil' pada relay?", "id": "Menciptakan medan magnet." },
  { "en": "Apa itu 'binary digit'?", "id": "Bit (angka 0 atau 1)." },
  { "en": "Apa itu 'processor memory'?", "id": "Memori di dalam unit prosesor." },
  { "en": "Contoh 'input devices'?", "id": "Tombol, saklar, sensor." },
  { "en": "Contoh 'output devices'?", "id": "Lampu, motor, buzzer, relay." },
  { "en": "Apa itu 'input image table'?", "id": "Tabel status semua input." },
  { "en": "Apa itu 'output image table'?", "id": "Tabel status semua output." },
  { "en": "Bagaimana PLC modular berkembang?", "id": "Dengan menambah modul pada rack." },
  { "en": "Fungsi 'internal relay'?", "id": "Sebagai relay bantu dalam program." },
  { "en": "Apa itu 'special relay'?", "id": "Relay dengan fungsi khusus." },
  { "en": "Tantangan kendali komputer digital awal?", "id": "Sistem eksternal bersifat analog." },
  { "en": "Kapan kendali komputer digital dimulai?", "id": "Dimulai pada tahun 1970-an." },
  { "en": "Apa itu 'unintelligent slave devices'?", "id": "Sensor, aktuator, tanpa prosesor." },
  { "en": "Tugas host di 'hierarchical control'?", "id": "I/O, eksekusi algoritma, interaksi." },
  { "en": "Paradigma industri tahun 1980-an?", "id": "Memaksimalkan penggunaan mikroprosesor." },
  { "en": "Fungsi 'data communication links'?", "id": "Menghubungkan host dan prosesor lokal." },
  { "en": "Apa itu 'intelligent nodes'?", "id": "Prosesor yang bekerja bersama." },
  { "en": "Tugas robot yang umum?", "id": "Berat, berbahaya, berulang, dan kotor." },
  { "en": "Apa itu 'power supply' robot?", "id": "Unit penyedia tenaga." },
  { "en": "Fungsi 'gripper' pada robot?", "id": "Menggenggam dan menahan objek." },
  { "en": "Tujuan 'synchromotion'?", "id": "Membuat proses lebih efektif efisien." },
  { "en": "Bagaimana 'gantry robot' bergerak?", "id": "Secara linear pada sumbu X-Y-Z." },
  { "en": "Aplikasi umum 'SCARA robot'?", "id": "Pick and place, perakitan." },
  { "en": "Tipe modul I/O paling umum?", "id": "Diskrit (ON/OFF)." },
  { "en": "Kapan 'triac' menghantarkan arus?", "id": "Saat dipicu oleh sinyal." },
  { "en": "Fungsi 'leadscrew'?", "id": "Mengubah gerak putar jadi linear." },
  { "en": "Apa itu 'baud rate'?", "id": "Kecepatan transmisi data." },
  { "en": "Fungsi 'terminator' pada jaringan?", "id": "Mencegah pantulan sinyal." },
  { "en": "Standar kabel untuk sensor analog?", "id": "Twisted-pair shielded cable." },
  { "en": "Fungsi 'set value' pada timer?", "id": "Menentukan durasi waktu timer." },
  { "en": "Apa itu 'BCD Type' timer?", "id": "Timer menggunakan format BCD." },
  { "en": "Bagaimana cara kerja 'KEEP'?", "id": "Menahan output ON setelah input mati." },
  { "en": "Cara mereset instruksi 'KEEP'?", "id": "Dengan memberikan sinyal ke input reset." },
  { "en": "Apa itu 'pulse' pada buzzer?", "id": "Sinyal ON-OFF berirama." },
  { "en": "Fungsi 'internal relay' di ladder?", "id": "Sebagai bit memori perantara." },
  { "en": "Contoh sinyal referensi analog?", "id": "Tegangan 5 volt." },
  { "en": "Bagaimana hasil konversi ADC dihitung?", "id": "(Sinyal/Referensi) x Resolusi." },
  { "en": "Apa itu 'DeviceNet'?", "id": "Jenis jaringan komunikasi industri." },
  { "en": "Apa itu 'factory setting'?", "id": "Pengaturan awal dari pabrik." },
  { "en": "Bagaimana mengubah mode baca modul?", "id": "Melalui 'voltage/current switch'." },
  { "en": "Fungsi 'Memory Cassette Slot'?", "id": "Untuk memori eksternal." },
  { "en": "Fungsi 'Expansion I/O Unit Connector'?", "id": "Menghubungkan modul ekspansi." },
  { "en": "Apa itu 'peripheral USB port'?", "id": "Port USB untuk koneksi periferal." },
  { "en": "Bagaimana 'sinking input' bekerja?", "id": "Menerima arus dari sensor sourcing." },
  { "en": "Bagaimana 'sourcing input' bekerja?", "id": "Memberi arus ke sensor sinking." },
  { "en": "Fungsi 'I/O Control Unit'?", "id": "Menghubungkan Expansion Rack." },
  { "en": "Posisi 'I/O Control Unit'?", "id": "Langsung di sebelah kanan CPU." },
  { "en": "Apa itu 'end cover'?", "id": "Penutup di ujung rack PLC." },
  { "en": "Berapa modul Pulse I/O maksimal?", "id": "Hingga dua modul." },
  { "en": "Posisi modul Pulse I/O?", "id": "Di sebelah kiri CPU Unit." },
  { "en": "Bagaimana membersihkan heat sink?", "id": "Dengan udara bertekanan atau kuas." },
  { "en": "Tanda-tanda koneksi I/O kendur?", "id": "Operasi tidak stabil, error intermiten." },
  { "en": "Bagaimana 'lockout' dilakukan?", "id": "Memasang gembok pada sumber daya." },
  { "en": "Bagaimana 'tagout' dilakukan?", "id": "Memasang label peringatan." },
  { "en": "Efek 'ground loop'?", "id": "Menimbulkan noise pada sinyal." },
  { "en": "Tanda tegangan sinyal input rendah?", "id": "Dibawah 10-15% dari nilai normal." },
  { "en": "Fungsi 'phototransistor' pada opto-isolator?", "id": "Mendeteksi cahaya dari LED." },
  { "en": "Bagaimana 'DIFD' (Differentiate Down) bekerja?", "id": "Aktif sesaat saat sinyal OFF." },
  { "en": "Fungsi 'DIFD' pada pintu garasi?", "id": "Mendeteksi mobil sudah lewat." },
  { "en": "Kapan 'limit switch' aktif?", "id": "Saat objek mencapai batas mekanis." },
  { "en": "Fungsi 'limit switch' pintu garasi?", "id": "Mendeteksi pintu terbuka/tertutup penuh." },
  { "en": "Apa itu 'HMI'?", "id": "Human Machine Interface." },
  { "en": "Fungsi HMI?", "id": "Antarmuka grafis untuk operator." },
  { "en": "Apa itu 'conveyor'?", "id": "Ban berjalan untuk memindahkan barang." },
  { "en": "Apa itu 'brush motor'?", "id": "Motor penggerak sikat pembersih." },
  { "en": "Abstrak modul 13?", "id": "ADC, pengubah sinyal analog digital." },
  { "en": "Tujuan Sub-CPMK 4.5?", "id": "Merancang, memprogram sensor analog PLC." },
  { "en": "Fungsi terminal modular?", "id": "Tempat koneksi pengkabelan PLC." },
  { "en": "Contoh komponen selain PLC?", "id": "Trafo isolasi, catu daya, relay." },
  { "en": "Apa itu 'remote I/O'?", "id": "Modul I/O di lokasi jauh." },
  { "en": "Singkatan NEMA?", "id": "National Electrical Manufacturers Association." },
  { "en": "Fungsi jendela akrilik panel?", "id": "Kemudahan instalasi dan perawatan." },
  { "en": "Apa itu 'hot spot'?", "id": "Area panas dari peralatan listrik." },
  { "en": "Contoh perangkat 'saluran masuk'?", "id": "Transformer, pemutus daya." },
  { "en": "Bagaimana kabel TTL/analog dirutekan?", "id": "Tidak paralel dengan saluran AC." },
  { "en": "Berapa ukuran kabel ground rack?", "id": "Ukuran 4mm atau rekomendasi produsen." },
  { "en": "Apa itu 'common AC source'?", "id": "Sumber AC sama untuk sistem." },
  { "en": "Logika sirkuit emergency?", "id": "Harus menggunakan logika sederhana." },
  { "en": "Singkatan MCR?", "id": "Master Control Relay." },
  { "en": "Singkatan SCR?", "id": "Safety Control Relay." },
  { "en": "Bagaimana jika CPU mati?", "id": "Semua input dan output berhenti." },
  { "en": "Contoh metode pelabelan kabel?", "id": "Shrink-tube atau tape." },
  { "en": "Apa itu 'ground loops'?", "id": "Dihindari dengan koneksi ground satu-sisi." },
  { "en": "Contoh perangkat solid-state?", "id": "Transistor dan triac." },
  { "en": "Penyebab 'false signal'?", "id": "Contact bounce atau interferensi elektrik." },
  { "en": "Fungsi 'reverse-bias' dioda?", "id": "Meredam lonjakan tegangan induksi." },
  { "en": "Kapan 'mode stop' PLC digunakan?", "id": "Mencegah operasi otomatis saat pemeriksaan." },
  { "en": "Indikator diagnostik sistem?", "id": "AC OK, DC OK, Processor OK." },
  { "en": "Tanda kesalahan pengkabelan input?", "id": "Indikator LED menyala tidak sesuai." },
  { "en": "Apa itu 'program dummy'?", "id": "Program sederhana untuk pengujian." },
  { "en": "Tujuan 'lockout and tagout'?", "id": "Keamanan personil saat perawatan." },
  { "en": "Penyebab 'ground loop'?", "id": "Dua jalur listrik pada ground." },
  { "en": "Abstrak modul 9?", "id": "Memori penyimpan informasi, program, data." },
  { "en": "Tujuan Sub-CPMK 4.2?", "id": "Memahami dan membuat pemetaan memori." },
  { "en": "Fungsi 'writing' ke memori?", "id": "Menyimpan data ke dalam memori." },
  { "en": "Fungsi 'reading' dari memori?", "id": "Mengambil data dari memori." },
  { "en": "Format himpunan data memori?", "id": "Umumnya format 16 bit word." },
  { "en": "Bagaimana 'tabel status' direvisi?", "id": "Terus-menerus oleh CPU." },
  { "en": "Apa itu 'bit asosiasi'?", "id": "Bit yang terkait dengan perangkat I/O." },
  { "en": "Fungsi 'working relay'?", "id": "Sebagai bit atau koil internal." },
  { "en": "Contoh 'flag' pada PLC?", "id": "Status hasil operasi matematika." },
  { "en": "Fungsi memori program pengguna?", "id": "Menyimpan program kontrol PLC." },
  { "en": "Abstrak modul 11?", "id": "Sensor menangkap fenomena fisis/kimia." },
  { "en": "Tujuan Sub-CPMK 4.4?", "id": "Merancang dan memprogram Sensor Digital." },
  { "en": "Analogi mata manusia?", "id": "Sensor jarak, warna, bentuk." },
  { "en": "Bagaimana sensor mengirim informasi?", "id": "Melalui besaran listrik." },
  { "en": "Rentang tegangan sensor analog?", "id": "0-5V, 1-5V, 0-10V, +/-10V." },
  { "en": "Rentang arus sensor analog?", "id": "4mA hingga 20mA." },
  { "en": "Kapan 'sourcing' dan 'sinking' berlaku?", "id": "Hanya pada listrik DC." },
  { "en": "Bagaimana sensor NPN bekerja?", "id": "Mirip transistor NPN." },
  { "en": "Bagaimana sensor PNP bekerja?", "id": "Mirip transistor PNP." },
  { "en": "Hambatan pada reed switch?", "id": "Load (modul PLC) dan internal (r)." },
  { "en": "Abstrak modul 14?", "id": "Otomasi dibedakan jadi fixed/flexible." },
  { "en": "Tujuan Sub-CPMK 5.2?", "id": "Memahami penggunaan komputer dan robot." },
  { "en": "Contoh otomasi sehari-hari?", "id": "Mesin cuci, pengering tangan otomatis." },
  { "en": "Kapan kendali komputer digital berkembang?", "id": "Mulai tahun 1970-an." },
  { "en": "Contoh aplikasi 'real-time control'?", "id": "Kontrol pembangkit listrik, kilang." },
  { "en": "Apa itu 'mimic panels'?", "id": "Panel visual status sistem." },
  { "en": "Arti 'robota'?", "id": "Bekerja atau pekerja." },
  { "en": "Karakteristik robot masa kini?", "id": "Dapat berinteraksi dengan lingkungan." },
  { "en": "Fungsi 'power supply' robot?", "id": "Menyediakan tenaga untuk kontroler/manipulator." },
  { "en": "Fungsi 'finger/jaws' gripper?", "id": "Memberikan kontak pada objek." },
  { "en": "Syarat objek gripper vakum?", "id": "Rata, bersih, dan halus." },
  { "en": "Kelemahan magnet permanen?", "id": "Sulit dikontrol." },
  { "en": "Bentuk 'arm robot'?", "id": "Seperti tangan manusia." },
  { "en": "Fungsi 'I/O rack-based'?", "id": "Terdiri dari beberapa modul I/O." },
  { "en": "Apa itu 'file number'?", "id": "Nomor identifikasi file dalam memori." },
  { "en": "Apa itu 'element number'?", "id": "Nomor elemen dalam file." },
  { "en": "Apa itu 'subelement number'?", "id": "Nomor sub-elemen." },
  { "en": "Bagaimana 'soft PLC' bekerja?", "id": "Mensimulasikan fungsi PLC di PC." },
  { "en": "Fungsi 'terminal assembly'?", "id": "Koneksi ke perangkat di lapangan." },
  { "en": "Apa itu 'printed circuit board'?", "id": "Papan sirkuit elektronik." },
  { "en": "Kelemahan output relay?", "id": "Respon lebih lambat dari solid-state." },
  { "en": "Apa itu 'level transmitter'?", "id": "Sensor untuk mengukur ketinggian cairan." },
  { "en": "Fungsi 'thermistor'?", "id": "Resistor peka terhadap suhu." },
  { "en": "Apa itu 'PID control'?", "id": "Kontrol proportional, integral, derivative." },
  { "en": "Fungsi 'stepper drive'?", "id": "Mengubah pulsa jadi gerakan motor." },
  { "en": "Gerakan 'linear slide'?", "id": "Maju dan mundur." },
  { "en": "Fungsi 'CANbus'?", "id": "Jaringan untuk perangkat diskrit." },
  { "en": "Fungsi 'Fieldbus' / 'Profibus'?", "id": "Jaringan untuk perangkat analog." },
  { "en": "Contoh 'main path'?", "id": "Terminal input (I/O point)." },
  { "en": "Fungsi 'return path'?", "id": "Jalur kembali ke kutub negatif." },
  { "en": "Apa itu 'loop' tertutup?", "id": "Rangkaian listrik yang lengkap." },
  { "en": "Kapan lampu indikator menyala?", "id": "Saat loop arusnya tertutup." },
  { "en": "Tujuan konfigurasi PLC modular?", "id": "Agar CPU mengenali modul terpasang." },
  { "en": "Alamat input 16-bit pertama?", "id": "0.00 hingga 0.15." },
  { "en": "Alamat input 16-bit kedua?", "id": "1.00 hingga 1.15." },
  { "en": "Abstrak modul 12?", "id": "ADC, perangkat pengubah sinyal analog." },
  { "en": "Bagian mendasar ADC?", "id": "Resolusi dan kecepatan sampling." },
  { "en": "Bagaimana kerapatan nilai ADC ditentukan?", "id": "Oleh resolusi (jumlah bit)." },
  { "en": "Satuan SPS?", "id": "Sample per second." },
  { "en": "Contoh modul ADC Omron?", "id": "CJ1W AD081." },
  { "en": "Apa itu 'unit number switch'?", "id": "Saklar putar untuk mengatur alamat." },
  { "en": "Apa itu 'Serial PLC Link'?", "id": "Jenis jaringan komunikasi PLC." },
  { "en": "Bagaimana 'hot swap' bekerja?", "id": "Mengganti modul tanpa mematikan sistem." },
  { "en": "Bagaimana 'CX-Programmer' terhubung ke PLC?", "id": "Online via kabel USB/serial." },
  { "en": "Bagaimana mengubah tampilan memori?", "id": "Dari Hexa ke Decimal." },
  { "en": "Fungsi 'built-in analog I/O'?", "id": "I/O analog terintegrasi di CPU." },
  { "en": "Contoh PLC dengan 'built-in I/O'?", "id": "Omron CP1H tipe CPU XA." },
  { "en": "Fungsi 'Analog Control' pada PLC?", "id": "Potensiometer untuk mengatur nilai analog." },
  { "en": "Fungsi 'DIP Switch' di PLC?", "id": "Mengatur konfigurasi dasar." },
  { "en": "Fungsi 'seven-segment LED display'?", "id": "Menampilkan kode status atau error." },
  { "en": "Abstrak modul 15?", "id": "Sistem Otomasi Industri." },
  { "en": "Kompetensi modul 15?", "id": "Mengetahui konsep otomasi industri." },
  { "en": "Urutan pertama cuci mobil?", "id": "Masukkan uang dan tekan Start." },
  { "en": "Urutan kedua cuci mobil?", "id": "Pertama-tama dicuci dengan air." },
  { "en": "Urutan ketiga cuci mobil?", "id": "Mengambil air dengan deterjen." },
  { "en": "Urutan keempat cuci mobil?", "id": "Dicuci dengan kain." },
  { "en": "Urutan kelima cuci mobil?", "id": "Dicuci kembali dengan air." },
  { "en": "Urutan terakhir cuci mobil?", "id": "Airnya dilap dan selesai." },
  { "en": "Contoh 'peralatan di pabrik'?", "id": "Kontrol conveyor, mesin perakitan." },
  { "en": "Contoh 'peralatan untuk bisnis'?", "id": "Mesin cuci, mesin penjual tiket." },
  { "en": "Contoh 'kontrol otomatisasi'?", "id": "Tempat parkir, lampu lalu lintas." },
  { "en": "Contoh alat di 'Panel Operasi'?", "id": "Pushbutton switch, selector switch." },
  { "en": "Contoh alat di 'Panel Kontrol'?", "id": "Kontaktor elektromagnetik, relay, PLC." },
  { "en": "Alat input mesin cuci mobil?", "id": "Tombol start/stop, saklar deteksi." },
  { "en": "Alat output mesin cuci mobil?", "id": "Pompa, motor sikat, lampu." },
  { "en": "Bagian dari 'pushbutton'?", "id": "Kontak bergerak, kontak diam, per." },
  { "en": "Bagian dari 'relay'?", "id": "Elektromagnet, kontak, per." },
  { "en": "Alamat input START konveyor?", "id": "0.00." },
  { "en": "Alamat input STOP konveyor?", "id": "0.01." },
  { "en": "Alamat output LAMPU#1 bel kuis?", "id": "100.01." },
  { "en": "Alamat internal relay bel kuis?", "id": "6.01, 6.02, 6.03." },
  { "en": "Alamat timer bel kuis?", "id": "TIMER 000 #60." },
  { "en": "Fungsi kontak T0000 di ladder?", "id": "Mereset instruksi KEEP." },
  { "en": "Fungsi kontak P_0_02s di ladder?", "id": "Memberikan pulsa kedip." },
  { "en": "Abstrak modul 10?", "id": "Penyambungan input dan output PLC." },
  { "en": "Tujuan Sub-CPMK 4.3?", "id": "Membuat wiring input dan output." },
  { "en": "Contoh 'rack-based' PLC?", "id": "Allen-Bradley PLC-5, SLC 500." },
  { "en": "Contoh 'tag-based' PLC?", "id": "Allen-Bradley ControlLogix." },
  { "en": "Bagian alamat AB SLC 500?", "id": "Tipe, file, elemen, bit." },
  { "en": "Apa itu 'word level addressing'?", "id": "Pengalamatan untuk modul analog." },
  { "en": "Apa itu 'bit level addressing'?", "id": "Pengalamatan untuk modul diskrit." },
  { "en": "Contoh 'base address'?", "id": "<Local:6:1.Data.0>." },
  { "en": "Tegangan modul I/O AC/DC?", "id": "12V, 24V, 48V, 120V, 230V." },
  { "en": "Tegangan modul I/O DC (TTL)?", "id": "5V DC." },
  { "en": "Contoh output yang dikontrol PLC?", "id": "Control valve, chart recorder, drive." },
  { "en": "Berapa channel modul analog?", "id": "Bisa 4, 8, atau 16." },
  { "en": "Contoh 'initialization parameter'?", "id": "Data untuk modul positioning." },
  { "en": "Contoh 'jaringan proprietary'?", "id": "Jaringan khusus dari produsen PLC." },
  { "en": "Alamat output SPRAY mobil cuci?", "id": "100.00." },
  { "en": "Alamat output CONVEYOR mobil cuci?", "id": "100.01." },
  { "en": "Alamat output BRUSH mobil cuci?", "id": "100.02." },
  { "en": "Abstrak modul 12?", "id": "Analog to Digital Converter (ADC)." },
  { "en": "Bagaimana resolusi ADC bekerja?", "id": "Makin tinggi bit, makin teliti." },
  { "en": "Efek kecepatan sampling tinggi?", "id": "Nilai terwakili makin rapat/teliti." },
  { "en": "Contoh sensor analog Omron?", "id": "CJ1W AD081." },
  { "en": "Alamat analog input CP1H?", "id": "CIO 200, 201, 202, 203." },
  { "en": "Resolusi analog CP1H?", "id": "Bisa 1/6000 atau 1/12000." },
  { "en": "Bagaimana mengaktifkan channel analog CP1H?", "id": "Ceklis 'Use' pada setting." },
  { "en": "Fungsi 'Option >> Transfer to PLC'?", "id": "Mengirim setting ke PLC." },
  { "en": "Alamat input SENSOR mobil cuci?", "id": "0.02." },
  { "en": "Fungsi kontak SENSOR di ladder?", "id": "Mengaktifkan motor sikat (brush)." },
  { "en": "Abstrak modul 11?", "id": "Sensor digital pada PLC." },
  { "en": "Analogi otak manusia?", "id": "Sebagai kendali utama (controller)." },
  { "en": "Sinyal dari mata ke otak?", "id": "Impulse signal listrik." },
  { "en": "Bagaimana otak menerjemahkan sinyal?", "id": "Menjadi jarak, warna, dan bentuk." },
  { "en": "Apa itu sensor 2 kabel?", "id": "Hanya punya kabel Brown dan Blue." },
  { "en": "Kelebihan sensor 2 kabel?", "id": "Harganya lebih murah." },
  { "en": "Fungsi 'load' pada sirkuit?", "id": "Membatasi arus yang mengalir." },
  { "en": "Arus yang benar pada sensor?", "id": "24V / (Load + r)." },
  { "en": "Arus yang salah pada sensor?", "id": "24V / r (menyebabkan kerusakan)." },
  { "en": "Alamat input OPEN safety crane?", "id": "0.00." },
  { "en": "Alamat input LOCK safety crane?", "id": "0.01 (NC)." },
  { "en": "Alamat input EMERGENCY safety crane?", "id": "0.02." },
  { "en": "Alamat input OVERLOAD safety crane?", "id": "0.03." },
  { "en": "Alamat output K MAJU crane?", "id": "100.00." },
  { "en": "Alamat output LAMPU HIJAU crane?", "id": "100.06." },
  { "en": "Alamat output LAMPU MERAH crane?", "id": "100.07." },
  { "en": "Fungsi 'KEEP' pada safety crane?", "id": "Mengaktifkan internal relay IR-OPEN." },
  { "en": "Apa yang mereset IR-OPEN crane?", "id": "Sensor OL dan IR-LOCK." },
  { "en": "Kapan lampu merah crane berkedip?", "id": "Saat sensor OL aktif." },
  { "en": "Kapan lampu merah crane menyala?", "id": "Saat IR-LOCK sedang aktif." },
  { "en": "Syarat motor crane aktif?", "id": "Internal relay IR-OPEN harus aktif." },
  { "en": "Fungsi 'interlock' motor crane?", "id": "Hanya satu gerakan bisa beroperasi." },
  { "en": "Abstrak modul 9?", "id": "Tipe memori pada PLC." },
  { "en": "Apa itu 'CMOS-RAM'?", "id": "RAM dengan konsumsi arus rendah." },
  { "en": "Bagaimana Flash EEPROM bekerja?", "id": "Otomatis menjadi cadangan dari RAM." },
  { "en": "Contoh PLC Omron Compact?", "id": "Tipe CP1E." },
  { "en": "Contoh PLC Omron Modular?", "id": "Tipe CJ2M." },
  { "en": "Istilah lain PLC Compact?", "id": "Jenis 'based'." },
  { "en": "Istilah lain PLC Modular?", "id": "Sistem 'rack'." },
  { "en": "Alamat input BEL #1 kuis?", "id": "0.01." },
  { "en": "Alamat output LAMPU#1 kuis?", "id": "100.01." },
  { "en": "Abstrak modul 14?", "id": "Komputer dan Robot dalam Otomasi." },
  { "en": "Contoh aplikasi 'engineering' otomasi?", "id": "Pengolahan otomatis data elektronis." },
  { "en": "Contoh 'intelligence component'?", "id": "Komponen dengan mikroprosesor." },
  { "en": "Kapan komputer 'mid-range' mahal?", "id": "Dibanding kendali berbasis mikroprosesor." },
  { "en": "Contoh 'mid-range computer'?", "id": "DEC PDP-11." },
  { "en": "Apa itu 'DSP'?", "id": "Digital Signal Processor." },
  { "en": "Apa itu 'Local Area Network'?", "id": "Jaringan komunikasi data lokal." },
  { "en": "Tujuan utama pemilihan peralatan?", "id": "Menentukan tinggi rendahnya efisiensi." },
  { "en": "Apa itu 'articulated arm robot'?", "id": "Robot dengan beberapa sendi putar." },
  { "en": "Apa itu 'cylindrical coordinate robot'?", "id": "Robot dengan area kerja silinder." },
  { "en": "Abstrak modul 13?", "id": "Instalasi, Perawatan, Troubleshooting PLC." },
  { "en": "Gejala jika input device korslet?", "id": "Perangkat tidak akan mati." },
  { "en": "Gejala jika 'off-state leakage' tinggi?", "id": "Input tidak mau mati." },
  { "en": "Gejala jika 'wiring' input terbuka?", "id": "Perangkat tidak bisa menyala." },
  { "en": "Penyebab output tidak mau mati?", "id": "Output di-force on, sirkuit rusak." },
  { "en": "Penyebab output tidak mau menyala?", "id": "Tegangan rendah, wiring, modul rusak." },
  { "en": "Alamat input SENSOR-1 pintu garasi?", "id": "0.00." },
  { "en": "Alamat input SENSOR-2 pintu garasi?", "id": "0.01." },
  { "en": "Alamat input LIMIT SWITCH-1?", "id": "0.03." },
  { "en": "Alamat output PINTU.BUKA?", "id": "100.00." },
  { "en": "Alamat output PINTU.TUTUP?", "id": "100.01." },
  { "en": "Fungsi IR-600 di pintu garasi?", "id": "Mengaktifkan proses membuka pintu." },
  { "en": "Apa yang mereset IR-600?", "id": "Sensor-2 LOW atau Limit Switch-2 ON." },
  { "en": "Abstrak modul 10?", "id": "Kunci belajar PLC dan otomasi." },
  { "en": "Tujuan Sub-CPMK 4.3?", "id": "Memahami dan membuat wiring I/O." },
  { "en": "Apa itu 'local I/O'?", "id": "I/O yang terpasang bersama prosesor." },
  { "en": "Apa itu 'remote I/O'?", "id": "I/O di lokasi jauh." },
  { "en": "Komponen 'rack-based I/O'?", "id": "Power supply, prosesor, modul I/O." },
  { "en": "Apa itu 'slot number'?", "id": "Lokasi fisik modul I/O." },
  { "en": "Tipe file 'O'?", "id": "File type untuk output." },
  { "en": "Tipe file 'I'?", "id": "File type untuk input." },
  { "en": "Contoh 'real-world address'?", "id": "Alamat fisik terminal, slot." },
  { "en": "Contoh 'memory address'?", "id": "Alamat data di memori PLC." },
  { "en": "Fungsi 'input instruction' di ladder?", "id": "Membaca status input." },
  { "en": "Fungsi 'output instruction' di ladder?", "id": "Mengontrol status output." },
  { "en": "Contoh perangkat input analog?", "id": "Thermocouple." },
  { "en": "Contoh perangkat output analog?", "id": "Meter analog." },
  { "en": "Fungsi 'Triac' pada output?", "id": "Saklar elektronik untuk beban AC." },
  { "en": "Fungsi 'transistor' pada output?", "id": "Saklar elektronik untuk beban DC." },
  { "en": "Fungsi 'sourcing sensor'?", "id": "Memberikan arus ke modul sinking." },
  { "en": "Fungsi 'sinking sensor'?", "id": "Menerima arus dari modul sourcing." },
  { "en": "Apa itu 'level indicator'?", "id": "Menampilkan ketinggian cairan." },
  { "en": "Apa itu 'control valve'?", "id": "Katup yang dikontrol secara otomatis." },
  { "en": "Fungsi 'encoder'?", "id": "Menghasilkan pulsa untuk mengukur posisi." },
  { "en": "Apa itu 'actuator'?", "id": "Perangkat yang menghasilkan gerakan." },
  { "en": "Contoh 'actuator'?", "id": "Valve (katup)." },
  { "en": "Apa itu 'process variable' (PV)?", "id": "Nilai terukur dari proses." },
  { "en": "Apa itu 'set point' (SP)?", "id": "Nilai target yang diinginkan." },
  { "en": "Bagaimana 'error' (E) dihitung?", "id": "E = SP - PV." },
  { "en": "Fungsi 'servo motor'?", "id": "Motor untuk kontrol posisi presisi." },
  { "en": "Fungsi 'stepper motor'?", "id": "Motor yang bergerak per langkah." },
  { "en": "Apa itu 'I/O point'?", "id": "Titik koneksi untuk input/output." },
  { "en": "Nama lain 'ban berjalan'?", "id": "Conveyor belt." },
  { "en": "Contoh 'pulse I/O module'?", "id": "Modul untuk sinyal pulsa cepat." },
  { "en": "Berapa unit konfigurasi maksimal?", "id": "10 unit (max.)." },
  { "en": "Abstrak modul 13?", "id": "Instalasi, Perawatan, dan Troubleshooting PLC." },
  { "en": "Apa itu 'kontaminasi' pada PLC?", "id": "Debu, kelembaban, karat." },
  { "en": "Fungsi 'fan' atau 'blower'?", "id": "Membantu membuang panas." },
  { "en": "Fungsi 'thermostat' di panel?", "id": "Mengendalikan pemanas untuk kondensasi." },
  { "en": "Bagaimana penempatan CPU?", "id": "Pada ketinggian kerja yang baik." },
  { "en": "Bagaimana penempatan 'rack I/O'?", "id": "Di bawah atau di samping CPU." },
  { "en": "Fungsi 'kabel tie'?", "id": "Mengikat dan merapikan gabungan kabel." },
  { "en": "Fungsi 'bleeding resistor'?", "id": "Mengatasi kebocoran arus input." },
  { "en": "Fungsi 'dioda' pada beban induktif?", "id": "Menekan lonjakan tegangan DC." },
  { "en": "Fungsi 'RC snubber circuit'?", "id": "Menekan lonjakan tegangan AC." },
  { "en": "Fungsi 'MOV' (Metal Oxide Varistor)?", "id": "Pelindung lonjakan tegangan." },
  { "en": "Arti indikator 'AC OK'?", "id": "Daya AC dalam kondisi baik." },
  { "en": "Arti indikator 'DC OK'?", "id": "Daya DC dalam kondisi baik." },
  { "en": "Arti indikator 'Processor OK'?", "id": "Prosesor bekerja dengan baik." },
  { "en": "Penyebab 'ground loop'?", "id": "Shield terhubung ground di dua sisi." },
  { "en": "Abstrak modul 15?", "id": "Simulasi dengan CX Designer." },
  { "en": "Tujuan 'sequence control'?", "id": "Beroperasi sesuai urutan yang ditentukan." },
  { "en": "Fungsi 'relay'?", "id": "Saklar listrik yang dioperasikan elektromagnet." },
  { "en": "Bagaimana 'pushbutton' bekerja?", "id": "Menutup/membuka kontak saat ditekan." },
  { "en": "Abstrak modul 9?", "id": "Memori adalah elemen penyimpan informasi." },
  { "en": "Apa itu 'word'?", "id": "Sekelompok bit, biasanya 16." },
  { "en": "Apa itu 'volatile memory'?", "id": "Data hilang jika daya mati." },
  { "en": "Apa itu 'non-volatile memory'?", "id": "Data tetap ada tanpa daya." },
  { "en": "Fungsi 'ROM'?", "id": "Menyimpan sistem operasi PLC." },
  { "en": "Fungsi 'RAM'?", "id": "Penyimpanan data sementara." },
  { "en": "Fungsi 'EEPROM'?", "id": "Penyimpanan program permanen yang bisa diubah." },
  { "en": "Fungsi 'Flash EEPROM'?", "id": "Penyimpanan cepat dan backup RAM." },
  { "en": "Ciri 'PLC Compact'?", "id": "Semua komponen jadi satu unit." },
  { "en": "Ciri 'PLC Modular'?", "id": "Komponen dapat dipisah-pisah." },
  { "en": "Fungsi 'tabel input'?", "id": "Menyimpan status masukan dari modul." },
  { "en": "Fungsi 'tabel output'?", "id": "Menyimpan status sinyal untuk modul." },
  { "en": "Fungsi 'bit-bit internal'?", "id": "Menyimpan data koil relay internal." },
  { "en": "Fungsi 'register/word'?", "id": "Menyimpan data dalam ukuran byte/word." },
  { "en": "Abstrak modul 11?", "id": "Sensor adalah perangkat penangkap fenomena." },
  { "en": "Bagaimana sensor bekerja?", "id": "Menangkap fenomena, mengirim sinyal listrik." },
  { "en": "Keluaran 'sensor digital'?", "id": "Hanya kondisi 1 atau 0." },
  { "en": "Keluaran 'sensor analog'?", "id": "Rentang nilai tegangan atau arus." },
  { "en": "Tipe 'sensor PNP'?", "id": "Tipe sourcing." },
  { "en": "Tipe 'sensor NPN'?", "id": "Tipe sinking." },
  { "en": "Fungsi 'reed switch'?", "id": "Saklar aktif karena medan magnet." },
  { "en": "Abstrak modul 14?", "id": "Penggunaan komputer dan robot." },
  { "en": "Jenis 'fixed automation'?", "id": "Menggunakan peralatan mekanik." },
  { "en": "Jenis 'flexible automation'?", "id": "Menggunakan sistem berbasis komputer." },
  { "en": "Apa itu 'hierarchical control'?", "id": "Satu host mengontrol banyak slave." },
  { "en": "Apa itu 'distributed control'?", "id": "Setiap komponen dikontrol komputer lokal." },
  { "en": "Apa itu 'heterarchical control'?", "id": "Sistem tanpa host komputer." },
  { "en": "Definisi 'robot'?", "id": "Alat elektro-mekanik melakukan tugas fisik." },
  { "en": "Fungsi 'manipulator'?", "id": "Lengan yang memberikan gerakan robot." },
  { "en": "Fungsi 'kontroler' robot?", "id": "Jantung atau otak dari robot." },
  { "en": "Fungsi 'efektor'?", "id": "Alat di ujung lengan robot." },
  { "en": "Jenis 'gripper'?", "id": "Mekanik, vakum, magnetik." },
  { "en": "Fungsi 'robot industri'?", "id": "Pekerjaan yang berulang dan berbahaya." },
  { "en": "Fungsi 'robot bergerak'?", "id": "Eksplorasi, pencarian, layanan." },
  { "en": "Abstrak modul 12?", "id": "ADC mengubah sinyal analog ke digital." },
  { "en": "Fungsi 'resolusi' ADC?", "id": "Menentukan ketelitian nilai konversi." },
  { "en": "Fungsi 'kecepatan sampling'?", "id": "Seberapa sering sinyal dikonversi." },
  { "en": "Satuan kecepatan sampling?", "id": "SPS (sample per second) atau Hz." },
  { "en": "Prinsip kerja ADC?", "id": "Perbandingan sinyal input dan referensi." },
  { "en": "Alamat 'Special I/O' Omron?", "id": "Mulai dari CIO 2000." },
  { "en": "Fungsi 'unit number' modul?", "id": "Memberi alamat pembacaan input." },
  { "en": "Bagaimana mengkonfigurasi modul analog?", "id": "Melalui IO Table Unit Setup." },
  { "en": "Fungsi 'built-in analog I/O'?", "id": "I/O analog yang menyatu CPU." },
  { "en": "Resolusi built-in analog CP1H?", "id": "Bisa 1/6000 atau 1/12000." },
  { "en": "Bagaimana mengubah resolusi analog?", "id": "Melalui 'Setting' di program." },
  { "en": "Fungsi 'DIFU' (Differentiate Up)?", "id": "Aktif sesaat saat sinyal ON." },
  { "en": "Fungsi `pemeriksaan visual`?", "id": "Memastikan semua komponen terpasang." },
  { "en": "Tujuan `lockout and tagout`?", "id": "Keamanan personel saat perbaikan." },
  { "en": "Penyebab `ground loop`?", "id": "Dua atau lebih jalur ground." },
  { "en": "Bagaimana `watchdog timer` bekerja?", "id": "Memonitor durasi proses scan." },
  { "en": "Tanda `sekring putus`?", "id": "Indikator status sekring menyala." },
  { "en": "Fungsi `transducer`?", "id": "Mengubah satu bentuk energi lain." },
  { "en": "Fungsi `preamp` pada sensor?", "id": "Penguat sinyal awal dari sensor." },
  { "en": "Bagaimana `reed switch` bekerja?", "id": "Aktif karena adanya medan magnet." },
  { "en": "Fungsi `kontak diam`?", "id": "Terminal tetap pada saklar." },
  { "en": "Fungsi `kontak bergerak`?", "id": "Terminal bergerak pada saklar." },
  { "en": "Fungsi `coil` pada relay?", "id": "Membangkitkan medan elektromagnetik." },
  { "en": "Apa itu `input image table`?", "id": "Memori status semua input." },
  { "en": "Apa itu `output image table`?", "id": "Memori status semua output." },
  { "en": "Tujuan `internal relay`?", "id": "Sebagai bit perantara dalam logika." },
  { "en": "Kapan `kendali digital` dimulai?", "id": "Sekitar tahun 1970-an." },
  { "en": "Contoh `unintelligent slave devices`?", "id": "Sensor, aktuator tanpa prosesor." },
  { "en": "Arti kata `robota`?", "id": "Pekerja atau bekerja paksa." },
  { "en": "Apa itu `synchromotion`?", "id": "Beberapa robot bekerja secara serempak." },
  { "en": "Fungsi `wrist` pada robot?", "id": "Bagian ujung lengan robot." },
  { "en": "Bahan `jaws` pada gripper?", "id": "Bisa diganti sesuai kebutuhan." },
  { "en": "Bagaimana `pompa vakum` bekerja?", "id": "Piston mekanis menciptakan kehampaan." },
  { "en": "Bagaimana `sistem venturi` bekerja?", "id": "Aliran udara menciptakan kevampukan." },
  { "en": "Fungsi `gantry robot`?", "id": "Bekerja di area kerja persegi." },
  { "en": "Fungsi `SCARA robot`?", "id": "Perakitan presisi kecepatan tinggi." },
  { "en": "Kapan `triac` digunakan?", "id": "Untuk mengontrol beban tegangan AC." },
  { "en": "Fungsi `leadscrew` pada aktuator?", "id": "Mengubah gerak putar jadi linear." },
  { "en": "Fungsi `terminator` jaringan?", "id": "Mencegah pantulan sinyal di ujung." },
  { "en": "Kabel untuk sensor `thermocouple`?", "id": "Kabel twisted pair berpelindung." },
  { "en": "Fungsi `set value` pada timer?", "id": "Mengatur durasi waktu tunda." },
  { "en": "Bagaimana instruksi `KEEP` bekerja?", "id": "Menahan output tetap aktif." },
  { "en": "Fungsi `pulse` pada output?", "id": "Menghasilkan sinyal ON-OFF berirama." },
  { "en": "Bagaimana `ADC` menghitung nilai?", "id": "(Sinyal/Referensi) x Resolusi." },
  { "en": "Apa itu `DeviceNet`?", "id": "Protokol jaringan komunikasi industri." },
  { "en": "Fungsi `Memory Cassette Slot`?", "id": "Menyimpan atau memuat program." },
  { "en": "Fungsi `Expansion Connector`?", "id": "Menghubungkan unit I/O tambahan." },
  { "en": "Fungsi `peripheral port`?", "id": "Koneksi ke perangkat pemrograman." },
  { "en": "Bagaimana `sinking input` bekerja?", "id": "Menerima arus dari perangkat sourcing." },
  { "en": "Bagaimana `sourcing input` bekerja?", "id": "Memberi arus ke perangkat sinking." },
  { "en": "Posisi `I/O Control Unit`?", "id": "Segera di kanan CPU." },
  { "en": "Fungsi `end cover`?", "id": "Menutup slot terakhir pada rack." },
  { "en": "Posisi `Pulse I/O Module`?", "id": "Di sebelah kiri unit CPU." },
  { "en": "Tindakan `saat koneksi kendur`?", "id": "Kencangkan kembali semua terminal." },
  { "en": "Fungsi `DIFD` di ladder?", "id": "Aktif sesaat saat sinyal OFF." },
  { "en": "Fungsi `limit switch`?", "id": "Mendeteksi batas pergerakan mekanis." },
  { "en": "Fungsi `HMI`?", "id": "Antarmuka grafis antara operator-mesin." },
  { "en": "Apa itu `brush motor`?", "id": "Motor untuk memutar sikat pembersih." },
  { "en": "Fungsi `penekan lonjakan`?", "id": "Melindungi dari lonjakan tegangan." },
  { "en": "Fungsi `bleeding resistor`?", "id": "Mengatasi kebocoran arus di input." },
  { "en": "Fungsi `RC snubber`?", "id": "Menekan lonjakan tegangan beban AC." },
  { "en": "Bagaimana `MOV` melindungi sirkuit?", "id": "Memotong tegangan puncak yang tinggi." },
  { "en": "Tujuan `pemeriksaan statis`?", "id": "Memverifikasi pengkabelan tanpa menjalankan program." },
  { "en": "Bagaimana `mode TEST` PLC bekerja?", "id": "Menjalankan logika tanpa mengaktifkan output." },
  { "en": "Tanda `baterai lemah` di PLC?", "id": "Indikator BATT (merah) menyala." },
  { "en": "Alat ukur `ground loop`?", "id": "Menggunakan Ohm meter." },
  { "en": "Output `sensor NPN` saat deteksi?", "id": "Menjadi 0 Volt (LOW)." },
  { "en": "Output `sensor PNP` saat deteksi?", "id": "Menjadi 24 Volt (HIGH)." },
  { "en": "Apa itu `loop semu`?", "id": "Sinyal palsu karena kabel terkelupas." },
  { "en": "Berapa `resolusi ADC 8-bit`?", "id": "256 tingkat diskrit." },
  { "en": "Berapa `resolusi ADC 12-bit`?", "id": "4096 tingkat diskrit." },
  { "en": "Bagaimana `unit number` diatur?", "id": "Dengan memutar saklar putar." },
  { "en": "Fungsi `hot swap`?", "id": "Mengganti modul saat sistem berjalan." },
  { "en": "Cara `mengubah tampilan memori`?", "id": "Pilih format Hexa atau Decimal." },
  { "en": "Fungsi `built-in I/O`?", "id": "I/O yang terintegrasi di CPU." },
  { "en": "Fungsi `DIP Switch` pada PLC?", "id": "Mengatur mode operasi dasar." },
  { "en": "Fungsi `seven-segment display`?", "id": "Menampilkan kode status atau error." },
  { "en": "Apa itu `baud rate`?", "id": "Kecepatan transfer data serial." },
  { "en": "Apa itu `proprietary network`?", "id": "Jaringan khusus dari satu produsen." },
  { "en": "Fungsi `timer BCD`?", "id": "Timer menggunakan format BCD." },
  { "en": "Cara `mereset KEEP`?", "id": "Mengaktifkan input reset-nya." },
  { "en": "Karakteristik `memori volatile`?", "id": "Membutuhkan daya untuk menyimpan data." },
  { "en": "Karakteristik `memori non-volatile`?", "id": "Menyimpan data tanpa daya." },
  { "en": "Bagaimana `EPROM` dihapus?", "id": "Dengan sinar ultraviolet." },
  { "en": "Bagaimana `EEPROM` dihapus?", "id": "Dengan sinyal listrik." },
  { "en": "Contoh `bit khusus` Omron?", "id": "P_On (Always On)." },
  { "en": "Tugas `host` di distributed control?", "id": "Koordinasi antar prosesor lokal." },
  { "en": "Apa itu `derajat kebebasan` robot?", "id": "Jumlah sumbu gerakan independen." },
  { "en": "Bahan `mangkok vakum`?", "id": "Terbuat dari karet elastis." },
  { "en": "Kapan `magnet permanen` ideal?", "id": "Di lingkungan yang mudah meledak." },
  { "en": "Fungsi `saringan udara` panel?", "id": "Mencegah debu masuk ke panel." },
  { "en": "Bagaimana `kabel sinyal` dirutekan?", "id": "Terpisah dari kabel daya AC." },
  { "en": "Tujuan `kode warna` kabel?", "id": "Memudahkan identifikasi sinyal." },
  { "en": "Fungsi `ferrules` pada kabel?", "id": "Melindungi ujung serabut kabel." },
  { "en": "Tindakan saat `tegangan sinyal rendah`?", "id": "Periksa sumber tegangan atau kabel." },
  { "en": "Tindakan saat `output tidak bekerja`?", "id": "Periksa sekring, kabel, dan perangkat." },
  { "en": "Bagaimana `logika interlock` bekerja?", "id": "Satu output mematikan output lain." },
  { "en": "Fungsi `conveyor` di pabrik?", "id": "Memindahkan produk antar stasiun kerja." },
  { "en": "Tegangan `TTL level`?", "id": "Sekitar 5 Volt DC." },
  { "en": "Apa itu `rack` PLC?", "id": "Kerangka fisik untuk menempatkan modul." },
  { "en": "Apa itu `slot` PLC?", "id": "Posisi spesifik untuk modul." },
  { "en": "Fungsi `processor module`?", "id": "Unit utama pemrosesan (CPU)." },
  { "en": "Contoh `aplikasi robot`?", "id": "Pengelasan, pengecatan, perakitan." },
  { "en": "Apa itu `power supply unit`?", "id": "Modul penyedia daya untuk rack." },
  { "en": "Fungsi `communication module`?", "id": "Menghubungkan PLC ke jaringan." },
  { "en": "Apa itu `digital input`?", "id": "Input dengan sinyal ON/OFF." },
  { "en": "Apa itu `analog input`?", "id": "Input dengan sinyal rentang nilai." },
  { "en": "Fungsi `backplane` pada rack?", "id": "Menghubungkan modul secara elektrik." },
  { "en": "Apa itu `sinking output`?", "id": "Menarik arus ke common." },
  { "en": "Apa itu `sourcing output`?", "id": "Memberikan arus dari sumber." },
  { "en": "Fungsi `Push Button`?", "id": "Memberi sinyal input sesaat." },
  { "en": "Fungsi `Limit Switch`?", "id": "Mendeteksi batas posisi fisik." },
  { "en": "Fungsi `Solenoid Valves`?", "id": "Mengontrol aliran fluida." },
  { "en": "Fungsi `Relay` pada output?", "id": "Mengontrol beban arus besar." },
  { "en": "Apa itu `PLC Compact`?", "id": "PLC dengan I/O terintegrasi." },
  { "en": "Apa itu `PLC Modular`?", "id": "PLC dengan modul-modul terpisah." },
  { "en": "Fungsi `IO Table Unit Setup`?", "id": "Mengkonfigurasi modul I/O." },
  { "en": "Tampilan default memori PLC?", "id": "Hexadecimal." },
  { "en": "Bagaimana `ADC` bekerja?", "id": "Mengubah sinyal analog jadi digital." },
  { "en": "Apa itu `resolusi` ADC?", "id": "Ketelitian konversi sinyal." },
  { "en": "Apa itu `kecepatan sampling`?", "id": "Frekuensi konversi sinyal." },
  { "en": "Alamat `Special I/O`?", "id": "Mulai dari CIO 2000." },
  { "en": "Fungsi `unit number`?", "id": "Identitas modul pada rack." },
  { "en": "Apa itu `built-in analog`?", "id": "I/O analog terintegrasi di CPU." },
  { "en": "Fungsi `DIFD`?", "id": "Deteksi sinyal tepi turun." },
  { "en": "Fungsi `DIFU`?", "id": "Deteksi sinyal tepi naik." },
  { "en": "Apa itu `HMI`?", "id": "Antarmuka antara manusia dan mesin." },
  { "en": "Fungsi `CX Designer`?", "id": "Membuat aplikasi HMI." },
  { "en": "Fungsi `CX Programmer`?", "id": "Membuat program ladder PLC." },
  { "en": "Apa itu `tata letak` sistem?", "id": "Penempatan komponen dalam panel." },
  { "en": "Fungsi `panel PLC`?", "id": "Melindungi komponen dari lingkungan." },
  { "en": "Bagaimana `grounding` yang benar?", "id": "Hubungkan semua sasis ke ground." },
  { "en": "Fungsi `sirkuit emergency`?", "id": "Mematikan sistem dalam keadaan darurat." },
  { "en": "Apa itu `bleeding resistor`?", "id": "Mengatasi kebocoran arus input." },
  { "en": "Fungsi `sirkuit snubber`?", "id": "Menekan lonjakan tegangan beban induktif." },
  { "en": "Fungsi `lockout/tagout`?", "id": "Keamanan personel saat perawatan." },
  { "en": "Apa itu `ground loop`?", "id": "Dua atau lebih jalur ground." },
  { "en": "Bagaimana `watchdog timer` bekerja?", "id": "Memonitor siklus scan CPU." },
  { "en": "Fungsi `pohon troubleshooting`?", "id": "Panduan sistematis mencari kesalahan." },
  { "en": "Apa itu `fixed automation`?", "id": "Otomasi untuk satu tugas spesifik." },
  { "en": "Apa itu `flexible automation`?", "id": "Otomasi yang bisa diprogram ulang." },
  { "en": "Apa itu `hierarchical control`?", "id": "Satu kontroler utama (host)." },
  { "en": "Apa itu `distributed control`?", "id": "Beberapa kontroler lokal terdistribusi." },
  { "en": "Apa itu `heterarchical control`?", "id": "Kontroler bekerja setara tanpa host." },
  { "en": "Definisi `robot`?", "id": "Alat elektro-mekanik melakukan tugas." },
  { "en": "Fungsi `manipulator`?", "id": "Lengan mekanik robot." },
  { "en": "Fungsi `kontroler` robot?", "id": "Otak yang mengendalikan robot." },
  { "en": "Fungsi `efektor` robot?", "id": "Alat di ujung lengan robot." },
  { "en": "Jenis `gripper`?", "id": "Mekanik, vakum, dan magnetik." },
  { "en": "Fungsi `robot industri`?", "id": "Pekerjaan berulang di pabrik." },
  { "en": "Fungsi `memori` PLC?", "id": "Menyimpan program dan data." },
  { "en": "Apa itu `memori volatile`?", "id": "Data hilang saat daya mati." },
  { "en": "Apa itu `memori non-volatile`?", "id": "Data tersimpan tanpa daya." },
  { "en": "Fungsi `RAM`?", "id": "Memori kerja sementara." },
  { "en": "Fungsi `ROM`?", "id": "Menyimpan firmware sistem." },
  { "en": "Fungsi `EEPROM`?", "id": "Memori program yang bisa dihapus." },
  { "en": "Apa itu `tabel input`?", "id": "Area memori status input." },
  { "en": "Apa itu `tabel output`?", "id": "Area memori status output." },
  { "en": "Fungsi `internal relay`?", "id": "Sebagai flag atau memori internal." },
  { "en": "Apa itu `sequence control`?", "id": "Kontrol berdasarkan urutan langkah." },
  { "en": "Fungsi `relay`?", "id": "Saklar yang dikontrol secara elektrik." },
  { "en": "Apa itu `kontak N.O.`?", "id": "Normally Open, normalnya terbuka." },
  { "en": "Apa itu `kontak N.C.`?", "id": "Normally Close, normalnya tertutup." },
  { "en": "Tujuan `interlock`?", "id": "Mencegah dua aksi konflik." },
  { "en": "Apa itu `sensor digital`?", "id": "Sensor dengan output ON/OFF." },
  { "en": "Apa itu `sensor analog`?", "id": "Sensor dengan output rentang nilai." },
  { "en": "Tipe sensor `PNP`?", "id": "Tipe sourcing." },
  { "en": "Tipe sensor `NPN`?", "id": "Tipe sinking." },
  { "en": "Fungsi `reed switch`?", "id": "Saklar yang diaktifkan medan magnet." },
  { "en": "Berapa bit `Basic I/O Area`?", "id": "2.560 bit (160 word)." },
  { "en": "Berapa bit `Link Area`?", "id": "3.200 bit (200 word)." },
  { "en": "Berapa bit `CPU Bus Unit Area`?", "id": "6.400 bit (200 word)." },
  { "en": "Berapa bit `Special I/O Area`?", "id": "15.360 bit (960 word)." },
  { "en": "Alamat `CPU Bus Unit Area`?", "id": "CIO 1500 hingga 1899." },
  { "en": "Fungsi kontak `CR1-1`?", "id": "Sebagai kontak pengunci (latching)." },
  { "en": "Apa yang mengaktifkan `CR1`?", "id": "Tombol Start PLC System." },
  { "en": "Apa yang mematikan `CR1`?", "id": "Tombol Stop atau Emergency." },
  { "en": "Gejala input `di-force on`?", "id": "Program anggap ON walau device OFF." },
  { "en": "Penyebab `output tidak mau menyala`?", "id": "Kabel putus, sekring rusak." },
  { "en": "Alamat SENSOR-3 pintu garasi?", "id": "0.02." },
  { "en": "Alamat `LIMIT SWITCH-2`?", "id": "0.04." },
  { "en": "Fungsi `IR.BUKA` pintu garasi?", "id": "Internal relay untuk membuka pintu." },
  { "en": "Fungsi `IR.TUTUP` pintu garasi?", "id": "Internal relay untuk menutup pintu." },
  { "en": "Apa itu `Cornea`?", "id": "Bagian terluar mata." },
  { "en": "Apa itu `Pupil`?", "id": "Bukaan di tengah iris." },
  { "en": "Apa itu `Retina`?", "id": "Lapisan peka cahaya di mata." },
  { "en": "Fungsi `Optical nerve`?", "id": "Mengirim sinyal visual ke otak." },
  { "en": "Kontak apa mereset `T#1`?", "id": "NC dari Timer T#8." },
  { "en": "Apa yang mengaktifkan `L#2`?", "id": "NO dari Timer T#1." },
  { "en": "Alamat `START` lampu berjalan?", "id": "1:0.00." },
  { "en": "Alamat `STOP` lampu berjalan?", "id": "0.01." },
  { "en": "Apa itu `apendage` robot?", "id": "Bagian tambahan atau lengan." },
  { "en": "Apa itu `base` robot?", "id": "Bagian dasar atau fondasi." },
  { "en": "Tegangan input modul `CJ1W-IA111`?", "id": "AC Input Unit." },
  { "en": "Deskripsi modul `CJ1W-ID211`?", "id": "DC Input Unit." },
  { "en": "Deskripsi modul `CJ1W-OC211`?", "id": "Relay Output Unit." },
  { "en": "Berapa output `CJ1W-OC211`?", "id": "16 outputs." },
  { "en": "Apa arti `OFF` pada indikator BATT?", "id": "Baterai dalam kondisi normal." },
  { "en": "Gejala jika `input voltage` rendah?", "id": "Input tidak mau menyala." },
  { "en": "Tindakan saat `output korslet`?", "id": "Ganti sekring dan perbaiki wiring." },
  { "en": "Apa itu `PLC Memory` window?", "id": "Menampilkan isi memori CIO." },
  { "en": "Fungsi `Start Address` di monitor?", "id": "Menentukan alamat awal yang ditampilkan." },
  { "en": "Bagaimana `Servo-controller` bekerja?", "id": "Mengontrol posisi motor servo." },
  { "en": "Fungsi `Variable speed controller`?", "id": "Mengatur kecepatan putaran motor." },
  { "en": "Fungsi `Chart recorder`?", "id": "Mencatat data dari waktu ke waktu." },
  { "en": "Apa itu `field wiring`?", "id": "Pengkabelan dari PLC ke perangkat." },
  { "en": "Apa itu `internal module circuit`?", "id": "Sirkuit internal di dalam modul." },
  { "en": "Fungsi `backplane`?", "id": "Menghubungkan semua modul di rack." },
  { "en": "Apa itu `rack` PLC?", "id": "Kerangka fisik untuk menampung modul." },
  { "en": "Apa itu `slot` PLC?", "id": "Posisi spesifik untuk sebuah modul." },
  { "en": "Fungsi `CPU Unit`?", "id": "Unit prosesor utama PLC." },
  { "en": "Fungsi `Analog Input Unit`?", "id": "Modul untuk menerima sinyal analog." },
  { "en": "Fungsi `Analog Output Unit`?", "id": "Modul untuk mengirim sinyal analog." },
  { "en": "Contoh sinyal dari `Sensor Suhu`?", "id": "Sinyal analog (tegangan/arus)." },
  { "en": "Fungsi `Regulator` pada output?", "id": "Mengatur output (misal: suhu)." },
  { "en": "Fungsi `Servo-controller`?", "id": "Mengontrol posisi motor servo presisi." },
  { "en": "Fungsi `Variable speed controller`?", "id": "Mengatur kecepatan putaran motor." },
  { "en": "Fungsi `Chart recorder`?", "id": "Merekam data proses secara grafis." },
  { "en": "Apa itu `Basic I/O Area`?", "id": "Area memori untuk I/O digital." },
  { "en": "Apa itu `Link Area`?", "id": "Area memori untuk komunikasi data." },
  { "en": "Apa itu `Special I/O Area`?", "id": "Area memori untuk I/O analog." },
  { "en": "Fungsi `MODE switch` di modul?", "id": "Mengatur mode operasi normal." },
  { "en": "Fungsi `Voltage/current switch`?", "id": "Memilih tipe sinyal input." },
  { "en": "Fungsi `Unit number switches`?", "id": "Mengatur alamat unit modul." },
  { "en": "Berapa `resolusi` modul AD081?", "id": "4,000 atau 8,000." },
  { "en": "Bagaimana `unit number` memberi alamat?", "id": "Alamat = 2000 + (n x 10)." },
  { "en": "Langkah pertama `konfigurasi IO`?", "id": "Masuk ke I/O Table." },
  { "en": "Bagaimana `membuat tabel IO` otomatis?", "id": "Pilih Option lalu Create." },
  { "en": "Tindakan setelah `setting parameter`?", "id": "Transfer to PLC." },
  { "en": "Fungsi `built-in AD/DA`?", "id": "Konverter A/D dan D/A terintegrasi." },
  { "en": "Apa itu `factory settings`?", "id": "Pengaturan default dari pabrik." },
  { "en": "Bagaimana `mengubah range` input analog?", "id": "Pilih range di setting." },
  { "en": "Fungsi `terminal block`?", "id": "Titik koneksi untuk kabel." },
  { "en": "Kapan `kabel shield` digunakan?", "id": "Untuk melindungi sinyal dari noise." },
  { "en": "Penyebab utama `kegagalan belajar` PLC?", "id": "Tidak bisa menerapkan kondisi riil." },
  { "en": "Apa itu `rack-based I/O`?", "id": "Modul I/O dipasang pada rack." },
  { "en": "Apa itu `tag-based addressing`?", "id": "Pengalamatan menggunakan nama (tag)." },
  { "en": "Fungsi `optical isolator`?", "id": "Memberikan isolasi elektrik antar sirkuit." },
  { "en": "Kapan `interposing relay` dibutuhkan?", "id": "Saat mengontrol beban arus besar." },
  { "en": "Apa itu `level transmitter`?", "id": "Sensor pengukur level cairan." },
  { "en": "Fungsi `CJC` pada thermocouple?", "id": "Kompensasi suhu sambungan dingin." },
  { "en": "Apa itu `closed-loop control`?", "id": "Kontrol dengan umpan balik (feedback)." },
  { "en": "Fungsi `servo drive`?", "id": "Mengontrol pergerakan motor servo." },
  { "en": "Bagaimana `stepper motor` bergerak?", "id": "Bertahap sesuai jumlah pulsa." },
  { "en": "Apa itu `common` pada input?", "id": "Terminal referensi bersama." },
  { "en": "Fungsi `end cover` di rack?", "id": "Menutup slot kosong terakhir." },
  { "en": "Fungsi `ladder diagram`?", "id": "Bahasa pemrograman grafis untuk PLC." },
  { "en": "Apa itu `bit`?", "id": "Unit data terkecil (0/1)." },
  { "en": "Apa itu `byte`?", "id": "Sekelompok 8 bit." },
  { "en": "Apa itu `word`?", "id": "Sekelompok 16 bit." },
  { "en": "Fungsi `baterai cadangan`?", "id": "Menjaga data di RAM." },
  { "en": "Tipe `PLC Omron Compact`?", "id": "Contohnya CP1E." },
  { "en": "Tipe `PLC Omron Modular`?", "id": "Contohnya CJ2M." },
  { "en": "Fungsi `working relay`?", "id": "Sebagai bit memori internal." },
  { "en": "Fungsi `data memory`?", "id": "Menyimpan nilai data (word)." },
  { "en": "Fungsi `timer` di PLC?", "id": "Memberikan waktu tunda." },
  { "en": "Fungsi `counter` di PLC?", "id": "Menghitung jumlah kejadian." },
  { "en": "Bagaimana `interlock` bekerja?", "id": "Mencegah dua output aktif bersamaan." },
  { "en": "Kapan `panel PLC` diperlukan?", "id": "Untuk melindungi dari lingkungan industri." },
  { "en": "Bagaimana `mengurangi panas` di panel?", "id": "Menggunakan kipas atau blower." },
  { "en": "Tujuan `grounding` yang baik?", "id": "Keamanan dan mengurangi noise listrik." },
  { "en": "Apa itu `sirkuit MCR`?", "id": "Master Control Relay untuk emergency." },
  { "en": "Bagaimana `kabel shield` diground?", "id": "Hanya pada satu sisi saja." },
  { "en": "Tindakan pertama `troubleshooting`?", "id": "Identifikasi masalah dan sumbernya." },
  { "en": "Apa itu `pohon troubleshooting`?", "id": "Diagram alur untuk memecahkan masalah." },
  { "en": "Apa itu `fixed automation`?", "id": "Otomasi untuk satu jenis produk." },
  { "en": "Apa itu `flexible automation`?", "id": "Otomasi yang mudah diubah." },
  { "en": "Contoh `flexible automation`?", "id": "Robot industri, mesin CNC." },
  { "en": "Tugas `robot` di industri?", "id": "Pekerjaan berat, berulang, berbahaya." },
  { "en": "Hukum `pertama robot`?", "id": "Tidak boleh melukai manusia." },
  { "en": "Hukum `kedua robot`?", "id": "Harus mematuhi perintah manusia." },
  { "en": "Hukum `ketiga robot`?", "id": "Harus melindungi eksistensi dirinya." },
  { "en": "Fungsi `gripper mekanik`?", "id": "Menjepit objek secara fisik." },
  { "en": "Fungsi `gripper vakum`?", "id": "Mengangkat objek dengan hisapan." },
  { "en": "Fungsi `gripper magnetik`?", "id": "Mengangkat objek logam dengan magnet." },
  { "en": "Fungsi `sequence control`?", "id": "Menjalankan proses secara berurutan." },
  { "en": "Bagaimana `relay` bekerja?", "id": "Menggunakan elektromagnet untuk menggerakkan saklar." },
  { "en": "Fungsi `N.O. contact`?", "id": "Normalnya terbuka, tertutup saat aktif." },
  { "en": "Fungsi `N.C. contact`?", "id": "Normalnya tertutup, terbuka saat aktif." },
  { "en": "Tujuan `simulasi CX Designer`?", "id": "Menguji HMI sebelum implementasi." },
  { "en": "Fungsi `sensor PNP`?", "id": "Output sourcing (memberi tegangan)." },
  { "en": "Fungsi `sensor NPN`?", "id": "Output sinking (menarik ke ground)." },
  { "en": "Fungsi `reed switch`?", "id": "Mendeteksi medan magnet." },
  { "en": "Aplikasi `reed switch`?", "id": "Sensor posisi pada silinder pneumatik." },
  { "en": "Kontak apa menginterlock motor MUNDUR?", "id": "Kontak NC dari motor MAJU." },
  { "en": "Kontak apa menginterlock motor KANAN?", "id": "Kontak NC dari motor KIRI." },
  { "en": "Kontak apa menginterlock motor TURUN?", "id": "Kontak NC dari motor NAIK." },
  { "en": "Syarat lampu HIJAU crane menyala?", "id": "Internal Relay IR-OPEN aktif." },
  { "en": "Penyebab lampu MERAH crane berkedip?", "id": "Sensor OVERLOAD aktif." },
  { "en": "Bagaimana mereset `OVERLOAD`?", "id": "Dengan menekan tombol OPEN." },
  { "en": "Apa itu `Cilliary Muscle`?", "id": "Otot pada mata." },
  { "en": "Apa itu `Iris` pada mata?", "id": "Bagian berwarna pada mata." },
  { "en": "Apa itu `Lens` pada mata?", "id": "Lensa untuk memfokuskan cahaya." },
  { "en": "Fungsi `Start PLC System`?", "id": "Mengaktifkan sirkuit kontrol utama." },
  { "en": "Fungsi `Stop PLC System`?", "id": "Mematikan sirkuit kontrol utama." },
  { "en": "Fungsi `Emergency Stop PB`?", "id": "Mematikan sistem dalam kondisi darurat." },
  { "en": "Alamat input BEL 3?", "id": "1:0.03." },
  { "en": "Alamat output BEL 2?", "id": "Q:100.02." },
  { "en": "Fungsi `KEEP 1` di ladder?", "id": "Menginterlock tombol BEL 2 & 3." },
  { "en": "Fungsi `TIMER` di bel kuis?", "id": "Mereset sistem setelah 5 detik." },
  { "en": "Fungsi `PLC IO Table` window?", "id": "Mengkonfigurasi dan melihat modul I/O." },
  { "en": "Apa itu `Inner Board`?", "id": "Board internal pada CPU." },
  { "en": "Apa itu `Main Rack`?", "id": "Rack utama tempat CPU berada." },
  { "en": "Deskripsi modul `CJ1W-CT021`?", "id": "High-speed Counter Unit." },
  { "en": "Fungsi `Verify` di IO Table?", "id": "Memverifikasi konfigurasi I/O." },
  { "en": "Fungsi `Delete` di IO Table?", "id": "Menghapus modul dari konfigurasi." },
  { "en": "Fungsi `Online Add Unit`?", "id": "Menambah modul saat PLC online." },
  { "en": "Fungsi `Change Order` di monitor?", "id": "Mengubah urutan tampilan data." },
  { "en": "Fungsi `Force On/Off`?", "id": "Memaksa status bit ON/OFF." },
  { "en": "Apa itu `Feedback Signals`?", "id": "Sinyal umpan balik dari sistem." },
  { "en": "Apa itu `Computer Outputs`?", "id": "Sinyal keluaran dari komputer." },
  { "en": "Tujuan `pemilihan peralatan` produksi?", "id": "Menentukan efisiensi proses produksi." },
  { "en": "Fungsi `per` pada pushbutton?", "id": "Mengembalikan tombol ke posisi awal." },
  { "en": "Fungsi `Pushbutton` di HMI?", "id": "Memberikan input ke PLC." },
  { "en": "Fungsi `Lampu` di HMI?", "id": "Menampilkan status output dari PLC." },
  { "en": "Apa itu `Conveyor`?", "id": "Ban berjalan untuk memindahkan barang." },
  { "en": "Fungsi `Buzzer`?", "id": "Memberikan sinyal suara." },
  { "en": "Apa itu `Solenoid Valve`?", "id": "Katup yang dikontrol secara elektrik." },
  { "en": "Fungsi `Motor Starter`?", "id": "Menghidupkan dan mematikan motor." },
  { "en": "Tujuan `simulasi` CX Designer?", "id": "Menguji program HMI dan PLC." },
  { "en": "Apa itu `interlock`?", "id": "Mencegah dua aksi berjalan bersamaan." },
  { "en": "Fungsi `Emergency Stop`?", "id": "Menghentikan semua operasi seketika." },
  { "en": "Apa itu `Level Control`?", "id": "Mengontrol ketinggian cairan dalam tangki." },
  { "en": "Fungsi `Sensor Level`?", "id": "Mendeteksi ketinggian cairan." },
  { "en": "Fungsi `Pump` (Pompa)?", "id": "Memindahkan cairan." },
  { "en": "Apa itu `PLC`?", "id": "Programmable Logic Controller." },
  { "en": "Bahasa pemrograman `PLC`?", "id": "Ladder Diagram." },
  { "en": "Fungsi `Ladder Diagram`?", "id": "Representasi grafis dari logika kontrol." },
  { "en": "Apa itu `Rung`?", "id": "Satu baris logika di ladder." },
  { "en": "Fungsi `Kontak NO`?", "id": "Tertutup saat input aktif." },
  { "en": "Fungsi `Kontak NC`?", "id": "Terbuka saat input aktif." },
  { "en": "Fungsi `Coil`?", "id": "Representasi output di ladder." },
  { "en": "Fungsi `Timer`?", "id": "Memberikan waktu tunda." },
  { "en": "Fungsi `Counter`?", "id": "Menghitung jumlah pulsa atau kejadian." },
  { "en": "Apa itu `input module`?", "id": "Menerima sinyal dari perangkat input." },
  { "en": "Apa itu `output module`?", "id": "Mengirim sinyal ke perangkat output." },
  { "en": "Fungsi `CPU` di PLC?", "id": "Mengeksekusi program dan memproses data." },
  { "en": "Apa itu `Power Supply`?", "id": "Memberikan daya ke sistem PLC." },
  { "en": "Tipe `sensor` berdasarkan output?", "id": "Digital dan Analog." },
  { "en": "Output `sensor digital`?", "id": "Sinyal ON atau OFF." },
  { "en": "Output `sensor analog`?", "id": "Rentang nilai tegangan atau arus." },
  { "en": "Apa itu sensor `PNP`?", "id": "Tipe sourcing (output positif)." },
  { "en": "Apa itu sensor `NPN`?", "id": "Tipe sinking (output negatif)." },
  { "en": "Fungsi `troubleshooting`?", "id": "Mencari dan memperbaiki kesalahan." },
  { "en": "Tindakan pertama `troubleshooting`?", "id": "Mengidentifikasi masalah." },
  { "en": "Fungsi `indikator LED` di modul?", "id": "Menunjukkan status input atau output." },
  { "en": "Apa itu `grounding`?", "id": "Menghubungkan sistem ke tanah." },
  { "en": "Tujuan `grounding`?", "id": "Keamanan dan mengurangi gangguan listrik." },
  { "en": "Apa itu `otomasi industri`?", "id": "Penggunaan sistem kontrol di industri." },
  { "en": "Tujuan `otomasi`?", "id": "Meningkatkan efisiensi dan produktivitas." },
  { "en": "Apa itu `robot industri`?", "id": "Robot yang digunakan dalam manufaktur." },
  { "en": "Fungsi `robot`?", "id": "Melakukan pekerjaan berulang dan berbahaya." },
  { "en": "Apa itu `manipulator`?", "id": "Lengan mekanik dari robot." },
  { "en": "Apa itu `efektor`?", "id": "Alat di ujung lengan robot." },
  { "en": "Jenis `gripper`?", "id": "Mekanik, vakum, magnetik." },
  { "en": "Apa itu `memori volatile`?", "id": "Memori yang datanya hilang tanpa daya." },
  { "en": "Contoh `memori volatile`?", "id": "RAM." },
  { "en": "Apa itu `memori non-volatile`?", "id": "Memori yang menyimpan data tanpa daya." },
  { "en": "Contoh `memori non-volatile`?", "id": "ROM, EEPROM, Flash." },
  { "en": "Fungsi `RAM` di PLC?", "id": "Menyimpan data program pengguna." },
  { "en": "Fungsi `ROM` di PLC?", "id": "Menyimpan sistem operasi." },
  { "en": "Apa itu `ADC`?", "id": "Analog to Digital Converter." },
  { "en": "Fungsi `ADC`?", "id": "Mengubah sinyal analog ke digital." },
  { "en": "Apa itu `DAC`?", "id": "Digital to Analog Converter." },
  { "en": "Fungsi `DAC`?", "id": "Mengubah sinyal digital ke analog." },
  { "en": "Apa itu `resolusi`?", "id": "Tingkat ketelitian pengukuran." },
  { "en": "Apa itu `sinking`?", "id": "Menarik arus ke common/ground." },
  { "en": "Apa itu `sourcing`?", "id": "Memberikan arus dari sumber tegangan." },
  { "en": "Alamat input SENSOR-1 pintu otomatis?", "id": "0.00." },
  { "en": "Alamat output PINTU.BUKA?", "id": "100.00." },
  { "en": "Fungsi `LIMIT SWITCH-1`?", "id": "Mendeteksi pintu terbuka penuh." },
  { "en": "Fungsi `LIMIT SWITCH-2`?", "id": "Mendeteksi pintu tertutup penuh." },
  { "en": "Alamat input OVERLOAD crane?", "id": "0.03." },
  { "en": "Alamat output K KANAN crane?", "id": "100.03." },
  { "en": "Fungsi `IR-LOCK` pada crane?", "id": "Mengunci operasi saat ada masalah." },
  { "en": "Alamat input START mobil cuci?", "id": "0.00." },
  { "en": "Alamat input STOP mobil cuci?", "id": "0.01." },
  { "en": "Alamat output SPRAY mobil cuci?", "id": "100.00." },
  { "en": "Alamat output CONVEYOR mobil cuci?", "id": "100.01." },
  { "en": "Alamat output BRUSH mobil cuci?", "id": "100.02." },
  { "en": "Alamat input BEL #1 kuis?", "id": "0.01." },
  { "en": "Alamat input BEL #2 kuis?", "id": "0.02." },
  { "en": "Alamat input BEL #3 kuis?", "id": "0.03." },
  { "en": "Alamat output LAMPU #1 kuis?", "id": "100.01." },
  { "en": "Alamat output LAMPU #2 kuis?", "id": "100.02." },
  { "en": "Alamat output LAMPU #3 kuis?", "id": "100.03." },
  { "en": "Fungsi `KEEP` di ladder?", "id": "Instruksi pengunci atau latching." },
  { "en": "Fungsi `DIFD` di ladder?", "id": "Mendeteksi sinyal tepi turun." },
  { "en": "Fungsi `DIFU` di ladder?", "id": "Mendeteksi sinyal tepi naik." },
  { "en": "Apa itu `common` pada output?", "id": "Terminal referensi bersama untuk output." },
  { "en": "Bagaimana `menguji input` PLC?", "id": "Aktifkan perangkat dan lihat indikator." },
  { "en": "Bagaimana `menguji output` PLC?", "id": "Gunakan fungsi force atau program tes." },
  { "en": "Tindakan jika `indikator FLT` menyala?", "id": "Periksa kesalahan pada prosesor." },
  { "en": "Tindakan jika `indikator BATT` menyala?", "id": "Ganti baterai cadangan." },
  { "en": "Apa itu `rack` pada PLC modular?", "id": "Kerangka untuk menempatkan modul." },
  { "en": "Apa itu `slot` pada PLC modular?", "id": "Posisi untuk satu modul." },
  { "en": "Tujuan `pemetaan memori`?", "id": "Mengetahui alamat dan kapasitas I/O." },
  { "en": "Apa itu `backplane`?", "id": "Papan sirkuit di belakang rack." },
  { "en": "Fungsi `backplane`?", "id": "Menghubungkan modul dengan CPU." },
  { "en": "Bagaimana `instalasi modul`?", "id": "Masukkan modul ke dalam slot." },
  { "en": "Mengapa `ventilasi` panel penting?", "id": "Untuk membuang panas." },
  { "en": "Bahaya `debu konduktif`?", "id": "Dapat menyebabkan korsleting." },
  { "en": "Fungsi `Hukum Robot`?", "id": "Panduan etika untuk robot." },
  { "en": "Apa itu `artificial intelligence`?", "id": "Kecerdasan buatan pada mesin." },
  { "en": "Tujuan `pemilihan peralatan` yang tepat?", "id": "Mencapai efisiensi proses yang tinggi." },
  { "en": "Apa itu `I/O point`?", "id": "Terminal koneksi untuk input/output." },
  { "en": "Apa itu `field device`?", "id": "Perangkat yang terhubung ke PLC." },
  { "en": "Fungsi `terminator` pada jaringan?", "id": "Mencegah pantulan sinyal." },
  { "en": "Fungsi `kabel twisted-pair`?", "id": "Mengurangi interferensi elektromagnetik." },
  { "en": "Apa itu `common positive`?", "id": "Terminal common terhubung ke positif." },
  { "en": "Apa itu `common negative`?", "id": "Terminal common terhubung ke negatif." },
  { "en": "Fungsi `KEEP(011)`?", "id": "Instruksi pengunci di CX-Programmer." },
  { "en": "Apa itu `DIFD(014)`?", "id": "Instruksi deteksi tepi turun." },
  { "en": "Tujuan `interlock` motor crane?", "id": "Hanya satu gerakan bisa aktif." },
  { "en": "Bagaimana `mereset` emergency crane?", "id": "Menggunakan tombol OPEN." },
  { "en": "Kapan `lampu merah` crane berkedip?", "id": "Saat terjadi OVERLOAD." },
  { "en": "Fungsi `sensor berat` crane?", "id": "Mendeteksi kondisi OVERLOAD." },
  { "en": "Alamat `IR31` pada crane?", "id": "600.00." },
  { "en": "Alamat input `MAJU` crane?", "id": "0.04." },
  { "en": "Alamat input `MUNDUR` crane?", "id": "0.05." },
  { "en": "Alamat input `KIRI` crane?", "id": "0.06." },
  { "en": "Alamat input `KANAN` crane?", "id": "0.07." },
  { "en": "Alamat input `NAIK` crane?", "id": "0.08." },
  { "en": "Alamat input `TURUN` crane?", "id": "0.09." },
  { "en": "Fungsi `P_0_02s` di ladder?", "id": "Pulsa kedip 0.02 detik." },
  { "en": "Fungsi `T0001` di ladder?", "id": "Timer nomor 1." },
  { "en": "Apa itu `set value` #20?", "id": "Timer diatur selama 2 detik." },
  { "en": "Fungsi `PLC Memory Window`?", "id": "Melihat isi memori PLC." },
  { "en": "Bagaimana `mengubah format` data?", "id": "Pilih Hex, Decimal, Biner." },
  { "en": "Fungsi `analog control` di PLC?", "id": "Potensiometer untuk mengatur nilai." },
  { "en": "Fungsi `seven-segment display`?", "id": "Menampilkan kode status/error." },
  { "en": "Fungsi `DIP switch` di PLC?", "id": "Mengatur konfigurasi dasar." },
  { "en": "Fungsi `expansion unit connector`?", "id": "Menghubungkan modul I/O tambahan." },
  { "en": "Fungsi `memory cassette slot`?", "id": "Untuk memori eksternal." },
  { "en": "Apa itu `built-in I/O`?", "id": "I/O yang terintegrasi di CPU." },
  { "en": "Range tegangan `built-in analog`?", "id": "0-10V, 0-5V, 1-5V, dll." },
  { "en": "Range arus `built-in analog`?", "id": "0-20mA, 4-20mA." },
  { "en": "Fungsi `Servo-controller` pada robot?", "id": "Mengontrol posisi motor servo." },
  { "en": "Fungsi `regulator` pada proses?", "id": "Menjaga variabel proses tetap stabil." },
  { "en": "Berapa `resolusi` modul AD081?", "id": "4000 atau 8000." },
  { "en": "Bagaimana `mengatur unit number`?", "id": "Dengan saklar putar di modul." },
  { "en": "Fungsi `hot swap`?", "id": "Mengganti modul saat sistem aktif." },
  { "en": "Alamat `DeviceNet Area`?", "id": "CIO 3200 hingga 3799." },
  { "en": "Fungsi `phototransistor` di optoisolator?", "id": "Mendeteksi cahaya dari LED." },
  { "en": "Fungsi `bridge rectifier`?", "id": "Mengubah tegangan AC ke DC." },
  { "en": "Penyebab `false signal`?", "id": "Contact bounce atau interferensi listrik." },
  { "en": "Fungsi `MOV` (Varistor)?", "id": "Melindungi sirkuit dari lonjakan tegangan." },
  { "en": "Arti `AC OK` indikator?", "id": "Catu daya AC normal." },
  { "en": "Arti `Processor OK` indikator?", "id": "CPU bekerja dengan baik." },
  { "en": "Tindakan saat `output tidak aktif`?", "id": "Periksa sekring, kabel, dan perangkat." },
  { "en": "Apa itu `real-time control`?", "id": "Sistem yang merespon seketika." },
  { "en": "Apa itu `distributed system`?", "id": "Sistem dengan banyak kontroler lokal." },
  { "en": "Fungsi `LAN`?", "id": "Local Area Network, jaringan lokal." },
  { "en": "Apa itu `apendage` pada robot?", "id": "Bagian lengan tambahan robot." },
  { "en": "Fungsi `power supply unit`?", "id": "Menyediakan daya untuk semua modul." },
  { "en": "Fungsi `backplane` di rack?", "id": "Menghubungkan modul secara elektrik." },
  { "en": "Apa itu `sinking output`?", "id": "Menarik arus ke common." },
  { "en": "Apa itu `sourcing output`?", "id": "Memberikan arus dari sumber." },
  { "en": "Fungsi `timer` pada lampu berjalan?", "id": "Mengatur durasi nyala setiap lampu." },
  { "en": "Fungsi `interlock` di bel kuis?", "id": "Mencegah pemain lain menekan bel." },
  { "en": "Tujuan `simulasi` pintu garasi?", "id": "Menguji logika program sebelum dipasang." },
  { "en": "Fungsi `sensor-1` pintu garasi?", "id": "Mendeteksi mobil yang dikenal." },
  { "en": "Fungsi `sensor-2` pintu garasi?", "id": "Mendeteksi mobil melewati pintu." },
  { "en": "Fungsi `sensor-3` pintu garasi?", "id": "Mendeteksi mesin mobil hidup." },
  { "en": "Kapan `pintu garasi` menutup?", "id": "Setelah mobil lewat atau LS1 aktif." },
  { "en": "Fungsi `level control`?", "id": "Menjaga level cairan di tangki." },
  { "en": "Kapan `valve_1` terbuka?", "id": "Saat tombol start ditekan." },
  { "en": "Kapan `pump_1` berjalan?", "id": "3 detik setelah valve_1 terbuka." },
  { "en": "Kapan `valve_2` terbuka?", "id": "Saat level cairan > 70 lt." },
  { "en": "Kapan `valve_2` tertutup?", "id": "Saat level cairan < 20 lt." },
  { "en": "Fungsi `timer` di level control?", "id": "Memberikan jeda waktu antar aksi." },
  { "en": "Fungsi `input discrete`?", "id": "Memberikan sinyal ON/OFF." },
  { "en": "Fungsi `output discrete`?", "id": "Mengaktifkan/mematikan perangkat." },
  { "en": "Tegangan `TTL level`?", "id": "5V DC." },
  { "en": "Fungsi `encoder`?", "id": "Memberikan feedback posisi atau kecepatan." },
  { "en": "Apa itu `PID`?", "id": "Proportional, Integral, Derivative." },
  { "en": "Tujuan `PID control`?", "id": "Menjaga variabel proses tetap stabil." },
  { "en": "Apa itu `closed-loop`?", "id": "Sistem kontrol dengan umpan balik." },
  { "en": "Fungsi `servo motor`?", "id": "Motor untuk kontrol posisi presisi." },
  { "en": "Fungsi `stepper motor`?", "id": "Motor yang bergerak per langkah." },
  { "en": "Fungsi `CANbus`?", "id": "Jaringan komunikasi untuk perangkat." },
  { "en": "Fungsi `Profibus`?", "id": "Jaringan komunikasi untuk proses." },
  { "en": "Apa itu `common` pada I/O?", "id": "Terminal referensi bersama." },
  { "en": "Fungsi `coil` di relay?", "id": "Menghasilkan medan magnet." },
  { "en": "Fungsi `kontak` di relay?", "id": "Membuka atau menutup sirkuit." },
  { "en": "Tujuan `pemetaan memori`?", "id": "Mengetahui alamat dan jumlah I/O." },
  { "en": "Apa itu `hot swap`?", "id": "Mengganti modul tanpa mematikan daya." },
  { "en": "Fungsi `firmware`?", "id": "Perangkat lunak sistem operasi." },
  { "en": "Dimana `firmware` disimpan?", "id": "Di dalam ROM." },
  { "en": "Fungsi `Hukum Robot`?", "id": "Menjadi panduan etika bagi robot." },
  { "en": "Apa itu `work envelope`?", "id": "Area jangkauan kerja robot." },
  { "en": "Bagaimana `gripper vakum` bekerja?", "id": "Menggunakan perbedaan tekanan udara." },
  { "en": "Syarat `gripper vakum`?", "id": "Permukaan objek harus rata." },
  { "en": "Kelebihan `gripper magnetik`?", "id": "Bisa mengangkat objek berlubang." },
  { "en": "Tujuan `ventilasi panel`?", "id": "Mencegah komponen dari panas berlebih." },
  { "en": "Bagaimana `kabel sinyal` dirutekan?", "id": "Terpisah dari kabel daya tinggi." },
  { "en": "Fungsi `sekring`?", "id": "Melindungi sirkuit dari arus berlebih." },
  { "en": "Apa itu `contact bounce`?", "id": "Loncatan sinyal pada saklar mekanik." },
  { "en": "Fungsi `noise filter`?", "id": "Menghilangkan sinyal gangguan." },
  { "en": "Apa itu `proprietary network`?", "id": "Jaringan khusus dari satu vendor." },
  { "en": "Fungsi `BCD`?", "id": "Binary-Coded Decimal." },
  { "en": "Apa itu `latching`?", "id": "Mengunci kondisi output tetap ON." },
  { "en": "Fungsi `sinking input module`?", "id": "Menerima arus dari sensor sourcing." },
  { "en": "Fungsi `sourcing input module`?", "id": "Memberikan arus ke sensor sinking." },
  { "en": "Alamat `Pulse I/O Area`?", "id": "CIO 2960 hingga 2963." },
  { "en": "Berapa `Serial PLC Link Words`?", "id": "1.440 bit (90 word)." },
  { "en": "Alamat `Internal I/O Area`?", "id": "CIO 1300 hingga 1499." },
  { "en": "Tujuan `pemanasan` di panel?", "id": "Mencegah kondensasi atau pengembunan." },
  { "en": "Tindakan saat `output device` korslet?", "id": "Ganti sekring dan perbaiki wiring." },
  { "en": "Penyebab `input tidak mau` OFF?", "id": "Perangkat korslet atau leakage current." },
  { "en": "Tujuan `Hukum Ke-Nol` Robot?", "id": "Melindungi kemanusiaan secara keseluruhan." }


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
