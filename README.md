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


  { "en": "Apa Itu Kualitas Daya?", "id": "Karakteristik Tegangan, Arus, Dan Frekuensi." },
  { "en": "Mengapa Kualitas Daya Penting?", "id": "Mempengaruhi Kinerja Peralatan Listrik." },
  { "en": "Apa Konsekuensi Kualitas Daya Buruk?", "id": "Kerusakan Peralatan, Kehilangan Data, Inefisiensi." },
  { "en": "Siapa Yang Bertanggung Jawab Atas Kualitas Daya?", "id": "Penyedia Utilitas Dan Pengguna Akhir." },
  { "en": "Apa Saja Kategori Utama Masalah Kualitas Daya?", "id": "Variasi Tegangan, Harmonik, Transien, Interupsi." },
  { "en": "Apa Itu Tegangan Nominal?", "id": "Nilai Tegangan Sistem Yang Diharapkan." },
  { "en": "Apa Itu Frekuensi Nominal?", "id": "Frekuensi Sistem (50/60 Hz)." },
  { "en": "Apa Itu Voltage Sag (Turun Tegangan)?", "id": "Penurunan Tegangan RMS Jangka Pendek." },
  { "en": "Berapa Durasi Voltage Sag?", "id": "Setengah Siklus Hingga Satu Menit." },
  { "en": "Apa Penyebab Umum Voltage Sag?", "id": "Penyalaan Motor Besar Atau Gangguan." },
  { "en": "Apa Efek Voltage Sag?", "id": "Reset Komputer, Kontaktor Terbuka." },
  { "en": "Apa Itu Voltage Swell (Naik Tegangan)?", "id": "Kenaikan Tegangan RMS Jangka Pendek." },
  { "en": "Apa Penyebab Voltage Swell?", "id": "Pemutusan Beban Besar Atau Gangguan." },
  { "en": "Apa Itu Interupsi?", "id": "Kehilangan Total Tegangan Suplai." },
  { "en": "Apa Itu Interupsi Sesaat?", "id": "Interupsi Berdurasi Sangat Pendek." },
  { "en": "Apa Itu Interupsi Sementara?", "id": "Interupsi Berdurasi Hingga Beberapa Detik." },
  { "en": "Apa Itu Interupsi Berkelanjutan?", "id": "Interupsi Berdurasi Lebih Dari Satu Menit." },
  { "en": "Apa Itu Undervoltage?", "id": "Kondisi Tegangan Di Bawah Batas Normal." },
  { "en": "Apa Durasi Undervoltage?", "id": "Lebih Dari Satu Menit." },
  { "en": "Apa Itu Overvoltage?", "id": "Kondisi Tegangan Di Atas Batas Normal." },
  { "en": "Apa Penyebab Overvoltage?", "id": "Tap Trafo Salah, Kapasitor Bank." },
  { "en": "Apa Itu Transien?", "id": "Peristiwa Tegangan Atau Arus Sub-siklus." },
  { "en": "Apa Dua Jenis Transien?", "id": "Impulsif Dan Osilatoris." },
  { "en": "Apa Itu Transien Impulsif?", "id": "Pulsa Searah Polaritas Tunggal." },
  { "en": "Apa Penyebab Transien Impulsif?", "id": "Sambaran Petir, Switching Beban." },
  { "en": "Apa Itu Transien Osilatoris?", "id": "Pulsa Yang Berosilasi." },
  { "en": "Apa Penyebab Transien Osilatoris?", "id": "Switching Kapasitor Bank." },
  { "en": "Apa Itu Notching?", "id": "Gangguan Periodik Pada Gelombang Tegangan." },
  { "en": "Apa Penyebab Notching?", "id": "Operasi Normal Konverter Daya." },
  { "en": "Apa Itu Harmonik?", "id": "Komponen Frekuensi Kelipatan Dari Fundamental." },
  { "en": "Apa Itu Frekuensi Fundamental?", "id": "Frekuensi Dasar Sistem Tenaga." },
  { "en": "Apa Penyebab Harmonik?", "id": "Beban Non-linier." },
  { "en": "Apa Itu Beban Non-linier?", "id": "Beban Dimana Arus Tidak Sinusoidal." },
  { "en": "Contoh Beban Non-linier Adalah Apa?", "id": "Komputer, Penyearah, Dan Lampu LED." },
  { "en": "Apa Efek Harmonik?", "id": "Pemanasan Berlebih, Distorsi Tegangan." },
  { "en": "Apa Itu Total Harmonic Distortion (THD)?", "id": "Ukuran Total Distorsi Harmonik." },
  { "en": "Jenis THD (Total Harmonic Distortion) Apa Yang Diukur?", "id": "THD Tegangan Dan THD Arus." },
  { "en": "Apa Itu Harmonik Orde Tiga?", "id": "Harmonik Pada Tiga Kali Frekuensi." },
  { "en": "Apa Masalah Harmonik Orde Tiga?", "id": "Dapat Menyebabkan Arus Netral Berlebih." },
  { "en": "Apa Itu Interharmonik?", "id": "Komponen Frekuensi Bukan Kelipatan Fundamental." },
  { "en": "Apa Itu Fluktuasi Tegangan?", "id": "Variasi Sistematis Pada Tegangan." },
  { "en": "Apa Itu Flicker?", "id": "Persepsi Visual Dari Fluktuasi Tegangan." },
  { "en": "Apa Penyebab Flicker?", "id": "Beban Besar Yang Berubah Cepat." },
  { "en": "Contoh Penyebab Flicker Adalah Apa?", "id": "Tungku Busur Listrik, Mesin Las." },
  { "en": "Apa Itu Ketidakseimbangan Tegangan?", "id": "Tegangan Tiga Fasa Tidak Sama." },
  { "en": "Apa Efek Ketidakseimbangan Tegangan?", "id": "Pemanasan Berlebih Pada Motor Tiga Fasa." },
  { "en": "Apa Penyebab Ketidakseimbangan Tegangan?", "id": "Distribusi Beban Satu Fasa Tidak Merata." },
  { "en": "Apa Itu Variasi Frekuensi?", "id": "Penyimpangan Dari Frekuensi Nominal." },
  { "en": "Apa Penyebab Variasi Frekuensi?", "id": "Ketidakseimbangan Antara Pembangkitan Dan Beban." },
  { "en": "Apa Itu Peralatan Sensitif?", "id": "Peralatan Yang Mudah Terpengaruh." },
  { "en": "Apa Itu Point of Common Coupling (PCC)?", "id": "Titik Dimana Pengguna Terhubung Ke Jaringan." },
  { "en": "Apa Itu Standar Kualitas Daya?", "id": "Menetapkan Batas Untuk Fenomena Kualitas Daya." },
  { "en": "Sebutkan Standar Kualitas Daya Populer?", "id": "IEEE 519, EN 50160." },
  { "en": "Apa Yang Diatur IEEE (Institute of Electrical and Electronics Engineers) 519?", "id": "Batas Distorsi Harmonik." },
  { "en": "Apa Yang Diatur EN 50160?", "id": "Karakteristik Tegangan Di Jaringan Publik." },
  { "en": "Apa Itu Power Quality Monitoring?", "id": "Proses Mengukur Dan Menganalisis Kualitas Daya." },
  { "en": "Apa Itu Penganalisis Kualitas Daya?", "id": "Instrumen Untuk Mengukur Fenomena Kualitas Daya." },
  { "en": "Apa Itu Gelombang (Waveform)?", "id": "Bentuk Grafis Dari Sinyal." },
  { "en": "Apa Itu Gelombang Sinus?", "id": "Bentuk Gelombang Ideal Untuk Tegangan AC." },
  { "en": "Apa Itu Nilai RMS (Root Mean Square)?", "id": "Nilai Efektif Dari Gelombang AC." },
  { "en": "Apa Itu Crest Factor?", "id": "Rasio Nilai Puncak Terhadap Nilai RMS." },
  { "en": "Berapa Crest Factor Gelombang Sinus?", "id": "Sekitar 1.414." },
  { "en": "Apa Itu Faktor K?", "id": "Peringkat Transformator Untuk Beban Harmonik." },
  { "en": "Apa Itu Pembumian (Grounding)?", "id": "Koneksi Konduktif Ke Bumi." },
  { "en": "Mengapa Pembumian Penting Untuk Kualitas Daya?", "id": "Menyediakan Referensi Tegangan Yang Stabil." },
  { "en": "Apa Itu Derau (Noise)?", "id": "Sinyal Listrik Frekuensi Tinggi." },
  { "en": "Apa Itu Common-Mode Noise?", "id": "Derau Antara Konduktor Dan Ground." },
  { "en": "Apa Itu Differential-Mode Noise?", "id": "Derau Antara Dua Konduktor." },
  { "en": "Apa Itu Solusi Kualitas Daya?", "id": "Peralatan Untuk Memitigasi Masalah." },
  { "en": "Apa Itu Uninterruptible Power Supply (UPS)?", "id": "Menyediakan Daya Cadangan Saat Interupsi." },
  { "en": "Apa Tipe UPS (Uninterruptible Power Supply) Online?", "id": "Selalu Mengkonversi Daya, Perlindungan Terbaik." },
  { "en": "Apa Itu Surge Protective Device (SPD)?", "id": "Melindungi Peralatan Dari Lonjakan Tegangan." },
  { "en": "Nama Lain SPD (Surge Protective Device) Adalah Apa?", "id": "Surge Arrester Atau TVSS." },
  { "en": "Apa Itu Transient Voltage Surge Suppressor (TVSS)?", "id": "Nama Lain Untuk SPD." },
  { "en": "Apa Itu Metal Oxide Varistor (MOV)?", "id": "Komponen Umum Dalam SPD." },
  { "en": "Apa Itu Filter Harmonik?", "id": "Mengurangi Distorsi Harmonik." },
  { "en": "Apa Itu Filter Harmonik Pasif?", "id": "Menggunakan Induktor Dan Kapasitor." },
  { "en": "Apa Itu Filter Harmonik Aktif?", "id": "Menggunakan Elektronika Daya Untuk Membatalkan." },
  { "en": "Apa Itu Dynamic Voltage Restorer (DVR)?", "id": "Mengkompensasi Voltage Sag Dan Swell." },
  { "en": "Apa Itu Static VAR Compensator (SVC)?", "id": "Mengatur Tegangan Dengan Daya Reaktif." },
  { "en": "Apa Itu STATCOM (Static Synchronous Compensator)?", "id": "SVC Berbasis Konverter Elektronika Daya." },
  { "en": "Apa Itu Transformator Isolasi?", "id": "Mentransfer Daya Melalui Medan Magnet." },
  { "en": "Bagaimana Transformator Isolasi Membantu?", "id": "Dapat Memblokir Derau Common-Mode." },
  { "en": "Apa Itu Zero Sequence Blocking Transformer?", "id": "Memblokir Arus Harmonik Orde Tiga." },
  { "en": "Apa Itu Regulator Tegangan?", "id": "Menjaga Tegangan Output Tetap Konstan." },
  { "en": "Apa Itu Power Conditioner?", "id": "Perangkat Yang Meningkatkan Kualitas Daya." },
  { "en": "Apa Itu Daya Nyata (Real Power)?", "id": "Daya Aktual Yang Melakukan Pekerjaan." },
  { "en": "Apa Satuan Daya Nyata?", "id": "Watt (W)." },
  { "en": "Apa Itu Daya Reaktif (Reactive Power)?", "id": "Daya Untuk Medan Magnet Dan Listrik." },
  { "en": "Apa Satuan Daya Reaktif?", "id": "Volt-Ampere Reactive (VAR)." },
  { "en": "Apa Itu Daya Tampak (Apparent Power)?", "id": "Kombinasi Vektor Daya Nyata Dan Reaktif." },
  { "en": "Apa Satuan Daya Tampak?", "id": "Volt-Ampere (VA)." },
  { "en": "Apa Itu Faktor Daya (Power Factor)?", "id": "Rasio Daya Nyata Terhadap Daya Tampak." },
  { "en": "Berapa Nilai Faktor Daya Ideal?", "id": "Satu (Unity)." },
  { "en": "Apa Penyebab Faktor Daya Rendah?", "id": "Beban Induktif Seperti Motor." },
  { "en": "Apa Itu Power Factor Correction (PFC)?", "id": "Upaya Untuk Meningkatkan Faktor Daya." },
  { "en": "Bagaimana Cara Memperbaiki Faktor Daya?", "id": "Menambahkan Kapasitor Bank." },
  { "en": "Apa Itu Kurva CBEMA (Computer and Business Equipment Manufacturers Association)?", "id": "Kurva Toleransi Tegangan Untuk Komputer." },
  { "en": "Apa Itu Kurva ITIC (Information Technology Industry Council)?", "id": "Versi Terbaru Dari Kurva CBEMA." },
  { "en": "Apa Efek Pemanasan Harmonik?", "id": "Meningkatkan Kerugian Tembaga Dan Inti." },
  { "en": "Apa Itu Efek Kulit (Skin Effect)?", "id": "Arus Frekuensi Tinggi Mengalir Di Permukaan." },
  { "en": "Bagaimana Harmonik Memperburuk Efek Kulit?", "id": "Meningkatkan Resistansi Efektif Konduktor." },
  { "en": "Apa Itu Resonansi Paralel?", "id": "Osilasi Antara Kapasitor Dan Induktansi." },
  { "en": "Bagaimana Harmonik Dapat Menyebabkan Resonansi?", "id": "Frekuensi Harmonik Cocok Dengan Frekuensi Resonansi." },
  { "en": "Apa Akibat Resonansi Harmonik?", "id": "Tegangan Dan Arus Sangat Tinggi." },
  { "en": "Apa Itu Beban Linier?", "id": "Beban Dimana Arus Berbentuk Sinusoidal." },
  { "en": "Sebutkan Contoh Beban Linier?", "id": "Pemanas Resistif, Lampu Pijar, Motor Induksi." },
  { "en": "Apa Itu Distorsi Gelombang?", "id": "Penyimpangan Dari Bentuk Gelombang Sinus Ideal." },
  { "en": "Apa Itu Analisis Fourier?", "id": "Memecah Gelombang Menjadi Komponen Sinusoidal." },
  { "en": "Apa Itu Fast Fourier Transform (FFT)?", "id": "Algoritma Cepat Untuk Analisis Fourier." },
  { "en": "Apa Itu Spektrum Frekuensi?", "id": "Plot Amplitudo Terhadap Frekuensi." },
  { "en": "Apa Itu Gelombang Kotak (Square Wave)?", "id": "Mengandung Harmonik Orde Ganjil." },
  { "en": "Apa Itu Penyearah Enam Pulsa?", "id": "Menghasilkan Harmonik Orde 5, 7, 11, 13." },
  { "en": "Apa Itu Harmonik Karakteristik?", "id": "Harmonik Yang Dihasilkan Oleh Jenis Konverter." },
  { "en": "Apa Itu Urutan Fasa?", "id": "Urutan Tegangan Fasa Mencapai Puncak." },
  { "en": "Apa Itu Harmonik Urutan Positif?", "id": "Berputar Searah Dengan Fundamental." },
  { "en": "Apa Itu Harmonik Urutan Negatif?", "id": "Berputar Berlawanan Arah Dengan Fundamental." },
  { "en": "Apa Efek Harmonik Urutan Negatif?", "id": "Menyebabkan Pemanasan Berlebih Pada Motor." },
  { "en": "Apa Itu Harmonik Urutan Nol?", "id": "Sefasa Di Ketiga Fasa." },
  { "en": "Di Mana Arus Harmonik Urutan Nol Mengalir?", "id": "Di Konduktor Netral." },
  { "en": "Apa Itu Triplen Harmonics?", "id": "Harmonik Kelipatan Tiga (3, 9, 15)." },
  { "en": "Apakah Triplen Harmonics Termasuk Urutan Nol?", "id": "Ya, Triplen Adalah Harmonik Urutan Nol." },
  { "en": "Apa Itu Overloading Netral?", "id": "Arus Netral Melebihi Kapasitas." },
  { "en": "Apa Itu Survey Kualitas Daya?", "id": "Investigasi Untuk Mengidentifikasi Masalah." },
  { "en": "Apa Langkah Pertama Survey?", "id": "Mengumpulkan Informasi Dan Data Awal." },
  { "en": "Apa Itu Pengukuran Jangka Panjang?", "id": "Pemantauan Kualitas Daya Selama Waktu." },
  { "en": "Apa Itu Pengukuran Diagnostik?", "id": "Pengukuran Jangka Pendek Untuk Masalah." },
  { "en": "Apa Itu Current Transformer (CT)?", "id": "Sensor Untuk Mengukur Arus Tinggi." },
  { "en": "Apa Itu Potential Transformer (PT)?", "id": "Sensor Untuk Mengukur Tegangan Tinggi." },
  { "en": "Apa Itu Kompensasi Daya Reaktif?", "id": "Nama Lain Untuk Koreksi Faktor Daya." },
  { "en": "Apa Itu Kapasitor Bank?", "id": "Sekelompok Kapasitor Untuk Kompensasi." },
  { "en": "Apa Itu Detuned Filter?", "id": "Filter Pasif Untuk Menghindari Resonansi." },
  { "en": "Apa Itu Tuned Filter?", "id": "Filter Pasif Yang Ditala Ke Frekuensi." },
  { "en": "Apa Itu Shunt Active Power Filter (APF)?", "id": "Menyuntikkan Arus Harmonik Kompensasi." },
  { "en": "Apa Itu Series Active Power Filter?", "id": "Mengkompensasi Masalah Tegangan." },
  { "en": "Apa Itu Unified Power Quality Conditioner (UPQC)?", "id": "Menggabungkan Filter Shunt Dan Seri." },
  { "en": "Apa Itu Voltage Regulation?", "id": "Menjaga Tegangan Dalam Batas." },
  { "en": "Apa Itu On-Load Tap Changer (OLTC)?", "id": "Mengubah Rasio Transformator Saat Berbeban." },
  { "en": "Apa Itu Step-Voltage Regulator?", "id": "Jenis Transformator Dengan Tap Changer." },
  { "en": "Apa Itu Ferroresonant Transformer?", "id": "Transformator Yang Menggunakan Resonansi." },
  { "en": "Apa Keuntungan Ferroresonant Transformer?", "id": "Regulasi Tegangan Baik, Penekanan Derau." },
  { "en": "Apa Kerugian Ferroresonant Transformer?", "id": "Sensitif Terhadap Variasi Frekuensi." },
  { "en": "Apa Itu Motor-Generator Set?", "id": "Mengisolasi Beban Secara Mekanis." },
  { "en": "Apa Itu Sag-Proofing?", "id": "Membuat Peralatan Tahan Terhadap Sag." },
  { "en": "Apa Itu Voltage Source Inverter (VSI)?", "id": "Inverter Berbasis Sumber Tegangan." },
  { "en": "Apa Itu Pulse Width Modulation (PWM)?", "id": "Teknik Switching Untuk Mengontrol Output." },
  { "en": "Apa Itu Variable Frequency Drive (VFD)?", "id": "Mengontrol Kecepatan Motor AC." },
  { "en": "Apakah VFD (Variable Frequency Drive) Beban Linier?", "id": "Tidak, VFD Adalah Beban Non-linier." },
  { "en": "Apa Itu DC Link?", "id": "Tahap DC Dalam Konverter." },
  { "en": "Apa Fungsi Kapasitor DC Link?", "id": "Menstabilkan Tegangan DC." },
  { "en": "Apa Itu Pengereman Regeneratif?", "id": "Mengembalikan Energi Pengereman." },
  { "en": "Apa Itu Switching Frekuensi Tinggi?", "id": "Menghasilkan Harmonik Frekuensi Tinggi." },
  { "en": "Apa Itu EMI (Electromagnetic Interference) Filter?", "id": "Menekan Gangguan Frekuensi Radio." },
  { "en": "Apa Itu Gangguan Terkonduksi?", "id": "EMI Merambat Melalui Kabel." },
  { "en": "Apa Itu Gangguan Terdiasi?", "id": "EMI Merambat Melalui Udara." },
  { "en": "Apa Itu Kompatibilitas Elektromagnetik (EMC)?", "id": "Kemampuan Perangkat Berfungsi." },
  { "en": "Apa Itu Ground Loop?", "id": "Jalur Arus Tidak Diinginkan." },
  { "en": "Apa Itu Single-Point Grounding?", "id": "Semua Ground Terhubung Di Satu Titik." },
  { "en": "Apa Itu Multi-Point Grounding?", "id": "Ground Terhubung Di Banyak Titik." },
  { "en": "Apa Itu Sistem Tenaga Fasa-Tunggal?", "id": "Menggunakan Dua Atau Tiga Kawat." },
  { "en": "Apa Itu Sistem Tenaga Tiga-Fasa?", "id": "Sistem Paling Umum Untuk Transmisi." },
  { "en": "Apa Itu Koneksi Wye (Bintang)?", "id": "Koneksi Tiga-Fasa Dengan Titik Netral." },
  { "en": "Apa Itu Koneksi Delta?", "id": "Koneksi Tiga-Fasa Tanpa Netral." },
  { "en": "Apa Itu Arus Urutan Nol?", "id": "Komponen Arus Yang Sefasa." },
  { "en": "Apakah Arus Urutan Nol Mengalir Di Delta?", "id": "Tidak, Terperangkap Di Dalam Delta." },
  { "en": "Apa Itu Komponen Simetris?", "id": "Metode Analisis Sistem Tidak Seimbang." },
  { "en": "Apa Itu Transformator K-Factor?", "id": "Transformator Untuk Beban Non-linier." },
  { "en": "Apa Itu Derating?", "id": "Mengoperasikan Peralatan Di Bawah Kapasitas." },
  { "en": "Apa Itu Gangguan (Fault)?", "id": "Kondisi Abnormal Seperti Hubung Singkat." },
  { "en": "Apa Itu Arus Gangguan?", "id": "Arus Sangat Tinggi Selama Gangguan." },
  { "en": "Apa Itu Perangkat Proteksi?", "id": "Sekering, Pemutus Sirkuit, Relai." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Saklar Otomatis Untuk Melindungi." },
  { "en": "Apa Itu Sekering (Fuse)?", "id": "Pelindung Sekali Pakai." },
  { "en": "Apa Itu Koordinasi Proteksi?", "id": "Memastikan Perangkat Proteksi Bekerja." },
  { "en": "Apa Itu Recloser?", "id": "Pemutus Sirkuit Yang Menutup Kembali." },
  { "en": "Apa Itu Flicker Meter?", "id": "Instrumen Untuk Mengukur Flicker." },
  { "en": "Apa Itu Short-Circuit Ratio (SCR)?", "id": "Ukuran Kekuatan Sistem Tenaga." },
  { "en": "Apa Itu Sistem Kaku (Stiff System)?", "id": "Sistem Dengan SCR Tinggi." },
  { "en": "Apa Itu Sistem Lemah (Weak System)?", "id": "Sistem Dengan SCR Rendah." },
  { "en": "Apa Itu Islanding?", "id": "Bagian Jaringan Beroperasi Terisolasi." },
  { "en": "Apa Itu Distributed Generation (DG)?", "id": "Pembangkit Listrik Skala Kecil." },
  { "en": "Contoh DG (Distributed Generation) Adalah Apa?", "id": "Panel Surya, Turbin Angin Kecil." },
  { "en": "Bagaimana DG (Distributed Generation) Mempengaruhi Kualitas Daya?", "id": "Dapat Menyebabkan Variasi Tegangan." },
  { "en": "Apa Itu Microgrid?", "id": "Jaringan Listrik Lokal Yang Dapat." },
  { "en": "Apa Itu Smart Grid?", "id": "Jaringan Listrik Dengan Komunikasi." },
  { "en": "Apa Itu Advanced Metering Infrastructure (AMI)?", "id": "Infrastruktur Untuk Meteran Cerdas." },
  { "en": "Apa Itu Meteran Cerdas?", "id": "Meteran Dengan Komunikasi Dua Arah." },
  { "en": "Apa Itu Phasor Measurement Unit (PMU)?", "id": "Mengukur Fasor Tegangan Dan Arus." },
  { "en": "Apa Itu Sinkrofasor?", "id": "Fasor Yang Disinkronkan Dengan Waktu." },
  { "en": "Apa Itu Wide Area Monitoring System (WAMS)?", "id": "Memantau Jaringan Luas Menggunakan PMU." },
  { "en": "Apa Itu Ketidakstabilan Tegangan?", "id": "Penurunan Tegangan Progresif." },
  { "en": "Apa Itu Voltage Collapse?", "id": "Bentuk Ekstrem Ketidakstabilan Tegangan." },
  { "en": "Apa Itu Analisis Aliran Daya?", "id": "Menghitung Tegangan Dan Aliran." },
  { "en": "Apa Itu Jaringan Distribusi?", "id": "Bagian Sistem Tenaga." },
  { "en": "Apa Itu Jaringan Transmisi?", "id": "Menyalurkan Daya Jarak Jauh." },
  { "en": "Apa Itu Gardu Induk?", "id": "Menghubungkan Jaringan Transmisi." },
  { "en": "Apa Itu Sistem SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem Pemantauan Dan Kontrol." },
  { "en": "Apa Itu Studi Kasus Kualitas Daya?", "id": "Analisis Masalah Kualitas Daya." },
  { "en": "Apa Itu Troubleshooting?", "id": "Proses Mengidentifikasi Dan Memperbaiki." },
  { "en": "Apa Itu Analisis Akar Penyebab?", "id": "Menemukan Penyebab Dasar Masalah." },
  { "en": "Apa Itu Osilasi Torsi?", "id": "Osilasi Mekanis Pada Poros Motor." },
  { "en": "Bagaimana Harmonik Menyebabkan Osilasi Torsi?", "id": "Interaksi Antara Medan Magnet Harmonik." },
  { "en": "Apa Itu Gangguan Tegangan?", "id": "Kategori Umum Untuk Sag Dan Swell." },
  { "en": "Apa Itu Karakteristik Beban?", "id": "Bagaimana Beban Menarik Arus." },
  { "en": "Apa Itu Penandatanganan Kualitas Daya?", "id": "Karakteristik Unik Dari Gangguan Tertentu." },
  { "en": "Apa Itu Pemantauan Kualitas Daya Jarak Jauh?", "id": "Mengumpulkan Data Kualitas Daya Dari Jarak." },
  { "en": "Apa Itu Agregasi Data?", "id": "Menggabungkan Data Dari Berbagai Sumber." },
  { "en": "Apa Itu Analisis Statistik?", "id": "Menganalisis Data Menggunakan Metode Statistik." },
  { "en": "Apa Itu Indeks Kualitas Daya?", "id": "Angka Tunggal Yang Mewakili Kualitas." },
  { "en": "Apa Itu Sistem Tenaga Listrik Kapal?", "id": "Jaringan Listrik Terisolasi Di Kapal." },
  { "en": "Apa Tantangan Kualitas Daya Di Kapal?", "id": "Jaringan Lemah, Beban Besar." },
  { "en": "Apa Itu Sistem Tenaga Listrik Pesawat?", "id": "Jaringan Listrik Di Pesawat." },
  { "en": "Berapa Frekuensi Umum Di Pesawat?", "id": "400 Hz." },
  { "en": "Mengapa Frekuensi 400 Hz Digunakan?", "id": "Memungkinkan Transformator Yang Lebih Kecil." },
  { "en": "Apa Itu Ferroresonansi?", "id": "Jenis Overvoltage Non-linier." },
  { "en": "Kapan Ferroresonansi Terjadi?", "id": "Interaksi Antara Trafo Dan Kapasitansi." },
  { "en": "Apa Itu Inrush Current?", "id": "Arus Puncak Saat Transformator Dinyalakan." },
  { "en": "Berapa Besar Inrush Current?", "id": "Bisa Mencapai 10-15 Kali Arus." },
  { "en": "Apa Efek Inrush Current?", "id": "Dapat Menyebabkan Voltage Sag." },
  { "en": "Apa Itu Solid-State Transformer (SST)?", "id": "Transformator Berbasis Elektronika Daya." },
  { "en": "Bagaimana SST (Solid-State Transformer) Dapat Meningkatkan Kualitas Daya?", "id": "Menyediakan Kontrol Tegangan Dan Isolasi." },
  { "en": "Apa Itu Konverter Matriks?", "id": "Konverter AC-AC Langsung." },
  { "en": "Apa Itu Kontrol Prediktif?", "id": "Strategi Kontrol Yang Menggunakan Model." },
  { "en": "Apa Itu Kontrol Logika Fuzzy?", "id": "Menggunakan Logika Fuzzy Untuk Kontrol." },
  { "en": "Apa Itu Jaringan Saraf Tiruan?", "id": "Model Komputasi Untuk Pembelajaran." },
  { "en": "Apa Itu Desain Filter?", "id": "Merancang Filter Untuk Frekuensi Tertentu." },
  { "en": "Apa Itu Faktor Kualitas (Q)?", "id": "Ukuran Kualitas Rangkaian Resonan." },
  { "en": "Filter Dengan Q Tinggi Berarti Apa?", "id": "Sangat Selektif Tetapi Dapat Beresonansi." },
  { "en": "Apa Itu Damping?", "id": "Menambahkan Resistansi Untuk Mengurangi Osilasi." },
  { "en": "Apa Itu Teori Daya Reaktif Sesaat?", "id": "Juga Dikenal Sebagai Teori P-Q." },
  { "en": "Untuk Apa Teori P-Q Digunakan?", "id": "Untuk Mengontrol Filter Daya Aktif." },
  { "en": "Apa Itu Synchronous Reference Frame (SRF) Theory?", "id": "Metode Kontrol Berbasis Transformasi D-Q." },
  { "en": "Apa Itu Transformasi Clarke?", "id": "Mengubah Besaran Tiga-Fasa Menjadi Dua-Fasa." },
  { "en": "Apa Itu Transformasi Park?", "id": "Mengubah Besaran Stasioner Menjadi Berputar." },
  { "en": "Apa Itu Kerangka Referensi D-Q?", "id": "Kerangka Referensi Yang Berputar." },
  { "en": "Apa Itu Phase-Locked Loop (PLL)?", "id": "Sirkuit Untuk Sinkronisasi Dengan Jaringan." },
  { "en": "Apa Itu Sinyal Umpan Balik?", "id": "Sinyal Output Yang Digunakan Untuk Kontrol." },
  { "en": "Apa Itu Sistem Loop Tertutup?", "id": "Sistem Kontrol Dengan Umpan Balik." },
  { "en": "Apa Itu Stabilitas Loop?", "id": "Kemampuan Sistem Untuk Tetap Stabil." },
  { "en": "Apa Itu Diagram Bode?", "id": "Plot Penguatan Dan Fasa." },
  { "en": "Apa Itu Gain Margin?", "id": "Ukuran Kestabilan Relatif." },
  { "en": "Apa Itu Phase Margin?", "id": "Ukuran Kestabilan Relatif Lainnya." },
  { "en": "Apa Itu Propagasi Gangguan?", "id": "Bagaimana Gangguan Menyebar Melalui Sistem." },
  { "en": "Apa Itu Kekebalan Peralatan?", "id": "Kemampuan Peralatan Menahan Gangguan." },
  { "en": "Apa Itu Kompatibilitas?", "id": "Kemampuan Berbagai Peralatan." },
  { "en": "Apa Itu Kontrak Kualitas Daya?", "id": "Perjanjian Antara Utilitas Dan Pelanggan." },
  { "en": "Apa Itu Benchmarking Kualitas Daya?", "id": "Membandingkan Kinerja Dengan Standar." },
  { "en": "Apa Itu Audit Kualitas Daya?", "id": "Evaluasi Komprehensif Terhadap Sistem." },
  { "en": "Apa Itu Simpangan (Excursion)?", "id": "Penyimpangan Dari Nilai Normal." },
  { "en": "Apa Itu Durasi Peristiwa?", "id": "Lamanya Waktu Suatu Peristiwa." },
  { "en": "Apa Itu Magnitudo Peristiwa?", "id": "Besarnya Penyimpangan." },
  { "en": "Apa Itu Bentuk Gelombang Distorsi?", "id": "Gelombang Yang Tidak Berbentuk Sinus." },
  { "en": "Apa Itu Pemotongan Puncak (Peak Clipping)?", "id": "Puncak Gelombang Sinus Terpotong." },
  { "en": "Apa Penyebab Pemotongan Puncak?", "id": "Saturasi Transformator Atau Beban." },
  { "en": "Apa Itu Perataan Dasar (Flat-topping)?", "id": "Bentuk Lain Dari Distorsi." },
  { "en": "Apa Itu Arus Bocor?", "id": "Arus Yang Mengalir Ke Ground." },
  { "en": "Apa Itu Ground Fault Circuit Interrupter (GFCI)?", "id": "Perangkat Keselamatan Terhadap Arus Bocor." },
  { "en": "Apa Itu Sistem Pembumian?", "id": "Koneksi Ke Massa Bumi." },
  { "en": "Apa Itu Sistem TN?", "id": "Sistem Pembumian Dengan Netral." },
  { "en": "Apa Itu Sistem TT?", "id": "Sistem Pembumian Dengan Elektroda." },
  { "en": "Apa Itu Sistem IT?", "id": "Sistem Pembumian Terisolasi." },
  { "en": "Apa Itu Ikatan Ekuipotensial?", "id": "Menghubungkan Semua Bagian Konduktif." },
  { "en": "Apa Itu Impedansi Ground?", "id": "Impedansi Jalur Ke Ground." },
  { "en": "Apa Itu Pengukuran Empat Titik?", "id": "Metode Untuk Mengukur Impedansi." },
  { "en": "Apa Itu Osilasi?", "id": "Fluktuasi Periodik." },
  { "en": "Apa Itu Damping?", "id": "Pengurangan Amplitudo Osilasi." },
  { "en": "Apa Itu Sirkuit RLC?", "id": "Sirkuit Dengan Resistor, Induktor, Kapasitor." },
  { "en": "Apa Itu Respon Transien?", "id": "Perilaku Sistem Setelah Gangguan." },
  { "en": "Apa Itu Keadaan Tunak (Steady State)?", "id": "Perilaku Sistem Setelah Transien." },
  { "en": "Apa Itu Mitigasi?", "id": "Tindakan Untuk Mengurangi Masalah." },
  { "en": "Apa Itu Penyearah Terkendali?", "id": "Penyearah Yang Outputnya Dapat Diatur." },
  { "en": "Apa Itu Sudut Penyalaan?", "id": "Sudut Untuk Memicu Thyristor." },
  { "en": "Apa Itu Inverter?", "id": "Mengubah DC Menjadi AC." },
  { "en": "Apa Itu Inverter Sumber Tegangan?", "id": "Inverter Dengan Sumber DC Tegangan." },
  { "en": "Apa Itu Inverter Sumber Arus?", "id": "Inverter Dengan Sumber DC Arus." },
  { "en": "Apa Itu Modulasi Lebar Pulsa?", "id": "Teknik Switching Untuk Inverter." },
  { "en": "Apa Itu Frekuensi Switching?", "id": "Laju Saklar Dihidupkan Dan Dimatikan." },
  { "en": "Apa Itu Rangkaian Snubber?", "id": "Melindungi Saklar Dari Lonjakan." },
  { "en": "Apa Itu Saklar Elektronik Daya?", "id": "Transistor, Thyristor." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor)?", "id": "Saklar Cepat Untuk Aplikasi." },
  { "en": "Apa Itu IGBT (Insulated-Gate Bipolar Transistor)?", "id": "Saklar Untuk Aplikasi Daya Tinggi." },
  { "en": "Apa Itu Thyristor?", "id": "Saklar Semi-terkendali." },
  { "en": "Apa Itu Commutation?", "id": "Proses Mematikan Thyristor." },
  { "en": "Apa Itu Natural Commutation?", "id": "Komutasi Alami Dalam Jaringan AC." },
  { "en": "Apa Itu Forced Commutation?", "id": "Memaksa Thyristor Mati." },
  { "en": "Apa Itu Efisiensi Konverter?", "id": "Rasio Daya Output Terhadap Input." },
  { "en": "Apa Itu Kerugian Switching?", "id": "Kehilangan Daya Selama Transisi." },
  { "en": "Apa Itu Kerugian Konduksi?", "id": "Kehilangan Daya Saat Saklar." },
  { "en": "Apa Itu Soft Switching?", "id": "Teknik Mengurangi Kerugian Switching." },
  { "en": "Apa Itu Zero Voltage Switching (ZVS)?", "id": "Switching Saat Tegangan Nol." },
  { "en": "Apa Itu Zero Current Switching (ZCS)?", "id": "Switching Saat Arus Nol." },
  { "en": "Apa Itu Konverter Resonan?", "id": "Menggunakan Resonansi Untuk Soft Switching." },
  { "en": "Apa Itu Manajemen Termal?", "id": "Mengelola Panas Yang Dihasilkan." },
  { "en": "Apa Itu Heatsink?", "id": "Perangkat Untuk Mendinginkan Komponen." },
  { "en": "Apa Itu Resistansi Termal?", "id": "Hambatan Terhadap Aliran Panas." },
  { "en": "Apa Itu Studi Kasus?", "id": "Analisis Terperinci Masalah Nyata." },
  { "en": "Apa Itu Klasifikasi Peristiwa?", "id": "Mengidentifikasi Jenis Gangguan." },
  { "en": "Apa Itu Pembelajaran Mesin?", "id": "AI Yang Belajar Dari Data." },
  { "en": "Bagaimana Pembelajaran Mesin Digunakan?", "id": "Untuk Klasifikasi Peristiwa Kualitas Daya." },
  { "en": "Apa Itu Kompensasi?", "id": "Tindakan Untuk Memperbaiki Masalah." },
  { "en": "Apa Itu Pengkondisian Daya?", "id": "Meningkatkan Kualitas Daya Listrik." },
  { "en": "Apa Itu Sistem Tenaga Listrik?", "id": "Jaringan Untuk Menyalurkan Listrik." },
  { "en": "Apa Itu Pembangkitan?", "id": "Proses Menghasilkan Energi Listrik." },
  { "en": "Apa Itu Transmisi?", "id": "Menyalurkan Listrik Jarak Jauh." },
  { "en": "Apa Itu Distribusi?", "id": "Menyalurkan Listrik Ke Pengguna." },
  { "en": "Apa Itu Beban Listrik?", "id": "Peralatan Yang Mengkonsumsi Listrik." },
  { "en": "Apa Itu Jaringan Listrik (Grid)?", "id": "Jaringan Transmisi Dan Distribusi." },
  { "en": "Apa Itu Keandalan (Reliability)?", "id": "Kemampuan Menyediakan Listrik." },
  { "en": "Apa Itu Indeks Keandalan SAIDI (System Average Interruption Duration Index)?", "id": "Indeks Durasi Interupsi Rata-rata." },
  { "en": "Apa Itu Indeks Keandalan SAIFI (System Average Interruption Frequency Index)?", "id": "Indeks Frekuensi Interupsi Rata-rata." },
  { "en": "Apa Itu Pemadaman (Blackout)?", "id": "Kehilangan Daya Total." },
  { "en": "Apa Itu Brownout?", "id": "Penurunan Tegangan Yang Disengaja." },
  { "en": "Apa Itu Analisis Gelombang (Waveform)?", "id": "Mempelajari Bentuk Sinyal Listrik." },
  { "en": "Apa Itu Distorsi?", "id": "Penyimpangan Dari Bentuk Gelombang." },
  { "en": "Apa Itu Fasor?", "id": "Representasi Vektor Dari Gelombang." },
  { "en": "Apa Itu Magnitudo Fasor?", "id": "Mewakili Amplitudo Gelombang." },
  { "en": "Apa Itu Sudut Fasor?", "id": "Mewakili Fasa Gelombang." },
  { "en": "Apa Itu Diagram Fasor?", "id": "Diagram Yang Menunjukkan Hubungan." },
  { "en": "Apa Itu Beban Resistif?", "id": "Tegangan Dan Arus Sefasa." },
  { "en": "Apa Itu Beban Induktif?", "id": "Arus Tertinggal Dari Tegangan." },
  { "en": "Apa Itu Beban Kapasitif?", "id": "Arus Mendahului Tegangan." },
  { "en": "Apa Itu Impedansi?", "id": "Ukuran Total Hambatan Arus." },
  { "en": "Apa Itu Resistansi?", "id": "Bagian Nyata Dari Impedansi." },
  { "en": "Apa Itu Reaktansi?", "id": "Bagian Imajiner Dari Impedansi." },
  { "en": "Apa Itu Reaktansi Induktif?", "id": "Reaktansi Yang Disebabkan Induktor." },
  { "en": "Apa Itu Reaktansi Kapasitif?", "id": "Reaktansi Yang Disebabkan Kapasitor." },
  { "en": "Apa Itu Frekuensi Resonansi?", "id": "Saat Reaktansi Induktif Sama." },
  { "en": "Apa Itu Segitiga Daya?", "id": "Hubungan Grafis Antara Jenis." },
  { "en": "Apa Itu Faktor Daya Tertinggal (Lagging)?", "id": "Disebabkan Oleh Beban Induktif." },
  { "en": "Apa Itu Faktor Daya Mendahului (Leading)?", "id": "Disebabkan Oleh Beban Kapasitif." },
  { "en": "Apa Itu Faktor Daya Sejati?", "id": "Memperhitungkan Efek Harmonik." },
  { "en": "Apa Itu Faktor Daya Perpindahan?", "id": "Faktor Daya Pada Frekuensi." },
  { "en": "Apa Itu Model Sistem Tenaga?", "id": "Representasi Matematis Dari Jaringan." },
  { "en": "Apa Itu Analisis Aliran Beban?", "id": "Nama Lain Untuk Analisis." },
  { "en": "Apa Itu Analisis Sirkuit Pendek?", "id": "Menganalisis Arus Selama Gangguan." },
  { "en": "Apa Itu Analisis Stabilitas?", "id": "Mempelajari Kemampuan Sistem." },
  { "en": "Apa Itu Stabilitas Sudut Rotor?", "id": "Kemampuan Generator Tetap Sinkron." },
  { "en": "Apa Itu Stabilitas Transien?", "id": "Respons Sistem Terhadap Gangguan." },
  { "en": "Apa Itu Stabilitas Sinyal Kecil?", "id": "Respons Sistem Terhadap Gangguan." },
  { "en": "Apa Itu Osilasi Daya?", "id": "Fluktuasi Daya Antar Generator." },
  { "en": "Apa Itu Peredaman (Damping)?", "id": "Pengurangan Amplitudo Osilasi." },
  { "en": "Apa Itu Power System Stabilizer (PSS)?", "id": "Kontroler Untuk Meredam Osilasi." },
  { "en": "Apa Itu Switchgear?", "id": "Kombinasi Saklar Dan Pemutus." },
  { "en": "Apa Itu Busbar?", "id": "Konduktor Logam Untuk Distribusi." },
  { "en": "Apa Itu Isolator?", "id": "Bahan Untuk Mencegah Aliran." },
  { "en": "Apa Itu Surge Arrester?", "id": "Melindungi Peralatan Dari Petir." },
  { "en": "Apa Itu Relai Proteksi?", "id": "Mendeteksi Gangguan Dan Mengisolasi." },
  { "en": "Apa Itu Relai Arus Lebih?", "id": "Bekerja Berdasarkan Tingkat Arus." },
  { "en": "Apa Itu Relai Diferensial?", "id": "Membandingkan Arus Masuk Dan Keluar." },
  { "en": "Apa Itu Relai Jarak?", "id": "Melindungi Jalur Transmisi." },
  { "en": "Apa Itu Zona Proteksi?", "id": "Area Yang Dilindungi Oleh Relai." },
  { "en": "Apa Itu Kecepatan Operasi Relai?", "id": "Seberapa Cepat Relai Bekerja." },
  { "en": "Apa Itu Sensitivitas Relai?", "id": "Tingkat Minimum Untuk Operasi." },
  { "en": "Apa Itu Selektivitas?", "id": "Kemampuan Mengisolasi Bagian." },
  { "en": "Apa Itu Sistem SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem Kontrol Jarak Jauh." },
  { "en": "Apa Itu Remote Terminal Unit (RTU)?", "id": "Perangkat Di Lapangan." },
  { "en": "Apa Itu Master Station?", "id": "Pusat Kontrol Sistem SCADA." },
  { "en": "Apa Itu Protokol Komunikasi?", "id": "Aturan Untuk Pertukaran Data." },
  { "en": "Apa Itu DNP3 (Distributed Network Protocol)?", "id": "Protokol Umum Untuk SCADA." },
  { "en": "Apa Itu IEC (International Electrotechnical Commission) 61850?", "id": "Standar Komunikasi Untuk Gardu." },
  { "en": "Apa Itu Kualitas Pelayanan?", "id": "Ukuran Kinerja Penyedia Utilitas." },
  { "en": "Apa Itu Ekonomi Kualitas Daya?", "id": "Analisis Biaya Dan Manfaat." },
  { "en": "Apa Itu Biaya Gangguan?", "id": "Kerugian Finansial Akibat Kualitas." },
  { "en": "Apa Itu Klasifikasi Beban?", "id": "Mengelompokkan Beban Berdasarkan." },
  { "en": "Apa Itu Beban Kritis?", "id": "Beban Yang Membutuhkan Kualitas." },
  { "en": "Apa Itu Desain Toleran Gangguan?", "id": "Desain Sistem Yang Tahan." },
  { "en": "Apa Itu Topologi Jaringan?", "id": "Struktur Koneksi Jaringan." },
  { "en": "Apa Itu Jaringan Radial?", "id": "Memiliki Satu Jalur Suplai." },
  { "en": "Apa Itu Jaringan Loop?", "id": "Memiliki Dua Jalur Suplai." },
  { "en": "Apa Itu Jaringan Jala (Mesh)?", "id": "Memiliki Banyak Jalur Suplai." },
  { "en": "Jaringan Mana Yang Paling Andal?", "id": "Jaringan Jala." },
  { "en": "Apa Itu Sumber Daya Energi Terdistribusi (DER)?", "id": "Nama Lain Untuk Distributed." },
  { "en": "Apa Itu Co-generation?", "id": "Menghasilkan Listrik Dan Panas." },
  { "en": "Apa Itu Tri-generation?", "id": "Menghasilkan Listrik, Panas, Dingin." },
  { "en": "Apa Itu Kendaraan Listrik (EV)?", "id": "Kendaraan Yang Digerakkan Listrik." },
  { "en": "Bagaimana EV (Electric Vehicle) Mempengaruhi Kualitas Daya?", "id": "Pengisian Dapat Menyebabkan Harmonik." },
  { "en": "Apa Itu Vehicle-to-Grid (V2G)?", "id": "Mobil Listrik Menyediakan Daya." },
  { "en": "Apa Itu Pengisian Cerdas (Smart Charging)?", "id": "Mengontrol Pengisian EV." },
  { "en": "Apa Itu Penyimpanan Energi?", "id": "Menyimpan Energi Untuk Digunakan." },
  { "en": "Apa Itu Baterai?", "id": "Menyimpan Energi Kimia." },
  { "en": "Apa Itu Superkapasitor?", "id": "Menyimpan Energi Dalam Medan." },
  { "en": "Apa Itu Flywheel?", "id": "Menyimpan Energi Kinetik." },
  { "en": "Apa Itu Compressed Air Energy Storage (CAES)?", "id": "Menyimpan Energi Dalam Udara." },
  { "en": "Apa Itu Pumped Hydro Storage?", "id": "Menyimpan Energi Dalam Air." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Surya?", "id": "Mengubah Sinar Matahari." },
  { "en": "Apa Itu Inverter Surya?", "id": "Mengubah DC Dari Panel Surya." },
  { "en": "Apa Itu Maximum Power Point Tracking (MPPT)?", "id": "Mendapatkan Daya Maksimal." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Angin?", "id": "Mengubah Energi Angin." },
  { "en": "Apa Itu Turbin Angin?", "id": "Mesin Yang Menangkap Angin." },
  { "en": "Apa Itu Generator Induksi?", "id": "Jenis Generator Umum." },
  { "en": "Apa Itu Generator Sinkron?", "id": "Generator Yang Berputar Sinkron." },
  { "en": "Apa Itu Penyearah Jembatan Tak Terkendali?", "id": "Penyearah Yang Menggunakan Dioda." },
  { "en": "Apa Itu Penyearah Jembatan Terkendali?", "id": "Penyearah Yang Menggunakan Thyristor." },
  { "en": "Apa Itu Faktor Tuntutan (Demand Factor)?", "id": "Rasio Tuntutan Maksimum." },
  { "en": "Apa Itu Faktor Beban (Load Factor)?", "id": "Rasio Beban Rata-rata." },
  { "en": "Apa Itu Faktor Keanekaragaman (Diversity Factor)?", "id": "Rasio Jumlah Tuntutan." },
  { "en": "Apa Itu Pemodelan Beban?", "id": "Representasi Matematis Dari Beban." },
  { "en": "Apa Itu Model Beban Statis?", "id": "Beban Sebagai Fungsi Tegangan." },
  { "en": "Apa Itu Model Beban Dinamis?", "id": "Beban Sebagai Fungsi Waktu." },
  { "en": "Apa Itu ZIP Model?", "id": "Model Beban (Constant Z, I, P)." },
  { "en": "Apa Itu Analisis Harmonik?", "id": "Mempelajari Efek Dari Harmonik." },
  { "en": "Apa Itu Aliran Daya Harmonik?", "id": "Menganalisis Aliran Harmonik." },
  { "en": "Apa Itu Impedansi Harmonik?", "id": "Impedansi Sistem Pada Frekuensi." },
  { "en": "Apa Itu Sumber Harmonik?", "id": "Peralatan Yang Menghasilkan Harmonik." },
  { "en": "Apa Itu Respon Frekuensi Sistem?", "id": "Bagaimana Sistem Merespons Frekuensi." },
  { "en": "Apa Itu Plot Impedansi?", "id": "Grafik Impedansi Terhadap Frekuensi." },
  { "en": "Apa Itu Anti-Resonansi?", "id": "Kondisi Impedansi Tinggi." },
  { "en": "Apa Itu Pemodelan Transformator?", "id": "Representasi Matematis Transformator." },
  { "en": "Apa Itu Model Rangkaian Ekuivalen?", "id": "Sirkuit Sederhana Yang Mewakili." },
  { "en": "Apa Itu Arus Magnetisasi?", "id": "Arus Untuk Menciptakan Fluks." },
  { "en": "Apa Itu Saturasi Inti?", "id": "Inti Tidak Dapat Menahan." },
  { "en": "Apa Efek Saturasi?", "id": "Menghasilkan Arus Harmonik." },
  { "en": "Apa Itu Histeresis?", "id": "Fenomena Magnetik Dalam Inti." },
  { "en": "Apa Itu Kerugian Inti?", "id": "Kerugian Histeresis Dan Arus." },
  { "en": "Apa Itu Pemodelan Saluran Transmisi?", "id": "Mewakili Saluran Untuk Analisis." },
  { "en": "Apa Itu Model Pi?", "id": "Model Rangkaian Ekuivalen." },
  { "en": "Apa Itu Model Garis Panjang?", "id": "Model Akurat Untuk Saluran." },
  { "en": "Apa Itu Parameter Terdistribusi?", "id": "Resistansi, Induktansi, Kapasitansi." },
  { "en": "Apa Itu Gelombang Berjalan?", "id": "Gelombang Yang Merambat." },
  { "en": "Apa Itu Pantulan Gelombang?", "id": "Terjadi Akibat Ketidakcocokan." },
  { "en": "Apa Itu Koefisien Pantul?", "id": "Rasio Gelombang Pantul." },
  { "en": "Apa Itu Lattice Diagram?", "id": "Diagram Untuk Menganalisis." },
  { "en": "Apa Itu Electromagnetic Transients Program (EMTP)?", "id": "Perangkat Lunak Simulasi." },
  { "en": "Apa Itu Pensinyalan Jalur Daya?", "id": "Mengirim Sinyal Kontrol." },
  { "en": "Apa Itu Ripple Control?", "id": "Contoh Pensinyalan Jalur." },
  { "en": "Apa Itu Gelombang Elektromagnetik?", "id": "Perambatan Medan Listrik." },
  { "en": "Apa Itu Kompatibilitas Elektromagnetik (EMC)?", "id": "Kemampuan Perangkat Bekerja." },
  { "en": "Apa Itu Interferensi Elektromagnetik (EMI)?", "id": "Gangguan Elektromagnetik." },
  { "en": "Apa Itu Emisi?", "id": "Pelepasan Energi EM." },
  { "en": "Apa Itu Kekebalan?", "id": "Kemampuan Menahan Gangguan." },
  { "en": "Apa Itu Kopling?", "id": "Transfer Energi Gangguan." },
  { "en": "Apa Itu Grounding Loop?", "id": "Nama Lain Untuk Ground Loop." },
  { "en": "Apa Itu Kualitas Sinyal?", "id": "Ukuran Kualitas Sinyal." },
  { "en": "Apa Itu Jitter?", "id": "Variasi Waktu Sinyal." },
  { "en": "Apa Itu Derau Fasa?", "id": "Representasi Jitter Dalam Frekuensi." },
  { "en": "Apa Itu Sistem Tenaga DC?", "id": "Menggunakan Arus Searah." },
  { "en": "Apa Keuntungan Sistem DC?", "id": "Tidak Ada Daya Reaktif." },
  { "en": "Apa Itu Konverter DC-DC?", "id": "Mengubah Level Tegangan DC." },
  { "en": "Apa Itu Konverter AC-DC?", "id": "Nama Lain Penyearah." },
  { "en": "Apa Itu Konverter DC-AC?", "id": "Nama Lain Inverter." },
  { "en": "Apa Itu Konverter AC-AC?", "id": "Mengubah Tegangan Atau Frekuensi." },
  { "en": "Apa Itu Sistem Tenaga Hibrida?", "id": "Menggabungkan Sumber AC Dan DC." },
  { "en": "Apa Itu Inverter Siap Jaringan?", "id": "Inverter Dengan Fitur Pendukung." },
  { "en": "Apa Itu Kontrol Volt-VAR?", "id": "Mengatur Tegangan Dengan Daya." },
  { "en": "Apa Itu Kontrol Frekuensi-Watt?", "id": "Mengatur Daya Untuk Menstabilkan." },
  { "en": "Apa Itu Ride-Through Capability?", "id": "Kemampuan Tetap Terhubung." },
  { "en": "Apa Itu Low Voltage Ride-Through (LVRT)?", "id": "Melewati Gangguan Tegangan Rendah." },
  { "en": "Apa Itu High Voltage Ride-Through (HVRT)?", "id": "Melewati Gangguan Tegangan Tinggi." },
  { "en": "Apa Itu Ketidakstabilan?", "id": "Sistem Tidak Kembali Ke." },
  { "en": "Apa Itu Osilasi Sub-Sinkron?", "id": "Osilasi Di Bawah Frekuensi." },
  { "en": "Apa Itu Kontrol Tambahan?", "id": "Sistem Kontrol Untuk Meredam." },
  { "en": "Apa Itu Teori Kontrol?", "id": "Cabang Teknik Dan Matematika." },
  { "en": "Apa Itu Kontrol Klasik?", "id": "Berbasis Fungsi Transfer." },
  { "en": "Apa Itu Kontrol Modern?", "id": "Berbasis Ruang Keadaan." },
  { "en": "Apa Itu Ruang Keadaan?", "id": "Representasi Matematis Sistem." },
  { "en": "Apa Itu Kontrol Umpan Balik?", "id": "Menggunakan Output Untuk Mengoreksi." },
  { "en": "Apa Itu Kontrol Umpan Maju?", "id": "Mengantisipasi Gangguan." },
  { "en": "Apa Itu Kontrol Adaptif?", "id": "Kontroler Yang Menyesuaikan." },
  { "en": "Apa Itu Kontrol Kokoh?", "id": "Kontroler Yang Tahan." },
  { "en": "Apa Itu Kontrol Optimal?", "id": "Mengoptimalkan Kriteria Kinerja." },
  { "en": "Apa Itu Estimasi Keadaan?", "id": "Memperkirakan Keadaan Internal." },
  { "en": "Apa Itu Filter Kalman?", "id": "Estimator Optimal Untuk Sistem." },
  { "en": "Apa Itu Artificial Intelligence (AI)?", "id": "Kecerdasan Buatan." },
  { "en": "Apa Itu Jaringan Saraf Tiruan?", "id": "Model Pembelajaran Mesin." },
  { "en": "Apa Itu Logika Fuzzy?", "id": "Logika Untuk Menangani." },
  { "en": "Apa Itu Algoritma Genetik?", "id": "Algoritma Optimisasi." },
  { "en": "Apa Itu Sistem Pakar?", "id": "Sistem Berbasis Pengetahuan." },
  { "en": "Apa Itu Data Mining?", "id": "Menemukan Pola Dalam Data." },
  { "en": "Apa Itu Big Data?", "id": "Kumpulan Data Sangat Besar." },
  { "en": "Apa Itu Klasifikasi?", "id": "Mengelompokkan Data." },
  { "en": "Apa Itu Regresi?", "id": "Memprediksi Nilai Kontinu." },
  { "en": "Apa Itu Klastering?", "id": "Menemukan Grup Dalam Data." },
  { "en": "Apa Itu Penilaian Kualitas Daya?", "id": "Mengevaluasi Tingkat Kualitas Daya." },
  { "en": "Apa Itu Diagnostik?", "id": "Mengidentifikasi Penyebab Masalah." },
  { "en": "Apa Itu Prognostik?", "id": "Memprediksi Masalah Di Masa." },
  { "en": "Apa Itu Kesehatan Sistem?", "id": "Ukuran Kondisi Sistem." },
  { "en": "Apa Itu Keandalan?", "id": "Probabilitas Beroperasi Tanpa." },
  { "en": "Apa Itu Ketersediaan?", "id": "Proporsi Waktu Sistem." },
  { "en": "Apa Itu Keterpeliharaan?", "id": "Kemudahan Dalam Memperbaiki." },
  { "en": "Apa Itu FMEA (Failure Mode and Effects Analysis)?", "id": "Analisis Mode Dan Efek." },
  { "en": "Apa Itu FTA (Fault Tree Analysis)?", "id": "Analisis Pohon Kesalahan." },
  { "en": "Apa Itu Keamanan Siber?", "id": "Melindungi Sistem Dari Serangan." },
  { "en": "Apa Itu Kerentanan?", "id": "Kelemahan Yang Dapat." },
  { "en": "Apa Itu Ancaman?", "id": "Sesuatu Yang Dapat." },
  { "en": "Apa Itu Risiko?", "id": "Kemungkinan Kerugian." },
  { "en": "Apa Itu Kontrol Keamanan?", "id": "Tindakan Untuk Mengurangi Risiko." },
  { "en": "Apa Itu Enkripsi?", "id": "Mengamankan Data." },
  { "en": "Apa Itu Otentikasi?", "id": "Memverifikasi Identitas." },
  { "en": "Apa Itu Otorisasi?", "id": "Memberikan Izin." },
  { "en": "Apa Itu Tanda Tangan Digital?", "id": "Memastikan Keaslian Dan Integritas." },
  { "en": "Apa Itu Standar?", "id": "Spesifikasi Yang Disepakati." },
  { "en": "Apa Itu Regulasi?", "id": "Aturan Yang Diberlakukan." },
  { "en": "Apa Itu Kepatuhan?", "id": "Mematuhi Standar Dan Regulasi." },
  { "en": "Apa Itu Deregulasi?", "id": "Mengurangi Atau Menghilangkan." },
  { "en": "Apa Itu Pasar Listrik?", "id": "Tempat Jual Beli Listrik." },
  { "en": "Apa Itu Harga Pasar Real-Time?", "id": "Harga Listrik Yang Berubah." },
  { "en": "Apa Itu Manajemen Sisi Permintaan (DSM)?", "id": "Mempengaruhi Pola Konsumsi Energi." },
  { "en": "Apa Itu Efisiensi Energi?", "id": "Menggunakan Lebih Sedikit Energi." },
  { "en": "Apa Itu Konservasi Energi?", "id": "Mengurangi Penggunaan Energi." },
  { "en": "Apa Itu Gelombang Tegangan?", "id": "Bentuk Gelombang Tegangan AC." },
  { "en": "Apa Itu Gelombang Arus?", "id": "Bentuk Gelombang Arus AC." },
  { "en": "Apa Itu Distorsi Notch?", "id": "Nama Lain Untuk Notching." },
  { "en": "Apa Itu Komutasi?", "id": "Proses Transfer Arus." },
  { "en": "Apa Itu Penyearah Terkendali Fasa?", "id": "Sumber Umum Harmonik." },
  { "en": "Apa Itu Inverter Sumber Tegangan?", "id": "Sumber Harmonik Lainnya." },
  { "en": "Apa Itu Switch-Mode Power Supply (SMPS)?", "id": "Catu Daya Yang Menghasilkan." },
  { "en": "Apa Itu Arus Input SMPS?", "id": "Berbentuk Pulsa Dan Kaya." },
  { "en": "Apa Itu Tungku Busur Listrik?", "id": "Beban Besar Yang Menyebabkan." },
  { "en": "Apa Itu Mesin Las?", "id": "Beban Yang Menyebabkan Fluktuasi." },
  { "en": "Apa Itu Saturasi Magnetik?", "id": "Fenomena Non-linier Dalam Trafo." },
  { "en": "Apa Itu Arus Magnetisasi?", "id": "Arus Untuk Membangkitkan Fluks." },
  { "en": "Apa Itu Inrush Current?", "id": "Arus Puncak Saat Menyalakan." },
  { "en": "Apa Itu Motor Induksi?", "id": "Beban Umum Di Industri." },
  { "en": "Apakah Motor Induksi Linier?", "id": "Cukup Linier Saat Berjalan." },
  { "en": "Apa Itu Arus Awal Motor?", "id": "Arus Sangat Tinggi Saat." },
  { "en": "Apa Itu Soft Starter?", "id": "Mengurangi Arus Awal Motor." },
  { "en": "Apa Itu Variable Frequency Drive (VFD)?", "id": "Mengontrol Kecepatan Motor." },
  { "en": "Apakah VFD Linier?", "id": "Tidak, VFD Adalah Beban." },
  { "en": "Apa Itu Lampu Fluoresen?", "id": "Beban Non-linier." },
  { "en": "Apa Itu Ballast Elektronik?", "id": "Catu Daya Untuk Lampu." },
  { "en": "Apa Itu Peralatan Medis?", "id": "Seringkali Sensitif Terhadap Kualitas." },
  { "en": "Apa Itu Pusat Data?", "id": "Sangat Kritis Terhadap Kualitas." },
  { "en": "Apa Itu Redundansi?", "id": "Memiliki Sistem Cadangan." },
  { "en": "Apa Itu Sistem N+1?", "id": "Satu Unit Cadangan." },
  { "en": "Apa Itu Titik Kegagalan Tunggal?", "id": "Komponen Yang Kegagalannya." },
  { "en": "Apa Itu Analisis Statistik?", "id": "Menganalisis Data Kualitas Daya." },
  { "en": "Apa Itu Histogram?", "id": "Grafik Distribusi Frekuensi." },
  { "en": "Apa Itu Diagram Sebar (Scatter Plot)?", "id": "Grafik Hubungan Antara Dua." },
  { "en": "Apa Itu Kurva Durasi?", "id": "Menunjukkan Waktu Suatu Parameter." },
  { "en": "Apa Itu ITI (CBEMA) Curve?", "id": "Kurva Toleransi Tegangan." },
  { "en": "Apa Itu Kepatuhan Kualitas Daya?", "id": "Memenuhi Standar Dan Regulasi." },
  { "en": "Apa Itu Investigasi Kualitas Daya?", "id": "Menentukan Penyebab Masalah." },
  { "en": "Apa Itu Sumber Gangguan?", "id": "Peralatan Yang Menyebabkan." },
  { "en": "Apa Itu Jalur Propagasi?", "id": "Jalan Gangguan Menyebar." },
  { "en": "Apa Itu Korban Gangguan?", "id": "Peralatan Yang Terpengaruh." },
  { "en": "Apa Itu Deteksi Arah Gangguan?", "id": "Menentukan Dari Mana Gangguan." },
  { "en": "Apa Itu Pemecahan Masalah?", "id": "Proses Menemukan Dan Memperbaiki." },
  { "en": "Apa Itu Osiloskop?", "id": "Instrumen Untuk Melihat Bentuk." },
  { "en": "Apa Itu Multimeter True RMS?", "id": "Mengukur Nilai RMS Akurat." },
  { "en": "Apa Itu Penganalisis Spektrum?", "id": "Melihat Sinyal Dalam Domain." },
  { "en": "Apa Itu Logger Data?", "id": "Merekam Pengukuran Seiring Waktu." },
  { "en": "Apa Itu Pemicuan (Triggering)?", "id": "Memulai Pengukuran Berdasarkan." },
  { "en": "Apa Itu Pemicu Ambang Batas?", "id": "Pemicu Saat Sinyal Melewati." },
  { "en": "Apa Itu Pengambilan Sampel?", "id": "Mengubah Sinyal Analog." },
  { "en": "Apa Itu Aliasing?", "id": "Distorsi Akibat Laju." },
  { "en": "Apa Itu Jendela Waktu?", "id": "Durasi Pengamatan Untuk Analisis." },
  { "en": "Apa Itu Kebocoran Spektral?", "id": "Efek Jendela Waktu Terbatas." },
  { "en": "Apa Itu Windowing?", "id": "Fungsi Untuk Mengurangi Kebocoran." },
  { "en": "Apa Itu Jendela Hanning?", "id": "Jenis Jendela Umum." },
  { "en": "Apa Itu Jendela Persegi?", "id": "Tidak Menggunakan Jendela Sama." },
  { "en": "Apa Itu Resolusi Frekuensi?", "id": "Kemampuan Membedakan Frekuensi." },
  { "en": "Apa Itu Deteksi?", "id": "Mengidentifikasi Kehadiran Peristiwa." },
  { "en": "Apa Itu Klasifikasi?", "id": "Mengidentifikasi Jenis Peristiwa." },
  { "en": "Apa Itu Lokalisasi?", "id": "Menemukan Sumber Peristiwa." },
  { "en": "Apa Itu Sistem Pakar?", "id": "Sistem Berbasis Aturan." },
  { "en": "Apa Itu Wavelet Transform?", "id": "Analisis Waktu-Frekuensi." },
  { "en": "Apa Keuntungan Wavelet?", "id": "Baik Untuk Menganalisis Transien." },
  { "en": "Apa Itu Tegangan Common-Mode?", "id": "Tegangan Antara Konduktor." },
  { "en": "Apa Itu Tegangan Differential-Mode?", "id": "Tegangan Antara Dua Konduktor." },
  { "en": "Apa Itu Pemodelan Komponen?", "id": "Membuat Model Matematis." },
  { "en": "Apa Itu SimPowerSystems?", "id": "Perangkat Lunak Simulasi." },
  { "en": "Apa Itu ATP-EMTP?", "id": "Program Simulasi Transien." },
  { "en": "Apa Itu PSCAD/EMTDC?", "id": "Perangkat Lunak Simulasi." },
  { "en": "Apa Itu Validasi Model?", "id": "Membandingkan Hasil Model." },
  { "en": "Apa Itu Verifikasi Model?", "id": "Memeriksa Apakah Model." },
  { "en": "Apa Itu Pengukuran Di Lapangan?", "id": "Pengukuran Di Lokasi." },
  { "en": "Apa Itu Keselamatan Listrik?", "id": "Praktik Aman Saat Bekerja." },
  { "en": "Apa Itu Arc Flash?", "id": "Ledakan Listrik Berbahaya." },
  { "en": "Apa Itu Personal Protective Equipment (PPE)?", "id": "Peralatan Pelindung Diri." },
  { "en": "Apa Itu Lockout-Tagout (LOTO)?", "id": "Prosedur Untuk Mematikan." },
  { "en": "Apa Itu Sistem Tidak Terganggu?", "id": "Sistem Yang Diisolasi." },
  { "en": "Apa Itu Kondisioner Jalur Aktif?", "id": "Nama Lain Untuk Active." },
  { "en": "Apa Itu K-Factor Transformator?", "id": "Kemampuan Menangani Arus Harmonik." },
  { "en": "Apa Itu Derating Transformator?", "id": "Mengurangi Kapasitas Akibat." },
  { "en": "Apa Itu Konverter 12-Pulsa?", "id": "Menghasilkan Lebih Sedikit Harmonik." },
  { "en": "Apa Itu Konverter 18-Pulsa?", "id": "Menghasilkan Harmonik Yang Lebih." },
  { "en": "Apa Itu Transformator Pergeseran Fasa?", "id": "Digunakan Dalam Konverter Multi-pulsa." },
  { "en": "Apa Itu Interfensi Jalur Komunikasi?", "id": "Derau Mempengaruhi Jalur." },
  { "en": "Apa Itu Induksi?", "id": "Kopling Melalui Medan." },
  { "en": "Apa Itu Kenaikan Potensial Tanah?", "id": "Kenaikan Tegangan Tanah." },
  { "en": "Apa Itu Pelindung (Shielding)?", "id": "Menggunakan Lapisan Konduktif." },
  { "en": "Apa Itu Kabel Terpilin?", "id": "Mengurangi Kopling Magnetik." },
  { "en": "Apa Itu Sistem Penyeimbang Beban?", "id": "Mendistribusikan Beban Secara." },
  { "en": "Apa Itu Koreksi Faktor Daya Aktif?", "id": "Nama Lain Untuk Active." },
  { "en": "Apa Itu Konverter Boost PFC?", "id": "Topologi Umum Untuk PFC." },
  { "en": "Apa Itu Mode Konduksi Kritis?", "id": "Mode Operasi Untuk PFC." },
  { "en": "Apa Itu Regulasi Beban?", "id": "Kemampuan Menjaga Tegangan." },
  { "en": "Apa Itu Regulasi Jalur?", "id": "Kemampuan Menjaga Tegangan." },
  { "en": "Apa Itu Respon Transien?", "id": "Respons Terhadap Perubahan." },
  { "en": "Apa Itu Waktu Stabil?", "id": "Waktu Untuk Mencapai." },
  { "en": "Apa Itu Overshoot?", "id": "Output Melebihi Nilai." },
  { "en": "Apa Itu Standar IEC (International Electrotechnical Commission)?", "id": "Organisasi Standar Internasional." },
  { "en": "Apa Itu Standar IEEE (Institute of Electrical and Electronics Engineers)?", "id": "Organisasi Profesional Dan Standar." },
  { "en": "Apa Itu Standar ANSI (American National Standards Institute)?", "id": "Organisasi Standar Amerika Serikat." },
  { "en": "Apa Itu CENELEC?", "id": "Komite Eropa Untuk Standarisasi Elektroteknik." },
  { "en": "Apa Hubungan EN 50160 Dengan CENELEC?", "id": "EN 50160 Adalah Standar CENELEC." },
  { "en": "Apa Itu Gelombang Tegangan?", "id": "Bentuk Gelombang Tegangan Listrik." },
  { "en": "Apa Itu Gelombang Arus?", "id": "Bentuk Gelombang Arus Listrik." },
  { "en": "Apa Itu Distorsi Interharmonik?", "id": "Distorsi Yang Disebabkan Oleh Interharmonik." },
  { "en": "Apa Itu Komponen DC?", "id": "Adanya Tegangan Atau Arus DC." },
  { "en": "Apa Efek Komponen DC?", "id": "Dapat Menyebabkan Saturasi Transformator." },
  { "en": "Apa Itu Pensinyalan Frekuensi Rendah?", "id": "Sinyal Kontrol Yang Dikirim." },
  { "en": "Apa Itu Flickersturbance?", "id": "Gangguan Yang Menyebabkan Flicker." },
  { "en": "Apa Itu Pluviometer?", "id": "Alat Pengukur Curah Hujan." },
  { "en": "Apa Itu Transformator Saturable?", "id": "Transformator Yang Dirancang Untuk Saturasi." },
  { "en": "Apa Itu Regulator Tegangan Induksi?", "id": "Mengatur Tegangan Dengan Mengubah." },
  { "en": "Apa Itu Seri Kapasitor?", "id": "Kapasitor Yang Dipasang Seri." },
  { "en": "Apa Itu Resonansi Sub-Sinkron?", "id": "Osilasi Antara Generator Dan Jaringan." },
  { "en": "Apa Itu Batas Stabilitas?", "id": "Batas Operasi Aman Sistem." },
  { "en": "Apa Itu Redaman?", "id": "Pengurangan Amplitudo Osilasi." },
  { "en": "Apa Itu Osilasi Elektromekanis?", "id": "Osilasi Antara Rotor Generator." },
  { "en": "Apa Itu Model Mesin Sinkron?", "id": "Representasi Matematis Dari Generator." },
  { "en": "Apa Itu Model Beban?", "id": "Representasi Matematis Dari Beban." },
  { "en": "Apa Itu Simulasi Domain Waktu?", "id": "Menyelesaikan Persamaan Sistem." },
  { "en": "Apa Itu Simulasi Domain Frekuensi?", "id": "Menganalisis Sistem Dalam Domain." },
  { "en": "Apa Itu Eigenvalue Analysis?", "id": "Digunakan Untuk Menganalisis Stabilitas." },
  { "en": "Apa Itu Nilai Eigen?", "id": "Menunjukkan Mode Osilasi." },
  { "en": "Apa Itu Vektor Eigen?", "id": "Menunjukkan Bentuk Mode Osilasi." },
  { "en": "Apa Itu Partisipasi Faktor?", "id": "Menunjukkan Elemen Mana Yang." },
  { "en": "Apa Itu Teori Normalisasi?", "id": "Metode Analisis Sistem Non-linier." },
  { "en": "Apa Itu Bifurkasi?", "id": "Perubahan Kualitatif Dalam Perilaku." },
  { "en": "Apa Itu Bifurkasi Hopf?", "id": "Menuju Osilasi Stabil." },
  { "en": "Apa Itu Kekacauan (Chaos)?", "id": "Perilaku Kompleks Dan Tidak." },
  { "en": "Apa Itu Fraktal?", "id": "Pola Geometris Yang Berulang." },
  { "en": "Apa Itu Sistem Tenaga Kompleks?", "id": "Sistem Dengan Banyak Komponen." },
  { "en": "Apa Itu Topologi Jaringan?", "id": "Struktur Koneksi Jaringan." },
  { "en": "Apa Itu Teori Graf?", "id": "Cabang Matematika Untuk Studi." },
  { "en": "Apa Itu Node?", "id": "Simpul Dalam Graf." },
  { "en": "Apa Itu Edge?", "id": "Koneksi Antara Node." },
  { "en": "Apa Itu Jaringan Skala Kecil?", "id": "Jaringan Dengan Distribusi." },
  { "en": "Apa Itu Jaringan Bebas Skala?", "id": "Jaringan Dengan Distribusi." },
  { "en": "Apa Itu Ketahanan Jaringan?", "id": "Kemampuan Jaringan Menahan." },
  { "en": "Apa Itu Serangan Bertarget?", "id": "Serangan Pada Node Paling." },
  { "en": "Apa Itu Kegagalan Acak?", "id": "Kegagalan Acak Pada Node." },
  { "en": "Jaringan Mana Yang Lebih Tahan?", "id": "Jaringan Bebas Skala." },
  { "en": "Apa Itu Sistem Self-Organized Criticality?", "id": "Sistem Kompleks Yang Berevolusi." },
  { "en": "Apa Itu Longsoran (Avalanche)?", "id": "Rangkaian Kegagalan Berantai." },
  { "en": "Apa Itu Kaskade Kegagalan?", "id": "Penyebaran Kegagalan Dalam." },
  { "en": "Apa Itu Pemadaman Listrik Besar?", "id": "Seringkali Hasil Dari Kaskade." },
  { "en": "Apa Itu Sistem Proteksi Area Luas?", "id": "Skema Proteksi Terkoordinasi." },
  { "en": "Apa Itu Pemisahan Pulau Terkendali?", "id": "Memisahkan Jaringan Secara." },
  { "en": "Apa Itu Pencegahan?", "id": "Tindakan Untuk Mencegah Masalah." },
  { "en": "Apa Itu Koreksi?", "id": "Tindakan Untuk Memperbaiki Masalah." },
  { "en": "Apa Itu Keamanan Siber?", "id": "Perlindungan Dari Serangan." },
  { "en": "Apa Itu Kerentanan?", "id": "Kelemahan Dalam Sistem." },
  { "en": "Apa Itu Ancaman?", "id": "Sesuatu Yang Dapat." },
  { "en": "Apa Itu Risiko?", "id": "Potensi Kerugian." },
  { "en": "Apa Itu Penilaian Risiko?", "id": "Proses Mengevaluasi Risiko." },
  { "en": "Apa Itu Manajemen Risiko?", "id": "Mengelola Risiko Ke Tingkat." },
  { "en": "Apa Itu Enkripsi?", "id": "Mengamankan Data." },
  { "en": "Apa Itu Tanda Tangan Digital?", "id": "Memverifikasi Keaslian." },
  { "en": "Apa Itu Infrastruktur Kunci Publik (PKI)?", "id": "Mengelola Kunci Digital." },
  { "en": "Apa Itu Firewall?", "id": "Membatasi Lalu Lintas Jaringan." },
  { "en": "Apa Itu Sistem Deteksi Intrusi (IDS)?", "id": "Mendeteksi Aktivitas Mencurigakan." },
  { "en": "Apa Itu Sistem Pencegahan Intrusi (IPS)?", "id": "Mendeteksi Dan Memblokir." },
  { "en": "Apa Itu Honeypot?", "id": "Umpan Untuk Menarik Penyerang." },
  { "en": "Apa Itu Keamanan Fisik?", "id": "Melindungi Akses Fisik." },
  { "en": "Apa Itu Pengawasan Video?", "id": "Menggunakan Kamera Untuk Memantau." },
  { "en": "Apa Itu Kontrol Akses?", "id": "Membatasi Masuk Ke Area." },
  { "en": "Apa Itu Biometrik?", "id": "Menggunakan Karakteristik Fisik." },
  { "en": "Apa Itu Forensik Digital?", "id": "Menginvestigasi Kejahatan Digital." },
  { "en": "Apa Itu Rantai Pengawasan?", "id": "Mendokumentasikan Penanganan Bukti." },
  { "en": "Apa Itu Pemulihan Data?", "id": "Memulihkan Data Yang Hilang." },
  { "en": "Apa Itu Data Carving?", "id": "Memulihkan File Dari Data." },
  { "en": "Apa Itu Steganografi?", "id": "Menyembunyikan Data Di Dalam." },
  { "en": "Apa Itu Kriptanalisis?", "id": "Ilmu Memecahkan Kode." },
  { "en": "Apa Itu Rekayasa Sosial?", "id": "Memanipulasi Orang Untuk." },
  { "en": "Apa Itu Phishing?", "id": "Rekayasa Sosial Melalui Email." },
  { "en": "Apa Itu Malware?", "id": "Perangkat Lunak Berbahaya." },
  { "en": "Apa Itu Virus?", "id": "Malware Yang Menempel." },
  { "en": "Apa Itu Worm?", "id": "Malware Yang Menyebar." },
  { "en": "Apa Itu Trojan?", "id": "Malware Yang Menyamar." },
  { "en": "Apa Itu Ransomware?", "id": "Malware Yang Mengenkripsi File." },
  { "en": "Apa Itu Spyware?", "id": "Malware Yang Memata-matai." },
  { "en": "Apa Itu Adware?", "id": "Malware Yang Menampilkan Iklan." },
  { "en": "Apa Itu Botnet?", "id": "Jaringan Komputer Zombie." },
  { "en": "Apa Itu Serangan Denial-of-Service (DoS)?", "id": "Membuat Layanan Tidak." },
  { "en": "Apa Itu Serangan Distributed DoS (DDoS)?", "id": "DoS Dari Banyak Sumber." },
  { "en": "Apa Itu Zero-Day Exploit?", "id": "Mengeksploitasi Kerentanan Yang." },
  { "en": "Apa Itu Patch Keamanan?", "id": "Pembaruan Untuk Memperbaiki." },
  { "en": "Apa Itu Manajemen Patch?", "id": "Proses Menerapkan Patch." },
  { "en": "Apa Itu Penilaian Kerentanan?", "id": "Mengidentifikasi Kelemahan." },
  { "en": "Apa Itu Pengujian Penetrasi?", "id": "Mensimulasikan Serangan." },
  { "en": "Apa Itu Peretasan Etis?", "id": "Peretasan Dengan Izin." },
  { "en": "Apa Itu Tim Merah?", "id": "Tim Penyerang." },
  { "en": "Apa Itu Tim Biru?", "id": "Tim Pembela." },
  { "en": "Apa Itu Tim Ungu?", "id": "Menggabungkan Tim Merah." },
  { "en": "Apa Itu Respon Insiden?", "id": "Menangani Pelanggaran Keamanan." },
  { "en": "Apa Itu Rencana Respon Insiden?", "id": "Prosedur Untuk Menangani." },
  { "en": "Apa Itu Pemulihan Bencana?", "id": "Memulihkan Operasi Setelah." },
  { "en": "Apa Itu Rencana Kelangsungan Bisnis?", "id": "Menjaga Fungsi Bisnis." },
  { "en": "Apa Itu Cadangan Panas (Hot Backup)?", "id": "Sistem Cadangan Yang Selalu." },
  { "en": "Apa Itu Cadangan Dingin (Cold Backup)?", "id": "Sistem Cadangan Yang Harus." },
  { "en": "Apa Itu Perjanjian Tingkat Layanan (SLA)?", "id": "Kontrak Antara Penyedia Dan." },
  { "en": "Apa Itu Interupsi Momen (Momentary Interruption)?", "id": "Kehilangan Daya Kurang Dari." },
  { "en": "Apa Itu Indeks Kualitas Daya?", "id": "Metrik Tunggal Untuk Kualitas." },
  { "en": "Apa Itu Kompresi Data?", "id": "Mengurangi Ukuran Data Pengukuran." },
  { "en": "Apa Itu Agregasi Data?", "id": "Menggabungkan Data Dari Waktu." },
  { "en": "Apa Itu Penandaan Peristiwa?", "id": "Menandai Peristiwa Kualitas Daya." },
  { "en": "Apa Itu Korelasi Peristiwa?", "id": "Menghubungkan Peristiwa Yang Terkait." },
  { "en": "Apa Itu Kausalitas?", "id": "Hubungan Sebab Akibat." },
  { "en": "Apa Itu Jaringan Listrik Kapal?", "id": "Sistem Tenaga Listrik Di Kapal." },
  { "en": "Apa Itu Jaringan Listrik Pesawat?", "id": "Sistem Tenaga Listrik Di Pesawat." },
  { "en": "Apa Itu Frekuensi 400 Hz?", "id": "Frekuensi Umum Di Pesawat." },
  { "en": "Apa Itu Kereta Listrik?", "id": "Kereta Yang Digerakkan Listrik." },
  { "en": "Apa Itu Catu Daya Tepi Jalan?", "id": "Menyediakan Daya Untuk Kereta." },
  { "en": "Apa Itu Pantograf?", "id": "Kontak Geser Untuk Mengambil." },
  { "en": "Apa Itu Harmonik Yang Dihasilkan Kereta?", "id": "Dihasilkan Oleh Konverter Traksi." },
  { "en": "Apa Itu Sistem Industri?", "id": "Pabrik Dan Fasilitas Industri." },
  { "en": "Apa Itu Beban Tungku Busur?", "id": "Menyebabkan Flicker Dan Distorsi." },
  { "en": "Apa Itu Beban Las?", "id": "Menyebabkan Sag Dan Fluktuasi." },
  { "en": "Apa Itu Pusat Data?", "id": "Membutuhkan Kualitas Daya Sangat." },
  { "en": "Apa Itu Sistem Catu Daya Ganda?", "id": "Dua Suplai Independen." },
  { "en": "Apa Itu Saklar Transfer Statis (STS)?", "id": "Memindahkan Beban Antar Sumber." },
  { "en": "Apa Itu Waktu Transfer?", "id": "Waktu Untuk Beralih Sumber." },
  { "en": "Apa Itu Peralatan Medis?", "id": "Sangat Sensitif Terhadap Gangguan." },
  { "en": "Apa Itu Standar IEC 60601?", "id": "Standar Keamanan Peralatan Medis." },
  { "en": "Apa Itu Sistem Tenaga Terisolasi?", "id": "Digunakan Di Area Medis." },
  { "en": "Apa Itu Line Isolation Monitor (LIM)?", "id": "Memantau Sistem Terisolasi." },
  { "en": "Apa Itu Sistem Komersial?", "id": "Gedung Perkantoran, Pusat Belanja." },
  { "en": "Apa Itu Beban Pencahayaan?", "id": "Lampu LED Dan Fluoresen." },
  { "en": "Apa Itu Beban HVAC (Heating, Ventilation, and Air Conditioning)?", "id": "Pemanasan, Ventilasi, Pendingin." },
  { "en": "Apa Itu Sistem Perumahan?", "id": "Rumah Tinggal." },
  { "en": "Apa Itu Peralatan Elektronik Konsumen?", "id": "Televisi, Komputer, Audio." },
  { "en": "Apa Itu Penyearah Jembatan?", "id": "Input Dari Banyak Peralatan." },
  { "en": "Apa Itu Faktor Crest Arus?", "id": "Rasio Puncak Arus." },
  { "en": "Apa Itu Arus Netral?", "id": "Arus Di Konduktor Netral." },
  { "en": "Bagaimana Harmonik Mempengaruhi Arus Netral?", "id": "Harmonik Triplen Menjumlah Di Netral." },
  { "en": "Apa Itu Netral Berukuran Lebih?", "id": "Konduktor Netral Lebih Besar." },
  { "en": "Apa Itu Ekonomi Kualitas Daya?", "id": "Analisis Biaya Masalah Kualitas." },
  { "en": "Apa Itu Biaya Peralatan Mitigasi?", "id": "Biaya Untuk Memasang Solusi." },
  { "en": "Apa Itu Biaya Downtime?", "id": "Kerugian Akibat Produksi Berhenti." },
  { "en": "Apa Itu Biaya Kerusakan Peralatan?", "id": "Biaya Untuk Memperbaiki Atau." },
  { "en": "Apa Itu Biaya Energi?", "id": "Biaya Listrik Yang Dikonsumsi." },
  { "en": "Bagaimana Kualitas Daya Mempengaruhi Biaya Energi?", "id": "Harmonik Dan Faktor Daya." },
  { "en": "Apa Itu Penalti Faktor Daya?", "id": "Denda Dari Utilitas." },
  { "en": "Apa Itu Analisis Manfaat-Biaya?", "id": "Membandingkan Biaya Dan Manfaat." },
  { "en": "Apa Itu Return on Investment (ROI)?", "id": "Ukuran Profitabilitas Investasi." },
  { "en": "Apa Itu Payback Period?", "id": "Waktu Untuk Mengembalikan Investasi." },
  { "en": "Apa Itu Model Stokastik?", "id": "Model Yang Melibatkan Keacakan." },
  { "en": "Apa Itu Simulasi Monte Carlo?", "id": "Menggunakan Angka Acak." },
  { "en": "Apa Itu Prediksi Kualitas Daya?", "id": "Memperkirakan Masalah Di Masa." },
  { "en": "Apa Itu Perambatan Sag?", "id": "Bagaimana Sag Menyebar." },
  { "en": "Apa Itu Area Kerentanan?", "id": "Wilayah Yang Terkena Dampak." },
  { "en": "Apa Itu Sistem Otomasi Distribusi?", "id": "Mengotomatiskan Operasi Jaringan." },
  { "en": "Apa Itu Self-Healing Grid?", "id": "Jaringan Yang Dapat Memulihkan." },
  { "en": "Apa Itu Fault Location, Isolation, and Service Restoration (FLISR)?", "id": "Fungsi Self-Healing." },
  { "en": "Apa Itu Recloser Cerdas?", "id": "Recloser Dengan Kemampuan Komunikasi." },
  { "en": "Apa Itu Saklar Cerdas?", "id": "Saklar Dengan Kontrol Jarak." },
  { "en": "Apa Itu Sensor Cerdas?", "id": "Sensor Dengan Kemampuan Pemrosesan." },
  { "en": "Apa Itu Jaringan Sensor?", "id": "Kumpulan Sensor Yang Terhubung." },
  { "en": "Apa Itu Analisis Data?", "id": "Memeriksa Data Untuk Menarik." },
  { "en": "Apa Itu Visualisasi Data?", "id": "Menampilkan Data Secara Grafis." },
  { "en": "Apa Itu Dasbor?", "id": "Tampilan Visual Informasi." },
  { "en": "Apa Itu Sistem Informasi Geografis (GIS)?", "id": "Memetakan Data Secara Geografis." },
  { "en": "Apa Itu Gelombang Berjalan?", "id": "Pulsa Yang Merambat." },
  { "en": "Apa Itu Lokasi Gangguan Gelombang Berjalan?", "id": "Menggunakan Waktu Tiba Gelombang." },
  { "en": "Apa Itu Osilasi Frekuensi Tinggi?", "id": "Transien Osilatoris." },
  { "en": "Apa Itu Resonansi Inti Transformator?", "id": "Osilasi Antara Induktansi." },
  { "en": "Apa Itu Impedansi Karakteristik?", "id": "Impedansi Jalur Transmisi." },
  { "en": "Apa Itu Refleksi?", "id": "Gelombang Memantul Kembali." },
  { "en": "Apa Itu Transmisi?", "id": "Gelombang Melewati Batas." },
  { "en": "Apa Itu Koefisien Refleksi?", "id": "Rasio Gelombang Pantul." },
  { "en": "Apa Itu Koefisien Transmisi?", "id": "Rasio Gelombang Transmisi." },
  { "en": "Apa Itu Diagram Tangga (Lattice Diagram)?", "id": "Menganalisis Refleksi Gelombang." },
  { "en": "Apa Itu Petir?", "id": "Sumber Transien Alami." },
  { "en": "Apa Itu Sambaran Langsung?", "id": "Petir Menyambar Langsung." },
  { "en": "Apa Itu Sambaran Tidak Langsung?", "id": "Petir Menyambar Dekat." },
  { "en": "Apa Itu Gelombang Petir?", "id": "Bentuk Gelombang Transien." },
  { "en": "Apa Itu Waktu Muka (Front Time)?", "id": "Waktu Naik Gelombang." },
  { "en": "Apa Itu Waktu Ekor (Tail Time)?", "id": "Waktu Turun Gelombang." },
  { "en": "Apa Itu Proteksi Petir?", "id": "Melindungi Sistem Dari Petir." },
  { "en": "Apa Itu Kawat Tanah (Shield Wire)?", "id": "Menangkap Sambaran Petir." },
  { "en": "Apa Itu Arrester Surja?", "id": "Nama Lain Surge Arrester." },
  { "en": "Apa Itu Tegangan Sisa?", "id": "Tegangan Di Arrester." },
  { "en": "Apa Itu Energi Surja?", "id": "Energi Yang Diserap Arrester." },
  { "en": "Apa Itu Switching Elektromagnetik?", "id": "Transien Akibat Operasi." },
  { "en": "Apa Itu Restrike?", "id": "Busur Api Muncul Kembali." },
  { "en": "Apa Itu Current Chopping?", "id": "Pemutusan Arus Secara." },
  { "en": "Apa Itu Tegangan Pemulihan Transien (TRV)?", "id": "Tegangan Di Kontak." },
  { "en": "Apa Itu Capacitor Switching?", "id": "Menghasilkan Transien Osilatoris." },
  { "en": "Apa Itu Magnifikasi Tegangan?", "id": "Peningkatan Tegangan Akibat." },
  { "en": "Apa Itu Inrush Arus?", "id": "Nama Lain Inrush Current." },
  { "en": "Apa Itu Ferroresonansi?", "id": "Osilasi Non-linier." },
  { "en": "Apa Itu Mode Fundamental?", "id": "Mode Osilasi Dasar." },
  { "en": "Apa Itu Mode Subharmonik?", "id": "Osilasi Di Bawah Frekuensi." },
  { "en": "Apa Itu Mode Kuasi-Periodik?", "id": "Osilasi Dengan Beberapa." },
  { "en": "Apa Itu Mode Kacau (Chaotic)?", "id": "Osilasi Tidak Teratur." },
  { "en": "Apa Itu Inisiasi Ferroresonansi?", "id": "Peristiwa Yang Memulai." },
  { "en": "Apa Itu Damping Ferroresonansi?", "id": "Mengurangi Osilasi Ferroresonansi." },
  { "en": "Bagaimana Cara Meredam Ferroresonansi?", "id": "Menambahkan Beban Resistif." },
  { "en": "Apa Itu Model Kualitas Daya?", "id": "Representasi Matematis Dari Gangguan." },
  { "en": "Apa Itu Model Stokastik?", "id": "Model Yang Melibatkan Probabilitas." },
  { "en": "Apa Itu Model Deterministik?", "id": "Model Tanpa Elemen Acak." },
  { "en": "Apa Itu Estimasi Parameter?", "id": "Menentukan Parameter Model." },
  { "en": "Apa Itu Metode Momen?", "id": "Metode Statistik Untuk Estimasi." },
  { "en": "Apa Itu Estimasi Kemungkinan Maksimum?", "id": "Metode Statistik Lainnya." },
  { "en": "Apa Itu Filter Kalman?", "id": "Estimator Optimal Untuk Sistem." },
  { "en": "Apa Itu Jaringan Saraf Tiruan?", "id": "Digunakan Untuk Klasifikasi." },
  { "en": "Apa Itu Pohon Keputusan?", "id": "Model Klasifikasi Berbentuk Pohon." },
  { "en": "Apa Itu Support Vector Machine (SVM)?", "id": "Model Klasifikasi Lainnya." },
  { "en": "Apa Itu Komputasi Evolusioner?", "id": "Algoritma Optimisasi." },
  { "en": "Apa Itu Logika Fuzzy?", "id": "Menangani Ketidakpastian." },
  { "en": "Apa Itu Indeks Kualitas Daya?", "id": "Angka Tunggal Untuk Kualitas." },
  { "en": "Apa Itu Agregasi?", "id": "Menggabungkan Beberapa Ukuran." },
  { "en": "Apa Itu Pembobotan?", "id": "Memberi Bobot Pada Ukuran." },
  { "en": "Apa Itu Normalisasi?", "id": "Menskalakan Data Ke Rentang." },
  { "en": "Apa Itu Kualitas Suplai?", "id": "Kualitas Daya Dari Sisi." },
  { "en": "Apa Itu Kualitas Permintaan?", "id": "Pengaruh Beban Terhadap Kualitas." },
  { "en": "Apa Itu Interaksi Beban-Sistem?", "id": "Bagaimana Beban Dan Sistem." },
  { "en": "Apa Itu Kekakuan Jaringan?", "id": "Kemampuan Jaringan Menahan." },
  { "en": "Apa Itu Arus Hubung Singkat?", "id": "Ukuran Kekakuan Jaringan." },
  { "en": "Apa Itu Sistem Tenaga Radial?", "id": "Aliran Daya Satu Arah." },
  { "en": "Apa Itu Sistem Tenaga Jala?", "id": "Aliran Daya Multi-arah." },
  { "en": "Apa Itu Pembangkit Terdistribusi?", "id": "Pembangkit Listrik Skala Kecil." },
  { "en": "Apa Itu Efek Pulau?", "id": "Pembangkit Terdistribusi Tetap." },
  { "en": "Apa Itu Deteksi Anti-Islanding?", "id": "Metode Untuk Mendeteksi Efek." },
  { "en": "Apa Itu Metode Pasif?", "id": "Memantau Parameter Jaringan." },
  { "en": "Apa Itu Metode Aktif?", "id": "Menyuntikkan Sinyal Kecil." },
  { "en": "Apa Itu Pergeseran Frekuensi?", "id": "Metode Deteksi Aktif." },
  { "en": "Apa Itu Pergeseran Impedansi?", "id": "Metode Deteksi Aktif Lainnya." },
  { "en": "Apa Itu Inverter Siap Jaringan?", "id": "Inverter Dengan Fitur Cerdas." },
  { "en": "Apa Itu Standar IEEE 1547?", "id": "Standar Untuk Menghubungkan." },
  { "en": "Apa Itu Ride-Through?", "id": "Kemampuan Tetap Terhubung." },
  { "en": "Apa Itu Kontrol Volt-VAR?", "id": "Mengatur Tegangan Dengan Daya." },
  { "en": "Apa Itu Kontrol Frekuensi-Watt?", "id": "Mengatur Daya Untuk Menstabilkan." },
  { "en": "Apa Itu Soft Start?", "id": "Fitur Inverter Untuk Mengurangi." },
  { "en": "Apa Itu Kendaraan Listrik?", "id": "Beban Dan Sumber Potensial." },
  { "en": "Apa Itu Pengisian Cerdas?", "id": "Mengontrol Pengisian Kendaraan." },
  { "en": "Apa Itu Vehicle-to-Grid (V2G)?", "id": "Kendaraan Memberi Daya." },
  { "en": "Apa Itu Baterai?", "id": "Penyimpanan Energi Kimia." },
  { "en": "Apa Itu Sistem Manajemen Baterai (BMS)?", "id": "Mengelola Dan Melindungi." },
  { "en": "Apa Itu State of Charge (SoC)?", "id": "Tingkat Pengisian Baterai." },
  { "en": "Apa Itu State of Health (SoH)?", "id": "Kondisi Kesehatan Baterai." },
  { "en": "Apa Itu Siklus Hidup?", "id": "Jumlah Siklus Pengisian." },
  { "en": "Apa Itu Penyimpanan Energi Skala Grid?", "id": "Menyimpan Energi Dalam Jumlah." },
  { "en": "Apa Itu Arbitrase Energi?", "id": "Membeli Murah, Menjual Mahal." },
  { "en": "Apa Itu Regulasi Frekuensi?", "id": "Membantu Menstabilkan Frekuensi." },
  { "en": "Apa Itu Cadangan Berputar?", "id": "Kapasitas Cadangan Online." },
  { "en": "Apa Itu Black Start?", "id": "Memulai Kembali Jaringan." },
  { "en": "Apa Itu Pencahayaan LED (Light-Emitting Diode)?", "id": "Sumber Harmonik." },
  { "en": "Apa Itu Driver LED?", "id": "Catu Daya Untuk LED." },
  { "en": "Apa Itu Inrush Current LED?", "id": "Arus Puncak Saat Menyalakan." },
  { "en": "Apa Itu Peredupan (Dimming)?", "id": "Mengatur Kecerahan Cahaya." },
  { "en": "Bagaimana Peredupan Mempengaruhi Harmonik?", "id": "Dapat Meningkatkan Distorsi." },
  { "en": "Apa Itu Flicker Lampu LED?", "id": "Fluktuasi Kecerahan." },
  { "en": "Apa Itu Sistem HVAC (Heating, Ventilation, and Air Conditioning)?", "id": "Beban Besar Di Gedung." },
  { "en": "Apa Itu Kompresor Kecepatan Variabel?", "id": "Menggunakan VFD (Variable Frequency Drive)." },
  { "en": "Apa Itu Pusat Data?", "id": "Beban Kritis Dengan Banyak." },
  { "en": "Apa Itu Redundansi Catu Daya?", "id": "Memiliki Catu Daya Cadangan." },
  { "en": "Apa Itu Power Distribution Unit (PDU)?", "id": "Mendistribusikan Daya Di Rak." },
  { "en": "Apa Itu Static Transfer Switch (STS)?", "id": "Beralih Antar Sumber." },
  { "en": "Apa Itu Saklar Transfer Otomatis (ATS)?", "id": "Saklar Mekanis." },
  { "en": "Apa Itu Generator Cadangan?", "id": "Menyediakan Daya Selama." },
  { "en": "Apa Itu Waktu Peralihan?", "id": "Waktu Untuk Beralih." },
  { "en": "Apa Itu Pengukuran Kualitas Daya?", "id": "Proses Mengukur Parameter." },
  { "en": "Apa Itu Akurasi Instrumen?", "id": "Seberapa Dekat Pengukuran." },
  { "en": "Apa Itu Presisi Instrumen?", "id": "Seberapa Konsisten Pengukuran." },
  { "en": "Apa Itu Resolusi?", "id": "Perubahan Terkecil Yang Dapat." },
  { "en": "Apa Itu Bandwidth?", "id": "Rentang Frekuensi Yang Dapat." },
  { "en": "Apa Itu Laju Sampling?", "id": "Seberapa Sering Sinyal." },
  { "en": "Apa Itu Kuantisasi?", "id": "Mengubah Nilai Analog." },
  { "en": "Apa Itu Bit Resolusi?", "id": "Jumlah Bit Yang Digunakan." },
  { "en": "Apa Itu Standar IEC 61000-4-30?", "id": "Standar Untuk Metode Pengukuran." },
  { "en": "Apa Itu Kelas A?", "id": "Kelas Akurasi Tertinggi." },
  { "en": "Apa Itu Kelas S?", "id": "Kelas Untuk Survei." },
  { "en": "Apa Itu Agregasi Waktu?", "id": "Menggabungkan Data Selama Interval." },
  { "en": "Apa Itu Interval 10-Menit?", "id": "Interval Umum Untuk Agregasi." },
  { "en": "Apa Itu Penandaan (Flagging)?", "id": "Menandai Data Selama Peristiwa." },
  { "en": "Apa Itu Sinkronisasi Waktu?", "id": "Memastikan Semua Instrumen." },
  { "en": "Apa Itu Global Positioning System (GPS)?", "id": "Digunakan Untuk Sinkronisasi." },
  { "en": "Apa Itu Probabilitas?", "id": "Ukuran Kemungkinan." },
  { "en": "Apa Itu Statistik?", "id": "Ilmu Mengumpulkan Dan Menganalisis." },
  { "en": "Apa Itu Rata-rata?", "id": "Ukuran Tendensi Sentral." },
  { "en": "Apa Itu Standar Deviasi?", "id": "Ukuran Sebaran." },
  { "en": "Apa Itu Distribusi Probabilitas?", "id": "Fungsi Yang Menjelaskan." },
  { "en": "Apa Itu Distribusi Normal?", "id": "Distribusi Berbentuk Lonceng." },
  { "en": "Apa Itu Distribusi Weibull?", "id": "Digunakan Dalam Analisis Keandalan." },
  { "en": "Apa Itu Indeks Statistik?", "id": "Seperti 95-persentil." },
  { "en": "Apa Itu Analisis Tren?", "id": "Mencari Pola Seiring Waktu." },
  { "en": "Apa Itu Analisis Korelasi?", "id": "Mencari Hubungan Antar Variabel." },
  { "en": "Apa Itu Pelaporan Kualitas Daya?", "id": "Menyajikan Hasil Analisis." },
  { "en": "Apa Itu Laporan Eksekutif?", "id": "Ringkasan Tingkat Tinggi." },
  { "en": "Apa Itu Laporan Teknis?", "id": "Laporan Rinci Untuk Insinyur." },
  { "en": "Apa Itu Visualisasi?", "id": "Menampilkan Data Secara Grafis." },
  { "en": "Apa Itu Kode Warna?", "id": "Menggunakan Warna Untuk Menunjukkan." },
  { "en": "Apa Itu Peta Kontur?", "id": "Visualisasi Data Geografis." },
  { "en": "Apa Itu Kompatibilitas Peralatan?", "id": "Kemampuan Peralatan Beroperasi." },
  { "en": "Apa Itu Toleransi Peralatan?", "id": "Tingkat Gangguan Yang Dapat." },
  { "en": "Apa Itu Kurva Toleransi?", "id": "Grafik Magnitudo Terhadap Durasi." },
  { "en": "Apa Itu Kurva CBEMA (Computer and Business Equipment Manufacturers Association)?", "id": "Kurva Toleransi Awal." },
  { "en": "Apa Itu Kurva ITIC (Information Technology Industry Council)?", "id": "Kurva Toleransi Modern." },
  { "en": "Apa Itu Kurva SEMI F47?", "id": "Standar Untuk Industri Semikonduktor." },
  { "en": "Apa Itu Desain Untuk Kekebalan?", "id": "Merancang Peralatan Agar Tahan." },
  { "en": "Apa Itu Catu Daya Tahan Sag?", "id": "Catu Daya Yang Dapat." },
  { "en": "Apa Itu Energi Tersimpan?", "id": "Energi Di Kapasitor Internal." },
  { "en": "Apa Itu Kontrol Umpan Maju?", "id": "Mengantisipasi Gangguan." },
  { "en": "Apa Itu Sistem Kontrol Digital?", "id": "Menggunakan Prosesor Digital." },
  { "en": "Apa Itu Digital Signal Processor (DSP)?", "id": "Prosesor Khusus Untuk Sinyal." },
  { "en": "Apa Itu Field-Programmable Gate Array (FPGA)?", "id": "Sirkuit Terpadu Yang Dapat." },
  { "en": "Apa Itu Kontrol Real-Time?", "id": "Sistem Kontrol Dengan Batas." },
  { "en": "Apa Itu Perangkat Lunak Tertanam?", "id": "Perangkat Lunak Di Dalam." },
  { "en": "Apa Itu Simulasi Real-Time?", "id": "Simulasi Yang Berjalan." },
  { "en": "Apa Itu Hardware-in-the-Loop (HIL)?", "id": "Simulasi Yang Melibatkan." },
  { "en": "Apa Itu Topologi Jaringan Listrik?", "id": "Struktur Fisik Jaringan." },
  { "en": "Apa Itu Jaringan Transmisi?", "id": "Menyalurkan Daya Jarak Jauh." },
  { "en": "Apa Itu Jaringan Distribusi?", "id": "Menyalurkan Daya Ke Pelanggan." },
  { "en": "Apa Itu Jaringan Tegangan Rendah?", "id": "Jaringan Distribusi Akhir." },
  { "en": "Apa Itu Jaringan Tegangan Menengah?", "id": "Antara Transmisi Dan Distribusi." },
  { "en": "Apa Itu Jaringan Tegangan Tinggi?", "id": "Jaringan Transmisi." },
  { "en": "Apa Itu Jaringan Tegangan Ekstra Tinggi?", "id": "Jaringan Transmisi Jarak Jauh." },
  { "en": "Apa Itu Jaringan Tegangan Ultra Tinggi?", "id": "Jaringan Transmisi Paling Tinggi." },
  { "en": "Apa Itu Arus Searah Tegangan Tinggi (HVDC)?", "id": "Transmisi Daya Menggunakan DC." },
  { "en": "Apa Keuntungan HVDC (High-Voltage Direct Current)?", "id": "Kerugian Rendah, Kontrol Aliran." },
  { "en": "Apa Itu Stasiun Konverter?", "id": "Mengubah Antara AC Dan DC." },
  { "en": "Apa Itu Line-Commutated Converter (LCC)?", "id": "Konverter HVDC Klasik." },
  { "en": "Apa Itu Voltage-Sourced Converter (VSC)?", "id": "Konverter HVDC Modern." },
  { "en": "Apa Itu Jaringan HVDC Multi-Terminal?", "id": "Menghubungkan Lebih Dari Dua." },
  { "en": "Apa Itu Flexible AC Transmission System (FACTS)?", "id": "Sistem Berbasis Elektronika." },
  { "en": "Apa Itu Kompensasi Seri?", "id": "Memasang Kapasitor Seri." },
  { "en": "Apa Itu Kompensasi Shunt?", "id": "Memasang Komponen Paralel." },
  { "en": "Apa Itu Thyristor-Controlled Series Capacitor (TCSC)?", "id": "Kapasitor Seri Terkendali." },
  { "en": "Apa Itu Static Synchronous Series Compensator (SSSC)?", "id": "Kompensator Seri Berbasis VSC." },
  { "en": "Apa Itu Interline Power Flow Controller (IPFC)?", "id": "Mengontrol Aliran Daya." },
  { "en": "Apa Itu Harmonik?", "id": "Distorsi Bentuk Gelombang." },
  { "en": "Apa Itu Analisis Harmonik?", "id": "Mempelajari Penyebaran Harmonik." },
  { "en": "Apa Itu Spektrum Harmonik?", "id": "Plot Amplitudo Harmonik." },
  { "en": "Apa Itu Orde Harmonik?", "id": "Kelipatan Frekuensi Fundamental." },
  { "en": "Apa Itu Fasa Harmonik?", "id": "Sudut Fasa Komponen Harmonik." },
  { "en": "Apa Itu Daya Harmonik?", "id": "Daya Akibat Interaksi." },
  { "en": "Apa Itu Transformator Zig-Zag?", "id": "Transformator Khusus Untuk Menekan." },
  { "en": "Apa Itu Grounding?", "id": "Koneksi Ke Bumi." },
  { "en": "Apa Itu Sistem Pembumian Solid?", "id": "Netral Terhubung Langsung." },
  { "en": "Apa Itu Sistem Pembumian Resistansi?", "id": "Netral Terhubung Melalui." },
  { "en": "Apa Itu Sistem Pembumian Reaktansi?", "id": "Netral Terhubung Melalui." },
  { "en": "Apa Itu Sistem Tidak Dibumikan?", "id": "Netral Tidak Terhubung." },
  { "en": "Apa Itu Pembumian Frekuensi Tinggi?", "id": "Penting Untuk Proteksi EMI." },
  { "en": "Apa Itu Kualitas Daya Di Industri?", "id": "Tantangan Utama Akibat Beban." },
  { "en": "Apa Itu Adjustable Speed Drive (ASD)?", "id": "Nama Lain Untuk VFD." },
  { "en": "Apa Itu Pemanasan Induksi?", "id": "Beban Non-linier." },
  { "en": "Apa Itu Pengelasan Busur?", "id": "Beban Fluktuatif." },
  { "en": "Apa Itu Sistem Otomasi?", "id": "Sistem Kontrol Otomatis." },
  { "en": "Apa Itu Programmable Logic Controller (PLC)?", "id": "Komputer Digital Untuk Industri." },
  { "en": "Bagaimana PLC Dipengaruhi Kualitas Daya?", "id": "Sag Dapat Menyebabkan Reset." },
  { "en": "Apa Itu Robot Industri?", "id": "Beban Dinamis." },
  { "en": "Apa Itu Kualitas Daya Di Gedung Komersial?", "id": "Didominasi Oleh Beban Elektronik." },
  { "en": "Apa Itu Pencahayaan Fluoresen?", "id": "Sumber Harmonik." },
  { "en": "Apa Itu Pencahayaan LED?", "id": "Sumber Harmonik Lainnya." },
  { "en": "Apa Itu Peralatan Kantor?", "id": "Komputer, Printer, Fotokopi." },
  { "en": "Apa Itu Sistem HVAC?", "id": "Beban Motor Besar." },
  { "en": "Apa Itu Lift?", "id": "Beban Dinamis." },
  { "en": "Apa Itu Kualitas Daya Di Rumah Sakit?", "id": "Kualitas Daya Sangat Kritis." },
  { "en": "Apa Itu Sistem Tenaga Esensial?", "id": "Sistem Cadangan Untuk Beban." },
  { "en": "Apa Itu Peralatan Pencitraan Medis?", "id": "Sensitif Terhadap Variasi." },
  { "en": "Apa Itu Peralatan Pendukung Kehidupan?", "id": "Membutuhkan Suplai Daya." },
  { "en": "Apa Itu Kualitas Daya Di Pertanian?", "id": "Tantangan Akibat Jaringan." },
  { "en": "Apa Itu Beban Irigasi?", "id": "Beban Motor Besar." },
  { "en": "Apa Itu Stray Voltage?", "id": "Tegangan Kecil Yang Dapat." },
  { "en": "Apa Itu Pembangkit Listrik?", "id": "Sumber Energi Listrik." },
  { "en": "Apa Itu Sistem Eksitasi?", "id": "Menyediakan Medan Magnet." },
  { "en": "Apa Itu Automatic Voltage Regulator (AVR)?", "id": "Mengatur Tegangan Output." },
  { "en": "Apa Itu Osilasi Torsional?", "id": "Osilasi Mekanis Pada Poros." },
  { "en": "Apa Itu Interaksi Sub-Sinkron?", "id": "Osilasi Antara Jaringan." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Air?", "id": "Menggunakan Energi Air." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Termal?", "id": "Menggunakan Panas." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Nuklir?", "id": "Menggunakan Energi Nuklir." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Gas?", "id": "Menggunakan Turbin Gas." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Diesel?", "id": "Menggunakan Mesin Diesel." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Surya Fotovoltaik?", "id": "Menggunakan Panel Surya." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Angin?", "id": "Menggunakan Turbin Angin." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Geotermal?", "id": "Menggunakan Panas Bumi." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Biomassa?", "id": "Membakar Bahan Organik." },
  { "en": "Apa Itu Kualitas Daya Dan Keandalan?", "id": "Dua Konsep Yang Terkait." },
  { "en": "Apakah Interupsi Masalah Kualitas Daya?", "id": "Dianggap Sebagai Masalah." },
  { "en": "Apa Itu Penilaian Risiko?", "id": "Mengevaluasi Risiko Terkait." },
  { "en": "Apa Itu Probabilitas Kegagalan?", "id": "Kemungkinan Suatu Komponen." },
  { "en": "Apa Itu Analisis Dampak?", "id": "Mengevaluasi Konsekuensi." },
  { "en": "Apa Itu Pohon Kejadian?", "id": "Diagram Untuk Menganalisis." },
  { "en": "Apa Itu Pohon Kesalahan?", "id": "Diagram Untuk Menganalisis." },
  { "en": "Apa Itu Pemeliharaan Berbasis Kondisi?", "id": "Pemeliharaan Berdasarkan Kondisi." },
  { "en": "Apa Itu Pemantauan Kondisi?", "id": "Memantau Parameter Peralatan." },
  { "en": "Apa Itu Termografi?", "id": "Menggunakan Kamera Inframerah." },
  { "en": "Apa Itu Analisis Getaran?", "id": "Menganalisis Getaran Mesin." },
  { "en": "Apa Itu Analisis Oli?", "id": "Menganalisis Oli Pelumas." },
  { "en": "Apa Itu Pengujian Pelepasan Sebagian?", "id": "Mendeteksi Pelepasan Sebagian." },
  { "en": "Apa Itu Regulasi Kualitas Daya?", "id": "Aturan Yang Ditetapkan." },
  { "en": "Apa Itu Komisi Regulasi Energi?", "id": "Badan Pemerintah Yang Mengatur." },
  { "en": "Apa Itu Tarif Listrik?", "id": "Harga Yang Dibayar Konsumen." },
  { "en": "Apa Itu Tarif Berdasarkan Waktu?", "id": "Harga Bervariasi Sepanjang Hari." },
  { "en": "Apa Itu Harga Real-Time?", "id": "Harga Berubah Terus-menerus." },
  { "en": "Apa Itu Respon Permintaan?", "id": "Konsumen Mengubah Penggunaan." },
  { "en": "Apa Itu Pengurangan Beban?", "id": "Mengurangi Konsumsi Selama Puncak." },
  { "en": "Apa Itu Pembangkit Cadangan?", "id": "Generator Untuk Daya Darurat." },
  { "en": "Apa Itu Saklar Transfer Otomatis (ATS)?", "id": "Beralih Antara Sumber Daya." },
  { "en": "Apa Itu Operasi Pulau?", "id": "Sistem Berjalan Terisolasi." },
  { "en": "Apa Itu Resinkronisasi?", "id": "Menghubungkan Kembali Ke Jaringan." },
  { "en": "Apa Itu Analisis Gelombang Harmonik?", "id": "Menganalisis Komponen Harmonik." },
  { "en": "Apa Itu Transformasi Fourier Cepat?", "id": "Algoritma Untuk Analisis Spektrum." },
  { "en": "Apa Itu Jendela Waktu?", "id": "Durasi Sinyal Yang Dianalisis." },
  { "en": "Apa Itu Kebocoran Spektral?", "id": "Penyebaran Energi Dalam Spektrum." },
  { "en": "Apa Itu Fungsi Jendela?", "id": "Mengurangi Kebocoran Spektral." },
  { "en": "Apa Itu Picket-Fence Effect?", "id": "Kesalahan Akibat Resolusi Frekuensi." },
  { "en": "Apa Itu Transformasi Wavelet?", "id": "Analisis Waktu-Frekuensi." },
  { "en": "Apa Keuntungan Transformasi Wavelet?", "id": "Baik Untuk Menganalisis Transien." },
  { "en": "Apa Itu Estimasi Frekuensi?", "id": "Menentukan Frekuensi Sinyal." },
  { "en": "Apa Itu Deteksi Zero-Crossing?", "id": "Metode Sederhana Untuk Frekuensi." },
  { "en": "Apa Itu Phase-Locked Loop (PLL)?", "id": "Sirkuit Untuk Melacak Frekuensi." },
  { "en": "Apa Itu Estimasi Fasor?", "id": "Menentukan Magnitudo Dan Fasa." },
  { "en": "Apa Itu Standar IEEE C37.118?", "id": "Standar Untuk Sinkrofasor." },
  { "en": "Apa Itu Pengukuran RMS?", "id": "Menghitung Nilai RMS Sinyal." },
  { "en": "Apa Itu Sliding Window?", "id": "Teknik Untuk Perhitungan RMS." },
  { "en": "Apa Itu Deteksi Peristiwa?", "id": "Mengidentifikasi Gangguan Kualitas Daya." },
  { "en": "Apa Itu Pemicu (Trigger)?", "id": "Kondisi Untuk Memulai Perekaman." },
  { "en": "Apa Itu Perekaman Bentuk Gelombang?", "id": "Menangkap Bentuk Gelombang." },
  { "en": "Apa Itu Kompresi Data?", "id": "Mengurangi Ukuran Data." },
  { "en": "Apa Itu Representasi Fitur?", "id": "Mengekstrak Fitur Penting." },
  { "en": "Apa Itu Tegangan Seimbang?", "id": "Tegangan Tiga Fasa Sama." },
  { "en": "Apa Itu Arus Seimbang?", "id": "Arus Tiga Fasa Sama." },
  { "en": "Apa Itu Beban Seimbang?", "id": "Impedansi Sama Di Setiap." },
  { "en": "Apa Itu Faktor Ketidakseimbangan?", "id": "Ukuran Ketidakseimbangan." },
  { "en": "Apa Itu Komponen Urutan Negatif?", "id": "Menyebabkan Pemanasan Motor." },
  { "en": "Apa Itu Komponen Urutan Nol?", "id": "Menyebabkan Arus Di Netral." },
  { "en": "Apa Itu Sistem Tenaga Empat Kawat?", "id": "Tiga Fasa Dengan Netral." },
  { "en": "Apa Itu Sistem Tenaga Tiga Kawat?", "id": "Tiga Fasa Tanpa Netral." },
  { "en": "Apa Itu Pembumian Netral?", "id": "Menghubungkan Titik Netral." },
  { "en": "Apa Itu Gangguan Tanah?", "id": "Hubungan Antara Fasa." },
  { "en": "Apa Itu Tegangan Lebih Transien?", "id": "Lonjakan Tegangan Jangka." },
  { "en": "Apa Itu Waktu Naik?", "id": "Waktu Pulsa Naik." },
  { "en": "Apa Itu Durasi Pulsa?", "id": "Lebar Pulsa Transien." },
  { "en": "Apa Itu Energi Pulsa?", "id": "Energi Dalam Pulsa." },
  { "en": "Apa Itu Proteksi Tegangan Lebih?", "id": "Melindungi Dari Tegangan." },
  { "en": "Apa Itu Tingkat Proteksi Tegangan?", "id": "Tegangan Sisa Di Perangkat." },
  { "en": "Apa Itu Koordinasi Isolasi?", "id": "Mengoordinasikan Kekuatan Isolasi." },
  { "en": "Apa Itu Jarak Bebas (Clearance)?", "id": "Jarak Terpendek Di Udara." },
  { "en": "Apa Itu Jarak Rambat (Creepage)?", "id": "Jarak Terpendek Di Permukaan." },
  { "en": "Apa Itu Ketinggian?", "id": "Ketinggian Mempengaruhi Kekuatan." },
  { "en": "Apa Itu Polusi?", "id": "Kontaminasi Mempengaruhi Isolasi." },
  { "en": "Apa Itu Busur Api (Arcing)?", "id": "Pelepasan Listrik Di Udara." },
  { "en": "Apa Itu Model Kualitas Daya?", "id": "Representasi Matematis." },
  { "en": "Apa Itu Model Sag Stokastik?", "id": "Memprediksi Kejadian Sag." },
  { "en": "Apa Itu Propagasi Sag?", "id": "Bagaimana Sag Menyebar." },
  { "en": "Apa Itu Area Kerentanan?", "id": "Area Yang Terkena Sag." },
  { "en": "Apa Itu Peta Kerentanan?", "id": "Visualisasi Area Kerentanan." },
  { "en": "Apa Itu Toleransi Beban?", "id": "Kemampuan Beban Menahan." },
  { "en": "Apa Itu Karakterisasi Beban?", "id": "Menentukan Toleransi Beban." },
  { "en": "Apa Itu Pengujian Beban?", "id": "Menerapkan Gangguan Ke Beban." },
  { "en": "Apa Itu Generator Sag?", "id": "Peralatan Untuk Menghasilkan Sag." },
  { "en": "Apa Itu Solusi Mitigasi?", "id": "Teknologi Untuk Mengurangi." },
  { "en": "Apa Itu Analisis Ekonomi?", "id": "Mengevaluasi Kelayakan Solusi." },
  { "en": "Apa Itu Biaya Siklus Hidup?", "id": "Total Biaya Selama Umur." },
  { "en": "Apa Itu Nilai Sekarang Bersih (NPV)?", "id": "Ukuran Profitabilitas." },
  { "en": "Apa Itu Tingkat Pengembalian Internal (IRR)?", "id": "Tingkat Diskonto Yang Membuat." },
  { "en": "Apa Itu Analisis Sensitivitas?", "id": "Mempelajari Bagaimana Hasil." },
  { "en": "Apa Itu Pohon Keputusan?", "id": "Alat Untuk Pengambilan Keputusan." },
  { "en": "Apa Itu Penilaian Risiko?", "id": "Mengevaluasi Risiko Terkait." },
  { "en": "Apa Itu Jaringan Bayesian?", "id": "Model Grafis Probabilistik." },
  { "en": "Apa Itu Kecerdasan Buatan?", "id": "Kecerdasan Yang Ditunjukkan." },
  { "en": "Apa Itu Sistem Berbasis Pengetahuan?", "id": "Sistem Yang Menggunakan." },
  { "en": "Apa Itu Rekonfigurasi Jaringan?", "id": "Mengubah Topologi Jaringan." },
  { "en": "Apa Itu Penempatan Kapasitor Optimal?", "id": "Menentukan Lokasi Terbaik." },
  { "en": "Apa Itu Penempatan Filter Optimal?", "id": "Menentukan Lokasi Terbaik." },
  { "en": "Apa Itu Alokasi Harmonik?", "id": "Membagi Kapasitas Distorsi." },
  { "en": "Apa Itu Tanggung Jawab Harmonik?", "id": "Menentukan Sumber Distorsi." },
  { "en": "Apa Itu Pengukuran Sinkron?", "id": "Pengukuran Yang Disinkronkan." },
  { "en": "Apa Itu Pemantauan Kualitas Daya Terdistribusi?", "id": "Menggunakan Banyak Monitor." },
  { "en": "Apa Itu Arus Netral Berlebih?", "id": "Arus Tinggi Di Konduktor." },
  { "en": "Apa Penyebab Arus Netral Berlebih?", "id": "Beban Non-linier Fasa-Tunggal." },
  { "en": "Apa Itu Transformator Penurun Netral?", "id": "Mengurangi Arus Netral." },
  { "en": "Apa Itu Filter Urutan Nol?", "id": "Filter Untuk Arus Harmonik." },
  { "en": "Apa Itu Tegangan Transien Pemulihan (TRV)?", "id": "Tegangan Di Kontak." },
  { "en": "Apa Itu Pembatasan Arus?", "id": "Membatasi Magnitudo Arus." },
  { "en": "Apa Itu Sekering Pembatas Arus?", "id": "Sekering Yang Membatasi." },
  { "en": "Apa Itu Pemutus Sirkuit Pembatas Arus?", "id": "Pemutus Sirkuit." },
  { "en": "Apa Itu Osilasi Torsi Sub-Sinkron?", "id": "Interaksi Antara Sistem." },
  { "en": "Apa Itu Kompensasi Seri?", "id": "Menyebabkan Risiko Osilasi." },
  { "en": "Apa Itu Filter Aktif Hibrida?", "id": "Menggabungkan Beberapa Tipe." },
  { "en": "Apa Itu Sistem Pembangkit Listrik?", "id": "Menghasilkan Energi Listrik." },
  { "en": "Apa Itu Sistem Transmisi?", "id": "Menyalurkan Energi Listrik." },
  { "en": "Apa Itu Sistem Distribusi?", "id": "Mendistribusikan Energi." },
  { "en": "Apa Itu Beban Pengguna Akhir?", "id": "Peralatan Yang Menggunakan." },
  { "en": "Apa Itu Keseimbangan Pembangkitan-Beban?", "id": "Pembangkitan Harus Sama." },
  { "en": "Apa Itu Kontrol Frekuensi?", "id": "Menjaga Keseimbangan." },
  { "en": "Apa Itu Kontrol Tegangan?", "id": "Menjaga Keseimbangan." },
  { "en": "Apa Itu Cadangan Operasi?", "id": "Kapasitas Pembangkitan Cadangan." },
  { "en": "Apa Itu Keandalan?", "id": "Kemampuan Menyediakan." },
  { "en": "Apa Itu Keamanan?", "id": "Kemampuan Menahan Gangguan." },
  { "en": "Apa Itu Pasar Listrik?", "id": "Sistem Untuk Jual Beli." }



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
