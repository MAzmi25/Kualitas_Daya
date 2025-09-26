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


  { "en": "Apa Itu Kualitas Daya?", "id": "Karakteristik Tegangan, Arus, Dan Frekuensi Listrik." },
  { "en": "Mengapa Kualitas Daya Penting?", "id": "Mempengaruhi Kinerja Dan Umur Peralatan." },
  { "en": "Apa Konsekuensi Kualitas Daya Buruk?", "id": "Kerusakan Peralatan, Kehilangan Produksi, Inefisiensi." },
  { "en": "Siapa Yang Bertanggung Jawab Atas Kualitas Daya?", "id": "Penyedia Utilitas Dan Pengguna Akhir." },
  { "en": "Apa Saja Kategori Utama Masalah Kualitas Daya?", "id": "Variasi Tegangan, Harmonik, Transien, Dan Interupsi." },
  { "en": "Apa Itu Tegangan Nominal?", "id": "Nilai Tegangan Sistem Yang Diharapkan." },
  { "en": "Apa Itu Frekuensi Nominal?", "id": "Frekuensi Sistem Yang Diharapkan (50/60 Hz)." },
  { "en": "Apa Itu Voltage Sag (Turun Tegangan)?", "id": "Penurunan Tegangan RMS (Root Mean Square) Jangka Pendek." },
  { "en": "Berapa Durasi Voltage Sag?", "id": "Setengah Siklus Hingga Satu Menit." },
  { "en": "Apa Penyebab Umum Voltage Sag?", "id": "Penyalaan Motor Besar Atau Gangguan Jaringan." },
  { "en": "Apa Efek Voltage Sag?", "id": "Reset Komputer, Kontaktor Terbuka, Produksi Gagal." },
  { "en": "Apa Itu Voltage Swell (Naik Tegangan)?", "id": "Kenaikan Tegangan RMS (Root Mean Square) Jangka Pendek." },
  { "en": "Apa Penyebab Voltage Swell?", "id": "Pemutusan Beban Besar Atau Gangguan Fasa." },
  { "en": "Apa Itu Interupsi?", "id": "Kehilangan Total Tegangan Suplai Listrik." },
  { "en": "Apa Itu Interupsi Sesaat?", "id": "Interupsi Berdurasi Sangat Pendek (Kurang 3 Detik)." },
  { "en": "Apa Itu Interupsi Sementara?", "id": "Interupsi Berdurasi Hingga Beberapa Detik." },
  { "en": "Apa Itu Interupsi Berkelanjutan?", "id": "Interupsi Berdurasi Lebih Dari Satu Menit." },
  { "en": "Apa Itu Undervoltage?", "id": "Kondisi Tegangan Di Bawah Batas Normal." },
  { "en": "Apa Durasi Undervoltage?", "id": "Lebih Dari Satu Menit Lamanya." },
  { "en": "Apa Itu Overvoltage?", "id": "Kondisi Tegangan Di Atas Batas Normal." },
  { "en": "Apa Penyebab Overvoltage?", "id": "Tap Trafo Salah Atau Kapasitor Bank." },
  { "en": "Apa Itu Transien?", "id": "Peristiwa Tegangan Atau Arus Sub-siklus." },
  { "en": "Apa Dua Jenis Transien?", "id": "Impulsif Dan Osilatoris." },
  { "en": "Apa Itu Transien Impulsif?", "id": "Pulsa Searah Dengan Polaritas Tunggal." },
  { "en": "Apa Penyebab Transien Impulsif?", "id": "Sambaran Petir Atau Switching Beban." },
  { "en": "Apa Itu Transien Osilatoris?", "id": "Pulsa Tegangan Atau Arus Yang Berosilasi." },
  { "en": "Apa Penyebab Transien Osilatoris?", "id": "Switching Kapasitor Bank Atau Kabel Bawah Tanah." },
  { "en": "Apa Itu Notching?", "id": "Gangguan Periodik Pada Gelombang Tegangan." },
  { "en": "Apa Penyebab Notching?", "id": "Operasi Normal Dari Konverter Daya." },
  { "en": "Apa Itu Harmonik?", "id": "Komponen Frekuensi Kelipatan Dari Fundamental." },
  { "en": "Apa Itu Frekuensi Fundamental?", "id": "Frekuensi Dasar Sistem Tenaga (50/60 Hz)." },
  { "en": "Apa Penyebab Harmonik?", "id": "Beban Non-linier." },
  { "en": "Apa Itu Beban Non-linier?", "id": "Beban Dimana Arus Tidak Berbentuk Sinusoidal." },
  { "en": "Sebutkan Contoh Beban Non-linier?", "id": "Komputer, VFD (Variable Frequency Drive), Penyearah, Lampu LED." },
  { "en": "Apa Efek Harmonik?", "id": "Pemanasan Berlebih, Distorsi Tegangan, Kerusakan." },
  { "en": "Apa Itu Total Harmonic Distortion (THD)?", "id": "Ukuran Total Dari Distorsi Harmonik." },
  { "en": "THD (Total Harmonic Distortion) Apa Yang Diukur?", "id": "THD Tegangan (THDv) Dan Arus (THDi)." },
  { "en": "Apa Itu Harmonik Orde Tiga?", "id": "Harmonik Pada Tiga Kali Frekuensi Fundamental." },
  { "en": "Apa Masalah Dengan Harmonik Orde Tiga?", "id": "Menyebabkan Arus Berlebih Di Konduktor Netral." },
  { "en": "Apa Itu Interharmonik?", "id": "Komponen Frekuensi Bukan Kelipatan Bilangan Bulat." },
  { "en": "Apa Itu Fluktuasi Tegangan?", "id": "Variasi Sistematis Pada Amplop Tegangan." },
  { "en": "Apa Itu Flicker?", "id": "Persepsi Visual Dari Fluktuasi Kecerahan Cahaya." },
  { "en": "Apa Penyebab Flicker?", "id": "Beban Besar Yang Berubah Secara Cepat." },
  { "en": "Contoh Penyebab Flicker Adalah Apa?", "id": "Tungku Busur Listrik Atau Mesin Las." },
  { "en": "Apa Itu Ketidakseimbangan Tegangan?", "id": "Tegangan Pada Sistem Tiga Fasa Tidak Sama." },
  { "en": "Apa Efek Ketidakseimbangan Tegangan?", "id": "Menyebabkan Pemanasan Berlebih Pada Motor." },
  { "en": "Apa Penyebab Ketidakseimbangan Tegangan?", "id": "Distribusi Beban Satu Fasa Tidak Merata." },
  { "en": "Apa Itu Variasi Frekuensi?", "id": "Penyimpangan Dari Frekuensi Nominal Sistem." },
  { "en": "Apa Penyebab Variasi Frekuensi?", "id": "Ketidakseimbangan Antara Pembangkitan Dan Beban." },
  { "en": "Apa Itu Peralatan Sensitif?", "id": "Peralatan Yang Mudah Terpengaruh Gangguan." },
  { "en": "Apa Itu Point of Common Coupling (PCC)?", "id": "Titik Sambungan Antara Utilitas Dan Pengguna." },
  { "en": "Apa Itu Standar Kualitas Daya?", "id": "Menetapkan Batas Fenomena Kualitas Daya." },
  { "en": "Sebutkan Standar Kualitas Daya Populer?", "id": "IEEE (Institute of Electrical and Electronics Engineers) 519, EN 50160." },
  { "en": "Apa Yang Diatur IEEE (Institute of Electrical and Electronics Engineers) 519?", "id": "Batas Distorsi Harmonik Pada PCC." },
  { "en": "Apa Yang Diatur EN 50160?", "id": "Karakteristik Tegangan Di Jaringan Publik Eropa." },
  { "en": "Apa Itu Power Quality Monitoring?", "id": "Proses Mengukur Dan Menganalisis Kualitas Daya." },
  { "en": "Apa Itu Penganalisis Kualitas Daya?", "id": "Instrumen Pengukur Fenomena Kualitas Daya." },
  { "en": "Apa Itu Gelombang (Waveform)?", "id": "Bentuk Grafis Dari Sebuah Sinyal." },
  { "en": "Apa Itu Gelombang Sinus?", "id": "Bentuk Gelombang Ideal Untuk Tegangan AC." },
  { "en": "Apa Itu Nilai RMS (Root Mean Square)?", "id": "Nilai Efektif Dari Gelombang AC." },
  { "en": "Apa Itu Crest Factor?", "id": "Rasio Nilai Puncak Terhadap Nilai RMS." },
  { "en": "Berapa Crest Factor Gelombang Sinus?", "id": "Akar Dua (Sekitar 1.414)." },
  { "en": "Apa Itu Faktor K?", "id": "Peringkat Transformator Untuk Menangani Beban Harmonik." },
  { "en": "Apa Itu Pembumian (Grounding)?", "id": "Koneksi Konduktif Ke Massa Bumi." },
  { "en": "Mengapa Pembumian Penting Untuk Kualitas Daya?", "id": "Menyediakan Referensi Tegangan Yang Stabil." },
  { "en": "Apa Itu Derau (Noise)?", "id": "Sinyal Listrik Frekuensi Tinggi Yang Tidak Diinginkan." },
  { "en": "Apa Itu Common-Mode Noise?", "id": "Derau Antara Konduktor Dan Ground." },
  { "en": "Apa Itu Differential-Mode Noise?", "id": "Derau Antara Dua Konduktor Jalur." },
  { "en": "Apa Itu Solusi Kualitas Daya?", "id": "Peralatan Untuk Memitigasi Masalah Kualitas Daya." },
  { "en": "Apa Itu Uninterruptible Power Supply (UPS)?", "id": "Menyediakan Daya Cadangan Saat Listrik Padam." },
  { "en": "Apa Tipe UPS (Uninterruptible Power Supply) Online?", "id": "Memberikan Perlindungan Kualitas Daya Terbaik." },
  { "en": "Apa Itu Surge Protective Device (SPD)?", "id": "Melindungi Peralatan Dari Lonjakan Tegangan Transien." },
  { "en": "Nama Lain SPD (Surge Protective Device) Adalah Apa?", "id": "Surge Arrester Atau TVSS (Transient Voltage Surge Suppressor)." },
  { "en": "Apa Itu Transient Voltage Surge Suppressor (TVSS)?", "id": "Nama Lain Untuk SPD (Surge Protective Device)." },
  { "en": "Apa Itu Metal Oxide Varistor (MOV)?", "id": "Komponen Umum Dalam SPD (Surge Protective Device)." },
  { "en": "Apa Itu Filter Harmonik?", "id": "Perangkat Untuk Mengurangi Distorsi Harmonik." },
  { "en": "Apa Itu Filter Harmonik Pasif?", "id": "Menggunakan Induktor Dan Kapasitor (LC)." },
  { "en": "Apa Itu Filter Harmonik Aktif?", "id": "Menggunakan Elektronika Daya Untuk Mengkompensasi Harmonik." },
  { "en": "Apa Itu Dynamic Voltage Restorer (DVR)?", "id": "Mengkompensasi Voltage Sag Dan Swell Secara Cepat." },
  { "en": "Apa Itu Static VAR Compensator (SVC)?", "id": "Mengatur Tegangan Dengan Mengontrol Daya Reaktif." },
  { "en": "Apa Itu STATCOM (Static Synchronous Compensator)?", "id": "SVC (Static VAR Compensator) Berbasis Konverter." },
  { "en": "Apa Itu Transformator Isolasi?", "id": "Mentransfer Daya Melalui Induksi Magnetik." },
  { "en": "Bagaimana Transformator Isolasi Membantu?", "id": "Dapat Memblokir Derau Common-Mode." },
  { "en": "Apa Itu Zero Sequence Blocking Transformer?", "id": "Memblokir Arus Harmonik Urutan Nol." },
  { "en": "Apa Itu Regulator Tegangan?", "id": "Menjaga Tegangan Output Agar Tetap Konstan." },
  { "en": "Apa Itu Power Conditioner?", "id": "Perangkat Yang Meningkatkan Kualitas Daya Listrik." },
  { "en": "Apa Itu Daya Nyata (Real Power)?", "id": "Daya Aktual Yang Digunakan Oleh Beban." },
  { "en": "Apa Satuan Daya Nyata?", "id": "Watt (W), Kilowatt (kW), Megawatt (MW)." },
  { "en": "Apa Itu Daya Reaktif (Reactive Power)?", "id": "Daya Untuk Membangkitkan Medan Magnet Dan Listrik." },
  { "en": "Apa Satuan Daya Reaktif?", "id": "VAR (Volt-Ampere Reactive)." },
  { "en": "Apa Itu Daya Tampak (Apparent Power)?", "id": "Kombinasi Vektor Dari Daya Nyata Dan Reaktif." },
  { "en": "Apa Satuan Daya Tampak?", "id": "VA (Volt-Ampere)." },
  { "en": "Apa Itu Faktor Daya (Power Factor)?", "id": "Rasio Daya Nyata Terhadap Daya Tampak." },
  { "en": "Berapa Nilai Faktor Daya Ideal?", "id": "Satu (Unity Power Factor)." },
  { "en": "Apa Penyebab Faktor Daya Rendah?", "id": "Beban Induktif Seperti Motor Dan Transformator." },
  { "en": "Apa Itu Power Factor Correction (PFC)?", "id": "Upaya Untuk Meningkatkan Faktor Daya Mendekati Satu." },
  { "en": "Bagaimana Cara Memperbaiki Faktor Daya?", "id": "Menambahkan Kapasitor Bank Secara Paralel." },
  { "en": "Apa Itu Kurva CBEMA (Computer and Business Equipment Manufacturers Association)?", "id": "Kurva Toleransi Tegangan Peralatan Komputer." },
  { "en": "Apa Itu Kurva ITIC (Information Technology Industry Council)?", "id": "Pembaruan Dari Kurva CBEMA (Computer and Business Equipment Manufacturers Association)." },
  { "en": "Apa Efek Pemanasan Harmonik?", "id": "Meningkatkan Kerugian Tembaga Dan Inti Besi." },
  { "en": "Apa Itu Efek Kulit (Skin Effect)?", "id": "Arus Frekuensi Tinggi Mengalir Di Permukaan." },
  { "en": "Bagaimana Harmonik Memperburuk Efek Kulit?", "id": "Meningkatkan Resistansi Efektif Dari Konduktor." },
  { "en": "Apa Itu Resonansi Paralel?", "id": "Osilasi Antara Kapasitor Dan Induktansi Jaringan." },
  { "en": "Bagaimana Harmonik Dapat Menyebabkan Resonansi?", "id": "Saat Frekuensi Harmonik Cocok Dengan Resonansi." },
  { "en": "Apa Akibat Resonansi Harmonik?", "id": "Tegangan Dan Arus Sangat Tinggi Berbahaya." },
  { "en": "Apa Itu Beban Linier?", "id": "Beban Dimana Arus Berbentuk Sinusoidal Sempurna." },
  { "en": "Sebutkan Contoh Beban Linier?", "id": "Pemanas Resistif, Lampu Pijar, Motor Induksi." },
  { "en": "Apa Itu Distorsi Gelombang?", "id": "Penyimpangan Dari Bentuk Gelombang Sinus Ideal." },
  { "en": "Apa Itu Analisis Fourier?", "id": "Memecah Gelombang Menjadi Komponen Frekuensi Sinusoidal." },
  { "en": "Apa Itu Fast Fourier Transform (FFT)?", "id": "Algoritma Cepat Untuk Melakukan Analisis Fourier." },
  { "en": "Apa Itu Spektrum Frekuensi?", "id": "Plot Amplitudo Terhadap Frekuensi." },
  { "en": "Apa Itu Gelombang Kotak (Square Wave)?", "id": "Gelombang Yang Mengandung Harmonik Orde Ganjil." },
  { "en": "Apa Itu Penyearah Enam Pulsa?", "id": "Menghasilkan Harmonik Orde 5, 7, 11, 13." },
  { "en": "Apa Itu Harmonik Karakteristik?", "id": "Harmonik Yang Dihasilkan Oleh Jenis Konverter." },
  { "en": "Apa Itu Urutan Fasa?", "id": "Urutan Tegangan Fasa Mencapai Puncaknya." },
  { "en": "Apa Itu Harmonik Urutan Positif?", "id": "Berputar Searah Dengan Frekuensi Fundamental." },
  { "en": "Apa Itu Harmonik Urutan Negatif?", "id": "Berputar Berlawanan Arah Dengan Fundamental." },
  { "en": "Apa Efek Harmonik Urutan Negatif?", "id": "Menyebabkan Pemanasan Berlebih Pada Motor Listrik." },
  { "en": "Apa Itu Harmonik Urutan Nol?", "id": "Sefasa Di Ketiga Fasa Sistem." },
  { "en": "Di Mana Arus Harmonik Urutan Nol Mengalir?", "id": "Di Konduktor Netral Sistem Empat Kawat." },
  { "en": "Apa Itu Triplen Harmonics?", "id": "Harmonik Kelipatan Tiga (3, 9, 15)." },
  { "en": "Apakah Triplen Harmonics Termasuk Urutan Nol?", "id": "Ya, Triplen Adalah Harmonik Urutan Nol." },
  { "en": "Apa Itu Overloading Netral?", "id": "Arus Di Netral Melebihi Kapasitasnya." },
  { "en": "Apa Itu Survey Kualitas Daya?", "id": "Investigasi Untuk Mengidentifikasi Masalah Kualitas Daya." },
  { "en": "Apa Langkah Pertama Survey?", "id": "Mengumpulkan Informasi Awal Dan Data Sistem." },
  { "en": "Apa Itu Pengukuran Jangka Panjang?", "id": "Pemantauan Kualitas Daya Selama Beberapa Waktu." },
  { "en": "Apa Itu Pengukuran Diagnostik?", "id": "Pengukuran Jangka Pendek Untuk Masalah Spesifik." },
  { "en": "Apa Itu Current Transformer (CT)?", "id": "Sensor Untuk Mengukur Arus Tinggi Akurat." },
  { "en": "Apa Itu Potential Transformer (PT)?", "id": "Sensor Untuk Mengukur Tegangan Tinggi Akurat." },
  { "en": "Apa Itu Kompensasi Daya Reaktif?", "id": "Nama Lain Untuk Koreksi Faktor Daya." },
  { "en": "Apa Itu Kapasitor Bank?", "id": "Sekelompok Kapasitor Untuk Kompensasi Daya Reaktif." },
  { "en": "Apa Itu Detuned Filter?", "id": "Filter Pasif Untuk Menghindari Resonansi Harmonik." },
  { "en": "Apa Itu Tuned Filter?", "id": "Filter Pasif Ditala Ke Frekuensi Harmonik." },
  { "en": "Apa Itu Shunt Active Power Filter (APF)?", "id": "Menyuntikkan Arus Harmonik Kompensasi Ke Jaringan." },
  { "en": "Apa Itu Series Active Power Filter?", "id": "Mengkompensasi Masalah Sisi Tegangan." },
  { "en": "Apa Itu Unified Power Quality Conditioner (UPQC)?", "id": "Menggabungkan Filter Harmonik Shunt Dan Seri." },
  { "en": "Apa Itu Voltage Regulation?", "id": "Menjaga Tegangan Dalam Batas Yang Ditentukan." },
  { "en": "Apa Itu On-Load Tap Changer (OLTC)?", "id": "Mengubah Rasio Transformator Saat Beroperasi." },
  { "en": "Apa Itu Step-Voltage Regulator?", "id": "Jenis Transformator Dengan Tap Changer Otomatis." },
  { "en": "Apa Itu Ferroresonant Transformer?", "id": "Transformator Yang Menggunakan Prinsip Resonansi." },
  { "en": "Apa Keuntungan Ferroresonant Transformer?", "id": "Regulasi Tegangan Baik Dan Penekanan Derau." },
  { "en": "Apa Kerugian Ferroresonant Transformer?", "id": "Sensitif Terhadap Variasi Frekuensi Dan Beban." },
  { "en": "Apa Itu Motor-Generator Set?", "id": "Mengisolasi Beban Secara Mekanis Dari Jaringan." },
  { "en": "Apa Itu Sag-Proofing?", "id": "Membuat Peralatan Tahan Terhadap Voltage Sag." },
  { "en": "Apa Itu Voltage Source Inverter (VSI)?", "id": "Inverter Yang Berbasis Sumber Tegangan DC." },
  { "en": "Apa Itu Pulse Width Modulation (PWM)?", "id": "Teknik Switching Untuk Mengontrol Output Konverter." },
  { "en": "Apa Itu Variable Frequency Drive (VFD)?", "id": "Perangkat Untuk Mengontrol Kecepatan Motor AC." },
  { "en": "Apakah VFD (Variable Frequency Drive) Beban Linier?", "id": "Tidak, VFD Adalah Beban Non-linier." },
  { "en": "Apa Itu DC (Direct Current) Link?", "id": "Tahap DC Dalam Konverter Daya AC-DC-AC." },
  { "en": "Apa Fungsi Kapasitor DC (Direct Current) Link?", "id": "Menstabilkan Tegangan Bus DC." },
  { "en": "Apa Itu Pengereman Regeneratif?", "id": "Mengembalikan Energi Pengereman Ke Sumber Daya." },
  { "en": "Apa Itu Switching Frekuensi Tinggi?", "id": "Menghasilkan Harmonik Pada Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu EMI (Electromagnetic Interference) Filter?", "id": "Menekan Gangguan Frekuensi Radio." },
  { "en": "Apa Itu Gangguan Terkonduksi?", "id": "EMI (Electromagnetic Interference) Merambat Melalui Kabel." },
  { "en": "Apa Itu Gangguan Terdiasi?", "id": "EMI (Electromagnetic Interference) Merambat Melalui Udara." },
  { "en": "Apa Itu Kompatibilitas Elektromagnetik (EMC)?", "id": "Kemampuan Perangkat Berfungsi Tanpa Menyebabkan EMI." },
  { "en": "Apa Itu Ground Loop?", "id": "Jalur Arus Tidak Diinginkan Melalui Ground." },
  { "en": "Apa Itu Single-Point Grounding?", "id": "Semua Ground Terhubung Di Satu Titik Referensi." },
  { "en": "Apa Itu Multi-Point Grounding?", "id": "Ground Terhubung Di Banyak Titik." },
  { "en": "Apa Itu Sistem Tenaga Fasa-Tunggal?", "id": "Menggunakan Dua Atau Tiga Kawat Suplai." },
  { "en": "Apa Itu Sistem Tenaga Tiga-Fasa?", "id": "Sistem Paling Umum Untuk Transmisi Daya." },
  { "en": "Apa Itu Koneksi Wye (Bintang)?", "id": "Koneksi Tiga-Fasa Dengan Titik Netral." },
  { "en": "Apa Itu Koneksi Delta?", "id": "Koneksi Tiga-Fasa Tanpa Titik Netral." },
  { "en": "Apa Itu Arus Urutan Nol?", "id": "Komponen Arus Yang Sefasa Pada Tiga Fasa." },
  { "en": "Apakah Arus Urutan Nol Mengalir Di Delta?", "id": "Tidak, Arus Ini Terperangkap Di Dalam Delta." },
  { "en": "Apa Itu Komponen Simetris?", "id": "Metode Analisis Sistem Tiga-Fasa Tidak Seimbang." },
  { "en": "Apa Itu Transformator K-Factor?", "id": "Transformator Dirancang Khusus Untuk Beban Non-linier." },
  { "en": "Apa Itu Derating?", "id": "Mengoperasikan Peralatan Di Bawah Kapasitas Penuhnya." },
  { "en": "Apa Itu Gangguan (Fault)?", "id": "Kondisi Abnormal Seperti Hubung Singkat." },
  { "en": "Apa Itu Arus Gangguan?", "id": "Arus Sangat Tinggi Selama Kondisi Gangguan." },
  { "en": "Apa Itu Perangkat Proteksi?", "id": "Sekering, Pemutus Sirkuit, Dan Relai." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Saklar Otomatis Untuk Melindungi Sirkuit." },
  { "en": "Apa Itu Sekering (Fuse)?", "id": "Perangkat Pelindung Arus Berlebih Sekali Pakai." },
  { "en": "Apa Itu Koordinasi Proteksi?", "id": "Memastikan Perangkat Proteksi Bekerja Secara Selektif." },
  { "en": "Apa Itu Recloser?", "id": "Pemutus Sirkuit Yang Dapat Menutup Kembali." },
  { "en": "Apa Itu Flicker Meter?", "id": "Instrumen Khusus Untuk Mengukur Flicker." },
  { "en": "Apa Itu Short-Circuit Ratio (SCR)?", "id": "Ukuran Kekuatan Atau Kekakuan Sistem Tenaga." },
  { "en": "Apa Itu Sistem Kaku (Stiff System)?", "id": "Sistem Dengan SCR (Short-Circuit Ratio) Tinggi." },
  { "en": "Apa Itu Sistem Lemah (Weak System)?", "id": "Sistem Dengan SCR (Short-Circuit Ratio) Rendah." },
  { "en": "Apa Itu Islanding?", "id": "Bagian Jaringan Beroperasi Terisolasi Dari Grid." },
  { "en": "Apa Itu Distributed Generation (DG)?", "id": "Pembangkit Listrik Skala Kecil Dan Tersebar." },
  { "en": "Contoh DG (Distributed Generation) Adalah Apa?", "id": "Panel Surya Atap, Turbin Angin Kecil." },
  { "en": "Bagaimana DG (Distributed Generation) Mempengaruhi Kualitas Daya?", "id": "Dapat Menyebabkan Variasi Tegangan Dan Harmonik." },
  { "en": "Apa Itu Microgrid?", "id": "Jaringan Listrik Lokal Yang Dapat Beroperasi." },
  { "en": "Apa Itu Smart Grid?", "id": "Jaringan Listrik Dengan Komunikasi Dan Kontrol." },
  { "en": "Apa Itu Advanced Metering Infrastructure (AMI)?", "id": "Infrastruktur Komunikasi Untuk Meteran Cerdas." },
  { "en": "Apa Itu Meteran Cerdas?", "id": "Meteran Dengan Komunikasi Dua Arah." },
  { "en": "Apa Itu Phasor Measurement Unit (PMU)?", "id": "Mengukur Fasor Tegangan Dan Arus Sinkron." },
  { "en": "Apa Itu Sinkrofasor?", "id": "Fasor Yang Disinkronkan Dengan Waktu GPS." },
  { "en": "Apa Itu Wide Area Monitoring System (WAMS)?", "id": "Memantau Jaringan Luas Menggunakan PMU." },
  { "en": "Apa Itu Ketidakstabilan Tegangan?", "id": "Penurunan Tegangan Progresif Dan Tak Terkendali." },
  { "en": "Apa Itu Voltage Collapse?", "id": "Bentuk Ekstrem Dari Ketidakstabilan Tegangan." },
  { "en": "Apa Itu Analisis Aliran Daya?", "id": "Menghitung Tegangan Dan Aliran Daya Jaringan." },
  { "en": "Apa Itu Jaringan Distribusi?", "id": "Bagian Sistem Tenaga Ke Pelanggan Akhir." },
  { "en": "Apa Itu Jaringan Transmisi?", "id": "Menyalurkan Daya Jarak Jauh Tegangan Tinggi." },
  { "en": "Apa Itu Gardu Induk?", "id": "Menghubungkan Jaringan Transmisi Dan Distribusi." },
  { "en": "Apa Itu Sistem SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem Pemantauan Dan Kontrol Jarak Jauh." },
  { "en": "Apa Itu Studi Kasus Kualitas Daya?", "id": "Analisis Terperinci Masalah Kualitas Daya Nyata." },
  { "en": "Apa Itu Troubleshooting?", "id": "Proses Mengidentifikasi Dan Memperbaiki Masalah." },
  { "en": "Apa Itu Analisis Akar Penyebab?", "id": "Menemukan Penyebab Dasar Suatu Masalah." },
  { "en": "Apa Itu Osilasi Torsi?", "id": "Osilasi Mekanis Pada Poros Motor Listrik." },
  { "en": "Bagaimana Harmonik Menyebabkan Osilasi Torsi?", "id": "Interaksi Antara Medan Magnet Harmonik." },
  { "en": "Apa Itu Gangguan Tegangan?", "id": "Kategori Umum Untuk Sag, Swell, Interupsi." },
  { "en": "Apa Itu Karakteristik Beban?", "id": "Bagaimana Sebuah Beban Menarik Arus Listrik." },
  { "en": "Apa Itu Penandatanganan Kualitas Daya?", "id": "Karakteristik Unik Dari Gangguan Kualitas Daya." },
  { "en": "Apa Itu Pemantauan Kualitas Daya Jarak Jauh?", "id": "Mengumpulkan Data Kualitas Daya Dari Jarak Jauh." },
  { "en": "Apa Itu Agregasi Data?", "id": "Menggabungkan Data Dari Berbagai Sumber Pengukuran." },
  { "en": "Apa Itu Analisis Statistik?", "id": "Menganalisis Data Menggunakan Metode Statistik." },
  { "en": "Apa Itu Indeks Kualitas Daya?", "id": "Angka Tunggal Mewakili Kualitas Daya Keseluruhan." },
  { "en": "Apa Itu Sistem Tenaga Listrik Kapal?", "id": "Jaringan Listrik Terisolasi Di Atas Kapal." },
  { "en": "Apa Tantangan Kualitas Daya Di Kapal?", "id": "Jaringan Lemah Dan Beban Sangat Besar." },
  { "en": "Apa Itu Sistem Tenaga Listrik Pesawat?", "id": "Jaringan Listrik Di Dalam Pesawat Terbang." },
  { "en": "Berapa Frekuensi Umum Di Pesawat?", "id": "400 Hz (Hertz)." },
  { "en": "Mengapa Frekuensi 400 Hz Digunakan?", "id": "Memungkinkan Transformator Dan Motor Lebih Kecil." },
  { "en": "Apa Itu Ferroresonansi?", "id": "Jenis Overvoltage Non-linier Yang Kompleks." },
  { "en": "Kapan Ferroresonansi Terjadi?", "id": "Interaksi Trafo Dan Kapasitansi Jaringan." },
  { "en": "Apa Itu Inrush Current?", "id": "Arus Puncak Saat Transformator Diberi Energi." },
  { "en": "Berapa Besar Inrush Current?", "id": "Bisa Mencapai 10-15 Kali Arus Nominal." },
  { "en": "Apa Efek Inrush Current?", "id": "Dapat Menyebabkan Voltage Sag Sesaat." },
  { "en": "Apa Itu Solid-State Transformer (SST)?", "id": "Transformator Cerdas Berbasis Elektronika Daya." },
  { "en": "Bagaimana SST (Solid-State Transformer) Dapat Meningkatkan Kualitas Daya?", "id": "Menyediakan Kontrol Tegangan Dan Isolasi Gangguan." },
  { "en": "Apa Itu Konverter Matriks?", "id": "Konverter AC-AC (Alternating Current) Langsung Tanpa Tautan DC." },
  { "en": "Apa Itu Kontrol Prediktif?", "id": "Strategi Kontrol Yang Menggunakan Model Sistem." },
  { "en": "Apa Itu Kontrol Logika Fuzzy?", "id": "Menggunakan Logika Fuzzy Untuk Keputusan Kontrol." },
  { "en": "Apa Itu Jaringan Saraf Tiruan?", "id": "Model Komputasi Untuk Pembelajaran Dari Data." },
  { "en": "Apa Itu Desain Filter?", "id": "Merancang Filter Untuk Frekuensi Tertentu." },
  { "en": "Apa Itu Faktor Kualitas (Q)?", "id": "Ukuran Kualitas Atau Selektivitas Rangkaian Resonan." },
  { "en": "Filter Dengan Q (Faktor Kualitas) Tinggi Berarti Apa?", "id": "Sangat Selektif Tetapi Rentan Beresonansi." },
  { "en": "Apa Itu Damping?", "id": "Menambahkan Resistansi Untuk Mengurangi Efek Osilasi." },
  { "en": "Apa Itu Teori Daya Reaktif Sesaat?", "id": "Juga Dikenal Sebagai Teori P-Q Akagi." },
  { "en": "Untuk Apa Teori P-Q Digunakan?", "id": "Untuk Mengontrol Filter Daya Aktif." },
  { "en": "Apa Itu Synchronous Reference Frame (SRF) Theory?", "id": "Metode Kontrol Berbasis Transformasi D-Q." },
  { "en": "Apa Itu Transformasi Clarke?", "id": "Mengubah Besaran Tiga-Fasa Menjadi Dua-Fasa." },
  { "en": "Apa Itu Transformasi Park?", "id": "Mengubah Besaran Stasioner Menjadi Kerangka Berputar." },
  { "en": "Apa Itu Kerangka Referensi D-Q?", "id": "Kerangka Referensi Yang Berputar Sinkron." },
  { "en": "Apa Itu Phase-Locked Loop (PLL)?", "id": "Sirkuit Untuk Sinkronisasi Fasa Dengan Jaringan." },
  { "en": "Apa Itu Sinyal Umpan Balik?", "id": "Sinyal Output Yang Digunakan Untuk Koreksi." },
  { "en": "Apa Itu Sistem Loop Tertutup?", "id": "Sistem Kontrol Yang Menggunakan Umpan Balik." },
  { "en": "Apa Itu Stabilitas Loop?", "id": "Kemampuan Sistem Untuk Tetap Stabil." },
  { "en": "Apa Itu Diagram Bode?", "id": "Plot Penguatan Dan Fasa Terhadap Frekuensi." },
  { "en": "Apa Itu Gain Margin?", "id": "Ukuran Kestabilan Relatif Suatu Sistem." },
  { "en": "Apa Itu Phase Margin?", "id": "Ukuran Kestabilan Relatif Lainnya." },
  { "en": "Apa Itu Propagasi Gangguan?", "id": "Bagaimana Gangguan Menyebar Melalui Sistem Tenaga." },
  { "en": "Apa Itu Kekebalan Peralatan?", "id": "Kemampuan Peralatan Menahan Gangguan Listrik." },
  { "en": "Apa Itu Kompatibilitas?", "id": "Kemampuan Berbagai Peralatan Berfungsi Bersama." },
  { "en": "Apa Itu Kontrak Kualitas Daya?", "id": "Perjanjian Antara Utilitas Dan Pelanggan Besar." },
  { "en": "Apa Itu Benchmarking Kualitas Daya?", "id": "Membandingkan Kinerja Dengan Standar Industri." },
  { "en": "Apa Itu Audit Kualitas Daya?", "id": "Evaluasi Komprehensif Terhadap Sistem Kelistrikan." },
  { "en": "Apa Itu Simpangan (Excursion)?", "id": "Penyimpangan Dari Nilai Normal Atau Nominal." },
  { "en": "Apa Itu Durasi Peristiwa?", "id": "Lamanya Waktu Suatu Peristiwa Berlangsung." },
  { "en": "Apa Itu Magnitudo Peristiwa?", "id": "Besarnya Penyimpangan Dari Nilai Nominal." },
  { "en": "Apa Itu Bentuk Gelombang Distorsi?", "id": "Gelombang Yang Tidak Lagi Berbentuk Sinus." },
  { "en": "Apa Itu Pemotongan Puncak (Peak Clipping)?", "id": "Puncak Gelombang Sinus Terpotong Atau Rata." },
  { "en": "Apa Penyebab Pemotongan Puncak?", "id": "Saturasi Transformator Atau Catu Daya." },
  { "en": "Apa Itu Perataan Dasar (Flat-topping)?", "id": "Bentuk Lain Dari Distorsi Puncak Gelombang." },
  { "en": "Apa Itu Arus Bocor?", "id": "Arus Yang Mengalir Ke Jalur Ground." },
  { "en": "Apa Itu Ground Fault Circuit Interrupter (GFCI)?", "id": "Perangkat Keselamatan Terhadap Arus Bocor Listrik." },
  { "en": "Apa Itu Sistem Pembumian?", "id": "Koneksi Ke Massa Bumi Untuk Keselamatan." },
  { "en": "Apa Itu Sistem TN?", "id": "Sistem Pembumian Dengan Netral Terhubung Langsung." },
  { "en": "Apa Itu Sistem TT?", "id": "Sistem Pembumian Dengan Elektroda Ground Terpisah." },
  { "en": "Apa Itu Sistem IT?", "id": "Sistem Pembumian Terisolasi Atau Impedansi Tinggi." },
  { "en": "Apa Itu Ikatan Ekuipotensial?", "id": "Menghubungkan Semua Bagian Konduktif Secara Bersamaan." },
  { "en": "Apa Itu Impedansi Ground?", "id": "Impedansi Jalur Ke Massa Bumi." },
  { "en": "Apa Itu Pengukuran Empat Titik?", "id": "Metode Akurat Untuk Mengukur Impedansi Rendah." },
  { "en": "Apa Itu Osilasi?", "id": "Fluktuasi Periodik Di Sekitar Nilai Rata-rata." },
  { "en": "Apa Itu Damping?", "id": "Pengurangan Amplitudo Osilasi Seiring Waktu." },
  { "en": "Apa Itu Sirkuit RLC?", "id": "Sirkuit Dengan Resistor, Induktor, Dan Kapasitor." },
  { "en": "Apa Itu Respon Transien?", "id": "Perilaku Sistem Segera Setelah Gangguan." },
  { "en": "Apa Itu Keadaan Tunak (Steady State)?", "id": "Perilaku Sistem Setelah Transien Mereda." },
  { "en": "Apa Itu Mitigasi?", "id": "Tindakan Untuk Mengurangi Masalah Kualitas Daya." },
  { "en": "Apa Itu Penyearah Terkendali?", "id": "Penyearah Dimana Outputnya Dapat Diatur." },
  { "en": "Apa Itu Sudut Penyalaan?", "id": "Sudut Untuk Memicu Saklar Thyristor." },
  { "en": "Apa Itu Inverter?", "id": "Perangkat Yang Mengubah DC Menjadi AC." },
  { "en": "Apa Itu Inverter Sumber Tegangan?", "id": "Inverter Dengan Sumber DC (Direct Current) Tegangan." },
  { "en": "Apa Itu Inverter Sumber Arus?", "id": "Inverter Dengan Sumber DC (Direct Current) Arus." },
  { "en": "Apa Itu Modulasi Lebar Pulsa?", "id": "Teknik Switching Umum Untuk Inverter." },
  { "en": "Apa Itu Frekuensi Switching?", "id": "Laju Saklar Dihidupkan Dan Dimatikan." },
  { "en": "Apa Itu Rangkaian Snubber?", "id": "Melindungi Saklar Dari Lonjakan Tegangan." },
  { "en": "Apa Itu Saklar Elektronik Daya?", "id": "Transistor, Thyristor, GTO (Gate Turn-Off Thyristor)." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor)?", "id": "Saklar Cepat Untuk Aplikasi Daya Rendah." },
  { "en": "Apa Itu IGBT (Insulated-Gate Bipolar Transistor)?", "id": "Saklar Untuk Aplikasi Daya Menengah-Tinggi." },
  { "en": "Apa Itu Thyristor?", "id": "Saklar Semi-terkendali, Sulit Dimatikan." },
  { "en": "Apa Itu Commutation?", "id": "Proses Mematikan Saklar Thyristor." },
  { "en": "Apa Itu Natural Commutation?", "id": "Komutasi Alami Dalam Jaringan AC (Alternating Current)." },
  { "en": "Apa Itu Forced Commutation?", "id": "Memaksa Thyristor Mati Menggunakan Sirkuit Tambahan." },
  { "en": "Apa Itu Efisiensi Konverter?", "id": "Rasio Daya Output Terhadap Daya Input." },
  { "en": "Apa Itu Kerugian Switching?", "id": "Kehilangan Daya Selama Transisi Saklar (On/Off)." },
  { "en": "Apa Itu Kerugian Konduksi?", "id": "Kehilangan Daya Saat Saklar Dalam Keadaan On." },
  { "en": "Apa Itu Soft Switching?", "id": "Teknik Mengurangi Kerugian Daya Switching." },
  { "en": "Apa Itu Zero Voltage Switching (ZVS)?", "id": "Switching Saat Tegangan Melintasi Saklar Nol." },
  { "en": "Apa Itu Zero Current Switching (ZCS)?", "id": "Switching Saat Arus Melalui Saklar Nol." },
  { "en": "Apa Itu Konverter Resonan?", "id": "Menggunakan Resonansi Untuk Mencapai Soft Switching." },
  { "en": "Apa Itu Manajemen Termal?", "id": "Mengelola Panas Yang Dihasilkan Komponen." },
  { "en": "Apa Itu Heatsink?", "id": "Perangkat Untuk Mendinginkan Komponen Elektronik." },
  { "en": "Apa Itu Resistansi Termal?", "id": "Hambatan Terhadap Aliran Energi Panas." },
  { "en": "Apa Itu Studi Kasus?", "id": "Analisis Terperinci Masalah Kualitas Daya Nyata." },
  { "en": "Apa Itu Klasifikasi Peristiwa?", "id": "Mengidentifikasi Jenis Gangguan Kualitas Daya." },
  { "en": "Apa Itu Pembelajaran Mesin?", "id": "AI (Artificial Intelligence) Yang Belajar Dari Data." },
  { "en": "Bagaimana Pembelajaran Mesin Digunakan?", "id": "Untuk Klasifikasi Otomatis Peristiwa Kualitas Daya." },
  { "en": "Apa Itu Kompensasi?", "id": "Tindakan Untuk Memperbaiki Masalah Kualitas Daya." },
  { "en": "Apa Itu Pengkondisian Daya?", "id": "Meningkatkan Kualitas Daya Listrik Secara Aktif." },
  { "en": "Apa Itu Sistem Tenaga Listrik?", "id": "Jaringan Untuk Menyalurkan Listrik Dari Pembangkit." },
  { "en": "Apa Itu Pembangkitan?", "id": "Proses Menghasilkan Energi Listrik." },
  { "en": "Apa Itu Transmisi?", "id": "Menyalurkan Listrik Jarak Jauh Tegangan Tinggi." },
  { "en": "Apa Itu Distribusi?", "id": "Menyalurkan Listrik Ke Pengguna Akhir." },
  { "en": "Apa Itu Beban Listrik?", "id": "Peralatan Yang Mengkonsumsi Energi Listrik." },
  { "en": "Apa Itu Jaringan Listrik (Grid)?", "id": "Jaringan Transmisi Dan Distribusi Terinterkoneksi." },
  { "en": "Apa Itu Keandalan (Reliability)?", "id": "Kemampuan Menyediakan Listrik Tanpa Interupsi." },
  { "en": "Apa Itu Indeks Keandalan SAIDI (System Average Interruption Duration Index)?", "id": "Indeks Durasi Interupsi Rata-rata Sistem." },
  { "en": "Apa Itu Indeks Keandalan SAIFI (System Average Interruption Frequency Index)?", "id": "Indeks Frekuensi Interupsi Rata-rata Sistem." },
  { "en": "Apa Itu Pemadaman (Blackout)?", "id": "Kehilangan Daya Total Di Area Luas." },
  { "en": "Apa Itu Brownout?", "id": "Penurunan Tegangan Yang Disengaja Oleh Utilitas." },
  { "en": "Apa Itu Analisis Gelombang (Waveform)?", "id": "Mempelajari Bentuk Sinyal Listrik Terhadap Waktu." },
  { "en": "Apa Itu Distorsi?", "id": "Penyimpangan Dari Bentuk Gelombang Sinus Ideal." },
  { "en": "Apa Itu Fasor?", "id": "Representasi Vektor Dari Gelombang Sinusoidal." },
  { "en": "Apa Itu Magnitudo Fasor?", "id": "Mewakili Amplitudo Atau Nilai RMS Gelombang." },
  { "en": "Apa Itu Sudut Fasor?", "id": "Mewakili Pergeseran Fasa Relatif Gelombang." },
  { "en": "Apa Itu Diagram Fasor?", "id": "Diagram Yang Menunjukkan Hubungan Antar Fasor." },
  { "en": "Apa Itu Beban Resistif?", "id": "Beban Dimana Tegangan Dan Arus Sefasa." },
  { "en": "Apa Itu Beban Induktif?", "id": "Beban Dimana Arus Tertinggal Dari Tegangan." },
  { "en": "Apa Itu Beban Kapasitif?", "id": "Beban Dimana Arus Mendahului Tegangan." },
  { "en": "Apa Itu Impedansi?", "id": "Ukuran Total Hambatan Terhadap Arus AC." },
  { "en": "Apa Itu Resistansi?", "id": "Bagian Nyata Dari Impedansi (Disipasi Daya)." },
  { "en": "Apa Itu Reaktansi?", "id": "Bagian Imajiner Dari Impedansi (Penyimpanan Energi)." },
  { "en": "Apa Itu Reaktansi Induktif?", "id": "Reaktansi Yang Disebabkan Oleh Induktor." },
  { "en": "Apa Itu Reaktansi Kapasitif?", "id": "Reaktansi Yang Disebabkan Oleh Kapasitor." },
  { "en": "Apa Itu Frekuensi Resonansi?", "id": "Saat Reaktansi Induktif Sama Dengan Kapasitif." },
  { "en": "Apa Itu Segitiga Daya?", "id": "Hubungan Grafis Antara Jenis-jenis Daya." },
  { "en": "Apa Itu Faktor Daya Tertinggal (Lagging)?", "id": "Kondisi Yang Disebabkan Oleh Beban Induktif." },
  { "en": "Apa Itu Faktor Daya Mendahului (Leading)?", "id": "Kondisi Yang Disebabkan Oleh Beban Kapasitif." },
  { "en": "Apa Itu Faktor Daya Sejati?", "id": "Memperhitungkan Efek Distorsi Harmonik." },
  { "en": "Apa Itu Faktor Daya Perpindahan?", "id": "Faktor Daya Hanya Pada Frekuensi Fundamental." },
  { "en": "Apa Itu Model Sistem Tenaga?", "id": "Representasi Matematis Dari Jaringan Listrik." },
  { "en": "Apa Itu Analisis Aliran Beban?", "id": "Nama Lain Untuk Analisis Aliran Daya." },
  { "en": "Apa Itu Analisis Sirkuit Pendek?", "id": "Menganalisis Arus Selama Terjadi Gangguan." },
  { "en": "Apa Itu Analisis Stabilitas?", "id": "Mempelajari Kemampuan Sistem Kembali Ke Keseimbangan." },
  { "en": "Apa Itu Stabilitas Sudut Rotor?", "id": "Kemampuan Generator Tetap Bekerja Sinkron." },
  { "en": "Apa Itu Stabilitas Transien?", "id": "Respons Sistem Terhadap Gangguan Besar." },
  { "en": "Apa Itu Stabilitas Sinyal Kecil?", "id": "Respons Sistem Terhadap Gangguan Kecil." },
  { "en": "Apa Itu Osilasi Daya?", "id": "Fluktuasi Daya Aktif Antar Generator." },
  { "en": "Apa Itu Peredaman (Damping)?", "id": "Pengurangan Amplitudo Osilasi Secara Bertahap." },
  { "en": "Apa Itu Power System Stabilizer (PSS)?", "id": "Kontroler Tambahan Untuk Meredam Osilasi." },
  { "en": "Apa Itu Switchgear?", "id": "Kombinasi Saklar Dan Pemutus Sirkuit." },
  { "en": "Apa Itu Busbar?", "id": "Konduktor Logam Untuk Distribusi Daya Terpusat." },
  { "en": "Apa Itu Isolator?", "id": "Bahan Untuk Mencegah Aliran Arus Listrik." },
  { "en": "Apa Itu Surge Arrester?", "id": "Melindungi Peralatan Dari Sambaran Petir." },
  { "en": "Apa Itu Relai Proteksi?", "id": "Mendeteksi Gangguan Dan Mengisolasi Bagian." },
  { "en": "Apa Itu Relai Arus Lebih?", "id": "Bekerja Berdasarkan Tingkat Arus Yang Berlebih." },
  { "en": "Apa Itu Relai Diferensial?", "id": "Membandingkan Arus Masuk Dan Keluar Peralatan." },
  { "en": "Apa Itu Relai Jarak?", "id": "Melindungi Jalur Transmisi Berdasarkan Impedansi." },
  { "en": "Apa Itu Zona Proteksi?", "id": "Area Yang Dilindungi Oleh Sistem Relai." },
  { "en": "Apa Itu Kecepatan Operasi Relai?", "id": "Seberapa Cepat Relai Bekerja Setelah Gangguan." },
  { "en": "Apa Itu Sensitivitas Relai?", "id": "Tingkat Minimum Gangguan Untuk Operasi." },
  { "en": "Apa Itu Selektivitas?", "id": "Kemampuan Mengisolasi Bagian Yang Terganggu Saja." },
  { "en": "Apa Itu Sistem SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem Kontrol Jarak Jauh Untuk Jaringan." },
  { "en": "Apa Itu Remote Terminal Unit (RTU)?", "id": "Perangkat Di Lapangan Yang Mengumpulkan Data." },
  { "en": "Apa Itu Master Station?", "id": "Pusat Kontrol Sistem SCADA (Supervisory Control and Data Acquisition)." },
  { "en": "Apa Itu Protokol Komunikasi?", "id": "Aturan Formal Untuk Pertukaran Data." },
  { "en": "Apa Itu DNP3 (Distributed Network Protocol)?", "id": "Protokol Komunikasi Umum Untuk Utilitas." },
  { "en": "Apa Itu IEC (International Electrotechnical Commission) 61850?", "id": "Standar Komunikasi Untuk Otomasi Gardu Induk." },
  { "en": "Apa Itu Kualitas Pelayanan?", "id": "Ukuran Kinerja Penyedia Utilitas Listrik." },
  { "en": "Apa Itu Ekonomi Kualitas Daya?", "id": "Analisis Biaya Dan Manfaat Kualitas Daya." },
  { "en": "Apa Itu Biaya Gangguan?", "id": "Kerugian Finansial Akibat Kualitas Daya Buruk." },
  { "en": "Apa Itu Klasifikasi Beban?", "id": "Mengelompokkan Beban Berdasarkan Sensitivitasnya." },
  { "en": "Apa Itu Beban Kritis?", "id": "Beban Yang Membutuhkan Kualitas Daya Tinggi." },
  { "en": "Apa Itu Desain Toleran Gangguan?", "id": "Desain Sistem Yang Tahan Terhadap Gangguan." },
  { "en": "Apa Itu Topologi Jaringan?", "id": "Struktur Koneksi Fisik Jaringan Listrik." },
  { "en": "Apa Itu Jaringan Radial?", "id": "Memiliki Satu Jalur Suplai Dari Sumber." },
  { "en": "Apa Itu Jaringan Loop?", "id": "Memiliki Dua Jalur Suplai Ke Beban." },
  { "en": "Apa Itu Jaringan Jala (Mesh)?", "id": "Memiliki Banyak Jalur Suplai Yang Terhubung." },
  { "en": "Jaringan Mana Yang Paling Andal?", "id": "Jaringan Jala (Mesh)." },
  { "en": "Apa Itu Sumber Daya Energi Terdistribusi (DER)?", "id": "Nama Lain Untuk Distributed Generation (DG)." },
  { "en": "Apa Itu Co-generation?", "id": "Menghasilkan Listrik Dan Panas Secara Bersamaan." },
  { "en": "Apa Itu Tri-generation?", "id": "Menghasilkan Listrik, Panas, Dan Pendinginan." },
  { "en": "Apa Itu Kendaraan Listrik (EV)?", "id": "Kendaraan Yang Digerakkan Oleh Motor Listrik." },
  { "en": "Bagaimana EV (Electric Vehicle) Mempengaruhi Kualitas Daya?", "id": "Pengisian Dapat Menyebabkan Harmonik Dan Sag." },
  { "en": "Apa Itu Vehicle-to-Grid (V2G)?", "id": "Mobil Listrik Menyediakan Daya Ke Jaringan." },
  { "en": "Apa Itu Pengisian Cerdas (Smart Charging)?", "id": "Mengontrol Waktu Dan Laju Pengisian EV." },
  { "en": "Apa Itu Penyimpanan Energi?", "id": "Menyimpan Energi Untuk Digunakan Di Lain Waktu." },
  { "en": "Apa Itu Baterai?", "id": "Menyimpan Energi Dalam Bentuk Kimia." },
  { "en": "Apa Itu Superkapasitor?", "id": "Menyimpan Energi Dalam Medan Listrik." },
  { "en": "Apa Itu Flywheel?", "id": "Menyimpan Energi Dalam Bentuk Kinetik." },
  { "en": "Apa Itu Compressed Air Energy Storage (CAES)?", "id": "Menyimpan Energi Dalam Udara Bertekanan." },
  { "en": "Apa Itu Pumped Hydro Storage?", "id": "Menyimpan Energi Dengan Memompa Air Ke Atas." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Surya?", "id": "Mengubah Sinar Matahari Menjadi Listrik." },
  { "en": "Apa Itu Inverter Surya?", "id": "Mengubah DC Dari Panel Surya Menjadi AC." },
  { "en": "Apa Itu Maximum Power Point Tracking (MPPT)?", "id": "Mendapatkan Daya Maksimal Dari Panel Surya." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Angin?", "id": "Mengubah Energi Kinetik Angin Menjadi Listrik." },
  { "en": "Apa Itu Turbin Angin?", "id": "Mesin Yang Menangkap Energi Dari Angin." },
  { "en": "Apa Itu Generator Induksi?", "id": "Jenis Generator Umum Dalam Turbin Angin." },
  { "en": "Apa Itu Generator Sinkron?", "id": "Generator Yang Berputar Sinkron Dengan Jaringan." },
  { "en": "Apa Itu Penyearah Jembatan Tak Terkendali?", "id": "Penyearah Yang Hanya Menggunakan Dioda." },
  { "en": "Apa Itu Penyearah Jembatan Terkendali?", "id": "Penyearah Yang Menggunakan Thyristor Atau SCR." },
  { "en": "Apa Itu Faktor Tuntutan (Demand Factor)?", "id": "Rasio Tuntutan Maksimum Terhadap Beban Terhubung." },
  { "en": "Apa Itu Faktor Beban (Load Factor)?", "id": "Rasio Beban Rata-rata Terhadap Beban Puncak." },
  { "en": "Apa Itu Faktor Keanekaragaman (Diversity Factor)?", "id": "Rasio Jumlah Tuntutan Individual Terhadap Puncak." },
  { "en": "Apa Itu Pemodelan Beban?", "id": "Representasi Matematis Dari Perilaku Beban Listrik." },
  { "en": "Apa Itu Model Beban Statis?", "id": "Beban Sebagai Fungsi Tegangan Dan Frekuensi." },
  { "en": "Apa Itu Model Beban Dinamis?", "id": "Beban Sebagai Fungsi Waktu Dan Variabel." },
  { "en": "Apa Itu ZIP Model?", "id": "Model Beban (Constant Z, I, P)." },
  { "en": "Apa Itu Analisis Harmonik?", "id": "Mempelajari Penyebab Dan Efek Dari Harmonik." },
  { "en": "Apa Itu Aliran Daya Harmonik?", "id": "Menganalisis Aliran Arus Harmonik Di Jaringan." },
  { "en": "Apa Itu Impedansi Harmonik?", "id": "Impedansi Sistem Pada Frekuensi Harmonik." },
  { "en": "Apa Itu Sumber Harmonik?", "id": "Peralatan Yang Menghasilkan Arus Harmonik." },
  { "en": "Apa Itu Respon Frekuensi Sistem?", "id": "Bagaimana Sistem Merespons Frekuensi Yang Berbeda." },
  { "en": "Apa Itu Plot Impedansi?", "id": "Grafik Impedansi Terhadap Frekuensi Jaringan." },
  { "en": "Apa Itu Anti-Resonansi?", "id": "Kondisi Impedansi Sangat Tinggi Dekat Resonansi." },
  { "en": "Apa Itu Pemodelan Transformator?", "id": "Representasi Matematis Dari Transformator." },
  { "en": "Apa Itu Model Rangkaian Ekuivalen?", "id": "Sirkuit Sederhana Yang Mewakili Komponen." },
  { "en": "Apa Itu Arus Magnetisasi?", "id": "Arus Yang Dibutuhkan Untuk Menciptakan Fluks." },
  { "en": "Apa Itu Saturasi Inti?", "id": "Inti Tidak Dapat Menahan Lebih Banyak Fluks." },
  { "en": "Apa Efek Saturasi?", "id": "Menghasilkan Arus Harmonik Yang Signifikan." },
  { "en": "Apa Itu Histeresis?", "id": "Fenomena Magnetik Dalam Inti Besi." },
  { "en": "Apa Itu Kerugian Inti?", "id": "Kerugian Histeresis Dan Kerugian Arus Eddy." },
  { "en": "Apa Itu Pemodelan Saluran Transmisi?", "id": "Mewakili Saluran Untuk Analisis Kualitas Daya." },
  { "en": "Apa Itu Model Pi?", "id": "Model Rangkaian Ekuivalen Untuk Saluran." },
  { "en": "Apa Itu Model Garis Panjang?", "id": "Model Akurat Untuk Saluran Transmisi Panjang." },
  { "en": "Apa Itu Parameter Terdistribusi?", "id": "Resistansi, Induktansi, Kapasitansi Per Satuan." },
  { "en": "Apa Itu Gelombang Berjalan?", "id": "Gelombang Yang Merambat Sepanjang Saluran." },
  { "en": "Apa Itu Pantulan Gelombang?", "id": "Terjadi Akibat Ketidakcocokan Impedansi." },
  { "en": "Apa Itu Koefisien Pantul?", "id": "Rasio Gelombang Pantul Terhadap Gelombang Datang." },
  { "en": "Apa Itu Lattice Diagram?", "id": "Diagram Untuk Menganalisis Pantulan Gelombang." },
  { "en": "Apa Itu Electromagnetic Transients Program (EMTP)?", "id": "Perangkat Lunak Simulasi Untuk Transien." },
  { "en": "Apa Itu Pensinyalan Jalur Daya?", "id": "Mengirim Sinyal Kontrol Melalui Kabel Listrik." },
  { "en": "Apa Itu Ripple Control?", "id": "Contoh Pensinyalan Jalur Daya Frekuensi Rendah." },
  { "en": "Apa Itu Gelombang Elektromagnetik?", "id": "Perambatan Medan Listrik Dan Magnet." },
  { "en": "Apa Itu Kompatibilitas Elektromagnetik (EMC)?", "id": "Kemampuan Perangkat Bekerja Di Lingkungan EM." },
  { "en": "Apa Itu Interferensi Elektromagnetik (EMI)?", "id": "Gangguan Elektromagnetik Yang Tidak Diinginkan." },
  { "en": "Apa Itu Emisi?", "id": "Pelepasan Energi EM (Electromagnetic) Dari Perangkat." },
  { "en": "Apa Itu Kekebalan?", "id": "Kemampuan Menahan Gangguan EM (Electromagnetic)." },
  { "en": "Apa Itu Kopling?", "id": "Transfer Energi Gangguan Dari Sumber Ke Korban." },
  { "en": "Apa Itu Grounding Loop?", "id": "Nama Lain Untuk Masalah Ground Loop." },
  { "en": "Apa Itu Kualitas Sinyal?", "id": "Ukuran Kualitas Sinyal Kontrol Atau Data." },
  { "en": "Apa Itu Jitter?", "id": "Variasi Waktu Sinyal Dari Posisi Ideal." },
  { "en": "Apa Itu Derau Fasa?", "id": "Representasi Jitter Dalam Domain Frekuensi." },
  { "en": "Apa Itu Sistem Tenaga DC (Direct Current)?", "id": "Sistem Yang Menggunakan Arus Searah." },
  { "en": "Apa Keuntungan Sistem DC (Direct Current)?", "id": "Tidak Ada Daya Reaktif Atau Masalah Frekuensi." },
  { "en": "Apa Itu Konverter DC-DC?", "id": "Mengubah Satu Level Tegangan DC Ke Lainnya." },
  { "en": "Apa Itu Konverter AC-DC?", "id": "Nama Lain Untuk Penyearah (Rectifier)." },
  { "en": "Apa Itu Konverter DC-AC?", "id": "Nama Lain Untuk Inverter." },
  { "en": "Apa Itu Konverter AC-AC?", "id": "Mengubah Tegangan Atau Frekuensi AC." },
  { "en": "Apa Itu Sistem Tenaga Hibrida?", "id": "Sistem Yang Menggabungkan Sumber AC Dan DC." },
  { "en": "Apa Itu Inverter Siap Jaringan?", "id": "Inverter Dengan Fitur Pendukung Jaringan Cerdas." },
  { "en": "Apa Itu Kontrol Volt-VAR?", "id": "Mengatur Tegangan Dengan Mengontrol Daya Reaktif." },
  { "en": "Apa Itu Kontrol Frekuensi-Watt?", "id": "Mengatur Daya Untuk Menstabilkan Frekuensi Jaringan." },
  { "en": "Apa Itu Ride-Through Capability?", "id": "Kemampuan Tetap Terhubung Selama Gangguan." },
  { "en": "Apa Itu Low Voltage Ride-Through (LVRT)?", "id": "Melewati Gangguan Tegangan Rendah Tanpa Trip." },
  { "en": "Apa Itu High Voltage Ride-Through (HVRT)?", "id": "Melewati Gangguan Tegangan Tinggi Tanpa Trip." },
  { "en": "Apa Itu Ketidakstabilan?", "id": "Sistem Tidak Kembali Ke Keseimbangan." },
  { "en": "Apa Itu Osilasi Sub-Sinkron?", "id": "Osilasi Di Bawah Frekuensi Fundamental Sistem." },
  { "en": "Apa Itu Kontrol Tambahan?", "id": "Sistem Kontrol Untuk Meredam Osilasi Jaringan." },
  { "en": "Apa Itu Teori Kontrol?", "id": "Cabang Teknik Dan Matematika Untuk Sistem." },
  { "en": "Apa Itu Kontrol Klasik?", "id": "Metode Berbasis Fungsi Transfer Dan Frekuensi." },
  { "en": "Apa Itu Kontrol Modern?", "id": "Metode Berbasis Ruang Keadaan (State-Space)." },
  { "en": "Apa Itu Ruang Keadaan?", "id": "Representasi Matematis Dinamika Sistem." },
  { "en": "Apa Itu Kontrol Umpan Balik?", "id": "Menggunakan Sinyal Output Untuk Mengoreksi Input." },
  { "en": "Apa Itu Kontrol Umpan Maju?", "id": "Mengantisipasi Gangguan Sebelum Mempengaruhi Output." },
  { "en": "Apa Itu Kontrol Adaptif?", "id": "Kontroler Yang Menyesuaikan Parameternya Secara Online." },
  { "en": "Apa Itu Kontrol Kokoh?", "id": "Kontroler Yang Tahan Terhadap Ketidakpastian Model." },
  { "en": "Apa Itu Kontrol Optimal?", "id": "Mengoptimalkan Suatu Kriteria Kinerja Sistem." },
  { "en": "Apa Itu Estimasi Keadaan?", "id": "Memperkirakan Keadaan Internal Sistem Dari Pengukuran." },
  { "en": "Apa Itu Filter Kalman?", "id": "Estimator Optimal Untuk Sistem Linier Gaussian." },
  { "en": "Apa Itu Artificial Intelligence (AI)?", "id": "Kecerdasan Buatan Yang Ditunjukkan Oleh Mesin." },
  { "en": "Apa Itu Jaringan Saraf Tiruan?", "id": "Model Pembelajaran Mesin Terinspirasi Otak." },
  { "en": "Apa Itu Logika Fuzzy?", "id": "Logika Yang Menangani Ketidakpastian Dan Ambiguitas." },
  { "en": "Apa Itu Algoritma Genetik?", "id": "Algoritma Optimisasi Berbasis Prinsip Evolusi." },
  { "en": "Apa Itu Sistem Pakar?", "id": "Sistem Berbasis Pengetahuan Untuk Domain Tertentu." },
  { "en": "Apa Itu Data Mining?", "id": "Menemukan Pola Tersembunyi Dalam Kumpulan Data." },
  { "en": "Apa Itu Big Data?", "id": "Kumpulan Data Sangat Besar Dan Kompleks." },
  { "en": "Apa Itu Klasifikasi?", "id": "Tugas Mengelompokkan Data Ke Dalam Kategori." },
  { "en": "Apa Itu Regresi?", "id": "Tugas Memprediksi Nilai Kontinu." },
  { "en": "Apa Itu Klastering?", "id": "Menemukan Grup Alami Dalam Data." },
  { "en": "Apa Itu Penilaian Kualitas Daya?", "id": "Proses Mengevaluasi Tingkat Kualitas Daya Sistem." },
  { "en": "Apa Itu Diagnostik?", "id": "Proses Mengidentifikasi Penyebab Masalah." },
  { "en": "Apa Itu Prognostik?", "id": "Proses Memprediksi Masalah Di Masa Depan." },
  { "en": "Apa Itu Kesehatan Sistem?", "id": "Ukuran Kondisi Operasional Sistem Saat Ini." },
  { "en": "Apa Itu Keandalan?", "id": "Probabilitas Beroperasi Tanpa Kegagalan." },
  { "en": "Apa Itu Ketersediaan?", "id": "Proporsi Waktu Sistem Siap Beroperasi." },
  { "en": "Apa Itu Keterpeliharaan?", "id": "Kemudahan Dalam Memperbaiki Sistem Yang Gagal." },
  { "en": "Apa Itu FMEA (Failure Mode and Effects Analysis)?", "id": "Analisis Mode Dan Efek Kegagalan Sistem." },
  { "en": "Apa Itu FTA (Fault Tree Analysis)?", "id": "Analisis Pohon Kesalahan Secara Top-Down." },
  { "en": "Apa Itu Keamanan Siber?", "id": "Perlindungan Sistem Komputer Dari Serangan Siber." },
  { "en": "Apa Itu Kerentanan?", "id": "Kelemahan Dalam Sistem Yang Dapat Dieksploitasi." },
  { "en": "Apa Itu Ancaman?", "id": "Sesuatu Yang Dapat Menyebabkan Kerugian." },
  { "en": "Apa Itu Risiko?", "id": "Potensi Kerugian Akibat Ancaman." },
  { "en": "Apa Itu Kontrol Keamanan?", "id": "Tindakan Untuk Mengurangi Risiko Keamanan." },
  { "en": "Apa Itu Enkripsi?", "id": "Proses Mengamankan Data Dengan Kriptografi." },
  { "en": "Apa Itu Otentikasi?", "id": "Proses Memverifikasi Identitas Pengguna." },
  { "en": "Apa Itu Otorisasi?", "id": "Proses Memberikan Izin Akses." },
  { "en": "Apa Itu Tanda Tangan Digital?", "id": "Memastikan Keaslian Dan Integritas Data." },
  { "en": "Apa Itu Standar?", "id": "Spesifikasi Yang Disepakati Bersama." },
  { "en": "Apa Itu Regulasi?", "id": "Aturan Yang Diberlakukan Oleh Otoritas." },
  { "en": "Apa Itu Kepatuhan?", "id": "Tindakan Mematuhi Standar Dan Regulasi." },
  { "en": "Apa Itu Deregulasi?", "id": "Mengurangi Atau Menghilangkan Peraturan Pemerintah." },
  { "en": "Apa Itu Pasar Listrik?", "id": "Sistem Untuk Jual Beli Energi Listrik." },
  { "en": "Apa Itu Harga Pasar Real-Time?", "id": "Harga Listrik Yang Berubah Setiap Saat." },
  { "en": "Apa Itu Manajemen Sisi Permintaan (DSM)?", "id": "Mempengaruhi Pola Konsumsi Energi Pelanggan." },
  { "en": "Apa Itu Efisiensi Energi?", "id": "Menggunakan Lebih Sedikit Energi Untuk Pekerjaan Sama." },
  { "en": "Apa Itu Konservasi Energi?", "id": "Mengurangi Penggunaan Energi Secara Keseluruhan." },
  { "en": "Apa Itu Gelombang Tegangan?", "id": "Bentuk Gelombang Tegangan AC (Alternating Current)." },
  { "en": "Apa Itu Gelombang Arus?", "id": "Bentuk Gelombang Arus AC (Alternating Current)." },
  { "en": "Apa Itu Distorsi Notch?", "id": "Nama Lain Untuk Fenomena Notching." },
  { "en": "Apa Itu Komutasi?", "id": "Proses Transfer Arus Antar Saklar." },
  { "en": "Apa Itu Penyearah Terkendali Fasa?", "id": "Sumber Umum Distorsi Harmonik." },
  { "en": "Apa Itu Inverter Sumber Tegangan?", "id": "Sumber Harmonik Pada Sisi AC." },
  { "en": "Apa Itu Switch-Mode Power Supply (SMPS)?", "id": "Catu Daya Yang Menghasilkan Arus Harmonik." },
  { "en": "Apa Itu Arus Input SMPS (Switch-Mode Power Supply)?", "id": "Berbentuk Pulsa Dan Kaya Akan Harmonik." },
  { "en": "Apa Itu Tungku Busur Listrik?", "id": "Beban Besar Yang Menyebabkan Flicker Hebat." },
  { "en": "Apa Itu Mesin Las?", "id": "Beban Yang Menyebabkan Fluktuasi Dan Sag." },
  { "en": "Apa Itu Saturasi Magnetik?", "id": "Fenomena Non-linier Dalam Inti Transformator." },
  { "en": "Apa Itu Arus Magnetisasi?", "id": "Arus Untuk Membangkitkan Fluks Magnetik." },
  { "en": "Apa Itu Inrush Current?", "id": "Arus Puncak Saat Menyalakan Transformator." },
  { "en": "Apa Itu Motor Induksi?", "id": "Beban Umum Di Sektor Industri." },
  { "en": "Apakah Motor Induksi Linier?", "id": "Cukup Linier Saat Berjalan Stabil." },
  { "en": "Apa Itu Arus Awal Motor?", "id": "Arus Sangat Tinggi Saat Motor Dinyalakan." },
  { "en": "Apa Itu Soft Starter?", "id": "Mengurangi Arus Awal Motor Induksi." },
  { "en": "Apa Itu Variable Frequency Drive (VFD)?", "id": "Perangkat Untuk Mengontrol Kecepatan Motor." },
  { "en": "Apakah VFD (Variable Frequency Drive) Linier?", "id": "Tidak, VFD Adalah Beban Non-linier." },
  { "en": "Apa Itu Lampu Fluoresen?", "id": "Beban Non-linier Yang Menghasilkan Harmonik." },
  { "en": "Apa Itu Ballast Elektronik?", "id": "Catu Daya Untuk Lampu Fluoresen." },
  { "en": "Apa Itu Peralatan Medis?", "id": "Seringkali Sensitif Terhadap Gangguan Kualitas Daya." },
  { "en": "Apa Itu Pusat Data?", "id": "Sangat Kritis Terhadap Kualitas Dan Keandalan." },
  { "en": "Apa Itu Redundansi?", "id": "Memiliki Sistem Cadangan Untuk Keandalan." },
  { "en": "Apa Itu Sistem N+1?", "id": "Satu Unit Cadangan Untuk N Unit." },
  { "en": "Apa Itu Titik Kegagalan Tunggal?", "id": "Komponen Yang Kegagalannya Melumpuhkan Sistem." },
  { "en": "Apa Itu Analisis Statistik?", "id": "Menganalisis Data Kualitas Daya Menggunakan Statistik." },
  { "en": "Apa Itu Histogram?", "id": "Grafik Distribusi Frekuensi Suatu Parameter." },
  { "en": "Apa Itu Diagram Sebar (Scatter Plot)?", "id": "Grafik Hubungan Antara Dua Variabel." },
  { "en": "Apa Itu Kurva Durasi?", "id": "Menunjukkan Waktu Suatu Parameter Melebihi Nilai." },
  { "en": "Apa Itu ITI (CBEMA) Curve?", "id": "Kurva Toleransi Tegangan Untuk Peralatan IT." },
  { "en": "Apa Itu Kepatuhan Kualitas Daya?", "id": "Tindakan Memenuhi Standar Dan Regulasi." },
  { "en": "Apa Itu Investigasi Kualitas Daya?", "id": "Proses Menentukan Penyebab Masalah." },
  { "en": "Apa Itu Sumber Gangguan?", "id": "Peralatan Yang Menyebabkan Masalah Kualitas Daya." },
  { "en": "Apa Itu Jalur Propagasi?", "id": "Jalan Dimana Gangguan Menyebar." },
  { "en": "Apa Itu Korban Gangguan?", "id": "Peralatan Yang Terpengaruh Oleh Gangguan." },
  { "en": "Apa Itu Deteksi Arah Gangguan?", "id": "Menentukan Dari Mana Arah Gangguan Datang." },
  { "en": "Apa Itu Pemecahan Masalah?", "id": "Proses Menemukan Dan Memperbaiki Masalah." },
  { "en": "Apa Itu Osiloskop?", "id": "Instrumen Untuk Melihat Bentuk Gelombang." },
  { "en": "Apa Itu Multimeter True RMS?", "id": "Mengukur Nilai RMS Akurat Untuk Gelombang." },
  { "en": "Apa Itu Penganalisis Spektrum?", "id": "Melihat Sinyal Dalam Domain Frekuensi." },
  { "en": "Apa Itu Logger Data?", "id": "Perangkat Merekam Pengukuran Seiring Waktu." },
  { "en": "Apa Itu Pemicuan (Triggering)?", "id": "Memulai Pengukuran Berdasarkan Suatu Peristiwa." },
  { "en": "Apa Itu Pemicu Ambang Batas?", "id": "Pemicu Saat Sinyal Melewati Level Tertentu." },
  { "en": "Apa Itu Pengambilan Sampel?", "id": "Mengubah Sinyal Analog Menjadi Data Digital." },
  { "en": "Apa Itu Aliasing?", "id": "Distorsi Akibat Laju Sampling Terlalu Rendah." },
  { "en": "Apa Itu Jendela Waktu?", "id": "Durasi Pengamatan Untuk Analisis Sinyal." },
  { "en": "Apa Itu Kebocoran Spektral?", "id": "Efek Samping Jendela Waktu Terbatas." },
  { "en": "Apa Itu Windowing?", "id": "Fungsi Matematis Untuk Mengurangi Kebocoran." },
  { "en": "Apa Itu Jendela Hanning?", "id": "Jenis Jendela Fungsi Yang Umum." },
  { "en": "Apa Itu Jendela Persegi?", "id": "Tidak Menggunakan Jendela Sama Sekali." },
  { "en": "Apa Itu Resolusi Frekuensi?", "id": "Kemampuan Membedakan Komponen Frekuensi." },
  { "en": "Apa Itu Deteksi?", "id": "Proses Mengidentifikasi Kehadiran Peristiwa." },
  { "en": "Apa Itu Klasifikasi?", "id": "Proses Mengidentifikasi Jenis Peristiwa." },
  { "en": "Apa Itu Lokalisasi?", "id": "Proses Menemukan Sumber Peristiwa." },
  { "en": "Apa Itu Sistem Pakar?", "id": "Sistem Berbasis Aturan Untuk Diagnostik." },
  { "en": "Apa Itu Wavelet Transform?", "id": "Metode Analisis Waktu-Frekuensi." },
  { "en": "Apa Keuntungan Wavelet?", "id": "Sangat Baik Untuk Menganalisis Transien." },
  { "en": "Apa Itu Tegangan Common-Mode?", "id": "Tegangan Antara Konduktor Dan Ground Referensi." },
  { "en": "Apa Itu Tegangan Differential-Mode?", "id": "Tegangan Antara Dua Konduktor Jalur." },
  { "en": "Apa Itu Pemodelan Komponen?", "id": "Membuat Model Matematis Dari Komponen Listrik." },
  { "en": "Apa Itu SimPowerSystems?", "id": "Toolbox Simulink Untuk Sistem Tenaga Listrik." },
  { "en": "Apa Itu ATP-EMTP (Alternative Transients Program)?", "id": "Program Simulasi Transien Elektromagnetik." },
  { "en": "Apa Itu PSCAD/EMTDC?", "id": "Perangkat Lunak Simulasi Sistem Tenaga." },
  { "en": "Apa Itu Validasi Model?", "id": "Membandingkan Hasil Model Dengan Pengukuran Nyata." },
  { "en": "Apa Itu Verifikasi Model?", "id": "Memeriksa Apakah Model Diimplementasikan Dengan Benar." },
  { "en": "Apa Itu Pengukuran Di Lapangan?", "id": "Pengukuran Yang Dilakukan Di Lokasi Sebenarnya." },
  { "en": "Apa Itu Keselamatan Listrik?", "id": "Praktik Aman Saat Bekerja Dengan Listrik." },
  { "en": "Apa Itu Arc Flash?", "id": "Ledakan Listrik Berbahaya Akibat Busur Api." },
  { "en": "Apa Itu Personal Protective Equipment (PPE)?", "id": "Peralatan Pelindung Diri." },
  { "en": "Apa Itu Lockout-Tagout (LOTO)?", "id": "Prosedur Untuk Mematikan Peralatan Dengan Aman." },
  { "en": "Apa Itu Sistem Tidak Terganggu?", "id": "Sistem Yang Diisolasi Dari Suplai Utama." },
  { "en": "Apa Itu Kondisioner Jalur Aktif?", "id": "Nama Lain Untuk Active Power Filter." },
  { "en": "Apa Itu K-Factor Transformator?", "id": "Kemampuan Transformator Menangani Arus Harmonik." },
  { "en": "Apa Itu Derating Transformator?", "id": "Mengurangi Kapasitas Trafo Akibat Pemanasan Harmonik." },
  { "en": "Apa Itu Konverter 12-Pulsa?", "id": "Menghasilkan Lebih Sedikit Harmonik Dibanding 6-Pulsa." },
  { "en": "Apa Itu Konverter 18-Pulsa?", "id": "Menghasilkan Harmonik Yang Lebih Rendah Lagi." },
  { "en": "Apa Itu Transformator Pergeseran Fasa?", "id": "Digunakan Dalam Konverter Multi-pulsa." },
  { "en": "Apa Itu Interfensi Jalur Komunikasi?", "id": "Derau Mempengaruhi Jalur Telekomunikasi." },
  { "en": "Apa Itu Induksi?", "id": "Kopling Melalui Medan Magnet." },
  { "en": "Apa Itu Kenaikan Potensial Tanah?", "id": "Kenaikan Tegangan Tanah Selama Gangguan." },
  { "en": "Apa Itu Pelindung (Shielding)?", "id": "Menggunakan Lapisan Konduktif Untuk Memblokir." },
  { "en": "Apa Itu Kabel Terpilin?", "id": "Mengurangi Kopling Magnetik Dan Crosstalk." },
  { "en": "Apa Itu Sistem Penyeimbang Beban?", "id": "Mendistribusikan Beban Secara Merata Antar Fasa." },
  { "en": "Apa Itu Koreksi Faktor Daya Aktif?", "id": "Nama Lain Untuk Active Power Filter." },
  { "en": "Apa Itu Konverter Boost PFC?", "id": "Topologi Umum Untuk PFC (Power Factor Correction)." },
  { "en": "Apa Itu Mode Konduksi Kritis?", "id": "Mode Operasi PFC (Power Factor Correction)." },
  { "en": "Apa Itu Regulasi Beban?", "id": "Kemampuan Menjaga Tegangan Dengan Perubahan Beban." },
  { "en": "Apa Itu Regulasi Jalur?", "id": "Kemampuan Menjaga Tegangan Dengan Perubahan Input." },
  { "en": "Apa Itu Respon Transien?", "id": "Respons Terhadap Perubahan Beban Mendadak." },
  { "en": "Apa Itu Waktu Stabil?", "id": "Waktu Untuk Mencapai Keadaan Tunak." },
  { "en": "Apa Itu Overshoot?", "id": "Output Melebihi Nilai Akhir Sementara." },
  { "en": "Apa Itu Standar IEC (International Electrotechnical Commission)?", "id": "Organisasi Standar Elektroteknik Internasional." },
  { "en": "Apa Itu Standar IEEE (Institute of Electrical and Electronics Engineers)?", "id": "Organisasi Profesional Dan Standar Teknik." },
  { "en": "Apa Itu Standar ANSI (American National Standards Institute)?", "id": "Organisasi Standar Di Amerika Serikat." },
  { "en": "Apa Itu CENELEC?", "id": "Komite Eropa Untuk Standarisasi Elektroteknik." },
  { "en": "Apa Hubungan EN 50160 Dengan CENELEC?", "id": "EN 50160 Adalah Standar Dari CENELEC." },
  { "en": "Apa Itu Gelombang Tegangan?", "id": "Bentuk Gelombang Tegangan Listrik AC." },
  { "en": "Apa Itu Gelombang Arus?", "id": "Bentuk Gelombang Arus Listrik AC." },
  { "en": "Apa Itu Distorsi Interharmonik?", "id": "Distorsi Yang Disebabkan Oleh Komponen Interharmonik." },
  { "en": "Apa Itu Komponen DC (Direct Current)?", "id": "Adanya Tegangan Atau Arus DC Di Sistem AC." },
  { "en": "Apa Efek Komponen DC (Direct Current)?", "id": "Dapat Menyebabkan Saturasi Inti Transformator." },
  { "en": "Apa Itu Pensinyalan Frekuensi Rendah?", "id": "Sinyal Kontrol Yang Dikirim Melalui Jaringan Listrik." },
  { "en": "Apa Itu Flickersturbance?", "id": "Gangguan Yang Menyebabkan Fenomena Flicker." },
  { "en": "Apa Itu Transformator Saturable?", "id": "Transformator Yang Dirancang Untuk Beroperasi Dalam Saturasi." },
  { "en": "Apa Itu Regulator Tegangan Induksi?", "id": "Mengatur Tegangan Dengan Mengubah Kopling Magnetik." },
  { "en": "Apa Itu Seri Kapasitor?", "id": "Kapasitor Yang Dipasang Seri Di Jalur Transmisi." },
  { "en": "Apa Itu Resonansi Sub-Sinkron?", "id": "Osilasi Antara Generator Dan Jaringan Seri." },
  { "en": "Apa Itu Batas Stabilitas?", "id": "Batas Operasi Aman Sistem Tenaga Listrik." },
  { "en": "Apa Itu Redaman?", "id": "Proses Pengurangan Amplitudo Osilasi." },
  { "en": "Apa Itu Osilasi Elektromekanis?", "id": "Osilasi Antara Rotor-rotor Generator Sinkron." },
  { "en": "Apa Itu Model Mesin Sinkron?", "id": "Representasi Matematis Dari Generator Sinkron." },
  { "en": "Apa Itu Model Beban?", "id": "Representasi Matematis Dari Perilaku Beban." },
  { "en": "Apa Itu Simulasi Domain Waktu?", "id": "Menyelesaikan Persamaan Sistem Seiring Waktu." },
  { "en": "Apa Itu Simulasi Domain Frekuensi?", "id": "Menganalisis Sistem Dalam Domain Frekuensi." },
  { "en": "Apa Itu Eigenvalue Analysis?", "id": "Digunakan Untuk Menganalisis Stabilitas Sinyal Kecil." },
  { "en": "Apa Itu Nilai Eigen?", "id": "Menunjukkan Mode Osilasi Dan Redamannya." },
  { "en": "Apa Itu Vektor Eigen?", "id": "Menunjukkan Bentuk Mode Osilasi Sistem." },
  { "en": "Apa Itu Partisipasi Faktor?", "id": "Menunjukkan Elemen Mana Yang Berpartisipasi." },
  { "en": "Apa Itu Teori Normalisasi?", "id": "Metode Analisis Sistem Non-linier." },
  { "en": "Apa Itu Bifurkasi?", "id": "Perubahan Kualitatif Dalam Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Bifurkasi Hopf?", "id": "Menuju Terbentuknya Osilasi Stabil." },
  { "en": "Apa Itu Kekacauan (Chaos)?", "id": "Perilaku Kompleks Dan Tidak Dapat Diprediksi." },
  { "en": "Apa Itu Fraktal?", "id": "Pola Geometris Yang Berulang Pada Setiap Skala." },
  { "en": "Apa Itu Sistem Tenaga Kompleks?", "id": "Sistem Dengan Banyak Komponen Yang Berinteraksi." },
  { "en": "Apa Itu Topologi Jaringan?", "id": "Struktur Koneksi Fisik Jaringan." },
  { "en": "Apa Itu Teori Graf?", "id": "Cabang Matematika Untuk Studi Graf." },
  { "en": "Apa Itu Node?", "id": "Simpul Atau Titik Dalam Graf." },
  { "en": "Apa Itu Edge?", "id": "Koneksi Antara Dua Node." },
  { "en": "Apa Itu Jaringan Skala Kecil?", "id": "Jaringan Dengan Distribusi Derajat Node." },
  { "en": "Apa Itu Jaringan Bebas Skala?", "id": "Jaringan Dengan Distribusi Derajat Hukum Pangkat." },
  { "en": "Apa Itu Ketahanan Jaringan?", "id": "Kemampuan Jaringan Menahan Gangguan." },
  { "en": "Apa Itu Serangan Bertarget?", "id": "Serangan Pada Node Paling Terhubung." },
  { "en": "Apa Itu Kegagalan Acak?", "id": "Kegagalan Acak Pada Node Jaringan." },
  { "en": "Jaringan Mana Yang Lebih Tahan?", "id": "Jaringan Bebas Skala Lebih Tahan." },
  { "en": "Apa Itu Sistem Self-Organized Criticality?", "id": "Sistem Kompleks Yang Berevolusi Ke Titik Kritis." },
  { "en": "Apa Itu Longsoran (Avalanche)?", "id": "Rangkaian Kegagalan Berantai Dalam Sistem." },
  { "en": "Apa Itu Kaskade Kegagalan?", "id": "Penyebaran Kegagalan Dalam Jaringan." },
  { "en": "Apa Itu Pemadaman Listrik Besar?", "id": "Seringkali Hasil Dari Kaskade Kegagalan." },
  { "en": "Apa Itu Sistem Proteksi Area Luas?", "id": "Skema Proteksi Jaringan Terkoordinasi." },
  { "en": "Apa Itu Pemisahan Pulau Terkendali?", "id": "Memisahkan Jaringan Secara Sengaja Saat Gangguan." },
  { "en": "Apa Itu Pencegahan?", "id": "Tindakan Untuk Mencegah Terjadinya Masalah." },
  { "en": "Apa Itu Koreksi?", "id": "Tindakan Untuk Memperbaiki Masalah Yang Terjadi." },
  { "en": "Apa Itu Keamanan Siber?", "id": "Perlindungan Sistem Komputer Dari Serangan Siber." },
  { "en": "Apa Itu Kerentanan?", "id": "Kelemahan Dalam Sistem Yang Dapat Dieksploitasi." },
  { "en": "Apa Itu Ancaman?", "id": "Sesuatu Yang Dapat Menyebabkan Kerugian." },
  { "en": "Apa Itu Risiko?", "id": "Potensi Kerugian Akibat Suatu Ancaman." },
  { "en": "Apa Itu Penilaian Risiko?", "id": "Proses Mengevaluasi Risiko Keamanan." },
  { "en": "Apa Itu Manajemen Risiko?", "id": "Mengelola Risiko Ke Tingkat Yang Dapat Diterima." },
  { "en": "Apa Itu Enkripsi?", "id": "Proses Mengamankan Data Dengan Kriptografi." },
  { "en": "Apa Itu Tanda Tangan Digital?", "id": "Memverifikasi Keaslian Dan Integritas Pesan." },
  { "en": "Apa Itu Infrastruktur Kunci Publik (PKI)?", "id": "Sistem Untuk Mengelola Kunci Digital." },
  { "en": "Apa Itu Firewall?", "id": "Sistem Keamanan Yang Membatasi Lalu Lintas Jaringan." },
  { "en": "Apa Itu Sistem Deteksi Intrusi (IDS)?", "id": "Sistem Yang Mendeteksi Aktivitas Jaringan Mencurigakan." },
  { "en": "Apa Itu Sistem Pencegahan Intrusi (IPS)?", "id": "Sistem Yang Mendeteksi Dan Memblokir Intrusi." },
  { "en": "Apa Itu Honeypot?", "id": "Sistem Umpan Untuk Menarik Dan Menganalisis." },
  { "en": "Apa Itu Keamanan Fisik?", "id": "Tindakan Melindungi Akses Fisik Ke Aset." },
  { "en": "Apa Itu Pengawasan Video?", "id": "Menggunakan Kamera Untuk Memantau Area." },
  { "en": "Apa Itu Kontrol Akses?", "id": "Sistem Membatasi Masuk Ke Area Terlarang." },
  { "en": "Apa Itu Biometrik?", "id": "Menggunakan Karakteristik Fisik Unik Untuk Otentikasi." },
  { "en": "Apa Itu Forensik Digital?", "id": "Proses Menginvestigasi Kejahatan Digital." },
  { "en": "Apa Itu Rantai Pengawasan?", "id": "Mendokumentasikan Penanganan Bukti Secara Kronologis." },
  { "en": "Apa Itu Pemulihan Data?", "id": "Proses Memulihkan Data Yang Hilang." },
  { "en": "Apa Itu Data Carving?", "id": "Memulihkan File Dari Data Mentah." },
  { "en": "Apa Itu Steganografi?", "id": "Menyembunyikan Data Di Dalam Data Lain." },
  { "en": "Apa Itu Kriptanalisis?", "id": "Ilmu Dan Seni Memecahkan Kode Kriptografi." },
  { "en": "Apa Itu Rekayasa Sosial?", "id": "Memanipulasi Orang Untuk Mendapatkan Informasi." },
  { "en": "Apa Itu Phishing?", "id": "Serangan Rekayasa Sosial Melalui Email." },
  { "en": "Apa Itu Malware?", "id": "Perangkat Lunak Berbahaya." },
  { "en": "Apa Itu Virus?", "id": "Malware Yang Menempel Pada Program Lain." },
  { "en": "Apa Itu Worm?", "id": "Malware Yang Dapat Menyebar Sendiri." },
  { "en": "Apa Itu Trojan?", "id": "Malware Yang Menyamar Sebagai Perangkat Lunak." },
  { "en": "Apa Itu Ransomware?", "id": "Malware Yang Mengenkripsi File Dan Meminta." },
  { "en": "Apa Itu Spyware?", "id": "Malware Yang Memata-matai Aktivitas Pengguna." },
  { "en": "Apa Itu Adware?", "id": "Malware Yang Menampilkan Iklan Tidak Diinginkan." },
  { "en": "Apa Itu Botnet?", "id": "Jaringan Komputer Zombie Yang Dikendalikan." },
  { "en": "Apa Itu Serangan Denial-of-Service (DoS)?", "id": "Membuat Layanan Tidak Tersedia Untuk Pengguna." },
  { "en": "Apa Itu Serangan Distributed DoS (DDoS)?", "id": "Serangan DoS Dari Banyak Sumber." },
  { "en": "Apa Itu Zero-Day Exploit?", "id": "Mengeksploitasi Kerentanan Yang Belum Diketahui." },
  { "en": "Apa Itu Patch Keamanan?", "id": "Pembaruan Perangkat Lunak Untuk Memperbaiki Kerentanan." },
  { "en": "Apa Itu Manajemen Patch?", "id": "Proses Menerapkan Patch Keamanan." },
  { "en": "Apa Itu Penilaian Kerentanan?", "id": "Proses Mengidentifikasi Kelemahan Keamanan." },
  { "en": "Apa Itu Pengujian Penetrasi?", "id": "Aktivitas Mensimulasikan Serangan Siber." },
  { "en": "Apa Itu Peretasan Etis?", "id": "Aktivitas Peretasan Dengan Izin." },
  { "en": "Apa Itu Tim Merah?", "id": "Tim Yang Berperan Sebagai Penyerang." },
  { "en": "Apa Itu Tim Biru?", "id": "Tim Yang Berperan Sebagai Pembela." },
  { "en": "Apa Itu Tim Ungu?", "id": "Tim Yang Menggabungkan Tim Merah Dan Biru." },
  { "en": "Apa Itu Respon Insiden?", "id": "Proses Menangani Pelanggaran Keamanan." },
  { "en": "Apa Itu Rencana Respon Insiden?", "id": "Prosedur Untuk Menangani Insiden Keamanan." },
  { "en": "Apa Itu Pemulihan Bencana?", "id": "Memulihkan Operasi Teknologi Informasi Setelah Bencana." },
  { "en": "Apa Itu Rencana Kelangsungan Bisnis?", "id": "Menjaga Fungsi Bisnis Tetap Berjalan." },
  { "en": "Apa Itu Cadangan Panas (Hot Backup)?", "id": "Sistem Cadangan Yang Selalu Beroperasi." },
  { "en": "Apa Itu Cadangan Dingin (Cold Backup)?", "id": "Sistem Cadangan Yang Harus Dinyalakan Manual." },
  { "en": "Apa Itu Perjanjian Tingkat Layanan (SLA)?", "id": "Kontrak Antara Penyedia Dan Pelanggan." },
  { "en": "Apa Itu Interupsi Momen (Momentary Interruption)?", "id": "Kehilangan Daya Kurang Dari Beberapa Detik." },
  { "en": "Apa Itu Indeks Kualitas Daya?", "id": "Metrik Tunggal Untuk Mengukur Kualitas Daya." },
  { "en": "Apa Itu Kompresi Data?", "id": "Mengurangi Ukuran Data Pengukuran Kualitas Daya." },
  { "en": "Apa Itu Agregasi Data?", "id": "Menggabungkan Data Dari Waktu Ke Waktu." },
  { "en": "Apa Itu Penandaan Peristiwa?", "id": "Menandai Peristiwa Kualitas Daya Dalam Data." },
  { "en": "Apa Itu Korelasi Peristiwa?", "id": "Menghubungkan Peristiwa Yang Terjadi Bersamaan." },
  { "en": "Apa Itu Kausalitas?", "id": "Hubungan Sebab Akibat Antara Peristiwa." },
  { "en": "Apa Itu Jaringan Listrik Kapal?", "id": "Sistem Tenaga Listrik Terisolasi Di Kapal." },
  { "en": "Apa Itu Jaringan Listrik Pesawat?", "id": "Sistem Tenaga Listrik Di Pesawat Terbang." },
  { "en": "Apa Itu Frekuensi 400 Hz?", "id": "Frekuensi Umum Dalam Sistem Tenaga Pesawat." },
  { "en": "Apa Itu Kereta Listrik?", "id": "Kereta Yang Digerakkan Oleh Tenaga Listrik." },
  { "en": "Apa Itu Catu Daya Tepi Jalan?", "id": "Menyediakan Daya Untuk Sistem Kereta Listrik." },
  { "en": "Apa Itu Pantograf?", "id": "Kontak Geser Untuk Mengambil Daya Listrik." },
  { "en": "Apa Itu Harmonik Yang Dihasilkan Kereta?", "id": "Dihasilkan Oleh Konverter Daya Traksi." },
  { "en": "Apa Itu Sistem Industri?", "id": "Pabrik Dan Fasilitas Manufaktur." },
  { "en": "Apa Itu Beban Tungku Busur?", "id": "Menyebabkan Flicker Dan Distorsi Tegangan Hebat." },
  { "en": "Apa Itu Beban Las?", "id": "Menyebabkan Sag Dan Fluktuasi Tegangan." },
  { "en": "Apa Itu Pusat Data?", "id": "Membutuhkan Kualitas Daya Sangat Andal." },
  { "en": "Apa Itu Sistem Catu Daya Ganda?", "id": "Dua Suplai Independen Untuk Redundansi." },
  { "en": "Apa Itu Saklar Transfer Statis (STS)?", "id": "Memindahkan Beban Antar Sumber Secara Cepat." },
  { "en": "Apa Itu Waktu Transfer?", "id": "Waktu Yang Dibutuhkan Untuk Beralih Sumber." },
  { "en": "Apa Itu Peralatan Medis?", "id": "Sangat Sensitif Terhadap Gangguan Kualitas Daya." },
  { "en": "Apa Itu Standar IEC 60601?", "id": "Standar Keamanan Internasional Untuk Peralatan Medis." },
  { "en": "Apa Itu Sistem Tenaga Terisolasi?", "id": "Digunakan Di Area Medis Kritis." },
  { "en": "Apa Itu Line Isolation Monitor (LIM)?", "id": "Memantau Integritas Sistem Tenaga Terisolasi." },
  { "en": "Apa Itu Sistem Komersial?", "id": "Gedung Perkantoran Dan Pusat Belanja." },
  { "en": "Apa Itu Beban Pencahayaan?", "id": "Lampu LED (Light-Emitting Diode) Dan Fluoresen." },
  { "en": "Apa Itu Beban HVAC (Heating, Ventilation, and Air Conditioning)?", "id": "Sistem Pemanasan, Ventilasi, Dan Pendingin Udara." },
  { "en": "Apa Itu Sistem Perumahan?", "id": "Rumah Tinggal Dan Apartemen." },
  { "en": "Apa Itu Peralatan Elektronik Konsumen?", "id": "Televisi, Komputer, Dan Perangkat Audio." },
  { "en": "Apa Itu Penyearah Jembatan?", "id": "Input Dari Banyak Peralatan Elektronik." },
  { "en": "Apa Itu Faktor Crest Arus?", "id": "Rasio Puncak Arus Terhadap Nilai RMS." },
  { "en": "Apa Itu Arus Netral?", "id": "Arus Di Konduktor Netral." },
  { "en": "Bagaimana Harmonik Mempengaruhi Arus Netral?", "id": "Harmonik Triplen Menjumlah Di Konduktor Netral." },
  { "en": "Apa Itu Netral Berukuran Lebih?", "id": "Konduktor Netral Lebih Besar Dari Konduktor Fasa." },
  { "en": "Apa Itu Ekonomi Kualitas Daya?", "id": "Analisis Biaya Masalah Dan Solusi Kualitas Daya." },
  { "en": "Apa Itu Biaya Peralatan Mitigasi?", "id": "Biaya Untuk Memasang Solusi Kualitas Daya." },
  { "en": "Apa Itu Biaya Downtime?", "id": "Kerugian Finansial Akibat Produksi Berhenti." },
  { "en": "Apa Itu Biaya Kerusakan Peralatan?", "id": "Biaya Untuk Memperbaiki Atau Mengganti Peralatan." },
  { "en": "Apa Itu Biaya Energi?", "id": "Biaya Listrik Yang Dikonsumsi." },
  { "en": "Bagaimana Kualitas Daya Mempengaruhi Biaya Energi?", "id": "Harmonik Dan Faktor Daya Buruk Meningkatkan." },
  { "en": "Apa Itu Penalti Faktor Daya?", "id": "Denda Dari Utilitas Akibat Faktor Daya." },
  { "en": "Apa Itu Analisis Manfaat-Biaya?", "id": "Membandingkan Biaya Dan Manfaat Suatu Proyek." },
  { "en": "Apa Itu Return on Investment (ROI)?", "id": "Ukuran Profitabilitas Suatu Investasi." },
  { "en": "Apa Itu Payback Period?", "id": "Waktu Yang Dibutuhkan Untuk Mengembalikan Investasi." },
  { "en": "Apa Itu Model Stokastik?", "id": "Model Yang Melibatkan Elemen Keacakan." },
  { "en": "Apa Itu Simulasi Monte Carlo?", "id": "Menggunakan Angka Acak Untuk Simulasi." },
  { "en": "Apa Itu Prediksi Kualitas Daya?", "id": "Memperkirakan Masalah Kualitas Daya Di Masa Depan." },
  { "en": "Apa Itu Perambatan Sag?", "id": "Bagaimana Sag Menyebar Melalui Jaringan." },
  { "en": "Apa Itu Area Kerentanan?", "id": "Wilayah Yang Terkena Dampak Suatu Gangguan." },
  { "en": "Apa Itu Sistem Otomasi Distribusi?", "id": "Mengotomatiskan Operasi Jaringan Distribusi." },
  { "en": "Apa Itu Self-Healing Grid?", "id": "Jaringan Yang Dapat Memulihkan Diri Otomatis." },
  { "en": "Apa Itu Fault Location, Isolation, and Service Restoration (FLISR)?", "id": "Fungsi Kunci Dari Self-Healing Grid." },
  { "en": "Apa Itu Recloser Cerdas?", "id": "Recloser Dengan Kemampuan Komunikasi Dan Kontrol." },
  { "en": "Apa Itu Saklar Cerdas?", "id": "Saklar Dengan Kontrol Jarak Jauh." },
  { "en": "Apa Itu Sensor Cerdas?", "id": "Sensor Dengan Kemampuan Pemrosesan Dan Komunikasi." },
  { "en": "Apa Itu Jaringan Sensor?", "id": "Kumpulan Sensor Yang Terhubung Untuk Pemantauan." },
  { "en": "Apa Itu Analisis Data?", "id": "Memeriksa Data Untuk Menarik Kesimpulan." },
  { "en": "Apa Itu Visualisasi Data?", "id": "Menampilkan Data Secara Grafis Untuk Pemahaman." },
  { "en": "Apa Itu Dasbor?", "id": "Tampilan Visual Informasi Kunci." },
  { "en": "Apa Itu Sistem Informasi Geografis (GIS)?", "id": "Memetakan Data Kualitas Daya Secara Geografis." },
  { "en": "Apa Itu Gelombang Berjalan?", "id": "Pulsa Yang Merambat Di Sepanjang Saluran." },
  { "en": "Apa Itu Lokasi Gangguan Gelombang Berjalan?", "id": "Menggunakan Waktu Tiba Gelombang Untuk Lokasi." },
  { "en": "Apa Itu Osilasi Frekuensi Tinggi?", "id": "Nama Lain Untuk Transien Osilatoris." },
  { "en": "Apa Itu Resonansi Inti Transformator?", "id": "Osilasi Antara Induktansi Saturable Dan Kapasitansi." },
  { "en": "Apa Itu Impedansi Karakteristik?", "id": "Impedansi Alami Dari Jalur Transmisi." },
  { "en": "Apa Itu Refleksi?", "id": "Gelombang Memantul Kembali Dari Ketidakcocokan." },
  { "en": "Apa Itu Transmisi?", "id": "Gelombang Melewati Batas Antar Media." },
  { "en": "Apa Itu Koefisien Refleksi?", "id": "Rasio Amplitudo Gelombang Pantul Terhadap Datang." },
  { "en": "Apa Itu Koefisien Transmisi?", "id": "Rasio Amplitudo Gelombang Transmisi Terhadap Datang." },
  { "en": "Apa Itu Diagram Tangga (Lattice Diagram)?", "id": "Alat Grafis Menganalisis Refleksi Gelombang." },
  { "en": "Apa Itu Petir?", "id": "Sumber Alami Transien Elektromagnetik." },
  { "en": "Apa Itu Sambaran Langsung?", "id": "Petir Menyambar Langsung Ke Fasilitas." },
  { "en": "Apa Itu Sambaran Tidak Langsung?", "id": "Petir Menyambar Dekat Fasilitas." },
  { "en": "Apa Itu Gelombang Petir?", "id": "Bentuk Gelombang Standar Transien Petir." },
  { "en": "Apa Itu Waktu Muka (Front Time)?", "id": "Waktu Naik Dari Gelombang Petir." },
  { "en": "Apa Itu Waktu Ekor (Tail Time)?", "id": "Waktu Turun Gelombang Petir." },
  { "en": "Apa Itu Proteksi Petir?", "id": "Sistem Untuk Melindungi Dari Sambaran Petir." },
  { "en": "Apa Itu Kawat Tanah (Shield Wire)?", "id": "Menangkap Sambaran Petir Di Saluran Transmisi." },
  { "en": "Apa Itu Arrester Surja?", "id": "Nama Lain Untuk Surge Arrester." },
  { "en": "Apa Itu Tegangan Sisa?", "id": "Tegangan Di Arrester Saat Menghantar." },
  { "en": "Apa Itu Energi Surja?", "id": "Energi Yang Diserap Oleh Arrester." },
  { "en": "Apa Itu Switching Elektromagnetik?", "id": "Transien Akibat Operasi Pemutusan." },
  { "en": "Apa Itu Restrike?", "id": "Busur Api Muncul Kembali Setelah Pemutusan." },
  { "en": "Apa Itu Current Chopping?", "id": "Pemutusan Arus Secara Tiba-tiba." },
  { "en": "Apa Itu Tegangan Pemulihan Transien (TRV)?", "id": "Tegangan Di Kontak Pemutus Sirkuit." },
  { "en": "Apa Itu Capacitor Switching?", "id": "Menghasilkan Transien Osilatoris Tegangan Tinggi." },
  { "en": "Apa Itu Magnifikasi Tegangan?", "id": "Peningkatan Tegangan Akibat Resonansi." },
  { "en": "Apa Itu Inrush Arus?", "id": "Nama Lain Untuk Inrush Current." },
  { "en": "Apa Itu Ferroresonansi?", "id": "Osilasi Non-linier Kompleks." },
  { "en": "Apa Itu Mode Fundamental?", "id": "Mode Osilasi Dasar Ferroresonansi." },
  { "en": "Apa Itu Mode Subharmonik?", "id": "Osilasi Di Bawah Frekuensi Fundamental." },
  { "en": "Apa Itu Mode Kuasi-Periodik?", "id": "Osilasi Dengan Beberapa Komponen Frekuensi." },
  { "en": "Apa Itu Mode Kacau (Chaotic)?", "id": "Osilasi Tidak Teratur Dan Tak Terduga." },
  { "en": "Apa Itu Inisiasi Ferroresonansi?", "id": "Peristiwa Yang Memulai Osilasi Ferroresonansi." },
  { "en": "Apa Itu Damping Ferroresonansi?", "id": "Proses Mengurangi Osilasi Ferroresonansi." },
  { "en": "Bagaimana Cara Meredam Ferroresonansi?", "id": "Menambahkan Beban Resistif Sementara." },
  { "en": "Apa Itu Model Kualitas Daya?", "id": "Representasi Matematis Dari Gangguan Daya." },
  { "en": "Apa Itu Model Stokastik?", "id": "Model Yang Melibatkan Elemen Probabilitas." },
  { "en": "Apa Itu Model Deterministik?", "id": "Model Tanpa Elemen Acak Sama Sekali." },
  { "en": "Apa Itu Estimasi Parameter?", "id": "Proses Menentukan Nilai Parameter Model." },
  { "en": "Apa Itu Metode Momen?", "id": "Metode Statistik Untuk Estimasi Parameter." },
  { "en": "Apa Itu Estimasi Kemungkinan Maksimum?", "id": "Metode Statistik Populer Untuk Estimasi." },
  { "en": "Apa Itu Filter Kalman?", "id": "Estimator Optimal Untuk Sistem Linier Dinamis." },
  { "en": "Apa Itu Jaringan Saraf Tiruan?", "id": "Digunakan Untuk Klasifikasi Peristiwa Kualitas Daya." },
  { "en": "Apa Itu Pohon Keputusan?", "id": "Model Klasifikasi Berbentuk Struktur Pohon." },
  { "en": "Apa Itu Support Vector Machine (SVM)?", "id": "Model Klasifikasi Lainnya Yang Populer." },
  { "en": "Apa Itu Komputasi Evolusioner?", "id": "Algoritma Optimisasi Berbasis Prinsip Evolusi." },
  { "en": "Apa Itu Logika Fuzzy?", "id": "Menangani Ketidakpastian Dalam Sistem Kontrol." },
  { "en": "Apa Itu Indeks Kualitas Daya?", "id": "Angka Tunggal Untuk Merepresentasikan Kualitas Daya." },
  { "en": "Apa Itu Agregasi?", "id": "Proses Menggabungkan Beberapa Ukuran Menjadi Satu." },
  { "en": "Apa Itu Pembobotan?", "id": "Memberi Bobot Pada Ukuran Berdasarkan Pentingnya." },
  { "en": "Apa Itu Normalisasi?", "id": "Menskalakan Data Ke Rentang Standar." },
  { "en": "Apa Itu Kualitas Suplai?", "id": "Kualitas Daya Dari Sisi Jaringan Utilitas." },
  { "en": "Apa Itu Kualitas Permintaan?", "id": "Pengaruh Beban Terhadap Kualitas Daya." },
  { "en": "Apa Itu Interaksi Beban-Sistem?", "id": "Bagaimana Beban Dan Sistem Saling Mempengaruhi." },
  { "en": "Apa Itu Kekakuan Jaringan?", "id": "Kemampuan Jaringan Menahan Gangguan Tegangan." },
  { "en": "Apa Itu Arus Hubung Singkat?", "id": "Indikator Kekakuan Suatu Jaringan Listrik." },
  { "en": "Apa Itu Sistem Tenaga Radial?", "id": "Aliran Daya Mengalir Dalam Satu Arah." },
  { "en": "Apa Itu Sistem Tenaga Jala?", "id": "Aliran Daya Dapat Mengalir Multi-arah." },
  { "en": "Apa Itu Pembangkit Terdistribusi?", "id": "Pembangkit Listrik Skala Kecil Di Jaringan." },
  { "en": "Apa Itu Efek Pulau?", "id": "Pembangkit Terdistribusi Tetap Beroperasi." },
  { "en": "Apa Itu Deteksi Anti-Islanding?", "id": "Metode Untuk Mendeteksi Efek Pulau." },
  { "en": "Apa Itu Metode Pasif?", "id": "Memantau Parameter Jaringan Untuk Deteksi." },
  { "en": "Apa Itu Metode Aktif?", "id": "Menyuntikkan Sinyal Kecil Untuk Deteksi." },
  { "en": "Apa Itu Pergeseran Frekuensi?", "id": "Metode Deteksi Anti-Islanding Aktif." },
  { "en": "Apa Itu Pergeseran Impedansi?", "id": "Metode Deteksi Aktif Lainnya." },
  { "en": "Apa Itu Inverter Siap Jaringan?", "id": "Inverter Dengan Fitur Pendukung Jaringan." },
  { "en": "Apa Itu Standar IEEE (Institute of Electrical and Electronics Engineers) 1547?", "id": "Standar Interkoneksi Sumber Daya Terdistribusi." },
  { "en": "Apa Itu Ride-Through?", "id": "Kemampuan Tetap Terhubung Saat Gangguan." },
  { "en": "Apa Itu Kontrol Volt-VAR?", "id": "Mengatur Tegangan Dengan Mengontrol Daya Reaktif." },
  { "en": "Apa Itu Kontrol Frekuensi-Watt?", "id": "Mengatur Daya Aktif Untuk Menstabilkan Frekuensi." },
  { "en": "Apa Itu Soft Start?", "id": "Fitur Inverter Untuk Mengurangi Inrush Current." },
  { "en": "Apa Itu Kendaraan Listrik?", "id": "Beban Dan Sumber Daya Potensial." },
  { "en": "Apa Itu Pengisian Cerdas?", "id": "Mengontrol Proses Pengisian Kendaraan Listrik." },
  { "en": "Apa Itu Vehicle-to-Grid (V2G)?", "id": "Kendaraan Listrik Memberi Daya Ke Jaringan." },
  { "en": "Apa Itu Baterai?", "id": "Perangkat Penyimpanan Energi Kimia." },
  { "en": "Apa Itu Sistem Manajemen Baterai (BMS)?", "id": "Sistem Elektronik Mengelola Dan Melindungi Baterai." },
  { "en": "Apa Itu State of Charge (SoC)?", "id": "Tingkat Pengisian Baterai Saat Ini." },
  { "en": "Apa Itu State of Health (SoH)?", "id": "Ukuran Kondisi Kesehatan Baterai." },
  { "en": "Apa Itu Siklus Hidup?", "id": "Jumlah Siklus Pengisian-Pengosongan Baterai." },
  { "en": "Apa Itu Penyimpanan Energi Skala Grid?", "id": "Menyimpan Energi Dalam Jumlah Sangat Besar." },
  { "en": "Apa Itu Arbitrase Energi?", "id": "Membeli Murah Dan Menjual Mahal Energi." },
  { "en": "Apa Itu Regulasi Frekuensi?", "id": "Membantu Menstabilkan Frekuensi Jaringan Listrik." },
  { "en": "Apa Itu Cadangan Berputar?", "id": "Kapasitas Cadangan Online Siap Pakai." },
  { "en": "Apa Itu Black Start?", "id": "Kemampuan Memulai Kembali Jaringan Tanpa Daya." },
  { "en": "Apa Itu Pencahayaan LED (Light-Emitting Diode)?", "id": "Sumber Umum Arus Harmonik." },
  { "en": "Apa Itu Driver LED?", "id": "Catu Daya Khusus Untuk Lampu LED." },
  { "en": "Apa Itu Inrush Current LED?", "id": "Arus Puncak Saat Menyalakan Lampu LED." },
  { "en": "Apa Itu Peredupan (Dimming)?", "id": "Proses Mengatur Kecerahan Cahaya." },
  { "en": "Bagaimana Peredupan Mempengaruhi Harmonik?", "id": "Dapat Meningkatkan Distorsi Arus Harmonik." },
  { "en": "Apa Itu Flicker Lampu LED?", "id": "Fluktuasi Kecerahan Yang Tidak Diinginkan." },
  { "en": "Apa Itu Sistem HVAC (Heating, Ventilation, and Air Conditioning)?", "id": "Beban Besar Di Gedung Komersial." },
  { "en": "Apa Itu Kompresor Kecepatan Variabel?", "id": "Menggunakan VFD (Variable Frequency Drive) Untuk Kontrol." },
  { "en": "Apa Itu Pusat Data?", "id": "Beban Kritis Dengan Banyak Catu Daya." },
  { "en": "Apa Itu Redundansi Catu Daya?", "id": "Memiliki Catu Daya Cadangan Untuk Keandalan." },
  { "en": "Apa Itu Power Distribution Unit (PDU)?", "id": "Perangkat Mendistribusikan Daya Di Dalam Rak." },
  { "en": "Apa Itu Static Transfer Switch (STS)?", "id": "Beralih Antar Sumber Daya Secara Cepat." },
  { "en": "Apa Itu Saklar Transfer Otomatis (ATS)?", "id": "Saklar Mekanis Untuk Sumber Cadangan." },
  { "en": "Apa Itu Generator Cadangan?", "id": "Menyediakan Daya Selama Pemadaman Listrik." },
  { "en": "Apa Itu Waktu Peralihan?", "id": "Waktu Yang Dibutuhkan Untuk Beralih Sumber." },
  { "en": "Apa Itu Pengukuran Kualitas Daya?", "id": "Proses Mengukur Parameter Kualitas Daya." },
  { "en": "Apa Itu Akurasi Instrumen?", "id": "Seberapa Dekat Pengukuran Dengan Nilai Benar." },
  { "en": "Apa Itu Presisi Instrumen?", "id": "Seberapa Konsisten Hasil Pengukuran." },
  { "en": "Apa Itu Resolusi?", "id": "Perubahan Terkecil Yang Dapat Diukur." },
  { "en": "Apa Itu Bandwidth?", "id": "Rentang Frekuensi Yang Dapat Diukur." },
  { "en": "Apa Itu Laju Sampling?", "id": "Seberapa Sering Sinyal Diukur Per Detik." },
  { "en": "Apa Itu Kuantisasi?", "id": "Mengubah Nilai Analog Menjadi Nilai Digital." },
  { "en": "Apa Itu Bit Resolusi?", "id": "Jumlah Bit Yang Digunakan Dalam Kuantisasi." },
  { "en": "Apa Itu Standar IEC (International Electrotechnical Commission) 61000-4-30?", "id": "Standar Internasional Untuk Metode Pengukuran." },
  { "en": "Apa Itu Kelas A?", "id": "Kelas Akurasi Tertinggi Dalam Standar." },
  { "en": "Apa Itu Kelas S?", "id": "Kelas Akurasi Untuk Survei Kualitas Daya." },
  { "en": "Apa Itu Agregasi Waktu?", "id": "Menggabungkan Data Selama Interval Waktu Tertentu." },
  { "en": "Apa Itu Interval 10-Menit?", "id": "Interval Umum Untuk Agregasi Data." },
  { "en": "Apa Itu Penandaan (Flagging)?", "id": "Menandai Data Selama Terjadi Peristiwa." },
  { "en": "Apa Itu Sinkronisasi Waktu?", "id": "Memastikan Semua Instrumen Memiliki Waktu Sama." },
  { "en": "Apa Itu Global Positioning System (GPS)?", "id": "Sering Digunakan Untuk Sinkronisasi Waktu." },
  { "en": "Apa Itu Probabilitas?", "id": "Ukuran Kemungkinan Suatu Peristiwa." },
  { "en": "Apa Itu Statistik?", "id": "Ilmu Mengumpulkan Dan Menganalisis Data." },
  { "en": "Apa Itu Rata-rata?", "id": "Ukuran Tendensi Sentral Data." },
  { "en": "Apa Itu Standar Deviasi?", "id": "Ukuran Sebaran Data." },
  { "en": "Apa Itu Distribusi Probabilitas?", "id": "Fungsi Yang Menjelaskan Probabilitas Nilai." },
  { "en": "Apa Itu Distribusi Normal?", "id": "Distribusi Probabilitas Berbentuk Lonceng." },
  { "en": "Apa Itu Distribusi Weibull?", "id": "Digunakan Dalam Analisis Keandalan Dan Angin." },
  { "en": "Apa Itu Indeks Statistik?", "id": "Seperti Nilai 95-persentil." },
  { "en": "Apa Itu Analisis Tren?", "id": "Mencari Pola Dalam Data Seiring Waktu." },
  { "en": "Apa Itu Analisis Korelasi?", "id": "Mencari Hubungan Antara Dua Variabel." },
  { "en": "Apa Itu Pelaporan Kualitas Daya?", "id": "Proses Menyajikan Hasil Analisis." },
  { "en": "Apa Itu Laporan Eksekutif?", "id": "Ringkasan Tingkat Tinggi Untuk Manajemen." },
  { "en": "Apa Itu Laporan Teknis?", "id": "Laporan Rinci Untuk Insinyur Teknik." },
  { "en": "Apa Itu Visualisasi?", "id": "Penyajian Data Secara Grafis." },
  { "en": "Apa Itu Kode Warna?", "id": "Menggunakan Warna Untuk Menunjukkan Tingkat Kepatuhan." },
  { "en": "Apa Itu Peta Kontur?", "id": "Visualisasi Data Geografis Kualitas Daya." },
  { "en": "Apa Itu Kompatibilitas Peralatan?", "id": "Kemampuan Peralatan Beroperasi Di Lingkungannya." },
  { "en": "Apa Itu Toleransi Peralatan?", "id": "Tingkat Gangguan Yang Dapat Ditoleransi." },
  { "en": "Apa Itu Kurva Toleransi?", "id": "Grafik Magnitudo Gangguan Terhadap Durasi." },
  { "en": "Apa Itu Kurva CBEMA (Computer and Business Equipment Manufacturers Association)?", "id": "Kurva Toleransi Tegangan Awal Untuk Komputer." },
  { "en": "Apa Itu Kurva ITIC (Information Technology Industry Council)?", "id": "Kurva Toleransi Tegangan Modern Untuk IT." },
  { "en": "Apa Itu Kurva SEMI F47?", "id": "Standar Toleransi Sag Untuk Industri Semikonduktor." },
  { "en": "Apa Itu Desain Untuk Kekebalan?", "id": "Merancang Peralatan Agar Tahan Terhadap Gangguan." },
  { "en": "Apa Itu Catu Daya Tahan Sag?", "id": "Catu Daya Yang Dapat Melewati Sag." },
  { "en": "Apa Itu Energi Tersimpan?", "id": "Energi Di Kapasitor Internal Catu Daya." },
  { "en": "Apa Itu Kontrol Umpan Maju?", "id": "Mengantisipasi Gangguan Sebelum Mempengaruhi Output." },
  { "en": "Apa Itu Sistem Kontrol Digital?", "id": "Menggunakan Prosesor Digital Untuk Loop Kontrol." },
  { "en": "Apa Itu Digital Signal Processor (DSP)?", "id": "Prosesor Khusus Untuk Pemrosesan Sinyal Cepat." },
  { "en": "Apa Itu Field-Programmable Gate Array (FPGA)?", "id": "Sirkuit Terpadu Yang Dapat Diprogram Ulang." },
  { "en": "Apa Itu Kontrol Real-Time?", "id": "Sistem Kontrol Dengan Batas Waktu Ketat." },
  { "en": "Apa Itu Perangkat Lunak Tertanam?", "id": "Perangkat Lunak Di Dalam Perangkat Keras." },
  { "en": "Apa Itu Simulasi Real-Time?", "id": "Simulasi Yang Berjalan Secepat Waktu Nyata." },
  { "en": "Apa Itu Hardware-in-the-Loop (HIL)?", "id": "Simulasi Yang Melibatkan Perangkat Keras Sebenarnya." },
  { "en": "Apa Itu Topologi Jaringan Listrik?", "id": "Struktur Fisik Dan Koneksi Jaringan." },
  { "en": "Apa Itu Jaringan Transmisi?", "id": "Menyalurkan Daya Jarak Jauh Tegangan Tinggi." },
  { "en": "Apa Itu Jaringan Distribusi?", "id": "Menyalurkan Daya Ke Pelanggan Akhir." },
  { "en": "Apa Itu Jaringan Tegangan Rendah?", "id": "Jaringan Distribusi Akhir Ke Konsumen." },
  { "en": "Apa Itu Jaringan Tegangan Menengah?", "id": "Antara Transmisi Dan Distribusi Tegangan Rendah." },
  { "en": "Apa Itu Jaringan Tegangan Tinggi?", "id": "Jaringan Transmisi Utama." },
  { "en": "Apa Itu Jaringan Tegangan Ekstra Tinggi?", "id": "Jaringan Transmisi Jarak Sangat Jauh." },
  { "en": "Apa Itu Jaringan Tegangan Ultra Tinggi?", "id": "Jaringan Transmisi Paling Tinggi Saat Ini." },
  { "en": "Apa Itu Arus Searah Tegangan Tinggi (HVDC)?", "id": "Transmisi Daya Listrik Menggunakan DC." },
  { "en": "Apa Keuntungan HVDC (High-Voltage Direct Current)?", "id": "Kerugian Rendah Dan Kontrol Aliran Daya." },
  { "en": "Apa Itu Stasiun Konverter?", "id": "Fasilitas Mengubah Antara AC Dan DC." },
  { "en": "Apa Itu Line-Commutated Converter (LCC)?", "id": "Konverter HVDC (High-Voltage Direct Current) Klasik." },
  { "en": "Apa Itu Voltage-Sourced Converter (VSC)?", "id": "Konverter HVDC (High-Voltage Direct Current) Modern." },
  { "en": "Apa Itu Jaringan HVDC Multi-Terminal?", "id": "Menghubungkan Lebih Dari Dua Titik Konverter." },
  { "en": "Apa Itu Flexible AC Transmission System (FACTS)?", "id": "Sistem Berbasis Elektronika Daya Untuk Jaringan AC." },
  { "en": "Apa Itu Kompensasi Seri?", "id": "Memasang Kapasitor Seri Di Jalur Transmisi." },
  { "en": "Apa Itu Kompensasi Shunt?", "id": "Memasang Komponen Paralel Dengan Jalur." },
  { "en": "Apa Itu Thyristor-Controlled Series Capacitor (TCSC)?", "id": "Kapasitor Seri Yang Nilainya Dapat Diatur." },
  { "en": "Apa Itu Static Synchronous Series Compensator (SSSC)?", "id": "Kompensator Seri Berbasis VSC (Voltage-Sourced Converter)." },
  { "en": "Apa Itu Interline Power Flow Controller (IPFC)?", "id": "Mengontrol Aliran Daya Antar Beberapa Jalur." },
  { "en": "Apa Itu Analisis Harmonik?", "id": "Proses Mempelajari Penyebaran Harmonik." },
  { "en": "Apa Itu Spektrum Harmonik?", "id": "Plot Amplitudo Harmonik Terhadap Frekuensi." },
  { "en": "Apa Itu Orde Harmonik?", "id": "Kelipatan Bilangan Bulat Dari Frekuensi Fundamental." },
  { "en": "Apa Itu Fasa Harmonik?", "id": "Sudut Fasa Relatif Dari Komponen Harmonik." },
  { "en": "Apa Itu Daya Harmonik?", "id": "Daya Akibat Interaksi Tegangan Dan Arus Harmonik." },
  { "en": "Apa Itu Transformator Zig-Zag?", "id": "Transformator Khusus Untuk Menekan Harmonik." },
  { "en": "Apa Itu Grounding?", "id": "Koneksi Ke Massa Bumi." },
  { "en": "Apa Itu Sistem Pembumian Solid?", "id": "Netral Terhubung Langsung Ke Ground." },
  { "en": "Apa Itu Sistem Pembumian Resistansi?", "id": "Netral Terhubung Melalui Resistor." },
  { "en": "Apa Itu Sistem Pembumian Reaktansi?", "id": "Netral Terhubung Melalui Reaktor." },
  { "en": "Apa Itu Sistem Tidak Dibumikan?", "id": "Netral Tidak Terhubung Ke Ground." },
  { "en": "Apa Itu Pembumian Frekuensi Tinggi?", "id": "Penting Untuk Proteksi EMI (Electromagnetic Interference)." },
  { "en": "Apa Itu Kualitas Daya Di Industri?", "id": "Tantangan Utama Akibat Beban Motor Besar." },
  { "en": "Apa Itu Adjustable Speed Drive (ASD)?", "id": "Nama Lain Untuk VFD (Variable Frequency Drive)." },
  { "en": "Apa Itu Pemanasan Induksi?", "id": "Beban Non-linier Yang Menghasilkan Harmonik." },
  { "en": "Apa Itu Pengelasan Busur?", "id": "Beban Fluktuatif Penyebab Flicker." },
  { "en": "Apa Itu Sistem Otomasi?", "id": "Sistem Kontrol Otomatis Dalam Industri." },
  { "en": "Apa Itu Programmable Logic Controller (PLC)?", "id": "Komputer Digital Untuk Kontrol Industri." },
  { "en": "Bagaimana PLC (Programmable Logic Controller) Dipengaruhi Kualitas Daya?", "id": "Sag Dapat Menyebabkan Reset Atau Malfungsi." },
  { "en": "Apa Itu Robot Industri?", "id": "Beban Dinamis Dengan Catu Daya Switching." },
  { "en": "Apa Itu Kualitas Daya Di Gedung Komersial?", "id": "Didominasi Oleh Beban Elektronik Dan Pencahayaan." },
  { "en": "Apa Itu Pencahayaan Fluoresen?", "id": "Sumber Umum Arus Harmonik." },
  { "en": "Apa Itu Pencahayaan LED?", "id": "Sumber Harmonik Lainnya." },
  { "en": "Apa Itu Peralatan Kantor?", "id": "Komputer, Printer, Dan Mesin Fotokopi." },
  { "en": "Apa Itu Sistem HVAC?", "id": "Beban Motor Besar Di Gedung." },
  { "en": "Apa Itu Lift?", "id": "Beban Dinamis Yang Menyebabkan Sag." },
  { "en": "Apa Itu Kualitas Daya Di Rumah Sakit?", "id": "Kualitas Daya Sangat Kritis Untuk Keselamatan." },
  { "en": "Apa Itu Sistem Tenaga Esensial?", "id": "Sistem Cadangan Untuk Beban Kritis Medis." },
  { "en": "Apa Itu Peralatan Pencitraan Medis?", "id": "Sangat Sensitif Terhadap Variasi Tegangan." },
  { "en": "Apa Itu Peralatan Pendukung Kehidupan?", "id": "Membutuhkan Suplai Daya Sangat Andal." },
  { "en": "Apa Itu Kualitas Daya Di Pertanian?", "id": "Tantangan Akibat Jaringan Pedesaan Yang Lemah." },
  { "en": "Apa Itu Beban Irigasi?", "id": "Beban Motor Besar Untuk Pompa Air." },
  { "en": "Apa Itu Stray Voltage?", "id": "Tegangan Kecil Yang Dapat Mempengaruhi Hewan." },
  { "en": "Apa Itu Pembangkit Listrik?", "id": "Sumber Utama Energi Listrik." },
  { "en": "Apa Itu Sistem Eksitasi?", "id": "Menyediakan Medan Magnet Untuk Generator." },
  { "en": "Apa Itu Automatic Voltage Regulator (AVR)?", "id": "Mengatur Tegangan Output Generator." },
  { "en": "Apa Itu Osilasi Torsional?", "id": "Osilasi Mekanis Pada Poros Generator-Turbin." },
  { "en": "Apa Itu Interaksi Sub-Sinkron?", "id": "Osilasi Antara Jaringan Listrik Dan Mekanis." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Air?", "id": "Menggunakan Energi Potensial Air." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Termal?", "id": "Menggunakan Panas Untuk Menghasilkan Uap." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Nuklir?", "id": "Menggunakan Energi Dari Reaksi Nuklir." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Gas?", "id": "Menggunakan Turbin Gas." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Diesel?", "id": "Menggunakan Mesin Diesel." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Surya Fotovoltaik?", "id": "Menggunakan Panel Surya." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Angin?", "id": "Menggunakan Turbin Angin." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Geotermal?", "id": "Menggunakan Panas Dari Dalam Bumi." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Biomassa?", "id": "Membakar Bahan Organik Untuk Energi." },
  { "en": "Apa Itu Kualitas Daya Dan Keandalan?", "id": "Dua Konsep Yang Terkait Erat." },
  { "en": "Apakah Interupsi Masalah Kualitas Daya?", "id": "Dianggap Sebagai Masalah Kualitas Dan Keandalan." },
  { "en": "Apa Itu Penilaian Risiko?", "id": "Mengevaluasi Risiko Terkait Gangguan Kualitas Daya." },
  { "en": "Apa Itu Probabilitas Kegagalan?", "id": "Kemungkinan Suatu Komponen Akan Gagal." },
  { "en": "Apa Itu Analisis Dampak?", "id": "Mengevaluasi Konsekuensi Dari Suatu Kegagalan." },
  { "en": "Apa Itu Pohon Kejadian?", "id": "Diagram Untuk Menganalisis Urutan Peristiwa." },
  { "en": "Apa Itu Pohon Kesalahan?", "id": "Diagram Untuk Menganalisis Penyebab Kegagalan." },
  { "en": "Apa Itu Pemeliharaan Berbasis Kondisi?", "id": "Pemeliharaan Berdasarkan Kondisi Aktual Peralatan." },
  { "en": "Apa Itu Pemantauan Kondisi?", "id": "Proses Memantau Parameter Peralatan." },
  { "en": "Apa Itu Termografi?", "id": "Teknik Pencitraan Menggunakan Radiasi Inframerah." },
  { "en": "Apa Itu Analisis Getaran?", "id": "Menganalisis Pola Getaran Mesin." },
  { "en": "Apa Itu Analisis Oli?", "id": "Menganalisis Oli Pelumas Untuk Tanda-tanda." },
  { "en": "Apa Itu Pengujian Pelepasan Sebagian?", "id": "Mendeteksi Pelepasan Sebagian Dalam Isolasi." },
  { "en": "Apa Itu Regulasi Kualitas Daya?", "id": "Aturan Yang Ditetapkan Oleh Regulator." },
  { "en": "Apa Itu Komisi Regulasi Energi?", "id": "Badan Pemerintah Yang Mengatur Industri Energi." },
  { "en": "Apa Itu Tarif Listrik?", "id": "Harga Yang Dibayar Konsumen Untuk Listrik." },
  { "en": "Apa Itu Tarif Berdasarkan Waktu?", "id": "Harga Listrik Bervariasi Sepanjang Hari." },
  { "en": "Apa Itu Harga Real-Time?", "id": "Harga Listrik Berubah Terus-menerus." },
  { "en": "Apa Itu Respon Permintaan?", "id": "Konsumen Mengubah Penggunaan Listrik Merespons Harga." },
  { "en": "Apa Itu Pengurangan Beban?", "id": "Mengurangi Konsumsi Listrik Selama Waktu Puncak." },
  { "en": "Apa Itu Pembangkit Cadangan?", "id": "Generator Untuk Menyediakan Daya Darurat." },
  { "en": "Apa Itu Saklar Transfer Otomatis (ATS)?", "id": "Beralih Antara Sumber Daya Jaringan Dan Cadangan." },
  { "en": "Apa Itu Operasi Pulau?", "id": "Sistem Berjalan Terisolasi Dari Jaringan Utama." },
  { "en": "Apa Itu Resinkronisasi?", "id": "Proses Menghubungkan Kembali Ke Jaringan Utama." },
  { "en": "Apa Itu Analisis Gelombang Harmonik?", "id": "Menganalisis Komponen Harmonik Dalam Bentuk Gelombang." },
  { "en": "Apa Itu Transformasi Fourier Cepat?", "id": "Algoritma Efisien Untuk Analisis Spektrum." },
  { "en": "Apa Itu Jendela Waktu?", "id": "Durasi Sinyal Yang Dianalisis Dalam FFT." },
  { "en": "Apa Itu Kebocoran Spektral?", "id": "Penyebaran Energi Dalam Spektrum Akibat Jendela." },
  { "en": "Apa Itu Fungsi Jendela?", "id": "Fungsi Matematis Untuk Mengurangi Kebocoran Spektral." },
  { "en": "Apa Itu Picket-Fence Effect?", "id": "Kesalahan Akibat Resolusi Frekuensi Terbatas." },
  { "en": "Apa Itu Transformasi Wavelet?", "id": "Metode Analisis Waktu-Frekuensi." },
  { "en": "Apa Keuntungan Transformasi Wavelet?", "id": "Sangat Baik Untuk Menganalisis Sinyal Transien." },
  { "en": "Apa Itu Estimasi Frekuensi?", "id": "Proses Menentukan Frekuensi Sinyal." },
  { "en": "Apa Itu Deteksi Zero-Crossing?", "id": "Metode Sederhana Untuk Mengukur Frekuensi." },
  { "en": "Apa Itu Phase-Locked Loop (PLL)?", "id": "Sirkuit Elektronik Untuk Melacak Frekuensi Dan Fasa." },
  { "en": "Apa Itu Estimasi Fasor?", "id": "Menentukan Magnitudo Dan Fasa Sinyal Sinusoidal." },
  { "en": "Apa Itu Standar IEEE (Institute of Electrical and Electronics Engineers) C37.118?", "id": "Standar Internasional Untuk Pengukuran Sinkrofasor." },
  { "en": "Apa Itu Pengukuran RMS?", "id": "Proses Menghitung Nilai RMS (Root Mean Square) Sinyal." },
  { "en": "Apa Itu Sliding Window?", "id": "Teknik Untuk Perhitungan RMS Secara Berkelanjutan." },
  { "en": "Apa Itu Deteksi Peristiwa?", "id": "Proses Mengidentifikasi Gangguan Kualitas Daya." },
  { "en": "Apa Itu Pemicu (Trigger)?", "id": "Kondisi Yang Ditetapkan Untuk Memulai Perekaman." },
  { "en": "Apa Itu Perekaman Bentuk Gelombang?", "id": "Proses Menangkap Bentuk Gelombang Aktual." },
  { "en": "Apa Itu Kompresi Data?", "id": "Mengurangi Ukuran Data Kualitas Daya." },
  { "en": "Apa Itu Representasi Fitur?", "id": "Mengekstrak Fitur Penting Dari Bentuk Gelombang." },
  { "en": "Apa Itu Tegangan Seimbang?", "id": "Tegangan Tiga Fasa Memiliki Magnitudo Sama." },
  { "en": "Apa Itu Arus Seimbang?", "id": "Arus Tiga Fasa Memiliki Magnitudo Sama." },
  { "en": "Apa Itu Beban Seimbang?", "id": "Impedansi Sama Di Setiap Fasa." },
  { "en": "Apa Itu Faktor Ketidakseimbangan?", "id": "Ukuran Kuantitatif Dari Ketidakseimbangan Tegangan." },
  { "en": "Apa Itu Komponen Urutan Negatif?", "id": "Menyebabkan Pemanasan Berlebih Pada Motor." },
  { "en": "Apa Itu Komponen Urutan Nol?", "id": "Menyebabkan Arus Mengalir Di Konduktor Netral." },
  { "en": "Apa Itu Sistem Tenaga Empat Kawat?", "id": "Sistem Tiga Fasa Dengan Konduktor Netral." },
  { "en": "Apa Itu Sistem Tenaga Tiga Kawat?", "id": "Sistem Tiga Fasa Tanpa Konduktor Netral." },
  { "en": "Apa Itu Pembumian Netral?", "id": "Menghubungkan Titik Netral Sistem Ke Tanah." },
  { "en": "Apa Itu Gangguan Tanah?", "id": "Hubungan Singkat Antara Fasa Dan Tanah." },
  { "en": "Apa Itu Tegangan Lebih Transien?", "id": "Lonjakan Tegangan Jangka Sangat Pendek." },
  { "en": "Apa Itu Waktu Naik?", "id": "Waktu Pulsa Naik Dari 10% Ke 90%." },
  { "en": "Apa Itu Durasi Pulsa?", "id": "Lebar Waktu Dari Pulsa Transien." },
  { "en": "Apa Itu Energi Pulsa?", "id": "Energi Yang Terkandung Dalam Pulsa Transien." },
  { "en": "Apa Itu Proteksi Tegangan Lebih?", "id": "Melindungi Peralatan Dari Tegangan Lebih." },
  { "en": "Apa Itu Tingkat Proteksi Tegangan?", "id": "Tegangan Sisa Di Perangkat Proteksi." },
  { "en": "Apa Itu Koordinasi Isolasi?", "id": "Mengoordinasikan Kekuatan Isolasi Peralatan." },
  { "en": "Apa Itu Jarak Bebas (Clearance)?", "id": "Jarak Terpendek Di Udara Antar Konduktor." },
  { "en": "Apa Itu Jarak Rambat (Creepage)?", "id": "Jarak Terpendek Di Permukaan Isolator." },
  { "en": "Apa Itu Ketinggian?", "id": "Ketinggian Mempengaruhi Kekuatan Dielektrik Udara." },
  { "en": "Apa Itu Polusi?", "id": "Kontaminasi Mempengaruhi Kinerja Isolator." },
  { "en": "Apa Itu Busur Api (Arcing)?", "id": "Pelepasan Listrik Di Udara Atau Gas." },
  { "en": "Apa Itu Model Kualitas Daya?", "id": "Representasi Matematis Dari Sistem Dan Gangguan." },
  { "en": "Apa Itu Model Sag Stokastik?", "id": "Memprediksi Frekuensi Dan Magnitudo Sag." },
  { "en": "Apa Itu Propagasi Sag?", "id": "Bagaimana Sag Menyebar Melalui Jaringan Listrik." },
  { "en": "Apa Itu Area Kerentanan?", "id": "Area Geografis Yang Terkena Dampak Sag." },
  { "en": "Apa Itu Peta Kerentanan?", "id": "Visualisasi Geografis Dari Area Kerentanan." },
  { "en": "Apa Itu Toleransi Beban?", "id": "Kemampuan Beban Menahan Gangguan Kualitas Daya." },
  { "en": "Apa Itu Karakterisasi Beban?", "id": "Proses Menentukan Toleransi Beban." },
  { "en": "Apa Itu Pengujian Beban?", "id": "Menerapkan Gangguan Kualitas Daya Ke Beban." },
  { "en": "Apa Itu Generator Sag?", "id": "Peralatan Laboratorium Untuk Menghasilkan Sag." },
  { "en": "Apa Itu Solusi Mitigasi?", "id": "Teknologi Untuk Mengurangi Masalah Kualitas Daya." },
  { "en": "Apa Itu Analisis Ekonomi?", "id": "Mengevaluasi Kelayakan Finansial Suatu Solusi." },
  { "en": "Apa Itu Biaya Siklus Hidup?", "id": "Total Biaya Selama Umur Proyek." },
  { "en": "Apa Itu Nilai Sekarang Bersih (NPV)?", "id": "Ukuran Profitabilitas Investasi." },
  { "en": "Apa Itu Tingkat Pengembalian Internal (IRR)?", "id": "Tingkat Diskonto Yang Membuat NPV Nol." },
  { "en": "Apa Itu Analisis Sensitivitas?", "id": "Mempelajari Bagaimana Hasil Berubah Dengan Input." },
  { "en": "Apa Itu Pohon Keputusan?", "id": "Alat Grafis Untuk Pengambilan Keputusan." },
  { "en": "Apa Itu Penilaian Risiko?", "id": "Proses Mengevaluasi Risiko Kualitas Daya." },
  { "en": "Apa Itu Jaringan Bayesian?", "id": "Model Grafis Probabilistik Untuk Inferensi." },
  { "en": "Apa Itu Kecerdasan Buatan?", "id": "Kecerdasan Yang Ditunjukkan Oleh Mesin." },
  { "en": "Apa Itu Sistem Berbasis Pengetahuan?", "id": "Sistem Yang Menggunakan Basis Pengetahuan." },
  { "en": "Apa Itu Rekonfigurasi Jaringan?", "id": "Mengubah Topologi Jaringan Untuk Optimasi." },
  { "en": "Apa Itu Penempatan Kapasitor Optimal?", "id": "Menentukan Lokasi Terbaik Untuk Kapasitor." },
  { "en": "Apa Itu Penempatan Filter Optimal?", "id": "Menentukan Lokasi Terbaik Untuk Filter." },
  { "en": "Apa Itu Alokasi Harmonik?", "id": "Membagi Kapasitas Distorsi Harmonik Jaringan." },
  { "en": "Apa Itu Tanggung Jawab Harmonik?", "id": "Menentukan Kontribusi Sumber Harmonik." },
  { "en": "Apa Itu Pengukuran Sinkron?", "id": "Pengukuran Yang Disinkronkan Waktu." },
  { "en": "Apa Itu Pemantauan Kualitas Daya Terdistribusi?", "id": "Menggunakan Banyak Monitor Di Seluruh Jaringan." },
  { "en": "Apa Itu Arus Netral Berlebih?", "id": "Arus Tinggi Di Konduktor Netral." },
  { "en": "Apa Penyebab Arus Netral Berlebih?", "id": "Beban Non-linier Fasa-Tunggal." },
  { "en": "Apa Itu Transformator Penurun Netral?", "id": "Mengurangi Arus Harmonik Di Netral." },
  { "en": "Apa Itu Filter Urutan Nol?", "id": "Filter Khusus Untuk Arus Harmonik Triplen." },
  { "en": "Apa Itu Tegangan Transien Pemulihan (TRV)?", "id": "Tegangan Di Kontak Pemutus Sirkuit." },
  { "en": "Apa Itu Pembatasan Arus?", "id": "Tindakan Membatasi Magnitudo Arus Gangguan." },
  { "en": "Apa Itu Sekering Pembatas Arus?", "id": "Sekering Yang Membatasi Arus Puncak." },
  { "en": "Apa Itu Pemutus Sirkuit Pembatas Arus?", "id": "Pemutus Sirkuit Dengan Kemampuan Pembatasan." },
  { "en": "Apa Itu Osilasi Torsi Sub-Sinkron?", "id": "Interaksi Antara Sistem Listrik Dan Mekanis." },
  { "en": "Apa Itu Kompensasi Seri?", "id": "Dapat Menyebabkan Risiko Osilasi Sub-Sinkron." },
  { "en": "Apa Itu Filter Aktif Hibrida?", "id": "Menggabungkan Beberapa Tipe Filter Aktif." },
  { "en": "Apa Itu Sistem Pembangkit Listrik?", "id": "Semua Komponen Untuk Menghasilkan Energi Listrik." },
  { "en": "Apa Itu Sistem Transmisi?", "id": "Semua Komponen Untuk Menyalurkan Energi." },
  { "en": "Apa Itu Sistem Distribusi?", "id": "Semua Komponen Untuk Mendistribusikan Energi." },
  { "en": "Apa Itu Beban Pengguna Akhir?", "id": "Peralatan Yang Menggunakan Energi Listrik." },
  { "en": "Apa Itu Keseimbangan Pembangkitan-Beban?", "id": "Pembangkitan Harus Sama Dengan Beban." },
  { "en": "Apa Itu Kontrol Frekuensi?", "id": "Menjaga Keseimbangan Pembangkitan-Beban." },
  { "en": "Apa Itu Kontrol Tegangan?", "id": "Menjaga Keseimbangan Daya Reaktif." },
  { "en": "Apa Itu Cadangan Operasi?", "id": "Kapasitas Pembangkitan Cadangan Yang Tersedia." },
  { "en": "Apa Itu Keandalan?", "id": "Kemampuan Menyediakan Suplai Listrik." },
  { "en": "Apa Itu Keamanan?", "id": "Kemampuan Menahan Gangguan Mendadak." },
  { "en": "Apa Itu Pasar Listrik?", "id": "Sistem Untuk Jual Beli Energi Listrik." }



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
