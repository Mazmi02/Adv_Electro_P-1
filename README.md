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


  { "en": "Apa Satuan Tegangan Listrik Dalam Standar Internasional?", "id": "Volt." },
  { "en": "Apa Fungsi Utama Dari Komponen Resistor?", "id": "Hambat Arus." },
  { "en": "Apa Rumus Hukum Ohm Pada Rangkaian Listrik?", "id": "V = I x R." },
  { "en": "Apa Satuan Dari Daya Listrik Nyata?", "id": "Watt." },
  { "en": "Apa Kepanjangan Dari Singkatan AC Dalam Listrik?", "id": "Alternating Current." },
  { "en": "Apa Kepanjangan Dari Singkatan DC Dalam Listrik?", "id": "Direct Current." },
  { "en": "Apa Fungsi Dari Kapasitor Dalam Rangkaian DC?", "id": "Simpan Muatan." },
  { "en": "Apa Satuan Dari Besaran Kapasitansi Kapasitor?", "id": "Farad." },
  { "en": "Apa Fungsi Induktor Pada Arus Bolak Balik?", "id": "Tahan Perubahan Arus." },
  { "en": "Apa Satuan Induktansi Pada Sebuah Induktor?", "id": "Henry." },
  { "en": "Apa Itu Dioda Dalam Elektronika Dasar?", "id": "Penyearah Arus." },
  { "en": "Apa Fungsi Dioda Zener Pada Regulator?", "id": "Stabilkan Tegangan." },
  { "en": "Apa Kepanjangan Dari BJT Pada Transistor?", "id": "Bipolar Junction Transistor." },
  { "en": "Apa Tiga Terminal Pada Transistor Bipolar?", "id": "Basis, Kolektor, Emitor." },
  { "en": "Apa Kepanjangan Dari MOSFET Pada Transistor?", "id": "Metal Oxide Semiconductor Field Effect Transistor." },
  { "en": "Apa Tiga Terminal Pada Transistor MOSFET?", "id": "Gate, Drain, Source." },
  { "en": "Apa Fungsi Utama Transformator Step Up?", "id": "Naikkan Tegangan." },
  { "en": "Apa Fungsi Utama Transformator Step Down?", "id": "Turunkan Tegangan." },
  { "en": "Apa Prinsip Dasar Induksi Elektromagnetik?", "id": "Fluks Hasilkan GGL." },
  { "en": "Apa Satuan Dari Frekuensi Listrik AC?", "id": "Hertz." },
  { "en": "Apa Rumus Menghitung Daya Listrik DC?", "id": "P = V x I." },
  { "en": "Apa Bunyi Hukum Kirchoff Current Law (KCL)?", "id": "Sigma Masuk = Sigma Keluar." },
  { "en": "Apa Bunyi Hukum Kirchoff Voltage Law (KVL)?", "id": "Sigma V = 0." },
  { "en": "Apa Fungsi Sekring Atau Fuse Pada Rangkaian?", "id": "Putus Arus Lebih." },
  { "en": "Apa Sifat Rangkaian Seri Terhadap Arus?", "id": "I Sama Rata." },
  { "en": "Apa Sifat Rangkaian Paralel Terhadap Tegangan?", "id": "V Sama Rata." },
  { "en": "Apa Rumus Tegangan RMS Gelombang Sinus?", "id": "0.707 x Vp." },
  { "en": "Apa Rumus Tegangan Puncak Ke Puncak (Vpp)?", "id": "2 x Vp." },
  { "en": "Apa Definisi Dari Bahan Konduktor Listrik?", "id": "Hantar Listrik." },
  { "en": "Apa Definisi Dari Bahan Isolator Listrik?", "id": "Tahan Arus." },
  { "en": "Apa Pembawa Muatan Pada Semikonduktor Tipe N?", "id": "Elektron." },
  { "en": "Apa Pembawa Muatan Pada Semikonduktor Tipe P?", "id": "Hole." },
  { "en": "Apa Kepanjangan Dari LED Dalam Elektronika?", "id": "Light Emitting Diode." },
  { "en": "Apa Fungsi Utama Dari Op-Amp?", "id": "Penguat Sinyal." },
  { "en": "Apa Kepanjangan Dari Op-Amp Itu Sendiri?", "id": "Operational Amplifier." },
  { "en": "Berapa Nilai Impedansi Input Op-Amp Ideal?", "id": "Tak Hingga." },
  { "en": "Berapa Nilai Impedansi Output Op-Amp Ideal?", "id": "Nol." },
  { "en": "Apa Fungsi Dari Rangkaian Rectifier?", "id": "Ubah AC Ke DC." },
  { "en": "Apa Fungsi Rangkaian Jembatan Wheatstone?", "id": "Ukur Resistansi Presisi." },
  { "en": "Apa Rumus Faktor Daya Atau Power Factor?", "id": "PF = P / S." },
  { "en": "Apa Akibat Jika Faktor Daya Rendah?", "id": "Arus Naik." },
  { "en": "Apa Fungsi Kapasitor Bank Di Industri?", "id": "Perbaiki PF." },
  { "en": "Apa Rumus Reaktansi Induktif (XL)?", "id": "2 x Pi x f x L." },
  { "en": "Apa Rumus Reaktansi Kapasitif (XC)?", "id": "1 / (2 x Pi x f x C)." },
  { "en": "Apa Definisi Dari Impedansi Listrik?", "id": "Total Hambatan AC." },
  { "en": "Apa Satuan Dari Muatan Listrik?", "id": "Coulomb." },
  { "en": "Apa Kepanjangan Dari PCB Dalam Elektronika?", "id": "Printed Circuit Board." },
  { "en": "Apa Fungsi Dari Relay Elektromekanik?", "id": "Saklar Elektromagnetik." },
  { "en": "Apa Rumus Logika Untuk Gerbang AND?", "id": "Y = A . B." },
  { "en": "Apa Rumus Logika Untuk Gerbang OR?", "id": "Y = A + B." },
  { "en": "Apa Rumus Logika Untuk Gerbang NOT?", "id": "Y = A Bar." },
  { "en": "Apa Itu Tabel Kebenaran Logika?", "id": "Peta Input Output." },
  { "en": "Apa Kepanjangan Dari IC Dalam Komponen?", "id": "Integrated Circuit." },
  { "en": "Apa Konversi Energi Pada Motor DC?", "id": "Listrik Ke Mekanik." },
  { "en": "Apa Konversi Energi Pada Generator Listrik?", "id": "Mekanik Ke Listrik." },
  { "en": "Apa Fungsi Komutator Pada Motor DC?", "id": "Penyearah Mekanis." },
  { "en": "Apa Itu Stator Pada Mesin Listrik?", "id": "Bagian Diam." },
  { "en": "Apa Itu Rotor Pada Mesin Listrik?", "id": "Bagian Putar." },
  { "en": "Apa Kepanjangan Dari RPM Kecepatan Motor?", "id": "Revolutions Per Minute." },
  { "en": "Apa Definisi Torsi Pada Motor Listrik?", "id": "Gaya Putar." },
  { "en": "Apa Fungsi Inverter Pada Sistem Tenaga?", "id": "Ubah DC Ke AC." },
  { "en": "Apa Kepanjangan Dari UPS Sistem Daya?", "id": "Uninterruptible Power Supply." },
  { "en": "Apa Fungsi Baterai Pada Sistem UPS?", "id": "Cadangan Daya." },
  { "en": "Apa Tujuan Utama Grounding Atau Pembumian?", "id": "Keamanan." },
  { "en": "Apa Warna Kabel Ground Standar IEC?", "id": "Kuning Hijau." },
  { "en": "Apa Fungsi MCB Di Rumah Tinggal?", "id": "Proteksi Beban Lebih." },
  { "en": "Apa Kepanjangan Dari MCB Itu Sendiri?", "id": "Miniature Circuit Breaker." },
  { "en": "Apa Itu Arus Bocor Pada Instalasi?", "id": "Arus Ke Tanah." },
  { "en": "Apa Kepanjangan ELCB Untuk Pengaman?", "id": "Earth Leakage Circuit Breaker." },
  { "en": "Apa Fungsi Utama Dari ELCB?", "id": "Cegah Setrum." },
  { "en": "Apa Syarat Terjadinya Frekuensi Resonansi?", "id": "XL = XC." },
  { "en": "Apa Definisi Bandwidth Pada Sinyal?", "id": "Lebar Pita." },
  { "en": "Apa Karakteristik Filter Low Pass (LPF)?", "id": "Lolos Frekuensi Bawah." },
  { "en": "Apa Karakteristik Filter High Pass (HPF)?", "id": "Lolos Frekuensi Atas." },
  { "en": "Apa Kepanjangan Dari ADC Sinyal?", "id": "Analog To Digital Converter." },
  { "en": "Apa Kepanjangan Dari DAC Sinyal?", "id": "Digital To Analog Converter." },
  { "en": "Apa Sifat Utama Sinyal Analog?", "id": "Kontinyu." },
  { "en": "Apa Sifat Utama Sinyal Digital?", "id": "Diskrit." },
  { "en": "Apa Sistem Bilangan Dasar Komputer?", "id": "Biner." },
  { "en": "Apa Itu Bit Dalam Data Digital?", "id": "Binary Digit." },
  { "en": "Berapa Bit Dalam Satu Byte?", "id": "8 Bit." },
  { "en": "Apa Kepanjangan PLC Di Industri?", "id": "Programmable Logic Controller." },
  { "en": "Apa Bahasa Pemrograman Visual PLC?", "id": "Ladder Diagram." },
  { "en": "Apa Fungsi Dasar Sensor Transduser?", "id": "Ubah Energi." },
  { "en": "Apa Sifat Sensor LDR Terhadap Cahaya?", "id": "Cahaya Naik, R Turun." },
  { "en": "Apa Kepanjangan LDR Pada Sensor?", "id": "Light Dependent Resistor." },
  { "en": "Apa Fungsi Sensor Termokopel?", "id": "Ukur Suhu." },
  { "en": "Apa Fungsi Transformator Arus (CT)?", "id": "Turunkan Arus." },
  { "en": "Apa Kepanjangan CT Pada Trafo?", "id": "Current Transformer." },
  { "en": "Apa Fungsi Transformator Tegangan (PT)?", "id": "Turunkan Tegangan." },
  { "en": "Apa Kepanjangan PT Pada Trafo?", "id": "Potential Transformer." },
  { "en": "Apa Rumus Rugi Tembaga Trafo?", "id": "I Kuadrat R." },
  { "en": "Apa Penyebab Rugi Inti Trafo?", "id": "Histeresis Dan Eddy." },
  { "en": "Apa Fenomena Skin Effect Pada Kabel?", "id": "Arus Di Kulit." },
  { "en": "Apa Itu Tegangan Breakdown Isolator?", "id": "Tegangan Tembus." },
  { "en": "Apa Kepanjangan SCR Komponen Daya?", "id": "Silicon Controlled Rectifier." },
  { "en": "Apa Fungsi Utama Komponen Triac?", "id": "Saklar AC 2 Arah." },
  { "en": "Apa Ciri Utama Motor Induksi?", "id": "Tanpa Sikat." },
  { "en": "Apa Rumus Slip Pada Motor Induksi?", "id": "(Ns - Nr) / Ns." },
  { "en": "Apa Satuan Internasional Fluks Magnetik?", "id": "Weber." },
  { "en": "Apa Satuan Internasional Densitas Fluks?", "id": "Tesla." },
  { "en": "Apa Rumus Gaya Lorentz Kawat Berarus?", "id": "F = B x I x L." },
  { "en": "Apa Bunyi Teorema Thevenin Rangkaian Listrik?", "id": "Sumber Tegangan Seri Resistansi." },
  { "en": "Apa Bunyi Teorema Norton Rangkaian Listrik?", "id": "Sumber Arus Paralel Resistansi." },
  { "en": "Apa Syarat Transfer Daya Maksimum?", "id": "R Beban Sama Dengan R Sumber." },
  { "en": "Apa Itu Prinsip Superposisi Rangkaian?", "id": "Analisis Tiap Sumber Terpisah." },
  { "en": "Apa Satuan Daya Reaktif (Q) AC?", "id": "VAR." },
  { "en": "Apa Satuan Daya Semu (S) AC?", "id": "Volt Ampere." },
  { "en": "Apa Rumus Segitiga Daya S Kuadrat?", "id": "P Kuadrat Tambah Q Kuadrat." },
  { "en": "Apa Hubungan Tegangan Line Pada Hubungan Star?", "id": "Akar Tiga Kali Tegangan Fasa." },
  { "en": "Apa Hubungan Arus Line Pada Hubungan Delta?", "id": "Akar Tiga Kali Arus Fasa." },
  { "en": "Apa Keuntungan Sistem Tiga Fasa Dibanding Satu?", "id": "Daya Lebih Besar Kabel Kecil." },
  { "en": "Apa Warna Kabel Netral Standar PUIL?", "id": "Biru." },
  { "en": "Apa Warna Kabel Fasa R Standar Baru?", "id": "Cokelat." },
  { "en": "Apa Kepanjangan MOSFET Tipe Enhancement?", "id": "Enhancement Metal Oxide Semiconductor." },
  { "en": "Apa Ciri Utama Penguat Kelas A?", "id": "Konduksi 360 Derajat." },
  { "en": "Apa Ciri Utama Penguat Kelas B?", "id": "Konduksi 180 Derajat." },
  { "en": "Apa Masalah Utama Pada Penguat Kelas B?", "id": "Crossover Distortion." },
  { "en": "Apa Ciri Utama Penguat Kelas C?", "id": "Efisiensi Tinggi Distorsi Besar." },
  { "en": "Apa Fungsi Kapasitor Kopling Pada Amplifier?", "id": "Blokir DC Loloskan AC." },
  { "en": "Apa Itu Common Mode Rejection Ratio (CMRR)?", "id": "Kemampuan Tolak Noise." },
  { "en": "Apa Definisi Slew Rate Pada Op-Amp?", "id": "Kecepatan Perubahan Tegangan Output." },
  { "en": "Apa Kepanjangan CMOS Dalam Logika Digital?", "id": "Complementary Metal Oxide Semiconductor." },
  { "en": "Apa Kelebihan Utama Gerbang Logika CMOS?", "id": "Konsumsi Daya Rendah." },
  { "en": "Apa Fungsi Flip-Flop Dalam Rangkaian Digital?", "id": "Penyimpan Memori 1 Bit." },
  { "en": "Apa Karakteristik Flip-Flop Tipe D?", "id": "Output Ikut Input Data." },
  { "en": "Apa Kondisi Toggle Pada JK Flip-Flop?", "id": "J Dan K Bernilai Satu." },
  { "en": "Apa Kepanjangan PWM Dalam Kontrol Motor?", "id": "Pulse Width Modulation." },
  { "en": "Apa Fungsi Utama Sinyal PWM?", "id": "Mengatur Daya Rata Rata." },
  { "en": "Apa Itu Duty Cycle Pada PWM?", "id": "Rasio Waktu On Terhadap Periode." },
  { "en": "Apa Kepanjangan IGBT Komponen Daya?", "id": "Insulated Gate Bipolar Transistor." },
  { "en": "Apa Gabungan Karakteristik Pada IGBT?", "id": "Input MOSFET Output BJT." },
  { "en": "Apa Fungsi Dioda Freewheeling Pada Relay?", "id": "Cegah Tegangan Balik Induktif." },
  { "en": "Apa Itu Efek Hall Atau Hall Effect?", "id": "Medan Magnet Hasilkan Beda Potensial." },
  { "en": "Apa Fungsi Sensor Hall Effect?", "id": "Deteksi Posisi Atau Arus." },
  { "en": "Apa Kepanjangan VFD Pengendali Motor?", "id": "Variable Frequency Drive." },
  { "en": "Apa Fungsi Utama Alat VFD?", "id": "Atur Kecepatan Motor AC." },
  { "en": "Apa Itu Soft Starter Pada Motor?", "id": "Kurangi Arus Lonjakan Awal." },
  { "en": "Apa Kepanjangan SCADA Sistem Kontrol?", "id": "Supervisory Control And Data Acquisition." },
  { "en": "Apa Kepanjangan HMI Dalam Industri?", "id": "Human Machine Interface." },
  { "en": "Apa Itu Open Loop Control System?", "id": "Tanpa Umpan Balik." },
  { "en": "Apa Itu Closed Loop Control System?", "id": "Pakai Umpan Balik." },
  { "en": "Apa Kepanjangan PID Controller?", "id": "Proportional Integral Derivative." },
  { "en": "Apa Fungsi Aksi Integral Pada PID?", "id": "Hilangkan Error Steady State." },
  { "en": "Apa Fungsi Aksi Derivatif Pada PID?", "id": "Respon Cepat Perubahan." },
  { "en": "Apa Itu Transformasi Laplace?", "id": "Ubah Domain Waktu Ke S." },
  { "en": "Apa Itu Transfer Function Sistem?", "id": "Output Dibagi Input Domain S." },
  { "en": "Apa Definisi Stabilitas Sistem Kontrol?", "id": "Output Terbatas Input Terbatas." },
  { "en": "Apa Itu Root Locus Dalam Kontrol?", "id": "Plot Akar Persamaan Karakteristik." },
  { "en": "Apa Itu Diagram Bode Plot?", "id": "Grafik Respon Frekuensi." },
  { "en": "Apa Satuan Gain Pada Bode Plot?", "id": "Decibel." },
  { "en": "Apa Rumus Decibel Untuk Tegangan?", "id": "20 Log Vout Per Vin." },
  { "en": "Apa Itu Efek Corona Pada Transmisi?", "id": "Ionisasi Udara Sekitar Kabel." },
  { "en": "Apa Penyebab Bunyi Mendesis SUTET?", "id": "Efek Corona." },
  { "en": "Apa Kepanjangan SUTET Di Indonesia?", "id": "Saluran Udara Tegangan Ekstra Tinggi." },
  { "en": "Berapa Tegangan Transmisi SUTET Indonesia?", "id": "500 Kilo Volt." },
  { "en": "Apa Fungsi Isolator Pada Tiang Listrik?", "id": "Sekat Kabel Dari Tiang." },
  { "en": "Apa Bahan Isolator Paling Umum?", "id": "Keramik Atau Polimer." },
  { "en": "Apa Fungsi Gardu Induk Listrik?", "id": "Transformasi Dan Distribusi Daya." },
  { "en": "Apa Fungsi Relay Buchholz Trafo?", "id": "Deteksi Gas Dalam Minyak." },
  { "en": "Apa Itu Tap Changer Pada Trafo?", "id": "Pengatur Rasio Belitan." },
  { "en": "Apa Fungsi Minyak Trafo?", "id": "Pendingin Dan Isolasi." },
  { "en": "Apa Itu Short Circuit Atau Hubung Singkat?", "id": "Arus Lewat Hambatan Nol." },
  { "en": "Apa Itu Overload Atau Beban Lebih?", "id": "Arus Melebihi Kapasitas Nominal." },
  { "en": "Apa Fungsi Lightning Arrester?", "id": "Lindungi Dari Sambaran Petir." },
  { "en": "Apa Kepanjangan PLTU Pembangkit?", "id": "Pembangkit Listrik Tenaga Uap." },
  { "en": "Apa Kepanjangan PLTA Pembangkit?", "id": "Pembangkit Listrik Tenaga Air." },
  { "en": "Apa Komponen Utama Pembangkit Surya?", "id": "Sel Fotovoltaik." },
  { "en": "Apa Arus Output Panel Surya?", "id": "DC." },
  { "en": "Apa Itu Grid Tie Inverter?", "id": "Sinkronisasi Surya Ke PLN." },
  { "en": "Apa Itu Baterai Lead Acid?", "id": "Aki Basah Asam Timbal." },
  { "en": "Apa Tegangan Nominal Satu Sel Li-Ion?", "id": "3.7 Volt." },
  { "en": "Apa Satuan Kapasitas Baterai?", "id": "Ampere Hour." },
  { "en": "Apa Itu Depth Of Discharge (DOD)?", "id": "Persentase Pemakaian Baterai." },
  { "en": "Apa Kepanjangan RAM Memori?", "id": "Random Access Memory." },
  { "en": "Apa Sifat Memori Volatile?", "id": "Data Hilang Tanpa Listrik." },
  { "en": "Apa Kepanjangan ROM Memori?", "id": "Read Only Memory." },
  { "en": "Apa Kepanjangan EEPROM Memori?", "id": "Electrically Erasable Programmable ROM." },
  { "en": "Apa Perbedaan Mikrokontroler Dan Mikroprosesor?", "id": "Mikrokontroler Ada RAM Internal." },
  { "en": "Apa Fungsi Crystal Oscillator Pada Mikro?", "id": "Sumber Detak Sistem." },
  { "en": "Apa Itu I2C Communication Protocol?", "id": "Komunikasi Serial Dua Kabel." },
  { "en": "Apa Dua Kabel Pada Protokol I2C?", "id": "SDA Dan SCL." },
  { "en": "Apa Kepanjangan SPI Communication?", "id": "Serial Peripheral Interface." },
  { "en": "Apa Kepanjangan UART Communication?", "id": "Universal Asynchronous Receiver Transmitter." },
  { "en": "Apa Itu Baud Rate Komunikasi?", "id": "Kecepatan Transfer Data." },
  { "en": "Apa Fungsi Multiplexer (MUX)?", "id": "Pilih Satu Dari Banyak Input." },
  { "en": "Apa Fungsi Demultiplexer (DEMUX)?", "id": "Sebar Satu Input Ke Banyak." },
  { "en": "Apa Itu Seven Segment Display?", "id": "Tampilan Tujuh Ruas LED." },
  { "en": "Apa Perbedaan Anoda Dan Katoda?", "id": "Positif Dan Negatif." },
  { "en": "Apa Itu Osiloskop?", "id": "Alat Visualisasi Sinyal." },
  { "en": "Apa Sumbu X Pada Osiloskop?", "id": "Waktu." },
  { "en": "Apa Sumbu Y Pada Osiloskop?", "id": "Tegangan." },
  { "en": "Apa Fungsi Probe Kalibrasi Osiloskop?", "id": "Kompensasi Kapasitansi Kabel." },
  { "en": "Apa Itu Permeabilitas Magnetik?", "id": "Kemampuan Bahan Loloskan Fluks." },
  { "en": "Apa Hukum Faraday Induksi?", "id": "GGL Sebanding Laju Fluks." },
  { "en": "Apa Hukum Lenz Tentang Arah Arus?", "id": "Arah Lawan Penyebabnya." },
  { "en": "Apa Itu Arus Eddy?", "id": "Arus Putar Di Inti Besi." },
  { "en": "Cara Mengurangi Arus Eddy Trafo?", "id": "Inti Besi Dibuat Berlapis." },
  { "en": "Apa Itu Histeresis Magnetik?", "id": "Keterlambatan Magnetisasi Bahan." },
  { "en": "Apa Itu Superkonduktor?", "id": "Hambatan Nol Suhu Rendah." },
  { "en": "Apa Kepanjangan EMI Gangguan?", "id": "Electromagnetic Interference." },
  { "en": "Apa Fungsi Ferite Bead Pada Kabel?", "id": "Redam Frekuensi Tinggi." },
  { "en": "Apa Fungsi Buck Converter DC Ke DC?", "id": "Turunkan Tegangan DC." },
  { "en": "Apa Fungsi Boost Converter DC Ke DC?", "id": "Naikkan Tegangan DC." },
  { "en": "Apa Fungsi Buck Boost Converter?", "id": "Naik Atau Turun Tegangan." },
  { "en": "Apa Metode Analisis Mesh Pada Rangkaian?", "id": "Hukum KVL Pada Loop." },
  { "en": "Apa Metode Analisis Nodal Pada Rangkaian?", "id": "Hukum KCL Pada Simpul." },
  { "en": "Apa Bunyi Teorema Millman Rangkaian?", "id": "Sederhanakan Sumber Paralel." },
  { "en": "Apa Syarat Nyquist Sampling Rate?", "id": "Dua Kali Frekuensi Sinyal." },
  { "en": "Apa Itu Aliasing Pada Sinyal Digital?", "id": "Cacat Sinyal Sampling Rendah." },
  { "en": "Apa Fungsi Fourier Series Matematika?", "id": "Sinyal Periodik Ke Sinus." },
  { "en": "Apa Fungsi Fourier Transform Sinyal?", "id": "Domain Waktu Ke Frekuensi." },
  { "en": "Apa Kepanjangan FFT Dalam Sinyal?", "id": "Fast Fourier Transform." },
  { "en": "Apa Kepanjangan AM Pada Radio?", "id": "Amplitude Modulation." },
  { "en": "Apa Kepanjangan FM Pada Radio?", "id": "Frequency Modulation." },
  { "en": "Apa Kepanjangan PM Pada Modulasi?", "id": "Phase Modulation." },
  { "en": "Berapa Bandwidth Sinyal AM Standar?", "id": "Dua Kali Frekuensi Modulasi." },
  { "en": "Apa Keunggulan Single Sideband (SSB)?", "id": "Hemat Bandwidth Dan Daya." },
  { "en": "Apa Kepanjangan ASK Modulasi Digital?", "id": "Amplitude Shift Keying." },
  { "en": "Apa Kepanjangan FSK Modulasi Digital?", "id": "Frequency Shift Keying." },
  { "en": "Apa Kepanjangan PSK Modulasi Digital?", "id": "Phase Shift Keying." },
  { "en": "Apa Kepanjangan QAM Modulasi Digital?", "id": "Quadrature Amplitude Modulation." },
  { "en": "Apa Fungsi Komponen Optocoupler?", "id": "Isolasi Listrik Optik." },
  { "en": "Apa Fungsi Varistor (VDR) Komponen?", "id": "Proteksi Tegangan Lebih." },
  { "en": "Apa Karakteristik Thermistor NTC?", "id": "Suhu Naik Hambatan Turun." },
  { "en": "Apa Karakteristik Thermistor PTC?", "id": "Suhu Naik Hambatan Naik." },
  { "en": "Apa Fungsi Photodiode Sensor?", "id": "Ubah Cahaya Jadi Arus." },
  { "en": "Apa Kelebihan Phototransistor Dibanding Photodiode?", "id": "Penguatan Arus Lebih Besar." },
  { "en": "Apa Fungsi Rangkaian Schmitt Trigger?", "id": "Bentuk Gelombang Kotak Tegas." },
  { "en": "Apa Itu Multivibrator Astabil?", "id": "Osilator Gelombang Kotak." },
  { "en": "Apa Itu Multivibrator Monostabil?", "id": "Satu Keadaan Stabil." },
  { "en": "Apa Itu Multivibrator Bistabil?", "id": "Dua Keadaan Stabil." },
  { "en": "Apa Kepanjangan FPGA Chip Digital?", "id": "Field Programmable Gate Array." },
  { "en": "Apa Kepanjangan ASIC Chip Khusus?", "id": "Application Specific Integrated Circuit." },
  { "en": "Apa Kepanjangan VHDL Bahasa Pemrograman?", "id": "VHSIC Hardware Description Language." },
  { "en": "Apa Fungsi Half Adder Digital?", "id": "Penjumlah Dua Bit." },
  { "en": "Apa Fungsi Full Adder Digital?", "id": "Penjumlah Tiga Bit Carry." },
  { "en": "Apa Fungsi Decoder Digital?", "id": "Kode Biner Ke Output." },
  { "en": "Apa Fungsi Encoder Digital?", "id": "Input Ke Kode Biner." },
  { "en": "Apa Fungsi Priority Encoder?", "id": "Input Prioritas Tertinggi." },
  { "en": "Apa Fungsi Shift Register?", "id": "Geser Bit Data." },
  { "en": "Apa Fungsi Counter Digital?", "id": "Pencacah Pulsa Input." },
  { "en": "Apa Itu Synchronous Counter?", "id": "Clock Serentak." },
  { "en": "Apa Itu Asynchronous Counter (Ripple)?", "id": "Clock Berurutan." },
  { "en": "Apa Kepanjangan LVDT Sensor Posisi?", "id": "Linear Variable Differential Transformer." },
  { "en": "Apa Fungsi Strain Gauge Sensor?", "id": "Ukur Regangan Mekanis." },
  { "en": "Apa Efek Piezoelectric Pada Kristal?", "id": "Tekanan Hasilkan Tegangan." },
  { "en": "Apa Fungsi Proximity Sensor Induktif?", "id": "Deteksi Logam Tanpa Sentuh." },
  { "en": "Apa Fungsi Proximity Sensor Kapasitif?", "id": "Deteksi Benda Non Logam." },
  { "en": "Apa Prinsip Ultrasonic Sensor?", "id": "Pantulan Gelombang Suara." },
  { "en": "Apa Kepanjangan RTD Sensor Suhu?", "id": "Resistance Temperature Detector." },
  { "en": "Apa Bahan Dasar Sensor Pt100?", "id": "Platinum." },
  { "en": "Apa Kepanjangan MPPT Panel Surya?", "id": "Maximum Power Point Tracking." },
  { "en": "Apa Fungsi Utama MPPT Surya?", "id": "Optimalkan Daya Output." },
  { "en": "Apa Kepanjangan Baterai NiCd?", "id": "Nickel Cadmium." },
  { "en": "Apa Kepanjangan Baterai NiMH?", "id": "Nickel Metal Hydride." },
  { "en": "Apa Kepanjangan Baterai LiFePO4?", "id": "Lithium Iron Phosphate." },
  { "en": "Apa Keunggulan Baterai LiFePO4?", "id": "Aman Dan Siklus Panjang." },
  { "en": "Apa Itu Depth Of Discharge (DOD)?", "id": "Persentase Pengurasan Baterai." },
  { "en": "Apa Itu State Of Charge (SOC)?", "id": "Persentase Sisa Energi." },
  { "en": "Apa Fungsi Busbar Pada Panel?", "id": "Rel Konduktor Utama." },
  { "en": "Apa Fungsi Switchgear Tegangan Menengah?", "id": "Distribusi Dan Proteksi Daya." },
  { "en": "Apa Kepanjangan ACB Pemutus Sirkuit?", "id": "Air Circuit Breaker." },
  { "en": "Apa Kepanjangan VCB Pemutus Sirkuit?", "id": "Vacuum Circuit Breaker." },
  { "en": "Apa Kepanjangan OCB Pemutus Sirkuit?", "id": "Oil Circuit Breaker." },
  { "en": "Apa Gas Isolator Pada GIS?", "id": "Sulfur Hexafluoride." },
  { "en": "Apa Rumus Kimia Gas SF6?", "id": "SF6." },
  { "en": "Apa Fungsi Trafo Distribusi?", "id": "Tegangan Menengah Ke Rendah." },
  { "en": "Apa Itu Gardu Distribusi Tipe Tiang?", "id": "Trafo Di Atas Tiang." },
  { "en": "Apa Itu Gardu Distribusi Tipe Beton?", "id": "Trafo Di Dalam Bangunan." },
  { "en": "Apa Karakteristik Kabel NYA?", "id": "Tembaga Tunggal Isolasi PVC." },
  { "en": "Apa Karakteristik Kabel NYM?", "id": "Selubung PVC Putih." },
  { "en": "Apa Karakteristik Kabel NYY?", "id": "Kabel Tanah Hitam." },
  { "en": "Apa Karakteristik Kabel NYFGbY?", "id": "Armour Baja Tanam Langsung." },
  { "en": "Apa Kepanjangan XLPE Isolasi Kabel?", "id": "Cross Linked Polyethylene." },
  { "en": "Apa Fungsi Kabel Twisted Pair?", "id": "Kurangi Interferensi Sinyal." },
  { "en": "Apa Fungsi Kabel Coaxial RF?", "id": "Transmisi Frekuensi Tinggi." },
  { "en": "Apa Media Transmisi Fiber Optik?", "id": "Kaca Atau Plastik." },
  { "en": "Apa Prinsip Dasar Fiber Optik?", "id": "Refleksi Internal Total." },
  { "en": "Apa Ciri Single Mode Fiber?", "id": "Inti Kecil Jarak Jauh." },
  { "en": "Apa Ciri Multi Mode Fiber?", "id": "Inti Besar Jarak Dekat." },
  { "en": "Apa Istilah Sambungan Fiber Optik?", "id": "Splicing." },
  { "en": "Apa Kepanjangan OTDR Alat Ukur?", "id": "Optical Time Domain Reflectometer." },
  { "en": "Apa Itu Attenuation Pada Kabel?", "id": "Peredaman Sinyal." },
  { "en": "Apa Satuan Gain Atau Redaman?", "id": "Desibel." },
  { "en": "Apa Kepanjangan SNR Sinyal?", "id": "Signal To Noise Ratio." },
  { "en": "Apa Rumus Menghitung SNR Sinyal?", "id": "Daya Sinyal Bagi Noise." },
  { "en": "Apa Penyebab Thermal Noise?", "id": "Gerakan Elektron Acak." },
  { "en": "Apa Penyebab Shot Noise?", "id": "Sifat Diskrit Muatan." },
  { "en": "Apa Itu Waveguide Gelombang Mikro?", "id": "Pipa Pemandu Gelombang." },
  { "en": "Apa Pola Radiasi Antena Dipol?", "id": "Angka Delapan." },
  { "en": "Apa Fungsi Antena Yagi Uda?", "id": "Antena Pengarah TV." },
  { "en": "Apa Definisi Gain Pada Antena?", "id": "Penguatan Sinyal Arah Tertentu." },
  { "en": "Apa Itu Polarisasi Antena?", "id": "Orientasi Medan Listrik." },
  { "en": "Apa Kepanjangan VSWR Transmisi?", "id": "Voltage Standing Wave Ratio." },
  { "en": "Berapa Nilai VSWR Paling Ideal?", "id": "Satu Banding Satu." },
  { "en": "Apa Fungsi Smith Chart RF?", "id": "Plot Impedansi Transmisi." },
  { "en": "Berapa Impedansi Standar Kabel Coaxial?", "id": "50 Ohm." },
  { "en": "Apa Fungsi Balun Pada Antena?", "id": "Balance To Unbalance." },
  { "en": "Apa Kepanjangan Radar Sistem?", "id": "Radio Detection And Ranging." },
  { "en": "Apa Prinsip Efek Doppler Radar?", "id": "Pergeseran Frekuensi Akibat Gerak." },
  { "en": "Apa Kriteria Kestabilan Routh Hurwitz?", "id": "Analisis Akar Persamaan Karakteristik." },
  { "en": "Apa Definisi Gain Margin Sistem Kontrol?", "id": "Batas Penguatan Sebelum Tidak Stabil." },
  { "en": "Apa Definisi Phase Margin Sistem Kontrol?", "id": "Batas Fasa Sebelum Tidak Stabil." },
  { "en": "Apa Fungsi Lead Compensator Kontrol?", "id": "Perbaiki Respon Transien." },
  { "en": "Apa Fungsi Lag Compensator Kontrol?", "id": "Kurangi Error Steady State." },
  { "en": "Apa Representasi State Space Control?", "id": "Model Matriks Domain Waktu." },
  { "en": "Apa Itu Matriks Transisi Keadaan?", "id": "Solusi Persamaan State Space." },
  { "en": "Apa Sifat Sistem Linear Time Invariant (LTI)?", "id": "Sifat Tetap Terhadap Waktu." },
  { "en": "Apa Fungsi Relay Diferensial Pada Trafo?", "id": "Bandingkan Arus Masuk Keluar." },
  { "en": "Apa Prinsip Kerja Relay Jarak (Distance)?", "id": "Ukur Impedansi Ke Titik Gangguan." },
  { "en": "Apa Fungsi Relay Arus Lebih (Overcurrent)?", "id": "Trip Jika Arus Melebihi Set." },
  { "en": "Apa Fungsi Relay Tegangan Kurang (Undervoltage)?", "id": "Trip Jika Tegangan Drop." },
  { "en": "Apa Fungsi Reverse Power Relay Generator?", "id": "Cegah Generator Jadi Motor." },
  { "en": "Apa Itu Zona Proteksi Sistem Tenaga?", "id": "Area Cakupan Satu Relay." },
  { "en": "Apa Sifat Koordinasi Proteksi Yang Baik?", "id": "Selektif Dan Cepat." },
  { "en": "Apa Itu Tegangan Langkah Atau Step Voltage?", "id": "Beda Potensial Dua Kaki." },
  { "en": "Apa Itu Tegangan Sentuh Atau Touch Voltage?", "id": "Beda Potensial Tangan Kaki." },
  { "en": "Apa Hukum Paschen Tegangan Tinggi?", "id": "Tegangan Tembus Fungsi Tekanan Jarak." },
  { "en": "Apa Itu Partial Discharge Isolasi?", "id": "Pelutan Listrik Parsial." },
  { "en": "Apa Alat Uji Tahanan Isolasi Kabel?", "id": "Megger Atau Insulation Tester." },
  { "en": "Apa Tegangan Uji Megger Standar?", "id": "500 Volt Atau 1000 Volt." },
  { "en": "Apa Fungsi Sela Bola (Sphere Gap)?", "id": "Ukur Tegangan Puncak Tinggi." },
  { "en": "Apa Itu Fenomena Treeing Pada Kabel?", "id": "Jalur Konduktif Cabang Pohon." },
  { "en": "Apa Fungsi Impuls Generator Tegangan Tinggi?", "id": "Simulasi Sambaran Petir." },
  { "en": "Apa Bentuk Gelombang Impuls Standar Petir?", "id": "1.2 Per 50 Mikro Detik." },
  { "en": "Apa Fungsi Watchdog Timer Mikrokontroler?", "id": "Reset Sistem Jika Macet." },
  { "en": "Apa Itu Interrupt Vector Table?", "id": "Alamat Memori Layanan Interupsi." },
  { "en": "Apa Kepanjangan DMA Pada Komputer?", "id": "Direct Memory Access." },
  { "en": "Apa Keuntungan Menggunakan DMA?", "id": "Akses Memori Tanpa CPU." },
  { "en": "Apa Arsitektur Von Neumann?", "id": "Bus Instruksi Data Gabung." },
  { "en": "Apa Arsitektur Harvard?", "id": "Bus Instruksi Data Pisah." },
  { "en": "Apa Kepanjangan RISC Arsitektur Prosesor?", "id": "Reduced Instruction Set Computer." },
  { "en": "Apa Kepanjangan CISC Arsitektur Prosesor?", "id": "Complex Instruction Set Computer." },
  { "en": "Apa Fungsi Pipelining Pada Prosesor?", "id": "Eksekusi Instruksi Paralel." },
  { "en": "Apa Itu Cache Memory?", "id": "Memori Cepat Dekat CPU." },
  { "en": "Apa Fungsi Stack Pointer (SP)?", "id": "Penunjuk Alamat Tumpukan Data." },
  { "en": "Apa Fungsi Program Counter (PC)?", "id": "Penunjuk Alamat Instruksi Berikutnya." },
  { "en": "Apa Kepanjangan ALU Dalam Prosesor?", "id": "Arithmetic Logic Unit." },
  { "en": "Apa Fungsi Flag Register Status?", "id": "Indikator Kondisi Operasi ALU." },
  { "en": "Apa Kepanjangan GPIO Mikrokontroler?", "id": "General Purpose Input Output." },
  { "en": "Apa Itu Pull Up Resistor?", "id": "Tarik Tegangan Ke VCC." },
  { "en": "Apa Itu Pull Down Resistor?", "id": "Tarik Tegangan Ke Ground." },
  { "en": "Apa Masalah Bouncing Pada Saklar Mekanik?", "id": "Sinyal Bergetar Saat Tekan." },
  { "en": "Cara Mengatasi Bouncing Saklar?", "id": "Debouncing Hardware Atau Software." },
  { "en": "Apa Fungsi Transformasi Z (Z-Transform)?", "id": "Analisis Sistem Waktu Diskrit." },
  { "en": "Apa Kepanjangan FIR Filter Digital?", "id": "Finite Impulse Response." },
  { "en": "Apa Kepanjangan IIR Filter Digital?", "id": "Infinite Impulse Response." },
  { "en": "Apa Kelebihan Filter FIR?", "id": "Fasa Linear Stabil." },
  { "en": "Apa Operasi Konvolusi Sinyal?", "id": "Gabungan Dua Fungsi Sinyal." },
  { "en": "Apa Itu Kuantisasi Pada ADC?", "id": "Pembulatan Nilai Analog." },
  { "en": "Apa Itu Quantization Error?", "id": "Selisih Asli Dan Digital." },
  { "en": "Apa Kepanjangan OSI Model Jaringan?", "id": "Open Systems Interconnection." },
  { "en": "Ada Berapa Layer Model OSI?", "id": "7 Layer." },
  { "en": "Apa Layer 1 Model OSI?", "id": "Physical Layer." },
  { "en": "Apa Layer 2 Model OSI?", "id": "Data Link Layer." },
  { "en": "Apa Layer 3 Model OSI?", "id": "Network Layer." },
  { "en": "Apa Satuan Data Pada Layer 1?", "id": "Bit." },
  { "en": "Apa Satuan Data Pada Layer 2?", "id": "Frame." },
  { "en": "Apa Satuan Data Pada Layer 3?", "id": "Packet." },
  { "en": "Apa Kepanjangan MAC Address?", "id": "Media Access Control." },
  { "en": "Berapa Panjang Bit MAC Address?", "id": "48 Bit." },
  { "en": "Apa Kepanjangan TCP Protokol?", "id": "Transmission Control Protocol." },
  { "en": "Apa Kepanjangan UDP Protokol?", "id": "User Datagram Protocol." },
  { "en": "Apa Bedanya TCP Dan UDP?", "id": "TCP Koneksi, UDP Tidak." },
  { "en": "Apa Fungsi Subnet Mask Jaringan?", "id": "Pisahkan Network Dan Host." },
  { "en": "Apa Itu Topologi Star Jaringan?", "id": "Semua Terhubung Satu Pusat." },
  { "en": "Apa Itu Topologi Mesh Jaringan?", "id": "Semua Saling Terhubung Langsung." },
  { "en": "Apa Fungsi Router Jaringan?", "id": "Hubungkan Network Berbeda." },
  { "en": "Apa Fungsi Switch Jaringan?", "id": "Hubungkan Perangkat Satu Network." },
  { "en": "Apa Osilator Hartley Menggunakan Apa?", "id": "Dua Induktor Satu Kapasitor." },
  { "en": "Apa Osilator Colpitts Menggunakan Apa?", "id": "Dua Kapasitor Satu Induktor." },
  { "en": "Apa Kelebihan Osilator Kristal?", "id": "Stabilitas Frekuensi Tinggi." },
  { "en": "Apa Fungsi Rangkaian Current Mirror?", "id": "Salin Arus Referensi." },
  { "en": "Apa Fungsi Pasangan Darlington Transistor?", "id": "Penguatan Arus Sangat Tinggi." },
  { "en": "Apa Rumus Penguatan Darlington Beta?", "id": "Beta Satu Kali Beta Dua." },
  { "en": "Apa Itu Feedback Negatif Amplifier?", "id": "Umpan Balik Kurangi Input." },
  { "en": "Apa Manfaat Feedback Negatif?", "id": "Stabilkan Gain Dan Bandwidth." },
  { "en": "Apa Itu Feedback Positif Amplifier?", "id": "Umpan Balik Tambah Input." },
  { "en": "Apa Aplikasi Feedback Positif?", "id": "Rangkaian Osilator." },
  { "en": "Apa Kepanjangan ECG Biomedis?", "id": "Electrocardiogram." },
  { "en": "Apa Yang Diukur Oleh ECG?", "id": "Aktivitas Listrik Jantung." },
  { "en": "Apa Kepanjangan EEG Biomedis?", "id": "Electroencephalogram." },
  { "en": "Apa Yang Diukur Oleh EEG?", "id": "Aktivitas Listrik Otak." },
  { "en": "Apa Kepanjangan EMG Biomedis?", "id": "Electromyogram." },
  { "en": "Apa Yang Diukur Oleh EMG?", "id": "Aktivitas Listrik Otot." },
  { "en": "Apa Itu Instrumentation Amplifier?", "id": "Penguat Beda Presisi Tinggi." },
  { "en": "Apa Resistor Toleransi Emas?", "id": "5 Persen." },
  { "en": "Apa Resistor Toleransi Perak?", "id": "10 Persen." },
  { "en": "Apa Kode Warna Resistor 1K Ohm?", "id": "Cokelat Hitam Merah." },
  { "en": "Apa Kode Warna Resistor 10K Ohm?", "id": "Cokelat Hitam Oranye." },
  { "en": "Apa Kode Warna Resistor 100 Ohm?", "id": "Cokelat Hitam Cokelat." },
  { "en": "Apa Kode Warna Resistor 220 Ohm?", "id": "Merah Merah Cokelat." },
  { "en": "Apa Kode Warna Resistor 4k7 Ohm?", "id": "Kuning Ungu Merah." },
  { "en": "Apa Komponen TRIAC Digunakan Untuk?", "id": "Dimmer Lampu AC." },
  { "en": "Apa Fungsi Snubber Circuit?", "id": "Lindungi Saklar Lonjakan Tegangan." },
  { "en": "Apa Itu Galvanic Isolation?", "id": "Pemisahan Aliran Listrik Langsung." },
  { "en": "Apa Itu Ground Loop Noise?", "id": "Arus Putar Antar Ground." },
  { "en": "Cara Mengatasi Ground Loop?", "id": "Satu Titik Grounding." },
  { "en": "Apa Definisi Energy Band Gap?", "id": "Celah Pita Valensi Konduksi." },
  { "en": "Apa Itu Tegangan Barrier Pada Silikon?", "id": "0.7 Volt." },
  { "en": "Apa Itu Tegangan Barrier Pada Germanium?", "id": "0.3 Volt." },
  { "en": "Apa Fungsi Doping Pada Semikonduktor?", "id": "Tambah Pembawa Muatan." },
  { "en": "Apa Itu Depletion Region Sambungan PN?", "id": "Daerah Kosong Pembawa Muatan." },
  { "en": "Apa Itu Reverse Bias Pada Dioda?", "id": "Prategangan Mundur Tahan Arus." },
  { "en": "Apa Itu Forward Bias Pada Dioda?", "id": "Prategangan Maju Alirkan Arus." },
  { "en": "Apa Rumus Konstanta Waktu Rangkaian RC?", "id": "Tau = R x C." },
  { "en": "Apa Rumus Konstanta Waktu Rangkaian RL?", "id": "Tau = L / R." },
  { "en": "Berapa Persen Pengisian Kapasitor Satu Tau?", "id": "63.2 Persen." },
  { "en": "Berapa Persen Pengosongan Kapasitor Satu Tau?", "id": "36.8 Persen." },
  { "en": "Apa Faktor Kualitas (Q) Rangkaian Resonansi?", "id": "Selektivitas Frekuensi Rangkaian." },
  { "en": "Apa Syarat Kriteria Barkhausen Osilator?", "id": "Gain Loop Satu Fasa Nol." },
  { "en": "Apa Kepanjangan BTS Dalam Telekomunikasi?", "id": "Base Transceiver Station." },
  { "en": "Apa Fungsi Handover Pada Seluler?", "id": "Pindah Koneksi Antar BTS." },
  { "en": "Apa Kepanjangan SIM Card Telepon?", "id": "Subscriber Identity Module." },
  { "en": "Apa Kepanjangan IMEI Identitas Perangkat?", "id": "International Mobile Equipment Identity." },
  { "en": "Apa Kepanjangan LTE Jaringan Seluler?", "id": "Long Term Evolution." },
  { "en": "Apa Teknologi Multiplexing 4G LTE?", "id": "OFDMA." },
  { "en": "Apa Kepanjangan MIMO Teknologi Antena?", "id": "Multiple Input Multiple Output." },
  { "en": "Apa Frekuensi Rentang VHF?", "id": "30 Sampai 300 MHz." },
  { "en": "Apa Frekuensi Rentang UHF?", "id": "300 Sampai 3000 MHz." },
  { "en": "Apa Itu Line Of Sight (LOS)?", "id": "Pandangan Lurus Tanpa Halangan." },
  { "en": "Apa Itu Ground Wave Propagation?", "id": "Rambat Ikuti Permukaan Bumi." },
  { "en": "Apa Itu Sky Wave Propagation?", "id": "Pantulan Lapisan Ionosfer." },
  { "en": "Apa Definisi Harmonisa Sistem Tenaga?", "id": "Gelombang Cacat Kelipatan Frekuensi." },
  { "en": "Apa Penyebab Utama Munculnya Harmonisa?", "id": "Beban Non Linear." },
  { "en": "Apa Kepanjangan THD Kualitas Daya?", "id": "Total Harmonic Distortion." },
  { "en": "Apa Itu Voltage Sag Kualitas Daya?", "id": "Penurunan Tegangan Sesaat." },
  { "en": "Apa Itu Voltage Swell Kualitas Daya?", "id": "Kenaikan Tegangan Sesaat." },
  { "en": "Apa Itu Flicker Pada Lampu?", "id": "Kedipan Akibat Fluktuasi Tegangan." },
  { "en": "Apa Efek Ferranti Pada Transmisi?", "id": "Tegangan Ujung Terima Tinggi." },
  { "en": "Apa Itu Kawat Guard Wire Transmisi?", "id": "Kawat Pelindung Petir Teratas." },
  { "en": "Apa Fungsi Damper Pada Kabel SUTET?", "id": "Redam Getaran Angin." },
  { "en": "Apa Kepanjangan SCADA Pengendali Industri?", "id": "Supervisory Control And Data Acquisition." },
  { "en": "Apa Itu RTU Dalam Sistem SCADA?", "id": "Remote Terminal Unit." },
  { "en": "Apa Protokol Komunikasi Standar SCADA?", "id": "Modbus Atau DNP3." },
  { "en": "Apa Fungsi Jembatan Maxwell?", "id": "Ukur Induktansi Kumparan." },
  { "en": "Apa Fungsi Jembatan Schering?", "id": "Ukur Kapasitansi Dan Rugi." },
  { "en": "Apa Fungsi Jembatan Kelvin?", "id": "Ukur Resistansi Sangat Rendah." },
  { "en": "Apa Definisi Akurasi Alat Ukur?", "id": "Kedekatan Dengan Nilai Sebenarnya." },
  { "en": "Apa Definisi Presisi Alat Ukur?", "id": "Konsistensi Hasil Pengukuran." },
  { "en": "Apa Itu Resolusi Alat Ukur?", "id": "Perubahan Terkecil Yang Terdeteksi." },
  { "en": "Apa Kepanjangan DMM Alat Ukur?", "id": "Digital Multimeter." },
  { "en": "Apa Itu Parallax Error Pengukuran?", "id": "Kesalahan Sudut Pandang Mata." },
  { "en": "Apa Kepanjangan AGV Robotika?", "id": "Automated Guided Vehicle." },
  { "en": "Apa Definisi Robot Manipulator?", "id": "Lengan Robot Industri." },
  { "en": "Apa Itu End Effector Robot?", "id": "Alat Ujung Lengan Robot." },
  { "en": "Apa Definisi Degrees Of Freedom (DOF)?", "id": "Jumlah Sumbu Gerak Bebas." },
  { "en": "Apa Fungsi Aktuator Pada Robot?", "id": "Penggerak Mekanik Robot." },
  { "en": "Apa Sensor IMU Mengukur Apa?", "id": "Orientasi Dan Percepatan." },
  { "en": "Apa Kepanjangan IMU Sensor Robot?", "id": "Inertial Measurement Unit." },
  { "en": "Apa Fungsi Gyroscope Sensor?", "id": "Ukur Kecepatan Sudut." },
  { "en": "Apa Fungsi Accelerometer Sensor?", "id": "Ukur Percepatan Linear." },
  { "en": "Apa Itu Motor Stepper?", "id": "Gerak Langkah Per Langkah." },
  { "en": "Apa Teknik Microstepping Motor Stepper?", "id": "Perhalus Langkah Gerakan Motor." },
  { "en": "Apa Kelebihan Motor BLDC?", "id": "Efisien Tanpa Sikat Arang." },
  { "en": "Apa Kepanjangan BLDC Motor?", "id": "Brushless Direct Current." },
  { "en": "Apa Itu Motor Servo?", "id": "Motor Dengan Umpan Balik." },
  { "en": "Apa Fungsi Encoder Pada Motor?", "id": "Sensor Posisi Poros." },
  { "en": "Apa Kepanjangan BMS Kendaraan Listrik?", "id": "Battery Management System." },
  { "en": "Apa Fungsi Cell Balancing BMS?", "id": "Samakan Tegangan Tiap Sel." },
  { "en": "Apa Itu Regenerative Braking EV?", "id": "Pengereman Hasilkan Listrik." },
  { "en": "Apa Itu Thermal Runaway Baterai?", "id": "Kenaikan Suhu Tak Terkendali." },
  { "en": "Apa Satuan Energi Baterai EV?", "id": "Kilowatt Hour." },
  { "en": "Apa Kepanjangan BEV Kendaraan?", "id": "Battery Electric Vehicle." },
  { "en": "Apa Kepanjangan PHEV Kendaraan?", "id": "Plug In Hybrid EV." },
  { "en": "Apa Kepanjangan ICE Kendaraan Konvensional?", "id": "Internal Combustion Engine." },
  { "en": "Apa Konektor Charging Standar Eropa?", "id": "Type 2 Mennekes." },
  { "en": "Apa Konektor Charging Cepat DC?", "id": "CCS Atau CHAdeMO." },
  { "en": "Apa Itu Bootloader Mikrokontroler?", "id": "Program Awal Saat Start." },
  { "en": "Apa Resolusi ADC 10 Bit?", "id": "1024 Tingkat." },
  { "en": "Apa Resolusi ADC 12 Bit?", "id": "4096 Tingkat." },
  { "en": "Apa Tegangan Logika TTL High?", "id": "5 Volt." },
  { "en": "Apa Tegangan Logika CMOS 3.3V High?", "id": "3.3 Volt." },
  { "en": "Apa Tegangan Logika RS232?", "id": "Positif Negatif 12 Volt." },
  { "en": "Apa Fungsi Real Time Clock (RTC)?", "id": "Penghitung Waktu Nyata." },
  { "en": "Apa Protokol Komunikasi Satu Kabel?", "id": "One Wire Protocol." },
  { "en": "Apa Itu Brown Out Reset?", "id": "Reset Saat Tegangan Drop." },
  { "en": "Apa Fungsi Fuse Bit Mikrokontroler?", "id": "Konfigurasi Hardware Level Rendah." },
  { "en": "Apa Itu Cross Talk Kabel?", "id": "Induksi Sinyal Kabel Sebelah." },
  { "en": "Apa Kepanjangan PCB Multilayer?", "id": "Papan Sirkuit Banyak Lapis." },
  { "en": "Apa Itu Via Pada PCB?", "id": "Lubang Penghubung Antar Layer." },
  { "en": "Apa Fungsi Solder Mask PCB?", "id": "Pelindung Jalur Tembaga." },
  { "en": "Apa Fungsi Silkscreen PCB?", "id": "Cetak Label Komponen." },
  { "en": "Apa Bahan Substrat PCB Standar?", "id": "FR4." },
  { "en": "Apa Teknik Soldering Reflow?", "id": "Pemanasan Oven Pasta Solder." },
  { "en": "Apa Teknik Soldering Wave?", "id": "Gelombang Timah Cair." },
  { "en": "Apa Kepanjangan SMD Komponen?", "id": "Surface Mount Device." },
  { "en": "Apa Kepanjangan THT Komponen?", "id": "Through Hole Technology." },
  { "en": "Apa Ukuran Resistor SMD 0805?", "id": "0.08 Kali 0.05 Inci." },
  { "en": "Apa Fungsi Heatsink Elektronika?", "id": "Buang Panas Komponen." },
  { "en": "Apa Itu Thermal Paste?", "id": "Isi Celah Kontak Panas." },
  { "en": "Apa Satuan Resistansi Termal?", "id": "Derajat Celcius Per Watt." },
  { "en": "Apa Hukum Kekekalan Energi?", "id": "Energi Tidak Bisa Musnah." },
  { "en": "Apa Efek Photovoltaic?", "id": "Cahaya Hasilkan Listrik." },
  { "en": "Apa Efek Seebeck Termoelektrik?", "id": "Beda Suhu Hasilkan Tegangan." },
  { "en": "Apa Efek Peltier Termoelektrik?", "id": "Arus Hasilkan Beda Suhu." },
  { "en": "Apa Itu Superkapasitor?", "id": "Kapasitor Kapasitas Sangat Besar." },
  { "en": "Apa Fungsi Slack Bus Pada Aliran Daya?", "id": "Penyeimbang Daya Aktif Reaktif." },
  { "en": "Apa Variabel Yang Diketahui Pada PV Bus?", "id": "Daya Aktif Dan Tegangan." },
  { "en": "Apa Variabel Yang Diketahui Pada PQ Bus?", "id": "Daya Aktif Dan Reaktif." },
  { "en": "Apa Metode Aliran Daya Paling Cepat Konvergen?", "id": "Newton Raphson." },
  { "en": "Apa Fungsi Matriks Jacobian Pada Load Flow?", "id": "Linearisasi Persamaan Non Linear." },
  { "en": "Apa Definisi Sistem Per Unit (PU)?", "id": "Nilai Sebenarnya Bagi Nilai Dasar." },
  { "en": "Apa Keuntungan Menggunakan Sistem Per Unit?", "id": "Sederhanakan Perhitungan Trafo." },
  { "en": "Apa Itu Breaking Capacity Circuit Breaker?", "id": "Arus Putus Maksimum Tanpa Rusak." },
  { "en": "Apa Itu Making Capacity Circuit Breaker?", "id": "Arus Sambung Maksimum Saat Gangguan." },
  { "en": "Apa Kepanjangan BIL Pada Isolasi?", "id": "Basic Impulse Insulation Level." },
  { "en": "Apa Fungsi Arrester Kelas Stasiun?", "id": "Proteksi Gardu Induk Utama." },
  { "en": "Apa Itu Skin Depth Pada RF?", "id": "Kedalaman Penetrasi Arus Permukaan." },
  { "en": "Apa Parameter S11 Pada Network Analyzer?", "id": "Koefisien Refleksi Input." },
  { "en": "Apa Parameter S21 Pada Network Analyzer?", "id": "Koefisien Transmisi Maju." },
  { "en": "Apa Arti Nilai Return Loss Tinggi?", "id": "Sedikit Sinyal Yang Memantul." },
  { "en": "Apa Arti Nilai Insertion Loss Rendah?", "id": "Sedikit Sinyal Yang Hilang." },
  { "en": "Apa Impedansi Titik Tengah Smith Chart?", "id": "Satu Ohm Ternormalisasi." },
  { "en": "Apa Fungsi Stub Tuner Pada Transmisi?", "id": "Penyesuai Impedansi." },
  { "en": "Apa Itu Intermodulation Distortion (IMD)?", "id": "Muncul Frekuensi Baru Akibat Nonlinier." },
  { "en": "Apa Itu Noise Figure (NF)?", "id": "Degradasi SNR Oleh Komponen." },
  { "en": "Apa Kepanjangan PCB Jenis FR4?", "id": "Flame Retardant 4." },
  { "en": "Apa Fungsi Conformal Coating Pada PCB?", "id": "Pelindung Kelembaban Dan Debu." },
  { "en": "Apa Fungsi Flux Pada Proses Solder?", "id": "Bersihkan Oksidasi Logam." },
  { "en": "Apa Masalah Cold Solder Joint?", "id": "Sambungan Timah Retak Atau Kusam." },
  { "en": "Apa Standar IPC Untuk Perakitan Elektronik?", "id": "IPC A 610." },
  { "en": "Apa Itu Blind Via Pada PCB?", "id": "Lubang Layer Luar Ke Dalam." },
  { "en": "Apa Itu Buried Via Pada PCB?", "id": "Lubang Antar Layer Dalam Saja." },
  { "en": "Apa Fungsi Ground Plane Pada PCB?", "id": "Referensi Tegangan Dan Shielding." },
  { "en": "Apa Itu Differential Pair Routing?", "id": "Dua Jalur Sinyal Saling Balik." },
  { "en": "Apa Tujuan Meandering Jalur PCB?", "id": "Samakan Panjang Jalur Waktu." },
  { "en": "Apa Itu Kalman Filter?", "id": "Estimasi Keadaan Sistem Berau." },
  { "en": "Apa Syarat Sistem Controllability?", "id": "Bisa Dikendalikan Ke Keadaan Apapun." },
  { "en": "Apa Syarat Sistem Observability?", "id": "Keadaan Bisa Diestimasi Dari Output." },
  { "en": "Apa Fungsi Observer Pada Kontrol?", "id": "Estimasi State Yang Tidak Terukur." },
  { "en": "Apa Definisi Overshoot Respon Transien?", "id": "Lonjakan Melebihi Setpoint." },
  { "en": "Apa Definisi Settling Time?", "id": "Waktu Mencapai Keadaan Mantap." },
  { "en": "Apa Definisi Rise Time?", "id": "Waktu Naik 10 Ke 90 Persen." },
  { "en": "Apa Fungsi Lyapunov Stability Criterion?", "id": "Cek Stabilitas Tanpa Linearitas." },
  { "en": "Apa Itu Model Predictive Control (MPC)?", "id": "Kontrol Berbasis Prediksi Masa Depan." },
  { "en": "Apa Fungsi Anti Windup Pada PID?", "id": "Cegah Saturasi Aksi Integral." },
  { "en": "Apa Jendela Hamming Pada DSP?", "id": "Kurangi Kebocoran Spektral." },
  { "en": "Apa Fungsi Zero Padding FFT?", "id": "Perhalus Tampilan Spektrum Frekuensi." },
  { "en": "Apa Perbedaan DFT Dan DTFT?", "id": "DFT Diskrit, DTFT Kontinyu." },
  { "en": "Apa Itu Operasi Korelasi Sinyal?", "id": "Ukuran Kemiripan Dua Sinyal." },
  { "en": "Apa Itu Autokorelasi Sinyal?", "id": "Korelasi Dengan Dirinya Sendiri." },
  { "en": "Apa Fungsi Downsampling Sinyal?", "id": "Kurangi Laju Sampel." },
  { "en": "Apa Fungsi Upsampling Sinyal?", "id": "Tambah Laju Sampel Interpolasi." },
  { "en": "Apa Itu Fixed Point DSP?", "id": "Aritmatika Bilangan Bulat." },
  { "en": "Apa Itu Floating Point DSP?", "id": "Aritmatika Bilangan Pecahan Desimal." },
  { "en": "Apa Keunggulan Floating Point DSP?", "id": "Dinamika Range Lebih Luas." },
  { "en": "Apa Kepanjangan MMU Komputer?", "id": "Memory Management Unit." },
  { "en": "Apa Fungsi Virtual Memory?", "id": "Perluas RAM Dengan Disk." },
  { "en": "Apa Itu Page Fault?", "id": "Data Tidak Ditemukan Di RAM." },
  { "en": "Apa Masalah Pipeline Hazard?", "id": "Instruksi Terhambat Konflik Data." },
  { "en": "Apa Fungsi Branch Prediction?", "id": "Tebak Cabang Instruksi Berikutnya." },
  { "en": "Apa Itu Cache Miss?", "id": "Data Tidak Ada Di Cache." },
  { "en": "Apa Tingkatan Cache Tercepat?", "id": "L1 Cache." },
  { "en": "Apa Itu Multicore Processor?", "id": "Banyak Inti Satu Chip." },
  { "en": "Apa Definisi Thread Dalam Prosesor?", "id": "Unit Eksekusi Terkecil Program." },
  { "en": "Apa Kepanjangan SIMD Instruksi?", "id": "Single Instruction Multiple Data." },
  { "en": "Apa Standar IEEE 802.11?", "id": "Protokol Wireless LAN WiFi." },
  { "en": "Apa Standar IEEE 802.3?", "id": "Protokol Ethernet Kabel." },
  { "en": "Apa Standar IEEE 802.15.4?", "id": "Protokol Zigbee Low Power." },
  { "en": "Apa Frekuensi Kerja Bluetooth?", "id": "2.4 Giga Hertz." },
  { "en": "Apa Kepanjangan NFC?", "id": "Near Field Communication." },
  { "en": "Apa Jarak Jangkauan NFC?", "id": "Kurang Dari 4 Senti." },
  { "en": "Apa Standar IEC 61850?", "id": "Otomasi Gardu Induk." },
  { "en": "Apa Protokol Goose Pada IEC 61850?", "id": "Pesan Cepat Antar IED." },
  { "en": "Apa Kepanjangan IED Sistem Tenaga?", "id": "Intelligent Electronic Device." },
  { "en": "Apa Standar IP Rating IP67?", "id": "Tahan Debu Tahan Celup Air." },
  { "en": "Apa Standar IP Rating IP68?", "id": "Tahan Debu Tahan Rendam Dalam." },
  { "en": "Apa Batas Betz Limit Turbin Angin?", "id": "Efisiensi Maksimal 59.3 Persen." },
  { "en": "Apa Itu Tip Speed Ratio (TSR)?", "id": "Rasio Kecepatan Ujung Baling Angin." },
  { "en": "Apa Masalah Islanding Pada Inverter?", "id": "Tetap Nyala Saat Grid Mati." },
  { "en": "Apa Metode MPPT Perturb And Observe?", "id": "Ubah Tegangan Cek Daya." },
  { "en": "Apa Fungsi Geothermal Power Plant?", "id": "Manfaatkan Panas Bumi." },
  { "en": "Apa Tipe Turbin PLTA Head Tinggi?", "id": "Turbin Pelton." },
  { "en": "Apa Tipe Turbin PLTA Head Rendah?", "id": "Turbin Kaplan." },
  { "en": "Apa Kepanjangan OTEC Energi Laut?", "id": "Ocean Thermal Energy Conversion." },
  { "en": "Apa Komponen Utama Fuel Cell?", "id": "Anoda Katoda Elektrolit." },
  { "en": "Apa Bahan Bakar Hidrogen Fuel Cell?", "id": "Hidrogen Dan Oksigen." },
  { "en": "Apa Output Sampingan Hidrogen Fuel Cell?", "id": "Air Murni." },
  { "en": "Apa Definisi Smart Grid?", "id": "Jaringan Listrik Cerdas Dua Arah." },
  { "en": "Apa Kepanjangan AMI Smart Grid?", "id": "Advanced Metering Infrastructure." },
  { "en": "Apa Fungsi Smart Meter?", "id": "Kirim Data Konsumsi Real Time." },
  { "en": "Apa Itu Demand Response?", "id": "Konsumen Kurangi Beban Saat Puncak." },
  { "en": "Apa Itu V2G Pada Mobil Listrik?", "id": "Vehicle To Grid." },
  { "en": "Apa Manfaat Teknologi V2G?", "id": "Mobil Jadi Cadangan Listrik Grid." },
  { "en": "Apa Kepanjangan HVDC Transmisi?", "id": "High Voltage Direct Current." },
  { "en": "Apa Kelebihan HVDC Jarak Jauh?", "id": "Rugi Daya Lebih Kecil." },
  { "en": "Apa Efek Ferranti Pada DC?", "id": "Tidak Ada Efek Ferranti." },
  { "en": "Apa Fungsi Converter Station HVDC?", "id": "Ubah AC Ke DC Bolak Balik." },
  { "en": "Apa Definisi Machine Learning?", "id": "Komputer Belajar Dari Data." },
  { "en": "Apa Fungsi Neural Network?", "id": "Tiru Cara Kerja Otak Manusia." },
  { "en": "Apa Itu Internet Of Things (IoT)?", "id": "Benda Terkoneksi Internet." },
  { "en": "Apa Protokol IoT MQTT?", "id": "Message Queuing Telemetry Transport." },
  { "en": "Apa Sifat Protokol MQTT?", "id": "Ringan Hemat Bandwidth." },
  { "en": "Apa Struktur Data JSON?", "id": "Format Teks Pertukaran Data." },
  { "en": "Apa Kepanjangan API Software?", "id": "Application Programming Interface." },
  { "en": "Apa Fungsi Cloud Computing?", "id": "Komputasi Via Server Internet." },
  { "en": "Apa Bunyi Hukum Gauss Untuk Medan Listrik?", "id": "Fluks Listrik Sebanding Muatan." },
  { "en": "Apa Bunyi Hukum Gauss Untuk Medan Magnet?", "id": "Fluks Magnet Tertutup Nol." },
  { "en": "Apa Bunyi Hukum Ampere Maxwell?", "id": "Arus Dan Perubahan Fluks Hasilkan Magnet." },
  { "en": "Apa Itu Gelombang Transversal Elektromagnetik?", "id": "Getar Tegak Lurus Rambat." },
  { "en": "Apa Definisi Poynting Vector?", "id": "Arah Aliran Daya Gelombang." },
  { "en": "Apa Kepanjangan VLSI Desain Chip?", "id": "Very Large Scale Integration." },
  { "en": "Apa Itu Wafer Dalam Elektronika?", "id": "Piringan Dasar Semikonduktor." },
  { "en": "Apa Proses Photolithography Pada Chip?", "id": "Cetak Pola Sirkuit Cahaya." },
  { "en": "Apa Bunyi Moore's Law?", "id": "Transistor Ganda Tiap Dua Tahun." },
  { "en": "Apa Kepanjangan SoC Dalam Chip?", "id": "System On Chip." },
  { "en": "Apa Itu Dark Current Pada Sensor?", "id": "Arus Tanpa Cahaya Masuk." },
  { "en": "Apa Definisi Entropi Shannon?", "id": "Ukuran Ketidakpastian Informasi." },
  { "en": "Apa Rumus Kapasitas Kanal Shannon?", "id": "B Log Dua Satu Plus SNR." },
  { "en": "Apa Fungsi Hamming Code?", "id": "Koreksi Kesalahan Satu Bit." },
  { "en": "Apa Itu Parity Bit?", "id": "Deteksi Kesalahan Ganjil Genap." },
  { "en": "Apa Fungsi Cyclic Redundancy Check (CRC)?", "id": "Deteksi Error Blok Data." },
  { "en": "Apa Itu Deadbeat Control System?", "id": "Error Nol Waktu Terbatas." },
  { "en": "Apa Fungsi Extended Kalman Filter (EKF)?", "id": "Estimasi Sistem Non Linear." },
  { "en": "Apa Itu Robust Control?", "id": "Tahan Terhadap Ketidakpastian Model." },
  { "en": "Apa Definisi Critical Clearing Time?", "id": "Waktu Maksimum Putus Gangguan." },
  { "en": "Apa Fungsi Power System Stabilizer (PSS)?", "id": "Redam Osilasi Frekuensi Rendah." },
  { "en": "Apa Fungsi Recloser Pada Jaringan?", "id": "Sambung Ulang Otomatis." },
  { "en": "Apa Itu Load Shedding Frekuensi?", "id": "Lepas Beban Saat Frekuensi Turun." },
  { "en": "Apa Itu Ferroresonansi Pada Trafo?", "id": "Resonansi Non Linear Inti Besi." },
  { "en": "Apa Bahaya Ferroresonansi?", "id": "Tegangan Lebih Merusak Isolasi." },
  { "en": "Apa Itu Serangan Man In The Middle?", "id": "Peretas Sadap Jalur Tengah." },
  { "en": "Apa Kepanjangan DoS Serangan Siber?", "id": "Denial Of Service." },
  { "en": "Apa Fungsi Firewall Pada Jaringan OT?", "id": "Filter Paket Data Masuk." },
  { "en": "Apa Kepanjangan VPN Jaringan?", "id": "Virtual Private Network." },
  { "en": "Apa Prinsip Kerja MRI Scanner?", "id": "Resonansi Magnetik Inti Atom." },
  { "en": "Apa Fungsi Pulse Oximeter?", "id": "Ukur Saturasi Oksigen Darah." },
  { "en": "Apa Sifat Orbit Geostasioner (GEO)?", "id": "Posisi Tetap Terhadap Bumi." },
  { "en": "Apa Sifat Low Earth Orbit (LEO)?", "id": "Orbit Rendah Latensi Kecil." },
  { "en": "Apa Fungsi Transponder Satelit?", "id": "Terima Dan Pancar Ulang." },
  { "en": "Apa Itu Uplink Komunikasi Satelit?", "id": "Sinyal Bumi Ke Satelit." },
  { "en": "Apa Itu Downlink Komunikasi Satelit?", "id": "Sinyal Satelit Ke Bumi." },
  { "en": "Apa Fungsi Synthetic Aperture Radar (SAR)?", "id": "Pencitraan Radar Resolusi Tinggi." },
  { "en": "Apa Itu Pitch Control Turbin Angin?", "id": "Atur Sudut Bilah Baling." },
  { "en": "Apa Itu Yaw Control Turbin Angin?", "id": "Putar Nacelle Hadap Angin." },
  { "en": "Apa Itu Clock Skew Digital?", "id": "Beda Waktu Tiba Clock." },
  { "en": "Apa Itu Jitter Pada Sinyal Digital?", "id": "Variasi Waktu Transisi Sinyal." },
  { "en": "Apa Bahaya Metastability Flip Flop?", "id": "Output Tidak Terdefinisi." },
  { "en": "Apa Itu Race Condition Logika?", "id": "Output Tergantung Urutan Sinyal." },
  { "en": "Apa Fungsi Deoupling Capacitor?", "id": "Stabilkan Tegangan Suplai IC." },
  { "en": "Apa Itu Crosstalk Pada Kabel Data?", "id": "Induksi Sinyal Jalur Sebelah." },
  { "en": "Apa Impedansi Karakteristik Kabel UTP?", "id": "100 Ohm." },
  { "en": "Apa Impedansi Karakteristik Kabel USB?", "id": "90 Ohm." },
  { "en": "Apa Kepanjangan CAN Bus Otomotif?", "id": "Controller Area Network." },
  { "en": "Apa Fitur Utama CAN Bus?", "id": "Tahan Noise Prioritas Pesan." },
  { "en": "Apa Resistor Terminasi CAN Bus?", "id": "120 Ohm." },
  { "en": "Apa Itu Wireless Power Transfer?", "id": "Kirim Daya Tanpa Kabel." },
  { "en": "Apa Prinsip Wireless Charging Qi?", "id": "Induksi Resonansi Magnetik." },
  { "en": "Apa Kepanjangan RFID Identifikasi?", "id": "Radio Frequency Identification." },
  { "en": "Apa Bedanya RFID Pasif Dan Aktif?", "id": "Pasif Tanpa Baterai Internal." },
  { "en": "Apa Frekuensi RFID Kartu E-KTP?", "id": "13.56 Mega Hertz." },
  { "en": "Apa Kepanjangan GPS Navigasi?", "id": "Global Positioning System." },
  { "en": "Berapa Minimal Satelit Untuk GPS?", "id": "Empat Satelit." },
  { "en": "Apa Prinsip Trilaterasi GPS?", "id": "Ukur Jarak Tiga Titik." },
  { "en": "Apa Itu Piezoelektrik Material?", "id": "Tekanan Hasilkan Muatan Listrik." },
  { "en": "Apa Aplikasi Piezoelektrik Umum?", "id": "Pemantik Api Dan Speaker." },
  { "en": "Apa Itu Superkonduktor Suhu Tinggi?", "id": "Hambatan Nol Di Atas Nitrogen Cair." },
  { "en": "Apa Efek Meissner Superkonduktor?", "id": "Tolak Medan Magnet Luar." },
  { "en": "Apa Itu Memristor Komponen?", "id": "Resistor Dengan Memori." },
  { "en": "Apa Kepanjangan MEMS Teknologi?", "id": "Micro Electro Mechanical Systems." },
  { "en": "Apa Contoh Aplikasi Sensor MEMS?", "id": "Akselerometer HP Dan Airbag." },
  { "en": "Apa Itu Graphene Material?", "id": "Karbon Setebal Satu Atom." },
  { "en": "Apa Keunggulan Graphene?", "id": "Konduktivitas Sangat Tinggi." },
  { "en": "Apa Itu Quantum Dot?", "id": "Partikel Semikonduktor Ukuran Nano." },
  { "en": "Apa Aplikasi Quantum Dot LED?", "id": "Layar TV Warna Akurat." },
  { "en": "Apa Itu Tunnel Diode?", "id": "Dioda Efek Terowongan Kuantum." },
  { "en": "Apa Karakteristik Tunnel Diode?", "id": "Resistansi Negatif." },
  { "en": "Apa Itu Gunn Diode?", "id": "Osilator Gelombang Mikro." },
  { "en": "Apa Itu PIN Diode?", "id": "Dioda Switching Frekuensi Tinggi." },
  { "en": "Apa Kepanjangan LASER?", "id": "Light Amplification Stimulated Emission Radiation." },
  { "en": "Apa Sifat Cahaya Laser?", "id": "Koheren Dan Monokromatik." },
  { "en": "Apa Kepanjangan LIDAR?", "id": "Light Detection And Ranging." },
  { "en": "Apa Fungsi LIDAR Pada Mobil Otonom?", "id": "Peta 3D Lingkungan Sekitar." },
  { "en": "Apa Itu Biometrics Security?", "id": "Identifikasi Ciri Fisik Manusia." },
  { "en": "Apa Fungsi Retina Scan?", "id": "Pindai Pola Pembuluh Darah." },
  { "en": "Apa Itu Deep Learning AI?", "id": "Jaringan Syaraf Tiruan Dalam." },
  { "en": "Apa Itu Edge Computing?", "id": "Proses Data Dekat Sumber." },
  { "en": "Apa Manfaat Edge Computing?", "id": "Kurangi Latensi Dan Bandwidth." },
  { "en": "Apa Itu Blockchain Dalam Energi?", "id": "Transaksi Energi Peer To Peer." },
  { "en": "Apa Itu Digital Twin?", "id": "Replika Digital Aset Fisik." },
  { "en": "Apa Manfaat Digital Twin?", "id": "Simulasi Dan Prediksi Perawatan." },
  { "en": "Apa Itu Augmented Reality (AR)?", "id": "Info Digital Di Dunia Nyata." },
  { "en": "Apa Fungsi AR Di Maintenance?", "id": "Panduan Visual Perbaikan." },
  { "en": "Apa Itu Virtual Reality (VR)?", "id": "Simulasi Lingkungan Penuh." },
  { "en": "Apa Kepanjangan H.264 Video?", "id": "Standar Kompresi Video." },
  { "en": "Apa Itu Bitrate Video?", "id": "Jumlah Bit Per Detik." },
  { "en": "Apa Itu Frame Rate Video?", "id": "Gambar Per Detik." },
  { "en": "Apa Resolusi 4K UHD?", "id": "3840 Kali 2160 Piksel." },
  { "en": "Apa Resolusi Full HD?", "id": "1920 Kali 1080 Piksel." },
  { "en": "Apa Itu Impedansi Akustik?", "id": "Hambatan Medium Terhadap Suara." },
  { "en": "Apa Rumus Kecepatan Cahaya?", "id": "3 Kali 10 Pangkat 8." },
  { "en": "Apa Satuan Intensitas Cahaya?", "id": "Candela." },
  { "en": "Apa Satuan Fluks Cahaya?", "id": "Lumen." },
  { "en": "Apa Satuan Illuminansi Cahaya?", "id": "Lux." },
  { "en": "Apa Fungsi Kapasitor Seri Pada Transmisi?", "id": "Kompensasi Induktansi Saluran." },
  { "en": "Apa Masalah Subsynchronous Resonance (SSR)?", "id": "Osilasi Mekanis Akibat Kapasitor Seri." },
  { "en": "Apa Fungsi Reaktor Shunt Pada Transmisi?", "id": "Serap Daya Reaktif Saat Beban Rendah." },
  { "en": "Apa Kepanjangan SVC Sistem Transmisi?", "id": "Static Var Compensator." },
  { "en": "Apa Fungsi STATCOM Pada Sistem Tenaga?", "id": "Inverter Pengatur Daya Reaktif." },
  { "en": "Apa Perbedaan Utama STATCOM Dan SVC?", "id": "STATCOM Pakai IGBT, SVC Pakai Thyristor." },
  { "en": "Apa Kepanjangan FACTS Sistem Tenaga?", "id": "Flexible AC Transmission Systems." },
  { "en": "Apa Itu Vector Group Trafo?", "id": "Konfigurasi Belitan Dan Geseran Fasa." },
  { "en": "Apa Arti Kode Dyn5 Pada Trafo?", "id": "Delta Star Netral Geser 150 Derajat." },
  { "en": "Apa Fungsi Minyak Isolasi Trafo?", "id": "Pendingin Dan Penyekat Listrik." },
  { "en": "Apa Tes DGA Pada Minyak Trafo?", "id": "Dissolved Gas Analysis." },
  { "en": "Apa Indikasi Gas Asetilen Pada Trafo?", "id": "Terjadi Busur Api Internal." },
  { "en": "Apa Itu Tegangan Impuls Petir?", "id": "Lonjakan Tegangan Sangat Cepat." },
  { "en": "Apa Definisi Creepage Distance Isolator?", "id": "Jarak Rambat Permukaan Isolator." },
  { "en": "Apa Definisi Clearance Distance Isolator?", "id": "Jarak Udara Terpendek Dua Titik." },
  { "en": "Apa Fungsi Horn Gap Pada Isolator?", "id": "Arahkan Busur Api Jauhi Isolator." },
  { "en": "Apa Kepanjangan GIS Gardu Induk?", "id": "Gas Insulated Switchgear." },
  { "en": "Apa Keunggulan Utama GIS?", "id": "Ukuran Kompak Hemat Lahan." },
  { "en": "Apa Itu Streamer Breakdown?", "id": "Mekanisme Tembus Listrik Gas." },
  { "en": "Apa Hukum Paschen Gas?", "id": "Tegangan Tembus Fungsi Tekanan Jarak." },
  { "en": "Apa Kepanjangan RTOS Sistem Embedded?", "id": "Real Time Operating System." },
  { "en": "Apa Sifat Hard Real Time System?", "id": "Toleransi Keterlambatan Nol." },
  { "en": "Apa Sifat Soft Real Time System?", "id": "Toleransi Keterlambatan Rendah." },
  { "en": "Apa Fungsi Scheduler Pada RTOS?", "id": "Atur Urutan Eksekusi Task." },
  { "en": "Apa Itu Context Switch Processor?", "id": "Simpan Dan Muat Status Task." },
  { "en": "Apa Masalah Priority Inversion RTOS?", "id": "Task Prioritas Tinggi Terhambat." },
  { "en": "Apa Fungsi Semaphore Pada Pemrograman?", "id": "Kontrol Akses Sumber Daya." },
  { "en": "Apa Fungsi Mutex Pada Pemrograman?", "id": "Kunci Eksklusif Satu Thread." },
  { "en": "Apa Itu Deadlock Pada Sistem Operasi?", "id": "Saling Tunggu Sumber Daya Macet." },
  { "en": "Apa Kepanjangan FPGA Chip Logika?", "id": "Field Programmable Gate Array." },
  { "en": "Apa Blok Dasar Penyusun FPGA?", "id": "Configurable Logic Block." },
  { "en": "Apa Fungsi Look Up Table (LUT)?", "id": "Implementasi Tabel Kebenaran Logika." },
  { "en": "Apa Perbedaan FPGA Dan Mikrokontroler?", "id": "FPGA Hardware Kustom Paralel." },
  { "en": "Apa Kepanjangan VHDL Bahasa Hardware?", "id": "VHSIC Hardware Description Language." },
  { "en": "Apa Kepanjangan Verilog Bahasa Hardware?", "id": "Verification Logic." },
  { "en": "Apa Fungsi JTAG Port?", "id": "Debugging Dan Pemrograman Chip." },
  { "en": "Apa Itu Boundary Scan Test?", "id": "Tes Koneksi Pin Tanpa Probe." },
  { "en": "Apa Kepanjangan USB Universal?", "id": "Universal Serial Bus." },
  { "en": "Berapa Pin Pada USB 2.0?", "id": "Empat Pin." },
  { "en": "Berapa Pin Pada USB Type C?", "id": "Dua Puluh Empat Pin." },
  { "en": "Apa Itu USB Power Delivery (PD)?", "id": "Protokol Charging Daya Besar." },
  { "en": "Apa Kepanjangan HDMI Video?", "id": "High Definition Multimedia Interface." },
  { "en": "Apa Sinyal Yang Dikirim HDMI?", "id": "Audio Dan Video Digital." },
  { "en": "Apa Kepanjangan VGA Video Lama?", "id": "Video Graphics Array." },
  { "en": "Apa Sinyal Yang Dikirim VGA?", "id": "Video Analog RGB." },
  { "en": "Apa Prinsip Kerja Layar LCD?", "id": "Kristal Cair Blokir Cahaya." },
  { "en": "Apa Kepanjangan OLED Layar?", "id": "Organic Light Emitting Diode." },
  { "en": "Apa Keunggulan Layar OLED?", "id": "Hitam Pekat Kontras Tinggi." },
  { "en": "Apa Itu Pixel Pitch Layar?", "id": "Jarak Antar Pusat Piksel." },
  { "en": "Apa Definisi Refresh Rate Monitor?", "id": "Frekuensi Pembaruan Gambar." },
  { "en": "Apa Kepanjangan CDMA Seluler?", "id": "Code Division Multiple Access." },
  { "en": "Apa Kepanjangan GSM Seluler?", "id": "Global System For Mobile." },
  { "en": "Apa Perbedaan Utama GSM Dan CDMA?", "id": "GSM Waktu, CDMA Kode." },
  { "en": "Apa Kepanjangan OFDMA Pada 4G?", "id": "Orthogonal Frequency Division Multiple Access." },
  { "en": "Apa Itu Multipath Fading?", "id": "Sinyal Pantul Tiba Beda Waktu." },
  { "en": "Apa Fungsi Equalizer Pada Penerima?", "id": "Koreksi Distorsi Kanal." },
  { "en": "Apa Itu Rake Receiver CDMA?", "id": "Gabung Sinyal Multipath." },
  { "en": "Apa Kepanjangan VSAT Satelit?", "id": "Very Small Aperture Terminal." },
  { "en": "Apa Frekuensi Ka Band Satelit?", "id": "26 Sampai 40 Giga Hertz." },
  { "en": "Apa Frekuensi Ku Band Satelit?", "id": "12 Sampai 18 Giga Hertz." },
  { "en": "Apa Frekuensi C Band Satelit?", "id": "4 Sampai 8 Giga Hertz." },
  { "en": "Apa Keunggulan C Band Satelit?", "id": "Tahan Cuaca Hujan." },
  { "en": "Apa Kelemahan Ku Band Satelit?", "id": "Rentan Redaman Hujan." },
  { "en": "Apa Itu Rain Fade?", "id": "Sinyal Hilang Karena Hujan." },
  { "en": "Apa Kepanjangan LNB Parabola?", "id": "Low Noise Block." },
  { "en": "Apa Fungsi LNB Pada Antena?", "id": "Turunkan Frekuensi Satelit Ke Kabel." },
  { "en": "Apa Itu Transponder Satelit?", "id": "Penguat Dan Pengubah Frekuensi." },
  { "en": "Apa Kepanjangan EDFA Fiber Optik?", "id": "Erbium Doped Fiber Amplifier." },
  { "en": "Apa Fungsi EDFA Dalam Jaringan?", "id": "Penguat Cahaya Tanpa Konversi Listrik." },
  { "en": "Apa Itu Dark Fiber?", "id": "Kabel Optik Belum Terpakai." },
  { "en": "Apa Kepanjangan FTTH Internet?", "id": "Fiber To The Home." },
  { "en": "Apa Kepanjangan GPON Internet?", "id": "Gigabit Passive Optical Network." },
  { "en": "Apa Itu Optical Splitter GPON?", "id": "Pembagi Cahaya Pasif." },
  { "en": "Apa Kepanjangan WDM Optik?", "id": "Wavelength Division Multiplexing." },
  { "en": "Apa Prinsip Dasar WDM?", "id": "Banyak Warna Satu Kabel." },
  { "en": "Apa Itu Dispersion Fiber Optik?", "id": "Pelebaran Pulsa Cahaya." },
  { "en": "Apa Akibat Dispersi Pada Data?", "id": "Interferensi Antar Simbol." },
  { "en": "Apa Kepanjangan OTDR Alat Tes?", "id": "Optical Time Domain Reflectometer." },
  { "en": "Apa Fungsi Splicer Fiber Optik?", "id": "Sambung Kabel Kaca Peleburan." },
  { "en": "Apa Kepanjangan LED Lampu?", "id": "Light Emitting Diode." },
  { "en": "Apa Satuan Efisiensi Cahaya?", "id": "Lumen Per Watt." },
  { "en": "Apa Definisi Color Temperature (CCT)?", "id": "Warna Cahaya Derajat Kelvin." },
  { "en": "Apa Warna Cahaya 3000K?", "id": "Warm White Kuning." },
  { "en": "Apa Warna Cahaya 6500K?", "id": "Cool Day Light Putih." },
  { "en": "Apa Definisi Color Rendering Index (CRI)?", "id": "Akurasi Warna Benda." },
  { "en": "Apa Nilai CRI Sempurna?", "id": "Seratus." },
  { "en": "Apa Fungsi Driver LED?", "id": "Suplai Arus Konstan." },
  { "en": "Apa Itu Dimming Pada Lampu?", "id": "Atur Intensitas Cahaya." },
  { "en": "Apa Teknik Dimming Paling Umum?", "id": "Pulse Width Modulation." },
  { "en": "Apa Kepanjangan DALI Protokol Lampu?", "id": "Digital Addressable Lighting Interface." },
  { "en": "Apa Fungsi Amplifier Kelas D?", "id": "Penguat Audio Switching Efisien." },
  { "en": "Apa Itu Total Harmonic Distortion (THD)?", "id": "Persentase Cacat Sinyal." },
  { "en": "Apa Fungsi Crossover Pada Speaker?", "id": "Pisah Frekuensi Suara." },
  { "en": "Apa Itu Tweeter Speaker?", "id": "Pengeras Suara Nada Tinggi." },
  { "en": "Apa Itu Woofer Speaker?", "id": "Pengeras Suara Nada Rendah." },
  { "en": "Apa Itu Subwoofer Speaker?", "id": "Nada Sangat Rendah Bass." },
  { "en": "Apa Impedansi Standar Speaker Rumah?", "id": "8 Ohm." },
  { "en": "Apa Impedansi Standar Speaker Mobil?", "id": "4 Ohm." },
  { "en": "Apa Konektor XLR Digunakan Untuk?", "id": "Audio Profesional Balanced." },
  { "en": "Apa Keuntungan Kabel Balanced Audio?", "id": "Hilangkan Noise Jarak Jauh." },
  { "en": "Apa Tegangan Listrik KRL Jabodetabek?", "id": "1500 Volt DC." },
  { "en": "Apa Nama Pengambil Arus Di Atas Kereta?", "id": "Pantograf." },
  { "en": "Apa Nama Rel Ketiga Pengantar Listrik Kereta?", "id": "Third Rail." },
  { "en": "Apa Jenis Motor Traksi KRL Modern?", "id": "Motor Induksi AC Tiga Fasa." },
  { "en": "Apa Sistem Elektrifikasi Kereta Cepat Whoosh?", "id": "25 Kilo Volt AC." },
  { "en": "Apa Fungsi Catenary Wire Pada Kereta?", "id": "Kabel Penyangga Kawat Kontak." },
  { "en": "Apa Fungsi Contact Wire Pada Kereta?", "id": "Kabel Gesek Pantograf." },
  { "en": "Apa Kepanjangan VVVF Pada Inverter Kereta?", "id": "Variable Voltage Variable Frequency." },
  { "en": "Apa Kepanjangan MTBF Keandalan Sistem?", "id": "Mean Time Between Failures." },
  { "en": "Apa Definisi Mean Time Between Failures?", "id": "Rata Rata Waktu Antar Kerusakan." },
  { "en": "Apa Kepanjangan MTTR Perbaikan Sistem?", "id": "Mean Time To Repair." },
  { "en": "Apa Rumus Ketersediaan Atau Availability Sistem?", "id": "MTBF Bagi Total Waktu." },
  { "en": "Apa Definisi Bathtub Curve?", "id": "Grafik Laju Kegagalan Alat." },
  { "en": "Apa Fase Awal Bathtub Curve?", "id": "Infant Mortality." },
  { "en": "Apa Kepanjangan PUE Data Center?", "id": "Power Usage Effectiveness." },
  { "en": "Berapa Nilai Ideal PUE Data Center?", "id": "Satu Koma Nol." },
  { "en": "Apa Rumus Menghitung PUE?", "id": "Total Daya Bagi Daya IT." },
  { "en": "Apa Kepanjangan HVAC Pendingin?", "id": "Heating Ventilation Air Conditioning." },
  { "en": "Apa Fungsi Phase Locked Loop (PLL)?", "id": "Sinkronisasi Fasa Sinyal." },
  { "en": "Apa Komponen Utama Rangkaian PLL?", "id": "VCO Dan Detektor Fasa." },
  { "en": "Apa Kepanjangan VCO Osilator?", "id": "Voltage Controlled Oscillator." },
  { "en": "Apa Fungsi Voltage Controlled Oscillator?", "id": "Frekuensi Berubah Sesuai Tegangan." },
  { "en": "Apa Fungsi Bypass Diode Panel Surya?", "id": "Cegah Hotspot Saat Teduh." },
  { "en": "Apa Fungsi Blocking Diode Panel Surya?", "id": "Cegah Arus Balik Malam Hari." },
  { "en": "Apa Definisi Azimuth Angle Panel Surya?", "id": "Arah Mata Angin Panel." },
  { "en": "Apa Definisi Tilt Angle Panel Surya?", "id": "Sudut Kemiringan Terhadap Tanah." },
  { "en": "Apa Efek Suhu Panas Pada Panel Surya?", "id": "Tegangan Dan Daya Turun." },
  { "en": "Apa Itu Isc Pada Panel Surya?", "id": "Arus Hubung Singkat." },
  { "en": "Apa Itu Voc Pada Panel Surya?", "id": "Tegangan Rangkaian Terbuka." },
  { "en": "Apa Kepanjangan FOC Kendali Motor?", "id": "Field Oriented Control." },
  { "en": "Apa Kepanjangan DTC Kendali Motor?", "id": "Direct Torque Control." },
  { "en": "Apa Fungsi Transformasi Clarke?", "id": "Tiga Fasa Ke Dua Fasa." },
  { "en": "Apa Fungsi Transformasi Park?", "id": "Kerangka Diam Ke Putar." },
  { "en": "Apa Itu Sinyal PWM Space Vector?", "id": "Teknik Modulasi Vektor Ruang." },
  { "en": "Apa Kelebihan Motor Universal?", "id": "Bisa AC Dan DC." },
  { "en": "Apa Aplikasi Umum Motor Universal?", "id": "Bor Tangan Dan Blender." },
  { "en": "Apa Ciri Fisik Motor Shaded Pole?", "id": "Cincin Tembaga Di Kutub." },
  { "en": "Apa Arah Putaran Motor Shaded Pole?", "id": "Menuju Cincin Bayangan." },
  { "en": "Apa Kepanjangan C-Rate Baterai?", "id": "Charge Discharge Rate." },
  { "en": "Apa Arti Baterai 1C Rate?", "id": "Habis Dalam Satu Jam." },
  { "en": "Apa Arti Baterai 2C Rate?", "id": "Habis Dalam 30 Menit." },
  { "en": "Apa Kepanjangan SoH Baterai?", "id": "State Of Health." },
  { "en": "Apa Definisi State Of Health?", "id": "Kondisi Kesehatan Baterai." },
  { "en": "Apa Penyebab Baterai Kembung?", "id": "Gas Akibat Elektrolit Terurai." },
  { "en": "Apa Itu Dendrit Pada Baterai Lithium?", "id": "Kristal Tajam Tembus Separator." },
  { "en": "Apa Bahaya Dendrit Baterai?", "id": "Hubung Singkat Internal." },
  { "en": "Apa Kepanjangan DHCP Jaringan?", "id": "Dynamic Host Configuration Protocol." },
  { "en": "Apa Fungsi Protokol DHCP?", "id": "Otomatisasi Alamat IP." },
  { "en": "Apa Kepanjangan DNS Internet?", "id": "Domain Name System." },
  { "en": "Apa Fungsi Protokol DNS?", "id": "Terjemah Nama Ke IP." },
  { "en": "Apa Kepanjangan NTP Waktu?", "id": "Network Time Protocol." },
  { "en": "Apa Kepanjangan FTP Transfer File?", "id": "File Transfer Protocol." },
  { "en": "Apa Kepanjangan SSH Remote?", "id": "Secure Shell." },
  { "en": "Apa Port Standar HTTP?", "id": "Port 80." },
  { "en": "Apa Port Standar HTTPS?", "id": "Port 443." },
  { "en": "Apa Itu Triplen Harmonics?", "id": "Harmonisa Kelipatan Tiga." },
  { "en": "Dimana Arus Triplen Harmonics Berkumpul?", "id": "Kawat Netral." },
  { "en": "Apa Itu K-Factor Transformator?", "id": "Rating Tahan Panas Harmonisa." },
  { "en": "Apa Fungsi Active Power Filter?", "id": "Injeksi Arus Lawan Harmonisa." },
  { "en": "Apa Itu Passive Filter Harmonisa?", "id": "Kombinasi L Dan C Resonansi." },
  { "en": "Apa Bahan Dielektrik Kapasitor Keramik?", "id": "Barium Titanate." },
  { "en": "Apa Bahan Dielektrik Kapasitor Elco?", "id": "Aluminium Oksida." },
  { "en": "Apa Bahan Dielektrik Kapasitor Mylar?", "id": "Polyester." },
  { "en": "Apa Sifat Bahan Feromagnetik?", "id": "Tarik Kuat Magnet." },
  { "en": "Apa Sifat Bahan Diamagnetik?", "id": "Tolak Lemah Magnet." },
  { "en": "Apa Sifat Bahan Paramagnetik?", "id": "Tarik Lemah Magnet." },
  { "en": "Apa Definisi Suhu Curie Magnet?", "id": "Batas Hilang Sifat Magnet." },
  { "en": "Apa Satuan Koersivitas Magnet?", "id": "Ampere Per Meter." },
  { "en": "Apa Itu Remanensi Magnet?", "id": "Sisa Magnet Tanpa Medan." },
  { "en": "Apa Itu Knee Point Kurva CT?", "id": "Titik Mulai Jenuh." },
  { "en": "Apa Akibat Jika CT Jenuh?", "id": "Arus Sekunder Tidak Linear." },
  { "en": "Apa Kelas Akurasi CT Proteksi?", "id": "5P20 Atau 10P10." },
  { "en": "Apa Kelas Akurasi CT Metering?", "id": "0.2 Atau 0.5S." },
  { "en": "Apa Arti Kode 5P20 Pada CT?", "id": "Eror 5 Persen 20 Kali Arus." },
  { "en": "Apa Fungsi Burden Pada CT?", "id": "Beban Rangkaian Sekunder." },
  { "en": "Apa Bahaya Sekunder CT Terbuka?", "id": "Tegangan Tinggi Meledak." },
  { "en": "Apa Itu Wiring Diagram Skematik?", "id": "Gambar Logika Rangkaian." },
  { "en": "Apa Itu Wiring Diagram Layout?", "id": "Gambar Tata Letak Fisik." },
  { "en": "Apa Simbol NO Pada Saklar?", "id": "Normally Open." },
  { "en": "Apa Simbol NC Pada Saklar?", "id": "Normally Closed." },
  { "en": "Apa Fungsi Limit Switch Mekanik?", "id": "Deteksi Batas Gerakan." },
  { "en": "Apa Fungsi Float Switch Tangki?", "id": "Deteksi Level Cairan." },
  { "en": "Apa Itu Reed Switch Magnetik?", "id": "Saklar Kaca Kontak Magnet." },
  { "en": "Apa Kepanjangan SSR Relay?", "id": "Solid State Relay." },
  { "en": "Apa Kelebihan SSR Dibanding Relay Biasa?", "id": "Cepat Dan Tanpa Bunyi." },
  { "en": "Apa Kekurangan SSR Relay?", "id": "Panas Dan Ada Arus Bocor." },
  { "en": "Apa Itu Zero Crossing Detector?", "id": "Deteksi Tegangan Nol AC." },
  { "en": "Apa Fungsi Zero Crossing Pada Dimmer?", "id": "Sinkronisasi Pemicuan Triac." },
  { "en": "Apa Kepanjangan EMI Filter?", "id": "Electromagnetic Interference Filter." },
  { "en": "Apa Komponen Utama EMI Filter?", "id": "Induktor Common Mode Dan Kapasitor." },
  { "en": "Apa Itu Capacitor X Rating?", "id": "Aman Hubung Singkat L-N." },
  { "en": "Apa Itu Capacitor Y Rating?", "id": "Aman Hubung Singkat L-G." },
  { "en": "Apa Rumus Energi Tersimpan Kapasitor?", "id": "Setengah C V Kuadrat." },
  { "en": "Apa Rumus Energi Tersimpan Induktor?", "id": "Setengah L I Kuadrat." },
  { "en": "Apa Satuan Konstanta Dielektrik?", "id": "Farad Per Meter." },
  { "en": "Apa Nilai Permitivitas Ruang Hampa?", "id": "8.85 Kali 10 Pangkat Min 12." },
  { "en": "Apa Efek Josephson Superkonduktor?", "id": "Arus Terowongan Tanpa Tegangan." },
  { "en": "Apa Fungsi Alat SQUID?", "id": "Magnetometer Sangat Sensitif." },
  { "en": "Apa Kepanjangan SQUID Fisika?", "id": "Superconducting Quantum Interference Device." },
  { "en": "Apa Itu Fenomena Hunting Motor Sinkron?", "id": "Osilasi Rotor Sekitar Kecepatan Sinkron." },
  { "en": "Apa Fungsi Damper Winding Motor Sinkron?", "id": "Redam Osilasi Hunting." },
  { "en": "Apa Kurva V Pada Motor Sinkron?", "id": "Grafik Arus Jangkar Vs Eksitasi." },
  { "en": "Apa Titik Terendah Kurva V?", "id": "Faktor Daya Satu." },
  { "en": "Apa Itu Fenomena Crawling Motor Induksi?", "id": "Putar Lambat Harmonisa Ganjil." },
  { "en": "Apa Itu Fenomena Cogging Motor Induksi?", "id": "Rotor Terkunci Slot Stator." },
  { "en": "Apa Cara Mencegah Cogging Motor?", "id": "Miringkan Slot Rotor Skewing." },
  { "en": "Apa Itu Pengereman Plugging?", "id": "Balik Fasa Saat Berputar." },
  { "en": "Apa Itu Pengereman Dinamis?", "id": "Buang Energi Ke Resistor." },
  { "en": "Apa Perbedaan Rotor Salient Pole?", "id": "Kutub Menonjol Putaran Rendah." },
  { "en": "Apa Perbedaan Rotor Cylindrical?", "id": "Silinder Halus Putaran Tinggi." },
  { "en": "Apa Efek Miller Pada Transistor?", "id": "Peningkatan Kapasitansi Input." },
  { "en": "Apa Efek Early Pada BJT?", "id": "Modulasi Lebar Basis." },
  { "en": "Apa Tegangan Pinch Off JFET?", "id": "Saluran Tertutup Total." },
  { "en": "Apa Transkonduktansi (Gm) MOSFET?", "id": "Perubahan Arus Drain Per Gate." },
  { "en": "Apa Itu Gain Bandwidth Product?", "id": "Perkalian Penguatan Dan Frekuensi." },
  { "en": "Apa Itu Thermal Runaway Transistor?", "id": "Panas Berlebih Hancurkan Komponen." },
  { "en": "Apa Fungsi Heat Pipe Pendingin?", "id": "Transfer Panas Fase Uap." },
  { "en": "Apa Itu Surge Impedance Loading (SIL)?", "id": "Daya Natural Saluran Transmisi." },
  { "en": "Apa Nilai Impedansi Surge (Zs) Udara?", "id": "400 Sampai 600 Ohm." },
  { "en": "Apa Nilai Impedansi Surge (Zs) Kabel?", "id": "50 Sampai 75 Ohm." },
  { "en": "Apa Fungsi Transposisi Kabel Transmisi?", "id": "Seimbangkan Impedansi Fasa." },
  { "en": "Apa Itu String Efficiency Isolator?", "id": "Distribusi Tegangan Antar Piringan." },
  { "en": "Apa Fungsi Grading Ring Isolator?", "id": "Ratakan Distribusi Tegangan." },
  { "en": "Apa Itu Arcing Horn Isolator?", "id": "Lindungi Isolator Dari Busur." },
  { "en": "Apa Definisi Sag Atau Andongan?", "id": "Lengkungan Kabel Antar Tiang." },
  { "en": "Apa Pengaruh Suhu Terhadap Sag?", "id": "Suhu Naik Sag Bertambah." },
  { "en": "Apa Itu Little Endian Memori?", "id": "Byte Kecil Alamat Awal." },
  { "en": "Apa Itu Big Endian Memori?", "id": "Byte Besar Alamat Awal." },
  { "en": "Apa Definisi Nibble Data?", "id": "Empat Bit." },
  { "en": "Apa Definisi Word Data 32 Bit?", "id": "Empat Byte." },
  { "en": "Apa Itu Stack Overflow?", "id": "Memori Tumpukan Penuh." },
  { "en": "Apa Itu Heap Memory?", "id": "Alokasi Memori Dinamis." },
  { "en": "Apa Fungsi Volatile Keyword C?", "id": "Cegah Optimasi Kompiler Variabel." },
  { "en": "Apa Itu Interrupt Latency?", "id": "Waktu Tanggap Sinyal Interupsi." },
  { "en": "Apa Pola Lissajous Osiloskop?", "id": "Grafik XY Beda Fasa." },
  { "en": "Apa Bentuk Lissajous Fasa 90 Derajat?", "id": "Lingkaran." },
  { "en": "Apa Bentuk Lissajous Fasa 0 Derajat?", "id": "Garis Lurus Miring Kanan." },
  { "en": "Apa Fungsi Logic Analyzer?", "id": "Analisis Banyak Sinyal Digital." },
  { "en": "Apa Fungsi Spectrum Analyzer?", "id": "Analisis Domain Frekuensi." },
  { "en": "Apa Fungsi Function Generator?", "id": "Pembangkit Gelombang Standar." },
  { "en": "Apa Fungsi LCR Meter?", "id": "Ukur Induktansi Kapasitansi Resistansi." },
  { "en": "Apa Fungsi Tachometer?", "id": "Ukur Kecepatan Putar Motor." },
  { "en": "Apa Fungsi Stroboscope?", "id": "Ukur RPM Cahaya Kilat." },
  { "en": "Apa Logam Termokopel Tipe K?", "id": "Chromel Dan Alumel." },
  { "en": "Apa Logam Termokopel Tipe J?", "id": "Iron Dan Constantan." },
  { "en": "Apa Rentang Suhu Termokopel Tipe K?", "id": "Minus 200 Sampai 1250 Celcius." },
  { "en": "Apa Efek Hall Tegangan?", "id": "Tegangan Tegak Lurus Arus Magnet." },
  { "en": "Apa Bunyi Hukum Biot Savart?", "id": "Medan Magnet Kawat Berarus." },
  { "en": "Apa Konstanta Permeabilitas Ruang Hampa?", "id": "4 Pi Kali 10 Pangkat Min 7." },
  { "en": "Apa Itu Dithering Pada Audio?", "id": "Noise Acak Samarkan Kuantisasi." },
  { "en": "Apa Fungsi Windowing FFT?", "id": "Kurangi Kebocoran Spektral." },
  { "en": "Apa Itu Spectrogram?", "id": "Visualisasi Frekuensi Terhadap Waktu." },
  { "en": "Apa Beda IIR Dan FIR Filter?", "id": "IIR Ada Umpan Balik." },
  { "en": "Apa Itu Zero Padding Sinyal?", "id": "Tambah Nol Perhalus Spektrum." },
  { "en": "Apa Kepanjangan LOTO Safety?", "id": "Lock Out Tag Out." },
  { "en": "Apa Tujuan Prosedur LOTO?", "id": "Cegah Energi Nyala Tak Sengaja." },
  { "en": "Apa Itu Arc Flash Boundary?", "id": "Jarak Aman Ledakan Busur." },
  { "en": "Apa Satuan Energi Insiden Arc Flash?", "id": "Kalori Per Sentimeter Persegi." },
  { "en": "Apa Fungsi APD Sarung Tangan HV?", "id": "Isolasi Tegangan Sentuh." },
  { "en": "Apa Kepanjangan PPE Safety?", "id": "Personal Protective Equipment." },
  { "en": "Apa Standar Warna Kabel DC Positif?", "id": "Merah." },
  { "en": "Apa Standar Warna Kabel DC Negatif?", "id": "Hitam." },
  { "en": "Apa Fungsi Galvanometer?", "id": "Deteksi Arus Sangat Kecil." },
  { "en": "Apa Prinsip Kerja Amperemeter Clamp?", "id": "Induksi Magnetik Kabel Tunggal." },
  { "en": "Apa Kepanjangan RMS Multimeter?", "id": "Root Mean Square." },
  { "en": "Apa Bedanya True RMS Dan Averaging?", "id": "True RMS Akurat Gelombang Cacat." },
  { "en": "Apa Fungsi Earth Tester?", "id": "Ukur Tahanan Pentanahan." },
  { "en": "Apa Metode Tiga Titik Earth Tester?", "id": "Metode Jatuh Tegangan." },
  { "en": "Apa Standar Tahanan Grounding Rumah?", "id": "Maksimal 5 Ohm." },
  { "en": "Apa Standar Tahanan Grounding Petir?", "id": "Maksimal 1 Ohm." },
  { "en": "Apa Itu IP Rating IP54?", "id": "Tahan Debu Tahan Cipratan." },
  { "en": "Apa Itu IP Rating IP65?", "id": "Tahan Debu Tahan Semprotan." },
  { "en": "Apa Kepanjangan NEMA Enclosure?", "id": "National Electrical Manufacturers Association." },
  { "en": "Apa Perbedaan Sensor NPN Dan PNP?", "id": "NPN Sinking PNP Sourcing." },
  { "en": "Apa Arti Output Sinking?", "id": "Menyambung Ke Ground." },
  { "en": "Apa Arti Output Sourcing?", "id": "Menyambung Ke VCC." },
  { "en": "Apa Itu Protocol Hart Industri?", "id": "Sinyal Digital Tumpang Analog." },
  { "en": "Apa Kepanjangan DCS Sistem Kontrol?", "id": "Distributed Control System." },
  { "en": "Apa Perbedaan DCS Dan PLC?", "id": "DCS Fokus Proses Kontinyu." },
  { "en": "Apa Itu Safety Instrumented System (SIS)?", "id": "Sistem Proteksi Kegagalan Proses." },
  { "en": "Apa Kepanjangan SIL Dalam Safety?", "id": "Safety Integrity Level." },
  { "en": "Ada Berapa Tingkatan Level SIL?", "id": "Empat Level." },
  { "en": "Apa Kepanjangan PID Gambar Teknik?", "id": "Piping And Instrumentation Diagram." },
  { "en": "Apa Simbol Lingkaran Pada PID?", "id": "Instrumen Terpasang Di Lapangan." },
  { "en": "Apa Simbol Lingkaran Garis PID?", "id": "Instrumen Di Ruang Kontrol." },
  { "en": "Apa Fungsi Thermowell?", "id": "Lindungi Sensor Suhu." },
  { "en": "Apa Fungsi Orifice Plate?", "id": "Ukur Aliran Beda Tekanan." },
  { "en": "Apa Prinsip Kerja Flowmeter Magnetik?", "id": "Hukum Faraday Induksi." },
  { "en": "Apa Prinsip Kerja Flowmeter Coriolis?", "id": "Gaya Coriolis Massa Alir." },
  { "en": "Apa Prinsip Kerja Flowmeter Ultrasonic?", "id": "Waktu Tempuh Gelombang Suara." },
  { "en": "Apa Itu Load Cell Timbangan?", "id": "Sensor Berat Strain Gauge." },
  { "en": "Apa Fungsi Wheatstone Bridge Load Cell?", "id": "Konversi Resistansi Ke Tegangan." },
  { "en": "Apa Itu Hysteresis Error Sensor?", "id": "Beda Nilai Naik Turun." },
  { "en": "Apa Itu Linearity Error Sensor?", "id": "Penyimpangan Dari Garis Lurus." },
  { "en": "Apa Itu Stability Sensor?", "id": "Konsistensi Jangka Panjang." },
  { "en": "Apa Itu Proximity Effect Kabel?", "id": "Arus Terdesak Kabel Tetangga." },
  { "en": "Apa Fungsi Register DDR Pada AVR?", "id": "Atur Arah Data Port." },
  { "en": "Apa Fungsi Register PORT Pada AVR?", "id": "Output Logika Atau Pull Up." },
  { "en": "Apa Fungsi Register PIN Pada AVR?", "id": "Baca Status Input Pin." },
  { "en": "Apa Kepanjangan DDR Pada Mikrokontroler?", "id": "Data Direction Register." },
  { "en": "Apa Itu Prescaler Timer?", "id": "Pembagi Frekuensi Clock Timer." },
  { "en": "Apa Fungsi PWM Fast Mode?", "id": "Frekuensi PWM Tinggi." },
  { "en": "Apa Fungsi PWM Phase Correct?", "id": "Resolusi PWM Tinggi." },
  { "en": "Apa Itu Brown Out Detection (BOD)?", "id": "Reset Saat Tegangan Tidak Stabil." },
  { "en": "Apa Fungsi Watchdog Timer Reset?", "id": "Restart Sistem Saat Hang." },
  { "en": "Apa Itu External Interrupt?", "id": "Interupsi Dari Pin Luar." },
  { "en": "Apa Bunyi Teorema Reciprocity?", "id": "Respon Sama Meski Sumber Ditukar." },
  { "en": "Apa Bunyi Teorema Tellegen?", "id": "Total Daya Rangkaian Tertutup Nol." },
  { "en": "Apa Bunyi Teorema Substitusi?", "id": "Ganti Cabang Dengan Sumber Sama." },
  { "en": "Apa Itu Mesh Analysis?", "id": "Analisis Arus Loop Tertutup." },
  { "en": "Apa Itu Nodal Analysis?", "id": "Analisis Tegangan Titik Simpul." },
  { "en": "Apa Itu Supernode Dalam Rangkaian?", "id": "Sumber Tegangan Antara Dua Node." },
  { "en": "Apa Itu Supermesh Dalam Rangkaian?", "id": "Sumber Arus Antara Dua Loop." },
  { "en": "Apa Konstanta Waktu Rangkaian RC?", "id": "Resistansi Kali Kapasitansi." },
  { "en": "Apa Respon Transien Overdamped?", "id": "Lambat Tanpa Osilasi." },
  { "en": "Apa Respon Transien Underdamped?", "id": "Cepat Tapi Berosilasi." },
  { "en": "Apa Itu Zona Fresnel?", "id": "Area Ellipsoid Rambatan Gelombang." },
  { "en": "Apa Syarat Line Of Sight (LOS)?", "id": "Zona Fresnel Pertama Bersih." },
  { "en": "Apa Itu Fading Multipath?", "id": "Sinyal Lemah Akibat Pantulan." },
  { "en": "Apa Itu Isotropic Radiator?", "id": "Antena Sumber Titik Ideal." },
  { "en": "Berapa Gain Antena Isotropik?", "id": "Nol dBi." },
  { "en": "Apa Kepanjangan dBi Pada Antena?", "id": "Decibel Isotropic." },
  { "en": "Apa Kepanjangan dBd Pada Antena?", "id": "Decibel Dipole." },
  { "en": "Apa Hubungan dBi Dan dBd?", "id": "dBi Sama Dengan dBd Tambah 2.15." },
  { "en": "Apa Itu Beamwidth Antena?", "id": "Sudut Pancaran Daya Setengah." },
  { "en": "Apa Itu Front To Back Ratio?", "id": "Perbandingan Sinyal Depan Belakang." },
  { "en": "Apa Definisi Infinite Bus?", "id": "Tegangan Dan Frekuensi Konstan." },
  { "en": "Apa Persamaan Swing Equation?", "id": "Dinamika Rotor Generator Sinkron." },
  { "en": "Apa Itu Transient Stability?", "id": "Kestabilan Gangguan Besar Tiba Tiba." },
  { "en": "Apa Itu Dynamic Stability?", "id": "Kestabilan Gangguan Kecil Lama." },
  { "en": "Apa Fungsi Critical Clearing Time?", "id": "Waktu Maksimum Putus Gangguan." },
  { "en": "Apa Itu Equal Area Criterion?", "id": "Metode Grafis Stabilitas Transien." },
  { "en": "Apa Penyebab Voltage Collapse?", "id": "Kekurangan Daya Reaktif Ekstrem." },
  { "en": "Apa Itu Black Start Pembangkit?", "id": "Start Tanpa Pasokan Listrik Luar." },
  { "en": "Apa Fungsi Load Shedding?", "id": "Lepas Beban Jaga Frekuensi." },
  { "en": "Apa Itu Spinning Reserve?", "id": "Cadangan Daya Generator Berputar." },
  { "en": "Apa Teknologi Jaringan LPWAN?", "id": "Low Power Wide Area Network." },
  { "en": "Apa Kepanjangan LoRa?", "id": "Long Range." },
  { "en": "Apa Teknik Modulasi LoRa?", "id": "Chirp Spread Spectrum." },
  { "en": "Apa Topologi Jaringan LoRaWAN?", "id": "Star Of Stars." },
  { "en": "Apa Kepanjangan NB-IoT?", "id": "Narrowband Internet Of Things." },
  { "en": "Apa Jalur Frekuensi NB-IoT?", "id": "Pita Frekuensi Seluler LTE." },
  { "en": "Apa Protokol Zigbee Menggunakan Standar?", "id": "IEEE 802.15.4." },
  { "en": "Apa Keunggulan Zigbee Mesh?", "id": "Jangkauan Luas Lewat Lompatan." },
  { "en": "Apa Kepanjangan BLE?", "id": "Bluetooth Low Energy." },
  { "en": "Apa Fungsi Beacon BLE?", "id": "Pancar Identitas Ke Sekitar." },
  { "en": "Apa Konfigurasi Cascode Amplifier?", "id": "Common Emitter Seri Common Base." },
  { "en": "Apa Keuntungan Cascode Amplifier?", "id": "Bandwidth Lebar Isolasi Tinggi." },
  { "en": "Apa Konfigurasi Differential Pair?", "id": "Dua Transistor Emitter Gabung." },
  { "en": "Apa Fungsi Cermin Arus (Current Mirror)?", "id": "Salin Arus Referensi Konstan." },
  { "en": "Apa Itu Wilson Current Mirror?", "id": "Cermin Arus Impedansi Tinggi." },
  { "en": "Apa Itu Schmitt Trigger Inverting?", "id": "Histeresis Pembalik Sinyal." },
  { "en": "Apa Fungsi Buffer Amplifier?", "id": "Penyangga Impedansi Gain Satu." },
  { "en": "Apa Itu Bootstrapping Circuit?", "id": "Meningkatkan Impedansi Input." },
  { "en": "Apa Fungsi Voltage Follower?", "id": "Tegangan Output Ikuti Input." },
  { "en": "Apa Itu Virtual Ground Op-Amp?", "id": "Titik Referensi Semu Nol Volt." },
  { "en": "Apa Kepanjangan MOSFET Depletion Mode?", "id": "Saluran Ada Tanpa Gate." },
  { "en": "Apa Kepanjangan MOSFET Enhancement Mode?", "id": "Saluran Muncul Saat Gate Aktif." },
  { "en": "Apa Itu Body Diode MOSFET?", "id": "Dioda Parasitik Internal." },
  { "en": "Apa Kelebihan SiC MOSFET?", "id": "Tahan Tegangan Dan Suhu Tinggi." },
  { "en": "Apa Kelebihan GaN Transistor?", "id": "Switching Frekuensi Sangat Tinggi." },
  { "en": "Apa Kepanjangan SiC Semikonduktor?", "id": "Silicon Carbide." },
  { "en": "Apa Kepanjangan GaN Semikonduktor?", "id": "Gallium Nitride." },
  { "en": "Apa Itu Safe Operating Area (SOA)?", "id": "Batas Aman Arus Tegangan." },
  { "en": "Apa Rumus Daya Disipasi Transistor?", "id": "Vce Kali Ic." },
  { "en": "Apa Fungsi Snubber RC?", "id": "Redam Lonjakan Tegangan Switching." },
  { "en": "Apa Peta Karnaugh (K-Map)?", "id": "Metode Penyederhanaan Logika Grafis." },
  { "en": "Apa Itu Don't Care Condition?", "id": "Output Bebas Nilai Apapun." },
  { "en": "Apa Itu Propagation Delay Gate?", "id": "Waktu Tunda Sinyal Gerbang." },
  { "en": "Apa Itu Fan Out Logika?", "id": "Jumlah Gerbang Bisa Didrive." },
  { "en": "Apa Itu Fan In Logika?", "id": "Jumlah Input Gerbang Logika." },
  { "en": "Apa Sifat Logika TTL?", "id": "Cepat Tapi Boros Daya." },
  { "en": "Apa Sifat Logika CMOS?", "id": "Lambat Tapi Hemat Daya." },
  { "en": "Apa Masalah Floating Input CMOS?", "id": "Osilasi Dan Panas Berlebih." },
  { "en": "Apa Itu Tristate Logic?", "id": "High Low Dan High Impedance." },
  { "en": "Apa Fungsi High Impedance (Hi-Z)?", "id": "Lepas Hubungan Dari Bus." },
  { "en": "Apa Prinsip Motor Reluktansi?", "id": "Rotor Cari Reluktansi Terendah." },
  { "en": "Apa Kelebihan Switched Reluctance Motor?", "id": "Konstruksi Rotor Sederhana Kokoh." },
  { "en": "Apa Itu Flux Weakening Control?", "id": "Operasi Di Atas Kecepatan Dasar." },
  { "en": "Apa Fungsi Soft Starter?", "id": "Naikkan Tegangan Bertahap." },
  { "en": "Apa Teknik Pengereman Regeneratif?", "id": "Motor Jadi Generator Balik Daya." },
  { "en": "Apa Fungsi Braking Resistor?", "id": "Buang Energi Pengereman Panas." },
  { "en": "Apa Itu V/f Control?", "id": "Jaga Rasio Tegangan Frekuensi." },
  { "en": "Apa Tujuan V/f Konstan?", "id": "Torsi Konstan Kecepatan Berubah." },
  { "en": "Apa Itu Slip Compensation?", "id": "Tambah Frekuensi Saat Beban Naik." },
  { "en": "Apa Definisi Transformasi Laplace?", "id": "Ubah Waktu Ke Frekuensi Kompleks." },
  { "en": "Apa Itu Region Of Convergence (ROC)?", "id": "Daerah Transformasi Z Konvergen." },
  { "en": "Apa Sifat Konvolusi Domain Waktu?", "id": "Perkalian Domain Frekuensi." },
  { "en": "Apa Sifat Perkalian Domain Waktu?", "id": "Konvolusi Domain Frekuensi." },
  { "en": "Apa Fungsi Delta Dirac?", "id": "Sampling Sinyal Kontinyu." },
  { "en": "Apa Integral Dari Delta Dirac?", "id": "Satu." },
  { "en": "Apa Itu Parseval's Theorem?", "id": "Energi Waktu Sama Dengan Frekuensi." },
  { "en": "Apa Bentuk Sinyal White Noise?", "id": "Rapat Spektral Daya Rata." },
  { "en": "Apa Bentuk Sinyal Pink Noise?", "id": "Daya Turun 3dB Per Oktaf." },
  { "en": "Apa Itu Zero Order Hold?", "id": "Pertahankan Nilai Sampel." },
  { "en": "Apa Itu Proses Oksidasi Wafer?", "id": "Pembentukan Lapisan Silikon Dioksida." },
  { "en": "Apa Fungsi Photoresist Pada Lithography?", "id": "Masker Pelindung Cahaya UV." },
  { "en": "Apa Itu Proses Etching Wafer?", "id": "Pengikisan Lapisan Material." },
  { "en": "Apa Bedanya Wet Etching Dan Dry Etching?", "id": "Cairan Kimia Lawan Plasma." },
  { "en": "Apa Itu Ion Implantation?", "id": "Tembak Ion Doping Ke Wafer." },
  { "en": "Apa Fungsi Proses Annealing?", "id": "Perbaiki Struktur Kristal Rusak." },
  { "en": "Apa Itu Clean Room Kelas 10?", "id": "Maksimal 10 Partikel Per Kaki." },
  { "en": "Apa Kepanjangan MEMS Teknologi Mikro?", "id": "Micro Electro Mechanical Systems." },
  { "en": "Apa Itu Parameter Z Two Port Network?", "id": "Parameter Impedansi Rangkaian." },
  { "en": "Apa Itu Parameter Y Two Port Network?", "id": "Parameter Admitansi Rangkaian." },
  { "en": "Apa Itu Parameter H Two Port Network?", "id": "Parameter Hibrid Rangkaian." },
  { "en": "Apa Itu Parameter ABCD Two Port Network?", "id": "Parameter Transmisi Rangkaian." },
  { "en": "Apa Syarat Rangkaian Resiprokal?", "id": "Z Satu Dua Sama Dengan Z Dua Satu." },
  { "en": "Apa Syarat Rangkaian Simetris?", "id": "Z Satu Satu Sama Dengan Z Dua Dua." },
  { "en": "Apa Kepanjangan AGC Sistem Tenaga?", "id": "Automatic Generation Control." },
  { "en": "Apa Fungsi Automatic Generation Control?", "id": "Atur Daya Output Generator Otomatis." },
  { "en": "Apa Kepanjangan ACE Area Control?", "id": "Area Control Error." },
  { "en": "Apa Komponen Utama Area Control Error?", "id": "Selisih Frekuensi Dan Daya Tie." },
  { "en": "Apa Itu Load Frequency Control (LFC)?", "id": "Jaga Frekuensi Tetap Nominal." },
  { "en": "Apa Itu Governor Droop Control?", "id": "Penurunan Kecepatan Saat Beban Naik." },
  { "en": "Berapa Nilai Standar Droop Speed?", "id": "Empat Sampai Lima Persen." },
  { "en": "Apa Fungsi Flywheel Energy Storage?", "id": "Simpan Energi Kinetik Putaran." },
  { "en": "Apa Kelebihan Flywheel Storage?", "id": "Respon Cepat Siklus Hidup Panjang." },
  { "en": "Apa Kepanjangan PLA Logika?", "id": "Programmable Logic Array." },
  { "en": "Apa Kepanjangan PAL Logika?", "id": "Programmable Array Logic." },
  { "en": "Apa Perbedaan PLA Dan PAL?", "id": "PLA Dua Plane Bisa Diprogram." },
  { "en": "Apa Itu CPLD Chip?", "id": "Complex Programmable Logic Device." },
  { "en": "Apa Elemen Dasar CPLD?", "id": "Macrocell." },
  { "en": "Apa Itu Inter Symbol Interference (ISI)?", "id": "Tumpang Tindih Pulsa Data." },
  { "en": "Apa Penyebab Utama ISI?", "id": "Bandwidth Kanal Terbatas." },
  { "en": "Apa Fungsi Eye Diagram?", "id": "Evaluasi Kualitas Sinyal Digital." },
  { "en": "Apa Arti Mata Terbuka Lebar?", "id": "Margin Noise Bagus Minim ISI." },
  { "en": "Apa Itu Gauge Factor Strain Gauge?", "id": "Sensitivitas Perubahan Resistansi." },
  { "en": "Apa Konfigurasi Quarter Bridge?", "id": "Satu Strain Gauge Aktif." },
  { "en": "Apa Konfigurasi Half Bridge?", "id": "Dua Strain Gauge Aktif." },
  { "en": "Apa Konfigurasi Full Bridge?", "id": "Empat Strain Gauge Aktif." },
  { "en": "Apa Kelebihan Full Bridge?", "id": "Sensitivitas Tertinggi Kompensasi Suhu." },
  { "en": "Apa Itu Piezoelectric Accelerometer?", "id": "Kristal Hasilkan Tegangan Saat Getar." },
  { "en": "Apa Itu Charge Amplifier?", "id": "Penguat Sinyal Sensor Piezoelektrik." },
  { "en": "Apa Kepanjangan VSD Penggerak Motor?", "id": "Variable Speed Drive." },
  { "en": "Apa Perbedaan VSD Dan VFD?", "id": "VSD Istilah Umum, VFD Khusus AC." },
  { "en": "Apa Fungsi Dynamic Braking Resistor?", "id": "Serap Energi Regeneratif Motor." },
  { "en": "Apa Itu DC Injection Braking?", "id": "Suntik Arus DC Ke Stator." },
  { "en": "Apa Efek DC Injection?", "id": "Kunci Poros Motor Diam." },
  { "en": "Apa Itu Motor Chopper?", "id": "Pemotong Arus DC." },
  { "en": "Apa Kelas Operasi Chopper Tipe A?", "id": "Satu Kuadran Motoring." },
  { "en": "Apa Kelas Operasi Chopper Tipe B?", "id": "Satu Kuadran Regenerative." },
  { "en": "Apa Kelas Operasi Chopper Tipe E?", "id": "Empat Kuadran Penuh." },
  { "en": "Apa Kepanjangan SMPS Power Supply?", "id": "Switched Mode Power Supply." },
  { "en": "Apa Kelebihan Utama SMPS?", "id": "Efisien Dan Ukuran Kecil." },
  { "en": "Apa Topologi Flyback Converter?", "id": "Isolasi Trafo Simpan Energi." },
  { "en": "Apa Topologi Forward Converter?", "id": "Transfer Energi Langsung Trafo." },
  { "en": "Apa Fungsi Optoisolator Feedback SMPS?", "id": "Umpan Balik Terisolasi." },
  { "en": "Apa Itu Ripple Voltage Output?", "id": "Sisa Komponen AC Pada DC." },
  { "en": "Apa Rumus Ripple Factor?", "id": "Vrms AC Bagi Vrata DC." },
  { "en": "Apa Kepanjangan EMC Standar?", "id": "Electromagnetic Compatibility." },
  { "en": "Apa Itu Radiated Emission?", "id": "Gangguan Memancar Lewat Udara." },
  { "en": "Apa Itu Conducted Emission?", "id": "Gangguan Merambat Lewat Kabel." },
  { "en": "Apa Fungsi Faraday Cage?", "id": "Blokir Medan Elektromagnetik Luar." },
  { "en": "Apa Bahan Terbaik Faraday Cage?", "id": "Tembaga Atau Aluminium Mesh." },
  { "en": "Apa Itu Antenna Gain dBi?", "id": "Penguatan Relatif Isotropik." },
  { "en": "Apa Itu Antenna Beam Tilt?", "id": "Kemiringan Pola Pancar Vertikal." },
  { "en": "Apa Fungsi Antenna Sectoral?", "id": "Pancar Area Sudut Tertentu." },
  { "en": "Apa Itu Sidelobe Antena?", "id": "Pancaran Samping Yang Tidak Diinginkan." },
  { "en": "Apa Efek Sidelobe Tinggi?", "id": "Interferensi Ke Arah Lain." },
  { "en": "Apa Kepanjangan TDM Multiplexing?", "id": "Time Division Multiplexing." },
  { "en": "Apa Kepanjangan FDM Multiplexing?", "id": "Frequency Division Multiplexing." },
  { "en": "Apa Kepanjangan WDM Multiplexing?", "id": "Wavelength Division Multiplexing." },
  { "en": "Apa Itu Guard Band Frekuensi?", "id": "Jarak Kosong Antar Kanal." },
  { "en": "Apa Fungsi Guard Band?", "id": "Cegah Interferensi Antar Kanal." },
  { "en": "Apa Kepanjangan PSTN Telepon?", "id": "Public Switched Telephone Network." },
  { "en": "Apa Itu PBX Telepon Kantor?", "id": "Private Branch Exchange." },
  { "en": "Apa Kepanjangan VoIP Suara?", "id": "Voice Over Internet Protocol." },
  { "en": "Apa Protokol SIP VoIP?", "id": "Session Initiation Protocol." },
  { "en": "Apa Kepanjangan RTP Protokol?", "id": "Real Time Transport Protocol." },
  { "en": "Apa Itu Codec Audio?", "id": "Kompresi Dan Dekompresi Suara." },
  { "en": "Apa Contoh Codec G.711?", "id": "PCM Standar Telepon." },
  { "en": "Apa Fungsi Fiber Bragg Grating?", "id": "Filter Panjang Gelombang Optik." },
  { "en": "Apa Prinsip Fiber Bragg Grating?", "id": "Refleksi Bragg Dalam Serat." },
  { "en": "Apa Itu Mode Field Diameter (MFD)?", "id": "Diameter Efektif Cahaya Fiber." },
  { "en": "Apa Itu Numerical Aperture Fiber?", "id": "Kemampuan Terima Sudut Cahaya." },
  { "en": "Apa Itu Macro Bending Loss?", "id": "Rugi Tekukan Besar Kabel." },
  { "en": "Apa Itu Micro Bending Loss?", "id": "Rugi Tekukan Mikroskopis Inti." },
  { "en": "Apa Kepanjangan VRLA Aki?", "id": "Valve Regulated Lead Acid." },
  { "en": "Apa Kelebihan Aki AGM?", "id": "Elektrolit Terserap Glass Mat." },
  { "en": "Apa Kelebihan Aki Gel?", "id": "Elektrolit Bentuk Gel Silika." },
  { "en": "Apa Itu Peukert's Law?", "id": "Kapasitas Baterai Tergantung Arus." },
  { "en": "Apa Rumus Konstanta Peukert?", "id": "Eksponen K Khas Baterai." },
  { "en": "Apa Itu Equalizing Charge?", "id": "Cas Tegangan Tinggi Ratakan Sel." },
  { "en": "Apa Kepanjangan OCV Baterai?", "id": "Open Circuit Voltage." },
  { "en": "Apa Hubungan OCV Dan SOC?", "id": "Indikasi Kapasitas Sisa Baterai." },
  { "en": "Apa Itu Sulfasi Pada Aki?", "id": "Kristal Timbal Sulfat Keras." },
  { "en": "Apa Penyebab Utama Sulfasi?", "id": "Baterai Kosong Terlalu Lama." },
  { "en": "Apa Fungsi Desulfator?", "id": "Pecah Kristal Dengan Pulsa." },
  { "en": "Apa Kepanjangan UPS Online Double Conversion?", "id": "AC DC AC Terus Menerus." },
  { "en": "Apa Kelebihan UPS Online?", "id": "Waktu Pindah Nol Detik." },
  { "en": "Apa Kepanjangan UPS Line Interactive?", "id": "Ada AVR Dan Switch." },
  { "en": "Apa Waktu Pindah UPS Offline?", "id": "Beberapa Milidetik." },
  { "en": "Apa Kode ANSI 50 Relay Proteksi?", "id": "Instantaneous Overcurrent." },
  { "en": "Apa Kode ANSI 51 Relay Proteksi?", "id": "Time Overcurrent." },
  { "en": "Apa Kode ANSI 87 Relay Proteksi?", "id": "Differential Relay." },
  { "en": "Apa Kode ANSI 27 Relay Proteksi?", "id": "Undervoltage Relay." },
  { "en": "Apa Kode ANSI 59 Relay Proteksi?", "id": "Overvoltage Relay." },
  { "en": "Apa Kode ANSI 81 Relay Proteksi?", "id": "Frequency Relay." },
  { "en": "Apa Kode ANSI 67 Relay Proteksi?", "id": "Directional Overcurrent." },
  { "en": "Apa Kode ANSI 21 Relay Proteksi?", "id": "Distance Relay." },
  { "en": "Apa Kode ANSI 63 Relay Trafo?", "id": "Pressure Relief Atau Buchholz." },
  { "en": "Apa Kode ANSI 32 Relay Generator?", "id": "Reverse Power Relay." },
  { "en": "Apa Kode ANSI 40 Relay Generator?", "id": "Loss Of Field." },
  { "en": "Apa Kode ANSI 51N Relay Proteksi?", "id": "Neutral Time Overcurrent." },
  { "en": "Apa Kode ANSI 51G Relay Proteksi?", "id": "Ground Time Overcurrent." },
  { "en": "Apa Fungsi Reclosing Relay (79)?", "id": "Tutup Balik PMT Otomatis." },
  { "en": "Apa Fungsi Lockout Relay (86)?", "id": "Kunci Sistem Setelah Trip." },
  { "en": "Apa Itu Cut In Speed Turbin Angin?", "id": "Kecepatan Awal Hasilkan Daya." },
  { "en": "Apa Itu Cut Out Speed Turbin Angin?", "id": "Kecepatan Henti Demi Keamanan." },
  { "en": "Apa Itu Rated Speed Turbin Angin?", "id": "Kecepatan Daya Nominal Tercapai." },
  { "en": "Apa Fungsi Gearbox Turbin Angin?", "id": "Naikkan RPM Rotor Generator." },
  { "en": "Apa Jenis Generator Turbin Angin Modern?", "id": "DFIG Atau PMSG." },
  { "en": "Apa Kepanjangan DFIG Generator?", "id": "Doubly Fed Induction Generator." },
  { "en": "Apa Kepanjangan PMSG Generator?", "id": "Permanent Magnet Synchronous Generator." },
  { "en": "Apa Kelebihan PMSG Dibanding DFIG?", "id": "Tanpa Gearbox Efisiensi Tinggi." },
  { "en": "Apa Itu Stall Control Turbin Angin?", "id": "Bilah Diam Batasi Aerodinamis." },
  { "en": "Apa Itu Pitch Control Turbin Angin?", "id": "Sudut Bilah Berputar Aktif." },
  { "en": "Apa Itu Fill Factor Sel Surya?", "id": "Ukuran Kualitas Sel PV." },
  { "en": "Apa Nilai Fill Factor Yang Baik?", "id": "Di Atas 0.75." },
  { "en": "Apa Efek Shading Parsial Panel Surya?", "id": "Hotspot Dan Daya Drop." },
  { "en": "Apa Tipe Sel Surya Efisiensi Tertinggi?", "id": "Monokristalin." },
  { "en": "Apa Ciri Fisik Sel Monokristalin?", "id": "Warna Hitam Sudut Terpotong." },
  { "en": "Apa Ciri Fisik Sel Polikristalin?", "id": "Warna Biru Bercak Kotak." },
  { "en": "Apa Itu Sel Surya Thin Film?", "id": "Lapisan Tipis Fleksibel." },
  { "en": "Apa Bahan Sel Surya Thin Film?", "id": "Amorphous Silicon Atau CdTe." },
  { "en": "Apa Itu Perovskite Solar Cell?", "id": "Material Baru Efisiensi Tinggi." },
  { "en": "Apa Fungsi Solar Tracker?", "id": "Ikuti Gerakan Matahari." },
  { "en": "Apa Keuntungan Single Axis Tracker?", "id": "Ikuti Timur Ke Barat." },
  { "en": "Apa Keuntungan Dual Axis Tracker?", "id": "Ikuti Elevasi Dan Azimuth." },
  { "en": "Apa Itu Net Metering PLTS Atap?", "id": "Ekspor Impor Listrik PLN." },
  { "en": "Apa Itu Bi Directional Meter?", "id": "Meteran Listrik Dua Arah." },
  { "en": "Apa Fungsi Schottky Diode?", "id": "Switching Cepat Tegangan Rendah." },
  { "en": "Apa Tegangan Jatuh Schottky Diode?", "id": "0.2 Sampai 0.4 Volt." },
  { "en": "Apa Fungsi Varactor Diode?", "id": "Kapasitor Variabel Tegangan." },
  { "en": "Apa Aplikasi Utama Varactor Diode?", "id": "Tuning Frekuensi VCO." },
  { "en": "Apa Fungsi Tunnel Diode?", "id": "Osilator Frekuensi Microwave." },
  { "en": "Apa Fenomena Resistansi Negatif?", "id": "Tegangan Naik Arus Turun." },
  { "en": "Apa Fungsi TVS Diode?", "id": "Proteksi Tegangan Transien." },
  { "en": "Apa Kepanjangan TVS Komponen?", "id": "Transient Voltage Suppressor." },
  { "en": "Apa Bedanya TVS Dan Zener?", "id": "TVS Respon Lebih Cepat." },
  { "en": "Apa Fungsi Gun Diode?", "id": "Pembangkit Gelombang Mikro." },
  { "en": "Apa Fungsi PIN Diode?", "id": "Saklar RF Frekuensi Tinggi." },
  { "en": "Apa Struktur PIN Diode?", "id": "P Intrinsic N." },
  { "en": "Apa Itu Near Field Antena?", "id": "Medan Dekat Induksi." },
  { "en": "Apa Itu Far Field Antena?", "id": "Medan Jauh Radiasi." },
  { "en": "Apa Itu Antenna Polarization Mismatch?", "id": "Rugi Daya Beda Polarisasi." },
  { "en": "Apa Polarisasi Antena TV UHF?", "id": "Horizontal." },
  { "en": "Apa Polarisasi Antena HT Handy Talky?", "id": "Vertikal." },
  { "en": "Apa Polarisasi Sinyal Satelit Parabola?", "id": "Sirkular Atau Linear." },
  { "en": "Apa Itu LHCP Antena?", "id": "Left Hand Circular Polarization." },
  { "en": "Apa Itu RHCP Antena?", "id": "Right Hand Circular Polarization." },
  { "en": "Apa Fungsi Antena Helical?", "id": "Hasilkan Polarisasi Sirkular." },
  { "en": "Apa Fungsi Antena Patch Microstrip?", "id": "Antena Profil Tipis PCB." },
  { "en": "Apa Itu Phased Array Antenna?", "id": "Arah Pancar Dikontrol Elektronik." },
  { "en": "Apa Keunggulan Phased Array?", "id": "Geser Beam Tanpa Gerak Fisik." },
  { "en": "Apa Aplikasi Phased Array?", "id": "Radar Militer Dan 5G." },
  { "en": "Apa Itu Beamforming 5G?", "id": "Fokus Sinyal Ke User." },
  { "en": "Apa Itu Massive MIMO?", "id": "Ratusan Antena Base Station." },
  { "en": "Apa Kepanjangan IoT Protokol CoAP?", "id": "Constrained Application Protocol." },
  { "en": "Apa Basis Transport CoAP?", "id": "UDP." },
  { "en": "Apa Struktur Data CoAP?", "id": "Mirip HTTP Tapi Ringan." },
  { "en": "Apa Kepanjangan 6LoWPAN?", "id": "IPv6 Over Low Power WPAN." },
  { "en": "Apa Fungsi 6LoWPAN?", "id": "IP Address Untuk Sensor Kecil." },
  { "en": "Apa Itu Mesh Networking?", "id": "Jaringan Laba Laba Saling Sambung." },
  { "en": "Apa Sifat Self Healing Mesh?", "id": "Cari Jalur Baru Jika Putus." },
  { "en": "Apa Itu OTA Update IoT?", "id": "Over The Air Firmware Update." },
  { "en": "Apa Frekuensi Gelombang Mikro Oven?", "id": "2.45 Giga Hertz." },
  { "en": "Kenapa Oven Pakai 2.45 GHz?", "id": "Resonansi Molekul Air." },
  { "en": "Apa Itu Magnetron Oven?", "id": "Tabung Vakum Pembangkit Microwave." },
  { "en": "Apa Tegangan Kerja Magnetron?", "id": "Ribuan Volt DC." },
  { "en": "Apa Fungsi Kapasitor High Voltage Oven?", "id": "Pengganda Tegangan." },
  { "en": "Apa Bahaya Membuka Oven Mikrowave?", "id": "Sisa Muatan Kapasitor Tinggi." },
  { "en": "Apa Itu Crowbar Circuit?", "id": "Short Circuit Saat Overvoltage." },
  { "en": "Apa Komponen Utama Crowbar?", "id": "Thyristor Atau SCR." },
  { "en": "Apa Fungsi Clamp Circuit?", "id": "Geser Level Tegangan Sinyal." },
  { "en": "Apa Fungsi Clipper Circuit?", "id": "Potong Puncak Sinyal." },
  { "en": "Apa Komponen Utama Clipper?", "id": "Dioda Dan Resistor." },
  { "en": "Apa Itu Precision Rectifier?", "id": "Penyearah Tegangan Sangat Kecil." },
  { "en": "Apa Komponen Precision Rectifier?", "id": "Op Amp Dan Dioda." },
  { "en": "Apa Kelemahan Dioda Biasa?", "id": "Tegangan Lutut 0.7 Volt." },
  { "en": "Apa Solusi Dioda Ideal?", "id": "Super Diode Op Amp." },
  { "en": "Apa Itu Current Sense Resistor?", "id": "Resistor Nilai Kecil Ukur Arus." },
  { "en": "Apa Metode Pengukuran Shunt?", "id": "Ukur Tegangan Jatuh Resistor." },
  { "en": "Apa Metode Pengukuran Hall Effect?", "id": "Ukur Medan Magnet Konduktor." },
  { "en": "Apa Kelebihan Sensor Hall Effect?", "id": "Terisolasi Dan Rugi Daya Nol." },
  { "en": "Apa Itu Rogowski Coil?", "id": "Sensor Arus AC Fleksibel." },
  { "en": "Apa Output Rogowski Coil?", "id": "Tegangan Turunan Terhadap Waktu." },
  { "en": "Apa Kelebihan Rogowski Coil?", "id": "Tidak Pernah Jenuh." },
  { "en": "Apa Kepanjangan LED Display?", "id": "Light Emitting Diode." },
  { "en": "Apa Itu Common Anode 7-Segment?", "id": "Kaki Positif Gabung Satu." },
  { "en": "Apa Itu Common Cathode 7-Segment?", "id": "Kaki Negatif Gabung Satu." },
  { "en": "Apa Fungsi Decoder BCD To 7-Segment?", "id": "Ubah Biner Ke Tampilan Angka." },
  { "en": "Apa Itu Multiplexing Display?", "id": "Nyala Bergantian Sangat Cepat." },
  { "en": "Apa Keuntungan Multiplexing Display?", "id": "Hemat Pin Mikrokontroler." },
  { "en": "Apa Masalah Ghosting Pada Display?", "id": "Sisa Bayangan Nyala Redup." },
  { "en": "Apa Definisi Refresh Rate Display?", "id": "Frekuensi Penyegaran Gambar." },
  { "en": "Apa Itu POV Display?", "id": "Persistence Of Vision." },
  { "en": "Apa Prinsip Persistence Of Vision?", "id": "Mata Tahan Bayangan Sesaat." },
  { "en": "Apa Fungsi Shift Register 74HC595?", "id": "Output Serial Ke Paralel." },
  { "en": "Apa Fungsi Shift Register 74HC165?", "id": "Input Paralel Ke Serial." },
  { "en": "Apa Itu Latch Pada Elektronika?", "id": "Kunci Data Sementara." },
  { "en": "Apa Beda Latch Dan Flip-Flop?", "id": "Latch Level, FF Edge Triggered." },
  { "en": "Apa Sifat JK Flip-Flop Kondisi 11?", "id": "Toggle Output." },
  { "en": "Apa Sifat RS Flip-Flop Kondisi 11?", "id": "Terlarang Atau Invalid." },
  { "en": "Apa Fungsi Debounce Circuit Hardware?", "id": "Bersihkan Sinyal Saklar Mekanis." },
  { "en": "Apa Komponen Debounce Sederhana?", "id": "Kapasitor Dan Resistor." },
  { "en": "Apa Itu Schmitt Trigger Input?", "id": "Input Histeresis Tahan Noise." },
  { "en": "Apa Simbol Gerbang NAND?", "id": "AND Dengan Lingkaran Output." },
  { "en": "Apa Simbol Gerbang NOR?", "id": "OR Dengan Lingkaran Output." },
  { "en": "Apa Sifat Universal Gate NAND?", "id": "Bisa Bentuk Semua Gerbang." },
  { "en": "Apa Sifat Universal Gate NOR?", "id": "Bisa Bentuk Semua Gerbang." },
  { "en": "Apa Hukum De Morgan Pertama?", "id": "NAND Sama Dengan Bubbled OR." },
  { "en": "Apa Hukum De Morgan Kedua?", "id": "NOR Sama Dengan Bubbled AND." },
  { "en": "Apa Itu Karnaugh Map 4 Variabel?", "id": "Tabel Grid 16 Kotak." },
  { "en": "Apa Fungsi Gray Code?", "id": "Hanya Satu Bit Berubah." },
  { "en": "Apa Aplikasi Gray Code?", "id": "Encoder Posisi Putar." },
  { "en": "Apa Itu Half Duplex Komunikasi?", "id": "Dua Arah Bergantian." },
  { "en": "Apa Itu Full Duplex Komunikasi?", "id": "Dua Arah Bersamaan." },
  { "en": "Apa Itu Simplex Komunikasi?", "id": "Satu Arah Saja." },
  { "en": "Apa Contoh Komunikasi Simplex?", "id": "Radio Siaran Atau TV." },
  { "en": "Apa Contoh Komunikasi Half Duplex?", "id": "Walkie Talkie." },
  { "en": "Apa Contoh Komunikasi Full Duplex?", "id": "Telepon Seluler." },
  { "en": "Apa Standar RS485 Komunikasi?", "id": "Serial Diferensial Jarak Jauh." },
  { "en": "Apa Keunggulan RS485 Dibanding RS232?", "id": "Tahan Noise Multipoint." },
  { "en": "Berapa Jumlah Node Maksimal RS485?", "id": "32 Unit Load Standar." },
  { "en": "Apa Fungsi Terminator Resistor RS485?", "id": "Cegah Pantulan Sinyal." },
  { "en": "Berapa Nilai Resistor Terminasi RS485?", "id": "120 Ohm." },
  { "en": "Apa Itu Differential Signaling?", "id": "Kirim Data Beda Tegangan." },
  { "en": "Apa Keuntungan Differential Signaling?", "id": "Kebal Noise Common Mode." },
  { "en": "Apa Standar Tegangan USB 5V?", "id": "5 Volt DC." },
  { "en": "Apa Arus Maksimal USB 2.0?", "id": "500 Mili Ampere." },
  { "en": "Apa Arus Maksimal USB 3.0?", "id": "900 Mili Ampere." },
  { "en": "Apa Kabel Data USB Warna Hijau?", "id": "Data Plus." },
  { "en": "Apa Kabel Data USB Warna Putih?", "id": "Data Minus." },
  { "en": "Apa Fungsi Pin ID USB OTG?", "id": "Deteksi Host Atau Device." },
  { "en": "Apa Kepanjangan OTG Pada USB?", "id": "On The Go." },
  { "en": "Apa Itu Protokol Master Slave?", "id": "Satu Pengendali Banyak Pengikut." },
  { "en": "Apa Itu Address Bus?", "id": "Jalur Alamat Memori." },
  { "en": "Apa Itu Data Bus?", "id": "Jalur Transfer Data." },
  { "en": "Apa Itu Control Bus?", "id": "Jalur Sinyal Kendali." },
  { "en": "Apa Lebar Data Bus 8-Bit?", "id": "8 Jalur Paralel." },
  { "en": "Apa Lebar Alamat Bus 16-Bit?", "id": "65536 Alamat." },
  { "en": "Apa Fungsi Memory Map?", "id": "Peta Alokasi Alamat Memori." },
  { "en": "Apa Itu Interrupt Service Routine (ISR)?", "id": "Kode Penanganan Interupsi." },
  { "en": "Apa Sifat Fungsi ISR?", "id": "Harus Singkat Dan Cepat." },
  { "en": "Apa Itu Polling Dalam Pemrograman?", "id": "Cek Status Berulang Ulang." },
  { "en": "Apa Kelemahan Metode Polling?", "id": "Boros Waktu Prosesor." },
  { "en": "Apa Kelebihan Metode Interrupt?", "id": "Prosesor Bisa Kerjakan Lain." },
  { "en": "Apa Itu Atomic Operation?", "id": "Operasi Tak Bisa Disela." },
  { "en": "Apa Fungsi Atomic Block?", "id": "Cegah Korupsi Data Bersama." },
  { "en": "Apa Itu Race Condition Software?", "id": "Hasil Tergantung Urutan Eksekusi." },
  { "en": "Apa Definisi Bandgap Reference?", "id": "Sumber Tegangan Referensi Stabil." },
  { "en": "Berapa Tegangan Bandgap Silikon?", "id": "Sekitar 1.2 Volt." },
  { "en": "Apa Fungsi Voltage Regulator Linear?", "id": "Buang Kelebihan Tegangan Panas." },
  { "en": "Apa Efisiensi Regulator Linear?", "id": "Rendah Jika Beda Besar." },
  { "en": "Apa Kepanjangan LDO Regulator?", "id": "Low Drop Out." },
  { "en": "Apa Kelebihan LDO Regulator?", "id": "Beda Tegangan Masuk Kecil." },
  { "en": "Apa Fungsi Kapasitor Input Regulator?", "id": "Stabilkan Suplai Masuk." },
  { "en": "Apa Fungsi Kapasitor Output Regulator?", "id": "Cegah Osilasi Respon Beban." },
  { "en": "Apa Masalah ESR Kapasitor Tinggi?", "id": "Ripple Tegangan Meningkat." },
  { "en": "Apa Kepanjangan ESR Kapasitor?", "id": "Equivalent Series Resistance." },
  { "en": "Apa Kepanjangan ESL Kapasitor?", "id": "Equivalent Series Inductance." },
  { "en": "Apa Pengaruh ESL Pada Frekuensi Tinggi?", "id": "Kapasitor Bersifat Induktif." },
  { "en": "Apa Itu Self Resonant Frequency Kapasitor?", "id": "Batas Sifat Kapasitif." },
  { "en": "Apa Jenis Kapasitor Frekuensi Tinggi Terbaik?", "id": "Keramik NPO Atau C0G." },
  { "en": "Apa Jenis Kapasitor Audio Terbaik?", "id": "Film Polypropylene." },
  { "en": "Apa Jenis Kapasitor Power Supply Besar?", "id": "Elektrolit Aluminium." },
  { "en": "Apa Sifat Kapasitor Tantalum?", "id": "Kapasitas Besar Ukuran Kecil." },
  { "en": "Apa Bahaya Kapasitor Tantalum?", "id": "Meledak Jika Tegangan Balik." },
  { "en": "Apa Itu Dielectric Absorption?", "id": "Kapasitor Terisi Sendiri." },
  { "en": "Apa Istilah Lain Dielectric Absorption?", "id": "Memory Effect Kapasitor." },
  { "en": "Apa Itu Superheterodyne Receiver?", "id": "Campur Frekuensi Ke Intermediate." },
  { "en": "Apa Kepanjangan IF Radio?", "id": "Intermediate Frequency." },
  { "en": "Berapa IF Standar Radio AM?", "id": "455 Kilo Hertz." },
  { "en": "Berapa IF Standar Radio FM?", "id": "10.7 Mega Hertz." },
  { "en": "Apa Fungsi Mixer Pada Radio?", "id": "Campur RF Dan Osilator." },
  { "en": "Apa Sinyal Output Mixer?", "id": "Jumlah Dan Selisih Frekuensi." },
  { "en": "Apa Itu Image Frequency?", "id": "Frekuensi Bayangan Pengganggu." },
  { "en": "Apa Fungsi AGC Pada Radio?", "id": "Stabilkan Volume Output." },
  { "en": "Apa Kepanjangan AGC Radio?", "id": "Automatic Gain Control." },
  { "en": "Apa Itu Squelch Pada Radio?", "id": "Bisukan Noise Tanpa Sinyal." },
  { "en": "Apa Definisi Selectivity Radio?", "id": "Kemampuan Pilih Satu Frekuensi." },
  { "en": "Apa Definisi Sensitivity Radio?", "id": "Kemampuan Terima Sinyal Lemah." },
  { "en": "Apa Definisi Fidelity Radio?", "id": "Akurasi Reproduksi Suara Asli." },
  { "en": "Apa Fungsi Discriminator FM?", "id": "Demodulasi Sinyal FM." },
  { "en": "Apa Fungsi Envelope Detector AM?", "id": "Demodulasi Sinyal AM." },
  { "en": "Apa Komponen Envelope Detector?", "id": "Dioda Dan Kapasitor Paralel." },
  { "en": "Apa Itu Pre-Emphasis FM?", "id": "Kuatkan Nada Tinggi Pemancar." },
  { "en": "Apa Itu De Emphasis Pada Penerima FM?", "id": "Redam Noise Frekuensi Tinggi." },
  { "en": "Apa Karakteristik Respon Filter Butterworth?", "id": "Passband Datar Maksimal." },
  { "en": "Apa Karakteristik Respon Filter Chebyshev?", "id": "Ripple Passband Roll Off Curam." },
  { "en": "Apa Karakteristik Respon Filter Bessel?", "id": "Fasa Linear Delay Konstan." },
  { "en": "Apa Kelemahan Filter Chebyshev?", "id": "Fasa Tidak Linear." },
  { "en": "Apa Itu Order Filter?", "id": "Jumlah Komponen Reaktif Penyusun." },
  { "en": "Berapa Kemiringan Roll Off Filter Orde 1?", "id": "Minus 20 Decibel Per Dekade." },
  { "en": "Berapa Kemiringan Roll Off Filter Orde 2?", "id": "Minus 40 Decibel Per Dekade." },
  { "en": "Apa Fungsi Rangkaian Sallen Key?", "id": "Topologi Filter Aktif Populer." },
  { "en": "Apa Syarat Osilasi Jembatan Wien?", "id": "Gain Penguat Harus Tiga." },
  { "en": "Apa Komponen Penentu Frekuensi Kristal?", "id": "Potongan Bahan Piezoelektrik." },
  { "en": "Apa Rangkaian Ekuivalen Kristal Quartz?", "id": "RLC Seri Paralel Kapasitor." },
  { "en": "Apa Itu Komponen Simetris Sistem Tenaga?", "id": "Analisis Fasa Tidak Seimbang." },
  { "en": "Apa Itu Urutan Positif (Positive Sequence)?", "id": "Rotasi Normal A B C." },
  { "en": "Apa Itu Urutan Negatif (Negative Sequence)?", "id": "Rotasi Terbalik A C B." },
  { "en": "Apa Itu Urutan Nol (Zero Sequence)?", "id": "Tiga Fasa Segaris." },
  { "en": "Apa Penyebab Munculnya Urutan Negatif?", "id": "Beban Tidak Seimbang." },
  { "en": "Apa Penyebab Munculnya Urutan Nol?", "id": "Gangguan Ke Tanah." },
  { "en": "Apa Jenis Gangguan Paling Sering Terjadi?", "id": "Satu Fasa Ke Tanah." },
  { "en": "Apa Jenis Gangguan Arus Terbesar?", "id": "Tiga Fasa Hubung Singkat." },
  { "en": "Apa Hubungan Tegangan Fasa Dan Line Star?", "id": "Vline Akar Tiga Vfasa." },
  { "en": "Apa Hubungan Arus Fasa Dan Line Delta?", "id": "Iline Akar Tiga Ifasa." },
  { "en": "Apa Fungsi Grounding Resistor (NGR)?", "id": "Batasi Arus Gangguan Tanah." },
  { "en": "Apa Kepanjangan NGR Sistem Tenaga?", "id": "Neutral Grounding Resistor." },
  { "en": "Apa Definisi Setup Time Flip Flop?", "id": "Data Stabil Sebelum Clock." },
  { "en": "Apa Definisi Hold Time Flip Flop?", "id": "Data Stabil Setelah Clock." },
  { "en": "Apa Akibat Pelanggaran Setup Time?", "id": "Metastability Output." },
  { "en": "Apa Perbedaan Mesin Mealy?", "id": "Output Tergantung State Input." },
  { "en": "Apa Perbedaan Mesin Moore?", "id": "Output Tergantung State Saja." },
  { "en": "Apa Itu State Diagram?", "id": "Visualisasi Perpindahan Logika." },
  { "en": "Apa Itu One Hot Encoding?", "id": "Satu Bit High Per State." },
  { "en": "Apa Kelebihan One Hot Encoding?", "id": "Dekoding Cepat Logika Sederhana." },
  { "en": "Apa Fungsi Schmitt Trigger Digital?", "id": "Perbaiki Sinyal Tepi Rusak." },
  { "en": "Apa Itu Clock Skew?", "id": "Beda Waktu Tiba Clock." },
  { "en": "Apa Masalah Ripple Carry Adder?", "id": "Lambat Karena Carry Merambat." },
  { "en": "Apa Solusi Untuk Ripple Carry Adder?", "id": "Look Ahead Carry Adder." },
  { "en": "Apa Fungsi Siklokonverter (Cycloconverter)?", "id": "Ubah Frekuensi AC Langsung." },
  { "en": "Apa Fungsi Matrix Converter?", "id": "AC Ke AC Tanpa DC Link." },
  { "en": "Apa Sudut Penyalaan (Firing Angle) SCR?", "id": "Sudut Tunda Konduksi." },
  { "en": "Apa Sudut Pemadaman (Extinction Angle)?", "id": "Waktu Off Sebelum Tegangan Maju." },
  { "en": "Apa Masalah Commutation Failure?", "id": "Gagal Matikan Thyristor." },
  { "en": "Apa Itu Dual Converter?", "id": "Dua Bridge Arah Berlawanan." },
  { "en": "Apa Fungsi Freewheeling Diode Motor?", "id": "Jalur Arus Sisa Induktif." },
  { "en": "Apa Itu Reaksi Jangkar Mesin DC?", "id": "Distorsi Fluks Utama Stator." },
  { "en": "Apa Akibat Reaksi Jangkar?", "id": "Bunga Api Pada Sikat." },
  { "en": "Apa Fungsi Kutub Bantu (Interpole)?", "id": "Perbaiki Komutasi Kurangi Bunga Api." },
  { "en": "Apa Fungsi Belitan Kompensasi?", "id": "Netralisir Reaksi Jangkar Beban Besar." },
  { "en": "Apa Itu Rugi Histeresis Mesin?", "id": "Energi Balik Arah Domain Magnet." },
  { "en": "Apa Itu Rugi Eddy Current Mesin?", "id": "Arus Sirkulasi Inti Besi." },
  { "en": "Cara Mengurangi Rugi Eddy Current?", "id": "Inti Besi Dibuat Berlapis." },
  { "en": "Cara Mengurangi Rugi Histeresis?", "id": "Gunakan Bahan Besi Lunak." },
  { "en": "Apa Kelas Isolasi F Suhu Maksimal?", "id": "155 Derajat Celcius." },
  { "en": "Apa Kelas Isolasi H Suhu Maksimal?", "id": "180 Derajat Celcius." },
  { "en": "Apa Kelas Isolasi Y Suhu Maksimal?", "id": "90 Derajat Celcius." },
  { "en": "Apa Metode Tuning PID Ziegler Nichols?", "id": "Osilasi Batas Gain Kritis." },
  { "en": "Apa Itu Gain Margin Bode Plot?", "id": "Jarak Magnitude Ke 0dB." },
  { "en": "Apa Itu Phase Margin Bode Plot?", "id": "Jarak Fasa Ke 180 Derajat." },
  { "en": "Apa Syarat Kestabilan Nyquist?", "id": "Tidak Lingkupi Titik Minus Satu." },
  { "en": "Apa Itu Root Locus?", "id": "Jejak Akar Terhadap Gain." },
  { "en": "Apa Jika Root Locus Di Kanan?", "id": "Sistem Tidak Stabil." },
  { "en": "Apa Fungsi Feedforward Control?", "id": "Antisipasi Gangguan Sebelum Error." },
  { "en": "Apa Kelemahan Feedforward Control?", "id": "Butuh Model Sistem Akurat." },
  { "en": "Apa Itu Dead Time Sistem?", "id": "Tunda Respon Setelah Input." },
  { "en": "Apa Efek Dead Time Pada Kontrol?", "id": "Kurangi Stabilitas Sistem." },
  { "en": "Apa Kepanjangan SCADA Master Station?", "id": "Pusat Kendali Dan Monitoring." },
  { "en": "Apa Fungsi Remote Terminal Unit (RTU)?", "id": "Kumpul Data Lapangan Ke Master." },
  { "en": "Apa Protokol DNP3 Digunakan Dimana?", "id": "Otomasi Gardu Listrik." },
  { "en": "Apa Keunggulan Protokol DNP3?", "id": "Timestamping Dan Klasifikasi Data." },
  { "en": "Apa Kepanjangan IEC 60870-5-104?", "id": "Protokol Telecontrol Berbasis TCP IP." },
  { "en": "Apa Itu Tagging Pada SCADA?", "id": "Tanda Larangan Operasi Alat." },
  { "en": "Apa Fungsi Historian Server?", "id": "Simpan Data Sejarah Operasi." },
  { "en": "Apa Itu Bandgap Reference Voltage?", "id": "Tegangan Stabil Terhadap Suhu." },
  { "en": "Apa Tegangan Bandgap Silicon?", "id": "Sekitar 1.25 Volt." },
  { "en": "Apa Fungsi Current Limiting Resistor?", "id": "Batasi Arus Ke Beban." },
  { "en": "Apa Itu Bleeder Resistor?", "id": "Kuras Muatan Kapasitor Sisa." },
  { "en": "Apa Fungsi Ballast Resistor?", "id": "Stabilkan Arus Rangkaian Paralel." },
  { "en": "Apa Itu Zero Ohm Resistor?", "id": "Jumper Berbentuk Resistor." },
  { "en": "Apa Fungsi Zero Ohm Resistor?", "id": "Jembatan Jalur PCB." },
  { "en": "Apa Sifat Termistor NTC Inrush?", "id": "Dingin Tinggi Panas Rendah." },
  { "en": "Apa Fungsi NTC Pada Power Supply?", "id": "Cegah Lonjakan Arus Start." },
  { "en": "Apa Itu Parasitic Inductance?", "id": "Induktansi Kaki Komponen." },
  { "en": "Apa Itu Parasitic Capacitance?", "id": "Kapasitansi Antar Jalur." },
  { "en": "Apa Efek Parasitik Frekuensi Tinggi?", "id": "Ubah Karakteristik Impedansi." },
  { "en": "Apa Itu Via Stitched Shielding?", "id": "Pagar Via Kelilingi Sinyal." },
  { "en": "Apa Fungsi Teardrop Pada Pad PCB?", "id": "Kuatkan Sambungan Jalur." },
  { "en": "Apa Itu Annular Ring PCB?", "id": "Cincin Tembaga Lubang Bor." },
  { "en": "Apa Itu DRC Dalam Desain PCB?", "id": "Design Rule Check." },
  { "en": "Apa Itu ERC Dalam Desain Skematik?", "id": "Electrical Rule Check." },
  { "en": "Apa Itu BOM Dalam Manufaktur?", "id": "Bill Of Materials." },
  { "en": "Apa Itu Gerber File?", "id": "Format Standar Cetak PCB." },
  { "en": "Apa Fungsi Pick And Place Machine?", "id": "Pasang Komponen SMD Otomatis." },
  { "en": "Apa Itu AOI Inspection?", "id": "Automated Optical Inspection." },
  { "en": "Apa Kepanjangan QFN Package?", "id": "Quad Flat No Leads." },
  { "en": "Apa Kepanjangan BGA Package?", "id": "Ball Grid Array." },
  { "en": "Apa Kesulitan Utama Soldering BGA?", "id": "Kaki Tidak Terlihat Mata." },
  { "en": "Apa Metode Inspeksi Kaki BGA?", "id": "Sinar X Ray." },
  { "en": "Apa Itu Reballing BGA?", "id": "Pasang Ulang Bola Timah." },
  { "en": "Apa Fungsi Kapasitor Bypass Pada IC?", "id": "Hubung Singkat Noise Ke Ground." },
  { "en": "Apa Fungsi Kapasitor Coupling Audio?", "id": "Blokir DC Loloskan Sinyal AC." },
  { "en": "Apa Efek Kapasitor Kopling Terlalu Kecil?", "id": "Bass Atau Frekuensi Rendah Hilang." },
  { "en": "Apa Itu Miller Effect Pada Op-Amp?", "id": "Kompensasi Frekuensi Internal Stabil." },
  { "en": "Apa Kepanjangan PSRR Op-Amp?", "id": "Power Supply Rejection Ratio." },
  { "en": "Apa Definisi PSRR?", "id": "Kemampuan Tolak Noise Power Supply." },
  { "en": "Apa Itu Input Offset Voltage?", "id": "Tegangan Beda Input Nol Output." },
  { "en": "Apa Itu Input Bias Current?", "id": "Arus Masuk Basis Transistor Input." },
  { "en": "Apa Fungsi Pin 555 Timer Reset?", "id": "Paksa Output Low Jika Ground." },
  { "en": "Apa Fungsi Pin 555 Control Voltage?", "id": "Atur Ambang Batas Komparator." },
  { "en": "Apa Kepanjangan ARINC Avionik?", "id": "Aeronautical Radio Incorporated." },
  { "en": "Apa Standar Bus Data Pesawat Komersial?", "id": "ARINC 429." },
  { "en": "Apa Format Data ARINC 429?", "id": "Bipolar Return To Zero." },
  { "en": "Apa Kepanjangan LIN Bus Otomotif?", "id": "Local Interconnect Network." },
  { "en": "Apa Perbedaan LIN Dan CAN Bus?", "id": "LIN Lebih Lambat Dan Murah." },
  { "en": "Apa Fungsi ECU Pada Mobil?", "id": "Electronic Control Unit." },
  { "en": "Apa Protokol OBD-II Otomotif?", "id": "On Board Diagnostics Generasi Dua." },
  { "en": "Apa Letak Port OBD-II Standar?", "id": "Bawah Dasbor Sisi Pengemudi." },
  { "en": "Apa Sinyal EOG Bio-instrumentasi?", "id": "Electrooculogram Gerakan Mata." },
  { "en": "Apa Sinyal ERG Bio-instrumentasi?", "id": "Electroretinogram Respon Retina." },
  { "en": "Apa Fungsi Defibrillator Jantung?", "id": "Reset Irama Jantung Kejut Listrik." },
  { "en": "Apa Bentuk Gelombang Defibrillator Bifasik?", "id": "Arus Bolak Balik Dua Arah." },
  { "en": "Apa Bahaya Arus Bocor Mikro Medis?", "id": "Fibrilasi Ventrikel Jantung." },
  { "en": "Apa Standar Keselamatan Alat Medis?", "id": "IEC 60601." },
  { "en": "Apa Fungsi Isolation Transformer Medis?", "id": "Pisahkan Pasien Dari Ground PLN." },
  { "en": "Apa Itu Hardware Security Module (HSM)?", "id": "Prosesor Kriptografi Fisik Aman." },
  { "en": "Apa Fungsi TPM Pada Motherboard?", "id": "Trusted Platform Module Enkripsi." },
  { "en": "Apa Algoritma Enkripsi Simetris Standar?", "id": "AES Advanced Encryption Standard." },
  { "en": "Apa Algoritma Enkripsi Asimetris Standar?", "id": "RSA Rivest Shamir Adleman." },
  { "en": "Apa Itu Side Channel Attack?", "id": "Analisis Konsumsi Daya Chip." },
  { "en": "Apa Fungsi Physical Unclonable Function (PUF)?", "id": "Sidik Jari Unik Silikon Chip." },
  { "en": "Apa Impedansi Kabel Coaxial RG6?", "id": "75 Ohm." },
  { "en": "Apa Aplikasi Kabel Coaxial RG6?", "id": "Antena TV Dan CCTV." },
  { "en": "Apa Impedansi Kabel Coaxial RG58?", "id": "50 Ohm." },
  { "en": "Apa Aplikasi Kabel Coaxial RG58?", "id": "Radio Komunikasi Dan Wi-Fi." },
  { "en": "Apa Impedansi Kabel Twisted Pair Cat5?", "id": "100 Ohm." },
  { "en": "Apa Fungsi Shielding Pada Kabel STP?", "id": "Lindungi Dari EMI Eksternal." },
  { "en": "Apa Masalah Skew Pada Kabel Paralel?", "id": "Bit Datang Tidak Bersamaan." },
  { "en": "Apa Itu Crosstalk NEXT?", "id": "Near End Crosstalk." },
  { "en": "Apa Itu Crosstalk FEXT?", "id": "Far End Crosstalk." },
  { "en": "Apa Itu Gain Scheduling Control?", "id": "Ubah Parameter PID Sesuai Kondisi." },
  { "en": "Apa Itu Adaptive Control?", "id": "Kontroler Menyesuaikan Diri Otomatis." },
  { "en": "Apa Itu Fuzzy Logic Control?", "id": "Logika Samar Nilai Antara." },
  { "en": "Apa Himpunan Keanggotaan Fuzzy?", "id": "Derajat Kebenaran Nol Sampai Satu." },
  { "en": "Apa Proses Defuzzifikasi?", "id": "Ubah Nilai Samar Ke Tegas." },
  { "en": "Apa Itu Neural Network Control?", "id": "Pembelajaran Mesin Tiru Otak." },
  { "en": "Apa Lapisan Tersembunyi (Hidden Layer)?", "id": "Antara Input Dan Output Neuron." },
  { "en": "Apa Fungsi Aktivasi Sigmoid?", "id": "Ubah Output Jadi 0 Sampai 1." },
  { "en": "Apa Itu Deadband Pada Sensor?", "id": "Rentang Input Tanpa Respon Output." },
  { "en": "Apa Itu Span Pada Alat Ukur?", "id": "Selisih Nilai Maksimum Dan Minimum." },
  { "en": "Apa Itu Rangeability Sensor?", "id": "Rasio Sinyal Maksimum Minimum Terukur." },
  { "en": "Apa Itu Response Time T90?", "id": "Waktu Capai 90 Persen Nilai." },
  { "en": "Apa Prinsip Sensor PIR?", "id": "Deteksi Perubahan Panas Inframerah." },
  { "en": "Apa Kepanjangan PIR Sensor?", "id": "Passive Infrared Receiver." },
  { "en": "Apa Fungsi Lensa Fresnel PIR?", "id": "Fokuskan Sinar Ke Sensor." },
  { "en": "Apa Output Sensor Ultrasonic?", "id": "Pulsa Durasi Waktu Pantul." },
  { "en": "Apa Rumus Jarak Ultrasonic?", "id": "Waktu Kali Kecepatan Suara Bagi Dua." },
  { "en": "Apa Kecepatan Suara Di Udara?", "id": "340 Meter Per Detik." },
  { "en": "Apa Itu Blind Zone Sensor?", "id": "Jarak Terdekat Tidak Terdeteksi." },
  { "en": "Apa Masalah Crosstalk Sensor Ultrasonic?", "id": "Interferensi Gelombang Sensor Lain." },
  { "en": "Apa Itu Layar E-Ink?", "id": "Tinta Elektronik Elektroforesis." },
  { "en": "Apa Kelebihan Layar E-Ink?", "id": "Hemat Daya Hanya Saat Ganti." },
  { "en": "Apa Itu Layar TFT LCD?", "id": "Thin Film Transistor Liquid Crystal." },
  { "en": "Apa Itu Dead Pixel?", "id": "Piksel Mati Hitam Atau Putih." },
  { "en": "Apa Itu Burn In Layar OLED?", "id": "Gambar Hantu Permanen Akibat Umur." },
  { "en": "Apa Definisi Candela Per Meter Persegi?", "id": "Satuan Luminansi Atau Nit." },
  { "en": "Apa Hukum Kuadrat Terbalik Cahaya?", "id": "Intensitas Turun Kuadrat Jarak." },
  { "en": "Apa Efek Flicker Cahaya Pada Mata?", "id": "Kelelahan Mata Dan Sakit Kepala." },
  { "en": "Apa Kepanjangan SVM Mesin Learning?", "id": "Support Vector Machine." },
  { "en": "Apa Fungsi Kernel SVM?", "id": "Petakan Data Ke Dimensi Tinggi." },
  { "en": "Apa Itu Convolutional Neural Network (CNN)?", "id": "Jaringan Syaraf Olah Gambar." },
  { "en": "Apa Fungsi Pooling Layer CNN?", "id": "Kurangi Dimensi Data Fitur." },
  { "en": "Apa Fungsi Dropout Neural Network?", "id": "Cegah Overfitting Matikan Neuron Acak." },
  { "en": "Apa Itu Overfitting Model AI?", "id": "Hafal Data Latih Gagal Tes." },
  { "en": "Apa Itu Underfitting Model AI?", "id": "Model Terlalu Sederhana Eror Tinggi." },
  { "en": "Apa Kepanjangan HASL PCB Finish?", "id": "Hot Air Solder Leveling." },
  { "en": "Apa Kelebihan ENIG PCB Finish?", "id": "Permukaan Emas Rata Tahan Oksidasi." },
  { "en": "Apa Kepanjangan ENIG PCB?", "id": "Electroless Nickel Immersion Gold." },
  { "en": "Apa Itu Fiducial Mark PCB?", "id": "Titik Referensi Mesin Pick Place." },
  { "en": "Apa Itu Mouse Bites PCB?", "id": "Lubang Kecil Pemisah Panel PCB." },
  { "en": "Apa Fungsi Panelizing PCB?", "id": "Gabung Banyak PCB Satu Papan." },
  { "en": "Apa Itu V-Cut PCB?", "id": "Goresan Pemisah Panel PCB." },
  { "en": "Apa Definisi Kualitas Daya 9s?", "id": "99.9999999 Persen Ketersediaan." },
  { "en": "Apa Itu Isochronous Governor?", "id": "Frekuensi Tetap Beban Berubah." },
  { "en": "Apa Itu Droop Governor?", "id": "Frekuensi Turun Beban Naik." },
  { "en": "Apa Fungsi Load Sharing Generator?", "id": "Bagi Beban Sesuai Kapasitas." },
  { "en": "Apa Syarat Paralel Generator Sinkron?", "id": "Tegangan Frekuensi Fasa Urutan Sama." },
  { "en": "Apa Alat Cek Sinkronisasi Manual?", "id": "Lampu Sinkronoskop." },
  { "en": "Apa Indikasi Lampu Gelap Sempurna?", "id": "Beda Tegangan Nol Siap Paralel." },
  { "en": "Apa Itu Cogeneration (CHP)?", "id": "Hasilkan Listrik Dan Panas Bersama." },
  { "en": "Apa Kepanjangan CHP Pembangkit?", "id": "Combined Heat And Power." },
  { "en": "Apa Kelebihan Combined Cycle?", "id": "Efisiensi Termal Sangat Tinggi." },
  { "en": "Apa Bahan Bakar PLTN?", "id": "Uranium 235." },
  { "en": "Apa Fungsi Moderator Reaktor Nuklir?", "id": "Lambatkan Neutron Cepat." },
  { "en": "Apa Fungsi Batang Kendali Nuklir?", "id": "Serap Neutron Hentikan Reaksi." },
  { "en": "Apa Jenis Reaktor PWR?", "id": "Pressurized Water Reactor." },
  { "en": "Apa Jenis Reaktor BWR?", "id": "Boiling Water Reactor." },
  { "en": "Apa Kepanjangan HMI Pada Sistem Otomasi?", "id": "Human Machine Interface." },
  { "en": "Apa Perbedaan Protokol Modbus RTU Dan TCP?", "id": "RTU Serial, TCP Ethernet." },
  { "en": "Apa Tipe Kabel Standar Modbus RTU?", "id": "Twisted Pair Shielded." },
  { "en": "Apa Kepanjangan Profibus Dalam Industri?", "id": "Process Field Bus." },
  { "en": "Apa Kecepatan Maksimum Profibus DP?", "id": "12 Mega Bit Per Detik." },
  { "en": "Apa Fungsi Terminator Pada Bus Profibus?", "id": "Cegah Pantulan Sinyal Ujung Kabel." },
  { "en": "Apa Tegangan Bias Pada RS485?", "id": "Jaga Logika Saat Bus Idle." },
  { "en": "Apa Itu Token Ring Network?", "id": "Hak Kirim Data Bergilir." },
  { "en": "Apa Kepanjangan OPC Server Industri?", "id": "Open Platform Communications." },
  { "en": "Apa Fungsi OPC UA?", "id": "Interkoneksi Mesin Beda Merek." },
  { "en": "Apa Kepanjangan UA Pada OPC UA?", "id": "Unified Architecture." },
  { "en": "Apa Itu Industrial Ethernet?", "id": "Ethernet Tahan Lingkungan Keras." },
  { "en": "Apa Kepanjangan PoE Switch?", "id": "Power Over Ethernet." },
  { "en": "Apa Standar Tegangan PoE 802.3af?", "id": "48 Volt DC." },
  { "en": "Apa Fungsi VLAN Pada Switch?", "id": "Pisah Jaringan Secara Logika." },
  { "en": "Apa Kepanjangan VLAN Jaringan?", "id": "Virtual Local Area Network." },
  { "en": "Apa Fungsi Spanning Tree Protocol (STP)?", "id": "Cegah Loop Pada Jaringan Switch." },
  { "en": "Apa Itu Broadcast Storm?", "id": "Banjir Paket Data Lumpuhkan Jaringan." },
  { "en": "Apa Kepanjangan MTU Jaringan?", "id": "Maximum Transmission Unit." },
  { "en": "Berapa Ukuran MTU Standar Ethernet?", "id": "1500 Byte." },
  { "en": "Apa Fungsi Transient Recovery Voltage (TRV)?", "id": "Tegangan Muncul Saat Breaker Buka." },
  { "en": "Apa Itu Rate Of Rise Of TRV?", "id": "Kecepatan Kenaikan Tegangan Balik." },
  { "en": "Apa Fungsi Grading Capacitor Pada CB?", "id": "Ratakan Distribusi Tegangan TRV." },
  { "en": "Apa Kepanjangan PMT Pemutus Tenaga?", "id": "Pemutus Tenaga." },
  { "en": "Apa Kepanjangan PMS Pemisah?", "id": "Pemisah Saklar." },
  { "en": "Apa Syarat Operasi PMS?", "id": "Tanpa Beban Arus." },
  { "en": "Apa Fungsi Interlock PMS Dan PMT?", "id": "Cegah Kesalahan Urutan Operasi." },
  { "en": "Apa Itu Arc Chute Pada Breaker?", "id": "Ruang Pemadam Busur Api." },
  { "en": "Apa Fungsi Blowout Coil?", "id": "Tiup Busur Api Ke Chute." },
  { "en": "Apa Sifat Gas SF6 Saat Terbakar?", "id": "Tidak Beracun Tapi Mendesak Oksigen." },
  { "en": "Apa Produk Dekomposisi SF6 Berbahaya?", "id": "Bubuk Putih Beracun." },
  { "en": "Apa Tekanan Gas SF6 Pada GIS?", "id": "3 Sampai 5 Bar." },
  { "en": "Apa Itu Restricted Earth Fault (REF)?", "id": "Proteksi Gangguan Tanah Zona Terbatas." },
  { "en": "Apa Syarat Pemasangan CT REF?", "id": "Satu Di Netral Tiga Fasa." },
  { "en": "Apa Itu High Impedance Differential?", "id": "Relay Diferensial Tahan Saturasi CT." },
  { "en": "Apa Fungsi Metrosil Pada Relay?", "id": "Potong Tegangan Puncak Berlebih." },
  { "en": "Apa Itu CT Knee Point Voltage?", "id": "Titik Mulai Jenuh Inti CT." },
  { "en": "Apa Akibat Burden CT Terlalu Besar?", "id": "CT Cepat Jenuh Akurasi Turun." },
  { "en": "Apa Itu Negative Sequence Relay?", "id": "Deteksi Beban Tidak Seimbang." },
  { "en": "Apa Bahaya Arus Urutan Negatif?", "id": "Panaskan Rotor Generator." },
  { "en": "Apa Kepanjangan PMU Sistem Tenaga?", "id": "Phasor Measurement Unit." },
  { "en": "Apa Fungsi WAMS Sistem Tenaga?", "id": "Monitor Area Luas Real Time." },
  { "en": "Apa Kepanjangan WAMS?", "id": "Wide Area Monitoring System." },
  { "en": "Apa Itu Synchrophasor?", "id": "Pengukuran Fasa Terkalibrasi GPS." },
  { "en": "Apa Kepanjangan PCB Trace Impedance?", "id": "Impedansi Jalur Tembaga." },
  { "en": "Apa Faktor Penentu Impedansi Trace?", "id": "Lebar Jalur Dan Tebal Dielektrik." },
  { "en": "Apa Itu Microstrip Line PCB?", "id": "Jalur Di Luar Ground Plane." },
  { "en": "Apa Itu Stripline PCB?", "id": "Jalur Diapit Dua Ground Plane." },
  { "en": "Apa Keuntungan Stripline Dibanding Microstrip?", "id": "Radiasi EMI Lebih Rendah." },
  { "en": "Apa Itu Differential Pair Impedance?", "id": "Impedansi Dua Jalur Berpasangan." },
  { "en": "Apa Standar Impedansi USB Differential?", "id": "90 Ohm." },
  { "en": "Apa Standar Impedansi Ethernet Differential?", "id": "100 Ohm." },
  { "en": "Apa Itu Length Matching PCB?", "id": "Samakan Panjang Jalur Paralel." },
  { "en": "Apa Tujuan Length Matching?", "id": "Samakan Waktu Tiba Sinyal." },
  { "en": "Apa Itu Serpentine Routing?", "id": "Jalur Berkelok Tambah Panjang." },
  { "en": "Apa Itu Stitching Via?", "id": "Via Penghubung Ground Plane." },
  { "en": "Apa Fungsi Decoupling Capacitor Dekat IC?", "id": "Suplai Arus Sesaat Cepat." },
  { "en": "Apa Itu Bulk Capacitor?", "id": "Kapasitor Besar Suplai Utama." },
  { "en": "Apa Itu Loop Inductance Decoupling?", "id": "Induktansi Jalur Kaki Kapasitor." },
  { "en": "Cara Minimalkan Loop Inductance?", "id": "Letakkan Kapasitor Sangat Dekat Pin." },
  { "en": "Apa Kepanjangan BIST Pada Chip?", "id": "Built In Self Test." },
  { "en": "Apa Kepanjangan DFT Desain Chip?", "id": "Design For Testability." },
  { "en": "Apa Fungsi Scan Chain Testing?", "id": "Tes Logika Internal Chip." },
  { "en": "Apa Itu Electromigration Pada IC?", "id": "Atom Pindah Akibat Arus Tinggi." },
  { "en": "Apa Akibat Electromigration?", "id": "Jalur Putus Atau Hubung Singkat." },
  { "en": "Apa Itu Hot Carrier Injection?", "id": "Elektron Enerjik Rusak Oksida Gate." },
  { "en": "Apa Itu Latch Up CMOS?", "id": "Short Circuit Struktur Parasitik." },
  { "en": "Cara Mencegah Latch Up?", "id": "Isolasi Baik Dan Guard Ring." },
  { "en": "Apa Itu ESD Protection Diode?", "id": "Buang Muatan Statis Ke Ground." },
  { "en": "Apa Kepanjangan HBM Model ESD?", "id": "Human Body Model." },
  { "en": "Apa Kepanjangan CDM Model ESD?", "id": "Charged Device Model." },
  { "en": "Apa Itu Dark Current Photodiode?", "id": "Arus Bocor Tanpa Cahaya." },
  { "en": "Apa Itu Quantum Efficiency Sensor?", "id": "Rasio Elektron Per Foton." },
  { "en": "Apa Itu Responsivity Photodiode?", "id": "Ampere Output Per Watt Cahaya." },
  { "en": "Apa Kepanjangan APD Photodiode?", "id": "Avalanche Photodiode." },
  { "en": "Apa Prinsip Kerja Avalanche Photodiode?", "id": "Penggandaan Elektron Efek Avalanche." },
  { "en": "Apa Kelebihan APD Photodiode?", "id": "Sensitivitas Sangat Tinggi." },
  { "en": "Apa Itu NEP Noise Equivalent Power?", "id": "Daya Cahaya Setara Noise." },
  { "en": "Apa Itu Specific Detectivity Sensor?", "id": "Kinerja Sensor Ternormalisasi Luas." },
  { "en": "Apa Fungsi Bolometer Sensor?", "id": "Ukur Daya Radiasi Panas." },
  { "en": "Apa Prinsip Kerja Bolometer?", "id": "Perubahan Resistansi Akibat Panas." },
  { "en": "Apa Aplikasi Microbolometer?", "id": "Kamera Thermal Imaging." },
  { "en": "Apa Kepanjangan TEM Gelombang?", "id": "Transverse Electromagnetic." },
  { "en": "Apa Mode TE Pada Waveguide?", "id": "Transverse Electric." },
  { "en": "Apa Mode TM Pada Waveguide?", "id": "Transverse Magnetic." },
  { "en": "Apa Itu Cutoff Frequency Waveguide?", "id": "Frekuensi Minimum Bisa Merambat." },
  { "en": "Apa Bentuk Waveguide Paling Umum?", "id": "Persegi Panjang Pipa Logam." },
  { "en": "Apa Fungsi Isolator Gelombang Mikro?", "id": "Teruskan Sinyal Satu Arah." },
  { "en": "Apa Fungsi Circulator Gelombang Mikro?", "id": "Arahkan Sinyal Port Berurutan." },
  { "en": "Apa Bahan Pembatut Ferrite?", "id": "Keramik Magnetik Non Konduktif." },
  { "en": "Apa Sifat Bahan Ferrite?", "id": "Permeabilitas Tinggi Resistivitas Tinggi." },
  { "en": "Apa Itu YIG Oscillator?", "id": "Yttrium Iron Garnet Tunable." },
  { "en": "Apa Fungsi Directional Coupler?", "id": "Cuplik Daya Arah Tertentu." },
  { "en": "Apa Itu Coupling Factor dB?", "id": "Rasio Daya Cuplikan Utama." },
  { "en": "Apa Itu Isolation Factor dB?", "id": "Rasio Daya Bocor Balik." },
  { "en": "Apa Fungsi Bias Tee RF?", "id": "Gabung Sinyal RF Dan DC." },
  { "en": "Apa Itu Power Added Efficiency (PAE)?", "id": "Efisiensi Penguat Daya RF." },
  { "en": "Apa Titik Kompresi 1dB (P1dB)?", "id": "Output Melenceng 1dB Dari Linear." },
  { "en": "Apa Sumbu D Pada Kontrol FOC?", "id": "Sumbu Komponen Fluks Magnetik." },
  { "en": "Apa Sumbu Q Pada Kontrol FOC?", "id": "Sumbu Komponen Penghasil Torsi." },
  { "en": "Apa Tujuan Utama Field Oriented Control?", "id": "Kontrol Motor AC Seperti DC." },
  { "en": "Apa Fungsi Blok DSP Slice FPGA?", "id": "Operasi Aritmatika Kecepatan Tinggi." },
  { "en": "Apa Itu Block RAM (BRAM) FPGA?", "id": "Memori Penyimpanan Data Internal." },
  { "en": "Apa Itu Soft Core Processor FPGA?", "id": "Prosesor Dibangun Dari Gerbang Logika." },
  { "en": "Apa Itu Hard Core Processor FPGA?", "id": "Prosesor Fisik Tertanam Di Chip." },
  { "en": "Apa Kepanjangan SERDES Komunikasi Data?", "id": "Serializer Deserializer." },
  { "en": "Apa Fungsi SERDES Pada Chip?", "id": "Ubah Data Paralel Ke Serial Cepat." },
  { "en": "Apa Itu Eye Mask Margin?", "id": "Area Larangan Sinyal Eye Diagram." },
  { "en": "Apa Itu Pre-Emphasis Sinyal Digital?", "id": "Kuatkan Bit Transisi Awal." },
  { "en": "Apa Itu Equalization Pada Penerima Serial?", "id": "Pulihkan Sinyal Terkena Low Pass." },
  { "en": "Apa Kepanjangan PCIe Bus Komputer?", "id": "Peripheral Component Interconnect Express." },
  { "en": "Apa Itu Lane Pada PCIe?", "id": "Satu Pasang Jalur Tx Rx." },
  { "en": "Apa Encoding Data PCIe Gen 3?", "id": "128b 130b Encoding." },
  { "en": "Apa Encoding Data PCIe Gen 1?", "id": "8b 10b Encoding." },
  { "en": "Apa Tujuan Encoding 8b/10b?", "id": "Jaga Keseimbangan DC Sinyal." },
  { "en": "Apa Itu DC Balance Sinyal?", "id": "Jumlah Nol Dan Satu Seimbang." },
  { "en": "Apa Kepanjangan LVDS Sinyal?", "id": "Low Voltage Differential Signaling." },
  { "en": "Apa Arus Konstan Driver LVDS?", "id": "3.5 Mili Ampere." },
  { "en": "Apa Tegangan Terminasi LVDS?", "id": "100 Ohm." },
  { "en": "Apa Itu Common Mode Noise?", "id": "Noise Sama Di Kedua Kabel." },
  { "en": "Apa Itu Differential Mode Noise?", "id": "Noise Beda Fasa Di Kabel." },
  { "en": "Apa Fungsi Choke Common Mode?", "id": "Blokir Noise Common Mode." },
  { "en": "Apa Fungsi Ferrite Bead Pada Kabel?", "id": "Redam Noise Frekuensi Tinggi." },
  { "en": "Apa Sifat Resistif Ferrite Bead?", "id": "Dominan Pada Frekuensi Tinggi." },
  { "en": "Apa Itu Skin Depth Konduktor?", "id": "Kedalaman Aliran Arus AC." },
  { "en": "Apa Rumus Skin Depth?", "id": "Berbanding Terbalik Akar Frekuensi." },
  { "en": "Apa Material Pelapis Emas PCB?", "id": "Cegah Korosi Kontak Bagus." },
  { "en": "Apa Kepanjangan HASL Finish PCB?", "id": "Hot Air Solder Leveling." },
  { "en": "Apa Kelemahan HASL Untuk SMD?", "id": "Permukaan Tidak Rata Sempurna." },
  { "en": "Apa Itu Impedansi Gelombang Ruang Hampa?", "id": "377 Ohm." },
  { "en": "Apa Rumus Impedansi Ruang Hampa?", "id": "Akar Mu Nol Bagi Epsilon Nol." },
  { "en": "Apa Itu Konstanta Propagasi Gelombang?", "id": "Ukuran Perubahan Amplitudo Fasa." },
  { "en": "Apa Itu Attenuation Constant Alpha?", "id": "Redaman Sinyal Per Meter." },
  { "en": "Apa Itu Phase Constant Beta?", "id": "Pergeseran Fasa Per Meter." },
  { "en": "Apa Nilai VSWR Jika Match Sempurna?", "id": "Satu Banding Satu." },
  { "en": "Apa Nilai VSWR Jika Open Circuit?", "id": "Tak Terhingga." },
  { "en": "Apa Rumus Koefisien Refleksi Gamma?", "id": "Zl Min Zo Bagi Zl Plus Zo." },
  { "en": "Apa Itu Smith Chart Impedansi?", "id": "Plot Grafis Koefisien Refleksi." },
  { "en": "Apa Titik Tengah Smith Chart?", "id": "Impedansi Karakteristik Match." },
  { "en": "Apa Bagian Atas Smith Chart?", "id": "Wilayah Induktif." },
  { "en": "Apa Bagian Bawah Smith Chart?", "id": "Wilayah Kapasitif." },
  { "en": "Apa Garis Horizontal Tengah Smith Chart?", "id": "Sumbu Resistansi Murni." },
  { "en": "Apa Itu Quarter Wave Transformer?", "id": "Pencocok Impedansi Seperempat Gelombang." },
  { "en": "Apa Rumus Impedansi Quarter Wave?", "id": "Akar Zs Kali Zl." },
  { "en": "Apa Itu Balun Transformator?", "id": "Balanced To Unbalanced." },
  { "en": "Apa Fungsi Balun Pada Dipole?", "id": "Sambung Antena Simetris Ke Coax." },
  { "en": "Apa Itu Antenna Gain Isotropik?", "id": "Gain Referensi Segala Arah." },
  { "en": "Apa Itu Effective Isotropic Radiated Power?", "id": "Daya Pancar Semu Total." },
  { "en": "Apa Kepanjangan EIRP Telekomunikasi?", "id": "Effective Isotropic Radiated Power." },
  { "en": "Apa Itu Fresnel Zone Pertama?", "id": "Area Vital Rambatan Gelombang." },
  { "en": "Apa Syarat Line Of Sight Bersih?", "id": "60 Persen Fresnel Zone Kosong." },
  { "en": "Apa Itu Multipath Fading?", "id": "Interferensi Sinyal Pantulan." },
  { "en": "Apa Distribusi Rayleigh Fading?", "id": "Model Fading Tanpa LOS." },
  { "en": "Apa Distribusi Rician Fading?", "id": "Model Fading Ada LOS." },
  { "en": "Apa Fungsi Diversity Antenna?", "id": "Atasi Fading Dengan Banyak Antena." },
  { "en": "Apa Teknik Spatial Diversity?", "id": "Antena Diberi Jarak Fisik." },
  { "en": "Apa Kepanjangan MIMO Komunikasi?", "id": "Multiple Input Multiple Output." },
  { "en": "Apa Keuntungan Utama MIMO?", "id": "Tingkatkan Throughput Tanpa Bandwidth Tambahan." },
  { "en": "Apa Itu Spatial Multiplexing?", "id": "Kirim Data Beda Tiap Antena." },
  { "en": "Apa Definisi Noise Figure?", "id": "Penurunan SNR Akibat Komponen." },
  { "en": "Apa Rumus Noise Figure dB?", "id": "SNR Input Kurang SNR Output." },
  { "en": "Apa Itu Low Noise Amplifier (LNA)?", "id": "Penguat Awal Sinyal Lemah." },
  { "en": "Apa Posisi LNA Dalam Sistem?", "id": "Paling Dekat Dengan Antena." },
  { "en": "Apa Itu Power Amplifier (PA) RF?", "id": "Penguat Akhir Sebelum Antena." },
  { "en": "Apa Parameter P1dB Amplifier?", "id": "Titik Kompresi Gain 1dB." },
  { "en": "Apa Itu Third Order Intercept (IP3)?", "id": "Ukuran Linearitas Amplifier RF." },
  { "en": "Apa Efek Intermodulasi Orde 3?", "id": "Muncul Sinyal Dekat Frekuensi Asli." },
  { "en": "Apa Itu Image Reject Mixer?", "id": "Mixer Pembuang Frekuensi Bayangan." },
  { "en": "Apa Osilator Lokal (LO)?", "id": "Pembangkit Frekuensi Pencampur." },
  { "en": "Apa Itu Phase Noise Osilator?", "id": "Ketidakstabilan Fasa Domain Frekuensi." },
  { "en": "Apa Efek Phase Noise Tinggi?", "id": "Ganggu Kanal Frekuensi Sebelah." },
  { "en": "Apa Itu Allan Variance?", "id": "Ukuran Stabilitas Frekuensi Waktu." },
  { "en": "Apa Fungsi Atomic Clock?", "id": "Standar Frekuensi Sangat Presisi." },
  { "en": "Apa Bahan Jam Atom Sesium?", "id": "Cesium 133." },
  { "en": "Apa Definisi Satu Detik Standar?", "id": "9 Milyar Getaran Atom Cesium." },
  { "en": "Apa Kepanjangan MEMS Oscillator?", "id": "Micro Electro Mechanical Systems." },
  { "en": "Apa Kelebihan MEMS Oscillator?", "id": "Tahan Getaran Ukuran Kecil." },
  { "en": "Apa Itu Piezoelectric Effect?", "id": "Tekanan Hasilkan Muatan Listrik." },
  { "en": "Apa Itu Reverse Piezoelectric Effect?", "id": "Listrik Hasilkan Deformasi Mekanis." },
  { "en": "Apa Bahan Piezoelektrik Paling Umum?", "id": "PZT Lead Zirconate Titanate." },
  { "en": "Apa Fungsi Transduser Ultrasonik?", "id": "Ubah Listrik Jadi Suara." },
  { "en": "Apa Rumus Impedansi Akustik?", "id": "Densitas Kali Kecepatan Suara." },
  { "en": "Apa Fungsi Matching Layer Transduser?", "id": "Transfer Energi Ke Objek." },
  { "en": "Apa Itu Hydrophone?", "id": "Mikrofon Bawah Air." },
  { "en": "Apa Itu Strain Gauge Factor?", "id": "Rasio Perubahan R Terhadap Regangan." },
  { "en": "Apa Bahan Strain Gauge Metal?", "id": "Konstantan." },
  { "en": "Apa Pengaruh Suhu Pada Strain Gauge?", "id": "Resistansi Berubah Akibat Ekspansi." },
  { "en": "Apa Konfigurasi Dummy Gauge?", "id": "Kompensasi Suhu Jembatan Wheatstone." },
  { "en": "Apa Kepanjangan LVDT Sensor?", "id": "Linear Variable Differential Transformer." },
  { "en": "Apa Output LVDT Saat Di Tengah?", "id": "Nol Volt." },
  { "en": "Apa Kelebihan LVDT?", "id": "Resolusi Tinggi Tanpa Gesekan." },
  { "en": "Apa Fungsi Resolver Motor?", "id": "Sensor Posisi Sudut Absolut." },
  { "en": "Apa Sinyal Output Resolver?", "id": "Sinus Dan Cosinus." },
  { "en": "Apa Perbedaan Encoder Dan Resolver?", "id": "Encoder Digital, Resolver Analog." },
  { "en": "Apa Itu Hall Effect Latching?", "id": "Simpan Status Terakhir Magnet." },
  { "en": "Apa Satuan Dari Konduktansi Listrik?", "id": "Siemens." },
  { "en": "Apa Kebalikan Dari Besaran Resistansi?", "id": "Konduktansi." },
  { "en": "Apa Kebalikan Dari Besaran Impedansi?", "id": "Admitansi." },
  { "en": "Apa Simbol Variabel Admitansi?", "id": "Y." },
  { "en": "Apa Bagian Real Dari Admitansi?", "id": "Konduktansi (G)." },
  { "en": "Apa Bagian Imajiner Dari Admitansi?", "id": "Susseptansi (B)." },
  { "en": "Apa Koefisien Suhu Positif (PTC)?", "id": "Suhu Naik Resistansi Naik." },
  { "en": "Apa Koefisien Suhu Negatif (NTC)?", "id": "Suhu Naik Resistansi Turun." },
  { "en": "Apa Sifat Koefisien Suhu Tembaga?", "id": "Positif (PTC)." },
  { "en": "Apa Sifat Koefisien Suhu Semikonduktor Murni?", "id": "Negatif (NTC)." },
  { "en": "Apa Itu Miller Plateau Pada MOSFET?", "id": "Tegangan Gate Datar Saat Switching." },
  { "en": "Apa Efek Miller Plateau?", "id": "Memperlambat Waktu Switching." },
  { "en": "Apa Fungsi Tail Current Transistor?", "id": "Sumber Arus Penguat Diferensial." },
  { "en": "Apa Itu R-2R Ladder DAC?", "id": "Jaringan Resistor Seri Paralel." },
  { "en": "Apa Kelemahan R-2R Ladder DAC?", "id": "Butuh Presisi Resistor Tinggi." },
  { "en": "Apa Prinsip Sigma Delta ADC?", "id": "Oversampling Dan Noise Shaping." },
  { "en": "Apa Kelebihan Sigma Delta ADC?", "id": "Resolusi Tinggi Biaya Murah." },
  { "en": "Apa Prinsip Flash ADC?", "id": "Komparator Paralel Simultan." },
  { "en": "Apa Jumlah Komparator Flash ADC 8-bit?", "id": "255 Komparator." },
  { "en": "Apa Prinsip SAR ADC?", "id": "Pencarian Biner Tegangan Input." },
  { "en": "Apa Kepanjangan SAR ADC?", "id": "Successive Approximation Register." },
  { "en": "Apa Fungsi Sample And Hold?", "id": "Tahan Tegangan Selama Konversi." },
  { "en": "Apa Itu Aperture Time ADC?", "id": "Waktu Jendela Pengambilan Sampel." },
  { "en": "Apa Itu Aperture Jitter?", "id": "Ketidakstabilan Waktu Sampling." },
  { "en": "Apa Efek Jitter Pada ADC?", "id": "Turunkan SNR Frekuensi Tinggi." },
  { "en": "Apa Kepanjangan ENOB ADC?", "id": "Effective Number Of Bits." },
  { "en": "Apa Kepanjangan SINAD ADC?", "id": "Signal To Noise Distortion Ratio." },
  { "en": "Apa Kepanjangan SFDR ADC?", "id": "Spurious Free Dynamic Range." },
  { "en": "Apa Rumus Free Space Path Loss?", "id": "Redaman Sinyal Ruang Hampa." },
  { "en": "Apa Itu Fade Margin?", "id": "Cadangan Daya Atasi Fading." },
  { "en": "Apa Itu Link Budget?", "id": "Perhitungan Total Gain Loss." },
  { "en": "Apa Bunyi Teorema Shannon Hartley?", "id": "Batas Kapasitas Kanal Informasi." },
  { "en": "Apa Itu Symbol Rate?", "id": "Jumlah Perubahan Simbol Per Detik." },
  { "en": "Apa Satuan Symbol Rate?", "id": "Baud." },
  { "en": "Apa Hubungan Bit Rate Dan Baud Rate?", "id": "Bit Rate Sama Dengan Baud Kali Bit." },
  { "en": "Apa Itu Modulasi BPSK?", "id": "Dua Fasa Berlawanan." },
  { "en": "Berapa Bit Per Simbol BPSK?", "id": "Satu Bit." },
  { "en": "Apa Itu Modulasi QPSK?", "id": "Empat Fasa Kuadratur." },
  { "en": "Berapa Bit Per Simbol QPSK?", "id": "Dua Bit." },
  { "en": "Apa Itu Modulasi 16-QAM?", "id": "16 Titik Kombinasi Fasa Amplitudo." },
  { "en": "Berapa Bit Per Simbol 16-QAM?", "id": "Empat Bit." },
  { "en": "Apa Fungsi Constellation Diagram?", "id": "Visualisasi Modulasi Digital." },
  { "en": "Apa Arti Titik Konstelasi Kabur?", "id": "Noise Atau Jitter Tinggi." },
  { "en": "Apa Kepanjangan EVM Sinyal?", "id": "Error Vector Magnitude." },
  { "en": "Apa Kepanjangan BER Sinyal?", "id": "Bit Error Rate." },
  { "en": "Apa Alat Ukur BER?", "id": "Bit Error Rate Tester." },
  { "en": "Apa Fungsi Clock Recovery?", "id": "Ekstrak Detak Dari Data." },
  { "en": "Apa Karakteristik Manchester Encoding?", "id": "Transisi Di Tengah Bit." },
  { "en": "Apa Kelebihan Manchester Encoding?", "id": "Sinkronisasi Mudah Tanpa DC." },
  { "en": "Apa Kelemahan Manchester Encoding?", "id": "Boros Bandwidth Dua Kali." },
  { "en": "Apa Itu NRZ Encoding?", "id": "Non Return To Zero." },
  { "en": "Apa Itu AMI Encoding?", "id": "Alternate Mark Inversion." },
  { "en": "Apa Tujuan Scrambling Data?", "id": "Acak Bit Hindari Pola Tetap." },
  { "en": "Apa Tujuan Interleaving Data?", "id": "Sebar Error Berurutan." },
  { "en": "Apa Fungsi Reed Solomon Code?", "id": "Koreksi Error Blok Data." },
  { "en": "Apa Fungsi Convolutional Code?", "id": "Koreksi Error Aliran Data." },
  { "en": "Apa Fungsi Algoritma Viterbi?", "id": "Dekoding Kode Konvolusi." },
  { "en": "Apa Kode Koreksi Error 5G NR?", "id": "LDPC Dan Polar Codes." },
  { "en": "Apa Kepanjangan LDPC Code?", "id": "Low Density Parity Check." },
  { "en": "Apa Konsep Cognitive Radio?", "id": "Cari Spektrum Kosong Otomatis." },
  { "en": "Apa Kepanjangan SDR Radio?", "id": "Software Defined Radio." },
  { "en": "Apa Inti Dari Sistem SDR?", "id": "ADC DAC Dan FPGA." },
  { "en": "Apa Itu Femtocell?", "id": "BTS Seluler Skala Rumahan." },
  { "en": "Apa Itu Carrier Aggregation?", "id": "Gabung Frekuensi Tambah Kecepatan." },
  { "en": "Apa Kepanjangan VoLTE?", "id": "Voice Over LTE." },
  { "en": "Apa Definisi Lit Fiber?", "id": "Kabel Optik Aktif Terpakai." },
  { "en": "Apa Itu Chromatic Dispersion Fiber?", "id": "Cahaya Beda Warna Beda Cepat." },
  { "en": "Apa Itu Polarization Mode Dispersion?", "id": "Cahaya Beda Polarisasi Beda Cepat." },
  { "en": "Apa Itu Four Wave Mixing?", "id": "Intermodulasi Frekuensi Optik." },
  { "en": "Apa Itu Soliton Pulse?", "id": "Pulsa Cahaya Tahan Bentuk." },
  { "en": "Apa Itu Fiber Fuse Effect?", "id": "Kerusakan Fiber Merambat Cepat." },
  { "en": "Apa Penyebab Fiber Fuse?", "id": "Daya Tinggi Pada Kotoran." },
  { "en": "Apa Definisi Splicing Loss?", "id": "Rugi Daya Sambungan Fiber." },
  { "en": "Apa Definisi Return Loss Fiber?", "id": "Rugi Daya Pantulan Balik." },
  { "en": "Apa Warna Konektor UPC?", "id": "Biru." },
  { "en": "Apa Warna Konektor APC?", "id": "Hijau." },
  { "en": "Apa Ciri Fisik Konektor APC?", "id": "Ujung Miring 8 Derajat." },
  { "en": "Apa Kelebihan Konektor APC?", "id": "Pantulan Balik Sangat Kecil." },
  { "en": "Apa Itu EMI Conducted Emission?", "id": "Gangguan Merambat Lewat Kabel." },
  { "en": "Apa Itu EMI Radiated Emission?", "id": "Gangguan Memancar Lewat Udara." },
  { "en": "Apa Fungsi Common Mode Choke?", "id": "Redam Noise Fasa Sama." },
  { "en": "Apa Itu Ground Bounce?", "id": "Lonjakan Tegangan Ground Sesaat." },
  { "en": "Apa Penyebab Ground Bounce?", "id": "Induktansi Kaki IC Switching." },
  { "en": "Apa Itu Vcc Sag?", "id": "Tegangan Suplai Turun Sesaat." },
  { "en": "Apa Fungsi Bootstrap Capacitor?", "id": "Suplai Gate Mosfet Sisi Atas." },
  { "en": "Apa Itu Floating Gate Transistor?", "id": "Penyimpan Muatan Memori Flash." },
  { "en": "Apa Efek Hot Carrier Injection?", "id": "Degradasi Umur Transistor MOS." },
  { "en": "Apa Itu Breakdown Voltage BVdss?", "id": "Batas Tegangan Drain Source." },
  { "en": "Apa Itu On Resistance Rds(on)?", "id": "Hambatan Saat Mosfet Nyala." },
  { "en": "Apa Pengaruh Suhu Pada Rds(on)?", "id": "Suhu Naik Hambatan Naik." },
  { "en": "Apa Itu Gate Charge (Qg)?", "id": "Muatan Untuk Nyalakan Mosfet." },
  { "en": "Apa Pengaruh Qg Besar?", "id": "Switching Lebih Lambat." },
  { "en": "Apa Fungsi Flyback Diode Relay?", "id": "Buang Arus Balik Kumparan." },
  { "en": "Apa Nama Lain Flyback Diode?", "id": "Freewheeling Diode." }


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
