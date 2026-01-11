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


    { "en": "Apa Itu 'PVP' (Photovoltaic Power)?", "id": "Daya Listrik Dari Sel Surya." },
    { "en": "Apa Itu 'STC' (Standard Test Conditions)?", "id": "Kondisi Standar Pengujian Panel Surya." },
    { "en": "Apa Itu 'NOCT' (Nominal Operating Cell Temperature)?", "id": "Suhu Operasi Normal Sel Surya." },
    { "en": "Apa Itu 'AM' (Air Mass) Dalam Optik?", "id": "Ketebalan Atmosfer Yang Dilewati Cahaya." },
    { "en": "Apa Itu 'Albedo' (Pantulan Cahaya)?", "id": "Rasio Cahaya Pantul Permainan Benda." },
    { "en": "Apa Itu 'Azimuth Angle' (Sudut Azimut)?", "id": "Orientasi Horizontal Panel Terhadap Utara." },
    { "en": "Apa Itu 'Zenith Angle' (Sudut Zenit)?", "id": "Sudut Matahari Terhadap Garis Tegak." },
    { "en": "Apa Itu 'Tilt Angle' (Sudut Kemiringan)?", "id": "Sudut Kemiringan Panel Terhadap Tanah." },
    { "en": "Apa Itu 'String Inverter' (Inverter String)?", "id": "Inverter Untuk Serangkaian Panel Surya." },
    { "en": "Apa Itu 'Microinverter' (Inverter Mikro)?", "id": "Inverter Kecil Untuk Setiap Panel." },
    { "en": "Apa Itu 'BOS' (Balance Of System)?", "id": "Komponen Pendukung Sistem Tenaga Surya." },
    { "en": "Apa Itu 'Nacelle' (Ruang Mesin Turbin)?", "id": "Wadah Komponen Utama Turbin Angin." },
    { "en": "Apa Itu 'Pitch Control' (Kendali Sudut Bilah)?", "id": "Pengaturan Sudut Bilah Turbin Angin." },
    { "en": "Apa Itu 'Yaw Control' (Kendali Arah)?", "id": "Pengaturan Arah Turbin Menghadap Angin." },
    { "en": "Apa Itu 'Cut-In Speed' (Kecepatan Mulai)?", "id": "Kecepatan Angin Minimal Putar Turbin." },
    { "en": "Apa Itu 'Cut-Out Speed' (Kecepatan Berhenti)?", "id": "Kecepatan Angin Maksimal Operasi Aman." },
    { "en": "Apa Itu 'Penstock' (Pipa Pesat)?", "id": "Saluran Air Tekanan Tinggi Turbin." },
    { "en": "Apa Itu 'Draft Tube' (Pipa Lepas)?", "id": "Saluran Keluar Air Dari Turbin." },
    { "en": "Apa Itu 'Spillway' (Saluran Limpah)?", "id": "Saluran Pembuangan Kelebihan Air Bendungan." },
    { "en": "Apa Itu 'Surge Tank' (Tangki Pendatar)?", "id": "Peredam Tekanan Air Dalam Pipa." },
    { "en": "Apa Itu 'Francis Turbine' (Turbin Francis)?", "id": "Turbin Air Reaksi Aliran Campuran." },
    { "en": "Apa Itu 'Kaplan Turbine' (Turbin Kaplan)?", "id": "Turbin Air Reaksi Tipe Propeller." },
    { "en": "Apa Itu 'Pelton Wheel' (Kincir Pelton)?", "id": "Turbin Air Impuls Tekanan Tinggi." },
    { "en": "Apa Itu 'Rankine Cycle' (Siklus Rankine)?", "id": "Siklus Termodinamika Pembangkit Listrik Tenaga." },
    { "en": "Apa Itu 'Brayton Cycle' (Siklus Brayton)?", "id": "Siklus Dasar Pembangkit Listrik Gas." },
    { "en": "Apa Itu 'Combined Cycle' (Siklus Kombinasi)?", "id": "Gabungan Turbin Gas Dan Uap." },
    { "en": "Apa Itu 'HRSG' (Heat Recovery Steam Generator)?", "id": "Pembangkit Uap Dari Panas Buang." },
    { "en": "Apa Itu 'Condenser' (Kondensor)?", "id": "Pengubah Uap Menjadi Air Kembali." },
    { "en": "Apa Itu 'Cooling Tower' (Menara Pendingin)?", "id": "Alat Pembuang Panas Ke Atmosfer." },
    { "en": "Apa Itu 'Boiler' (Ketel Uap)?", "id": "Wadah Pemanas Air Menjadi Uap." },
    { "en": "Apa Itu 'Economizer' (Ekonomiser)?", "id": "Pemanas Awal Air Sebelum Boiler." },
    { "en": "Apa Itu 'Superheater' (Pemanas Lanjut)?", "id": "Peningkat Suhu Uap Melebihi Jenuh." },
    { "en": "Apa Itu 'Fly Ash' (Abu Terbang)?", "id": "Sisa Pembakaran Batubara Sangat Halus." },
    { "en": "Apa Itu 'Bottom Ash' (Abu Dasar)?", "id": "Sisa Pembakaran Berat Di Dasar." },
    { "en": "Apa Itu 'ESP' (Electrostatic Precipitator)?", "id": "Penyaring Debu Menggunakan Medan Listrik." },
    { "en": "Apa Itu 'FGD' (Flue Gas Desulfurization)?", "id": "Proses Penghilangan Sulfur Gas Buang." },
    { "en": "Apa Itu 'Moderator' (Moderator Nuklir)?", "id": "Bahan Pelambat Laju Neutron Cepat." },
    { "en": "Apa Itu 'Control Rod' (Batang Kendali)?", "id": "Alat Pengatur Laju Reaksi Nuklir." },
    { "en": "Apa Itu 'Coolant' (Pendingin Reaktor)?", "id": "Cairan Penyerap Panas Inti Reaktor." },
    { "en": "Apa Itu 'Nuclear Fission' (Fisi Nuklir)?", "id": "Pembelahan Inti Atom Penghasil Energi." },
    { "en": "Apa Itu 'Nuclear Fusion' (Fusion Nuklir)?", "id": "Penggabungan Inti Atom Penghasil Energi." },
    { "en": "Apa Itu 'Uranium-235' (Bahan Bakar Nuklir)?", "id": "Isotop Bahan Bakar Reaktor Nuklir." },
    { "en": "Apa Itu 'Half-Life' (Waktu Paruh)?", "id": "Waktu Peluruhan Setengah Massa Radioaktif." },
    { "en": "Apa Itu 'DHW' (Domestic Hot Water)?", "id": "Sistem Pemanas Air Kebutuhan Domestik." },
    { "en": "Apa Itu 'HVAC' (Heating Ventilation Air Conditioning)?", "id": "Sistem Pengatur Suhu, Udara Ruangan." },
    { "en": "Apa Itu 'Chiller' (Mesin Pendingin)?", "id": "Mesin Pendingin Air Skala Besar." },
    { "en": "Apa Itu 'VAV' (Variable Air Volume)?", "id": "Sistem Pengatur Aliran Udara Ruangan." },
    { "en": "Apa Itu 'FCU' (Fan Coil Unit)?", "id": "Unit Pendingin Ruangan Berbasis Kipas." },
    { "en": "Apa Itu 'AHU' (Air Handling Unit)?", "id": "Unit Pengolah Udara Sentral Gedung." },
    { "en": "Apa Itu 'BAS' (Building Automation System)?", "id": "Sistem Otomatis Kendali Fasilitas Gedung." },
    { "en": "Apa Itu 'Occupancy Sensor' (Sensor Okupansi)?", "id": "Sensor Pendeteksi Keberadaan Manusia Ruangan." },
    { "en": "Apa Itu 'Power Factor Lagging'?", "id": "Faktor Daya Beban Induktif Dominan." },
    { "en": "Apa Itu 'Power Factor Leading'?", "id": "Faktor Daya Beban Kapasitif Dominan." },
    { "en": "Apa Itu 'Reactive Power Compensation' (Kompensasi Daya Reaktif)?", "id": "Perbaikan Tegangan Dengan Daya Reaktif." },
    { "en": "Apa Itu 'PFC Capacitor' (Kapasitor PFC)?", "id": "Kapasitor Untuk Memperbaiki Faktor Daya." },
    { "en": "Apa Itu 'Active Harmonic Filter' (Filter Harmonisa Aktif)?", "id": "Alat Elektronik Penekan Arus Harmonisa." },
    { "en": "Apa Itu 'Voltage Sag' (Penurunan Tegangan)?", "id": "Penurunan Tegangan Listrik Durasi Singkat." },
    { "en": "Apa Itu 'Voltage Swell' (Kenaikan Tegangan)?", "id": "Kenaikan Tegangan Listrik Durasi Singkat." },
    { "en": "Apa Itu 'Voltage Flicker' (Kedipan Tegangan)?", "id": "Variasi Tegangan Berulang Cepat Sesaat." },
    { "en": "Apa Itu 'Transient Overvoltage' (Tegangan Lebih Transien)?", "id": "Lonjakan Tegangan Sangat Cepat, Tajam." },
    { "en": "Apa Itu 'THD-I' (Total Harmonic Distortion Current)?", "id": "Persentase Distorsi Harmonisa Arus Listrik." },
    { "en": "Apa Itu 'THD-V' (Total Harmonic Distortion Voltage)?", "id": "Persentase Distorsi Harmonisa Tegangan Listrik." },
    { "en": "Apa Itu 'Individual Harmonic' (Harmonisa Individual)?", "id": "Kandungan Harmonisa Pada Orde Tertentu." },
    { "en": "Apa Itu 'Interharmonic' (Interharmonisa)?", "id": "Frekuensi Di Luar Kelipatan Dasar." },
    { "en": "Apa Itu 'Pst' (Short-term Perceptibility)?", "id": "Indikator Kedipan Tegangan Jangka Pendek." },
    { "en": "Apa Itu 'Plt' (Long-term Perceptibility)?", "id": "Indikator Kedipan Tegangan Jangka Panjang." },
    { "en": "Apa Itu 'K-Factor' (Faktor-K) Transformator?", "id": "Kemampuan Transformator Menahan Beban Harmonisa." },
    { "en": "Apa Itu 'Derating' (Penurunan Kapasitas)?", "id": "Penurunan Kemampuan Alat Akibat Kondisi." },
    { "en": "Apa Itu 'Skin Effect' (Efek Kulit)?", "id": "Arus Mengalir Di Permukaan Konduktor." },
    { "en": "Apa Itu 'Proximity Effect' (Efek Kedekatan)?", "id": "Gangguan Arus Antar Konduktor Berdekatan." },
    { "en": "Apa Itu 'Dielectric Loss' (Rugi Dielektrik)?", "id": "Energi Hilang Panas Dalam Isolator." },
    { "en": "Apa Itu 'Corona Discharge' (Pelepasan Korona)?", "id": "Ionisasi Udara Akibat Tegangan Tinggi." },
    { "en": "Apa Itu 'Specific Resistance' (Resistivitas Jenis)?", "id": "Hambatan Jenis Suatu Bahan Penghantar." },
    { "en": "Apa Itu 'Conductivity' (Konduktivitas Listrik)?", "id": "Kemampuan Bahan Mengalirkan Arus Listrik." },
    { "en": "Apa Itu 'Permeability' (Permeabilitas Magnet)?", "id": "Kemampuan Bahan Menghantarkan Medan Magnet." },
    { "en": "Apa Itu 'Permittivity' (Permitivitas Listrik)?", "id": "Kemampuan Bahan Menyimpan Medan Listrik." },
    { "en": "Apa Itu 'Ferromagnetic' (Bahan Feromagnetik)?", "id": "Bahan Yang Sangat Mudah Magnet." },
    { "en": "Apa Itu 'Paramagnetic' (Bahan Paramagnetik)?", "id": "Bahan Yang Lemah Menarik Magnet." },
    { "en": "Apa Itu 'Diamagnetic' (Bahan Diamagnetik)?", "id": "Bahan Yang Menolak Medan Magnet." },
    { "en": "Apa Itu 'Eddy Current' (Arus Eddy)?", "id": "Arus Induksi Merugi Dalam Logam." },
    { "en": "Apa Itu 'Hysteresis' (Histeresis Magnet)?", "id": "Keterlambatan Magnetisasi Terhadap Medan Magnet." },
    { "en": "Apa Itu 'Curie Point' (Titik Curie)?", "id": "Suhu Hilangnya Sifat Magnet Bahan." },
    { "en": "Apa Itu 'Coercive Force' (Gaya Koersif)?", "id": "Medan Magnet Penghilang Magnet Sisa." },
    { "en": "Apa Itu 'Magnetic Flux' (Fluks Magnet)?", "id": "Total Garis Gaya Magnet Area." },
    { "en": "Apa Itu 'MMF' (Magnetomotive Force)?", "id": "Gaya Penggerak Fluks Dalam Sirkuit." },
    { "en": "Apa Itu 'Reluctance' (Reluktansi Magnetik)?", "id": "Hambatan Terhadap Jalur Fluks Magnet." },
    { "en": "Apa Itu 'Self-Inductance' (Induktansi Diri)?", "id": "Induksi Tegangan Dalam Kumparan Sendiri." },
    { "en": "Apa Itu 'Mutual-Inductance' (Induktansi Timbal Balik)?", "id": "Induksi Tegangan Antar Dua Kumparan." },
    { "en": "Apa Itu 'Lenz Law' (Hukum Lenz)?", "id": "Arus Induksi Melawan Perubahan Magnet." },
    { "en": "Apa Itu 'Faraday Law' (Hukum Faraday)?", "id": "Induksi Listrik Akibat Perubahan Magnet." },
    { "en": "Apa Itu 'Ampere Law' (Hukum Ampere)?", "id": "Hubungan Arus Dengan Medan Magnet." },
    { "en": "Apa Itu 'Gauss Law' (Hukum Gauss)?", "id": "Hubungan Muatan Dengan Medan Listrik." },
    { "en": "Apa Itu 'Lorentz Force' (Gaya Lorentz)?", "id": "Gaya Pada Muatan Dalam Medan." },
    { "en": "Apa Itu 'Joule Effect' (Efek Joule)?", "id": "Produksi Panas Akibat Arus Listrik." },
    { "en": "Apa Itu 'Seebeck Effect' (Efek Seebeck)?", "id": "Konversi Beda Suhu Menjadi Tegangan." },
    { "en": "Apa Itu 'Peltier Effect' (Efek Peltier)?", "id": "Pemindahan Panas Melalui Aliran Listrik." },
    { "en": "Apa Itu 'Piezoelectric' (Bahan Piezoelektrik)?", "id": "Bahan Penghasil Listrik Dari Tekanan." },
    { "en": "Apa Itu 'Superconductivity' (Superkonduktivitas)?", "id": "Kemampuan Menghantar Tanpa Hambatan Sama Sekali." },
    { "en": "Apa Itu 'Quantum Tunneling' (Terowongan Kuantum)?", "id": "Elektron Menembus Hambatan Potensial Tipis." },
    { "en": "Apa Itu 'Uptime' (Waktu Aktif)?", "id": "Durasi Sistem Beroperasi Tanpa Gangguan." },
    { "en": "Apa Itu 'NYA' (Kabel Tembaga Tunggal Isolasi PVC)?", "id": "Kabel Tunggal Berisolasi PVC, Terbuka." },
    { "en": "Apa Itu 'NYM' (Kabel Tembaga Banyak Isolasi PVC)?", "id": "Kabel Berisolasi Ganda, Pemasangan Dalam." },
    { "en": "Apa Itu 'NYY' (Kabel Tembaga Tanah Isolasi PVC)?", "id": "Kabel Tahan Cuaca, Pemasangan Tanah." },
    { "en": "Apa Itu 'NYAF' (Kabel Serabut Tembaga Isolasi PVC)?", "id": "Kabel Serabut Fleksibel, Pemasangan Panel." },
    { "en": "Apa Itu 'NYMHY' (Kabel Serabut Fleksibel Isolasi PVC)?", "id": "Kabel Fleksibel, Untuk Alat Rumah." },
    { "en": "Apa Itu 'BC' (Bare Copper)?", "id": "Kabel Tembaga Tanpa Lapisan Isolasi." },
    { "en": "Apa Itu 'XLPE' (Cross-Linked Polyethylene)?", "id": "Bahan Isolasi Tahan Suhu Tinggi." },
    { "en": "Apa Itu 'EPR' (Ethylene Propylene Rubber)?", "id": "Karet Isolasi Kabel Tegangan Tinggi." },
    { "en": "Apa Itu 'SWA' (Steel Wire Armoured)?", "id": "Kabel Dengan Perisai Kawat Baja." },
    { "en": "Apa Itu 'AWA' (Aluminium Wire Armoured)?", "id": "Kabel Dengan Perisai Kawat Aluminium." },
    { "en": "Apa Itu 'TT System' (Terra-Terra System)?", "id": "Pembumian Netral Dan Beban Terpisah." },
    { "en": "Apa Itu 'TN-C System' (Terra-Neutral Combined System)?", "id": "Penghantar Netral Dan Proteksi Gabung." },
    { "en": "Apa Itu 'TN-S System' (Terra-Neutral Separate System)?", "id": "Penghantar Netral Dan Proteksi Pisah." },
    { "en": "Apa Itu 'TN-C-S System' (Terra-Neutral Combined Separate System)?", "id": "Gabungan Sistem Netral Tanah Terpisah." },
    { "en": "Apa Itu 'IT System' (Isolasi-Terra System)?", "id": "Sistem Tanpa Pembumian Sisi Sumber." },
    { "en": "Apa Itu 'Main Switchboard' (Papan Hubung Utama)?", "id": "Panel Pusat Distribusi Listrik Utama." },
    { "en": "Apa Itu 'Sub-Distribution Board' (Papan Distribusi Cabang)?", "id": "Panel Pembagi Arus Skala Cabang." },
    { "en": "Apa Itu 'Final Circuit' (Sirkuit Akhir)?", "id": "Jalur Listrik Menuju Beban Langsung." },
    { "en": "Apa Itu 'Ring Main' (Sirkuit Gelang Utama)?", "id": "Jalur Listrik Berbentuk Loop Tertutup." },
    { "en": "Apa Itu 'Radial Circuit' (Sirkuit Radial)?", "id": "Jalur Listrik Satu Arah Tunggal." },
    { "en": "Apa Itu 'Double Pole Switch' (Saklar Dua Kutub)?", "id": "Saklar Pemutus Fase Dan Netral." },
    { "en": "Apa Itu 'Intermediate Switch' (Saklar Silang)?", "id": "Saklar Kendali Dari Tiga Tempat." },
    { "en": "Apa Itu 'Two-Way Switch' (Saklar Dua Arah)?", "id": "Saklar Kendali Dari Dua Tempat." },
    { "en": "Apa Itu 'Dimmer Switch' (Saklar Peredup)?", "id": "Alat Pengatur Intensitas Cahaya Lampu." },
    { "en": "Apa Itu 'Pull Cord Switch' (Saklar Tarik)?", "id": "Saklar Aktif Melalui Tarikan Tali." },
    { "en": "Apa Itu 'Occupancy Sensor' (Sensor Keberadaan)?", "id": "Pendeteksi Gerakan Manusia Dalam Ruang." },
    { "en": "Apa Itu 'PIR Sensor' (Passive Infrared Sensor)?", "id": "Sensor Gerak Berdasarkan Radiasi Panas." },
    { "en": "Apa Itu 'Microwave Sensor' (Sensor Gelombang Mikro)?", "id": "Pendeteksi Gerakan Melalui Pantulan Gelombang." },
    { "en": "Apa Itu 'Flame Detector' (Pendeteksi Nyala Api)?", "id": "Sensor Pendeteksi Radiasi Cahaya Api." },
    { "en": "Apa Itu 'Manual Call Point' (Titik Panggil Manual)?", "id": "Tombol Aktivasi Alarm Kebakaran Manual." },
    { "en": "Apa Itu 'Exit Sign' (Tanda Keluar Darurat)?", "id": "Penunjuk Arah Keluar Saat Darurat." },
    { "en": "Apa Itu 'High Bay' (Lampu Teluk Tinggi)?", "id": "Lampu Untuk Langit-Langit Sangat Tinggi." },
    { "en": "Apa Itu 'Low Bay' (Lampu Teluk Rendah)?", "id": "Lampu Untuk Langit-Langit Ketinggian Sedang." },
    { "en": "Apa Itu 'IK08 Rating' (Peringkat Dampak 08)?", "id": "Tingkat Perlindungan Terhadap Benturan Mekanik." },
    { "en": "Apa Itu 'IK10 Rating' (Peringkat Dampak 10)?", "id": "Perlindungan Maksimal Terhadap Benturan Mekanik." },
    { "en": "Apa Itu 'Class I Equipment' (Peralatan Kelas Satu)?", "id": "Peralatan Listrik Dengan Proteksi Arde." },
    { "en": "Apa Itu 'Class II Equipment' (Peralatan Kelas Dua)?", "id": "Peralatan Dengan Isolasi Ganda, Aman." },
    { "en": "Apa Itu 'Class III Equipment' (Peralatan Kelas Tiga)?", "id": "Peralatan Dengan Tegangan Sangat Rendah." },
    { "en": "Apa Itu 'Loop Impedance Tester' (Penguji Impedansi Loop)?", "id": "Alat Ukur Hambatan Jalur Gangguan." },
    { "en": "Apa Itu 'PAT Testing' (Portable Appliance Testing)?", "id": "Pengujian Keamanan Alat Listrik Portabel." },
    { "en": "Apa Itu 'Ducting' (Saluran Kabel)?", "id": "Jalur Pelindung Kabel Listrik Tertutup." },
    { "en": "Apa Itu 'Saddle Clamp' (Klem Pelana)?", "id": "Pengikat Pipa Kabel Pada Dinding." },
    { "en": "Apa Itu 'Beam Clamp' (Klem Balok)?", "id": "Pengikat Jalur Kabel Pada Balok." },
    { "en": "Apa Itu 'Unistrut' (Kanal Unistrut)?", "id": "Rangka Penyangga Jalur Kabel Modular." },
    { "en": "Apa Itu 'Junction Box' (Kotak Sambung)?", "id": "Wadah Tempat Penyambungan Kabel Listrik." },
    { "en": "Apa Itu 'Consumer Unit' (Unit Konsumen)?", "id": "Kotak Sekring Rumah Tangga Modern." },
    { "en": "Apa Itu 'Busbar Chamber' (Ruang Busbar)?", "id": "Kompartemen Khusus Penempatan Rel Tembaga." },
    { "en": "Apa Itu 'Earth Pit' (Sumur Arde)?", "id": "Titik Penanaman Elektroda Pembumian Tanah." },
    { "en": "Apa Itu 'Lightning Finial' (Ujung Penangkap Petir)?", "id": "Batang Logam Runcing Penangkap Petir." },
    { "en": "Apa Itu 'Surge Arrester Type 1' (Penangkap Surja Tipe Satu)?", "id": "Pelindung Lonjakan Akibat Sambaran Langsung." },
    { "en": "Apa Itu 'Harmonic Mitigator' (Mitigator Harmonisa)?", "id": "Alat Pengurang Gangguan Harmonisa Sistem." },
    { "en": "Apa Itu 'Detuned Reactor' (Reaktor Detuned)?", "id": "Induktor Pelindung Kapasitor Dari Harmonisa." },
    { "en": "Apa Itu 'Step Capacitor' (Kapasitor Bertahap)?", "id": "Unit Kapasitor Pengatur Faktor Daya." },
    { "en": "Apa Itu 'Annunciator Panel' (Panel Anunsitor)?", "id": "Panel Indikator Status Gangguan Visual." },
    { "en": "Apa Itu 'EMI Gasket' (Gasket EMI)?", "id": "Penutup Celah Penahan Radiasi Magnetik." },
    { "en": "Apa Itu 'Wire Mesh Tray' (Baki Jaring Kabel)?", "id": "Penyangga Kabel Berbentuk Jaring Besi." },
    { "en": "Apa Itu 'C-Channel' (Kanal C)?", "id": "Profil Baja Penyangga Instalasi Listrik." },
    { "en": "Apa Itu 'Threaded Rod' (Batang Ulir)?", "id": "Batang Besi Penggantung Jalur Kabel." },
    { "en": "Apa Itu 'Crimping' (Pengerutan)?", "id": "Proses Penyambungan Kabel Dengan Konektor." },
    { "en": "Apa Itu 'Draw Tape' (Pancingan Kabel)?", "id": "Alat Bantu Menarik Kabel Pipa." },
    { "en": "Apa Itu 'N2XY' (Kabel Tembaga Isolasi XLPE)?", "id": "Kabel Tanah Dengan Isolasi XLPE." },
    { "en": "Apa Itu 'NA2XY' (Kabel Aluminium Isolasi XLPE)?", "id": "Kabel Tanah Aluminium Isolasi XLPE." },
    { "en": "Apa Itu 'LSHF' (Low Smoke Halogen Free)?", "id": "Kabel Rendah Asap Tanpa Halogen." },
    { "en": "Apa Itu 'FRLS' (Fire Resistant Low Smoke)?", "id": "Kabel Tahan Api Rendah Asap." },
    { "en": "Apa Itu 'STA' (Steel Tape Armoured)?", "id": "Kabel Berperisai Pita Baja Pelindung." },
    { "en": "Apa Itu 'DSTA' (Double Steel Tape Armoured)?", "id": "Kabel Berperisai Pita Baja Ganda." },
    { "en": "Apa Itu 'Lead Sheath' (Selubung Timbal)?", "id": "Lapisan Timbal Pelindung Korosi Kabel." },
    { "en": "Apa Itu 'Screening' (Pelindung Tabir)?", "id": "Lapisan Logam Penahan Gangguan Sinyal." },
    { "en": "Apa Itu 'Bedding' (Lapisan Alas)?", "id": "Lapisan Bantalan Sebelum Perisai Baja." },
    { "en": "Apa Itu 'Outer Sheath' (Selubung Luar)?", "id": "Lapisan Plastik Paling Luar Kabel." },
    { "en": "Apa Itu 'Wire Stripper' (Pengupas Kabel)?", "id": "Alat Khusus Untuk Mengupas Isolasi." },
    { "en": "Apa Itu 'Bending Spring' (Pegas Pembengkok)?", "id": "Alat Bantu Menekuk Pipa PVC." },
    { "en": "Apa Itu 'Conduit Bender' (Pembengkok Pipa)?", "id": "Alat Penekuk Pipa Besi Listrik." },
    { "en": "Apa Itu 'Spirit Level' (Waterpass)?", "id": "Alat Pengukur Kedataran Instalasi Listrik." },
    { "en": "Apa Itu 'Wall Chaser' (Mesin Bobok)?", "id": "Alat Pembuat Jalur Kabel Dinding." },
    { "en": "Apa Itu 'Cable Winch' (Derek Kabel)?", "id": "Mesin Penarik Kabel Berat Jarak-Jauh." },
    { "en": "Apa Itu 'Swivel' (Konektor Putar)?", "id": "Alat Penyambung Kabel Penarik Berputar." },
    { "en": "Apa Itu 'Pulling Eye' (Mata Penarik)?", "id": "Ujung Penarik Kabel Pada Konduktor." },
    { "en": "Apa Itu 'Drum Jack' (Dongkrak Haspel)?", "id": "Alat Pengangkat Gulungan Kabel Besar." },
    { "en": "Apa Itu 'Cable Chamber' (Kamar Kabel)?", "id": "Ruang Bawah Tanah Pertemuan Kabel." },
    { "en": "Apa Itu 'Duct Bank' (Deretan Pipa)?", "id": "Kumpulan Pipa Kabel Dalam Beton." },
    { "en": "Apa Itu 'Trunking' (Kanal Kabel)?", "id": "Wadah Persegi Pelindung Jalur Kabel." },
    { "en": "Apa Itu 'Locknut' (Mur Pengunci)?", "id": "Mur Penahan Kelen Pada Panel." },
    { "en": "Apa Itu 'Earth Tag' (Label Arde)?", "id": "Terminal Hubung Tanah Pada Kelen." },
    { "en": "Apa Itu 'Shroud' (Selubung Kelen)?", "id": "Karet Pelindung Kelen Dari Air." },
    { "en": "Apa Itu 'Firestop' (Penahan Api)?", "id": "Bahan Penutup Lubang Penahan Api." },
    { "en": "Apa Itu 'Intumescent' (Bahan Mengembang)?", "id": "Bahan Yang Memuai Saat Panas." },
    { "en": "Apa Itu 'Saddle' (Klem Sadel)?", "id": "Penjepit Pipa Kabel Tipe Pelana." },
    { "en": "Apa Itu 'Cable Tie' (Pengikat Kabel)?", "id": "Tali Plastik Pengikat Kerapian Kabel." },
    { "en": "Apa Itu 'Marker' (Penanda Kabel)?", "id": "Label Identitas Jalur Kabel Listrik." },
    { "en": "Apa Itu 'Sleeve' (Selongsong Sambung)?", "id": "Tabung Penghubung Dua Ujung Kabel." },
    { "en": "Apa Itu 'Boot' (Sepatu Kabel)?", "id": "Pelindung Ujung Kabel Pada Terminasi." },
    { "en": "Apa Itu 'Breakout' (Pemisah Inti)?", "id": "Selongsong Pembagi Inti Kabel Banyak." },
    { "en": "Apa Itu 'Jointing' (Penyambungan)?", "id": "Proses Menyatukan Dua Ujung Kabel." },
    { "en": "Apa Itu 'Splice' (Sambungan)?", "id": "Teknik Penyambungan Kabel Secara Langsung." },
    { "en": "Apa Itu 'Connector' (Konektor)?", "id": "Komponen Penghubung Aliran Arus Listrik." },
    { "en": "Apa Itu 'Lug' (Sepatu Kabel)?", "id": "Terminal Logam Pada Ujung Kabel." },
    { "en": "Apa Itu 'Pin' (Pin Kontak)?", "id": "Bagian Logam Penusuk Konektor Listrik." },
    { "en": "Apa Itu 'Socket' (Soket Kontak)?", "id": "Bagian Lubang Penerima Pin Listrik." },
    { "en": "Apa Itu 'Downtime' (Waktu Padam)?", "id": "Durasi Sistem Berhenti Beroperasi Total." },
    { "en": "Apa Itu 'CVD' (Chemical Vapor Deposition)?", "id": "Proses Pengendapan Uap Kimia Tipis." },
    { "en": "Apa Itu 'PVD' (Physical Vapor Deposition)?", "id": "Proses Pengendapan Uap Fisik Lapisan." },
    { "en": "Apa Itu 'Fotolitografi' (Photolithography)?", "id": "Proses Pencetakan Pola Sirkuit Chip." },
    { "en": "Apa Itu 'Implantasi Ion' (Ion Implantation)?", "id": "Proses Penembakan Ion Ke Semikonduktor." },
    { "en": "Apa Itu 'Epitaksi' (Epitaxy)?", "id": "Pertumbuhan Kristal Baru Di Wafer." },
    { "en": "Apa Itu 'Etsa Plasma' (Plasma Etching)?", "id": "Pengikisan Material Menggunakan Gas Ion." },
    { "en": "Apa Itu 'Tungku Difusi' (Diffusion Furnace)?", "id": "Alat Pemanas Untuk Proses Difusi." },
    { "en": "Apa Itu 'Pengirisan Wafer' (Wafer Slicing)?", "id": "Pemotongan Batangan Silikon Menjadi Wafer." },
    { "en": "Apa Itu 'CMP' (Chemical Mechanical Planarization)?", "id": "Proses Perataan Permukaan Wafer Silikon." },
    { "en": "Apa Itu 'Fotoresis' (Photoresist)?", "id": "Material Peka Cahaya Untuk Litografi." },
    { "en": "Apa Itu 'Penyelaras Masker' (Mask Aligner)?", "id": "Alat Penepat Pola Masker Sirkuit." },
    { "en": "Apa Itu 'Mesin Stepper' (Stepper Machine)?", "id": "Alat Proyeksi Pola Sirkuit Mikro." },
    { "en": "Apa Itu 'Perekatan Die' (Die Bonding)?", "id": "Proses Menempelkan Chip Ke Bingkai." },
    { "en": "Apa Itu 'Perekatan Kawat' (Wire Bonding)?", "id": "Penyambungan Kawat Halus Antar Terminal." },
    { "en": "Apa Itu 'Bingkai Timah' (Lead Frame)?", "id": "Struktur Logam Penyangga Chip Internal." },
    { "en": "Apa Itu 'Enkapsulasi' (Encapsulation)?", "id": "Proses Pembungkusan Chip Pelindung Luar." },
    { "en": "Apa Itu 'Penskoran Wafer' (Wafer Scribing)?", "id": "Proses Pemberian Jalur Potong Wafer." },
    { "en": "Apa Itu 'Pendopingan Tipe-N' (N-Type Doping)?", "id": "Pemberian Unsur Pembawa Muatan Negatif." },
    { "en": "Apa Itu 'Pendopingan Tipe-P' (P-Type Doping)?", "id": "Pemberian Unsur Pembawa Muatan Positif." },
    { "en": "Apa Itu 'Oksida Gerbang' (Gate Oxide)?", "id": "Lapisan Isolator Tipis Pada Transistor." },
    { "en": "Apa Itu 'Polisilikon' (Polysilicon)?", "id": "Bahan Penghantar Gerbang Transistor MOSFET." },
    { "en": "Apa Itu 'Isolasi Parit Dangkal' (Shallow Trench Isolation)?", "id": "Pemisah Antar Komponen Dalam Chip." },
    { "en": "Apa Itu 'LOCOS' (Local Oxidation Of Silicon)?", "id": "Teknik Isolasi Menggunakan Oksidasi Lokal." },
    { "en": "Apa Itu 'Hukum Moore' (Moore's Law)?", "id": "Prediksi Penggandaan Jumlah Transistor Chip." },
    { "en": "Apa Itu 'SOA' (Safe Operating Area)?", "id": "Batas Operasi Aman Komponen Daya." },
    { "en": "Apa Itu 'RDSon' (Drain-Source On Resistance)?", "id": "Hambatan MOSFET Saat Kondisi Aktif." },
    { "en": "Apa Itu 'Muatan Gerbang' (Gate Charge)?", "id": "Energi Untuk Mengaktifkan Gerbang MOSFET." },
    { "en": "Apa Itu 'Waktu Pemulihan Terbalik' (Reverse Recovery Time)?", "id": "Waktu Dioda Berhenti Menghantar Arus." },
    { "en": "Apa Itu 'Pemulihan Lunak' (Soft Recovery)?", "id": "Penurunan Arus Dioda Secara Bertahap." },
    { "en": "Apa Itu 'Pemulihan Keras' (Hard Recovery)?", "id": "Penurunan Arus Dioda Secara Tiba-Tiba." },
    { "en": "Apa Itu 'Dioda Body' (Body Diode)?", "id": "Dioda Parasitik Internal Pada MOSFET." },
    { "en": "Apa Itu 'Keruntuhan Longsoran' (Avalanche Breakdown)?", "id": "Kerusakan Akibat Tegangan Terbalik Tinggi." },
    { "en": "Apa Itu 'Keruntuhan Zener' (Zener Breakdown)?", "id": "Mekanisme Aliran Arus Dioda Zener." },
    { "en": "Apa Itu 'Resistansi Termal' (Thermal Resistance)?", "id": "Hambatan Terhadap Aliran Panas Komponen." },
    { "en": "Apa Itu 'Suhu Persambungan' (Junction Temperature)?", "id": "Suhu Bagian Dalam Semikonduktor Aktif." },
    { "en": "Apa Itu 'Suhu Casing' (Case Temperature)?", "id": "Suhu Pada Permukaan Luar Komponen." },
    { "en": "Apa Itu 'Persambungan Ke Sekitar' (Junction To Ambient)?", "id": "Total Hambatan Panas Ke Lingkungan." },
    { "en": "Apa Itu 'Persambungan Ke Casing' (Junction To Case)?", "id": "Hambatan Panas Dari Chip Samping." },
    { "en": "Apa Itu 'Penurunan Rating Tegangan' (Voltage Derating)?", "id": "Pengurangan Batas Tegangan Operasi Aman." },
    { "en": "Apa Itu 'Penurunan Rating Arus' (Current Derating)?", "id": "Pengurangan Batas Arus Operasi Aman." },
    { "en": "Apa Itu 'Efisiensi Konversi' (Conversion Efficiency)?", "id": "Rasio Daya Keluar Terhadap Masuk." },
    { "en": "Apa Itu 'Daya Siaga' (Standby Power)?", "id": "Konsumsi Listrik Saat Alat Mati." },
    { "en": "Apa Itu 'Beban Tiruan' (Dummy Load)?", "id": "Resistor Pengganti Beban Untuk Pengujian." },
    { "en": "Apa Itu 'Beban Elektronik' (Electronic Load)?", "id": "Alat Simulasi Berbagai Karakteristik Beban." },
    { "en": "Apa Itu 'Operasi Empat Kuadran' (Four-Quadrant Operation)?", "id": "Kemampuan Mesin Melakukan Segala Gerak." },
    { "en": "Apa Itu 'Pengereman Regeneratif' (Regenerative Braking)?", "id": "Pengubahan Energi Gerak Menjadi Listrik." },
    { "en": "Apa Itu 'Tautan Arus Searah' (DC Link)?", "id": "Jalur Penghubung Antar Konverter Daya." },
    { "en": "Apa Itu 'Tegangan Bus' (Bus Voltage)?", "id": "Level Tegangan Pada Jalur Utama." },
    { "en": "Apa Itu 'Arus Riak' (Ripple Current)?", "id": "Variasi Arus Kecil Sisa Penyearahan." },
    { "en": "Apa Itu 'ESR' (Equivalent Series Resistance)?", "id": "Hambatan Internal Seri Sebuah Kapasitor." },
    { "en": "Apa Itu 'ESL' (Equivalent Series Inductance)?", "id": "Induktansi Internal Seri Sebuah Kapasitor." },
    { "en": "Apa Itu 'Kapasitansi Parasitik' (Parasitic Capacitance)?", "id": "Kapasitansi Tak Diinginkan Antar Jalur." },
    { "en": "Apa Itu 'Induktansi Parasitik' (Parasitic Inductance)?", "id": "Induktansi Tak Diinginkan Jalur Penghantar." },
    { "en": "Apa Itu 'Derau Mode Bersama' (Common Mode Noise)?", "id": "Gangguan Sinyal Pada Kedua Jalur." },
    { "en": "Apa Itu 'Derau Mode Diferensial' (Differential Mode Noise)?", "id": "Gangguan Sinyal Antar Dua Jalur." },
    { "en": "Apa Itu 'Pelindung Elektrostatik' (Electrostatic Shielding)?", "id": "Penghalang Pengaruh Medan Listrik Luar." },
    { "en": "Apa Itu 'Perisai Mu-Metal' (Mu-Metal Shielding)?", "id": "Pelindung Dari Pengaruh Medan Magnet." },
    { "en": "Apa Itu 'Balun Tegangan' (Voltage Balun)?", "id": "Penyeimbang Tegangan Antar Jalur Berbeda." },
    { "en": "Apa Itu 'Balun Arus' (Current Balun)?", "id": "Penyeimbang Arus Pada Jalur Antena." },
    { "en": "Apa Itu 'Rumus Kedalaman Kulit' (Skin Depth Formula)?", "id": "Perhitungan Penetrasi Arus Frekuensi Tinggi." },
    { "en": "Apa Itu 'Jarak Rayap' (Creepage Distance)?", "id": "Jarak Terpendek Sepanjang Permukaan Isolator." },
    { "en": "Apa Itu 'Jarak Bebas' (Clearance Distance)?", "id": "Jarak Terpendek Melalui Udara Terbuka." },
    { "en": "Apa Itu 'Konstanta Dielektrik' (Dielectric Constant)?", "id": "Rasio Kapasitansi Bahan Terhadap Vakum." },
    { "en": "Apa Itu 'Tangen Kerugian' (Loss Tangent)?", "id": "Ukuran Disipasi Energi Dalam Isolator." },
    { "en": "Apa Itu 'Selektivitas' (Selectivity)?", "id": "Kemampuan Memilih Sinyal Frekuensi Tertentu." },
    { "en": "Apa Itu 'Linearitas' (Linearity)?", "id": "Akurasi Output Terhadap Perubahan Input." },
    { "en": "Apa Itu 'Repeatabilitas' (Repeatability)?", "id": "Konsistensi Hasil Pengukuran Berulang Kali." },
    { "en": "Apa Itu 'Hanyutan Sinyal' (Signal Drift)?", "id": "Perubahan Nilai Output Seiring Waktu." },
    { "en": "Apa Itu 'Ofset Nol' (Zero Offset)?", "id": "Nilai Output Saat Input Nol." },
    { "en": "Apa Itu 'Kalibrasi Lapangan' (Field Calibration)?", "id": "Proses Penyesuaian Alat Di Lokasi." },
    { "en": "Apa Itu 'Ketertelusuran' (Traceability)?", "id": "Hubungan Kalibrasi Dengan Standar Nasional." },
    { "en": "Apa Itu 'Ketidakpastian Pengukuran' (Measurement Uncertainty)?", "id": "Rentang Keraguan Dari Hasil Pengukuran." },
    { "en": "Apa Itu 'Deviasi Standar' (Standard Deviation)?", "id": "Ukuran Persebaran Data Hasil Ukur." },
    { "en": "Apa Itu 'Pengkondisian Sinyal' (Signal Conditioning)?", "id": "Proses Penyesuaian Sinyal Untuk Digital." },
    { "en": "Apa Itu 'Konverter RMS' (RMS Converter)?", "id": "Pengubah Sinyal AC Menjadi DC." },
    { "en": "Apa Itu 'RMS Sebenarnya' (True RMS)?", "id": "Pengukuran Akurat Berbagai Bentuk Gelombang." },
    { "en": "Apa Itu 'Pendeteksi Amplop' (Envelope Detector)?", "id": "Pemisah Sinyal Informasi Dari Pembawa." },
    { "en": "Apa Itu 'Penguat Lock-In' (Lock-In Amplifier)?", "id": "Alat Ekstraksi Sinyal Dalam Derau." },
    { "en": "Apa Itu 'Integrator Boxcar' (Boxcar Integrator)?", "id": "Alat Pengambil Sampel Sinyal Cepat." },
    { "en": "Apa Itu 'Ujung Depan Analog' (Analog Front End)?", "id": "Rangkaian Pemroses Sinyal Analog Pertama." },
    { "en": "Apa Itu 'Pra-Penguat' (Preamplifier)?", "id": "Penguat Awal Untuk Sinyal Lemah." },
    { "en": "Apa Itu 'Penguat Penyangga' (Buffer Amplifier)?", "id": "Penguat Untuk Penyesuaian Impedansi Rangkaian." },
    { "en": "Apa Itu 'Resistor Terminasi' (Termination Resistor)?", "id": "Penyerap Pantulan Sinyal Ujung Kabel." },
    { "en": "Apa Itu 'Resistor Pembuang' (Discharge Resistor)?", "id": "Pengosong Muatan Kapasitor Demi Keamanan." },
    { "en": "Apa Itu 'RC Snubber' (Resistor Capacitor Snubber)?", "id": "Rangkaian Peredam Lonjakan Tegangan Transien." },
    { "en": "Apa Itu 'Dioda Komutasi' (Commutation Diode)?", "id": "Dioda Pengarah Arus Selama Pensakelaran." },
    { "en": "Apa Itu 'Komutasi' (Commutation)?", "id": "Proses Pengalihan Arus Antar Saklar." },
    { "en": "Apa Itu 'Komutasi Paksa' (Forced Commutation)?", "id": "Pemutusan Arus Menggunakan Rangkaian Tambahan." },
    { "en": "Apa Itu 'Komutasi Alami' (Natural Commutation)?", "id": "Pemutusan Arus Saat Melewati Nol." },
    { "en": "Apa Itu 'Komutasi Jalur' (Line Commutation)?", "id": "Komutasi Berdasarkan Tegangan Jala Listrik." },
    { "en": "Apa Itu 'Komutasi Beban' (Load Commutation)?", "id": "Komutasi Berdasarkan Karakteristik Beban Induktif." },
    { "en": "Apa Itu 'Sudut Overlap' (Overlap Angle)?", "id": "Durasi Arus Mengalir Di Dua." },
    { "en": "Apa Itu 'Sudut Penyulutan' (Firing Angle)?", "id": "Waktu Pemicuan Gerbang Tiristor Daya." },
    { "en": "Apa Itu 'Sudut Pemadaman' (Extinction Angle)?", "id": "Waktu Saat Arus Berhenti Mengalir." },
    { "en": "Apa Itu 'Waktu Tahan Mati' (Hold-Off Time)?", "id": "Waktu Tunggu Sebelum Penyulutan Kembali." },
    { "en": "Apa Itu 'Waktu Hidup' (Turn-On Time)?", "id": "Durasi Perubahan Status Menjadi Aktif." },
    { "en": "Apa Itu 'Waktu Mati' (Turn-Off Time)?", "id": "Durasi Perubahan Status Menjadi Mati." },
    { "en": "Apa Itu 'Waktu Penyimpanan' (Storage Time)?", "id": "Lama Muatan Tersimpan Sebelum Mati." },
    { "en": "Apa Itu 'Waktu Turun Arus' (Current Fall Time)?", "id": "Durasi Penurunan Arus Menuju Nol." },
    { "en": "Apa Itu 'Waktu Naik Tegangan' (Voltage Rise Time)?", "id": "Durasi Kenaikan Tegangan Menuju Nominal." },
    { "en": "Apa Itu 'ANSI 50' (Instantaneous Overcurrent)?", "id": "Proteksi Arus Lebih Tanpa Tunda." },
    { "en": "Apa Itu 'ANSI 51' (Time Overcurrent)?", "id": "Proteksi Arus Lebih Dengan Tunda." },
    { "en": "Apa Itu 'ANSI 27' (Undervoltage)?", "id": "Proteksi Tegangan Rendah Sistem Listrik." },
    { "en": "Apa Itu 'ANSI 59' (Overvoltage)?", "id": "Proteksi Tegangan Lebih Sistem Listrik." },
    { "en": "Apa Itu 'ANSI 87' (Differential Relay)?", "id": "Proteksi Selisih Arus Masuk, Keluar." },
    { "en": "Apa Itu 'ANSI 21' (Distance Relay)?", "id": "Proteksi Berdasarkan Impedansi Jalur Transmisi." },
    { "en": "Apa Itu 'ANSI 32' (Directional Power Relay)?", "id": "Proteksi Arah Aliran Daya Listrik." },
    { "en": "Apa Itu 'ANSI 40' (Loss Of Field)?", "id": "Proteksi Kehilangan Eksitasi Pada Generator." },
    { "en": "Apa Itu 'ANSI 46' (Reverse Phase)?", "id": "Proteksi Ketidakseimbangan Arus Fase Listrik." },
    { "en": "Apa Itu 'ANSI 47' (Phase Sequence Voltage)?", "id": "Proteksi Urutan Fase Tegangan Listrik." },
    { "en": "Apa Itu 'ANSI 49' (Thermal Overload)?", "id": "Proteksi Panas Berlebih Pada Peralatan." },
    { "en": "Apa Itu 'ANSI 52' (AC Circuit Breaker)?", "id": "Saklar Pemutus Arus Bolak-Balik Otomatis." },
    { "en": "Apa Itu 'ANSI 64' (Ground Detector)?", "id": "Pendeteksi Kebocoran Arus Ke Tanah." },
    { "en": "Apa Itu 'ANSI 67' (Directional Overcurrent)?", "id": "Proteksi Arus Lebih Dengan Arah." },
    { "en": "Apa Itu 'ANSI 74' (Alarm Relay)?", "id": "Relay Pemantau Kondisi Sirkuit Alarm." },
    { "en": "Apa Itu 'ANSI 79' (Reclosing Relay)?", "id": "Relay Penutup Kembali Sirkuit Otomatis." },
    { "en": "Apa Itu 'ANSI 81' (Frequency Relay)?", "id": "Proteksi Perubahan Frekuensi Jaringan Listrik." },
    { "en": "Apa Itu 'ANSI 86' (Lockout Relay)?", "id": "Relay Pengunci Sistem Setelah Gangguan." },
    { "en": "Apa Itu 'RCCB' (Residual Current Circuit Breaker)?", "id": "Pemutus Sirkuit Akibat Arus Sisa." },
    { "en": "Apa Itu 'RCBO' (Residual Current Breaker Overload)?", "id": "Pemutus Arus Sisa Dan Beban." },
    { "en": "Apa Itu 'Hipot Test' (High Potential Test)?", "id": "Uji Kekuatan Isolasi Tegangan Tinggi." },
    { "en": "Apa Itu 'Partial Discharge' (Pelepasan Sebagian)?", "id": "Kegagalan Isolasi Listrik Lokal Kecil." },
    { "en": "Apa Itu 'Tan Delta Test' (Uji Tan Delta)?", "id": "Uji Degradasi Isolasi Peralatan Listrik." },
    { "en": "Apa Itu 'Sweep Frequency Response Analysis' (Analisis Respon Frekuensi)?", "id": "Uji Integritas Mekanik Belitan Transformator." },
    { "en": "Apa Itu 'DGA' (Dissolved Gas Analysis)?", "id": "Analisis Gas Terlarut Minyak Transformator." },
    { "en": "Apa Itu 'Furan Analysis' (Analisis Furan)?", "id": "Uji Penuaan Isolasi Kertas Transformator." },
    { "en": "Apa Itu 'Polarization Index' (Indeks Polarisasi)?", "id": "Rasio Tahanan Isolasi Waktu Tertentu." },
    { "en": "Apa Itu 'Step Voltage Test' (Uji Tegangan Langkah)?", "id": "Uji Ketahanan Isolasi Secara Bertahap." },
    { "en": "Apa Itu 'Earth Resistance' (Tahanan Tanah)?", "id": "Hambatan Elektroda Pembumian Terhadap Bumi." },
    { "en": "Apa Itu 'Soil Resistivity' (Resistivitas Tanah)?", "id": "Ukuran Hambatan Jenis Tanah Lokal." },
    { "en": "Apa Itu 'Fall Of Potential Method' (Metode Jatuh Potensial)?", "id": "Teknik Pengukuran Tahanan Pentanahan Standar." },
    { "en": "Apa Itu 'Wenner Method' (Metode Wenner)?", "id": "Teknik Pengukuran Resistivitas Tanah Empat." },
    { "en": "Apa Itu 'Clamp-On Ground Tester' (Penguji Tanah Jepit)?", "id": "Alat Ukur Arde Tanpa Pasak." },
    { "en": "Apa Itu 'Inductive Load' (Beban Induktif)?", "id": "Beban Yang Mengonsumsi Daya Reaktif." },
    { "en": "Apa Itu 'Capacitive Load' (Beban Kapasitif)?", "id": "Beban Yang Menyuplai Daya Reaktif." },
    { "en": "Apa Itu 'Resistive Load' (Beban Resistif)?", "id": "Beban Yang Mengonsumsi Daya Aktif." },
    { "en": "Apa Itu 'Non-Linear Load' (Beban Non-Linear)?", "id": "Beban Penyebab Distorsi Gelombang Arus." },
    { "en": "Apa Itu 'Crest Factor' (Faktor Puncak)?", "id": "Rasio Nilai Puncak Terhadap Efektif." },
    { "en": "Apa Itu 'Displacement Power Factor' (Faktor Daya Pergeseran)?", "id": "Faktor Daya Akibat Pergeseran Fase." },
    { "en": "Apa Itu 'Distortion Power Factor' (Faktor Daya Distorsi)?", "id": "Faktor Daya Akibat Kandungan Harmonisa." },
    { "en": "Apa Itu 'Total Power Factor' (Faktor Daya Total)?", "id": "Hasil Kali Pergeseran Dan Distorsi." },
    { "en": "Apa Itu 'K-Factor Transformer' (Transformator Faktor-K)?", "id": "Transformator Khusus Untuk Beban Harmonisa." },
    { "en": "Apa Itu 'Zigzag Transformer' (Transformator Zigzag)?", "id": "Transformator Pembentuk Titik Netral Baru." },
    { "en": "Apa Itu 'Scott-T Connection' (Hubungan Scott-T)?", "id": "Pengubah Sistem Tiga Ke Dua." },
    { "en": "Apa Itu 'Open Delta Connection' (Hubungan Delta Terbuka)?", "id": "Transformasi Tiga Fase Dengan Dua." },
    { "en": "Apa Itu 'Burden' (Beban Instrumen)?", "id": "Beban Terhubung Pada Sekunder CT/PT." },
    { "en": "Apa Itu 'Saturation Curve' (Kurva Saturasi)?", "id": "Grafik Hubungan Arus Dan Tegangan." },
    { "en": "Apa Itu 'Accuracy Class' (Kelas Akurasi)?", "id": "Batas Kesalahan Pengukuran Alat Ukur." },
    { "en": "Apa Itu 'Composite Error' (Kesalahan Komposit)?", "id": "Total Kesalahan Arus Pada CT." },
    { "en": "Apa Itu 'Instrument Security Factor' (Faktor Keamanan Instrumen)?", "id": "Pelindung Meter Dari Arus Gangguan." },
    { "en": "Apa Itu 'Polarity' (Polaritas Listrik)?", "id": "Arah Aliran Arus Terminal Peralatan." },
    { "en": "Apa Itu 'Phase Shift' (Pergeseran Fase)?", "id": "Perbedaan Sudut Antar Dua Gelombang." },
    { "en": "Apa Itu 'In-Phase' (Satu Fase)?", "id": "Kondisi Dua Gelombang Berhimpit Bersamaan." },
    { "en": "Apa Itu 'Anti-Phase' (Berlawanan Fase)?", "id": "Kondisi Dua Gelombang Berlawanan Derajat." },
    { "en": "Apa Itu 'Phase Sequence' (Urutan Fase)?", "id": "Urutan Tegangan Fase Mencapai Puncak." },
    { "en": "Apa Itu 'Zero Sequence' (Urutan Nol)?", "id": "Komponen Simetris Untuk Gangguan Tanah." },
    { "en": "Apa Itu 'Positive Sequence' (Urutan Positif)?", "id": "Komponen Simetris Kondisi Operasi Normal." },
    { "en": "Apa Itu 'Negative Sequence' (Urutan Negatif)?", "id": "Komponen Simetris Kondisi Beban Tak-Seimbang." },
    { "en": "Apa Itu 'Fortescue Theorem' (Teorema Fortescue)?", "id": "Analisis Sistem Tak-Seimbang Melalui Komponen." },
    { "en": "Apa Itu 'Symmetrical Components' (Komponen Simetris)?", "id": "Metode Analisis Gangguan Listrik Tiga." },
    { "en": "Apa Itu 'Per-Unit System' (Sistem Per-Unit)?", "id": "Metode Perhitungan Nilai Normalisasi Listrik." },
    { "en": "Apa Itu 'Base Power' (Daya Dasar)?", "id": "Nilai Referensi Daya Untuk Perhitungan." },
    { "en": "Apa Itu 'Base Voltage' (Tegangan Dasar)?", "id": "Nilai Referensi Tegangan Untuk Perhitungan." },
    { "en": "Apa Itu 'Short Circuit MVA' (MVA Hubung Singkat)?", "id": "Kekuatan Hubung Singkat Titik Jaringan." },
    { "en": "Apa Itu 'Fault Level' (Tingkat Gangguan)?", "id": "Besar Arus Maksimum Saat Kerusakan." },
    { "en": "Apa Itu 'Arc Quenching' (Pemadaman Busur)?", "id": "Proses Penghilangan Percikan Listrik Saklar." },
    { "en": "Apa Itu 'Pre-Strike' (Pra-Loncatan)?", "id": "Loncatan Arus Sebelum Kontak Tertutup." },
    { "en": "Apa Itu 'Restrike' (Loncatan Ulang)?", "id": "Loncatan Arus Setelah Kontak Terbuka." },
    { "en": "Apa Itu 'TRV' (Transient Recovery Voltage)?", "id": "Tegangan Transien Muncul Setelah Pemutusan." },
    { "en": "Apa Itu 'RRRV' (Rate Of Rise Of Recovery Voltage)?", "id": "Kecepatan Kenaikan Tegangan Pemulihan Transien." },
    { "en": "Apa Itu 'Current Chopping' (Pencacahan Arus)?", "id": "Pemutusan Arus Sebelum Titik Nol." },
    { "en": "Apa Itu 'Capacitive Switching' (Pensakelaran Kapasitif)?", "id": "Proses Menghubung, Memutus Beban Kapasitor." },
    { "en": "Apa Itu 'Inductive Switching' (Pensakelaran Induktif)?", "id": "Proses Menghubung, Memutus Beban Induktor." },
    { "en": "Apa Itu 'Ferroresonance' (Feroresonansi)?", "id": "Resonansi Non-Linear Antar Kapasitor, Induktor." },
    { "en": "Apa Itu 'Sub-Synchronous Resonance' (Resonansi Sub-Sinkron)?", "id": "Interaksi Frekuensi Mekanik Dan Listrik." },
    { "en": "Apa Itu 'Torsional Vibration' (Getaran Torsional)?", "id": "Getaran Puntir Pada Poros Generator." },
    { "en": "Apa Itu 'Grid Code' (Kode Jaringan)?", "id": "Aturan Teknis Pengoperasian Sistem Listrik." },
    { "en": "Apa Itu 'Ride-Through Capability' (Kemampuan Bertahan)?", "id": "Kemampuan Pembangkit Menghadapi Gangguan Tegangan." },
    { "en": "Apa Itu 'Low Voltage Ride Through' (Bertahan Tegangan Rendah)?", "id": "Kemampuan Bertahan Saat Tegangan Turun." },
    { "en": "Apa Itu 'High Voltage Ride Through' (Bertahan Tegangan Tinggi)?", "id": "Kemampuan Bertahan Saat Tegangan Naik." },
    { "en": "Apa Itu 'Droop Control' (Kendali Droop)?", "id": "Pengaturan Frekuensi Berdasarkan Beban Aktif." },
    { "en": "Apa Itu 'Isochronous Mode' (Mode Isokron)?", "id": "Pengaturan Frekuensi Konstan Tanpa Perubahan." },
    { "en": "Apa Itu 'Inertia Constant' (Konstanta Inersia)?", "id": "Energi Simpanan Mekanik Bagian Berputar." },
    { "en": "Apa Itu 'Critical Clearing Time' (Waktu Pemutusan Kritis)?", "id": "Batas Waktu Maksimal Pemutusan Gangguan." },
    { "en": "Apa Itu 'Swing Equation' (Persamaan Ayunan)?", "id": "Model Matematika Dinamika Rotor Generator." },
    { "en": "Apa Itu 'Equal Area Criterion' (Kriteria Luas Sama)?", "id": "Metode Analisis Stabilitas Transien Generator." },
    { "en": "Apa Itu 'Governor Deadband' (Pita Mati Gubernur)?", "id": "Rentang Frekuensi Tanpa Respon Gubernur." },
    { "en": "Apa Itu 'Load Frequency Control' (Kendali Frekuensi Beban)?", "id": "Sistem Penjaga Frekuensi Jaringan Stabil." },
    { "en": "Apa Itu 'Tie-Line Bias Control' (Kendali Bias Jalur Ikat)?", "id": "Pengaturan Aliran Daya Antar Wilayah." },
    { "en": "Apa Itu 'Economic Dispatch' (Penyaluran Ekonomis)?", "id": "Optimasi Biaya Produksi Antar Pembangkit." },
    { "en": "Apa Itu 'Unit Commitment' (Komitmen Unit)?", "id": "Penentuan Jadwal Operasi Unit Pembangkit." },
    { "en": "Apa Itu 'Optimal Power Flow' (Aliran Daya Optimal)?", "id": "Analisis Aliran Daya Dengan Optimasi." },
    { "en": "Apa Itu 'Contingency Analysis' (Analisis Kontinjensi)?", "id": "Uji Ketahanan Sistem Terhadap Kegagalan." },
    { "en": "Apa Itu 'N-1 Criterion' (Kriteria N-1)?", "id": "Sistem Aman Jika Satu Komponen." },
    { "en": "Apa Itu 'Voltage Collapse' (Kolaps Tegangan)?", "id": "Kondisi Penurunan Tegangan Tak Terkendali." },
    { "en": "Apa Itu 'P-V Curve' (Kurva P-V)?", "id": "Hubungan Daya Aktif Terhadap Tegangan." },
    { "en": "Apa Itu 'Q-V Curve' (Kurva Q-V)?", "id": "Hubungan Daya Reaktif Terhadap Tegangan." },
    { "en": "Apa Itu 'Loadability Limit' (Batas Kemampuan Beban)?", "id": "Kapasitas Maksimal Jalur Menyalurkan Daya." },
    { "en": "Apa Itu 'Power System Stabilizer' (Stabilizer Sistem Daya)?", "id": "Alat Peredam Ayunan Daya Generator." },
    { "en": "Apa Itu 'Isotropic Antenna' (Antena Isotropik)?", "id": "Antena Pemancar Segala Arah Ideal." },
    { "en": "Apa Itu 'Dipole Antenna' (Antena Dipol)?", "id": "Antena Dengan Dua Lengan Konduktor." },
    { "en": "Apa Itu 'Monopole Antenna' (Antena Monopol)?", "id": "Antena Dengan Satu Lengan Konduktor." },
    { "en": "Apa Itu 'Yagi-Uda Antenna' (Antena Yagi-Uda)?", "id": "Antena Pengarah Dengan Elemen Pasif." },
    { "en": "Apa Itu 'Parabolic Antenna' (Antena Parabola)?", "id": "Antena Dengan Reflektor Berbentuk Parabola." },
    { "en": "Apa Itu 'Horn Antenna' (Antena Tanduk)?", "id": "Antena Berbentuk Pemandu Gelombang Melebar." },
    { "en": "Apa Itu 'Patch Antenna' (Antena Tambal)?", "id": "Antena Tipis Di Atas Dielektrik." },
    { "en": "Apa Itu 'Helical Antenna' (Antena Heliks)?", "id": "Antena Berbentuk Lilitan Kawat Spiral." },
    { "en": "Apa Itu 'Log-Periodic Antenna' (Antena Log-Periodik)?", "id": "Antena Dengan Pita Frekuensi Lebar." },
    { "en": "Apa Itu 'Phased Array' (Larik Terfase)?", "id": "Kumpulan Antena Pengarah Berkas Elektronik." },
    { "en": "Apa Itu 'Radiation Pattern' (Pola Radiasi)?", "id": "Representasi Grafis Distribusi Energi Antena." },
    { "en": "Apa Itu 'Antenna Gain' (Penguatan Antena)?", "id": "Efektivitas Pengarahan Daya Ke Titik." },
    { "en": "Apa Itu 'Beamwidth' (Lebar Berkas)?", "id": "Sudut Pancaran Utama Sinyal Antena." },
    { "en": "Apa Itu 'Main Lobe' (Cuping Utama)?", "id": "Arah Pancaran Daya Terbesar Antena." },
    { "en": "Apa Itu 'Side Lobe' (Cuping Samping)?", "id": "Pancaran Daya Tidak Diinginkan Antena." },
    { "en": "Apa Itu 'Back Lobe' (Cuping Belakang)?", "id": "Pancaran Daya Ke Arah Belakang." },
    { "en": "Apa Itu 'Directivity' (Direktivitas)?", "id": "Kemampuan Antena Memusatkan Radiasi Cahaya." },
    { "en": "Apa Itu 'Front-To-Back Ratio' (Rasio Depan-Belakang)?", "id": "Perbandingan Daya Depan Dan Belakang." },
    { "en": "Apa Itu 'Effective Aperture' (Bukaan Efektif)?", "id": "Luas Area Penangkapan Daya Antena." },
    { "en": "Apa Itu 'Polarization' (Polarisasi Antena)?", "id": "Arah Orientasi Medan Listrik Gelombang." },
    { "en": "Apa Itu 'Linear Polarization' (Polarisasi Linear)?", "id": "Medan Listrik Bergetar Satu Garis." },
    { "en": "Apa Itu 'Circular Polarization' (Polarisasi Melingkar)?", "id": "Medan Listrik Berputar Secara Melingkar." },
    { "en": "Apa Itu 'Elliptical Polarization' (Polarisasi Elips)?", "id": "Medan Listrik Berputar Bentuk Elips." },
    { "en": "Apa Itu 'Cross Polarization' (Polarisasi Silang)?", "id": "Komponen Polarisasi Yang Tidak Diinginkan." },
    { "en": "Apa Itu 'Impedance Matching' (Penyesuaian Impedansi)?", "id": "Maksimalisasi Transfer Daya Jalur Transmisi." },
    { "en": "Apa Itu 'VSWR' (Voltage Standing Wave Ratio)?", "id": "Rasio Gelombang Berdiri Tegangan Jalur." },
    { "en": "Apa Itu 'Reflection Coefficient' (Koefisien Refleksi)?", "id": "Rasio Gelombang Pantul Terhadap Datang." },
    { "en": "Apa Itu 'Return Loss' (Rugi Balik)?", "id": "Daya Hilang Akibat Pantulan Sinyal." },
    { "en": "Apa Itu 'Smith Chart' (Diagram Smith)?", "id": "Alat Grafis Analisis Impedansi Radio." },
    { "en": "Apa Itu 'Waveguide' (Pemandu Gelombang)?", "id": "Struktur Logam Penyalur Gelombang Mikro." },
    { "en": "Apa Itu 'Microstrip Line' (Jalur Mikrostrip)?", "id": "Jalur Transmisi Di Atas PCB." },
    { "en": "Apa Itu 'Stripline' (Jalur Stripline)?", "id": "Jalur Transmisi Di Dalam PCB." },
    { "en": "Apa Itu 'Coaxial Cable' (Kabel Koaksial)?", "id": "Kabel Dengan Dua Penghantar Seperpusat." },
    { "en": "Apa Itu 'Attenuation' (Redaman Sinyal)?", "id": "Penurunan Kekuatan Sinyal Selama Merambat." },
    { "en": "Apa Itu 'Cut-Off Frequency' (Frekuensi Pisah)?", "id": "Batas Frekuensi Terendah Pemandu Gelombang." },
    { "en": "Apa Itu 'Mode TE' (Transverse Electric Mode)?", "id": "Medan Listrik Tegak Lurus Arah." },
    { "en": "Apa Itu 'Mode TM' (Transverse Magnetic Mode)?", "id": "Medan Magnet Tegak Lurus Arah." },
    { "en": "Apa Itu 'Mode TEM' (Transverse Electromagnetic Mode)?", "id": "Kedua Medan Tegak Lurus Arah." },
    { "en": "Apa Itu 'Isolator' (Isolator Radio)?", "id": "Perangkat Penyalur Sinyal Satu Arah." },
    { "en": "Apa Itu 'Circulator' (Sirkulator)?", "id": "Perangkat Penyalur Sinyal Port Berurutan." },
    { "en": "Apa Itu 'Directional Coupler' (Coupler Pengarah)?", "id": "Alat Pencuplik Daya Sinyal Terarah." },
    { "en": "Apa Itu 'Power Divider' (Pembagi Daya)?", "id": "Alat Pembagi Sinyal Menjadi Beberapa." },
    { "en": "Apa Itu 'Mixer' (Pencampur Sinyal)?", "id": "Alat Pengubah Frekuensi Sinyal Radio." },
    { "en": "Apa Itu 'Local Oscillator' (Osilator Lokal)?", "id": "Pembangkit Frekuensi Referensi Dalam Mixer." },
    { "en": "Apa Itu 'Up-Converter' (Konverter Naik)?", "id": "Alat Peningkat Frekuensi Sinyal Radio." },
    { "en": "Apa Itu 'Down-Converter' (Konverter Turun)?", "id": "Alat Penurun Frekuensi Sinyal Radio." },
    { "en": "Apa Itu 'LNA' (Low Noise Amplifier)?", "id": "Penguat Sinyal Lemah Minim Derau." },
    { "en": "Apa Itu 'Power Amplifier' (Penguat Daya)?", "id": "Penguat Sinyal Sebelum Dikirim Antena." },
    { "en": "Apa Itu 'Noise Figure' (Angka Derau)?", "id": "Ukuran Degradasi Sinyal Oleh Perangkat." },
    { "en": "Apa Itu 'Noise Temperature' (Suhu Derau)?", "id": "Representasi Derau Dalam Satuan Kelvin." },
    { "en": "Apa Itu 'P1dB' (1dB Compression Point)?", "id": "Batas Linearitas Penguatan Sebuah Perangkat." },
    { "en": "Apa Itu 'IIP3' (Third-Order Input Intercept Point)?", "id": "Ukuran Distorsi Intermodulasi Perangkat Radio." },
    { "en": "Apa Itu 'Dynamic Range' (Rentang Dinamis)?", "id": "Selisih Sinyal Maksimum Dan Minimum." },
    { "en": "Apa Itu 'Bandpass Filter' (Filter Lolos Pita)?", "id": "Penyaring Sinyal Dalam Rentang Tertentu." },
    { "en": "Apa Itu 'Lowpass Filter' (Filter Lolos Rendah)?", "id": "Penyaring Sinyal Di Bawah Batas." },
    { "en": "Apa Itu 'Highpass Filter' (Filter Lolos Tinggi)?", "id": "Penyaring Sinyal Di Atas Batas." },
    { "en": "Apa Itu 'Bandstop Filter' (Filter Tolak Pita)?", "id": "Penghambat Sinyal Dalam Rentang Tertentu." },
    { "en": "Apa Itu 'Group Delay' (Tunda Kelompok)?", "id": "Waktu Rambat Sinyal Melalui Filter." },
    { "en": "Apa Itu 'Phase Linearity' (Linearitas Fase)?", "id": "Konsistensi Perubahan Fase Terhadap Frekuensi." },
    { "en": "Apa Itu 'Cavity Filter' (Filter Rongga)?", "id": "Filter Frekuensi Radio Berbasis Rongga." },
    { "en": "Apa Itu 'SAW Filter' (Surface Acoustic Wave Filter)?", "id": "Filter Berbasis Gelombang Suara Permukaan." },
    { "en": "Apa Itu 'Crystal Filter' (Filter Kristal)?", "id": "Filter Dengan Selektivitas Frekuensi Tinggi." },
    { "en": "Apa Itu 'Spectrum Analyzer' (Penganalisis Spektrum)?", "id": "Alat Visualisasi Amplitudo Terhadap Frekuensi." },
    { "en": "Apa Itu 'Vector Network Analyzer' (Penganalisis Jaringan Vektor)?", "id": "Alat Ukur Parameter Jaringan Radio." },
    { "en": "Apa Itu 'Signal Generator' (Generator Sinyal)?", "id": "Pembangkit Sinyal Radio Untuk Pengujian." },
    { "en": "Apa Itu 'Frequency Counter' (Pencacah Frekuensi)?", "id": "Alat Ukur Frekuensi Sinyal Presisi." },
    { "en": "Apa Itu 'Power Meter' (Meter Daya Radio)?", "id": "Alat Ukur Daya Sinyal Frekuensi." },
    { "en": "Apa Itu 'Anechoic Chamber' (Ruang Tanpa Gema)?", "id": "Ruang Khusus Pengujian Radiasi Antena." },
    { "en": "Apa Itu 'Near Field' (Medan Dekat)?", "id": "Daerah Radiasi Sangat Dekat Antena." },
    { "en": "Apa Itu 'Far Field' (Medan Jauh)?", "id": "Daerah Radiasi Berkas Sejajar Antena." },
    { "en": "Apa Itu 'Fraunhofer Distance' (Jarak Fraunhofer)?", "id": "Batas Mulainya Daerah Medan Jauh." },
    { "en": "Apa Itu 'Fresnel Zone' (Zona Fresnel)?", "id": "Daerah Elips Rambatan Gelombang Radio." },
    { "en": "Apa Itu 'Link Budget' (Anggaran Tautan)?", "id": "Perhitungan Total Daya Sistem Komunikasi." },
    { "en": "Apa Itu 'Path Loss' (Rugi Lintasan)?", "id": "Redaman Sinyal Akibat Jarak Propagasi." },
    { "en": "Apa Itu 'Fading' (Memudar)?", "id": "Variasi Kekuatan Sinyal Akibat Lingkungan." },
    { "en": "Apa Itu 'Multipath' (Lintasan Jamak)?", "id": "Sinyal Sampai Melalui Banyak Jalur." },
    { "en": "Apa Itu 'Doppler Effect' (Efek Doppler)?", "id": "Perubahan Frekuensi Akibat Gerakan Relatif." },
    { "en": "Apa Itu 'EVM' (Error Vector Magnitude)?", "id": "Ukuran Kualitas Modulasi Sinyal Digital." },
    { "en": "Apa Itu 'BER' (Bit Error Rate)?", "id": "Rasio Kesalahan Bit Saat Transmisi." },
    { "en": "Apa Itu 'SNR' (Signal-To-Noise Ratio)?", "id": "Perbandingan Kekuatan Sinyal Dan Derau." },
    { "en": "Apa Itu 'Effective Isotropic Radiated Power' (Daya Pancar Isotropik)?", "id": "Total Daya Yang Dipancarkan Antena." },
    { "en": "Apa Itu 'Radar Cross Section' (Penampang Radar)?", "id": "Ukuran Kemampuan Objek Memantulkan Radar." },
    { "en": "Apa Itu 'Duplexer' (Duplekser Radio)?", "id": "Alat Pembagi Jalur Kirim Terima." },
    { "en": "Apa Itu 'Diplexer' (Diplekser Radio)?", "id": "Pemisah Dua Frekuensi Berbeda Antena." },
    { "en": "Apa Itu 'Bias Tee' (Bias Tee)?", "id": "Komponen Injeksi Arus DC Jalur." },
    { "en": "Apa Itu 'Attenuator' (Peredam Sinyal)?", "id": "Komponen Penurun Daya Sinyal Radio." },
    { "en": "Apa Itu 'Phase Shifter' (Penggeser Fase Radio)?", "id": "Komponen Pengubah Sudut Fase Sinyal." },
    { "en": "Apa Itu 'RF Switch' (Saklar Radio)?", "id": "Saklar Pengalih Jalur Sinyal Frekuensi." },
    { "en": "Apa Itu 'Limiters' (Pembatas Radio)?", "id": "Pelindung Penerima Dari Daya Berlebih." },
    { "en": "Apa Itu 'Synthesizer' (Sintesis Frekuensi)?", "id": "Pembangkit Frekuensi Radio Terkendali Digital." },
    { "en": "Apa Itu 'Phase Noise' (Derau Fase)?", "id": "Ketidakstabilan Frekuensi Sinyal Dalam Domain." },
    { "en": "Apa Itu 'Jitter' (Guncangan Waktu)?", "id": "Variasi Waktu Sinyal Dalam Digital." },
    { "en": "Apa Itu 'Harmonic Distortion' (Distorsi Harmonisa)?", "id": "Munculnya Frekuensi Kelipatan Sinyal Asli." },
    { "en": "Apa Itu 'Intermodulation' (Intermodulasi)?", "id": "Munculnya Sinyal Akibat Campuran Frekuensi." },
    { "en": "Apa Itu 'Spurious Emission' (Emisi Palsu)?", "id": "Pancaran Sinyal Di Luar Pita." },
    { "en": "Apa Itu 'Spectrum Mask' (Masker Spektrum)?", "id": "Batas Emisi Frekuensi Yang Diizinkan." },
    { "en": "Apa Itu 'Isolation' (Isolasi Antar Port)?", "id": "Ukuran Pemisahan Sinyal Antar Jalur." },
    { "en": "Apa Itu 'VSAT' (Very Small Aperture Terminal)?", "id": "Stasiun Bumi Satelit Antena Kecil." },
    { "en": "Apa Itu 'Luminous Flux' (Fluks Cahaya)?", "id": "Total Cahaya Keluar Dari Sumber." },
    { "en": "Apa Itu 'Luminous Intensity' (Intensitas Cahaya)?", "id": "Kekuatan Cahaya Ke Arah Tertentu." },
    { "en": "Apa Itu 'Illuminance' (Iluminansi Cahaya)?", "id": "Kerapatan Fluks Cahaya Pada Permukaan." },
    { "en": "Apa Itu 'Luminance' (Luminansi Cahaya)?", "id": "Kecerahan Cahaya Dari Suatu Luasan." },
    { "en": "Apa Itu 'Efficacy' (Efikasi Lampu)?", "id": "Rasio Output Lumen Terhadap Watt." },
    { "en": "Apa Itu 'Beam Angle' (Sudut Berkas)?", "id": "Sudut Penyebaran Cahaya Utama Lampu." },
    { "en": "Apa Itu 'Color Temperature' (Suhu Warna)?", "id": "Warna Cahaya Berdasarkan Skala Kelvin." },
    { "en": "Apa Itu 'Warm White' (Putih Hangat)?", "id": "Cahaya Kuning Di Bawah Tiga-Ribu." },
    { "en": "Apa Itu 'Cool White' (Putih Dingin)?", "id": "Cahaya Putih Dengan Sedikit Biru." },
    { "en": "Apa Itu 'Daylight' (Cahaya Siang)?", "id": "Cahaya Terang Menyerupai Matahari Siang." },
    { "en": "Apa Itu 'CRI' (Color Rendering Index)?", "id": "Indeks Akurasi Penampilan Warna Alami." },
    { "en": "Apa Itu 'Glare' (Kesilauan Cahaya)?", "id": "Gangguan Penglihatan Akibat Cahaya Berlebih." },
    { "en": "Apa Itu 'Discomfort Glare' (Kesilauan Tidak Nyaman)?", "id": "Kesilauan Yang Menyebabkan Ketidaknyamanan Mata." },
    { "en": "Apa Itu 'Disability Glare' (Kesilauan Disabilitas)?", "id": "Kesilauan Yang Mengurangi Kemampuan Melihat." },
    { "en": "Apa Itu 'UGR' (Unified Glare Rating)?", "id": "Indeks Penilaian Tingkat Kesilauan Ruangan." },
    { "en": "Apa Itu 'Spacing Criterion' (Kriteria Jarak)?", "id": "Rasio Jarak Antar Lampu Maksimum." },
    { "en": "Apa Itu 'Maintenance Factor' (Faktor Pemeliharaan)?", "id": "Penyusutan Cahaya Akibat Usia, Debu." },
    { "en": "Apa Itu 'Utilization Factor' (Faktor Utilisasi)?", "id": "Rasio Cahaya Mencapai Bidang Kerja." },
    { "en": "Apa Itu 'Working Plane' (Bidang Kerja)?", "id": "Permukaan Tempat Aktivitas Cahaya Utama." },
    { "en": "Apa Itu 'Point-By-Point Method' (Metode Titik)?", "id": "Perhitungan Iluminansi Pada Titik Spesifik." },
    { "en": "Apa Itu 'Lumen Method' (Metode Lumen)?", "id": "Perhitungan Jumlah Lampu Rata-Rata Ruangan." },
    { "en": "Apa Itu 'Indirect Lighting' (Pencahayaan Tidak Langsung)?", "id": "Cahaya Memantul Dinding Sebelum Objek." },
    { "en": "Apa Itu 'Direct Lighting' (Pencahayaan Langsung)?", "id": "Cahaya Langsung Menuju Bidang Kerja." },
    { "en": "Apa Itu 'Task Lighting' (Pencahayaan Tugas)?", "id": "Cahaya Khusus Untuk Pekerjaan Tertentu." },
    { "en": "Apa Itu 'Accent Lighting' (Pencahayaan Aksen)?", "id": "Cahaya Penonjol Objek Estetika Tertentu." },
    { "en": "Apa Itu 'Ambient Lighting' (Pencahayaan Ambien)?", "id": "Pencahayaan Umum Seluruh Area Ruangan." },
    { "en": "Apa Itu 'Emergency Lighting' (Pencahayaan Darurat)?", "id": "Lampu Aktif Saat Listrik Utama Padam." },
    { "en": "Apa Itu 'Maintained Emergency Lighting' (Darurat Berlanjut)?", "id": "Lampu Menyala Dalam Segala Kondisi." },
    { "en": "Apa Itu 'Non-Maintained Emergency Lighting' (Darurat Sesaat)?", "id": "Lampu Hanya Menyala Saat Padam." },
    { "en": "Apa Itu 'Stroboscopic Effect' (Efek Stroboskopik)?", "id": "Ilusi Gerak Akibat Kedipan Cahaya." },
    { "en": "Apa Itu 'Flicker' (Kedipan Cahaya)?", "id": "Variasi Cepat Intensitas Cahaya Lampu." },
    { "en": "Apa Itu 'Photometry' (Fotometri)?", "id": "Ilmu Pengukuran Cahaya Tampak Mata." },
    { "en": "Apa Itu 'Goniophotometer' (Goniofotometer)?", "id": "Alat Ukur Distribusi Cahaya Lampu." },
    { "en": "Apa Itu 'Integrating Sphere' (Bola Integrasi)?", "id": "Alat Ukur Total Lumen Lampu." },
    { "en": "Apa Itu 'Polar Curve' (Kurva Polar)?", "id": "Grafik Distribusi Intensitas Cahaya Sumber." },
    { "en": "Apa Itu 'Isolux Contour' (Kontur Isolux)?", "id": "Garis Hubung Titik Iluminansi Sama." },
    { "en": "Apa Itu 'Inverse Square Law' (Hukum Kuadrat Terbalik)?", "id": "Cahaya Berkurang Terhadap Kuadrat Jarak." },
    { "en": "Apa Itu 'Cosine Law' (Hukum Kosinus)?", "id": "Iluminansi Tergantung Sudut Datang Cahaya." },
    { "en": "Apa Itu 'Solid Angle' (Sudut Ruang)?", "id": "Ukuran Tiga Dimensi Pancaran Cahaya." },
    { "en": "Apa Itu 'Steradian'?", "id": "Satuan Ukuran Untuk Sudut Ruang." },
    { "en": "Apa Itu 'Ballast' (Balas Magnetik)?", "id": "Pembatas Arus Lampu Pelepasan Gas." },
    { "en": "Apa Itu 'Electronic Ballast' (Balas Elektronik)?", "id": "Pengatur Arus Lampu Frekuensi Tinggi." },
    { "en": "Apa Itu 'Ignitor' (Pematik Lampu)?", "id": "Pemicu Tegangan Tinggi Awal Lampu." },
    { "en": "Apa Itu 'Capacitor' (Kapasitor Lampu)?", "id": "Komponen Perbaikan Faktor Daya Lampu." },
    { "en": "Apa Itu 'High Pressure Sodium' (Natrium Tekanan Tinggi)?", "id": "Lampu Efisiensi Tinggi Spektrum Kuning." },
    { "en": "Apa Itu 'Low Pressure Sodium' (Natrium Tekanan Rendah)?", "id": "Lampu Monokromatik Kuning Sangat Efisien." },
    { "en": "Apa Itu 'Metal Halide' (Halida Logam)?", "id": "Lampu Pelepasan Gas Spektrum Putih." },
    { "en": "Apa Itu 'Mercury Vapor' (Uap Merkuri)?", "id": "Lampu Pelepasan Gas Tekanan Tinggi." },
    { "en": "Apa Itu 'Induction Lamp' (Lampu Induksi)?", "id": "Lampu Tanpa Elektroda Umur Panjang." },
    { "en": "Apa Itu 'LED Driver' (Driver LED)?", "id": "Penyuplai Arus Konstan Chip LED." },
    { "en": "Apa Itu 'Heat Sink' (Pendingin LED)?", "id": "Alat Pembuang Panas Chip LED." },
    { "en": "Apa Itu 'Junction Temperature' (Suhu Sambungan LED)?", "id": "Suhu Internal Chip LED Aktif." },
    { "en": "Apa Itu 'L70 Rating' (Peringkat L70)?", "id": "Waktu Cahaya LED Turun Tujuh-Puluh-Persen." },
    { "en": "Apa Itu 'Binning' (Klasifikasi LED)?", "id": "Pengelompokan LED Berdasarkan Warna, Kecerahan." },
    { "en": "Apa Itu 'MacAdam Ellipse' (Elips MacAdam)?", "id": "Ukuran Konsistensi Warna Cahaya LED." },
    { "en": "Apa Itu 'SDCM' (Standard Deviation Of Color Matching)?", "id": "Ukuran Variasi Warna Sesuai Elips." },
    { "en": "Apa Itu 'Reflector' (Reflektor Cahaya)?", "id": "Alat Pemantul Cahaya Ke Arah." },
    { "en": "Apa Itu 'Diffuser' (Difuser Cahaya)?", "id": "Alat Penyebar Cahaya Menjadi Lembut." },
    { "en": "Apa Itu 'Louver' (Kisi-Kisi Cahaya)?", "id": "Penghalang Cahaya Pencegah Kesilauan Mata." },
    { "en": "Apa Itu 'Lens' (Lensa Optik)?", "id": "Alat Pembias Cahaya Fokus Tertentu." },
    { "en": "Apa Itu 'IP Rating' (Ingress Protection Rating)?", "id": "Standar Ketahanan Lampu Terhadap Lingkungan." },
    { "en": "Apa Itu 'Floodlight' (Lampu Sorot)?", "id": "Lampu Pencahayaan Luar Area Luas." },
    { "en": "Apa Itu 'Spotlight' (Lampu Fokus)?", "id": "Lampu Dengan Berkas Cahaya Sempit." },
    { "en": "Apa Itu 'Street Lighting' (Lampu Jalan)?", "id": "Pencahayaan Khusus Area Lintasan Kendaraan." },
    { "en": "Apa Itu 'Tunnel Lighting' (Lampu Terowongan)?", "id": "Pencahayaan Khusus Jalur Bawah Tanah." },
    { "en": "Apa Itu 'Aviation Obstruction Light' (Lampu Rintangan Penerbangan)?", "id": "Lampu Penanda Bangunan Tinggi Pesawat." },
    { "en": "Apa Itu 'Searchlight' (Lampu Pencari)?", "id": "Lampu Pancaran Cahaya Sangat Jauh." },
    { "en": "Apa Itu 'Light Pollution' (Polusi Cahaya)?", "id": "Cahaya Buatan Yang Mengganggu Langit." },
    { "en": "Apa Itu 'Sky Glow' (Pijar Langit)?", "id": "Cahaya Tersebar Di Atmosfer Malam." },
    { "en": "Apa Itu 'Light Trespass' (Pelampauan Cahaya)?", "id": "Cahaya Masuk Ke Area Pribadi." },
    { "en": "Apa Itu 'Daylight Harvesting' (Pemanenan Cahaya Siang)?", "id": "Sistem Penghemat Energi Cahaya Matahari." },
    { "en": "Apa Itu 'Smart Lighting' (Pencahayaan Cerdas)?", "id": "Sistem Lampu Terkendali Otomatis Digital." },
    { "en": "Apa Itu 'Occupancy Sensor' (Sensor Keberadaan)?", "id": "Saklar Otomatis Berdasarkan Gerakan Manusia." },
    { "en": "Apa Itu 'Photocell' (Sel Foto)?", "id": "Saklar Otomatis Berdasarkan Cahaya Sekitar." },
    { "en": "Apa Itu 'DALI' (Digital Addressable Lighting Interface)?", "id": "Protokol Komunikasi Kendali Lampu Digital." },
    { "en": "Apa Itu 'DMX512' (Digital Multiplex 512)?", "id": "Standar Kendali Lampu Panggung Hiburan." },
    { "en": "Apa Itu 'KNX' (Standar Otomasi Gedung)?", "id": "Protokol Kendali Pintar Fasilitas Gedung." },
    { "en": "Apa Itu 'Wireless Lighting Control' (Kendali Lampu Nirkabel)?", "id": "Sistem Kontrol Lampu Tanpa Kabel." },
    { "en": "Apa Itu 'Zigbee' (Protokol Nirkabel Zigbee)?", "id": "Jaringan Nirkabel Hemat Energi Industri." },
    { "en": "Apa Itu 'Li-Fi' (Light Fidelity)?", "id": "Komunikasi Data Nirkabel Melalui Cahaya." },
    { "en": "Apa Itu 'UVC Lamp' (Lampu UVC)?", "id": "Lampu Ultraviolet Untuk Sterilisasi Kuman." },
    { "en": "Apa Itu 'Germicidal Lamp' (Lampu Kuman)?", "id": "Lampu Pembunuh Bakteri Melalui Radiasi." },
    { "en": "Apa Itu 'Blacklight' (Lampu Hitam)?", "id": "Lampu Ultraviolet Gelombang Panjang Efek." },
    { "en": "Apa Itu 'Infrared Lamp' (Lampu Inframerah)?", "id": "Lampu Penghasil Radiasi Panas Tak-Tampak." },
    { "en": "Apa Itu 'Xenon Lamp' (Lampu Xenon)?", "id": "Lampu Gas Terang Untuk Proyektor." },
    { "en": "Apa Itu 'Neon Sign' (Tanda Neon)?", "id": "Lampu Dekorasi Tabung Berisi Gas." },
    { "en": "Apa Itu 'Cold Cathode' (Katoda Dingin)?", "id": "Lampu Pelepasan Tanpa Pemanasan Filamen." },
    { "en": "Apa Itu 'CCFL' (Cold Cathode Fluorescent Lamp)?", "id": "Lampu Latar Layar LCD Lama." },
    { "en": "Apa Itu 'OLED' (Organic Light Emitting Diode)?", "id": "Layar Organik Pemancar Cahaya Mandiri." },
    { "en": "Apa Itu 'Quantum Dot' (Titik Kuantum)?", "id": "Nanokristal Peningkat Kualitas Warna Layar." },
    { "en": "Apa Itu 'Edge-Lit' (Lampu Samping)?", "id": "Pencahayaan Layar Dari Bagian Tepi." },
    { "en": "Apa Itu 'Back-Lit' (Lampu Belakang)?", "id": "Pencahayaan Layar Dari Bagian Belakang." },
    { "en": "Apa Itu 'Local Dimming' (Peredupan Lokal)?", "id": "Teknik Kontrol Kontras Gambar Layar." },
    { "en": "Apa Itu 'PWM Dimming' (Peredupan PWM)?", "id": "Peredupan Melalui Pengaturan Lebar Pulsa." },
    { "en": "Apa Itu 'Analog Dimming' (Peredupan Analog)?", "id": "Peredupan Melalui Pengaturan Nilai Arus." },
    { "en": "Apa Itu 'Candela' (Kandela)?", "id": "Satuan Dasar Intensitas Cahaya Internasional." },
    { "en": "Apa Itu 'Foot-Candle' (Kandela-Kaki)?", "id": "Satuan Iluminansi Standar Amerika Serikat." },
    { "en": "Apa Itu 'Lambert' (Lambert)?", "id": "Satuan Ukuran Untuk Luminansi Cahaya." },
    { "en": "Apa Itu 'Nit' (Nit)?", "id": "Satuan Luminansi Per Meter Persegi." },
    { "en": "Apa Itu 'ATS' (Automatic Transfer Switch)?", "id": "Saklar Perpindahan Daya Listrik Otomatis." },
    { "en": "Apa Itu 'AMF' (Automatic Mains Failure)?", "id": "Sistem Menyalakan Genset Saat Padam." },
    { "en": "Apa Itu 'NGR' (Neutral Grounding Resistor)?", "id": "Hambatan Pembatas Arus Gangguan Tanah." },
    { "en": "Apa Itu 'Standby Power' (Daya Cadangan)?", "id": "Daya Siaga Saat Sumber Utama." },
    { "en": "Apa Itu 'Prime Power' (Daya Utama)?", "id": "Daya Operasi Terus-Menerus Beban Bervariasi." },
    { "en": "Apa Itu 'Continuous Power' (Daya Kontinu)?", "id": "Daya Konstan Tanpa Batas Waktu." },
    { "en": "Apa Itu 'Load Bank' (Bank Beban)?", "id": "Alat Simulasi Beban Uji Genset." },
    { "en": "Apa Itu 'Synchronizing Panel' (Panel Sinkron)?", "id": "Panel Penggabung Dua Sumber Listrik." },
    { "en": "Apa Itu 'Dark Start' (Mulai Gelap)?", "id": "Proses Start Pembangkit Tanpa Jaringan." },
    { "en": "Apa Itu 'Island Operation' (Operasi Pulau)?", "id": "Pembangkit Bekerja Terpisah Jaringan Utama." },
    { "en": "Apa Itu 'ROCOF' (Rate Of Change Of Frequency)?", "id": "Laju Perubahan Frekuensi Jaringan Listrik." },
    { "en": "Apa Itu 'UFLS' (Under Frequency Load Shedding)?", "id": "Pelepasan Beban Akibat Frekuensi Rendah." },
    { "en": "Apa Itu 'UVLS' (Under Voltage Load Shedding)?", "id": "Pelepasan Beban Akibat Tegangan Rendah." },
    { "en": "Apa Itu 'Vector Shift' (Pergeseran Vektor)?", "id": "Metode Deteksi Terlepasnya Jaringan Utama." },
    { "en": "Apa Itu 'Change Over Switch' (Saklar Pindah)?", "id": "Alat Pemindah Aliran Antar Sumber." },
    { "en": "Apa Itu 'Bus Tie' (Pengikat Rel)?", "id": "Penghubung Antar Dua Segmen Busbar." },
    { "en": "Apa Itu 'Air Circuit Breaker' (Pemutus Udara)?", "id": "Pemutus Arus Besar Media Udara." },
    { "en": "Apa Itu 'Vacuum Circuit Breaker' (Pemutus Vakum)?", "id": "Pemutus Arus Besar Media Vakum." },
    { "en": "Apa Itu 'Gas Circuit Breaker' (Pemutus Gas)?", "id": "Pemutus Arus Besar Media Gas." },
    { "en": "Apa Itu 'Oil Circuit Breaker' (Pemutus Minyak)?", "id": "Pemutus Arus Besar Media Minyak." },
    { "en": "Apa Itu 'Arc Chute' (Saluran Busur)?", "id": "Bagian Pemadam Busur Api Listrik." },
    { "en": "Apa Itu 'Short Circuit Current' (Arus Hubung Singkat)?", "id": "Aliran Arus Maksimal Saat Gangguan." },
    { "en": "Apa Itu 'Making Capacity' (Kapasitas Masuk)?", "id": "Kemampuan Breaker Menutup Arus Gangguan." },
    { "en": "Apa Itu 'Breaking Capacity' (Kapasitas Putus)?", "id": "Kemampuan Breaker Memutus Arus Gangguan." },
    { "en": "Apa Itu 'Closing Coil' (Kumparan Penutup)?", "id": "Elektromagnet Penggerak Penutupan Kontak Breaker." },
    { "en": "Apa Itu 'Shunt Trip' (Trip Shunt)?", "id": "Mekanisme Trip Breaker Melalui Sinyal." },
    { "en": "Apa Itu 'Under Voltage Trip' (Trip Tegangan Kurang)?", "id": "Mekanisme Trip Otomatis Tegangan Rendah." },
    { "en": "Apa Itu 'Auxiliary Contact' (Kontak Bantu)?", "id": "Kontak Tambahan Untuk Sinyal Kontrol." },
    { "en": "Apa Itu 'Interlock' (Saling Kunci)?", "id": "Sistem Keamanan Pencegah Operasi Bersamaan." },
    { "en": "Apa Itu 'Padlock' (Gembok Pengunci)?", "id": "Alat Pengunci Keamanan Fisik Breaker." },
    { "en": "Apa Itu 'Draw Out Breaker' (Breaker Tarik)?", "id": "Breaker Yang Bisa Dilepas Pasang." },
    { "en": "Apa Itu 'Fixed Breaker' (Breaker Tetap)?", "id": "Breaker Terpasang Tetap Pada Panel." },
    { "en": "Apa Itu 'Distribution Board' (Papan Distribusi)?", "id": "Panel Pembagi Energi Listrik Konsumen." },
    { "en": "Apa Itu 'Feeder Pillar' (Pilar Penyulang)?", "id": "Kotak Distribusi Luar Ruangan Kabel." },
    { "en": "Apa Itu 'Service Entrance' (Pintu Masuk Layanan)?", "id": "Titik Sambungan Listrik PLN Pelanggan." },
    { "en": "Apa Itu 'Metering Panel' (Panel Meteran)?", "id": "Panel Tempat Alat Ukur Energi." },
    { "en": "Apa Itu 'Control Transformer' (Transformator Kontrol)?", "id": "Transformator Penyuplai Arus Sirkuit Kendali." },
    { "en": "Apa Itu 'Power Factor' (Faktor Daya)?", "id": "Rasio Daya Aktif Terhadap Semu." },
    { "en": "Apa Itu 'Capacitor Step' (Tahap Kapasitor)?", "id": "Unit Kapasitor Dalam Panel Kompensasi." },
    { "en": "Apa Itu 'Detuned Reactor' (Reaktor Detuned)?", "id": "Pencegah Resonansi Harmonisa Pada Kapasitor." },
    { "en": "Apa Itu 'Active Filter' (Filter Aktif)?", "id": "Peredam Harmonisa Menggunakan Elektronika Daya." },
    { "en": "Apa Itu 'Passive Filter' (Filter Pasif)?", "id": "Peredam Harmonisa Menggunakan Komponen Pasif." },
    { "en": "Apa Itu 'Voltage Swell' (Tegangan Naik Sesaat)?", "id": "Kenaikan Tegangan Listrik Waktu Singkat." },
    { "en": "Apa Itu 'Voltage Sag' (Tegangan Turun Sesaat)?", "id": "Penurunan Tegangan Listrik Waktu Singkat." },
    { "en": "Apa Itu 'Voltage Flicker' (Kedipan Tegangan)?", "id": "Variasi Tegangan Berulang Cepat Mengganggu." },
    { "en": "Apa Itu 'Transient' (Tegangan Transien)?", "id": "Lonjakan Tegangan Sangat Singkat, Tajam." },
    { "en": "Apa Itu 'Notching' (Takikan Tegangan)?", "id": "Gangguan Tegangan Akibat Pensakelaran Komutasi." },
    { "en": "Apa Itu 'Phase Unbalance' (Ketidakseimbangan Fase)?", "id": "Perbedaan Beban Antar Fase Listrik." },
    { "en": "Apa Itu 'Neutral Overload' (Beban Lebih Netral)?", "id": "Arus Berlebih Pada Kabel Netral." },
    { "en": "Apa Itu 'Third Harmonic' (Harmonisa Ketiga)?", "id": "Gangguan Arus Frekuensi Seratus Limapuluh." },
    { "en": "Apa Itu 'Earth Leakage' (Kebocoran Tanah)?", "id": "Aliran Arus Tak Diinginkan Bumi." },
    { "en": "Apa Itu 'Ground Loop' (Loop Tanah)?", "id": "Arus Akibat Perbedaan Potensial Arde." },
    { "en": "Apa Itu 'Isolation Transformer' (Transformator Isolasi)?", "id": "Transformator Pemisah Sirkuit Dari Gangguan." },
    { "en": "Apa Itu 'Super Isolation Transformer'?", "id": "Transformator Dengan Perlindungan Gangguan Tinggi." },
    { "en": "Apa Itu 'Surge Protection Device' (Perangkat Proteksi Surja)?", "id": "Alat Pembuang Tegangan Lonjakan Tanah." },
    { "en": "Apa Itu 'Lightning Arrester' (Penangkap Petir)?", "id": "Alat Pelindung Gardu Dari Petir." },
    { "en": "Apa Itu 'Earthing Rod' (Batang Arde)?", "id": "Batang Logam Penyalur Arus Bumi." },
    { "en": "Apa Itu 'Grounding Grid' (Jala Arde)?", "id": "Jaringan Kabel Tanam Pembumian Luas." },
    { "en": "Apa Itu 'Bentonite' (Bentonit Tanah)?", "id": "Bahan Kimia Penurun Tahanan Tanah." },
    { "en": "Apa Itu 'Exothermic Welding' (Las Eksotermik)?", "id": "Penyambungan Kabel Arde Sistem Reaksi." },
    { "en": "Apa Itu 'Step Potential' (Potensial Langkah)?", "id": "Beda Tegangan Antara Dua Kaki." },
    { "en": "Apa Itu 'Touch Potential' (Potensial Sentuh)?", "id": "Beda Tegangan Antara Tangan, Kaki." },
    { "en": "Apa Itu 'Equipotential Bonding' (Ikatan Ekipotensial)?", "id": "Penyamaan Tegangan Seluruh Bagian Logam." },
    { "en": "Apa Itu 'Static Grounding' (Pentanahan Statis)?", "id": "Pembuangan Listrik Statis Ke Bumi." },
    { "en": "Apa Itu 'Anti Static Mat' (Keset Anti Statis)?", "id": "Alas Penghilang Muatan Listrik Statis." },
    { "en": "Apa Itu 'ESD' (Electrostatic Discharge)?", "id": "Pelepasan Muatan Listrik Statis Mendadak." },
    { "en": "Apa Itu 'Wrist Strap' (Tali Pergelangan)?", "id": "Kabel Grounding Tubuh Pekerja Elektronika." },
    { "en": "Apa Itu 'Shielding' (Pelindung Kabel)?", "id": "Lapisan Logam Penahan Gangguan Elektromagnetik." },
    { "en": "Apa Itu 'Twisted Pair' (Pasangan Berpilin)?", "id": "Kabel Berpilin Pengurang Induksi Magnetik." },
    { "en": "Apa Itu 'Ferrite Core' (Inti Ferit)?", "id": "Cincin Magnetik Penelan Gangguan Sinyal." },
    { "en": "Apa Itu 'Decibel' (Desibel)?", "id": "Satuan Logaritmik Perbandingan Kekuatan Sinyal." },
    { "en": "Apa Itu 'Signal To Noise Ratio' (Rasio Sinyal Derau)?", "id": "Perbandingan Antara Sinyal Dan Gangguan." },
    { "en": "Apa Itu 'Attenuation' (Redaman)?", "id": "Pengurangan Kekuatan Sinyal Jalur Kabel." },
    { "en": "Apa Itu 'Crosstalk' (Bicara Silang)?", "id": "Interferensi Sinyal Antar Kabel Berdekatan." },
    { "en": "Apa Itu 'Impedance' (Impedansi)?", "id": "Total Hambatan Arus Bolak Balik." },
    { "en": "Apa Itu 'Resistance' (Resistansi)?", "id": "Hambatan Aliran Arus Listrik Searah." },
    { "en": "Apa Itu 'Reactance' (Reaktansi)?", "id": "Hambatan Akibat Induktor Atau Kapasitor." },
    { "en": "Apa Itu 'Admittance' (Admitansi)?", "id": "Kebalikan Dari Nilai Impedansi Listrik." },
    { "en": "Apa Itu 'Conductance' (Konduktansi)?", "id": "Kebalikan Dari Nilai Resistansi Listrik." },
    { "en": "Apa Itu 'Susceptance' (Suseptansi)?", "id": "Kebalikan Dari Nilai Reaktansi Listrik." },
    { "en": "Apa Itu 'Phase Angle' (Sudut Fase)?", "id": "Selisih Waktu Tegangan Dan Arus." },
    { "en": "Apa Itu 'Vector Diagram' (Diagram Vektor)?", "id": "Representasi Grafis Nilai, Sudut Listrik." },
    { "en": "Apa Itu 'Phasor' (Fasor)?", "id": "Vektor Berputar Mewakili Sinyal Sinusoidal." },
    { "en": "Apa Itu 'Real Power' (Daya Nyata)?", "id": "Daya Listrik Menjadi Kerja Mekanik." },
    { "en": "Apa Itu 'Reactive Power' (Daya Reaktif)?", "id": "Daya Pembentuk Medan Magnet Listrik." },
    { "en": "Apa Itu 'Apparent Power' (Daya Semu)?", "id": "Total Daya Yang Dikirim Sumber." },
    { "en": "Apa Itu 'Voltage Drop' (Jatuh Tegangan)?", "id": "Kehilangan Tegangan Akibat Hambatan Kabel." },
    { "en": "Apa Itu 'Line Loss' (Rugi Saluran)?", "id": "Energi Listrik Terbuang Menjadi Panas." },
    { "en": "Apa Itu 'Skin Effect' (Efek Kulit)?", "id": "Arus Mengalir Di Permukaan Kabel." },
    { "en": "Apa Itu 'Proximity Effect' (Efek Kedekatan)?", "id": "Gangguan Arus Antar Kabel Dekat." },
    { "en": "Apa Itu 'Corona Effect' (Efek Korona)?", "id": "Ionisasi Udara Sekitar Kabel Tinggi." },
    { "en": "Apa Itu 'Dielectric Strength' (Kekuatan Dielektrik)?", "id": "Batas Tegangan Tahan Bahan Isolasi." },
    { "en": "Apa Itu 'Flashover' (Loncatan Api)?", "id": "Loncatan Listrik Melalui Permukaan Isolator." },
    { "en": "Apa Itu 'Puncture' (Tembus Listrik)?", "id": "Kegagalan Isolasi Menembus Bahan Padat." },
    { "en": "Apa Itu 'Tracking' (Jejak Listrik)?", "id": "Jalur Arus Merusak Permukaan Isolator." },
    { "en": "Apa Itu 'Creepage Distance' (Jarak Rambat)?", "id": "Jarak Terpendek Sepanjang Permukaan Isolasi." },
    { "en": "Apa Itu 'Clearance' (Jarak Bebas)?", "id": "Jarak Terpendek Melalui Udara Terbuka." },
    { "en": "Apa Itu 'Step Up' (Penaik Tegangan)?", "id": "Proses Menaikkan Level Tegangan Listrik." },
    { "en": "Apa Itu 'Step Down' (Penurun Tegangan)?", "id": "Proses Menurunkan Level Tegangan Listrik." },
    { "en": "Apa Itu 'ECG' (Electrocardiogram)?", "id": "Alat Ukur Aktivitas Listrik Jantung." },
    { "en": "Apa Itu 'EEG' (Electroencephalogram)?", "id": "Alat Ukur Aktivitas Listrik Otak." },
    { "en": "Apa Itu 'EMG' (Electromyogram)?", "id": "Alat Ukur Aktivitas Listrik Otot." },
    { "en": "Apa Itu 'MRI' (Magnetic Resonance Imaging)?", "id": "Pencitraan Medis Berbasis Medan Magnet." },
    { "en": "Apa Itu 'CT Scan' (Computed Tomography Scan)?", "id": "Pemindaian Tubuh Menggunakan Sinar-X Digital." },
    { "en": "Apa Itu 'AED' (Automated External Defibrillator)?", "id": "Alat Pacu Jantung Otomatis Darurat." },
    { "en": "Apa Itu 'TENS' (Transcutaneous Electrical Nerve Stimulation)?", "id": "Stimulasi Saraf Melalui Arus Listrik." },
    { "en": "Apa Itu 'GSU' (Generator Step-Up Transformer)?", "id": "Transformator Penaik Tegangan Utama Pembangkit." },
    { "en": "Apa Itu 'UAT' (Unit Auxiliary Transformer)?", "id": "Transformator Penyuplai Kebutuhan Internal Unit." },
    { "en": "Apa Itu 'RAT' (Reserve Auxiliary Transformer)?", "id": "Transformator Penyuplai Cadangan Internal Pembangkit." },
    { "en": "Apa Itu 'IED' (Intelligent Electronic Device)?", "id": "Perangkat Elektronik Pintar Sistem Proteksi." },
    { "en": "Apa Itu 'WAMS' (Wide Area Monitoring System)?", "id": "Sistem Pemantauan Jaringan Listrik Luas." },
    { "en": "Apa Itu 'PMU' (Phasor Measurement Unit)?", "id": "Alat Ukur Fasor Tegangan Cepat." },
    { "en": "Apa Itu 'PDC' (Phasor Data Concentrator)?", "id": "Pusat Pengumpul Data Dari PMU." },
    { "en": "Apa Itu 'SCADA' (Supervisory Control And Data Acquisition)?", "id": "Sistem Pengawasan Dan Akuisisi Data." },
    { "en": "Apa Itu 'HMI' (Human Machine Interface)?", "id": "Antarmuka Visual Pengguna Dan Mesin." },
    { "en": "Apa Itu 'DCS' (Distributed Control System)?", "id": "Sistem Kendali Proses Secara Terdistribusi." },
    { "en": "Apa Itu 'PLC' (Programmable Logic Controller)?", "id": "Kontroler Logika Terprogram Skala Industri." },
    { "en": "Apa Itu 'PID' (Proportional Integral Derivative)?", "id": "Algoritma Kendali Umpan Balik Kontinu." },
    { "en": "Apa Itu 'LQR' (Linear Quadratic Regulator)?", "id": "Metode Kendali Optimal Sistem Linear." },
    { "en": "Apa Itu 'MPC' (Model Predictive Control)?", "id": "Kendali Berbasis Prediksi Model Sistem." },
    { "en": "Apa Itu 'SISO' (Single Input Single Output)?", "id": "Sistem Dengan Satu Masukan Keluaran." },
    { "en": "Apa Itu 'MIMO' (Multiple Input Multiple Output)?", "id": "Sistem Banyak Masukan Dan Keluaran." },
    { "en": "Apa Itu 'BIBO' (Bounded Input Bounded Output)?", "id": "Kriteria Stabilitas Sistem Kendali Dasar." },
    { "en": "Apa Itu 'VCO' (Voltage Controlled Oscillator)?", "id": "Osilator Frekuensi Terkendali Tegangan Masuk." },
    { "en": "Apa Itu 'PLL' (Phase Locked Loop)?", "id": "Sirkuit Penjaga Keselarasan Fase Sinyal." },
    { "en": "Apa Itu 'PWM' (Pulse Width Modulation)?", "id": "Modulasi Lebar Pulsa Kendali Daya." },
    { "en": "Apa Itu 'PFM' (Pulse Frequency Modulation)?", "id": "Modulasi Frekuensi Pulsa Kendali Daya." },
    { "en": "Apa Itu 'SPWM' (Sinusoidal Pulse Width Modulation)?", "id": "Modulasi Pulsa Berbasis Gelombang Sinus." },
    { "en": "Apa Itu 'SVM' (Space Vector Modulation)?", "id": "Teknik Modulasi Vektor Ruang Digital." },
    { "en": "Apa Itu 'THD' (Total Harmonic Distortion)?", "id": "Persentase Total Gangguan Harmonisa Sinyal." },
    { "en": "Apa Itu 'TDD' (Total Demand Distortion)?", "id": "Distorsi Harmonisa Terhadap Arus Beban." },
    { "en": "Apa Itu 'PCC' (Point Of Common Coupling)?", "id": "Titik Sambung Antara Konsumen, Jaringan." },
    { "en": "Apa Itu 'UPS' (Uninterruptible Power Supply) Online?", "id": "Suplai Daya Melalui Inverter Kontinu." },
    { "en": "Apa Itu 'UPS' (Uninterruptible Power Supply) Offline?", "id": "Suplai Daya Cadangan Saat Padam." },
    { "en": "Apa Itu 'AVR' (Automatic Voltage Regulator)?", "id": "Sistem Penstabil Tegangan Keluaran Otomatis." },
    { "en": "Apa Itu 'PSS' (Power System Stabilizer)?", "id": "Alat Peredam Ayunan Daya Generator." },
    { "en": "Apa Itu 'LFC' (Load Frequency Control)?", "id": "Kendali Frekuensi Akibat Perubahan Beban." },
    { "en": "Apa Itu 'AGC' (Automatic Generation Control)?", "id": "Kendali Otomatis Output Daya Pembangkit." },
    { "en": "Apa Itu 'VAr' (Volt Ampere Reactive)?", "id": "Satuan Ukuran Untuk Daya Reaktif." },
    { "en": "Apa Itu 'VA' (Volt Ampere)?", "id": "Satuan Ukuran Untuk Daya Semu." },
    { "en": "Apa Itu 'Watt' (Satuan Daya Aktif)?", "id": "Satuan Ukuran Untuk Daya Nyata." },
    { "en": "Apa Itu 'Eddy Current' (Arus Eddy)?", "id": "Arus Induksi Merugi Dalam Besi." },
    { "en": "Apa Itu 'Hysteresis' (Histeresis Magnetik)?", "id": "Keterlambatan Magnetisasi Terhadap Medan Magnet." },
    { "en": "Apa Itu 'Proximity Effect' (Efek Kedekatan)?", "id": "Gangguan Arus Antar Konduktor Dekat." },
    { "en": "Apa Itu 'Ferranti Effect' (Efek Ferranti)?", "id": "Kenaikan Tegangan Di Ujung Transmisi." },
    { "en": "Apa Itu 'Corona Effect' (Efek Korona)?", "id": "Ionisasi Udara Sekitar Kabel Tegangan." },
    { "en": "Apa Itu 'Reluctance' (Reluktansi Magnetik)?", "id": "Hambatan Terhadap Aliran Fluks Magnet." },
    { "en": "Apa Itu 'Permeability' (Permeabilitas Magnetik)?", "id": "Kemampuan Bahan Menghantarkan Fluks Magnet." },
    { "en": "Apa Itu 'Induktansi Diri' (Self Inductance)?", "id": "Induksi Tegangan Pada Kumparan Sendiri." },
    { "en": "Apa Itu 'Induktansi Bersama' (Mutual Inductance)?", "id": "Induksi Tegangan Antar Dua Kumparan." },
    { "en": "Apa Itu 'Kapasitansi' (Capacitance)?", "id": "Kemampuan Menyimpan Muatan Listrik Kapasitor." },
    { "en": "Apa Itu 'Impedansi' (Impedance)?", "id": "Hambatan Total Arus Bolak Balik." },
    { "en": "Apa Itu 'Admitansi' (Admittance)?", "id": "Kebalikan Dari Nilai Impedansi Listrik." },
    { "en": "Apa Itu 'Konduktansi' (Conductance)?", "id": "Kebalikan Dari Nilai Resistansi Listrik." },
    { "en": "Apa Itu 'Suseptansi' (Susceptance)?", "id": "Kebalikan Dari Nilai Reaktansi Listrik." },
    { "en": "Apa Itu 'Reaktansi' (Reactance)?", "id": "Hambatan Akibat Sifat Induktif, Kapasitif." },
    { "en": "Apa Itu 'Fasor' (Phasor)?", "id": "Vektor Mewakili Sinyal Sinusoidal Listrik." },
    { "en": "Apa Itu 'Resonansi' (Resonance)?", "id": "Kondisi Reaktansi Induktif Kapasitif Sama." },
    { "en": "Apa Itu 'Q-Factor' (Quality Factor)?", "id": "Ukuran Kualitas Rangkaian Resonansi Listrik." },
    { "en": "Apa Itu 'Bandwidth' (Lebar Pita)?", "id": "Rentang Frekuensi Operasi Suatu Sistem." },
    { "en": "Apa Itu 'FFT' (Fast Fourier Transform)?", "id": "Algoritma Analisis Spektrum Frekuensi Cepat." },
    { "en": "Apa Itu 'ADC' (Analog To Digital Converter)?", "id": "Pengubah Sinyal Analog Menjadi Digital." },
    { "en": "Apa Itu 'DAC' (Digital To Analog Converter)?", "id": "Pengubah Sinyal Digital Menjadi Analog." },
    { "en": "Apa Itu 'UART' (Universal Asynchronous Receiver Transmitter)?", "id": "Protokol Komunikasi Data Serial Asinkron." },
    { "en": "Apa Itu 'SPI' (Serial Peripheral Interface)?", "id": "Protokol Komunikasi Data Serial Sinkron." },
    { "en": "Apa Itu 'I2C' (Inter-Integrated Circuit)?", "id": "Protokol Komunikasi Antar Chip Sederhana." },
    { "en": "Apa Itu 'CAN Bus' (Controller Area Network)?", "id": "Protokol Komunikasi Internal Kendaraan Otomotif." },
    { "en": "Apa Itu 'Modbus' (Protokol Modbus)?", "id": "Standar Komunikasi Data Industri Serial." },
    { "en": "Apa Itu 'Profibus' (Process Field Bus)?", "id": "Standar Jaringan Komunikasi Lapangan Industri." },
    { "en": "Apa Itu 'Ethernet' (Jaringan Ethernet)?", "id": "Standar Komunikasi Data Kabel Lokal." },
    { "en": "Apa Itu 'TCP/IP' (Transmission Control Protocol/Internet Protocol)?", "id": "Standar Protokol Pertukaran Data Internet." },
    { "en": "Apa Itu 'MQTT' (Message Queuing Telemetry Transport)?", "id": "Protokol Komunikasi Ringan Untuk IoT." },
    { "en": "Apa Itu 'IoT' (Internet Of Things)?", "id": "Jaringan Benda Terhubung Koneksi Internet." },
    { "en": "Apa Itu 'LDR' (Light Dependent Resistor)?", "id": "Hambatan Berubah Sesuai Intensitas Cahaya." },
    { "en": "Apa Itu 'NTC' (Negative Temperature Coefficient)?", "id": "Resistor Hambatan Turun Saat Panas." },
    { "en": "Apa Itu 'PTC' (Positive Temperature Coefficient)?", "id": "Resistor Hambatan Naik Saat Panas." },
    { "en": "Apa Itu 'SCR' (Silicon Controlled Rectifier)?", "id": "Penyearah Terkendali Untuk Saklar Daya." },
    { "en": "Apa Itu 'TRIAC' (Triode For Alternating Current)?", "id": "Saklar Elektronik Dua Arah AC." },
    { "en": "Apa Itu 'IGBT' (Insulated Gate Bipolar Transistor)?", "id": "Transistor Daya Tinggi Efisiensi Cepat." },
    { "en": "Apa Itu 'MOSFET' (Metal Oxide Semiconductor Field Effect Transistor)?", "id": "Transistor Efek Medan Gerbang Terisolasi." },
    { "en": "Apa Itu 'BJT' (Bipolar Junction Transistor)?", "id": "Transistor Pengendali Arus Dua Kutub." },
    { "en": "Apa Itu 'Dioda Zener' (Zener Diode)?", "id": "Dioda Pengatur Tegangan Tetap Stabil." },
    { "en": "Apa Itu 'Dioda Schottky' (Schottky Diode)?", "id": "Dioda Pensakelaran Cepat Tegangan Rendah." },
    { "en": "Apa Itu 'LED' (Light Emitting Diode)?", "id": "Dioda Pemancar Cahaya Arus Listrik." },
    { "en": "Apa Itu 'LCD' (Liquid Crystal Display)?", "id": "Layar Penampil Berbasis Kristal Cair." },
    { "en": "Apa Itu 'OLED' (Organic Light Emitting Diode)?", "id": "Panel Layar Organik Pemancar Cahaya." },
    { "en": "Apa Itu 'PCB' (Printed Circuit Board)?", "id": "Papan Jalur Penghubung Komponen Elektronika." },
    { "en": "Apa Itu 'IC' (Integrated Circuit)?", "id": "Sirkuit Elektronik Terintegrasi Dalam Chip." },
    { "en": "Apa Itu 'Microcontroller' (Mikrokontroler)?", "id": "Komputer Kecil Dalam Satu Chip." },
    { "en": "Apa Itu 'Microprocessor' (Mikroprosesor)?", "id": "Unit Pemroses Pusat Data Komputer." },
    { "en": "Apa Itu 'DSP' (Digital Signal Processor)?", "id": "Prosesor Khusus Pengolah Sinyal Digital." },
    { "en": "Apa Itu 'FPGA' (Field Programmable Gate Array)?", "id": "Sirkuit Terintegrasi Yang Dapat Diprogram." },
    { "en": "Apa Itu 'ASIC' (Application Specific Integrated Circuit)?", "id": "Chip Untuk Fungsi Khusus Saja." },
    { "en": "Apa Itu 'BOM' (Bill Of Materials)?", "id": "Daftar Lengkap Komponen Proyek Elektronika." },
    { "en": "Apa Itu 'SMD' (Surface Mount Device)?", "id": "Komponen Elektronika Menempel Di Permukaan." },
    { "en": "Apa Itu 'THT' (Through Hole Technology)?", "id": "Teknologi Komponen Melalui Lubang PCB." },
    { "en": "Apa Itu 'MTBF' (Mean Time Between Failures)?", "id": "Rata-Rata Waktu Antar Kegagalan Sistem." },
    { "en": "Apa Itu 'MTTR' (Mean Time To Repair)?", "id": "Rata-Rata Waktu Perbaikan Kerusakan Sistem." },
    { "en": "Apa Itu 'OEE' (Overall Equipment Effectiveness)?", "id": "Ukuran Efektivitas Penggunaan Peralatan Industri." },
    { "en": "Apa Itu 'Cpk' (Process Capability Index)?", "id": "Indeks Kemampuan Proses Produksi Stabil." },
    { "en": "Apa Itu 'Zero Stack' (Tumpukan Nol)?", "id": "Kondisi Tanpa Data Dalam Memori." },
    { "en": "Apa Itu 'Overflow' (Luberan Data)?", "id": "Kondisi Data Melebihi Kapasitas Simpan." },
    { "en": "Apa Itu 'Underflow' (Kekurangan Data)?", "id": "Kondisi Data Kurang Dari Batas." },
    { "en": "Apa Itu 'Bus Arbitration' (Arbitrase Bus)?", "id": "Proses Pengaturan Akses Jalur Data." },
    { "en": "Apa Itu 'Wait State' (Status Tunggu)?", "id": "Jeda Waktu Sinkronisasi Kecepatan Prosesor." },
    { "en": "Apa Itu 'Burst Mode' (Mode Ledakan)?", "id": "Transfer Data Cepat Tanpa Interupsi." },
    { "en": "Apa Itu 'Hot Swapping' (Tukar Panas)?", "id": "Penggantian Komponen Saat Listrik Aktif." },
    { "en": "Apa Itu 'Cold Boot' (Mulai Dingin)?", "id": "Proses Menyalakan Komputer Dari Mati." },
    { "en": "Apa Itu 'Warm Boot' (Mulai Hangat)?", "id": "Proses Memulai Ulang Sistem Komputer." },
    { "en": "Apa Itu 'Plug And Play' (Pasang Dan Pakai)?", "id": "Deteksi Otomatis Perangkat Keras Baru." },
    { "en": "Apa Itu 'Peripheral' (Perangkat Periferal)?", "id": "Peralatan Tambahan Terhubung Ke Komputer." },
    { "en": "Apa Itu 'Driver' (Perangkat Lunak Driver)?", "id": "Program Penghubung Hardware Dan Software." },
    { "en": "Apa Itu 'Kernel' (Inti Sistem Operasi)?", "id": "Program Utama Pengelola Sumber Daya." },
    { "en": "Apa Itu 'Thread' (Utas Pemrosesan)?", "id": "Unit Terkecil Dalam Eksekusi Program." },
    { "en": "Apa Itu 'Multitasking' (Banyak Tugas)?", "id": "Kemampuan Menjalankan Banyak Program Sekaligus." },
    { "en": "Apa Itu 'Deadlock' (Kebuntuan Proses)?", "id": "Kondisi Proses Saling Menunggu, Berhenti." },
    { "en": "Apa Itu 'Fragmentation' (Fragmentasi Memori)?", "id": "Pemisahan Penyimpanan Data Tidak Teratur." },
    { "en": "Apa Itu 'Defragmentation' (Defragmentasi)?", "id": "Proses Penataan Ulang Penyimpanan Data." },
    { "en": "Apa Itu 'Safe Mode' (Mode Aman)?", "id": "Mode Diagnosa Sistem Minimalis, Terbatas." },
    { "en": "Apa Itu 'Registry' (Registri Sistem)?", "id": "Database Pengaturan Konfigurasi Sistem Operasi." },
    { "en": "Apa Itu 'Cache' (Memori Penyangga)?", "id": "Penyimpanan Data Sementara Kecepatan Tinggi." },
    { "en": "Apa Itu 'Buffer' (Penyangga Data)?", "id": "Area Memori Penahan Aliran Data." },
    { "en": "Apa Itu 'Latency' (Latensi Jaringan)?", "id": "Waktu Tunda Pengiriman Data Digital." },
    { "en": "Apa Itu 'Bandwidth' (Lebar Pita Jaringan)?", "id": "Kapasitas Maksimal Jalur Komunikasi Data." },
    { "en": "Apa Itu 'Throughput' (Throughput Data)?", "id": "Kecepatan Transfer Data Sebenarnya, Nyata." },
    { "en": "Apa Itu 'Packet' (Paket Data)?", "id": "Satuan Unit Informasi Dalam Jaringan." },
    { "en": "Apa Itu 'Header' (Kepala Paket)?", "id": "Informasi Alamat Pada Awal Data." },
    { "en": "Apa Itu 'Payload' (Isi Data)?", "id": "Data Inti Yang Dikirimkan Jaringan." },
    { "en": "Apa Itu 'Footer' (Kaki Paket)?", "id": "Informasi Penutup Pada Akhir Data." },
    { "en": "Apa Itu 'Checksum' (Jumlah Periksa)?", "id": "Nilai Verifikasi Integritas Data Digital." },
    { "en": "Apa Itu 'Handshake' (Jabat Tangan)?", "id": "Proses Awal Kesepakatan Antar Perangkat." },
    { "en": "Apa Itu 'Broadcast' (Siaran Jaringan)?", "id": "Pengiriman Data Ke Semua Alamat." },
    { "en": "Apa Itu 'Multicast' (Siaran Kelompok)?", "id": "Pengiriman Data Ke Kelompok Tertentu." },
    { "en": "Apa Itu 'Unicast' (Siaran Tunggal)?", "id": "Pengiriman Data Ke Satu Alamat." },
    { "en": "Apa Itu 'Subnet Mask' (Masker Subjaringan)?", "id": "Angka Pemisah Alamat Jaringan, Host." },
    { "en": "Apa Itu 'Gateway' (Gerbang Jaringan)?", "id": "Titik Penghubung Antar Jaringan Berbeda." },
    { "en": "Apa Itu 'Router' (Alat Perute)?", "id": "Perangkat Pengarah Lalu Lintas Jaringan." },
    { "en": "Apa Itu 'Switch' (Saklar Jaringan)?", "id": "Penghubung Banyak Perangkat Jalur Lokal." },
    { "en": "Apa Itu 'Hub' (Pusat Jaringan)?", "id": "Titik Pusat Koneksi Jaringan Sederhana." },
    { "en": "Apa Itu 'Bridge' (Jembatan Jaringan)?", "id": "Penghubung Dua Segmen Jaringan Sama." },
    { "en": "Apa Itu 'Repeater' (Pengulang Sinyal)?", "id": "Alat Penguat Jangkauan Sinyal Jaringan." },
    { "en": "Apa Itu 'Access Point' (Titik Akses)?", "id": "Penyedia Koneksi Nirkabel Ke Kabel." },
    { "en": "Apa Itu 'Modem' (Modulator Demodulator)?", "id": "Pengubah Sinyal Digital Menjadi Analog." },
    { "en": "Apa Itu 'Ethernet' (Standar Ethernet)?", "id": "Teknologi Jaringan Kabel Lokal Standar." },
    { "en": "Apa Itu 'Token Ring' (Cincin Token)?", "id": "Metode Akses Jaringan Berbentuk Lingkaran." },
    { "en": "Apa Itu 'FDDI' (Fiber Distributed Data Interface)?", "id": "Standar Transmisi Data Serat Optik." },
    { "en": "Apa Itu 'ATM' (Asynchronous Transfer Mode)?", "id": "Teknik Komunikasi Data Berbasis Sel." },
    { "en": "Apa Itu 'Frame Relay' (Relay Bingkai)?", "id": "Layanan Komunikasi Data Jarak Jauh." },
    { "en": "Apa Itu 'VPN' (Virtual Private Network)?", "id": "Koneksi Jaringan Pribadi Yang Aman." },
    { "en": "Apa Itu 'Firewall' (Dinding Api)?", "id": "Sistem Keamanan Penyaring Data Jaringan." },
    { "en": "Apa Itu 'Proxy' (Server Proksi)?", "id": "Perantara Antar Pengguna Dan Internet." },
    { "en": "Apa Itu 'DMZ' (Demilitarized Zone)?", "id": "Area Jaringan Luar Yang Terpisah." },
    { "en": "Apa Itu 'Intranet' (Jaringan Internal)?", "id": "Jaringan Privat Khusus Dalam Organisasi." },
    { "en": "Apa Itu 'Extranet' (Jaringan Eksternal)?", "id": "Perluasan Intranet Untuk Pihak Luar." },
    { "en": "Apa Itu 'P2P' (Peer To Peer)?", "id": "Koneksi Langsung Antar Dua Perangkat." },
    { "en": "Apa Itu 'Cloud' (Komputasi Awan)?", "id": "Layanan Komputasi Melalui Jaringan Internet." },
    { "en": "Apa Itu 'SaaS' (Software As A Service)?", "id": "Layanan Perangkat Lunak Berbasis Awan." },
    { "en": "Apa Itu 'PaaS' (Platform As A Service)?", "id": "Layanan Platform Pengembangan Berbasis Awan." },
    { "en": "Apa Itu 'IaaS' (Infrastructure As A Service)?", "id": "Layanan Infrastruktur Fisik Berbasis Awan." },
    { "en": "Apa Itu 'API' (Application Programming Interface)?", "id": "Antarmuka Penghubung Antar Program Aplikasi." },
    { "en": "Apa Itu 'SDK' (Software Development Kit)?", "id": "Kumpulan Alat Pengembangan Perangkat Lunak." },
    { "en": "Apa Itu 'GUI' (Graphical User Interface)?", "id": "Antarmuka Pengguna Berbasis Tampilan Grafis." },
    { "en": "Apa Itu 'CLI' (Command Line Interface)?", "id": "Antarmuka Pengguna Berbasis Baris Perintah." },
    { "en": "Apa Itu 'Open Source' (Sumber Terbuka)?", "id": "Perangkat Lunak Kode Bebas Diakses." },
    { "en": "Apa Itu 'Proprietary' (Milik Sendiri)?", "id": "Perangkat Lunak Dengan Hak Eksklusif." },
    { "en": "Apa Itu 'Freeware' (Perangkat Gratis)?", "id": "Perangkat Lunak Gratis Tanpa Kode." },
    { "en": "Apa Itu 'Shareware' (Perangkat Berbagi)?", "id": "Perangkat Lunak Uji Coba Terbatas." },
    { "en": "Apa Itu 'Adware' (Perangkat Iklan)?", "id": "Perangkat Lunak Yang Menampilkan Iklan." },
    { "en": "Apa Itu 'Malware' (Perangkat Berbahaya)?", "id": "Perangkat Lunak Perusak Sistem Komputer." },
    { "en": "Apa Itu 'Virus' (Virus Komputer)?", "id": "Program Perusak Yang Bisa Menyalin." },
    { "en": "Apa Itu 'Worm' (Cacing Jaringan)?", "id": "Program Perusak Menyebar Melalui Jaringan." },
    { "en": "Apa Itu 'Trojan' (Kuda Troya)?", "id": "Program Berbahaya Menyamar Sebagai Berguna." },
    { "en": "Apa Itu 'Ransomware' (Perangkat Tebusan)?", "id": "Program Pengunci Data Meminta Tebusan." },
    { "en": "Apa Itu 'Spyware' (Perangkat Pengintai)?", "id": "Program Pencuri Informasi Pengguna Diam-Diam." },
    { "en": "Apa Itu 'Phishing' (Pengelabuan)?", "id": "Pencurian Identitas Melalui Komunikasi Palsu." },
    { "en": "Apa Itu 'Spam' (Pesan Sampah)?", "id": "Pesan Massal Tidak Diinginkan, Mengganggu." },
    { "en": "Apa Itu 'Encryption' (Enkripsi Data)?", "id": "Proses Pengacakan Data Menjadi Rahasia." },
    { "en": "Apa Itu 'Decryption' (Dekripsi Data)?", "id": "Proses Pembacaan Kembali Data Rahasia." },
    { "en": "Apa Itu 'Hash' (Fungsi Hash)?", "id": "Algoritma Pengubah Data Menjadi Unik." },
    { "en": "Apa Itu 'Digital Signature' (Tanda Tangan Digital)?", "id": "Verifikasi Keaslian Dokumen Elektronik Digital." },
    { "en": "Apa Itu 'SSL' (Secure Sockets Layer)?", "id": "Protokol Keamanan Komunikasi Internet Lama." },
    { "en": "Apa Itu 'TLS' (Transport Layer Security)?", "id": "Protokol Keamanan Komunikasi Internet Modern." },
    { "en": "Apa Itu 'HTTPS' (Hypertext Transfer Protocol Secure)?", "id": "Protokol Web Aman Dengan Enkripsi." },
    { "en": "Apa Itu 'FTP' (File Transfer Protocol)?", "id": "Protokol Untuk Pengiriman Berkas Digital." },
    { "en": "Apa Itu 'SMTP' (Simple Mail Transfer Protocol)?", "id": "Protokol Standar Pengiriman Surat Elektronik." },
    { "en": "Apa Itu 'POP3' (Post Office Protocol Version 3)?", "id": "Protokol Pengambilan Pesan Dari Server." },
    { "en": "Apa Itu 'IMAP' (Internet Message Access Protocol)?", "id": "Protokol Akses Pesan Tetap Sinkron." },
    { "en": "Apa Itu 'DNS' (Domain Name System)?", "id": "Penerjemah Nama Domain Menjadi IP." },
    { "en": "Apa Itu 'DHCP' (Dynamic Host Configuration Protocol)?", "id": "Pemberi Alamat IP Otomatis Perangkat." },
    { "en": "Apa Itu 'IP Address' (Alamat Protokol Internet)?", "id": "Alamat Unik Identitas Perangkat Jaringan." },
    { "en": "Apa Itu 'MAC Address' (Alamat Kontrol Media)?", "id": "Alamat Fisik Unik Perangkat Keras." },
    { "en": "Apa Itu 'Ping' (Packet Internet Groper)?", "id": "Alat Uji Koneksi Antar Perangkat." },
    { "en": "Apa Itu 'IPv4' (Internet Protocol Version 4)?", "id": "Sistem Alamat IP Tiga-Puluh-Dua Bit." },
    { "en": "Apa Itu 'IPv6' (Internet Protocol Version 6)?", "id": "Sistem Alamat IP Seratus-Dua-Puluh-Delapan Bit." },
    { "en": "Apa Itu '0402' (Resistor Package)?", "id": "Ukuran Komponen Resistor Sangat Kecil." },
    { "en": "Apa Itu '0603' (Resistor Package)?", "id": "Ukuran Komponen Resistor Mikro Standar." },
    { "en": "Apa Itu '0805' (Resistor Package)?", "id": "Ukuran Komponen Resistor Menengah Umum." },
    { "en": "Apa Itu '1206' (Resistor Package)?", "id": "Ukuran Komponen Resistor Daya Rendah." },
    { "en": "Apa Itu 'SOT-23' (Small Outline Transistor)?", "id": "Kemasan Transistor Tempel Tiga Pin." },
    { "en": "Apa Itu 'SOT-223' (Small Outline Transistor)?", "id": "Kemasan Transistor Tempel Daya Menengah." },
    { "en": "Apa Itu 'TO-92' (Transistor Outline 92)?", "id": "Kemasan Transistor Plastik Melalui Lubang." },
    { "en": "Apa Itu 'TO-220' (Transistor Outline 220)?", "id": "Kemasan Transistor Dengan Pendingin Logam." },
    { "en": "Apa Itu 'TO-247' (Transistor Outline 247)?", "id": "Kemasan Transistor Daya Tinggi Besar." },
    { "en": "Apa Itu 'DPAK' (Decawatt Package)?", "id": "Kemasan Transistor Tempel Sisi Pendingin." },
    { "en": "Apa Itu 'SOD-123' (Small Outline Diode)?", "id": "Kemasan Dioda Tempel Ukuran Kecil." },
    { "en": "Apa Itu 'DO-41' (Diode Outline 41)?", "id": "Kemasan Dioda Silinder Melalui Lubang." },
    { "en": "Apa Itu 'SOIC-8' (Small Outline Integrated Circuit)?", "id": "Kemasan Chip Kecil Delapan Pin." },
    { "en": "Apa Itu 'TSSOP-16' (Thin Shrink Small Outline)?", "id": "Kemasan Chip Tipis Enam Belas." },
    { "en": "Apa Itu 'LQFP-64' (Low-Profile Quad Flat Package)?", "id": "Kemasan Chip Persegi Empat Sisi." },
    { "en": "Apa Itu 'PLCC-44' (Plastic Leaded Chip Carrier)?", "id": "Kemasan Chip Plastik Soket Khusus." },
    { "en": "Apa Itu 'IP20' (Ingress Protection 20)?", "id": "Perlindungan Benda Padat, Tanpa Air." },
    { "en": "Apa Itu 'IP44' (Ingress Protection 44)?", "id": "Perlindungan Benda Padat, Percikan Air." },
    { "en": "Apa Itu 'IP54' (Ingress Protection 54)?", "id": "Perlindungan Debu Dan Percikan Air." },
    { "en": "Apa Itu 'IP55' (Ingress Protection 55)?", "id": "Perlindungan Debu Dan Semprotan Air." },
    { "en": "Apa Itu 'IP65' (Ingress Protection 65)?", "id": "Tahan Debu Total, Semprotan Air." },
    { "en": "Apa Itu 'IP66' (Ingress Protection 66)?", "id": "Tahan Debu Total, Pancaran Air." },
    { "en": "Apa Itu 'NEMA 1' (National Electrical Manufacturers Association)?", "id": "Kotak Pelindung Listrik Penggunaan Dalam." },
    { "en": "Apa Itu 'NEMA 3R' (National Electrical Manufacturers Association)?", "id": "Kotak Pelindung Listrik Tahan Hujan." },
    { "en": "Apa Itu 'NEMA 4' (National Electrical Manufacturers Association)?", "id": "Kotak Pelindung Listrik Tahan Air." },
    { "en": "Apa Itu 'NEMA 4X' (National Electrical Manufacturers Association)?", "id": "Kotak Pelindung Listrik Tahan Korosi." },
    { "en": "Apa Itu 'NEMA 12' (National Electrical Manufacturers Association)?", "id": "Kotak Pelindung Listrik Tahan Debu." },
    { "en": "Apa Itu 'IEC 60034' (Rotating Electrical Machines)?", "id": "Standar Internasional Untuk Mesin Berputar." },
    { "en": "Apa Itu 'IEC 60038' (Standard Voltages)?", "id": "Standar Internasional Untuk Tegangan Listrik." },
    { "en": "Apa Itu 'IEC 60051' (Measuring Instruments)?", "id": "Standar Untuk Alat Ukur Analog." },
    { "en": "Apa Itu 'IEC 60068' (Environmental Testing)?", "id": "Standar Pengujian Lingkungan Perangkat Elektronik." },
    { "en": "Apa Itu 'IEC 60076' (Power Transformers)?", "id": "Standar Internasional Untuk Transformator Daya." },
    { "en": "Apa Itu 'IEC 60079' (Explosive Atmospheres)?", "id": "Standar Peralatan Area Bahaya Ledakan." },
    { "en": "Apa Itu 'IEC 60204' (Safety Of Machinery)?", "id": "Standar Keamanan Listrik Mesin Industri." },
    { "en": "Apa Itu 'IEC 60269' (Low Voltage Fuses)?", "id": "Standar Internasional Untuk Sekring Rendah." },
    { "en": "Apa Itu 'IEC 60331' (Fire Resistance Cables)?", "id": "Standar Kabel Tahan Terhadap Api." },
    { "en": "Apa Itu 'IEC 60332' (Flame Retardant Cables)?", "id": "Standar Kabel Penghambat Rambatan Api." },
    { "en": "Apa Itu 'IEC 60364' (Low-Voltage Electrical Installations)?", "id": "Standar Internasional Untuk Instalasi Bangunan." },
    { "en": "Apa Itu 'IEC 60417' (Graphical Symbols)?", "id": "Simbol Grafis Untuk Peralatan Listrik." },
    { "en": "Apa Itu 'IEC 60445' (Human-Machine Interface)?", "id": "Standar Antarmuka Manusia Dan Mesin." },
    { "en": "Apa Itu 'IEC 60502' (Power Cables)?", "id": "Standar Kabel Daya Tegangan Rendah." },
    { "en": "Apa Itu 'IEC 60529' (Degrees Of Protection)?", "id": "Standar Kode Perlindungan Selubung Peralatan." },
    { "en": "Apa Itu 'IEC 60601' (Medical Electrical Equipment)?", "id": "Standar Keamanan Peralatan Listrik Medis." },
    { "en": "Apa Itu 'IEC 60617' (Graphical Symbols)?", "id": "Simbol Grafis Diagram Skematik Listrik." },
    { "en": "Apa Itu 'IEC 60664' (Insulation Coordination)?", "id": "Standar Koordinasi Isolasi Sistem Rendah." },
    { "en": "Apa Itu 'IEC 60721' (Environmental Conditions)?", "id": "Klasifikasi Kondisi Lingkungan Peralatan Listrik." },
    { "en": "Apa Itu 'IEC 60898' (Circuit Breakers Household)?", "id": "Standar Pemutus Arus Instalasi Rumah." },
    { "en": "Apa Itu 'IEC 60947' (Low-Voltage Switchgear)?", "id": "Standar Perangkat Hubung Bagi Industri." },
    { "en": "Apa Itu 'IEC 61000' (Electromagnetic Compatibility)?", "id": "Standar Internasional Kompatibilitas Elektromagnetik Perangkat." },
    { "en": "Apa Itu 'IEC 61131' (Programmable Controllers)?", "id": "Standar Internasional Untuk Pemrograman PLC." },
    { "en": "Apa Itu 'IEC 61439' (Low-Voltage Switchgear Assembly)?", "id": "Standar Perakitan Panel Hubung Rendah." },
    { "en": "Apa Itu 'IEC 61508' (Functional Safety)?", "id": "Standar Keamanan Fungsional Sistem Elektronik." },
    { "en": "Apa Itu 'IEC 61511' (Process Industry Safety)?", "id": "Standar Sistem Instrumen Keamanan Proses." },
    { "en": "Apa Itu 'IEC 61850' (Substation Automation)?", "id": "Standar Protokol Otomasi Gardu Induk." },
    { "en": "Apa Itu 'IEC 62040' (Uninterruptible Power Systems)?", "id": "Standar Internasional Untuk Perangkat UPS." },
    { "en": "Apa Itu 'IEC 62053' (Electricity Metering Equipment)?", "id": "Standar Alat Ukur Energi Listrik." },
    { "en": "Apa Itu 'IEC 62271' (High-Voltage Switchgear)?", "id": "Standar Perangkat Hubung Tegangan Tinggi." },
    { "en": "Apa Itu 'IEEE 80' (Substation Grounding)?", "id": "Panduan Standar Pentanahan Gardu Induk." },
    { "en": "Apa Itu 'IEEE 141' (Industrial Power Distribution)?", "id": "Panduan Distribusi Daya Listrik Industri." },
    { "en": "Apa Itu 'IEEE 142' (Grounding Industrial Systems)?", "id": "Panduan Standar Pembumian Sistem Industri." },
    { "en": "Apa Itu 'IEEE 242' (Protection Coordination)?", "id": "Panduan Koordinasi Proteksi Sistem Industri." },
    { "en": "Apa Itu 'IEEE 399' (Power System Analysis)?", "id": "Panduan Analisis Sistem Tenaga Listrik." },
    { "en": "Apa Itu 'IEEE 446' (Emergency Power Systems)?", "id": "Panduan Sistem Daya Darurat Industri." },
    { "en": "Apa Itu 'IEEE 493' (Reliability Analysis)?", "id": "Panduan Analisis Keandalan Sistem Listrik." },
    { "en": "Apa Itu 'IEEE 519' (Harmonic Control)?", "id": "Standar Batas Harmonisa Sistem Listrik." },
    { "en": "Apa Itu 'IEEE 841' (Severe Duty Motors)?", "id": "Standar Motor Listrik Tugas Berat." },
    { "en": "Apa Itu 'IEEE 1547' (Distributed Resources)?", "id": "Standar Interkoneksi Sumber Energi Terdistribusi." },
    { "en": "Apa Itu 'IEEE 1584' (Arc Flash Calculations)?", "id": "Panduan Perhitungan Bahaya Kilatan Busur." },
    { "en": "Apa Itu 'NFPA 70' (National Electrical Code)?", "id": "Aturan Standar Instalasi Listrik Amerika." },
    { "en": "Apa Itu 'NFPA 70E' (Electrical Safety Workplace)?", "id": "Standar Keselamatan Kerja Listrik Profesional." },
    { "en": "Apa Itu 'NFPA 79' (Industrial Machinery Standard)?", "id": "Standar Listrik Untuk Mesin Industri." },
    { "en": "Apa Itu 'NFPA 110' (Emergency Power Systems)?", "id": "Standar Untuk Sistem Daya Darurat." },
    { "en": "Apa Itu 'BS 7671' (British Standard Wiring Regulations)?", "id": "Aturan Standar Instalasi Listrik Inggris." },
    { "en": "Apa Itu 'AS/NZS 3000' (Wiring Rules)?", "id": "Standar Instalasi Listrik Australia, Selandia." },
    { "en": "Apa Itu 'VDE 0100' (Low Voltage Installations)?", "id": "Standar Jerman Untuk Instalasi Rendah." },
    { "en": "Apa Itu 'GOST R' (Russian National Standard)?", "id": "Standar Nasional Produk Negara Rusia." },
    { "en": "Apa Itu 'GB Standard' (Chinese National Standard)?", "id": "Standar Nasional Produk Negara Cina." },
    { "en": "Apa Itu 'JIS' (Japanese Industrial Standards)?", "id": "Standar Industri Nasional Negara Jepang." },
    { "en": "Apa Itu 'NEMA MG1' (Motors And Generators)?", "id": "Standar Untuk Mesin Listrik Berputar." },
    { "en": "Apa Itu 'UL 508A' (Industrial Control Panels)?", "id": "Standar Keamanan Panel Kontrol Industri." },
    { "en": "Apa Itu 'UL 489' (Molded Case Breakers)?", "id": "Standar Keamanan Pemutus Arus Kompak." },
    { "en": "Apa Itu 'UL 94' (Flammability Plastics)?", "id": "Standar Ketahanan Api Bahan Plastik." },
    { "en": "Apa Itu 'CE Mark' (Conformité Européenne)?", "id": "Tanda Kesesuaian Standar Keamanan Eropa." },
    { "en": "Apa Itu 'WEEE' (Waste Electrical Electronic Equipment)?", "id": "Aturan Pengelolaan Limbah Elektronik Digital." },
    { "en": "Apa Itu 'REACH' (Registration Evaluation Chemicals)?", "id": "Aturan Penggunaan Bahan Kimia Berbahaya." },
    { "en": "Apa Itu 'ATEX' (Atmosphères Explosibles)?", "id": "Arahan Sertifikasi Peralatan Area Ledakan." },
    { "en": "Apa Itu 'Ex d' (Flameproof Enclosure)?", "id": "Metode Proteksi Penahan Ledakan Internal." },
    { "en": "Apa Itu 'Ex e' (Increased Safety)?", "id": "Metode Proteksi Peningkatan Keamanan Listrik." },
    { "en": "Apa Itu 'Ex i' (Intrinsic Safety)?", "id": "Metode Proteksi Energi Rendah Aman." },
    { "en": "Apa Itu 'Ex n' (Non-Sparking)?", "id": "Metode Proteksi Tanpa Percikan Api." },
    { "en": "Apa Itu 'Ex p' (Pressurized Enclosure)?", "id": "Metode Proteksi Selubung Bertekanan Udara." },
    { "en": "Apa Itu 'Ex t' (Dust Ignition Protection)?", "id": "Metode Proteksi Terhadap Debu Mudah." },
    { "en": "Apa Itu 'Cat I' (Measurement Category 1)?", "id": "Kategori Ukur Sirkuit Elektronik Terlindungi." },
    { "en": "Apa Itu 'Cat II' (Measurement Category 2)?", "id": "Kategori Ukur Beban Rumah Tangga." },
    { "en": "Apa Itu 'Cat III' (Measurement Category 3)?", "id": "Kategori Ukur Instalasi Bangunan Tetap." },
    { "en": "Apa Itu 'Cat IV' (Measurement Category 4)?", "id": "Kategori Ukur Sumber Pasokan Utama." },
    { "en": "Apa Itu 'LOTO' (Lockout Tagout)?", "id": "Prosedur Keamanan Isolasi Energi Berbahaya." },
    { "en": "Apa Itu 'Arc Flash' (Kilatan Busur)?", "id": "Ledakan Cahaya Panas Akibat Arus." },
    { "en": "Apa Itu 'Boundary' (Batas Keamanan Listrik)?", "id": "Jarak Aman Dari Peralatan Bertegangan." },
    { "en": "Apa Itu 'Work Function' (Fungsi Kerja)?", "id": "Energi Minimum Pelepasan Elektron Logam." },
    { "en": "Apa Itu 'Fermi Level' (Tingkat Fermi)?", "id": "Tingkat Energi Elektron Paling Tinggi." },
    { "en": "Apa Itu 'Band Gap' (Celah Pita Energi)?", "id": "Pemisah Pita Valensi, Pita Konduksi." },
    { "en": "Apa Itu 'Valence Band' (Pita Valensi)?", "id": "Pita Energi Terluar Berisi Elektron." },
    { "en": "Apa Itu 'Conduction Band' (Pita Konduksi)?", "id": "Pita Tempat Elektron Bebas Mengalir." },
    { "en": "Apa Itu 'Intrinsic Semiconductor' (Semikonduktor Intrinsik)?", "id": "Semikonduktor Murni Tanpa Adanya Campuran." },
    { "en": "Apa Itu 'Extrinsic Semiconductor' (Semikonduktor Ekstrinsik)?", "id": "Semikonduktor Yang Sudah Diberi Pengotor." },
    { "en": "Apa Itu 'Majority Carrier' (Pembawa Mayoritas)?", "id": "Muatan Listrik Paling Dominan, Banyak." },
    { "en": "Apa Itu 'Minority Carrier' (Pembawa Minoritas)?", "id": "Muatan Listrik Yang Berjumlah Sedikit." },
    { "en": "Apa Itu 'Recombination' (Rekombinasi Muatan)?", "id": "Penyatuan Kembali Elektron Dan Lubang." },
    { "en": "Apa Itu 'Diffusion' (Difusi Muatan)?", "id": "Perpindahan Muatan Akibat Perbedaan Konsentrasi." },
    { "en": "Apa Itu 'Drift Current' (Arus Hanyut)?", "id": "Aliran Muatan Akibat Medan Listrik." },
    { "en": "Apa Itu 'Depletion Region' (Daerah Deplesi)?", "id": "Daerah Kosong Muatan Di Persambungan." },
    { "en": "Apa Itu 'Barrier Potential' (Potensial Penghalang)?", "id": "Tegangan Penahan Aliran Muatan Listrik." },
    { "en": "Apa Itu 'Reverse Bias' (Prategangan Terbalik)?", "id": "Tegangan Penghambat Aliran Arus Dioda." },
    { "en": "Apa Itu 'Forward Bias' (Prategangan Maju)?", "id": "Tegangan Pemicu Aliran Arus Dioda." },
    { "en": "Apa Itu 'Avalanche Breakdown' (Keruntuhan Longsoran)?", "id": "Loncatan Elektron Akibat Medan Kuat." },
    { "en": "Apa Itu 'Zener Breakdown' (Keruntuhan Zener)?", "id": "Aliran Arus Akibat Efek Terowongan." },
    { "en": "Apa Itu 'Tunneling' (Penembusan Terowongan Kuantum)?", "id": "Partikel Menembus Penghalang Potensial Tipis." },
    { "en": "Apa Itu 'Schottky Barrier' (Penghalang Schottky)?", "id": "Potensial Antara Logam Dan Semikonduktor." },
    { "en": "Apa Itu 'Ohmic Contact' (Kontak Ohmik)?", "id": "Hubungan Logam Semikonduktor Tanpa Penyearah." },
    { "en": "Apa Itu 'Photoelectric Effect' (Efek Fotolistrik)?", "id": "Pelepasan Elektron Akibat Radiasi Cahaya." },
    { "en": "Apa Itu 'Photovoltaic Effect' (Efek Fotovoltaik)?", "id": "Pembangkitan Tegangan Akibat Paparan Cahaya." },
    { "en": "Apa Itu 'Radiative Recombination' (Rekombinasi Radiatif)?", "id": "Rekombinasi Yang Memancarkan Foton Cahaya." },
    { "en": "Apa Itu 'Non-Radiative Recombination' (Rekombinasi Non-Radiatif)?", "id": "Rekombinasi Tanpa Adanya Pancaran Cahaya." },
    { "en": "Apa Itu 'Exciton' (Eksiton)?", "id": "Pasangan Terikat Antara Elektron, Lubang." },
    { "en": "Apa Itu 'Phonon' (Fonon)?", "id": "Kuantum Energi Dari Getaran Kristal." },
    { "en": "Apa Itu 'Hole' (Lubang Muatan)?", "id": "Kekosongan Elektron Bermuatan Listrik Positif." },
    { "en": "Apa Itu 'Doping' (Pendopingan)?", "id": "Penambahan Atom Pengotor Ke Kristal." },
    { "en": "Apa Itu 'Acceptor' (Akseptor)?", "id": "Atom Pengotor Penambah Jumlah Lubang." },
    { "en": "Apa Itu 'Donor' (Donor)?", "id": "Atom Pengotor Penambah Jumlah Elektron." },
    { "en": "Apa Itu 'Lattice' (Kisi Kristal)?", "id": "Susunan Atom Teratur Dalam Kristal." },
    { "en": "Apa Itu 'Miller Indices' (Indeks Miller)?", "id": "Sistem Notasi Arah Bidang Kristal." },
    { "en": "Apa Itu 'Epitaxy' (Epitaksi)?", "id": "Pertumbuhan Lapisan Kristal Di Atas." },
    { "en": "Apa Itu 'Polysilicon' (Polisilikon)?", "id": "Bahan Silikon Dari Banyak Kristal." },
    { "en": "Apa Itu 'Amorphous Silicon' (Silikon Amorf)?", "id": "Silikon Dengan Struktur Atom Acak." },
    { "en": "Apa Itu 'Monocrystalline' (Monokristalin)?", "id": "Bahan Terbuat Dari Kristal Tunggal." },
    { "en": "Apa Itu 'Polycrystalline' (Polikristalin)?", "id": "Bahan Terbuat Dari Banyak Kristal." },
    { "en": "Apa Itu 'Thin Film' (Film Tipis)?", "id": "Lapisan Material Sangat Tipis Mikro." },
    { "en": "Apa Itu 'Sputtering' (Sputtering)?", "id": "Proses Pengendapan Material Menggunakan Ion." },
    { "en": "Apa Itu 'Evaporation' (Penguapan Material)?", "id": "Proses Pengendapan Melalui Penguapan Panas." },
    { "en": "Apa Itu 'CVD' (Chemical Vapor Deposition)?", "id": "Pengendapan Lapisan Melalui Reaksi Kimia." },
    { "en": "Apa Itu 'Plasma Etching' (Pengikisan Plasma)?", "id": "Pengikisan Material Menggunakan Gas Plasma." },
    { "en": "Apa Itu 'Ion Implantation' (Implantasi Ion)?", "id": "Penembakan Ion Ke Dalam Wafer." },
    { "en": "Apa Itu 'Annealing' (Aniling)?", "id": "Proses Pemanasan Untuk Memperbaiki Kristal." },
    { "en": "Apa Itu 'Oxidation' (Oksidasi)?", "id": "Proses Penumbuhan Lapisan Oksida Silikon." },
    { "en": "Apa Itu 'Passivation' (Pasivasi)?", "id": "Lapisan Pelindung Permukaan Komponen Elektronik." },
    { "en": "Apa Itu 'Dielectric Constant' (Konstanta Dielektrik)?", "id": "Kemampuan Bahan Menyimpan Energi Listrik." },
    { "en": "Apa Itu 'Piezoelectricity' (Piezoelektrisitas)?", "id": "Tegangan Akibat Tekanan Mekanik Bahan." },
    { "en": "Apa Itu 'Pyroelectricity' (Piroelektrisitas)?", "id": "Tegangan Akibat Perubahan Suhu Bahan." },
    { "en": "Apa Itu 'Ferroelectricity' (Feroelektrisitas)?", "id": "Polarisasi Listrik Spontan Pada Bahan." },
    { "en": "Apa Itu 'Electrostriction' (Elektrostriksi)?", "id": "Perubahan Bentuk Akibat Medan Listrik." },
    { "en": "Apa Itu 'Magnetostriction' (Magnetostriksi)?", "id": "Perubahan Bentuk Akibat Medan Magnet." },
    { "en": "Apa Itu 'Paramagnetism' (Paramagnetisme)?", "id": "Sifat Magnet Lemah Mengikuti Medan." },
    { "en": "Apa Itu 'Diamagnetism' (Diamagnetisme)?", "id": "Sifat Magnet Menolak Medan Luar." },
    { "en": "Apa Itu 'Ferromagnetism' (Feromagnetisme)?", "id": "Sifat Magnet Kuat Secara Spontan." },
    { "en": "Apa Itu 'Antiferromagnetism' (Antiferomagnetisme)?", "id": "Momen Magnet Berlawanan, Saling Menghilangkan." },
    { "en": "Apa Itu 'Ferrimagnetism' (Ferimagnetisme)?", "id": "Momen Magnet Berlawanan, Tidak Sama." },
    { "en": "Apa Itu 'Superconductivity' (Superkonduktivitas)?", "id": "Hantaran Listrik Tanpa Hambatan Sama." },
    { "en": "Apa Itu 'Meissner Effect' (Efek Meissner)?", "id": "Penolakan Medan Magnet Oleh Superkonduktor." },
    { "en": "Apa Itu 'Josephson Junction' (Persambungan Josephson)?", "id": "Terowongan Elektron Antara Dua Superkonduktor." },
    { "en": "Apa Itu 'SQUID' (Superconducting Quantum Interference Device)?", "id": "Sensor Medan Magnet Sangat Sensitif." },
    { "en": "Apa Itu 'Hall Coefficient' (Koefisien Hall)?", "id": "Ukuran Kekuatan Efek Hall Bahan." },
    { "en": "Apa Itu 'Lorentz Force' (Gaya Lorentz)?", "id": "Gaya Muatan Dalam Medan Elektromagnetik." },
    { "en": "Apa Itu 'Cyclotron Frequency' (Frekuensi Siklotron)?", "id": "Frekuensi Gerak Melingkar Muatan Listrik." },
    { "en": "Apa Itu 'Plasma Frequency' (Frekuensi Plasma)?", "id": "Frekuensi Osilasi Elektron Dalam Plasma." },
    { "en": "Apa Itu 'Debye Length' (Jarak Debye)?", "id": "Jarak Lindung Listrik Dalam Plasma." },
    { "en": "Apa Itu 'Skin Depth' (Kedalaman Kulit)?", "id": "Jarak Penetrasi Arus Dalam Konduktor." },
    { "en": "Apa Itu 'Brewster Angle' (Sudut Brewster)?", "id": "Sudut Cahaya Tanpa Pantulan Linear." },
    { "en": "Apa Itu 'Total Internal Reflection' (Pemantulan Internal Total)?", "id": "Pemantulan Cahaya Sempurna Dalam Medium." },
    { "en": "Apa Itu 'Numerical Aperture' (Bukaan Numerik)?", "id": "Kemampuan Serat Menangkap Sinar Cahaya." },
    { "en": "Apa Itu 'Modal Dispersion' (Disperis Mode)?", "id": "Penyebaran Pulsa Cahaya Dalam Serat." },
    { "en": "Apa Itu 'Chromatic Dispersion' (Dispersi Kromatik)?", "id": "Penyebaran Cahaya Akibat Perbedaan Warna." },
    { "en": "Apa Itu 'Rayleigh Scattering' (Hamburan Rayleigh)?", "id": "Hamburan Cahaya Oleh Partikel Kecil." },
    { "en": "Apa Itu 'Stimulated Emission' (Pancaran Terstimulasi)?", "id": "Pancaran Cahaya Akibat Pemicu Foton." },
    { "en": "Apa Itu 'Spontaneous Emission' (Pancaran Spontan)?", "id": "Pancaran Cahaya Secara Alami, Acak." },
    { "en": "Apa Itu 'Population Inversion' (Pembalikan Populasi)?", "id": "Kondisi Energi Tinggi Lebih Banyak." },
    { "en": "Apa Itu 'Optical Cavity' (Rongga Optik)?", "id": "Ruang Resonansi Pantulan Gelombang Cahaya." },
    { "en": "Apa Itu 'Laser Threshold' (Ambang Batas Laser)?", "id": "Penguatan Minimum Untuk Aksi Laser." },
    { "en": "Apa Itu 'Coherence Length' (Jarak Koherensi)?", "id": "Jarak Cahaya Tetap Dalam Fase." },
    { "en": "Apa Itu 'Interferometry' (Interferometri)?", "id": "Teknik Pengukuran Menggunakan Interferensi Cahaya." },
    { "en": "Apa Itu 'Diffraction Grating' (Kisi Difraksi)?", "id": "Alat Pemecah Cahaya Menjadi Spektrum." },
    { "en": "Apa Itu 'Holography' (Holografi)?", "id": "Teknik Perekaman Gambar Tiga Dimensi." },
    { "en": "Apa Itu 'Fiber Bragg Grating' (Kisi Bragg Serat)?", "id": "Reflektor Cahaya Dalam Serat Optik." },
    { "en": "Apa Itu 'Optical Isolator' (Isolator Optik)?", "id": "Perangkat Penyalur Cahaya Satu Arah." },
    { "en": "Apa Itu 'Waveplate' (Pelambat Fase)?", "id": "Alat Pengubah Status Polarisasi Cahaya." },
    { "en": "Apa Itu 'Polarizer' (Polarisator)?", "id": "Penyaring Arah Getaran Gelombang Cahaya." },
    { "en": "Apa Itu 'Kerr Effect' (Efek Kerr)?", "id": "Perubahan Indeks Bias Medan Listrik." },
    { "en": "Apa Itu 'Pockels Effect' (Efek Pockels)?", "id": "Perubahan Bias Linear Tegangan Listrik." },
    { "en": "Apa Itu 'Faraday Rotation' (Rotasi Faraday)?", "id": "Rotasi Polarisasi Akibat Medan Magnet." },
    { "en": "Apa Itu 'Acousto-Optic Effect' (Efek Akusto-Optik)?", "id": "Interaksi Gelombang Suara Dan Cahaya." },
    { "en": "Apa Itu 'Photoluminescence' (Fotoluminesensi)?", "id": "Pancaran Cahaya Setelah Menyerap Foton." },
    { "en": "Apa Itu 'Electroluminescence' (Elektroluminesensi)?", "id": "Pancaran Cahaya Akibat Aliran Arus." },
    { "en": "Apa Itu 'Fluorescence' (Fluoresensi)?", "id": "Pancaran Cahaya Sesaat Setelah Eksitasi." },
    { "en": "Apa Itu 'Phosphorescence' (Fosforesensi)?", "id": "Pancaran Cahaya Tertunda Setelah Eksitasi." },
    { "en": "Apa Itu 'Radiometry' (Radiometri)?", "id": "Ilmu Pengukuran Energi Radiasi Elektromagnetik." },
    { "en": "Apa Itu 'Spectrometry' (Spektrometri)?", "id": "Ilmu Pengukuran Spektrum Panjang Gelombang." },
    { "en": "Apa Itu 'Quantum Efficiency' (Efisiensi Kuantum)?", "id": "Rasio Foton Menjadi Pembawa Muatan." },
    { "en": "Apa Itu 'Dark Current' (Arus Gelap)?", "id": "Arus Mengalir Saat Tanpa Cahaya." },
    { "en": "Apa Itu 'Responsivity' (Responsivitas)?", "id": "Rasio Arus Output Terhadap Daya." },
    { "en": "Apa Itu 'Specific Gravity' (Berat Jenis Elektrolit)?", "id": "Ukuran Kepekatan Cairan Asam Baterai." },
    { "en": "Apa Itu 'Load Factor' (Faktor Beban Listrik)?", "id": "Rasio Beban Rata-Rata Terhadap Puncak." },
    { "en": "Apa Itu 'Demand Factor' (Faktor Kebutuhan Daya)?", "id": "Rasio Beban Maksimum Terhadap Terpasang." },
    { "en": "Apa Itu 'Diversity Factor' (Faktor Keragaman)?", "id": "Rasio Total Puncak Terhadap Gabungan." },
    { "en": "Apa Itu 'Plant Factor' (Faktor Kapasitas Pembangkit)?", "id": "Rasio Energi Hasil Terhadap Kapasitas." },
    { "en": "Apa Itu 'Utilization Factor' (Faktor Pemanfaatan Daya)?", "id": "Rasio Beban Terpakai Terhadap Maksimum." },
    { "en": "Apa Itu 'Coercivity' (Koersivitas Magnetik)?", "id": "Ketahanan Bahan Terhadap Demagnetisasi Magnet." },
    { "en": "Apa Itu 'Remanence' (Remanensi Magnetik)?", "id": "Sisa Magnetisme Setelah Medan Hilang." },
    { "en": "Apa Itu 'Eddy Current Loss' (Rugi Arus Eddy)?", "id": "Energi Hilang Akibat Induksi Inti." },
    { "en": "Apa Itu 'Hysteresis Loss' (Rugi Histeresis)?", "id": "Energi Hilang Akibat Magnetisasi Bolak-Balik." },
    { "en": "Apa Itu 'Stray Loss' (Rugi-Rugi Liar)?", "id": "Kerugian Energi Listrik Tidak Terduga." },
    { "en": "Apa Itu 'Copper Loss' (Rugi Tembaga)?", "id": "Panas Akibat Hambatan Kawat Penghantar." },
    { "en": "Apa Itu 'Iron Loss' (Rugi Besi)?", "id": "Total Kerugian Energi Inti Magnetik." },
    { "en": "Apa Itu 'Voltage Regulation' (Regulasi Tegangan)?", "id": "Kemampuan Menjaga Tegangan Tetap Stabil." },
    { "en": "Apa Itu 'Efficiency' (Efisiensi Mesin)?", "id": "Rasio Daya Keluar Terhadap Masuk." },
    { "en": "Apa Itu 'Open Circuit Test' (Uji Beban Nol)?", "id": "Pengujian Karakteristik Inti Transformator Tanpa." },
    { "en": "Apa Itu 'Short Circuit Test' (Uji Hubung Singkat)?", "id": "Pengujian Karakteristik Lilitan Transformator Terhubung." },
    { "en": "Apa Itu 'Polarity Test' (Uji Kutub)?", "id": "Penentuan Arah Lilitan Kumparan Listrik." },
    { "en": "Apa Itu 'Back-To-Back Test' (Uji Sumpner)?", "id": "Pengujian Pemanasan Dua Transformator Sekaligus." },
    { "en": "Apa Itu 'Turns Ratio' (Rasio Lilitan)?", "id": "Perbandingan Jumlah Lilitan Primer, Sekunder." },
    { "en": "Apa Itu 'Voltage Ratio' (Rasio Tegangan)?", "id": "Perbandingan Tegangan Masukan Dan Keluaran." },
    { "en": "Apa Itu 'Current Ratio' (Rasio Arus)?", "id": "Perbandingan Arus Masukan Dan Keluaran." },
    { "en": "Apa Itu 'Isolation Transformer' (Transformator Isolasi)?", "id": "Pemisah Listrik Sisi Input Dan." },
    { "en": "Apa Itu 'Step-Up Transformer' (Transformator Penaik)?", "id": "Meningkatkan Level Tegangan Arus Bolak-Balik." },
    { "en": "Apa Itu 'Step-Down Transformer' (Transformator Penurun)?", "id": "Menurunkan Level Tegangan Arus Bolak-Balik." },
    { "en": "Apa Itu 'Autotransformer' (Transformator Otomatis)?", "id": "Transformator Dengan Satu Lilitan Bersama." },
    { "en": "Apa Itu 'Instrument Transformer' (Transformator Instrumen)?", "id": "Transformator Khusus Untuk Alat Ukur." },
    { "en": "Apa Itu 'CT' (Current Transformer)?", "id": "Transformator Penurun Arus Skala Besar." },
    { "en": "Apa Itu 'PT' (Potential Transformer)?", "id": "Transformator Penurun Tegangan Skala Besar." },
    { "en": "Apa Itu 'Burden' (Beban Sekunder)?", "id": "Impedansi Eksternal Pada Sekunder Transformator." },
    { "en": "Apa Itu 'Knee Point Voltage' (Tegangan Lutut)?", "id": "Batas Saturasi Magnetik Inti CT." },
    { "en": "Apa Itu 'Excitation Current' (Arus Eksitasi)?", "id": "Arus Pembangkit Medan Magnet Utama." },
    { "en": "Apa Itu 'Inrush Current' (Arus Inrush)?", "id": "Lonjakan Arus Saat Transformator Dinyalakan." },
    { "en": "Apa Itu 'Vector Group' (Grup Vektor)?", "id": "Hubungan Lilitan Transformator Tiga Fase." },
    { "en": "Apa Itu 'Star Connection' (Hubungan Bintang)?", "id": "Koneksi Fase Dengan Titik Netral." },
    { "en": "Apa Itu 'Delta Connection' (Hubungan Delta)?", "id": "Koneksi Fase Bentuk Segitiga Tertutup." },
    { "en": "Apa Itu 'Zigzag Connection' (Hubungan Zigzag)?", "id": "Hubungan Lilitan Pembentuk Titik Netral." },
    { "en": "Apa Itu 'Neutral Grounding' (Pentanahan Netral)?", "id": "Penghubung Titik Netral Menuju Bumi." },
    { "en": "Apa Itu 'NGR' (Neutral Grounding Resistor)?", "id": "Resistor Pembatas Arus Gangguan Tanah." },
    { "en": "Apa Itu 'Solid Grounding' (Pentanahan Langsung)?", "id": "Hubungan Netral Ke Bumi Tanpa." },
    { "en": "Apa Itu 'Reactance Grounding' (Pentanahan Reaktansi)?", "id": "Hubungan Netral Menggunakan Komponen Induktor." },
    { "en": "Apa Itu 'Arcing Ground' (Busur Tanah)?", "id": "Loncatan Arus Berulang Melalui Tanah." },
    { "en": "Apa Itu 'Peterson Coil' (Kumparan Peterson)?", "id": "Reaktor Penyeimbang Kapasitansi Saluran Transmisi." },
    { "en": "Apa Itu 'Fault Current' (Arus Gangguan)?", "id": "Aliran Listrik Saat Hubung Singkat." },
    { "en": "Apa Itu 'Bolted Fault' (Gangguan Baut)?", "id": "Hubung Singkat Tanpa Hambatan Kontak." },
    { "en": "Apa Itu 'Line To Ground Fault' (Gangguan Tanah)?", "id": "Hubung Singkat Antara Fase, Bumi." },
    { "en": "Apa Itu 'Line To Line Fault' (Gangguan Fase)?", "id": "Hubung Singkat Antara Dua Fase." },
    { "en": "Apa Itu 'Three Phase Fault' (Gangguan Tiga)?", "id": "Hubung Singkat Melibatkan Semua Fase." },
    { "en": "Apa Itu 'Symmetrical Components' (Komponen Simetris)?", "id": "Metode Analisis Sistem Tak Seimbang." },
    { "en": "Apa Itu 'Positive Sequence' (Urutan Positif)?", "id": "Komponen Urutan Sesuai Putaran Normal." },
    { "en": "Apa Itu 'Negative Sequence' (Urutan Negatif)?", "id": "Komponen Urutan Berlawanan Putaran Normal." },
    { "en": "Apa Itu 'Zero Sequence' (Urutan Nol)?", "id": "Komponen Urutan Tanpa Perbedaan Fase." },
    { "en": "Apa Itu 'Pickup Current' (Arus Batas)?", "id": "Arus Minimum Penggerak Kerja Relay." },
    { "en": "Apa Itu 'Plug Setting Multiplier' (Multiplier Setelan)?", "id": "Rasio Arus Gangguan Terhadap Setelan." },
    { "en": "Apa Itu 'Time Multiplier Setting' (Multiplier Waktu)?", "id": "Pengatur Waktu Operasi Relay Proteksi." },
    { "en": "Apa Itu 'IDMT' (Inverse Definite Minimum Time)?", "id": "Waktu Kerja Terbalik Intensitas Arus." },
    { "en": "Apa Itu 'Differential Relay' (Relay Diferensial)?", "id": "Proteksi Berdasarkan Selisih Arus Masuk." },
    { "en": "Apa Itu 'Distance Relay' (Relay Jarak)?", "id": "Proteksi Berdasarkan Nilai Impedansi Jalur." },
    { "en": "Apa Itu 'Mho Relay' (Relay Mho)?", "id": "Relay Jarak Dengan Karakteristik Admitansi." },
    { "en": "Apa Itu 'Buchholz Relay' (Relay Buchholz)?", "id": "Pengaman Gas Dan Tekanan Transformator." },
    { "en": "Apa Itu 'Circuit Breaker' (Pemutus Sirkuit)?", "id": "Saklar Pemutus Arus Gangguan Otomatis." },
    { "en": "Apa Itu 'Isolator' (Saklar Pemisah)?", "id": "Pemutus Rangkaian Dalam Kondisi Mati." },
    { "en": "Apa Itu 'Busbar' (Rel Tembaga)?", "id": "Penghantar Utama Arus Listrik Besar." },
    { "en": "Apa Itu 'Main Busbar' (Busbar Utama)?", "id": "Jalur Distribusi Energi Pusat Panel." },
    { "en": "Apa Itu 'Transfer Busbar' (Busbar Transfer)?", "id": "Jalur Cadangan Perpindahan Beban Listrik." },
    { "en": "Apa Itu 'Sectionalizer' (Seksonaliser)?", "id": "Alat Pemutus Jaringan Distribusi Otomatis." },
    { "en": "Apa Itu 'Recloser' (Rekloser)?", "id": "Pemutus Jaringan Penutup Kembali Otomatis." },
    { "en": "Apa Itu 'Surge Arrester' (Penangkap Surja)?", "id": "Alat Pembuang Tegangan Lebih Tanah." },
    { "en": "Apa Itu 'Earthing Screen' (Tabir Pembumian)?", "id": "Kawat Pelindung Gardu Dari Petir." },
    { "en": "Apa Itu 'Guard Wire' (Kawat Pelindung)?", "id": "Kabel Pengaman Saluran Transmisi Listrik." },
    { "en": "Apa Itu 'Stay Wire' (Kawat Tarik)?", "id": "Kawat Penahan Beban Tarik Tiang." },
    { "en": "Apa Itu 'Insulator' (Isolator Tiang)?", "id": "Penyangga Kabel Penghambat Arus Tanah." },
    { "en": "Apa Itu 'Pin Insulator' (Isolator Tumpu)?", "id": "Isolator Dudukan Kabel Tegangan Menengah." },
    { "en": "Apa Itu 'Suspension Insulator' (Isolator Gantung)?", "id": "Isolator Penahan Kabel Tegangan Tinggi." },
    { "en": "Apa Itu 'Strain Insulator' (Isolator Tarik)?", "id": "Isolator Penahan Beban Tarik Sudut." },
    { "en": "Apa Itu 'Sag' (Lendutan Kabel)?", "id": "Jarak Kabel Terendah Terhadap Tanah." },
    { "en": "Apa Itu 'Span' (Rentang Jarak)?", "id": "Jarak Mendatar Antara Dua Tiang." },
    { "en": "Apa Itu 'Corona' (Efek Korona)?", "id": "Pancaran Cahaya Biru Sekitar Kabel." },
    { "en": "Apa Itu 'Proximity Effect' (Efek Kedekatan)?", "id": "Interferensi Arus Antar Kabel Dekat." },
    { "en": "Apa Itu 'Ferranti Effect' (Efek Ferranti)?", "id": "Tegangan Ujung Terima Lebih Tinggi." },
    { "en": "Apa Itu 'Inductive Reactance' (Reaktansi Induktif)?", "id": "Hambatan Arus Akibat Medan Magnet." },
    { "en": "Apa Itu 'Capacitive Reactance' (Reaktansi Kapasitif)?", "id": "Hambatan Arus Akibat Medan Listrik." },
    { "en": "Apa Itu 'Impedance' (Impedansi Jalur)?", "id": "Total Hambatan Saluran Transmisi Listrik." },
    { "en": "Apa Itu 'Admittance' (Admitansi Jalur)?", "id": "Kemudahan Aliran Arus Saluran Listrik." },
    { "en": "Apa Itu 'Voltage Drop' (Jatuh Tegangan)?", "id": "Kehilangan Tegangan Sepanjang Jalur Kabel." },
    { "en": "Apa Itu 'Power Loss' (Rugi Daya)?", "id": "Energi Terbuang Menjadi Panas Saluran." },
    { "en": "Apa Itu 'Load Flow' (Aliran Beban)?", "id": "Analisis Distribusi Daya Dalam Jaringan." },
    { "en": "Apa Itu 'Swing Bus' (Bus Berayun)?", "id": "Referensi Tegangan Dan Sudut Sistem." },
    { "en": "Apa Itu 'PV Bus' (Generator Bus)?", "id": "Bus Dengan Daya, Tegangan Tetap." },
    { "en": "Apa Itu 'PQ Bus' (Load Bus)?", "id": "Bus Dengan Daya Beban Tetap." },
    { "en": "Apa Itu 'Per-Unit' (Sistem Per-Satuan)?", "id": "Metode Perhitungan Nilai Listrik Normalisasi." },
    { "en": "Apa Itu 'Base Power' (Daya Dasar)?", "id": "Referensi Daya Untuk Perhitungan Sistem." },
    { "en": "Apa Itu 'Base Voltage' (Tegangan Dasar)?", "id": "Referensi Tegangan Untuk Perhitungan Sistem." },
    { "en": "Apa Itu 'Sub-Transient' (Masa Sub-Transien)?", "id": "Kondisi Arus Awal Saat Gangguan." },
    { "en": "Apa Itu 'Synchronous Reactance' (Reaktansi Sinkron)?", "id": "Hambatan Dalam Mesin Listrik Sinkron." },
    { "en": "Apa Itu 'Armature Reaction' (Reaksi Jangkar)?", "id": "Pengaruh Medan Jangkar Terhadap Magnet." },
    { "en": "Apa Itu 'Excitation' (Eksitasi Magnet)?", "id": "Pemberian Arus Penguat Medan Magnet." },
    { "en": "Apa Itu 'Root Locus' (Tempat Kedudukan Akar)?", "id": "Metode Analisis Stabilitas Sistem Kendali." },
    { "en": "Apa Itu 'Bode Plot' (Diagram Bode)?", "id": "Grafik Respon Frekuensi Magnitudo, Fase." },
    { "en": "Apa Itu 'Nyquist Criterion' (Kriteria Nyquist)?", "id": "Penentu Stabilitas Berdasarkan Respon Frekuensi." },
    { "en": "Apa Itu 'Gain Margin' (Margin Penguatan)?", "id": "Batas Penambahan Penguatan Sebelum Tidak Stabil." },
    { "en": "Apa Itu 'Phase Margin' (Margin Fase)?", "id": "Batas Perubahan Fase Sebelum Tidak Stabil." },
    { "en": "Apa Itu 'Settling Time' (Waktu Penetapan)?", "id": "Waktu Sinyal Mencapai Kondisi Tunak." },
    { "en": "Apa Itu 'Rise Time' (Waktu Naik)?", "id": "Waktu Sinyal Naik Ke Nilai Akhir." },
    { "en": "Apa Itu 'Overshoot' (Loncatan Melampaui)?", "id": "Besar Sinyal Melebihi Nilai Tunak." },
    { "en": "Apa Itu 'Steady State Error' (Kesalahan Kondisi Tunak)?", "id": "Selisih Nilai Output Dan Referensi." },
    { "en": "Apa Itu 'Feedforward Control' (Kendali Umpan Maju)?", "id": "Kompensasi Gangguan Sebelum Mempengaruhi Sistem." },
    { "en": "Apa Itu 'Cascade Control' (Kendali Kaskade)?", "id": "Sistem Kendali Dengan Dua Loop." },
    { "en": "Apa Itu 'Deadband' (Pita Mati)?", "id": "Rentang Input Tanpa Respon Output." },
    { "en": "Apa Itu 'Anti-Windup' (Anti-Windup)?", "id": "Metode Mencegah Saturasi Nilai Integral." },
    { "en": "Apa Itu 'Hysteresis' (Histeresis Kendali)?", "id": "Perbedaan Respon Saat Naik Dan Turun." },
    { "en": "Apa Itu 'State Space' (Ruang Keadaan)?", "id": "Representasi Matematika Sistem Dinamis Kompleks." },
    { "en": "Apa Itu 'Controllability' (Keterkendalian)?", "id": "Kemampuan Mengarahkan Keadaan Sistem Tertentu." },
    { "en": "Apa Itu 'Observability' (Keteramatan)?", "id": "Kemampuan Mengetahui Keadaan Dari Output." },
    { "en": "Apa Itu 'Observer' (Pengamat Sistem)?", "id": "Estimator Keadaan Internal Sistem Dinamis." },
    { "en": "Apa Itu 'Kalman Filter' (Penyaring Kalman)?", "id": "Algoritma Estimasi Keadaan Dalam Derau." },
    { "en": "Apa Itu 'Robust Control' (Kendali Kokoh)?", "id": "Sistem Kendali Tahan Terhadap Ketidakpastian." },
    { "en": "Apa Itu 'Adaptive Control' (Kendali Adaptif)?", "id": "Sistem Kendali Dengan Parameter Berubah." },
    { "en": "Apa Itu 'Fuzzy Logic' (Logika Kabur)?", "id": "Sistem Kendali Berdasarkan Derajat Kebenaran." },
    { "en": "Apa Itu 'Neural Network' (Jaringan Saraf)?", "id": "Sistem Kendali Berdasarkan Pembelajaran Data." },
    { "en": "Apa Itu 'DCS' (Distributed Control System)?", "id": "Sistem Kendali Proses Industri Terpusat." },
    { "en": "Apa Itu 'OPC' (Open Platform Communications)?", "id": "Standar Pertukaran Data Industri Universal." },
    { "en": "Apa Itu 'Modbus TCP' (Transmission Control Protocol)?", "id": "Protokol Komunikasi Industri Melalui Ethernet." },
    { "en": "Apa Itu 'PROFINET' (Process Field Net)?", "id": "Standar Ethernet Industri Untuk Otomasi." },
    { "en": "Apa Itu 'EtherNet/IP' (Industrial Protocol)?", "id": "Protokol Komunikasi Industri Berbasis Ethernet." },
    { "en": "Apa Itu 'Foundation Fieldbus'?", "id": "Sistem Komunikasi Digital Instrumen Lapangan." },
    { "en": "Apa Itu 'WirelessHART' (Highway Addressable Remote Transducer)?", "id": "Protokol Nirkabel Untuk Instrumen Industri." },
    { "en": "Apa Itu 'Remote I/O' (Input Output Jarak Jauh)?", "id": "Modul Antarmuka Data Lokasi Berbeda." },
    { "en": "Apa Itu 'Analog Signal' (Sinyal Analog)?", "id": "Sinyal Kontinu Berdasarkan Nilai Fisik." },
    { "en": "Apa Itu 'Digital Signal' (Sinyal Digital)?", "id": "Sinyal Diskrit Berupa Angka Biner." },
    { "en": "Apa Itu 'Current Loop' (Loop Arus) 4-20mA?", "id": "Standar Transmisi Sinyal Analog Industri." },
    { "en": "Apa Itu 'Shielded Cable' (Kabel Terlindung)?", "id": "Kabel Dengan Pelindung Gangguan Elektromagnetik." },
    { "en": "Apa Itu 'Signal Conditioning' (Pengkondisian Sinyal)?", "id": "Pengubahan Sinyal Menjadi Standar Digital." },
    { "en": "Apa Itu 'Cold Junction Compensation' (Kompensasi Sambungan Dingin)?", "id": "Koreksi Suhu Referensi Pada Termokopel." },
    { "en": "Apa Itu 'Galvanic Isolation' (Isolasi Galvanik)?", "id": "Pemisahan Sirkuit Tanpa Kontak Elektrik." },
    { "en": "Apa Itu 'Explosion Proof' (Tahan Ledakan)?", "id": "Peralatan Penahan Ledakan Internal Peralatan." },
    { "en": "Apa Itu 'Intrinsically Safe' (Aman Secara Intrinsik)?", "id": "Sistem Energi Rendah Pencegah Ledakan." },
    { "en": "Apa Itu 'Zener Barrier' (Pembatas Zener)?", "id": "Alat Pelindung Arus Ke Area." },
    { "en": "Apa Itu 'Hazardous Area' (Area Berbahaya)?", "id": "Lokasi Berisiko Gas Mudah Terbakar." },
    { "en": "Apa Itu 'SIL' (Safety Integrity Level)?", "id": "Tingkat Keandalan Sistem Keselamatan Instrumen." },
    { "en": "Apa Itu 'SIS' (Safety Instrumented System)?", "id": "Sistem Otomatis Penjaga Keamanan Proses." },
    { "en": "Apa Itu 'ESD' (Emergency Shutdown System)?", "id": "Sistem Penghentian Darurat Proses Industri." },
    { "en": "Apa Itu 'LOPA' (Layers Of Protection Analysis)?", "id": "Metode Analisis Lapis Perlindungan Keselamatan." },
    { "en": "Apa Itu 'Accuracy' (Akurasi Alat Ukur)?", "id": "Kedekatan Hasil Ukur Nilai Sebenarnya." },
    { "en": "Apa Itu 'Precision' (Presisi Alat Ukur)?", "id": "Konsistensi Hasil Pengukuran Berulang Kali." },
    { "en": "Apa Itu 'Resolution' (Resolusi Alat Ukur)?", "id": "Perubahan Terkecil Yang Dapat Dideteksi." },
    { "en": "Apa Itu 'Linearity' (Linearitas Alat Ukur)?", "id": "Kesebandingan Output Terhadap Perubahan Input." },
    { "en": "Apa Itu 'Repeatability' (Repeatabilitas)?", "id": "Hasil Sama Untuk Kondisi Sama." },
    { "en": "Apa Itu 'Sensitivity' (Sensitivitas)?", "id": "Rasio Perubahan Output Terhadap Input." },
    { "en": "Apa Itu 'Calibration' (Kalibrasi)?", "id": "Proses Pengecekan Akurasi Alat Ukur." },
    { "en": "Apa Itu 'Zero Drift' (Hanyutan Nol)?", "id": "Perubahan Nilai Nol Seiring Waktu." },
    { "en": "Apa Itu 'Span' (Rentang Ukur)?", "id": "Selisih Nilai Maksimum Dan Minimum." },
    { "en": "Apa Itu 'Turndown Ratio' (Rasio Turun)?", "id": "Perbandingan Batas Maksimum Minimum Pengukuran." },
    { "en": "Apa Itu 'Hysteresis Error' (Kesalahan Histeresis)?", "id": "Perbedaan Pembacaan Saat Naik Turun." },
    { "en": "Apa Itu 'Total Error Band' (Pita Kesalahan Total)?", "id": "Akurasi Maksimal Dalam Seluruh Kondisi." },
    { "en": "Apa Itu 'Smart Transmitter' (Transmiter Cerdas)?", "id": "Alat Kirim Data Dengan Mikroprosesor." },
    { "en": "Apa Itu 'Actuator' (Aktuator)?", "id": "Penggerak Mekanik Berdasarkan Sinyal Kendali." },
    { "en": "Apa Itu 'Control Valve' (Katup Kendali)?", "id": "Alat Pengatur Laju Aliran Fluida." },
    { "en": "Apa Itu 'Positioner' (Pemosisian Katup)?", "id": "Alat Penjamin Posisi Bukaan Katup." },
    { "en": "Apa Itu 'Solenoid Valve' (Katup Solenoid)?", "id": "Katup Terkendali Medan Magnet Listrik." },
    { "en": "Apa Itu 'Fail Safe' (Gagal Aman)?", "id": "Kembali Ke Posisi Aman Saat." },
    { "en": "Apa Itu 'Fail Close' (Gagal Tertutup)?", "id": "Katup Menutup Saat Kehilangan Daya." },
    { "en": "Apa Itu 'Fail Open' (Gagal Terbuka)?", "id": "Katup Membuka Saat Kehilangan Daya." },
    { "en": "Apa Itu 'Fail Last' (Gagal Tetap)?", "id": "Katup Tetap Posisi Saat Kehilangan." },
    { "en": "Apa Itu 'Cavitation' (Kavitasi)?", "id": "Pembentukan Gelembung Uap Merusak Katup." },
    { "en": "Apa Itu 'Flashing' (Penguapan Kilat)?", "id": "Perubahan Cairan Menjadi Uap Permanen." },
    { "en": "Apa Itu 'Valve Trim' (Trim Katup)?", "id": "Bagian Internal Katup Bersentuhan Fluida." },
    { "en": "Apa Itu 'Cv Value' (Nilai Koefisien Aliran)?", "id": "Kapasitas Aliran Maksimal Sebuah Katup." },
    { "en": "Apa Itu 'Inherent Characteristic' (Karakteristik Inheren)?", "id": "Hubungan Bukaan Dan Aliran Katup." },
    { "en": "Apa Itu 'Linear Characteristic' (Karakteristik Linear)?", "id": "Aliran Sebanding Dengan Bukaan Katup." },
    { "en": "Apa Itu 'Equal Percentage' (Persentase Sama)?", "id": "Perubahan Aliran Konstan Terhadap Bukaan." },
    { "en": "Apa Itu 'Quick Opening' (Bukaan Cepat)?", "id": "Aliran Besar Pada Bukaan Kecil." },
    { "en": "Apa Itu 'Butterfly Valve' (Katup Kupu-Kupu)?", "id": "Katup Penutup Aliran Piringan Berputar." },
    { "en": "Apa Itu 'Ball Valve' (Katup Bola)?", "id": "Katup Penutup Aliran Bola Berlubang." },
    { "en": "Apa Itu 'Globe Valve' (Katup Bola Dunia)?", "id": "Katup Pengatur Aliran Presisi Tinggi." },
    { "en": "Apa Itu 'Gate Valve' (Katup Gerbang)?", "id": "Katup Pembuka Penutup Aliran Penuh." },
    { "en": "Apa Itu 'Check Valve' (Katup Cek)?", "id": "Katup Pencegah Aliran Balik Fluida." },
    { "en": "Apa Itu 'Pressure Relief Valve' (Katup Pelepas)?", "id": "Katup Pengaman Tekanan Berlebih Sistem." },
    { "en": "Apa Itu 'Bourdon Tube' (Tabung Bourdon)?", "id": "Sensor Tekanan Berbasis Kelengkungan Tabung." },
    { "en": "Apa Itu 'Orifice Plate' (Plat Orifice)?", "id": "Sensor Aliran Berdasarkan Beda Tekanan." },
    { "en": "Apa Itu 'Venturi Meter' (Meter Venturi)?", "id": "Alat Ukur Laju Aliran Cairan." },
    { "en": "Apa Itu 'Magnetic Flowmeter' (Meter Aliran Magnet)?", "id": "Alat Ukur Aliran Cairan Konduktif." },
    { "en": "Apa Itu 'Coriolis Flowmeter' (Meter Aliran Coriolis)?", "id": "Alat Ukur Massa Aliran Fluida." },
    { "en": "Apa Itu 'Vortex Flowmeter' (Meter Aliran Vortex)?", "id": "Alat Ukur Aliran Berdasarkan Pusaran." },
    { "en": "Apa Itu 'Ultrasonic Flowmeter' (Meter Aliran Ultrasonik)?", "id": "Alat Ukur Aliran Gelombang Suara." },
    { "en": "Apa Itu 'Rotameter' (Meter Area Variabel)?", "id": "Alat Ukur Aliran Pelampung Tabung." },
    { "en": "Apa Itu 'DP Cell' (Differential Pressure Cell)?", "id": "Transmiter Pengukur Perbedaan Tekanan Fluida." },
    { "en": "Apa Itu 'Level Switch' (Saklar Level)?", "id": "Sensor Pendeteksi Batas Ketinggian Cairan." },
    { "en": "Apa Itu 'Radar Level Meter' (Meter Level Radar)?", "id": "Sensor Ketinggian Menggunakan Gelombang Radar." },
    { "en": "Apa Itu 'Capacitive Level Sensor' (Sensor Level Kapasitif)?", "id": "Sensor Ketinggian Berdasarkan Nilai Kapasitansi." },
    { "en": "Apa Itu 'Hydrostatic Level' (Level Hidrostatis)?", "id": "Sensor Ketinggian Berdasarkan Tekanan Cairan." },
    { "en": "Apa Itu 'Thermocouple' (Termokopel)?", "id": "Sensor Suhu Berdasarkan Perbedaan Logam." },
    { "en": "Apa Itu 'RTD' (Resistance Temperature Detector)?", "id": "Sensor Suhu Berdasarkan Perubahan Hambatan." },
    { "en": "Apa Itu 'Pt100' (Platinum Resistance Thermometer)?", "id": "Sensor RTD Hambatan Seratus Ohm." },
    { "en": "Apa Itu 'Pyrometer' (Pirometer)?", "id": "Sensor Suhu Tinggi Tanpa Kontak." },
    { "en": "Apa Itu 'Thermowell' (Sumur Termal)?", "id": "Tabung Pelindung Sensor Suhu Industri." },
    { "en": "Apa Itu 'CPU' (Central Processing Unit)?", "id": "Unit Pemroses Pusat Data Komputer." },
    { "en": "Apa Itu 'ALU' (Arithmetic Logic Unit)?", "id": "Bagian Pemroses Operasi Hitung, Logika." },
    { "en": "Apa Itu 'CU' (Control Unit)?", "id": "Unit Pengatur Aliran Instruksi Komputer." },
    { "en": "Apa Itu 'Register' (Register Mikroprosesor)?", "id": "Tempat Penyimpanan Data Internal Cepat." },
    { "en": "Apa Itu 'Program Counter' (Penghitung Program)?", "id": "Register Penunjuk Alamat Instruksi Berikutnya." },
    { "en": "Apa Itu 'Instruction Register' (Register Instruksi)?", "id": "Register Penyimpan Kode Instruksi Aktif." },
    { "en": "Apa Itu 'Stack Pointer' (Penunjuk Tumpukan)?", "id": "Register Penunjuk Lokasi Memori Tumpukan." },
    { "en": "Apa Itu 'Accumulator' (Akumulator)?", "id": "Register Utama Penyimpan Hasil Perhitungan." },
    { "en": "Apa Itu 'Bus' (Jalur Bus)?", "id": "Kumpulan Kabel Penghubung Komponen Digital." },
    { "en": "Apa Itu 'Address Bus' (Bus Alamat)?", "id": "Jalur Penentu Lokasi Memori Data." },
    { "en": "Apa Itu 'Data Bus' (Bus Data)?", "id": "Jalur Perpindahan Informasi Antar Komponen." },
    { "en": "Apa Itu 'Control Bus' (Bus Kendali)?", "id": "Jalur Sinyal Perintah Operasi Perangkat." },
    { "en": "Apa Itu 'Machine Cycle' (Siklus Mesin)?", "id": "Urutan Proses Eksekusi Satu Instruksi." },
    { "en": "Apa Itu 'Fetch' (Pengambilan Instruksi)?", "id": "Proses Pengambilan Instruksi Dari Memori." },
    { "en": "Apa Itu 'Decode' (Pengodean Instruksi)?", "id": "Proses Penerjemahan Kode Instruksi Mesin." },
    { "en": "Apa Itu 'Execute' (Eksekusi Instruksi)?", "id": "Proses Pelaksanaan Perintah Oleh Prosesor." },
    { "en": "Apa Itu 'Interrupt' (Interupsi)?", "id": "Sinyal Penghenti Sementara Aliran Program." },
    { "en": "Apa Itu 'ISR' (Interrupt Service Routine)?", "id": "Fungsi Khusus Penanganan Sinyal Interupsi." },
    { "en": "Apa Itu 'Polling' (Pengambilan Suara)?", "id": "Pengecekan Status Perangkat Secara Berkala." },
    { "en": "Apa Itu 'DMA' (Direct Memory Access)?", "id": "Transfer Data Memori Tanpa CPU." },
    { "en": "Apa Itu 'Cache Memory' (Memori Cache)?", "id": "Memori Penyangga Kecepatan Sangat Tinggi." },
    { "en": "Apa Itu 'L1 Cache' (Cache Level Satu)?", "id": "Memori Cache Tercepat Dalam Inti." },
    { "en": "Apa Itu 'L2 Cache' (Cache Level Dua)?", "id": "Memori Cache Kapasitas Menengah Prosesor." },
    { "en": "Apa Itu 'L3 Cache' (Cache Level Tiga)?", "id": "Memori Cache Bersama Antar Inti." },
    { "en": "Apa Itu 'Pipeline' (Jalur Pipa)?", "id": "Teknik Pemrosesan Instruksi Secara Simultan." },
    { "en": "Apa Itu 'CISC' (Complex Instruction Set Computer)?", "id": "Arsitektur Prosesor Dengan Instruksi Kompleks." },
    { "en": "Apa Itu 'RISC' (Reduced Instruction Set Computer)?", "id": "Arsitektur Prosesor Dengan Instruksi Sederhana." },
    { "en": "Apa Itu 'Von Neumann Architecture' (Arsitektur Von Neumann)?", "id": "Sistem Komputer Dengan Memori Tunggal." },
    { "en": "Apa Itu 'Harvard Architecture' (Arsitektur Harvard)?", "id": "Sistem Komputer Dengan Memori Terpisah." },
    { "en": "Apa Itu 'Microcontroller' (Mikrokontroler)?", "id": "Sistem Komputer Dalam Satu Chip." },
    { "en": "Apa Itu 'EEPROM' (Electrically Erasable Programmable Read Only Memory)?", "id": "Memori Permanen Dapat Dihapus Listrik." },
    { "en": "Apa Itu 'Flash Memory' (Memori Flash)?", "id": "Penyimpanan Non-Volatil Kecepatan Sangat Tinggi." },
    { "en": "Apa Itu 'SRAM' (Static Random Access Memory)?", "id": "Memori RAM Cepat Tanpa Refresh." },
    { "en": "Apa Itu 'DRAM' (Dynamic Random Access Memory)?", "id": "Memori RAM Murah Dengan Refresh." },
    { "en": "Apa Itu 'SDRAM' (Synchronous Dynamic Random Access Memory)?", "id": "Memori RAM Sinkron Dengan Detak." },
    { "en": "Apa Itu 'DDR' (Double Data Rate)?", "id": "Teknologi RAM Dengan Transfer Ganda." },
    { "en": "Apa Itu 'GPU' (Graphics Processing Unit)?", "id": "Prosesor Khusus Pengolah Grafis, Gambar." },
    { "en": "Apa Itu 'ASIC' (Application Specific Integrated Circuit)?", "id": "Chip Khusus Untuk Satu Fungsi." },
    { "en": "Apa Itu 'VHDL' (VHSIC Hardware Description Language)?", "id": "Bahasa Pemodelan Perangkat Keras Digital." },
    { "en": "Apa Itu 'Verilog' (Bahasa Verilog)?", "id": "Bahasa Deskripsi Perangkat Keras Digital." },
    { "en": "Apa Itu 'RTOS' (Real-Time Operating System)?", "id": "Sistem Operasi Respon Waktu Cepat." },
    { "en": "Apa Itu 'Firmware' (Perangkat Tegar)?", "id": "Perangkat Lunak Permanen Dalam Hardware." },
    { "en": "Apa Itu 'Bootloader' (Pemuat Bot)?", "id": "Program Awal Pemanggil Sistem Operasi." },
    { "en": "Apa Itu 'Watchdog Timer' (Timer Penjaga)?", "id": "Sistem Pencegah Kegagalan Program Terhenti." },
    { "en": "Apa Itu 'Brown-Out Reset' (Reset Tegangan Turun)?", "id": "Reset Otomatis Saat Tegangan Rendah." },
    { "en": "Apa Itu 'JTAG' (Joint Test Action Group)?", "id": "Antarmuka Pengujian Dan Pemrograman Chip." },
    { "en": "Apa Itu 'In-Circuit Debugging' (Debugging Dalam Sirkuit)?", "id": "Proses Pelacakan Kesalahan Saat Berjalan." },
    { "en": "Apa Itu 'Emulator' (Emulator)?", "id": "Alat Peniru Fungsi Perangkat Lain." },
    { "en": "Apa Itu 'Simulator' (Simulator)?", "id": "Program Peniru Perilaku Sistem Fisik." },
    { "en": "Apa Itu 'GPIO' (General Purpose Input Output)?", "id": "Pin Digital Untuk Berbagai Fungsi." },
    { "en": "Apa Itu 'I2C' (Inter-Integrated Circuit)?", "id": "Protokol Komunikasi Dua Kabel Sederhana." },
    { "en": "Apa Itu 'CAN' (Controller Area Network)?", "id": "Protokol Komunikasi Handal Sistem Otomotif." },
    { "en": "Apa Itu 'USB' (Universal Serial Bus)?", "id": "Standar Koneksi Perangkat Luar Universal." },
    { "en": "Apa Itu 'Ethernet' (Ethernet)?", "id": "Standar Komunikasi Jaringan Kabel Lokal." },
    { "en": "Apa Itu 'Wi-Fi' (Wireless Fidelity)?", "id": "Teknologi Jaringan Lokal Tanpa Kabel." },
    { "en": "Apa Itu 'Bluetooth' (Bluetooth)?", "id": "Komunikasi Nirkabel Jarak Sangat Pendek." },
    { "en": "Apa Itu 'Zigbee' (Zigbee)?", "id": "Protokol Nirkabel Hemat Energi Industri." },
    { "en": "Apa Itu 'LoRa' (Long Range)?", "id": "Komunikasi Nirkabel Jarak Jauh Daya." },
    { "en": "Apa Itu 'MQTT' (Message Queuing Telemetry Transport)?", "id": "Protokol Pesan Ringan Untuk IoT." },
    { "en": "Apa Itu 'IoT' (Internet Of Things)?", "id": "Jaringan Benda Terhubung Internet Global." },
    { "en": "Apa Itu 'Sensor' (Sensor)?", "id": "Perangkat Pengubah Besaran Fisik Listrik." },
    { "en": "Apa Itu 'Actuator' (Aktuator)?", "id": "Perangkat Pengubah Listrik Gerak Mekanik." },
    { "en": "Apa Itu 'Sampling Rate' (Laju Pencuplikan)?", "id": "Jumlah Pengambilan Data Per Detik." },
    { "en": "Apa Itu 'Quantization' (Kuantisasi)?", "id": "Pembulatan Nilai Sinyal Ke Digital." },
    { "en": "Apa Itu 'Resolution' (Resolusi ADC)?", "id": "Tingkat Ketelitian Pengubahan Sinyal Digital." },
    { "en": "Apa Itu 'Aliasing' (Aliasing)?", "id": "Distorsi Sinyal Akibat Sampling Rendah." },
    { "en": "Apa Itu 'Nyquist Theorem' (Teorema Nyquist)?", "id": "Syarat Minimal Kecepatan Pencuplikan Sinyal." },
    { "en": "Apa Itu 'Antialiasing Filter' (Filter Antialiasing)?", "id": "Penyaring Sinyal Sebelum Proses Sampling." },
    { "en": "Apa Itu 'Digital Filter' (Filter Digital)?", "id": "Algoritma Pemroses Sinyal Dalam Digital." },
    { "en": "Apa Itu 'FIR Filter' (Finite Impulse Response)?", "id": "Filter Digital Dengan Respon Terbatas." },
    { "en": "Apa Itu 'IIR Filter' (Infinite Impulse Response)?", "id": "Filter Digital Dengan Respon Tak-Terbatas." },
    { "en": "Apa Itu 'Convolution' (Konvolusi)?", "id": "Operasi Matematika Penggabungan Dua Sinyal." },
    { "en": "Apa Itu 'Correlation' (Korelasi)?", "id": "Ukuran Kemiripan Antara Dua Sinyal." },
    { "en": "Apa Itu 'SNR' (Signal To Noise Ratio)?", "id": "Perbandingan Kekuatan Sinyal Dan Derau." },
    { "en": "Apa Itu 'Gain' (Penguatan)?", "id": "Rasio Output Terhadap Input Sinyal." },
    { "en": "Apa Itu 'Phase' (Fase)?", "id": "Posisi Sudut Gelombang Pada Waktu." },
    { "en": "Apa Itu 'Impedance' (Impedansi)?", "id": "Hambatan Total Arus Bolak Balik." },
    { "en": "Apa Itu 'Resonance' (Resonansi)?", "id": "Frekuensi Kondisi Getaran Maksimal Sistem." },
    { "en": "Apa Itu 'Oscillator' (Osilator)?", "id": "Pembangkit Sinyal Gelombang Periodik Listrik." },
    { "en": "Apa Itu 'Operational Amplifier' (Penguat Operasional)?", "id": "Penguat Tegangan Tinggi Impedansi Input." },
    { "en": "Apa Itu 'Feedback' (Umpan Balik)?", "id": "Pengembalian Sebagian Output Ke Input." },
    { "en": "Apa Itu 'Negative Feedback' (Umpan Balik Negatif)?", "id": "Umpan Balik Untuk Menstabilkan Sistem." },
    { "en": "Apa Itu 'Positive Feedback' (Umpan Balik Positif)?", "id": "Umpan Balik Untuk Memicu Osilasi." },
    { "en": "Apa Itu 'Comparator' (Komparator)?", "id": "Sirkuit Pembanding Dua Nilai Tegangan." },
    { "en": "Apa Itu 'Schmitt Trigger' (Pemicu Schmitt)?", "id": "Komparator Dengan Efek Histeresis Stabil." },
    { "en": "Apa Itu 'Multivibrator' (Multivibrator)?", "id": "Sirkuit Pembangkit Gelombang Kotak Digital." },
    { "en": "Apa Itu 'Monostable' (Monostabil)?", "id": "Multivibrator Dengan Satu Kondisi Stabil." },
    { "en": "Apa Itu 'Astable' (Astabil)?", "id": "Multivibrator Tanpa Adanya Kondisi Stabil." },
    { "en": "Apa Itu 'Bistable' (Bistabil)?", "id": "Multivibrator Dengan Dua Kondisi Stabil." },
    { "en": "Apa Itu 'Logic Gate' (Gerbang Logika)?", "id": "Blok Dasar Pembentuk Sirkuit Digital." },
    { "en": "Apa Itu 'Truth Table' (Tabel Kebenaran)?", "id": "Tabel Representasi Output Logika Digital." },
    { "en": "Apa Itu 'DOF' (Degrees Of Freedom)?", "id": "Jumlah Arah Gerakan Independen Robot." },
    { "en": "Apa Itu 'SCARA' (Selective Compliance Assembly Robot Arm)?", "id": "Robot Rakit Industri Lengan Horizontal." },
    { "en": "Apa Itu 'End Effector' (Alat Ujung Robot)?", "id": "Perangkat Kerja Pada Ujung Lengan." },
    { "en": "Apa Itu 'Cobot' (Collaborative Robot)?", "id": "Robot Bekerja Aman Bersama Manusia." },
    { "en": "Apa Itu 'SLAM' (Simultaneous Localization And Mapping)?", "id": "Navigasi Mandiri Robot Memetakan Ruang." },
    { "en": "Apa Itu 'Lidar' (Light Detection And Ranging)?", "id": "Sensor Jarak Pantulan Cahaya Laser." },
    { "en": "Apa Itu 'IMU' (Inertial Measurement Unit)?", "id": "Sensor Gerak, Orientasi, Dan Percepatan." },
    { "en": "Apa Itu 'Servo' (Motor Servo)?", "id": "Motor Dengan Kendali Posisi Presisi." },
    { "en": "Apa Itu 'Stepper' (Motor Langkah)?", "id": "Motor Bergerak Dalam Sudut Bertahap." },
    { "en": "Apa Itu 'Encoder' (Enkoder Rotasi)?", "id": "Sensor Pengukur Posisi Putaran Poros." },
    { "en": "Apa Itu 'Inverse Kinematics' (Kinematika Terbalik)?", "id": "Perhitungan Sudut Sendi Posisi Robot." },
    { "en": "Apa Itu 'Forward Kinematics' (Kinematika Maju)?", "id": "Perhitungan Posisi Ujung Sudut Sendi." },
    { "en": "Apa Itu 'Haptic Feedback' (Umpan Balik Haptik)?", "id": "Simulasi Rasa Sentuhan Melalui Getaran." },
    { "en": "Apa Itu 'PID' (Proportional Integral Derivative)?", "id": "Algoritma Kendali Stabilisasi Sistem Otomatis." },
    { "en": "Apa Itu 'Dead Reckoning' (Navigasi Perkiraan)?", "id": "Estimasi Posisi Berdasarkan Data Sebelumnya." },
    { "en": "Apa Itu 'Path Planning' (Perencanaan Jalur)?", "id": "Penentuan Rute Robot Tanpa Tabrakan." },
    { "en": "Apa Itu 'AGV' (Automated Guided Vehicle)?", "id": "Kendaraan Pengangkut Otomatis Jalur Tetap." },
    { "en": "Apa Itu 'AMR' (Autonomous Mobile Robot)?", "id": "Robot Bergerak Mandiri Tanpa Jalur." },
    { "en": "Apa Itu 'Mechatronics' (Mekatronika)?", "id": "Gabungan Mekanik, Elektronika, Dan Komputer." },
    { "en": "Apa Itu 'PLC' (Programmable Logic Controller)?", "id": "Kontroler Digital Khusus Industri Terprogram." },
    { "en": "Apa Itu 'Ladder Diagram' (Diagram Tangga)?", "id": "Bahasa Pemrograman Grafis Standar Industri." },
    { "en": "Apa Itu 'HMI' (Human Machine Interface)?", "id": "Layar Antarmuka Pengguna Dan Mesin." },
    { "en": "Apa Itu 'Smart Grid' (Jaringan Listrik Cerdas)?", "id": "Jaringan Listrik Digital Dua Arah." },
    { "en": "Apa Itu 'Microgrid' (Jaringan Listrik Mikro)?", "id": "Sistem Energi Lokal Mandiri Terpisah." },
    { "en": "Apa Itu 'VPP' (Virtual Power Plant)?", "id": "Kumpulan Sumber Energi Terdistribusi Digital." },
    { "en": "Apa Itu 'BESS' (Battery Energy Storage System)?", "id": "Sistem Penyimpanan Energi Baterai Besar." },
    { "en": "Apa Itu 'Inverter' (Pembalik Arus)?", "id": "Pengubah Arus Searah Menjadi Bolak-Balik." },
    { "en": "Apa Itu 'Rectifier' (Penyearah Arus)?", "id": "Pengubah Arus Bolak-Balik Menjadi Searah." },
    { "en": "Apa Itu 'MPPT' (Maximum Power Point Tracking)?", "id": "Optimasi Output Daya Panel Surya." },
    { "en": "Apa Itu 'Net Metering' (Meteran Net)?", "id": "Sistem Ekspor Impor Energi Listrik." },
    { "en": "Apa Itu 'EV' (Electric Vehicle)?", "id": "Kendaraan Dengan Penggerak Motor Listrik." },
    { "en": "Apa Itu 'HEV' (Hybrid Electric Vehicle)?", "id": "Kendaraan Mesin Bensin Dan Listrik." },
    { "en": "Apa Itu 'PHEV' (Plug-in Hybrid Electric Vehicle)?", "id": "Kendaraan Hibrida Yang Bisa Diisi." },
    { "en": "Apa Itu 'BEV' (Battery Electric Vehicle)?", "id": "Kendaraan Listrik Murni Bertenaga Baterai." },
    { "en": "Apa Itu 'BMS' (Battery Management System)?", "id": "Sistem Pengawas Dan Pelindung Baterai." },
    { "en": "Apa Itu 'SOC' (State Of Charge)?", "id": "Persentase Sisa Kapasitas Energi Baterai." },
    { "en": "Apa Itu 'SOH' (State Of Health)?", "id": "Kondisi Kesehatan Dan Umur Baterai." },
    { "en": "Apa Itu 'Regenerative Braking' (Pengereman Regeneratif)?", "id": "Pengisian Baterai Saat Melakukan Pengereman." },
    { "en": "Apa Itu 'V2G' (Vehicle To Grid)?", "id": "Penyaluran Energi Mobil Ke Jaringan." },
    { "en": "Apa Itu 'Telematics' (Telematika)?", "id": "Sistem Komunikasi Data Jarak Jauh." },
    { "en": "Apa Itu 'Cybersecurity' (Keamanan Siber)?", "id": "Perlindungan Sistem Dari Serangan Digital." },
    { "en": "Apa Itu 'Encryption' (Enkripsi)?", "id": "Pengubahan Data Menjadi Kode Rahasia." },
    { "en": "Apa Itu 'Firewall' (Dinding Api Jaringan)?", "id": "Penyaring Lalu Lintas Jaringan Komputer." },
    { "en": "Apa Itu 'VPN' (Virtual Private Network)?", "id": "Koneksi Jaringan Internet Pribadi Aman." },
    { "en": "Apa Itu 'Blockchain' (Rantai Blok)?", "id": "Sistem Catatan Transaksi Digital Terdistribusi." },
    { "en": "Apa Itu 'IoT' (Internet Of Things)?", "id": "Konektivitas Benda Fisik Melalui Internet." },
    { "en": "Apa Itu 'Edge Computing' (Komputasi Tepi)?", "id": "Pemrosesan Data Dekat Sumber Data." },
    { "en": "Apa Itu 'Cloud Computing' (Komputasi Awan)?", "id": "Penyimpanan Dan Pemrosesan Data Daring." },
    { "en": "Apa Itu 'Big Data' (Data Besar)?", "id": "Kumpulan Data Volume Sangat Besar." },
    { "en": "Apa Itu 'AI' (Artificial Intelligence)?", "id": "Kecerdasan Buatan Simulasi Proses Manusia." },
    { "en": "Apa Itu 'Machine Learning' (Pembelajaran Mesin)?", "id": "Pengembangan Algoritma Belajar Dari Data." },
    { "en": "Apa Itu 'Deep Learning' (Pembelajaran Mendalam)?", "id": "Jaringan Saraf Tiruan Banyak Lapisan." },
    { "en": "Apa Itu 'Computer Vision' (Visi Komputer)?", "id": "Kemampuan Komputer Mengenali Objek Visual." },
    { "en": "Apa Itu 'NLP' (Natural Language Processing)?", "id": "Pengolahan Bahasa Alami Oleh Komputer." },
    { "en": "Apa Itu 'Digital Twin' (Kembaran Digital)?", "id": "Representasi Virtual Dari Objek Fisik." },
    { "en": "Apa Itu 'IIoT' (Industrial Internet Of Things)?", "id": "Jaringan Perangkat Industri Terkoneksi Internet." },
    { "en": "Apa Itu 'RTU' (Remote Terminal Unit)?", "id": "Perangkat Pengumpul Data Lapangan Industri." },
    { "en": "Apa Itu 'Modbus' (Protokol Modbus)?", "id": "Standar Komunikasi Data Perangkat Industri." },
    { "en": "Apa Itu 'Profibus' (Process Field Bus)?", "id": "Standar Komunikasi Jaringan Lapangan Industri." },
    { "en": "Apa Itu 'Ethernet' (Eternet)?", "id": "Teknologi Jaringan Kabel Lokal Standar." },
    { "en": "Apa Itu 'Fiber Optic' (Serat Optik)?", "id": "Kabel Transmisi Data Melalui Cahaya." },
    { "en": "Apa Itu 'Splicing' (Penyambungan Serat)?", "id": "Proses Penggabungan Dua Ujung Optik." },
    { "en": "Apa Itu 'OTDR' (Optical Time Domain Reflectometer)?", "id": "Alat Ukur Karakteristik Serat Optik." },
    { "en": "Apa Itu 'Bandwidth' (Lebar Pita)?", "id": "Kapasitas Maksimal Pengiriman Data Digital." },
    { "en": "Apa Itu 'Jitter' (Guncangan Waktu)?", "id": "Variasi Waktu Kedatangan Paket Data." },
    { "en": "Apa Itu 'Latency' (Latensi)?", "id": "Waktu Tunda Pengiriman Data Jaringan." },
    { "en": "Apa Itu 'Packet Loss' (Kehilangan Paket)?", "id": "Kegagalan Paket Data Sampai Tujuan." },
    { "en": "Apa Itu 'Modulation' (Modulasi)?", "id": "Penumpangan Sinyal Informasi Ke Pembawa." },
    { "en": "Apa Itu 'Demodulation' (Demodulasi)?", "id": "Pemisahan Sinyal Informasi Dari Pembawa." },
    { "en": "Apa Itu 'Analog To Digital Converter' (Pengubah Analog Ke Digital)?", "id": "Pengubah Sinyal Fisik Menjadi Angka." },
    { "en": "Apa Itu 'Digital To Analog Converter' (Pengubah Digital Ke Analog)?", "id": "Pengubah Angka Menjadi Sinyal Fisik." },
    { "en": "Apa Itu 'Sampling' (Pencuplikan)?", "id": "Proses Pengambilan Nilai Sinyal Analog." },
    { "en": "Apa Itu 'Quantization' (Kuantisasi)?", "id": "Pembulatan Nilai Sinyal Ke Diskrit." },
    { "en": "Apa Itu 'Frequency' (Frekuensi)?", "id": "Jumlah Getaran Gelombang Per Detik." },
    { "en": "Apa Itu 'Amplitude' (Amplitudo)?", "id": "Tinggi Maksimal Gelombang Sinyal Listrik." },
    { "en": "Apa Itu 'Spectrum' (Spektrum Frekuensi)?", "id": "Rentang Gelombang Elektromagnetik Yang Tersedia." },
    { "en": "Apa Itu 'Resonance' (Resonansi)?", "id": "Getaran Maksimal Pada Frekuensi Tertentu." },
    { "en": "Apa Itu 'Harmonics' (Harmonisa)?", "id": "Gangguan Gelombang Kelipatan Frekuensi Dasar." },
    { "en": "Apa Itu 'Filtering' (Penyaringan)?", "id": "Proses Pembuangan Komponen Sinyal Pengganggu." },
    { "en": "Apa Itu 'Noise' (Derau)?", "id": "Sinyal Tidak Diinginkan Pengganggu Informasi." },
    { "en": "Apa Itu 'Signal To Noise Ratio' (Rasio Sinyal Terhadap Derau)?", "id": "Perbandingan Kekuatan Sinyal Dan Gangguan." },
    { "en": "Apa Itu 'Impedance' (Impedansi)?", "id": "Hambatan Total Arus Bolak-Balik Listrik." },
    { "en": "Apa Itu 'Inductance' (Induktansi)?", "id": "Kemampuan Menyimpan Energi Medan Magnet." },
    { "en": "Apa Itu 'Capacitance' (Kapasitansi)?", "id": "Kemampuan Menyimpan Energi Medan Listrik." },
    { "en": "Apa Itu 'Conductance' (Konduktansi)?", "id": "Kemudahan Aliran Arus Listrik Searah." },
    { "en": "Apa Itu 'Ohm's Law' (Hukum Ohm)?", "id": "Hubungan Tegangan, Arus, Dan Hambatan." },
    { "en": "Apa Itu 'Kirchhoff's Current Law' (Hukum Arus Kirchhoff)?", "id": "Total Arus Masuk Titik Sama." },
    { "en": "Apa Itu 'Kirchhoff's Voltage Law' (Hukum Tegangan Kirchhoff)?", "id": "Total Tegangan Dalam Loop Nol." },
    { "en": "Apa Itu 'Thevenin's Theorem' (Teorema Thevenin)?", "id": "Penyederhanaan Rangkaian Menjadi Sumber Tunggal." },
    { "en": "Apa Itu 'Norton's Theorem' (Teorema Norton)?", "id": "Penyederhanaan Rangkaian Menjadi Arus Tunggal." },
    { "en": "Apa Itu 'Superposition Theorem' (Teorema Superposisi)?", "id": "Analisis Rangkaian Dengan Banyak Sumber." },
    { "en": "Apa Itu 'Maximum Power Transfer' (Transfer Daya Maksimal)?", "id": "Kondisi Beban Sama Dengan Sumber." },
    { "en": "Apa Itu 'Power Factor Correction' (Perbaikan Faktor Daya)?", "id": "Proses Pengurangan Penggunaan Daya Reaktif." },
    { "en": "Apa Itu 'Star-Delta Transformation' (Transformasi Bintang-Delta)?", "id": "Konversi Hubungan Rangkaian Tiga Fase." },
    { "en": "Apa Itu 'Three Phase System' (Sistem Tiga Fase)?", "id": "Sistem Distribusi Listrik Tiga Kabel." },
    { "en": "Apa Itu 'HVDC' (High Voltage Direct Current)?", "id": "Transmisi Listrik Searah Tegangan Tinggi." },
    { "en": "Apa Itu 'IGBT' (Insulated Gate Bipolar Transistor)?", "id": "Saklar Elektronik Daya Kecepatan Tinggi." },
    { "en": "Apa Itu 'THD' (Total Harmonic Distortion)?", "id": "Persentase Distorsi Harmonisa Gelombang Listrik." },
    { "en": "Apa Itu 'VFD' (Variable Frequency Drive)?", "id": "Pengatur Kecepatan Motor Melalui Frekuensi." },
    { "en": "Apa Itu 'UPS' (Uninterruptible Power Supply)?", "id": "Penyuplai Daya Cadangan Tanpa Putus." },
    { "en": "Apa Itu 'BMS' (Battery Management System)?", "id": "Sistem Pengelola Dan Pelindung Baterai." },
    { "en": "Apa Itu 'MPPT' (Maximum Power Point Tracking)?", "id": "Optimasi Penyerapan Daya Panel Surya." },
    { "en": "Apa Itu 'PLC' (Programmable Logic Controller)?", "id": "Kontroler Logika Industri Dapat Diprogram." },
    { "en": "Apa Itu 'HMI' (Human Machine Interface)?", "id": "Antarmuka Visual Manusia Dan Mesin." },
    { "en": "Apa Itu 'LOTO' (Lockout Tagout)?", "id": "Prosedur Isolasi Energi Berbahaya Aman." },
    { "en": "Apa Itu 'PPE' (Personal Protective Equipment)?", "id": "Alat Pelindung Diri Kerja Listrik." },
    { "en": "Apa Itu 'GFCI' (Ground Fault Circuit Interrupter)?", "id": "Pemutus Sirkuit Deteksi Kebocoran Tanah." },
    { "en": "Apa Itu 'AFCI' (Arc Fault Circuit Interrupter)?", "id": "Pemutus Sirkuit Deteksi Percikan Api." },
    { "en": "Apa Itu 'MCCB' (Molded Case Circuit Breaker)?", "id": "Pemutus Arus Kompak Kapasitas Menengah." },
    { "en": "Apa Itu 'ACB' (Air Circuit Breaker)?", "id": "Pemutus Arus Besar Media Udara." },
    { "en": "Apa Itu 'VCB' (Vacuum Circuit Breaker)?", "id": "Pemutus Arus Besar Media Vakum." },
    { "en": "Apa Itu 'SF6' (Sulfur Hexafluoride Circuit Breaker)?", "id": "Pemutus Arus Media Gas Isolasi." },
    { "en": "Apa Itu 'CT' (Current Transformer)?", "id": "Transformator Pengubah Arus Skala Besar." },
    { "en": "Apa Itu 'PT' (Potential Transformer)?", "id": "Transformator Pengubah Tegangan Skala Besar." },
    { "en": "Apa Itu 'RTU' (Remote Terminal Unit)?", "id": "Unit Pengumpul Data Lapangan Jarak-Jauh." },
    { "en": "Apa Itu 'PFC' (Power Factor Correction)?", "id": "Sistem Perbaikan Nilai Faktor Daya." },
    { "en": "Apa Itu 'THD-V' (Total Harmonic Distortion Voltage)?", "id": "Distorsi Harmonisa Pada Tegangan Listrik." },
    { "en": "Apa Itu 'THD-I' (Total Harmonic Distortion Current)?", "id": "Distorsi Harmonisa Pada Arus Listrik." },
    { "en": "Apa Itu 'RMS' (Root Mean Square)?", "id": "Nilai Efektif Arus Bolak Balik." },
    { "en": "Apa Itu 'VAR' (Volt Ampere Reactive)?", "id": "Satuan Ukuran Untuk Daya Reaktif." },
    { "en": "Apa Itu 'KWh' (Kilowatt Hour)?", "id": "Satuan Ukuran Konsumsi Energi Listrik." },
    { "en": "Apa Itu 'ADC' (Analog To Digital Converter)?", "id": "Pengubah Sinyal Fisik Menjadi Digital." },
    { "en": "Apa Itu 'DAC' (Digital To Analog Converter)?", "id": "Pengubah Sinyal Digital Menjadi Fisik." },
    { "en": "Apa Itu 'PWM' (Pulse Width Modulation)?", "id": "Modulasi Lebar Pulsa Kendali Tegangan." },
    { "en": "Apa Itu 'MCU' (Microcontroller Unit)?", "id": "Komputer Kecil Dalam Satu Chip." },
    { "en": "Apa Itu 'RAM' (Random Access Memory)?", "id": "Memori Penyimpanan Data Sementara Cepat." },
    { "en": "Apa Itu 'ROM' (Read Only Memory)?", "id": "Memori Penyimpanan Data Permanen Baca." },
    { "en": "Apa Itu 'CAN' (Controller Area Network)?", "id": "Protokol Komunikasi Data Sistem Otomotif." },
    { "en": "Apa Itu 'IoT' (Internet Of Things)?", "id": "Jaringan Benda Fisik Terkoneksi Internet." },
    { "en": "Apa Itu 'RTD' (Resistance Temperature Detector)?", "id": "Sensor Suhu Berbasis Perubahan Hambatan." },
    { "en": "Apa Itu 'NTC' (Negative Temperature Coefficient)?", "id": "Hambatan Turun Saat Suhu Naik." },
    { "en": "Apa Itu 'PTC' (Positive Temperature Coefficient)?", "id": "Hambatan Naik Saat Suhu Naik." },
    { "en": "Apa Itu 'SCR' (Silicon Controlled Rectifier)?", "id": "Dioda Terkendali Untuk Saklar Daya." },
    { "en": "Apa Itu 'TRIAC' (Triode For Alternating Current)?", "id": "Saklar Elektronik Arus Bolak Balik." },
    { "en": "Apa Itu 'EMI' (Electromagnetic Interference)?", "id": "Gangguan Gelombang Elektromagnetik Tidak Diinginkan." },
    { "en": "Apa Itu 'EMC' (Electromagnetic Compatibility)?", "id": "Kemampuan Perangkat Beroperasi Tanpa Gangguan." },
    { "en": "Apa Itu 'RF' (Radio Frequency)?", "id": "Spektrum Frekuensi Gelombang Radio Komunikasi." },
    { "en": "Apa Itu 'VSWR' (Voltage Standing Wave Ratio)?", "id": "Rasio Gelombang Berdiri Tegangan Antena." },
    { "en": "Apa Itu 'BER' (Bit Error Rate)?", "id": "Rasio Kesalahan Bit Transmisi Digital." },
    { "en": "Apa Itu 'QAM' (Quadrature Amplitude Modulation)?", "id": "Modulasi Amplitudo Dan Fase Sekaligus." },
    { "en": "Apa Itu 'OFDM' (Orthogonal Frequency Division Multiplexing)?", "id": "Teknik Modulasi Banyak Sub-Pembawa Orto-Gonal." },
    { "en": "Apa Itu 'GPS' (Global Positioning System)?", "id": "Sistem Navigasi Satelit Global Akurat." },
    { "en": "Apa Itu 'Li-ion' (Lithium Ion Battery)?", "id": "Teknologi Baterai Isi Ulang Populer." },
    { "en": "Apa Itu 'SOC' (State Of Charge)?", "id": "Persentase Sisa Energi Dalam Baterai." },
    { "en": "Apa Itu 'DOD' (Depth Of Discharge)?", "id": "Persentase Energi Yang Diambil Baterai." },
    { "en": "Apa Itu 'V2G' (Vehicle To Grid)?", "id": "Transfer Energi Mobil Ke Jaringan." },
    { "en": "Apa Itu 'EV' (Electric Vehicle)?", "id": "Kendaraan Bertenaga Motor Listrik Murni." },
    { "en": "Apa Itu 'BESS' (Battery Energy Storage System)?", "id": "Sistem Penyimpanan Energi Baterai Skala." },
    { "en": "Apa Itu 'PV' (Photovoltaic)?", "id": "Teknologi Pengubah Cahaya Menjadi Listrik." },
    { "en": "Apa Itu 'CSP' (Concentrated Solar Power)?", "id": "Pembangkit Surya Berbasis Konsentrasi Panas." },
    { "en": "Apa Itu 'BOS' (Balance Of System)?", "id": "Komponen Pendukung Instalasi Panel Surya." },
    { "en": "Apa Itu 'STC' (Standard Test Conditions)?", "id": "Kondisi Standar Pengujian Efisiensi Surya." },
    { "en": "Apa Itu 'FACTS' (Flexible AC Transmission Systems)?", "id": "Sistem Transmisi Arus Bolak-Balik Fleksibel." },
    { "en": "Apa Itu 'STATCOM' (Static Synchronous Compensator)?", "id": "Kompensator Daya Reaktif Berbasis Inverter." },
    { "en": "Apa Itu 'SVC' (Static Var Compensator)?", "id": "Kompensator Daya Reaktif Berbasis Tiristor." },
    { "en": "Apa Itu 'UPFC' (Unified Power Flow Controller)?", "id": "Pengatur Aliran Daya Sistem Fleksibel." },
    { "en": "Apa Itu 'LCC' (Line Commutated Converter)?", "id": "Konverter Berbasis Komutasi Jaringan Listrik." },
    { "en": "Apa Itu 'VSC' (Voltage Source Converter)?", "id": "Konverter Berbasis Sumber Tegangan Searah." },
    { "en": "Apa Itu 'DER' (Distributed Energy Resources)?", "id": "Sumber Energi Tersebar Kapasitas Kecil." },
    { "en": "Apa Itu 'VPP' (Virtual Power Plant)?", "id": "Pembangkit Listrik Virtual Terpusat Digital." },
    { "en": "Apa Itu 'AMI' (Advanced Metering Infrastructure)?", "id": "Infrastruktur Meteran Listrik Pintar Digital." },
    { "en": "Apa Itu 'BIL' (Basic Insulation Level)?", "id": "Batas Ketahanan Isolasi Terhadap Surja." },
    { "en": "Apa Itu 'IP67' (Ingress Protection 67)?", "id": "Standar Tahan Debu, Rendaman Air." },
    { "en": "Apa Itu 'IK10' (Impact Protection 10)?", "id": "Standar Tahan Benturan Mekanik Maksimal." },
    { "en": "Apa Itu 'LOTO' (Lockout Tagout)?", "id": "Sistem Penguncian Keamanan Perbaikan Alat." },
    { "en": "Apa Itu 'SDLC' (Software Development Life Cycle)?", "id": "Siklus Hidup Pengembangan Perangkat Lunak." },
    { "en": "Apa Itu 'Agile' (Metodologi Agile)?", "id": "Pengembangan Perangkat Lunak Adaptif, Iteratif." },
    { "en": "Apa Itu 'Scrum' (Kerangka Kerja Scrum)?", "id": "Manajemen Proyek Tim Secara Iteratif." },
    { "en": "Apa Itu 'DevOps' (Development And Operations)?", "id": "Integrasi Pengembangan Dan Operasi Sistem." },
    { "en": "Apa Itu 'CI/CD' (Continuous Integration Continuous Deployment)?", "id": "Otomasi Integrasi Dan Pengiriman Perangkat." },
    { "en": "Apa Itu 'Microservices' (Arsitektur Mikroservis)?", "id": "Layanan Kecil Mandiri Terintegrasi Sistem." },
    { "en": "Apa Itu 'Docker' (Teknologi Docker)?", "id": "Platform Kontainerisasi Aplikasi Secara Portabel." },
    { "en": "Apa Itu 'Kubernetes' (Orkestrasi Kubernetes)?", "id": "Sistem Kelola Kontainer Aplikasi Otomatis." },
    { "en": "Apa Itu 'Git' (Sistem Kontrol Versi)?", "id": "Pelacak Perubahan Kode Sumber Perangkat." },
    { "en": "Apa Itu 'Frontend' (Pengembangan Sisi Depan)?", "id": "Bagian Aplikasi Berinteraksi Dengan Pengguna." },
    { "en": "Apa Itu 'Backend' (Pengembangan Sisi Belakang)?", "id": "Logika Data Internal Sebuah Aplikasi." },
    { "en": "Apa Itu 'Fullstack' (Pengembangan Fullstack)?", "id": "Kombinasi Pengembangan Sisi Depan, Belakang." },
    { "en": "Apa Itu 'REST' (Representational State Transfer)?", "id": "Standar Arsitektur Layanan Web Modern." },
    { "en": "Apa Itu 'GraphQL' (Bahasa Kueri Grafik)?", "id": "Bahasa Kueri Data Untuk API." },
    { "en": "Apa Itu 'SDK' (Software Development Kit)?", "id": "Paket Alat Pengembangan Perangkat Lunak." },
    { "en": "Apa Itu 'IDE' (Integrated Development Environment)?", "id": "Aplikasi Pusat Untuk Menulis Kode." },
    { "en": "Apa Itu 'Database' (Basis Data)?", "id": "Kumpulan Data Terstruktur Secara Digital." },
    { "en": "Apa Itu 'SQL' (Structured Query Language)?", "id": "Bahasa Kueri Basis Data Relasional." },
    { "en": "Apa Itu 'NoSQL' (Not Only SQL)?", "id": "Sistem Basis Data Non-Relasional Fleksibel." },
    { "en": "Apa Itu 'ORM' (Object Relational Mapping)?", "id": "Teknik Manipulasi Database Melalui Objek." },
    { "en": "Apa Itu 'Cloud Computing' (Komputasi Awan)?", "id": "Layanan Sumber Daya Komputer Online." },
    { "en": "Apa Itu 'SaaS' (Software As A Service)?", "id": "Layanan Perangkat Lunak Berbasis Langganan." },
    { "en": "Apa Itu 'IaaS' (Infrastructure As A Service)?", "id": "Layanan Infrastruktur IT Melalui Awan." },
    { "en": "Apa Itu 'PaaS' (Platform As A Service)?", "id": "Layanan Platform Pengembangan Melalui Awan." },
    { "en": "Apa Itu 'Serverless' (Komputasi Tanpa Server)?", "id": "Eksekusi Kode Tanpa Kelola Infrastruktur." },
    { "en": "Apa Itu 'Bandwidth' (Lebar Pita)?", "id": "Kapasitas Maksimum Pengiriman Data Jaringan." },
    { "en": "Apa Itu 'Throughput' (Throughput Data)?", "id": "Jumlah Data Berhasil Dikirimkan Sebenarnya." },
    { "en": "Apa Itu 'Encryption' (Enkripsi Data)?", "id": "Pengacakan Data Menjadi Kode Rahasia." },
    { "en": "Apa Itu 'Decryption' (Dekripsi Data)?", "id": "Proses Pembacaan Kembali Data Enkripsi." },
    { "en": "Apa Itu 'Firewall' (Dinding Api)?", "id": "Sistem Keamanan Penyaring Trafik Jaringan." },
    { "en": "Apa Itu 'VPN' (Virtual Private Network)?", "id": "Koneksi Jaringan Privat Yang Aman." },
    { "en": "Apa Itu 'SSL' (Secure Sockets Layer)?", "id": "Protokol Keamanan Komunikasi Internet Enkripsi." },
    { "en": "Apa Itu 'TLS' (Transport Layer Security)?", "id": "Versi Modern Pengamanan Komunikasi Data." },
    { "en": "Apa Itu 'Unit Testing' (Uji Unit)?", "id": "Pengujian Bagian Terkecil Kode Program." },
    { "en": "Apa Itu 'Integration Testing' (Uji Integrasi)?", "id": "Pengujian Hubungan Antar Modul Perangkat." },
    { "en": "Apa Itu 'Regression Testing' (Uji Regresi)?", "id": "Memastikan Fitur Lama Tetap Berfungsi." },
    { "en": "Apa Itu 'QA' (Quality Assurance)?", "id": "Penjaminan Kualitas Proses Pengembangan Perangkat." },
    { "en": "Apa Itu 'Bug' (Kesalahan Program)?", "id": "Kesalahan Logika Dalam Perangkat Lunak." },
    { "en": "Apa Itu 'Debug' (Debugging)?", "id": "Proses Pencarian Dan Perbaikan Kesalahan." },
    { "en": "Apa Itu 'Compiler' (Kompilator)?", "id": "Penerjemah Kode Ke Bahasa Mesin." },
    { "en": "Apa Itu 'Interpreter' (Interpretator)?", "id": "Eksekutor Kode Program Secara Langsung." },
    { "en": "Apa Itu 'Framework' (Kerangka Kerja)?", "id": "Struktur Dasar Pengembangan Perangkat Lunak." },
    { "en": "Apa Itu 'Library' (Pustaka Program)?", "id": "Kumpulan Fungsi Siap Pakai Program." },
    { "en": "Apa Itu 'Open Source' (Sumber Terbuka)?", "id": "Kode Sumber Bebas Diakses Umum." },
    { "en": "Apa Itu 'Proprietary' (Perangkat Berpemilik)?", "id": "Perangkat Lunak Dengan Hak Eksklusif." },
    { "en": "Apa Itu 'Middleware' (Perangkat Menengah)?", "id": "Penghubung Antar Aplikasi Atau Sistem." },
    { "en": "Apa Itu 'Cache' (Memori Singgah)?", "id": "Penyimpanan Data Sementara Kecepatan Tinggi." },
    { "en": "Apa Itu 'Algorithm' (Algoritma)?", "id": "Langkah Logis Penyelesaian Masalah Tertentu." },
    { "en": "Apa Itu 'Big O Notation' (Notasi Big O)?", "id": "Ukuran Kompleksitas Efisiensi Algoritma Program." },
    { "en": "Apa Itu 'Recursion' (Rekursi)?", "id": "Fungsi Yang Memanggil Dirinya Sendiri." },
    { "en": "Apa Itu 'Data Structure' (Struktur Data)?", "id": "Cara Mengatur Data Dalam Memori." },
    { "en": "Apa Itu 'Array' (Larik Data)?", "id": "Kumpulan Data Dengan Tipe Sama." },
    { "en": "Apa Itu 'Linked List' (Senarai Berantai)?", "id": "Struktur Data Linier Elemen Terhubung." },
    { "en": "Apa Itu 'Stack' (Tumpukan Data)?", "id": "Struktur Data Masuk Terakhir Keluar." },
    { "en": "Apa Itu 'Queue' (Antrean Data)?", "id": "Struktur Data Masuk Pertama Keluar." },
    { "en": "Apa Itu 'Hash Table' (Tabel Hash)?", "id": "Penyimpanan Data Menggunakan Kunci Unik." },
    { "en": "Apa Itu 'Binary Tree' (Pohon Biner)?", "id": "Struktur Data Dengan Dua Cabang." },
    { "en": "Apa Itu 'JSON' (JavaScript Object Notation)?", "id": "Format Pertukaran Data Ringan Teks." },
    { "en": "Apa Itu 'XML' (eXtensible Markup Language)?", "id": "Bahasa Penanda Untuk Struktur Data." },
    { "en": "Apa Itu 'HTML' (HyperText Markup Language)?", "id": "Bahasa Struktur Utama Halaman Web." },
    { "en": "Apa Itu 'CSS' (Cascading Style Sheets)?", "id": "Bahasa Pengatur Tampilan Visual Web." },
    { "en": "Apa Itu 'DOM' (Document Object Model)?", "id": "Representasi Struktur Dokumen Halaman Web." },
    { "en": "Apa Itu 'CDN' (Content Delivery Network)?", "id": "Jaringan Pengirim Konten Geografis Terdekat." },
    { "en": "Apa Itu 'Load Balancer' (Penyeimbang Beban)?", "id": "Distribusi Trafik Ke Banyak Server." },
    { "en": "Apa Itu 'High Availability' (Ketersediaan Tinggi)?", "id": "Sistem Tetap Aktif Tanpa Gangguan." },
    { "en": "Apa Itu 'Scalability' (Skalabilitas Sistem)?", "id": "Kemampuan Sistem Menangani Peningkatan Beban." },
    { "en": "Apa Itu 'Refactoring' (Penataan Ulang Kode)?", "id": "Perbaikan Struktur Kode Tanpa Ubah." },
    { "en": "Apa Itu 'Technical Debt' (Utang Teknis)?", "id": "Biaya Perbaikan Akibat Pengembangan Cepat." },
    { "en": "Apa Itu 'UML' (Unified Modeling Language)?", "id": "Bahasa Visual Pemodelan Desain Perangkat." },
    { "en": "Apa Itu 'Design Pattern' (Pola Desain)?", "id": "Solusi Umum Masalah Desain Software." },
    { "en": "Apa Itu 'Singleton' (Pola Singleton)?", "id": "Kelas Dengan Hanya Satu Instans." },
    { "en": "Apa Itu 'Observer Pattern' (Pola Pengamat)?", "id": "Sistem Notasi Perubahan Antar Objek." },
    { "en": "Apa Itu 'MVC' (Model View Controller)?", "id": "Pemisahan Logika, Data, Dan Tampilan." },
    { "en": "Apa Itu 'MVVM' (Model View ViewModel)?", "id": "Pola Arsitektur Pemisahan Antarmuka Grafis." },
    { "en": "Apa Itu 'SOLID' (Prinsip Desain SOLID)?", "id": "Lima Prinsip Desain Objek Berorientasi." },
    { "en": "Apa Itu 'OOP' (Object Oriented Programming)?", "id": "Pemrograman Berbasis Objek Dan Kelas." },
    { "en": "Apa Itu 'Functional Programming' (Pemrograman Fungsional)?", "id": "Pemrograman Berbasis Evaluasi Fungsi Matematika." },
    { "en": "Apa Itu 'Thread' (Utas Prosesor)?", "id": "Unit Terkecil Eksekusi Program Komputer." },
    { "en": "Apa Itu 'Concurrency' (Konkurensi)?", "id": "Kemampuan Menjalankan Banyak Tugas Sekaligus." },
    { "en": "Apa Itu 'Parallelism' (Paralelisme)?", "id": "Eksekusi Banyak Tugas Secara Bersamaan." },
    { "en": "Apa Itu 'Asynchronous' (Asinkron)?", "id": "Proses Berjalan Tanpa Menunggu Selesai." },
    { "en": "Apa Itu 'Callback' (Fungsi Panggil Balik)?", "id": "Fungsi Dieksekusi Setelah Tugas Selesai." },
    { "en": "Apa Itu 'Promise' (Janji Data)?", "id": "Objek Representasi Hasil Operasi Asinkron." },
    { "en": "Apa Itu 'Await' (Tunggu Asinkron)?", "id": "Menunggu Hasil Operasi Asinkron Selesai." },
    { "en": "Apa Itu 'Shell' (Antarmuka Baris Perintah)?", "id": "Program Penghubung Pengguna Dan Kernel." },
    { "en": "Apa Itu 'SSH' (Secure Shell)?", "id": "Akses Kendali Jarak Jauh Secara." },
    { "en": "Apa Itu 'Bash' (Bourne Again Shell)?", "id": "Bahasa Perintah Standar Sistem Unix." },
    { "en": "Apa Itu 'Root' (Pengguna Akar)?", "id": "Pengguna Dengan Akses Sistem Tertinggi." },
    { "en": "Apa Itu 'Sudo' (Superuser Do)?", "id": "Perintah Eksekusi Sebagai Pengguna Akar." },
    { "en": "Apa Itu 'Kernel' (Inti Sistem)?", "id": "Bagian Utama Pengelola Sumber Daya." },
    { "en": "Apa Itu 'Virtualization' (Virtualisasi)?", "id": "Teknologi Pembuatan Versi Virtual Hardware." },
    { "en": "Apa Itu 'Hypervisor' (Pengelola Mesin Virtual)?", "id": "Perangkat Lunak Pembuat Mesin Virtual." },
    { "en": "Apa Itu 'Sandbox' (Kotak Pasir)?", "id": "Lingkungan Terisolasi Untuk Pengujian Aman." },
    { "en": "Apa Itu 'Payload' (Beban Muatan)?", "id": "Isi Pesan Utama Data Digital." },
    { "en": "Apa Itu 'Cookie' (Kuki Web)?", "id": "Data Kecil Penyimpan Informasi Penjelajahan." },
    { "en": "Apa Itu 'Session' (Sesi Pengguna)?", "id": "Durasi Interaksi Pengguna Dengan Aplikasi." },
    { "en": "Apa Itu 'SQLi' (Structured Query Language Injection)?", "id": "Injeksi Perintah Basis Data Berbahaya." },
    { "en": "Apa Itu 'XSS' (Cross-Site Scripting)?", "id": "Injeksi Skrip Berbahaya Halaman Web." },
    { "en": "Apa Itu 'CSRF' (Cross-Site Request Forgery)?", "id": "Pemalsuan Permintaan Antar Situs Pengguna." },
    { "en": "Apa Itu 'DDoS' (Distributed Denial Of Service)?", "id": "Serangan Melumpuhkan Layanan Trafik Massal." },
    { "en": "Apa Itu 'Brute Force' (Serangan Paksa)?", "id": "Percobaan Kata Sandi Secara Berulang." },
    { "en": "Apa Itu 'Phishing' (Pengelabuan Digital)?", "id": "Penipuan Pencurian Informasi Sensitif Pengguna." },
    { "en": "Apa Itu 'Ransomware' (Perangkat Lunak Tebusan)?", "id": "Perangkat Lunak Pengunci Data Tebusan." },
    { "en": "Apa Itu 'Firewall' (Dinding Api Jaringan)?", "id": "Sistem Pertahanan Jaringan Komputer Internal." },
    { "en": "Apa Itu 'IDS' (Intrusion Detection System)?", "id": "Sistem Pendeteksi Gangguan Keamanan Jaringan." },
    { "en": "Apa Itu 'IPS' (Intrusion Prevention System)?", "id": "Sistem Pencegah Gangguan Keamanan Jaringan." },
    { "en": "Apa Itu 'WAF' (Web Application Firewall)?", "id": "Dinding Api Khusus Aplikasi Web." },
    { "en": "Apa Itu 'Zero Day' (Celah Hari Nol)?", "id": "Kerentanan Keamanan Belum Diketahui Vendor." },
    { "en": "Apa Itu 'Vulnerability' (Kerentanan Sistem)?", "id": "Kelemahan Keamanan Pada Suatu Sistem." },
    { "en": "Apa Itu 'Exploit' (Eksploitasi Kode)?", "id": "Kode Manfaatkan Kelemahan Keamanan Sistem." },
    { "en": "Apa Itu 'Payload' (Muatan Berbahaya)?", "id": "Bagian Kode Penyerang Eksekusi Target." },
    { "en": "Apa Itu 'Backdoor' (Pintu Belakang)?", "id": "Akses Rahasia Lewati Keamanan Sistem." },
    { "en": "Apa Itu 'Trojan Horse' (Kuda Troya)?", "id": "Program Berbahaya Menyamar Sebagai Legal." },
    { "en": "Apa Itu 'Adware' (Perangkat Iklan Berbahaya)?", "id": "Program Iklan Pengganggu Yang Berbahaya." },
    { "en": "Apa Itu 'Keylogger' (Perekam Ketukan Tombol)?", "id": "Program Perekam Setiap Ketukan Keyboard." },
    { "en": "Apa Itu 'Botnet' (Jaringan Bot)?", "id": "Kumpulan Komputer Terinfeksi Kendali Peretas." },
    { "en": "Apa Itu 'MitM' (Man In The Middle)?", "id": "Penyadapan Komunikasi Antar Dua Pihak." },
    { "en": "Apa Itu 'Sniffing' (Penyadapan Jaringan)?", "id": "Proses Pemantauan Lalu Lintas Jaringan." },
    { "en": "Apa Itu 'Spoofing' (Pemalsuan Identitas)?", "id": "Pemalsuan Informasi Identitas Dalam Jaringan." },
    { "en": "Apa Itu 'Social Engineering' (Rekayasa Sosial)?", "id": "Manipulasi Psikologis Untuk Informasi Rahasia." },
    { "en": "Apa Itu 'Cryptography' (Kriptografi)?", "id": "Ilmu Teknik Pengamanan Informasi Data." },
    { "en": "Apa Itu 'Symmetric Encryption' (Enkripsi Simetris)?", "id": "Enkripsi Menggunakan Satu Kunci Sama." },
    { "en": "Apa Itu 'Asymmetric Encryption' (Enkripsi Asimetris)?", "id": "Enkripsi Menggunakan Dua Kunci Berbeda." },
    { "en": "Apa Itu 'Public Key' (Kunci Publik)?", "id": "Kunci Terbuka Untuk Mengenkripsi Data." },
    { "en": "Apa Itu 'Private Key' (Kunci Pribadi)?", "id": "Kunci Rahasia Untuk Mendekripsi Data." },
    { "en": "Apa Itu 'Hashing' (Fungsi Hash)?", "id": "Pengubahan Data Menjadi Nilai Unik." },
    { "en": "Apa Itu 'Salt' (Garam Kriptografi)?", "id": "Data Acak Tambahan Untuk Hashing." },
    { "en": "Apa Itu '2FA' (Two-Factor Authentication)?", "id": "Verifikasi Identitas Melalui Dua Cara." },
    { "en": "Apa Itu 'MFA' (Multi-Factor Authentication)?", "id": "Verifikasi Identitas Melalui Banyak Cara." },
    { "en": "Apa Itu 'OTP' (One-Time Password)?", "id": "Kata Sandi Sekali Pakai Dinamis." },
    { "en": "Apa Itu 'Biometrics' (Otentikasi Biometrik)?", "id": "Verifikasi Identitas Berdasarkan Ciri Fisik." },
    { "en": "Apa Itu 'IAM' (Identity And Access Management)?", "id": "Manajemen Identitas Dan Akses Pengguna." },
    { "en": "Apa Itu 'RBAC' (Role-Based Access Control)?", "id": "Kontrol Akses Berdasarkan Peran Pengguna." },
    { "en": "Apa Itu 'ACL' (Access Control List)?", "id": "Daftar Aturan Izin Akses Jaringan." },
    { "en": "Apa Itu 'Principle Of Least Privilege'?", "id": "Pemberian Hak Akses Minimum Dibutuhkan." },
    { "en": "Apa Itu 'Zero Trust' (Arsitektur Tanpa Kepercayaan)?", "id": "Keamanan Dengan Verifikasi Ketat Berkelanjutan." },
    { "en": "Apa Itu 'SOC' (Security Operations Center)?", "id": "Pusat Pengawasan Keamanan Informasi Perusahaan." },
    { "en": "Apa Itu 'SIEM' (Security Information And Event Management)?", "id": "Sistem Analisis Log Keamanan Otomatis." },
    { "en": "Apa Itu 'Incident Response' (Respon Insiden)?", "id": "Tindakan Penanganan Serangan Keamanan Siber." },
    { "en": "Apa Itu 'Forensics' (Digital Forensics)?", "id": "Investigasi Barang Bukti Kejahatan Digital." },
    { "en": "Apa Itu 'Penetration Testing' (Uji Penetrasi)?", "id": "Simulasi Serangan Legal Uji Keamanan." },
    { "en": "Apa Itu 'Ethical Hacking' (Peretasan Etis)?", "id": "Peretasan Legal Untuk Memperbaiki Keamanan." },
    { "en": "Apa Itu 'Blue Team' (Tim Biru)?", "id": "Tim Bertugas Mempertahankan Keamanan Sistem." },
    { "en": "Apa Itu 'Red Team' (Tim Merah)?", "id": "Tim Bertugas Menyerang Uji Pertahanan." },
    { "en": "Apa Itu 'Purple Team' (Tim Ungu)?", "id": "Kolaborasi Antara Tim Penyerang, Bertahan." },
    { "en": "Apa Itu 'Bug Bounty' (Hadiah Bug)?", "id": "Imbalan Menemukan Kerentanan Keamanan Perangkat." },
    { "en": "Apa Itu 'Honey Pot' (Umpan Peretas)?", "id": "Sistem Jebakan Untuk Menarik Peretas." },
    { "en": "Apa Itu 'Sandboxing' (Lingkungan Terisolasi)?", "id": "Eksekusi Program Dalam Lingkungan Terbatas." },
    { "en": "Apa Itu 'Air Gap' (Pemisahan Fisik)?", "id": "Sistem Terisolasi Total Dari Jaringan." },
    { "en": "Apa Itu 'VPN' (Virtual Private Network)?", "id": "Saluran Komunikasi Internet Pribadi Aman." },
    { "en": "Apa Itu 'Proxy Server' (Server Proksi)?", "id": "Perantara Komunikasi Antara Klien, Internet." },
    { "en": "Apa Itu 'Tor' (The Onion Router)?", "id": "Jaringan Komunikasi Anonim Lapisan Enkripsi." },
    { "en": "Apa Itu 'Deep Web' (Web Dalam)?", "id": "Bagian Web Tidak Terindeks Mesin." },
    { "en": "Apa Itu 'Dark Web' (Web Gelap)?", "id": "Bagian Web Tersembunyi Akses Khusus." },
    { "en": "Apa Itu 'Blockchain' (Rantai Blok)?", "id": "Buku Besar Digital Terdistribusi Aman." },
    { "en": "Apa Itu 'Smart Contract' (Kontrak Pintar)?", "id": "Program Eksekusi Perjanjian Otomatis Digital." },
    { "en": "Apa Itu 'NFT' (Non-Fungible Token)?", "id": "Aset Digital Unik Tidak Tergantikan." },
    { "en": "Apa Itu 'DeFi' (Decentralized Finance)?", "id": "Sistem Keuangan Terdesentralisasi Berbasis Blockchain." },
    { "en": "Apa Itu 'Cryptocurrency' (Mata Uang Kripto)?", "id": "Mata Uang Digital Dengan Kriptografi." },
    { "en": "Apa Itu 'Steganography' (Steganografi)?", "id": "Seni Menyembunyikan Pesan Dalam Media." },
    { "en": "Apa Itu 'Data Breach' (Kebocoran Data)?", "id": "Insiden Paparan Data Rahasia Publik." },
    { "en": "Apa Itu 'GDPR' (General Data Protection Regulation)?", "id": "Regulasi Perlindungan Data Pribadi Eropa." },
    { "en": "Apa Itu 'UU PDP' (Undang-Undang Perlindungan Data Pribadi)?", "id": "Hukum Perlindungan Data Pribadi Indonesia." },
    { "en": "Apa Itu 'Compliance' (Kepatuhan Keamanan)?", "id": "Pemenuhan Standar Aturan Keamanan Siber." },
    { "en": "Apa Itu 'ISO 27001' (Standar Keamanan Informasi)?", "id": "Standar Internasional Sistem Manajemen Keamanan." },
    { "en": "Apa Itu 'NIST' (National Institute Of Standards And Technology)?", "id": "Kerangka Kerja Keamanan Siber Amerika." },
    { "en": "Apa Itu 'OWASP' (Open Web Application Security Project)?", "id": "Komunitas Standar Keamanan Aplikasi Web." },
    { "en": "Apa Itu 'Threat Intelligence' (Intelijen Ancaman)?", "id": "Informasi Mengenai Potensi Serangan Siber." },
    { "en": "Apa Itu 'Malvertising' (Iklan Berbahaya)?", "id": "Penyebaran Malware Melalui Iklan Online." },
    { "en": "Apa Itu 'Clickjacking' (Pembajakan Klik)?", "id": "Teknik Penipuan Klik Halaman Tersembunyi." },
    { "en": "Apa Itu 'Rootkit' (Kit Akar)?", "id": "Kumpulan Software Tersembunyi Akses Sistem." },
    { "en": "Apa Itu 'Script Kiddie' (Peretas Amatir)?", "id": "Peretas Menggunakan Alat Buatan Orang." },
    { "en": "Apa Itu 'White Hat' (Peretas Putih)?", "id": "Peretas Beretika Membantu Keamanan Sistem." },
    { "en": "Apa Itu 'Black Hat' (Peretas Hitam)?", "id": "Peretas Berbahaya Mencuri Data Sistem." },
    { "en": "Apa Itu 'Grey Hat' (Peretas Abu)?", "id": "Peretas Antara Etis Dan Ilegal." },
    { "en": "Apa Itu 'Hacktivism' (Aktivisme Peretasan)?", "id": "Peretasan Untuk Tujuan Sosial Politik." },
    { "en": "Apa Itu 'Cyber Warfare' (Perang Siber)?", "id": "Serangan Siber Antar Negara Berkonflik." },
    { "en": "Apa Itu 'Cyber Terrorism' (Terorisme Siber)?", "id": "Serangan Siber Menimbulkan Ketakutan Publik." },
    { "en": "Apa Itu 'Whaling' (Phishing Target Besar)?", "id": "Phishing Incar Pejabat Tinggi Perusahaan." },
    { "en": "Apa Itu 'Vishing' (Voice Phishing)?", "id": "Phishing Melalui Komunikasi Suara Telepon." },
    { "en": "Apa Itu 'Smishing' (SMS Phishing)?", "id": "Phishing Melalui Pesan Teks Singkat." },
    { "en": "Apa Itu 'Dumpster Diving' (Pencarian Sampah)?", "id": "Mencari Informasi Rahasia Di Sampah." },
    { "en": "Apa Itu 'Shoulder Surfing' (Mengintip Bahu)?", "id": "Mengintip Informasi Saat Korban Mengetik." },
    { "en": "Apa Itu 'Tailgating' (Membuntuti Masuk)?", "id": "Masuk Area Terlarang Mengikuti Orang." },
    { "en": "Apa Itu 'Juice Jacking' (Pembajakan Pengisian)?", "id": "Pencurian Data Melalui Port USB." },
    { "en": "Apa Itu 'Evil Twin' (Kembaran Jahat)?", "id": "Titik Akses Nirkabel Palsu Berbahaya." },
    { "en": "Apa Itu 'SQL Map' (Alat Injeksi SQL)?", "id": "Alat Otomasi Eksploitasi Injeksi SQL." },
    { "en": "Apa Itu 'Metasploit' (Kerangka Kerja Eksploitasi)?", "id": "Alat Pengujian Penetrasi Dan Eksploitasi." },
    { "en": "Apa Itu 'Wireshark' (Penganalisis Paket)?", "id": "Alat Analisis Protokol Jaringan Terbuka." },
    { "en": "Apa Itu 'Nmap' (Network Mapper)?", "id": "Alat Pemindaian Jaringan Dan Port." },
    { "en": "Apa Itu 'Burp Suite' (Alat Keamanan Web)?", "id": "Platform Pengujian Keamanan Aplikasi Web." },
    { "en": "Apa Itu 'EC2' (Elastic Compute Cloud)?", "id": "Layanan Server Virtual Fleksibel Awan." },
    { "en": "Apa Itu 'S3' (Simple Storage Service)?", "id": "Layanan Penyimpanan Objek Skala Besar." },
    { "en": "Apa Itu 'Lambda' (AWS Lambda)?", "id": "Layanan Komputasi Tanpa Server Otomatis." },
    { "en": "Apa Itu 'VPC' (Virtual Private Cloud)?", "id": "Jaringan Privat Terisolasi Dalam Awan." },
    { "en": "Apa Itu 'RDS' (Relational Database Service)?", "id": "Layanan Basis Data Relasional Terkelola." },
    { "en": "Apa Itu 'CloudFront' (Amazon CloudFront)?", "id": "Jaringan Pengirim Konten Global Cepat." },
    { "en": "Apa Itu 'Route 53' (Amazon Route 53)?", "id": "Layanan Sistem Nama Domain Awan." },
    { "en": "Apa Itu 'IAM' (Identity And Access Management)?", "id": "Pengaturan Identitas Dan Akses Pengguna." },
    { "en": "Apa Itu 'CloudWatch' (Amazon CloudWatch)?", "id": "Layanan Pemantauan Sumber Daya Awan." },
    { "en": "Apa Itu 'Auto Scaling' (Penskalaan Otomatis)?", "id": "Penyesuaian Kapasitas Server Secara Otomatis." },
    { "en": "Apa Itu 'Elastic Beanstalk' (AWS Elastic Beanstalk)?", "id": "Layanan Deployment Aplikasi Cepat Mudah." },
    { "en": "Apa Itu 'DynamoDB' (Amazon DynamoDB)?", "id": "Basis Data NoSQL Performa Tinggi." },
    { "en": "Apa Itu 'Redshift' (Amazon Redshift)?", "id": "Gudang Data Skala Besar Awan." },
    { "en": "Apa Itu 'Azure Virtual Machines'?", "id": "Layanan Komputasi Server Virtual Microsoft." },
    { "en": "Apa Itu 'Azure Blob Storage'?", "id": "Penyimpanan Data Tak Terstruktur Awan." },
    { "en": "Apa Itu 'Azure SQL Database'?", "id": "Layanan Basis Data Relasional Microsoft." },
    { "en": "Apa Itu 'Azure App Service'?", "id": "Platform Hosting Aplikasi Web Terkelola." },
    { "en": "Apa Itu 'Azure Functions' (Fungsi Azure)?", "id": "Komputasi Tanpa Server Berbasis Peristiwa." },
    { "en": "Apa Itu 'Azure Active Directory'?", "id": "Layanan Manajemen Identitas Dan Akses." },
    { "en": "Apa Itu 'Cosmos DB' (Azure Cosmos DB)?", "id": "Basis Data Multi-Model Terdistribusi Global." },
    { "en": "Apa Itu 'Compute Engine' (Google Compute Engine)?", "id": "Layanan Mesin Virtual Google Cloud." },
    { "en": "Apa Itu 'Cloud Storage' (Google Cloud Storage)?", "id": "Penyimpanan Objek Universal Milik Google." },
    { "en": "Apa Itu 'BigQuery' (Google BigQuery)?", "id": "Analisis Data Besar Kecepatan Tinggi." },
    { "en": "Apa Itu 'Cloud Run' (Google Cloud Run)?", "id": "Eksekusi Kontainer Tanpa Server Otomatis." },
    { "en": "Apa Itu 'App Engine' (Google App Engine)?", "id": "Platform Pengembangan Aplikasi Google Cloud." },
    { "en": "Apa Itu 'Kubernetes Engine' (Google Kubernetes Engine)?", "id": "Layanan Pengelola Kontainer Kubernetes Terkelola." },
    { "en": "Apa Itu 'Pub/Sub' (Google Cloud Pub/Sub)?", "id": "Layanan Pesan Antrean Skala Besar." },
    { "en": "Apa Itu 'Cloud Spanner' (Google Cloud Spanner)?", "id": "Basis Data Relasional Terdistribusi Global." },
    { "en": "Apa Itu 'On-Premise' (Lokal Di Tempat)?", "id": "Infrastruktur IT Milik Sendiri Lokal." },
    { "en": "Apa Itu 'Hybrid Cloud' (Awan Hibrida)?", "id": "Gabungan Awan Privat Dan Publik." },
    { "en": "Apa Itu 'Multi-Cloud' (Banyak Awan)?", "id": "Penggunaan Banyak Penyedia Layanan Awan." },
    { "en": "Apa Itu 'Virtualization' (Virtualisasi Perangkat)?", "id": "Pembuatan Versi Maya Perangkat Fisik." },
    { "en": "Apa Itu 'Container' (Kontainer Aplikasi)?", "id": "Paket Aplikasi Terisolasi Beserta Dependensi." },
    { "en": "Apa Itu 'Docker' (Platform Docker)?", "id": "Alat Pembuat Dan Pengelola Kontainer." },
    { "en": "Apa Itu 'Kubernetes' (Orkestrasi Kontainer)?", "id": "Sistem Otomasi Kelola Banyak Kontainer." },
    { "en": "Apa Itu 'Terraform' (Infrastruktur Kode)?", "id": "Alat Pengelola Infrastruktur Melalui Kode." },
    { "en": "Apa Itu 'Ansible' (Otomasi Konfigurasi)?", "id": "Alat Otomasi Konfigurasi Sistem Komputer." },
    { "en": "Apa Itu 'Serverless' (Komputasi Tanpa Server)?", "id": "Arsitektur Eksekusi Kode Tanpa Server." },
    { "en": "Apa Itu 'Edge Computing' (Komputasi Tepi)?", "id": "Pemrosesan Data Dekat Sumber Informasi." },
    { "en": "Apa Itu 'Data Science' (Ilmu Data)?", "id": "Ekstraksi Pengetahuan Dari Data Besar." },
    { "en": "Apa Itu 'Machine Learning' (Pembelajaran Mesin)?", "id": "Algoritma Belajar Dari Pengalaman Data." },
    { "en": "Apa Itu 'Artificial Intelligence' (Kecerdasan Buatan)?", "id": "Simulasi Kecerdasan Manusia Oleh Mesin." },
    { "en": "Apa Itu 'Neural Network' (Jaringan Saraf)?", "id": "Model Komputasi Terinspirasi Otak Manusia." },
    { "en": "Apa Itu 'Supervised Learning' (Pembelajaran Terawasi)?", "id": "Belajar Dari Data Berlabel Jelas." },
    { "en": "Apa Itu 'Unsupervised Learning' (Pembelajaran Tanpa Pengawasan)?", "id": "Mencari Pola Tersembunyi Data Mentah." },
    { "en": "Apa Itu 'Reinforcement Learning' (Pembelajaran Penguatan)?", "id": "Belajar Melalui Sistem Hadiah Hukuman." },
    { "en": "Apa Itu 'Regression' (Regresi Data)?", "id": "Prediksi Nilai Kontinu Berdasarkan Data." },
    { "en": "Apa Itu 'Classification' (Klasifikasi Data)?", "id": "Pengelompokan Data Ke Dalam Kategori." },
    { "en": "Apa Itu 'Clustering' (Klasterisasi Data)?", "id": "Pengelompokan Data Berdasarkan Kemiripan Sifat." },
    { "en": "Apa Itu 'Overfitting' (Penyesuaian Berlebih)?", "id": "Model Terlalu Fokus Data Latih." },
    { "en": "Apa Itu 'Underfitting' (Kurang Penyesuaian)?", "id": "Model Gagal Menangkap Pola Data." },
    { "en": "Apa Itu 'Data Mining' (Penggalian Data)?", "id": "Proses Penemuan Pola Dalam Database." },
    { "en": "Apa Itu 'Data Warehouse' (Gudang Data)?", "id": "Penyimpanan Data Terstruktur Untuk Analisis." },
    { "en": "Apa Itu 'Data Lake' (Danau Data)?", "id": "Penyimpanan Data Mentah Skala Besar." },
    { "en": "Apa Itu 'ETL' (Extract Transform Load)?", "id": "Proses Integrasi Data Ke Database." },
    { "en": "Apa Itu 'Business Intelligence' (Intelijen Bisnis)?", "id": "Analisis Data Untuk Keputusan Bisnis." },
    { "en": "Apa Itu 'Computer Vision' (Visi Komputer)?", "id": "Kemampuan Mesin Memahami Informasi Visual." },
    { "en": "Apa Itu 'Pandas' (Pustaka Pandas)?", "id": "Alat Analisis Data Bahasa Python." },
    { "en": "Apa Itu 'NumPy' (Numerical Python)?", "id": "Pustaka Komputasi Numerik Matriks Python." },
    { "en": "Apa Itu 'Scikit-Learn' (Pustaka Scikit-Learn)?", "id": "Alat Pembelajaran Mesin Bahasa Python." },
    { "en": "Apa Itu 'TensorFlow' (Platform TensorFlow)?", "id": "Kerangka Kerja Pembelajaran Mendalam Google." },
    { "en": "Apa Itu 'PyTorch' (Platform PyTorch)?", "id": "Kerangka Kerja Pembelajaran Mendalam Meta." },
    { "en": "Apa Itu 'Jupyter Notebook'?", "id": "Aplikasi Web Interaktif Penulisan Kode." },
    { "en": "Apa Itu 'SQL' (Structured Query Language)?", "id": "Bahasa Manajemen Basis Data Relasional." },
    { "en": "Apa Itu 'NoSQL' (Not Only SQL)?", "id": "Sistem Basis Data Non Relasional." },
    { "en": "Apa Itu 'Hadoop' (Apache Hadoop)?", "id": "Kerangka Kerja Penyimpanan Terdistribusi Besar." },
    { "en": "Apa Itu 'Spark' (Apache Spark)?", "id": "Mesin Analisis Data Kecepatan Tinggi." },
    { "en": "Apa Itu 'Kafka' (Apache Kafka)?", "id": "Platform Aliran Data Waktu Nyata." },
    { "en": "Apa Itu 'Airflow' (Apache Airflow)?", "id": "Alat Manajemen Alur Kerja Data." },
    { "en": "Apa Itu 'Tableau' (Perangkat Lunak Tableau)?", "id": "Alat Visualisasi Data Bisnis Interaktif." },
    { "en": "Apa Itu 'Power BI' (Microsoft Power BI)?", "id": "Layanan Analitik Bisnis Visual Microsoft." },
    { "en": "Apa Itu 'Data Cleansing' (Pembersihan Data)?", "id": "Proses Penghilangan Data Tidak Akurat." },
    { "en": "Apa Itu 'Feature Engineering' (Rekayasa Fitur)?", "id": "Pembuatan Variabel Baru Dari Data." },
    { "en": "Apa Itu 'Exploratory Data Analysis' (Analisis Data Eksploratif)?", "id": "Langkah Awal Memahami Karakteristik Data." },
    { "en": "Apa Itu 'Bias' (Bias Data)?", "id": "Ketidakadilan Dalam Pengambilan Sampel Data." },
    { "en": "Apa Itu 'Variance' (Varians Data)?", "id": "Ukuran Penyebaran Nilai Data Statistik." },
    { "en": "Apa Itu 'Confusion Matrix' (Matriks Kebingungan)?", "id": "Tabel Evaluasi Performa Model Klasifikasi." },
    { "en": "Apa Itu 'Accuracy' (Akurasi Model)?", "id": "Rasio Prediksi Benar Terhadap Total." },
    { "en": "Apa Itu 'Precision' (Presisi Model)?", "id": "Rasio Kebenaran Positif Prediksi Model." },
    { "en": "Apa Itu 'Recall' (Sensitivitas Model)?", "id": "Rasio Data Positif Berhasil Dideteksi." },
    { "en": "Apa Itu 'F1 Score' (Skor F1)?", "id": "Rata-Rata Harmonik Presisi Dan Recall." },
    { "en": "Apa Itu 'Algorithm' (Algoritma Komputer)?", "id": "Langkah-Langkah Logis Penyelesaian Masalah Digital." },
    { "en": "Apa Itu 'Model Deployment' (Penyebaran Model)?", "id": "Proses Implementasi Model Ke Produksi." },
    { "en": "Apa Itu 'API Gateway' (Gerbang API)?", "id": "Pintu Utama Manajemen Panggilan Antarmuka." },
    { "en": "Apa Itu 'Microservices' (Layanan Mikro)?", "id": "Arsitektur Aplikasi Berupa Layanan Kecil." },
    { "en": "Apa Itu 'Monolithic' (Arsitektur Monolitik)?", "id": "Aplikasi Besar Dalam Satu Kesatuan." },
    { "en": "Apa Itu 'DevOps' (Development And Operations)?", "id": "Budaya Integrasi Pengembangan Dan Operasional." },
    { "en": "Apa Itu 'CI/CD' (Continuous Integration Continuous Deployment)?", "id": "Otomasi Pengiriman Perangkat Lunak Berkelanjutan." },
    { "en": "Apa Itu 'Elasticity' (Elastisitas Awan)?", "id": "Kemampuan Adaptasi Sumber Daya Cepat." },
    { "en": "Apa Itu 'High Availability' (Ketersediaan Tinggi)?", "id": "Sistem Tetap Aktif Tanpa Henti." },
    { "en": "Apa Itu 'Disaster Recovery' (Pemulihan Bencana)?", "id": "Rencana Pemulihan Data Setelah Insiden." },
    { "en": "Apa Itu 'Backup' (Cadangan Data)?", "id": "Salinan Data Untuk Keamanan Informasi." },
    { "en": "Apa Itu 'Latency' (Latensi Jaringan)?", "id": "Waktu Tunda Pengiriman Paket Data." },
    { "en": "Apa Itu 'Edge Node' (Simpul Tepi)?", "id": "Perangkat Pemroses Data Garis Depan." }



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
