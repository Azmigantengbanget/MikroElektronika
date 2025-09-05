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


  { "en": "Apa itu mikroelektronika?", "id": "Studi fabrikasi komponen elektronik mikro." },
  { "en": "Bahan dasar utama mikroelektronika?", "id": "Semikonduktor silikon (Si)." },
  { "en": "Apa itu semikonduktor?", "id": "Bahan dengan konduktivitas dapat diatur." },
  { "en": "Apa itu konduktor?", "id": "Bahan yang mudah menghantarkan listrik." },
  { "en": "Apa itu isolator?", "id": "Bahan yang sulit menghantarkan listrik." },
  { "en": "Apa itu doping?", "id": "Penambahan atom pengotor pada semikonduktor." },
  { "en": "Tujuan proses doping?", "id": "Mengubah sifat kelistrikan semikonduktor." },
  { "en": "Semikonduktor murni disebut?", "id": "Semikonduktor intrinsik." },
  { "en": "Semikonduktor tak murni disebut?", "id": "Semikonduktor ekstrinsik." },
  { "en": "Doping dengan atom valensi 5?", "id": "Menghasilkan semikonduktor tipe-n." },
  { "en": "Contoh atom dopan tipe-n?", "id": "Fosfor (P), Arsen (As)." },
  { "en": "Dopan tipe-n disebut juga?", "id": "Atom donor." },
  { "en": "Doping dengan atom valensi 3?", "id": "Menghasilkan semikonduktor tipe-p." },
  { "en": "Contoh atom dopan tipe-p?", "id": "Boron (B), Galium (Ga)." },
  { "en": "Dopan tipe-p disebut juga?", "id": "Atom akseptor." },
  { "en": "Pembawa muatan mayoritas tipe-n?", "id": "Elektron." },
  { "en": "Pembawa muatan minoritas tipe-n?", "id": "Hole." },
  { "en": "Pembawa muatan mayoritas tipe-p?", "id": "Hole." },
  { "en": "Pembawa muatan minoritas tipe-p?", "id": "Elektron." },
  { "en": "Apa itu 'hole' dalam semikonduktor?", "id": "Kekosongan elektron pada ikatan kovalen." },
  { "en": "Muatan efektif dari 'hole'?", "id": "Positif." },
  { "en": "Gabungan tipe-p dan tipe-n membentuk?", "id": "Sambungan P-N (P-N junction)." },
  { "en": "Sambungan P-N adalah dasar dari?", "id": "Komponen dioda." },
  { "en": "Daerah di sekitar sambungan P-N?", "id": "Daerah deplesi (lapisan pengosongan)." },
  { "en": "Daerah deplesi tidak memiliki?", "id": "Pembawa muatan bebas." },
  { "en": "Apa itu tegangan penghalang (barrier potential)?", "id": "Medan listrik internal di daerah deplesi." },
  { "en": "Apa itu bias maju (forward bias)?", "id": "Tegangan positif ke sisi-p." },
  { "en": "Efek bias maju pada dioda?", "id": "Arus mengalir dengan mudah." },
  { "en": "Efek bias maju pada deplesi?", "id": "Daerah deplesi menyempit." },
  { "en": "Apa itu bias mundur (reverse bias)?", "id": "Tegangan negatif ke sisi-p." },
  { "en": "Efek bias mundur pada dioda?", "id": "Arus hampir tidak mengalir." },
  { "en": "Efek bias mundur pada deplesi?", "id": "Daerah deplesi melebar." },
  { "en": "Arus kecil pada bias mundur?", "id": "Arus bocor (leakage current)." },
  { "en": "Apa itu dioda?", "id": "Komponen yang melewatkan arus searah." },
  { "en": "Simbol anoda pada dioda menunjuk?", "id": "Sisi material tipe-p." },
  { "en": "Simbol katoda pada dioda menunjuk?", "id": "Sisi material tipe-n." },
  { "en": "Apa itu transistor?", "id": "Komponen semikonduktor untuk saklar/penguat." },
  { "en": "Dua jenis utama transistor?", "id": "BJT dan FET." },
  { "en": "Singkatan BJT (Bipolar Junction Transistor)?", "id": "Bipolar Junction Transistor." },
  { "en": "Singkatan FET (Field-Effect Transistor)?", "id": "Field-Effect Transistor." },
  { "en": "Struktur BJT (Bipolar Junction Transistor)?", "id": "NPN atau PNP." },
  { "en": "Tiga terminal BJT (Bipolar Junction Transistor)?", "id": "Emitor, Basis, Kolektor." },
  { "en": "BJT (Bipolar Junction Transistor) dikontrol oleh?", "id": "Arus pada terminal basis." },
  { "en": "FET (Field-Effect Transistor) dikontrol oleh?", "id": "Tegangan pada terminal gerbang." },
  { "en": "Jenis FET (Field-Effect Transistor) paling umum?", "id": "MOSFET." },
  { "en": "Singkatan MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor)?", "id": "Metal-Oxide-Semiconductor Field-Effect Transistor." },
  { "en": "Tiga terminal MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor)?", "id": "Source, Gate, Drain." },
  { "en": "Lapisan tipis di bawah gerbang MOSFET?", "id": "Lapisan oksida (isolator)." },
  { "en": "Keuntungan utama MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor)?", "id": "Impedansi input sangat tinggi." },
  { "en": "Apa itu sirkuit terpadu (IC)?", "id": "Sirkuit elektronik dalam satu chip." },
  { "en": "Singkatan IC (Integrated Circuit)?", "id": "Integrated Circuit." },
  { "en": "Bahan dasar IC (Integrated Circuit)?", "id": "Wafer silikon." },
  { "en": "Apa itu wafer silikon?", "id": "Kepingan tipis silikon kristal tunggal." },
  { "en": "Proses fabrikasi IC (Integrated Circuit) utama?", "id": "Fotolitografi." },
  { "en": "Apa itu fotolitografi?", "id": "Proses mentransfer pola ke wafer." },
  { "en": "Cairan peka cahaya dalam fotolitografi?", "id": "Photoresist." },
  { "en": "Apa itu etsa (etching)?", "id": "Proses menghilangkan material secara selektif." },
  { "en": "Apa itu deposisi?", "id": "Proses menumbuhkan lapisan tipis material." },
  { "en": "Apa itu oksidasi?", "id": "Proses menumbuhkan lapisan silikon dioksida." },
  { "en": "Fungsi silikon dioksida (SiO2)?", "id": "Sebagai isolator atau lapisan pelindung." },
  { "en": "Apa itu Hukum Moore?", "id": "Jumlah transistor pada IC berlipat ganda." },
  { "en": "Periode pelipatan ganda Hukum Moore?", "id": "Sekitar setiap dua tahun." },
  { "en": "Apa itu 'cleanroom'?", "id": "Ruangan super bersih untuk fabrikasi IC." },
  { "en": "Satuan ukuran fitur pada IC (Integrated Circuit)?", "id": "Nanometer (nm)." },
  { "en": "Apa itu logika digital?", "id": "Sistem berbasis nilai biner 0 dan 1." },
  { "en": "Gerbang logika dasar?", "id": "AND, OR, NOT." },
  { "en": "Apa itu gerbang NAND?", "id": "Gerbang NOT-AND." },
  { "en": "Apa itu gerbang NOR?", "id": "Gerbang NOT-OR." },
  { "en": "Gerbang universal adalah?", "id": "NAND dan NOR." },
  { "en": "Apa itu CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Teknologi dominan untuk IC digital." },
  { "en": "Singkatan CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Complementary Metal-Oxide-Semiconductor." },
  { "en": "CMOS (Complementary Metal-Oxide-Semiconductor) menggunakan transistor jenis?", "id": "PMOS dan NMOS." },
  { "en": "Singkatan PMOS (P-channel Metal-Oxide-Semiconductor)?", "id": "P-channel Metal-Oxide-Semiconductor." },
  { "en": "Singkatan NMOS (N-channel Metal-Oxide-Semiconductor)?", "id": "N-channel Metal-Oxide-Semiconductor." },
  { "en": "Keuntungan utama logika CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Konsumsi daya statis sangat rendah." },
  { "en": "Apa itu 'yield' dalam fabrikasi?", "id": "Persentase chip yang berfungsi baik." },
  { "en": "Apa itu 'die'?", "id": "Satu unit sirkuit individu di wafer." },
  { "en": "Proses pemotongan wafer menjadi 'die'?", "id": "Dicing." },
  { "en": "Apa itu 'packaging' IC (Integrated Circuit)?", "id": "Proses membungkus die dalam kemasan." },
  { "en": "Fungsi 'packaging' IC (Integrated Circuit)?", "id": "Melindungi die dan menyediakan koneksi." },
  { "en": "Apa itu 'wire bonding'?", "id": "Menghubungkan die ke pin kemasan." },
  { "en": "Apa itu 'band gap' energi?", "id": "Energi minimum eksitasi elektron." },
  { "en": "Satuan 'band gap' energi?", "id": "Electronvolt (eV)." },
  { "en": "Semikonduktor memiliki 'band gap'?", "id": "Kecil." },
  { "en": "Isolator memiliki 'band gap'?", "id": "Besar." },
  { "en": "Konduktor memiliki 'band gap'?", "id": "Nol atau tumpang tindih." },
  { "en": "Apa itu rekombinasi?", "id": "Elektron bebas mengisi sebuah hole." },
  { "en": "Apa itu 'carrier lifetime'?", "id": "Waktu rata-rata sebelum rekombinasi." },
  { "en": "Apa itu arus drift?", "id": "Gerakan muatan akibat medan listrik." },
  { "en": "Apa itu arus difusi?", "id": "Gerakan muatan akibat gradien konsentrasi." },
  { "en": "Apa itu 'mobility' (mobilitas) elektron?", "id": "Ukuran kemudahan elektron bergerak." },
  { "en": "Satuan 'mobility'?", "id": "cm²/Vs." },
  { "en": "Mobilitas elektron lebih tinggi dari?", "id": "Mobilitas hole." },
  { "en": "Apa itu 'avalanche breakdown'?", "id": "Kerusakan dioda akibat tumbukan berantai." },
  { "en": "Apa itu 'Zener breakdown'?", "id": "Kerusakan dioda akibat medan listrik kuat." },
  { "en": "Dioda Zener digunakan untuk?", "id": "Regulator tegangan." },
  { "en": "Apa itu dioda Schottky?", "id": "Dioda sambungan logam-semikonduktor." },
  { "en": "Keuntungan dioda Schottky?", "id": "Kecepatan switching sangat tinggi." },
  { "en": "Apa itu LED (Light Emitting Diode)?", "id": "Dioda yang memancarkan cahaya saat aktif." },
  { "en": "Singkatan LED (Light Emitting Diode)?", "id": "Light Emitting Diode." },
  { "en": "Warna LED (Light Emitting Diode) ditentukan oleh?", "id": "Material semikonduktor dan band gap." },
  { "en": "Apa itu fotodioda?", "id": "Dioda yang mendeteksi cahaya." },
  { "en": "Fotodioda dioperasikan pada kondisi?", "id": "Bias mundur (reverse bias)." },
  { "en": "Apa itu sel surya?", "id": "Dioda P-N besar pengubah cahaya." },
  { "en": "Sel surya mengubah energi cahaya menjadi?", "id": "Energi listrik." },
  { "en": "Efek yang mendasari sel surya?", "id": "Efek fotovoltaik." },
  { "en": "Apa itu kapasitor MOS (Metal-Oxide-Semiconductor)?", "id": "Struktur dasar dari MOSFET." },
  { "en": "Singkatan MOS (Metal-Oxide-Semiconductor)?", "id": "Metal-Oxide-Semiconductor." },
  { "en": "Tiga daerah operasi kapasitor MOS?", "id": "Akumulasi, deplesi, inversi." },
  { "en": "Daerah inversi pada kapasitor MOS?", "id": "Membentuk kanal konduktif." },
  { "en": "Apa itu tegangan threshold?", "id": "Tegangan minimum untuk membentuk kanal." },
  { "en": "Notasi tegangan threshold?", "id": "V_T atau V_th." },
  { "en": "MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) tipe enhancement?", "id": "Normalnya mati (off) tanpa tegangan gerbang." },
  { "en": "MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) tipe depletion?", "id": "Normalnya hidup (on) tanpa tegangan gerbang." },
  { "en": "Daerah operasi MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor)?", "id": "Cut-off, triode, saturasi." },
  { "en": "Kapan MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) beroperasi sebagai saklar?", "id": "Di daerah cut-off dan triode." },
  { "en": "Kapan MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) beroperasi sebagai penguat?", "id": "Di daerah saturasi." },
  { "en": "Daerah triode disebut juga?", "id": "Daerah linear atau oh mik." },
  { "en": "Apa itu 'channel length modulation'?", "id": "Efek orde kedua pada MOSFET." },
  { "en": "Apa itu 'body effect'?", "id": "Perubahan V_T akibat tegangan substrat." },
  { "en": "Apa itu 'scaling' transistor?", "id": "Proses mengecilkan dimensi transistor." },
  { "en": "Tujuan utama 'scaling'?", "id": "Meningkatkan kecepatan dan kepadatan chip." },
  { "en": "Apa itu 'short-channel effects'?", "id": "Efek negatif akibat 'scaling' berlebihan." },
  { "en": "Contoh 'short-channel effect'?", "id": "DIBL dan hot carrier." },
  { "en": "Singkatan DIBL (Drain-Induced Barrier Lowering)?", "id": "Drain-Induced Barrier Lowering." },
  { "en": "Apa itu 'hot carrier effect'?", "id": "Elektron berenergi tinggi merusak oksida." },
  { "en": "Apa itu 'latch-up' pada CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Korsleting internal parasitik." },
  { "en": "Struktur parasitik penyebab 'latch-up'?", "id": "Struktur SCR (Silicon-Controlled Rectifier) parasitik." },
  { "en": "Singkatan SCR (Silicon-Controlled Rectifier)?", "id": "Silicon-Controlled Rectifier." },
  { "en": "Apa itu model 'small-signal'?", "id": "Model linear untuk analisis AC." },
  { "en": "Parameter utama model 'small-signal' MOSFET?", "id": "Transkonduktansi (gm) dan resistansi output (ro)." },
  { "en": "Apa itu transkonduktansi (gm)?", "id": "Ukuran penguatan transistor." },
  { "en": "Tiga konfigurasi dasar penguat BJT?", "id": "Common-emitter, common-base, common-collector." },
  { "en": "Tiga konfigurasi dasar penguat FET?", "id": "Common-source, common-gate, common-drain." },
  { "en": "Penguat common-emitter memiliki penguatan tegangan?", "id": "Tinggi dan terbalik (180 derajat)." },
  { "en": "Penguat common-collector disebut juga?", "id": "Emitter follower." },
  { "en": "Penguatan tegangan emitter follower?", "id": "Sekitar satu (unity gain)." },
  { "en": "Penguat common-source memiliki penguatan tegangan?", "id": "Tinggi dan terbalik." },
  { "en": "Penguat common-drain disebut juga?", "id": "Source follower." },
  { "en": "Apa itu 'current mirror' (cermin arus)?", "id": "Sirkuit untuk menyalin arus." },
  { "en": "Apa itu penguat diferensial?", "id": "Menguatkan selisih antara dua input." },
  { "en": "Fungsi utama penguat diferensial?", "id": "Menolak noise common-mode." },
  { "en": "Apa itu 'common-mode signal'?", "id": "Sinyal yang sama pada kedua input." },
  { "en": "Apa itu 'differential-mode signal'?", "id": "Selisih sinyal antara dua input." },
  { "en": "Ukuran penolakan 'common-mode'?", "id": "CMRR (Common-Mode Rejection Ratio)." },
  { "en": "Singkatan CMRR (Common-Mode Rejection Ratio)?", "id": "Common-Mode Rejection Ratio." },
  { "en": "Nilai CMRR (Common-Mode Rejection Ratio) yang ideal?", "id": "Tak terhingga." },
  { "en": "Apa itu 'operational amplifier' (op-amp)?", "id": "Penguat diferensial gain sangat tinggi." },
  { "en": "Singkatan Op-Amp (Operational Amplifier)?", "id": "Operational Amplifier." },
  { "en": "Op-amp ideal memiliki gain?", "id": "Tak terhingga." },
  { "en": "Op-amp ideal memiliki impedansi input?", "id": "Tak terhingga." },
  { "en": "Op-amp ideal memiliki impedansi output?", "id": "Nol." },
  { "en": "Op-amp ideal memiliki bandwidth?", "id": "Tak terhingga." },
  { "en": "Konfigurasi dasar op-amp?", "id": "Inverting, non-inverting, buffer." },
  { "en": "Apa itu 'virtual ground'?", "id": "Titik dengan tegangan nol tapi tak terhubung." },
  { "en": "Apa itu 'slew rate'?", "id": "Laju perubahan output maksimum op-amp." },
  { "en": "Apa itu 'gain-bandwidth product' (GBP)?", "id": "Hasil kali gain dan bandwidth." },
  { "en": "Singkatan GBP (Gain-Bandwidth Product)?", "id": "Gain-Bandwidth Product." },
  { "en": "Apa itu memori?", "id": "Sirkuit untuk menyimpan data digital." },
  { "en": "Jenis memori semikonduktor utama?", "id": "RAM dan ROM." },
  { "en": "Singkatan RAM (Random-Access Memory)?", "id": "Random-Access Memory." },
  { "en": "Singkatan ROM (Read-Only Memory)?", "id": "Read-Only Memory." },
  { "en": "Sifat memori RAM (Random-Access Memory)?", "id": "Volatile (data hilang tanpa daya)." },
  { "en": "Sifat memori ROM (Read-Only Memory)?", "id": "Non-volatile (data tetap tanpa daya)." },
  { "en": "Dua jenis RAM (Random-Access Memory)?", "id": "SRAM dan DRAM." },
  { "en": "Singkatan SRAM (Static Random-Access Memory)?", "id": "Static Random-Access Memory." },
  { "en": "Singkatan DRAM (Dynamic Random-Access Memory)?", "id": "Dynamic Random-Access Memory." },
  { "en": "Sel memori SRAM (Static Random-Access Memory) dibuat dari?", "id": "Flip-flop (beberapa transistor)." },
  { "en": "Sel memori DRAM (Dynamic Random-Access Memory) dibuat dari?", "id": "Satu transistor dan satu kapasitor." },
  { "en": "Mana yang lebih cepat, SRAM atau DRAM?", "id": "SRAM." },
  { "en": "Mana yang lebih padat, SRAM atau DRAM?", "id": "DRAM." },
  { "en": "Mengapa DRAM (Dynamic Random-Access Memory) disebut dinamis?", "id": "Harus di-refresh secara periodik." },
  { "en": "Contoh penggunaan SRAM (Static Random-Access Memory)?", "id": "Memori cache prosesor." },
  { "en": "Contoh penggunaan DRAM (Dynamic Random-Access Memory)?", "id": "Memori utama komputer." },
  { "en": "Apa itu memori Flash?", "id": "Jenis memori non-volatile populer." },
  { "en": "Contoh memori Flash?", "id": "USB drive, SSD." },
  { "en": "Singkatan SSD (Solid-State Drive)?", "id": "Solid-State Drive." },
  { "en": "Jenis memori ROM (Read-Only Memory) yang bisa diprogram?", "id": "PROM, EPROM, EEPROM." },
  { "en": "Singkatan PROM (Programmable Read-Only Memory)?", "id": "Programmable Read-Only Memory." },
  { "en": "Singkatan EPROM (Erasable Programmable Read-Only Memory)?", "id": "Erasable Programmable Read-Only Memory." },
  { "en": "EPROM (Erasable Programmable Read-Only Memory) dihapus dengan?", "id": "Sinar ultraviolet." },
  { "en": "Singkatan EEPROM (Electrically Erasable Programmable Read-Only Memory)?", "id": "Electrically Erasable Programmable Read-Only Memory." },
  { "en": "EEPROM (Electrically Erasable Programmable Read-Only Memory) dihapus dengan?", "id": "Sinyal listrik." },
  { "en": "Memori flash adalah jenis dari?", "id": "EEPROM." },
  { "en": "Apa itu 'power dissipation' (disipasi daya)?", "id": "Energi yang diubah menjadi panas." },
  { "en": "Jenis disipasi daya pada CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Statis dan dinamis." },
  { "en": "Disipasi daya dinamis disebabkan oleh?", "id": "Proses switching (pengisian kapasitor)." },
  { "en": "Apa itu 'noise margin'?", "id": "Ukuran ketahanan gerbang logika terhadap noise." },
  { "en": "Apa itu 'fan-out'?", "id": "Jumlah input yang bisa dikemudikan output." },
  { "en": "Apa itu 'propagation delay'?", "id": "Waktu tunda sinyal melewati gerbang." },
  { "en": "Apa itu 'logic family'?", "id": "Kelompok gerbang logika dengan teknologi sama." },
  { "en": "Contoh 'logic family'?", "id": "TTL, CMOS, ECL." },
  { "en": "Singkatan TTL (Transistor-Transistor Logic)?", "id": "Transistor-Transistor Logic." },
  { "en": "Singkatan ECL (Emitter-Coupled Logic)?", "id": "Emitter-Coupled Logic." },
  { "en": "Karakteristik ECL (Emitter-Coupled Logic)?", "id": "Sangat cepat tetapi boros daya." },
  { "en": "Apa itu 'bandgap reference'?", "id": "Sirkuit penghasil tegangan referensi stabil." },
  { "en": "Stabilitas 'bandgap reference' terhadap?", "id": "Perubahan suhu dan catu daya." },
  { "en": "Apa itu 'active load'?", "id": "Beban yang menggunakan komponen aktif." },
  { "en": "Contoh 'active load'?", "id": "Cermin arus (current mirror)." },
  { "en": "Keuntungan menggunakan 'active load'?", "id": "Mendapatkan penguatan tegangan sangat tinggi." },
  { "en": "Apa itu penguat 'cascode'?", "id": "Kombinasi common-emitter dan common-base." },
  { "en": "Keuntungan penguat 'cascode'?", "id": "Bandwidth lebih lebar, gain tinggi." },
  { "en": "Apa itu penguat 'Darlington pair'?", "id": "Kombinasi dua BJT untuk gain arus." },
  { "en": "Penguatan arus 'Darlington pair'?", "id": "Sangat tinggi." },
  { "en": "Apa itu osilator?", "id": "Sirkuit yang menghasilkan gelombang periodik." },
  { "en": "Syarat terjadinya osilasi?", "id": "Kriteria Barkhausen." },
  { "en": "Apa isi kriteria Barkhausen?", "id": "Loop gain satu, pergeseran fasa 360°." },
  { "en": "Jenis osilator berdasarkan komponen?", "id": "Osilator RC, LC, kristal." },
  { "en": "Contoh osilator RC?", "id": "Phase-shift, Wien bridge." },
  { "en": "Contoh osilator LC?", "id": "Colpitts, Hartley, Clapp." },
  { "en": "Osilator mana yang paling stabil?", "id": "Osilator kristal." },
  { "en": "Apa itu kristal kuarsa?", "id": "Material dengan efek piezoelektrik." },
  { "en": "Apa itu efek piezoelektrik?", "id": "Menghasilkan tegangan saat ditekan." },
  { "en": "Apa itu 'phase-locked loop' (PLL)?", "id": "Sistem umpan balik untuk sinkronisasi fasa." },
  { "en": "Singkatan PLL (Phase-Locked Loop)?", "id": "Phase-Locked Loop." },
  { "en": "Komponen utama PLL (Phase-Locked Loop)?", "id": "Phase detector, LPF, VCO." },
  { "en": "Singkatan LPF (Low-Pass Filter)?", "id": "Low-Pass Filter." },
  { "en": "Singkatan VCO (Voltage-Controlled Oscillator)?", "id": "Voltage-Controlled Oscillator." },
  { "en": "Fungsi VCO (Voltage-Controlled Oscillator)?", "id": "Osilator yang frekuensinya dikontrol tegangan." },
  { "en": "Aplikasi PLL (Phase-Locked Loop)?", "id": "Sintesis frekuensi, demodulasi FM." },
  { "en": "Apa itu 'mixer' dalam elektronika?", "id": "Sirkuit non-linear pengali sinyal." },
  { "en": "Fungsi utama 'mixer'?", "id": "Mengubah frekuensi sinyal (heterodyning)." },
  { "en": "Output dari 'mixer'?", "id": "Jumlahan dan selisih frekuensi input." },
  { "en": "Apa itu 'intermodulation distortion'?", "id": "Produk frekuensi yang tidak diinginkan." },
  { "en": "Apa itu 'semiconductor fabrication plant' (fab)?", "id": "Pabrik pembuat chip semikonduktor." },
  { "en": "Singkatan Fab (Fabrication Plant)?", "id": "Fabrication Plant." },
  { "en": "Apa itu 'foundry'?", "id": "Fab yang memproduksi chip untuk perusahaan lain." },
  { "en": "Apa itu 'IDM (Integrated Device Manufacturer)'?", "id": "Perusahaan yang mendesain dan memproduksi chip." },
  { "en": "Singkatan IDM (Integrated Device Manufacturer)?", "id": "Integrated Device Manufacturer." },
  { "en": "Apa itu perusahaan 'fabless'?", "id": "Perusahaan yang hanya mendesain chip." },
  { "en": "Proses fisika di balik deposisi?", "id": "CVD atau PVD." },
  { "en": "Singkatan CVD (Chemical Vapor Deposition)?", "id": "Chemical Vapor Deposition." },
  { "en": "Singkatan PVD (Physical Vapor Deposition)?", "id": "Physical Vapor Deposition." },
  { "en": "Contoh PVD (Physical Vapor Deposition)?", "id": "Sputtering, evaporasi." },
  { "en": "Apa itu 'ion implantation'?", "id": "Teknik doping dengan menembakkan ion." },
  { "en": "Keuntungan 'ion implantation'?", "id": "Dosis dan kedalaman lebih terkontrol." },
  { "en": "Apa itu 'annealing'?", "id": "Proses pemanasan untuk memperbaiki kristal." },
  { "en": "Apa itu 'chemical-mechanical polishing' (CMP)?", "id": "Proses untuk meratakan permukaan wafer." },
  { "en": "Singkatan CMP (Chemical-Mechanical Polishing)?", "id": "Chemical-Mechanical Polishing." },
  { "en": "Apa itu 'design rules'?", "id": "Aturan geometri minimum dalam desain IC." },
  { "en": "Apa itu 'layout' IC (Integrated Circuit)?", "id": "Representasi geometris dari sirkuit." },
  { "en": "Apa itu 'tape-out'?", "id": "Proses finalisasi desain layout untuk fabrikasi." },
  { "en": "Apa itu 'photomask'?", "id": "Pola sirkuit yang digunakan dalam litografi." },
  { "en": "Apa itu 'stepper' atau 'scanner'?", "id": "Mesin untuk proses fotolitografi." },
  { "en": "Apa itu 'ASIC (Application-Specific Integrated Circuit)'?", "id": "IC yang dirancang untuk tugas spesifik." },
  { "en": "Singkatan ASIC (Application-Specific Integrated Circuit)?", "id": "Application-Specific Integrated Circuit." },
  { "en": "Apa itu 'FPGA (Field-Programmable Gate Array)'?", "id": "IC yang logikanya bisa diprogram." },
  { "en": "Singkatan FPGA (Field-Programmable Gate Array)?", "id": "Field-Programmable Gate Array." },
  { "en": "Keuntungan FPGA (Field-Programmable Gate Array) dibanding ASIC (Application-Specific Integrated Circuit)?", "id": "Fleksibel dan waktu pengembangan cepat." },
  { "en": "Bahasa untuk memprogram FPGA (Field-Programmable Gate Array)?", "id": "VHDL atau Verilog." },
  { "en": "Singkatan VHDL (VHSIC Hardware Description Language)?", "id": "VHSIC Hardware Description Language." },
  { "en": "Apa itu 'system on a chip' (SoC)?", "id": "Sistem lengkap dalam satu chip." },
  { "en": "Singkatan SoC (System on a Chip)?", "id": "System on a Chip." },
  { "en": "Contoh SoC (System on a Chip)?", "id": "Prosesor smartphone." },
  { "en": "Apa itu 'microcontroller' (MCU)?", "id": "Komputer kecil dalam satu chip." },
  { "en": "Singkatan MCU (Microcontroller Unit)?", "id": "Microcontroller Unit." },
  { "en": "Komponen utama MCU (Microcontroller Unit)?", "id": "CPU, RAM, ROM, I/O." },
  { "en": "Singkatan CPU (Central Processing Unit)?", "id": "Central Processing Unit." },
  { "en": "Singkatan I/O (Input/Output)?", "id": "Input/Output." },
  { "en": "Apa itu 'ESD (Electrostatic Discharge)'?", "id": "Pelepasan muatan listrik statis tiba-tiba." },
  { "en": "Singkatan ESD (Electrostatic Discharge)?", "id": "Electrostatic Discharge." },
  { "en": "Bahaya ESD (Electrostatic Discharge) bagi komponen?", "id": "Dapat merusak komponen secara permanen." },
  { "en": "Cara proteksi dari ESD (Electrostatic Discharge)?", "id": "Menggunakan gelang anti-statis." },
  { "en": "Apa itu 'thermal runaway'?", "id": "Kondisi di mana suhu terus meningkat." },
  { "en": "Komponen yang rentan 'thermal runaway'?", "id": "Transistor BJT." },
  { "en": "Apa itu 'heat sink' (pendingin)?", "id": "Komponen untuk membuang panas." },
  { "en": "Apa itu 'doping compensation'?", "id": "Menetralkan efek dopan dengan dopan lain." },
  { "en": "Apa itu 'degenerate semiconductor'?", "id": "Semikonduktor dengan level doping sangat tinggi." },
  { "en": "Sifat 'degenerate semiconductor'?", "id": "Berperilaku seperti logam." },
  { "en": "Apa itu 'Fermi level'?", "id": "Level energi dengan probabilitas okupansi 50%." },
  { "en": "Posisi 'Fermi level' di semikonduktor intrinsik?", "id": "Di tengah-tengah band gap." },
  { "en": "Posisi 'Fermi level' di tipe-n?", "id": "Dekat pita konduksi." },
  { "en": "Posisi 'Fermi level' di tipe-p?", "id": "Dekat pita valensi." },
  { "en": "Apa itu pita konduksi?", "id": "Pita energi tempat elektron bisa bergerak bebas." },
  { "en": "Apa itu pita valensi?", "id": "Pita energi tempat elektron terikat." },
  { "en": "Apa itu ' FinFET'?", "id": "Jenis arsitektur transistor non-planar." },
  { "en": "Tujuan arsitektur 'FinFET'?", "id": "Mengurangi efek kanal pendek." },
  { "en": "Apa itu 'gallium arsenide' (GaAs)?", "id": "Material semikonduktor selain silikon." },
  { "en": "Keuntungan GaAs (Gallium Arsenide) dibanding Si (Silikon)?", "id": "Mobilitas elektron lebih tinggi." },
  { "en": "Aplikasi GaAs (Gallium Arsenide)?", "id": "Sirkuit frekuensi radio (RF)." },
  { "en": "Singkatan RF (Radio Frequency)?", "id": "Radio Frequency." },
  { "en": "Apa itu 'silicon carbide' (SiC)?", "id": "Semikonduktor untuk aplikasi daya tinggi." },
  { "en": "Apa itu 'gallium nitride' (GaN)?", "id": "Semikonduktor lain untuk aplikasi daya." },
  { "en": "Apa itu 'SPICE'?", "id": "Program simulasi sirkuit elektronik." },
  { "en": "Singkatan SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Simulation Program with Integrated Circuit Emphasis." },
  { "en": "Apa itu 'semiconductor device modeling'?", "id": "Membuat model matematis perilaku perangkat." },
  { "en": "Apa itu 'noise figure' (NF)?", "id": "Ukuran degradasi SNR oleh komponen." },
  { "en": "Singkatan NF (Noise Figure)?", "id": "Noise Figure." },
  { "en": "NF (Noise Figure) yang ideal bernilai?", "id": "0 dB." },
  { "en": "Apa itu 'linearity' dalam penguat?", "id": "Kemampuan mereproduksi sinyal tanpa distorsi." },
  { "en": "Ukuran 'linearity' penguat?", "id": "P1dB, IP3." },
  { "en": "Singkatan P1dB (1 dB compression point)?", "id": "1 dB compression point." },
  { "en": "Singkatan IP3 (Third-order intercept point)?", "id": "Third-order intercept point." },
  { "en": "Apa itu 'amplifier class A'?", "id": "Penguat yang aktif sepanjang siklus." },
  { "en": "Efisiensi 'amplifier class A'?", "id": "Sangat rendah (maksimal 25-50%)." },
  { "en": "Linearitas 'amplifier class A'?", "id": "Paling tinggi." },
  { "en": "Apa itu 'amplifier class B'?", "id": "Penguat yang aktif setengah siklus." },
  { "en": "Efisiensi 'amplifier class B'?", "id": "Lebih tinggi (maksimal 78.5%)." },
  { "en": "Masalah utama 'amplifier class B'?", "id": "Crossover distortion." },
  { "en": "Apa itu 'crossover distortion'?", "id": "Distorsi saat sinyal melintasi nol." },
  { "en": "Apa itu 'amplifier class AB'?", "id": "Kombinasi Class A dan Class B." },
  { "en": "Tujuan 'amplifier class AB'?", "id": "Mengurangi crossover distortion." },
  { "en": "Apa itu 'amplifier class C'?", "id": "Penguat yang aktif kurang dari setengah siklus." },
  { "en": "Aplikasi 'amplifier class C'?", "id": "Penguat frekuensi radio (RF)." },
  { "en": "Apa itu 'amplifier class D'?", "id": "Penguat switching (non-linear)." },
  { "en": "Efisiensi 'amplifier class D'?", "id": "Sangat tinggi (bisa di atas 90%)." },
  { "en": "Prinsip kerja 'amplifier class D'?", "id": "Menggunakan pulse-width modulation (PWM)." },
  { "en": "Singkatan PWM (Pulse-Width Modulation)?", "id": "Pulse-Width Modulation." },
  { "en": "Apa itu 'differential pair'?", "id": "Sinonim untuk penguat diferensial." },
  { "en": "Apa itu 'Gilbert cell'?", "id": "Sirkuit mixer empat-kuadran." },
  { "en": "Aplikasi 'Gilbert cell'?", "id": "Mixer, modulator, dan pengganda." },
  { "en": "Apa itu 'beta' (β) BJT?", "id": "Penguatan arus DC (hFE)." },
  { "en": "Singkatan hFE (Hybrid parameter Forward current gain, Common Emitter)?", "id": "Hybrid parameter Forward current gain." },
  { "en": "Apa itu 'alpha' (α) BJT?", "id": "Rasio arus kolektor terhadap emitor." },
  { "en": "Nilai 'alpha' (α) BJT?", "id": "Selalu sedikit kurang dari satu." },
  { "en": "Hubungan 'beta' dan 'alpha' BJT?", "id": "Beta = alpha / (1 - alpha)." },
  { "en": "Apa itu 'Early effect'?", "id": "Modulasi lebar basis BJT." },
  { "en": "Penyebab 'Early effect'?", "id": "Perubahan tegangan kolektor-basis." },
  { "en": "Apa itu 'breakdown voltage'?", "id": "Tegangan maksimum yang bisa ditahan." },
  { "en": "Apa itu 'safe operating area' (SOA)?", "id": "Daerah operasi aman untuk transistor." },
  { "en": "Singkatan SOA (Safe Operating Area)?", "id": "Safe Operating Area." },
  { "en": "Apa itu 'Miller effect'?", "id": "Peningkatan kapasitansi input efektif." },
  { "en": "Penyebab 'Miller effect'?", "id": "Kapasitansi antara input dan output." },
  { "en": "Efek 'Miller effect'?", "id": "Membatasi respon frekuensi tinggi." },
  { "en": "Apa itu kapasitor 'parasitic'?", "id": "Kapasitansi yang tidak diinginkan." },
  { "en": "Apa itu 'logic gate'?", "id": "Blok bangunan dasar sirkuit digital." },
  { "en": "Keluaran gerbang AND bernilai 1 jika?", "id": "Semua input bernilai 1." },
  { "en": "Keluaran gerbang OR bernilai 1 jika?", "id": "Salah satu input bernilai 1." },
  { "en": "Keluaran gerbang NOT?", "id": "Kebalikan dari nilai inputnya." },
  { "en": "Keluaran gerbang XOR (Exclusive OR) bernilai 1 jika?", "id": "Inputnya berbeda." },
  { "en": "Singkatan XOR (Exclusive OR)?", "id": "Exclusive OR." },
  { "en": "Apa itu 'Boolean algebra'?", "id": "Aljabar untuk menganalisis sirkuit logika." },
  { "en": "Apa itu 'truth table' (tabel kebenaran)?", "id": "Tabel yang menunjukkan semua output logika." },
  { "en": "Apa itu 'Karnaugh map' (K-map)?", "id": "Metode grafis untuk penyederhanaan logika." },
  { "en": "Singkatan K-map (Karnaugh map)?", "id": "Karnaugh map." },
  { "en": "Apa itu 'multiplexer' (MUX)?", "id": "Sirkuit pemilih data." },
  { "en": "Singkatan MUX (Multiplexer)?", "id": "Multiplexer." },
  { "en": "Apa itu 'demultiplexer' (DEMUX)?", "id": "Sirkuit distributor data." },
  { "en": "Singkatan DEMUX (Demultiplexer)?", "id": "Demultiplexer." },
  { "en": "Apa itu 'encoder'?", "id": "Mengubah data ke bentuk kode." },
  { "en": "Apa itu 'decoder'?", "id": "Mengubah kode kembali ke data asli." },
  { "en": "Apa itu 'flip-flop'?", "id": "Elemen memori biner dasar." },
  { "en": "Jenis 'flip-flop'?", "id": "SR, JK, D, T." },
  { "en": "Apa itu 'latch'?", "id": "Jenis flip-flop tak ter-clock." },
  { "en": "Apa itu 'register'?", "id": "Kumpulan flip-flop untuk menyimpan data." },
  { "en": "Apa itu 'shift register'?", "id": "Register yang bisa menggeser datanya." },
  { "en": "Apa itu 'counter' (pencacah)?", "id": "Sirkuit yang menghitung pulsa." },
  { "en": "Apa itu sirkuit kombinasional?", "id": "Output hanya bergantung pada input saat ini." },
  { "en": "Contoh sirkuit kombinasional?", "id": "Encoder, decoder, MUX." },
  { "en": "Apa itu sirkuit sekuensial?", "id": "Output bergantung pada input dan state." },
  { "en": "Contoh sirkuit sekuensial?", "id": "Flip-flop, counter, register." },
  { "en": "Apa itu 'clock signal' (sinyal detak)?", "id": "Sinyal periodik untuk sinkronisasi." },
  { "en": "Apa itu 'state machine' (mesin keadaan)?", "id": "Model komputasi untuk sirkuit sekuensial." },
  { "en": "Apa itu 'Moore machine'?", "id": "State machine di mana output hanya bergantung state." },
  { "en": "Apa itu 'Mealy machine'?", "id": "State machine di mana output bergantung state dan input." },
  { "en": "Apa itu 'semiconductor memory'?", "id": "Memori yang dibuat dari semikonduktor." },
  { "en": "Apa itu 'memory address'?", "id": "Lokasi unik dari data di memori." },
  { "en": "Apa itu 'memory bus'?", "id": "Jalur komunikasi ke memori." },
  { "en": "Apa itu 'address bus'?", "id": "Bus untuk mengirim alamat memori." },
  { "en": "Apa itu 'data bus'?", "id": "Bus untuk mengirim data." },
  { "en": "Lebar 'data bus' menentukan?", "id": "Jumlah data yang bisa ditransfer." },
  { "en": "Apa itu 'semiconductor device'?", "id": "Komponen yang memanfaatkan sifat semikonduktor." },
  { "en": "Apa itu 'electron-hole pair'?", "id": "Pasangan elektron dan hole yang tercipta." },
  { "en": "Penyebab terciptanya 'electron-hole pair'?", "id": "Energi termal atau foton." },
  { "en": "Apa itu 'intrinsic carrier concentration'?", "id": "Konsentrasi pembawa muatan intrinsik." },
  { "en": "Apa itu 'extrinsic semiconductor'?", "id": "Sinonim untuk semikonduktor tak murni." },
  { "en": "Apa itu 'majority carrier'?", "id": "Sinonim untuk pembawa muatan mayoritas." },
  { "en": "Apa itu 'minority carrier'?", "id": "Sinonim untuk pembawa muatan minoritas." },
  { "en": "Apa itu 'diffusion length'?", "id": "Jarak rata-rata difusi minoritas." },
  { "en": "Apa itu 'resistivity' (resistivitas)?", "id": "Ukuran ketahanan material terhadap arus." },
  { "en": "Apa itu 'conductivity' (konduktivitas)?", "id": "Ukuran kemampuan material menghantarkan arus." },
  { "en": "Hubungan 'resistivity' dan 'conductivity'?", "id": "Saling berbanding terbalik." },
  { "en": "Apa itu 'Ohmic contact'?", "id": "Kontak logam-semikonduktor bersifat linear." },
  { "en": "Apa itu 'rectifying contact'?", "id": "Kontak yang bersifat seperti dioda." },
  { "en": "Dioda Schottky memiliki kontak?", "id": "Rectifying (logam-semikonduktor)." },
  { "en": "Apa itu 'work function'?", "id": "Energi untuk melepas elektron dari permukaan." },
  { "en": "Apa itu 'electron affinity'?", "id": "Energi yang dilepas saat elektron ditambahkan." },
  { "en": "Apa itu 'band diagram'?", "id": "Diagram yang menunjukkan level energi." },
  { "en": "Apa itu 'charge-coupled device' (CCD)?", "id": "Perangkat semikonduktor untuk sensor gambar." },
  { "en": "Singkatan CCD (Charge-Coupled Device)?", "id": "Charge-Coupled Device." },
  { "en": "Aplikasi CCD (Charge-Coupled Device)?", "id": "Kamera digital dan teleskop." },
  { "en": "Perbedaan utama CMOS (Complementary Metal-Oxide-Semiconductor) dan CCD (Charge-Coupled Device) sensor?", "id": "Arsitektur pembacaan piksel." },
  { "en": "Apa itu 'dark current'?", "id": "Arus kecil pada sensor gambar tanpa cahaya." },
  { "en": "Apa itu 'shot noise'?", "id": "Noise akibat sifat diskrit muatan." },
  { "en": "Apa itu 'thermal noise'?", "id": "Noise akibat agitasi termal elektron." },
  { "en": "Nama lain 'thermal noise'?", "id": "Johnson-Nyquist noise." },
  { "en": "Apa itu 'flicker noise' (1/f noise)?", "id": "Noise frekuensi rendah pada perangkat." },
  { "en": "Apa itu 'crosstalk'?", "id": "Interferensi antara sinyal yang berdekatan." },
  { "en": "Apa itu 'ground bounce'?", "id": "Fluktuasi tegangan ground pada IC." },
  { "en": "Apa itu 'decoupling capacitor'?", "id": "Kapasitor untuk menstabilkan catu daya." },
  { "en": "Fungsi 'decoupling capacitor'?", "id": "Menyediakan arus transien dan memfilter noise." },
  { "en": "Apa itu ADC (Analog-to-Digital Converter)?", "id": "Sirkuit pengubah sinyal analog ke digital." },
  { "en": "Singkatan ADC (Analog-to-Digital Converter)?", "id": "Analog-to-Digital Converter." },
  { "en": "Dua proses utama dalam ADC (Analog-to-Digital Converter)?", "id": "Sampling dan kuantisasi." },
  { "en": "Apa itu resolusi ADC (Analog-to-Digital Converter)?", "id": "Jumlah bit pada output digitalnya." },
  { "en": "Resolusi lebih tinggi berarti?", "id": "Hasil konversi lebih akurat." },
  { "en": "Apa itu laju sampling (sample rate)?", "id": "Seberapa sering sinyal diukur per detik." },
  { "en": "Apa itu 'quantization error'?", "id": "Error akibat pembulatan nilai amplitudo." },
  { "en": "Jenis ADC (Analog-to-Digital Converter) tercepat?", "id": "Flash ADC." },
  { "en": "Kelemahan Flash ADC (Analog-to-Digital Converter)?", "id": "Membutuhkan banyak komponen (komparator)." },
  { "en": "Jenis ADC (Analog-to-Digital Converter) populer di MCU?", "id": "Successive-Approximation (SAR) ADC." },
  { "en": "Singkatan SAR (Successive-Approximation Register)?", "id": "Successive-Approximation Register." },
  { "en": "Jenis ADC (Analog-to-Digital Converter) resolusi tertinggi?", "id": "Delta-Sigma ADC." },
  { "en": "Apa itu DAC (Digital-to-Analog Converter)?", "id": "Sirkuit pengubah sinyal digital ke analog." },
  { "en": "Singkatan DAC (Digital-to-Analog Converter)?", "id": "Digital-to-Analog Converter." },
  { "en": "Arsitektur DAC (Digital-to-Analog Converter) yang umum?", "id": "R-2R ladder." },
  { "en": "Apa itu DNL (Differential Nonlinearity)?", "id": "Ukuran error antar level output." },
  { "en": "Singkatan DNL (Differential Nonlinearity)?", "id": "Differential Nonlinearity." },
  { "en": "Apa itu INL (Integral Nonlinearity)?", "id": "Ukuran deviasi dari garis lurus ideal." },
  { "en": "Singkatan INL (Integral Nonlinearity)?", "id": "Integral Nonlinearity." },
  { "en": "Apa itu 'settling time' DAC?", "id": "Waktu output mencapai nilai final." },
  { "en": "Apa itu thyristor?", "id": "Saklar semikonduktor empat lapis (P-N-P-N)." },
  { "en": "Nama lain thyristor?", "id": "Silicon-Controlled Rectifier (SCR)." },
  { "en": "Fungsi utama thyristor?", "id": "Sebagai saklar daya tinggi." },
  { "en": "Apa itu TRIAC (Triode for Alternating Current)?", "id": "Thyristor dua arah untuk arus AC." },
  { "en": "Singkatan TRIAC (Triode for Alternating Current)?", "id": "Triode for Alternating Current." },
  { "en": "Aplikasi TRIAC (Triode for Alternating Current)?", "id": "Pengatur lampu dimmer, kontrol kecepatan motor." },
  { "en": "Apa itu IGBT (Insulated-Gate Bipolar Transistor)?", "id": "Transistor untuk aplikasi switching daya." },
  { "en": "Singkatan IGBT (Insulated-Gate Bipolar Transistor)?", "id": "Insulated-Gate Bipolar Transistor." },
  { "en": "IGBT (Insulated-Gate Bipolar Transistor) menggabungkan kelebihan dari?", "id": "MOSFET dan BJT." },
  { "en": "Apa itu 'power MOSFET'?", "id": "MOSFET yang dirancang untuk daya tinggi." },
  { "en": "Apa itu MEMS (Micro-Electro-Mechanical Systems)?", "id": "Sistem mekanik dan elektronik miniatur." },
  { "en": "Singkatan MEMS (Micro-Electro-Mechanical Systems)?", "id": "Micro-Electro-Mechanical Systems." },
  { "en": "Contoh perangkat MEMS (Micro-Electro-Mechanical Systems)?", "id": "Akselerometer, giroskop, mikrofon." },
  { "en": "Apa fungsi akselerometer?", "id": "Mengukur percepatan linear dan gravitasi." },
  { "en": "Apa fungsi giroskop?", "id": "Mengukur orientasi dan kecepatan angular." },
  { "en": "Apa itu sensor tekanan (pressure sensor)?", "id": "Mengukur tekanan fluida (cair/gas)." },
  { "en": "Apa itu 'actuator'?", "id": "Perangkat yang menghasilkan gerakan." },
  { "en": "Contoh 'actuator' MEMS (Micro-Electro-Mechanical Systems)?", "id": "Micromirror, microgripper." },
  { "en": "Apa itu impedansi matching?", "id": "Menyesuaikan impedansi untuk transfer daya maks." },
  { "en": "Alat bantu grafis impedansi matching?", "id": "Smith Chart." },
  { "en": "Apa itu S-parameters (scattering parameters)?", "id": "Parameter untuk karakterisasi jaringan RF." },
  { "en": "S11 pada S-parameters merepresentasikan?", "id": "Koefisien pantulan port input." },
  { "en": "S21 pada S-parameters merepresentasikan?", "id": "Penguatan (gain) maju." },
  { "en": "Apa itu LNA (Low-Noise Amplifier)?", "id": "Penguat sinyal lemah di bagian depan." },
  { "en": "Singkatan LNA (Low-Noise Amplifier)?", "id": "Low-Noise Amplifier." },
  { "en": "Karakteristik utama LNA (Low-Noise Amplifier)?", "id": "Noise figure sangat rendah." },
  { "en": "Apa itu PA (Power Amplifier)?", "id": "Penguat sinyal di bagian akhir." },
  { "en": "Singkatan PA (Power Amplifier)?", "id": "Power Amplifier." },
  { "en": "Karakteristik utama PA (Power Amplifier)?", "id": "Daya output tinggi dan efisien." },
  { "en": "Apa itu 'gain compression'?", "id": "Penurunan gain saat sinyal input besar." },
  { "en": "Apa itu 'bit line'?", "id": "Jalur untuk membaca/menulis data sel." },
  { "en": "Apa itu 'word line'?", "id": "Jalur untuk memilih baris sel memori." },
  { "en": "Apa itu 'memory array'?", "id": "Grid dua dimensi dari sel memori." },
  { "en": "Apa itu 'sense amplifier'?", "id": "Sirkuit untuk membaca sinyal kecil sel." },
  { "en": "Aplikasi 'sense amplifier'?", "id": "Pada memori DRAM dan SRAM." },
  { "en": "Apa itu 'memory controller'?", "id": "Mengelola aliran data ke/dari memori." },
  { "en": "Apa itu 'wear leveling'?", "id": "Teknik memperpanjang umur memori Flash." },
  { "en": "Tujuan 'wear leveling'?", "id": "Mendistribusikan siklus tulis secara merata." },
  { "en": "Apa itu 'error correction code' (ECC)?", "id": "Kode untuk mendeteksi dan memperbaiki error." },
  { "en": "Singkatan ECC (Error Correction Code)?", "id": "Error Correction Code." },
  { "en": "Memori ECC (Error Correction Code) digunakan di?", "id": "Server dan sistem kritis." },
  { "en": "Apa itu 'single-event upset' (SEU)?", "id": "Perubahan state bit akibat radiasi." },
  { "en": "Singkatan SEU (Single-Event Upset)?", "id": "Single-Event Upset." },
  { "en": "Apa itu 'radiation hardening'?", "id": "Membuat chip tahan terhadap radiasi." },
  { "en": "Aplikasi chip 'radiation hardened'?", "id": "Satelit dan perangkat luar angkasa." },
  { "en": "Apa itu 'gate oxide'?", "id": "Lapisan dielektrik tipis di bawah gerbang." },
  { "en": "Apa itu 'gate leakage'?", "id": "Arus bocor melalui gate oxide." },
  { "en": "Masalah 'gate leakage' terjadi pada?", "id": "Transistor dengan gate oxide sangat tipis." },
  { "en": "Solusi untuk 'gate leakage'?", "id": "Menggunakan material dielektrik high-k." },
  { "en": "Apa itu 'high-k dielectric'?", "id": "Material isolator dengan permitivitas tinggi." },
  { "en": "Apa itu 'metal gate'?", "id": "Mengganti gerbang polisilikon dengan logam." },
  { "en": "Teknologi 'high-k/metal gate' bertujuan?", "id": "Mengurangi bocor dan meningkatkan performa." },
  { "en": "Apa itu 'silicon on insulator' (SOI)?", "id": "Teknologi wafer dengan lapisan isolator." },
  { "en": "Singkatan SOI (Silicon on Insulator)?", "id": "Silicon on Insulator." },
  { "en": "Keuntungan teknologi SOI (Silicon on Insulator)?", "id": "Mengurangi kapasitansi parasitik, lebih cepat." },
  { "en": "Apa itu 'interconnect'?", "id": "Jalur logam yang menghubungkan transistor." },
  { "en": "Material 'interconnect' yang umum?", "id": "Aluminium atau tembaga." },
  { "en": "Apa itu 'via'?", "id": "Koneksi vertikal antara lapisan logam." },
  { "en": "Apa itu 'electromigration'?", "id": "Kerusakan interconnect akibat aliran elektron." },
  { "en": "Apa itu 'clock distribution network'?", "id": "Jaringan untuk mendistribusikan sinyal clock." },
  { "en": "Tantangan 'clock distribution network'?", "id": "Skew dan jitter." },
  { "en": "Apa itu 'clock skew'?", "id": "Perbedaan waktu tiba clock di tempat berbeda." },
  { "en": "Apa itu 'clock jitter'?", "id": "Variasi waktu pada tepi sinyal clock." },
  { "en": "Apa itu 'power delivery network' (PDN)?", "id": "Jaringan untuk mendistribusikan daya." },
  { "en": "Singkatan PDN (Power Delivery Network)?", "id": "Power Delivery Network." },
  { "en": "Masalah pada PDN (Power Delivery Network)?", "id": "IR drop." },
  { "en": "Apa itu 'IR drop'?", "id": "Penurunan tegangan akibat resistansi jalur." },
  { "en": "Apa itu 'pass transistor logic'?", "id": "Gaya desain logika menggunakan pass transistor." },
  { "en": "Apa itu 'transmission gate'?", "id": "Saklar elektronik dua arah." },
  { "en": "Komponen 'transmission gate'?", "id": "Satu PMOS dan satu NMOS paralel." },
  { "en": "Apa itu 'bus contention'?", "id": "Konflik saat beberapa output mencoba mengemudi bus." },
  { "en": "Solusi 'bus contention'?", "id": "Menggunakan output tri-state." },
  { "en": "Apa itu 'tri-state buffer'?", "id": "Buffer dengan state ketiga: impedansi tinggi." },
  { "en": "State impedansi tinggi disebut?", "id": "Hi-Z atau floating." },
  { "en": "Apa itu 'half adder'?", "id": "Sirkuit yang menjumlahkan dua bit." },
  { "en": "Output dari 'half adder'?", "id": "Sum (S) dan Carry (C)." },
  { "en": "Apa itu 'full adder'?", "id": "Sirkuit yang menjumlahkan tiga bit." },
  { "en": "Input 'full adder'?", "id": "Dua bit data dan satu carry-in." },
  { "en": "Apa itu 'ripple-carry adder'?", "id": "Penjumlah yang carry-nya merambat berurutan." },
  { "en": "Kelemahan 'ripple-carry adder'?", "id": "Lambat untuk jumlah bit besar." },
  { "en": "Apa itu 'look-ahead carry adder'?", "id": "Penjumlah yang lebih cepat." },
  { "en": "Apa itu 'comparator' (pembanding)?", "id": "Sirkuit yang membandingkan dua bilangan biner." },
  { "en": "Apa itu 'Arithmetic Logic Unit' (ALU)?", "id": "Bagian CPU untuk operasi aritmetika/logika." },
  { "en": "Singkatan ALU (Arithmetic Logic Unit)?", "id": "Arithmetic Logic Unit." },
  { "en": "Apa itu 'Programmable Logic Device' (PLD)?", "id": "Chip logika yang dapat dikonfigurasi." },
  { "en": "Singkatan PLD (Programmable Logic Device)?", "id": "Programmable Logic Device." },
  { "en": "Contoh PLD (Programmable Logic Device) sederhana?", "id": "PAL, GAL, CPLD." },
  { "en": "Singkatan PAL (Programmable Array Logic)?", "id": "Programmable Array Logic." },
  { "en": "Singkatan CPLD (Complex Programmable Logic Device)?", "id": "Complex Programmable Logic Device." },
  { "en": "Apa itu 'setup time'?", "id": "Waktu data harus stabil sebelum clock." },
  { "en": "Apa itu 'hold time'?", "id": "Waktu data harus stabil setelah clock." },
  { "en": "Pelanggaran 'setup/hold time' menyebabkan?", "id": "Metastability." },
  { "en": "Apa itu 'metastability'?", "id": "Kondisi output yang tidak menentu." },
  { "en": "Apa itu 'clock domain crossing' (CDC)?", "id": "Sinyal berpindah antar domain clock." },
  { "en": "Singkatan CDC (Clock Domain Crossing)?", "id": "Clock Domain Crossing." },
  { "en": "Apa itu 'Schmitt trigger'?", "id": "Komparator dengan histeresis." },
  { "en": "Fungsi histeresis pada 'Schmitt trigger'?", "id": "Mencegah transisi palsu akibat noise." },
  { "en": "Apa itu 'multivibrator'?", "id": "Sirkuit osilator/timer elektronik." },
  { "en": "Apa itu 'astable multivibrator'?", "id": "Osilator yang menghasilkan gelombang kotak." },
  { "en": "Apa itu 'monostable multivibrator'?", "id": "Sirkuit yang menghasilkan satu pulsa." },
  { "en": "Nama lain 'monostable multivibrator'?", "id": "One-shot." },
  { "en": "Apa itu 'bistable multivibrator'?", "id": "Sirkuit dengan dua state stabil." },
  { "en": "Dasar dari 'flip-flop' adalah?", "id": "Bistable multivibrator." },
  { "en": "Apa itu filter aktif?", "id": "Filter yang menggunakan komponen aktif (op-amp)." },
  { "en": "Keuntungan filter aktif?", "id": "Bisa memiliki penguatan (gain)." },
  { "en": "Apa itu filter pasif?", "id": "Filter yang hanya menggunakan R, L, C." },
  { "en": "Topologi filter aktif yang umum?", "id": "Sallen-Key." },
  { "en": "Apa itu 'direct bandgap'?", "id": "Rekombinasi efisien yang memancarkan cahaya." },
  { "en": "Contoh material 'direct bandgap'?", "id": "Gallium Arsenide (GaAs)." },
  { "en": "Apa itu 'indirect bandgap'?", "id": "Rekombinasi tidak efisien, menghasilkan panas." },
  { "en": "Contoh material 'indirect bandgap'?", "id": "Silikon (Si), Germanium (Ge)." },
  { "en": "Material untuk LED (Light Emitting Diode) harus?", "id": "Direct bandgap." },
  { "en": "Apa itu 'effective mass'?", "id": "Massa semu pembawa muatan di kristal." },
  { "en": "Apa itu 'Fermi-Dirac statistics'?", "id": "Statistik distribusi elektron pada level energi." },
  { "en": "Apa itu 'phonon'?", "id": "Kuantisasi dari getaran kisi kristal." },
  { "en": "Apa itu 'exciton'?", "id": "Pasangan elektron-hole yang terikat." },
  { "en": "Apa itu 'quantum well'?", "id": "Struktur semikonduktor yang mengurung partikel." },
  { "en": "Apa itu 'quantum tunneling'?", "id": "Partikel menembus penghalang potensial." },
  { "en": "Fenomena 'quantum tunneling' digunakan pada?", "id": "Tunnel diode, memori Flash." },
  { "en": "Apa itu 'tunnel diode'?", "id": "Dioda yang menunjukkan resistansi negatif." },
  { "en": "Apa itu 'Design for Testability' (DFT)?", "id": "Metodologi desain untuk memudahkan pengetesan." },
  { "en": "Singkatan DFT (Design for Testability)?", "id": "Design for Testability." },
  { "en": "Apa itu 'Automatic Test Equipment' (ATE)?", "id": "Mesin untuk menguji chip secara otomatis." },
  { "en": "Singkatan ATE (Automatic Test Equipment)?", "id": "Automatic Test Equipment." },
  { "en": "Apa itu 'scan chain'?", "id": "Rangkaian register geser untuk testing." },
  { "en": "Tujuan 'scan chain'?", "id": "Mengakses dan mengontrol state internal." },
  { "en": "Apa itu 'Built-In Self-Test' (BIST)?", "id": "Sirkuit yang bisa mengetes dirinya sendiri." },
  { "en": "Singkatan BIST (Built-In Self-Test)?", "id": "Built-In Self-Test." },
  { "en": "Apa itu 'JTAG (Joint Test Action Group)'?", "id": "Standar antarmuka untuk testing board." },
  { "en": "Singkatan JTAG (Joint Test Action Group)?", "id": "Joint Test Action Group." },
  { "en": "Port JTAG (Joint Test Action Group) disebut juga?", "id": "Test Access Port (TAP)." },
  { "en": "Apa itu 'boundary scan'?", "id": "Metodologi testing menggunakan JTAG." },
  { "en": "Apa itu 'signal integrity' (integritas sinyal)?", "id": "Kualitas sinyal listrik." },
  { "en": "Masalah 'signal integrity' yang umum?", "id": "Refleksi, crosstalk, ground bounce." },
  { "en": "Penyebab refleksi sinyal?", "id": "Ketidakcocokan impedansi (impedance mismatch)." },
  { "en": "Apa itu 'transmission line'?", "id": "Kabel yang propertinya harus diperhitungkan." },
  { "en": "Kapan sebuah kabel dianggap 'transmission line'?", "id": "Jika panjangnya signifikan dibanding panjang gelombang." },
  { "en": "Apa itu 'termination'?", "id": "Mengakhiri 'transmission line' dengan impedansi." },
  { "en": "Tujuan 'termination'?", "id": "Mencegah refleksi sinyal." },
  { "en": "Apa itu 'power integrity' (integritas daya)?", "id": "Kualitas dari sistem distribusi daya." },
  { "en": "Apa itu 'Verilog'?", "id": "Bahasa deskripsi hardware (HDL)." },
  { "en": "Singkatan HDL (Hardware Description Language)?", "id": "Hardware Description Language." },
  { "en": "Apa itu 'synthesis' dalam desain digital?", "id": "Proses mengubah HDL menjadi gerbang logika." },
  { "en": "Apa itu 'place and route'?", "id": "Proses menempatkan dan menghubungkan sel logika." },
  { "en": "Apa itu 'timing analysis'?", "id": "Analisis untuk memastikan sirkuit memenuhi timing." },
  { "en": "Apa itu 'critical path'?", "id": "Jalur dengan delay terpanjang di sirkuit." },
  { "en": "Kecepatan clock maksimum ditentukan oleh?", "id": "Delay pada critical path." },
  { "en": "Apa itu 'glitch'?", "id": "Transisi sinyal sesaat yang tidak diinginkan." },
  { "en": "Penyebab 'glitch'?", "id": "Delay yang tidak merata pada logika." },
  { "en": "Apa itu 'race condition'?", "id": "Kondisi output bergantung pada urutan sinyal." },
  { "en": "Apa itu 'intellectual property' (IP) core?", "id": "Blok desain sirkuit yang dapat digunakan kembali." },
  { "en": "Singkatan IP (Intellectual Property)?", "id": "Intellectual Property." },
  { "en": "Jenis IP (Intellectual Property) core?", "id": "Soft IP, Firm IP, Hard IP." },
  { "en": "Apa itu 'soft IP'?", "id": "IP dalam bentuk kode HDL." },
  { "en": "Apa itu 'hard IP'?", "id": "IP dalam bentuk layout fisik." },
  { "en": "Apa itu 'analog-to-digital converter' (ADC)?", "id": "Sinonim untuk ADC." },
  { "en": "Apa itu 'digital-to-analog converter' (DAC)?", "id": "Sinonim untuk DAC." },
  { "en": "Apa itu 'operational transconductance amplifier' (OTA)?", "id": "Penguat yang outputnya berupa arus." },
  { "en": "Singkatan OTA (Operational Transconductance Amplifier)?", "id": "Operational Transconductance Amplifier." },
  { "en": "Apa itu 'current feedback operational amplifier'?", "id": "Jenis op-amp dengan arsitektur berbeda." },
  { "en": "Keuntungan 'current feedback op-amp'?", "id": "Bandwidth yang lebih independen dari gain." },
  { "en": "Apa itu 'instrumentation amplifier'?", "id": "Penguat diferensial presisi tinggi." },
  { "en": "Ciri 'instrumentation amplifier'?", "id": "CMRR tinggi, impedansi input tinggi." },
  { "en": "Apa itu 'isolation amplifier'?", "id": "Penguat yang menyediakan isolasi galvanik." },
  { "en": "Tujuan 'isolation amplifier'?", "id": "Keselamatan dan memutus ground loop." },
  { "en": "Apa itu 'ground loop'?", "id": "Jalur tak diinginkan pada koneksi ground." },
  { "en": "Efek 'ground loop'?", "id": "Menimbulkan noise dan interferensi." },
  { "en": "Apa itu 'ferrite bead'?", "id": "Komponen pasif untuk menekan noise frekuensi tinggi." },
  { "en": "Bahan 'ferrite bead'?", "id": "Material ferit (magnetik)." },
  { "en": "Apa itu 'Register-Transfer Level' (RTL)?", "id": "Abstraksi desain sirkuit digital." },
  { "en": "Singkatan RTL (Register-Transfer Level)?", "id": "Register-Transfer Level." },
  { "en": "Desain RTL (Register-Transfer Level) ditulis menggunakan?", "id": "Bahasa HDL (VHDL/Verilog)." },
  { "en": "Apa itu 'verification' (verifikasi)?", "id": "Proses memeriksa kebenaran desain." },
  { "en": "Metode 'verification' yang umum?", "id": "Simulasi dan 'formal verification'." },
  { "en": "Apa itu 'logic synthesis'?", "id": "Proses mengubah RTL menjadi gerbang logika." },
  { "en": "Output dari 'logic synthesis'?", "id": "Netlist." },
  { "en": "Apa itu 'netlist'?", "id": "Deskripsi koneksi antar gerbang logika." },
  { "en": "Apa itu 'physical design'?", "id": "Proses mengubah netlist menjadi layout fisik." },
  { "en": "Tahap awal 'physical design'?", "id": "Floorplanning." },
  { "en": "Apa itu 'floorplanning'?", "id": "Perencanaan tata letak blok-blok besar." },
  { "en": "Apa itu 'placement'?", "id": "Proses menempatkan sel-sel standar." },
  { "en": "Apa itu 'routing'?", "id": "Proses menggambar jalur kabel (interconnect)." },
  { "en": "Apa itu 'design rule check' (DRC)?", "id": "Memeriksa layout terhadap aturan fabrikasi." },
  { "en": "Singkatan DRC (Design Rule Check)?", "id": "Design Rule Check." },
  { "en": "Apa itu 'layout versus schematic' (LVS)?", "id": "Membandingkan layout dengan skematik asli." },
  { "en": "Singkatan LVS (Layout Versus Schematic)?", "id": "Layout Versus Schematic." },
  { "en": "Apa itu 'process variation'?", "id": "Variasi minor dalam proses fabrikasi." },
  { "en": "Efek 'process variation'?", "id": "Menyebabkan perbedaan performa antar chip." },
  { "en": "Apa itu 'corner analysis'?", "id": "Simulasi pada kondisi proses ekstrem." },
  { "en": "Contoh 'corner' proses?", "id": "Fast, slow, typical." },
  { "en": "Apa itu 'yield modeling'?", "id": "Memprediksi yield dari suatu desain." },
  { "en": "Apa itu 'photolithography'?", "id": "Sinonim untuk fotolitografi." },
  { "en": "Jenis litografi paling canggih?", "id": "EUV (Extreme Ultraviolet) lithography." },
  { "en": "Singkatan EUV (Extreme Ultraviolet)?", "id": "Extreme Ultraviolet." },
  { "en": "Apa itu 'photoresist'?", "id": "Cairan peka cahaya untuk litografi." },
  { "en": "Jenis 'photoresist'?", "id": "Positif dan negatif." },
  { "en": "Apa itu 'reticle'?", "id": "Sinonim untuk photomask." },
  { "en": "Apa itu 'optical proximity correction' (OPC)?", "id": "Teknik memodifikasi mask untuk hasil cetak." },
  { "en": "Singkatan OPC (Optical Proximity Correction)?", "id": "Optical Proximity Correction." },
  { "en": "Apa itu 'MRAM (Magnetoresistive Random-Access Memory)'?", "id": "Memori non-volatile menggunakan spin elektron." },
  { "en": "Singkatan MRAM (Magnetoresistive Random-Access Memory)?", "id": "Magnetoresistive Random-Access Memory." },
  { "en": "Keuntungan MRAM (Magnetoresistive Random-Access Memory)?", "id": "Kecepatan tinggi dan non-volatile." },
  { "en": "Apa itu 'FeRAM (Ferroelectric Random-Access Memory)'?", "id": "RAM non-volatile menggunakan material feroelektrik." },
  { "en": "Singkatan FeRAM (Ferroelectric Random-Access Memory)?", "id": "Ferroelectric Random-Access Memory." },
  { "en": "Apa itu 'phase-change memory' (PCM)?", "id": "Memori non-volatile berbasis perubahan fasa." },
  { "en": "Singkatan PCM (Phase-Change Memory)?", "id": "Phase-Change Memory." },
  { "en": "Apa itu 'content-addressable memory' (CAM)?", "id": "Memori yang mencari berdasarkan konten." },
  { "en": "Singkatan CAM (Content-Addressable Memory)?", "id": "Content-Addressable Memory." },
  { "en": "Aplikasi CAM (Content-Addressable Memory)?", "id": "Tabel routing pada router jaringan." },
  { "en": "Apa itu 'sample-and-hold' (S/H) circuit?", "id": "Sirkuit untuk mengambil dan menahan sampel tegangan." },
  { "en": "Singkatan S/H (Sample-and-Hold)?", "id": "Sample-and-Hold." },
  { "en": "Komponen utama sirkuit S/H (Sample-and-Hold)?", "id": "Saklar dan kapasitor." },
  { "en": "Sirkuit S/H (Sample-and-Hold) adalah bagian penting dari?", "id": "ADC (Analog-to-Digital Converter)." },
  { "en": "Apa itu 'switched-capacitor circuit'?", "id": "Sirkuit yang meniru resistor dengan kapasitor." },
  { "en": "Keuntungan 'switched-capacitor circuit'?", "id": "Mudah diimplementasikan dalam IC." },
  { "en": "Aplikasi 'switched-capacitor circuit'?", "id": "Filter, data converter." },
  { "en": "Apa itu 'charge pump'?", "id": "Sirkuit untuk menaikkan tegangan DC." },
  { "en": "Prinsip kerja 'charge pump'?", "id": "Memompa muatan menggunakan kapasitor." },
  { "en": "Apa itu 'voltage doubler'?", "id": "Jenis charge pump sederhana." },
  { "en": "Apa itu 'Low-Dropout Regulator' (LDO)?", "id": "Regulator tegangan linear yang efisien." },
  { "en": "Singkatan LDO (Low-Dropout Regulator)?", "id": "Low-Dropout Regulator." },
  { "en": "Apa itu 'dropout voltage'?", "id": "Perbedaan tegangan input-output minimum." },
  { "en": "Apa itu 'switching regulator'?", "id": "Regulator tegangan yang menggunakan saklar." },
  { "en": "Keuntungan 'switching regulator'?", "id": "Efisiensi daya sangat tinggi." },
  { "en": "Jenis 'switching regulator'?", "id": "Buck, Boost, Buck-Boost." },
  { "en": "Fungsi 'buck converter'?", "id": "Menurunkan tegangan DC (step-down)." },
  { "en": "Fungsi 'boost converter'?", "id": "Menaikkan tegangan DC (step-up)." },
  { "en": "Apa itu 'velocity saturation'?", "id": "Kecepatan drift elektron mencapai batas maksimum." },
  { "en": "Penyebab 'velocity saturation'?", "id": "Medan listrik lateral yang sangat tinggi." },
  { "en": "Apa itu 'mobility degradation'?", "id": "Penurunan mobilitas akibat medan vertikal." },
  { "en": "Apa itu 'gate-induced drain leakage' (GIDL)?", "id": "Arus bocor di daerah drain." },
  { "en": "Singkatan GIDL (Gate-Induced Drain Leakage)?", "id": "Gate-Induced Drain Leakage." },
  { "en": "Apa itu 'subthreshold conduction'?", "id": "Arus kecil saat V_GS < V_T." },
  { "en": "Apa itu 'subthreshold slope'?", "id": "Ukuran efisiensi switching transistor." },
  { "en": "Apa itu 'hot-electron effect'?", "id": "Sinonim untuk 'hot carrier effect'." },
  { "en": "Apa itu 'electromigration'?", "id": "Pergerakan atom logam akibat aliran elektron." },
  { "en": "Efek 'electromigration'?", "id": "Menyebabkan kegagalan pada jalur interconnect." },
  { "en": "Apa itu 'stress migration'?", "id": "Kegagalan akibat stres mekanis." },
  { "en": "Apa itu 'time-dependent dielectric breakdown' (TDDB)?", "id": "Kegagalan isolator seiring waktu." },
  { "en": "Singkatan TDDB (Time-Dependent Dielectric Breakdown)?", "id": "Time-Dependent Dielectric Breakdown." },
  { "en": "Apa itu 'negative-bias temperature instability' (NBTI)?", "id": "Degradasi pada transistor PMOS." },
  { "en": "Singkatan NBTI (Negative-Bias Temperature Instability)?", "id": "Negative-Bias Temperature Instability." },
  { "en": "Apa itu 'positive-bias temperature instability' (PBTI)?", "id": "Degradasi pada transistor NMOS." },
  { "en": "Singkatan PBTI (Positive-Bias Temperature Instability)?", "id": "Positive-Bias Temperature Instability." },
  { "en": "Apa itu 'semiconductor wafer'?", "id": "Sinonim untuk wafer semikonduktor." },
  { "en": "Bahan wafer paling umum?", "id": "Silikon." },
  { "en": "Apa itu 'Czochralski method'?", "id": "Metode untuk menumbuhkan kristal silikon." },
  { "en": "Hasil dari 'Czochralski method'?", "id": "Ingot silikon kristal tunggal." },
  { "en": "Apa itu 'ingot'?", "id": "Batangan besar material kristal." },
  { "en": "Proses setelah menumbuhkan 'ingot'?", "id": "Mengirisnya menjadi wafer." },
  { "en": "Apa itu 'epitaxy'?", "id": "Proses menumbuhkan lapisan kristal tipis." },
  { "en": "Apa itu 'molecular-beam epitaxy' (MBE)?", "id": "Teknik epitaksi presisi tinggi." },
  { "en": "Singkatan MBE (Molecular-Beam Epitaxy)?", "id": "Molecular-Beam Epitaxy." },
  { "en": "Apa itu 'wafer bonding'?", "id": "Proses menyatukan dua wafer." },
  { "en": "Apa itu '3D IC'?", "id": "IC dengan beberapa lapisan die ditumpuk." },
  { "en": "Koneksi antar die pada '3D IC'?", "id": "Through-Silicon Via (TSV)." },
  { "en": "Singkatan TSV (Through-Silicon Via)?", "id": "Through-Silicon Via." },
  { "en": "Apa itu 'system in package' (SiP)?", "id": "Beberapa die dalam satu kemasan." },
  { "en": "Singkatan SiP (System in Package)?", "id": "System in Package." },
  { "en": "Perbedaan SoC (System on a Chip) dan SiP (System in Package)?", "id": "Monolitik versus integrasi dalam kemasan." },
  { "en": "Apa itu 'flip-chip'?", "id": "Metode pemasangan die terbalik." },
  { "en": "Tonjolan pada 'flip-chip' disebut?", "id": "Solder bumps." },
  { "en": "Apa itu 'underfill'?", "id": "Material untuk mengisi celah flip-chip." },
  { "en": "Apa itu 'printed circuit board' (PCB)?", "id": "Papan untuk merangkai komponen elektronik." },
  { "en": "Singkatan PCB (Printed Circuit Board)?", "id": "Printed Circuit Board." },
  { "en": "Apa itu 'common-centroid layout'?", "id": "Teknik layout untuk matching presisi." },
  { "en": "Tujuan 'common-centroid layout'?", "id": "Mengurangi efek gradien proses." },
  { "en": "Aplikasi 'common-centroid layout'?", "id": "Penguat diferensial dan cermin arus." },
  { "en": "Apa itu 'guard ring'?", "id": "Struktur untuk mengisolasi komponen." },
  { "en": "Fungsi utama 'guard ring'?", "id": "Mencegah 'latch-up' dan 'crosstalk'." },
  { "en": "Apa itu 'interdigitated layout'?", "id": "Layout dengan struktur seperti jari." },
  { "en": "Apa itu 'dummy device'?", "id": "Perangkat tambahan di tepi untuk keseragaman." },
  { "en": "Apa itu laser dioda?", "id": "Dioda semikonduktor yang menghasilkan sinar laser." },
  { "en": "Prinsip kerja laser dioda?", "id": "Emisi terstimulasi (stimulated emission)." },
  { "en": "Apa itu 'population inversion'?", "id": "Kondisi yang diperlukan untuk lasing." },
  { "en": "Apa itu fototransistor?", "id": "Transistor yang dikontrol oleh intensitas cahaya." },
  { "en": "Keuntungan fototransistor dibanding fotodioda?", "id": "Memiliki penguatan (gain) internal." },
  { "en": "Apa itu 'optocoupler'?", "id": "Komponen untuk isolasi sinyal listrik." },
  { "en": "Nama lain 'optocoupler'?", "id": "Opto-isolator." },
  { "en": "Komponen utama 'optocoupler'?", "id": "LED dan fotodetektor." },
  { "en": "Apa itu 'solid-state relay' (SSR)?", "id": "Relai elektronik tanpa bagian bergerak." },
  { "en": "Singkatan SSR (Solid-State Relay)?", "id": "Solid-State Relay." },
  { "en": "Keuntungan SSR (Solid-State Relay) dibanding relai mekanik?", "id": "Lebih cepat, lebih awet." },
  { "en": "Apa itu 'clock gating'?", "id": "Teknik untuk mengurangi konsumsi daya." },
  { "en": "Cara kerja 'clock gating'?", "id": "Mematikan clock pada blok tidak aktif." },
  { "en": "Apa itu 'power gating'?", "id": "Teknik mematikan catu daya blok." },
  { "en": "Apa itu 'dynamic logic'?", "id": "Gaya logika CMOS yang menggunakan precharge-evaluate." },
  { "en": "Keuntungan 'dynamic logic'?", "id": "Cepat dan jumlah transistor sedikit." },
  { "en": "Kelemahan 'dynamic logic'?", "id": "Kompleks, rentan terhadap 'charge sharing'." },
  { "en": "Apa itu 'domino logic'?", "id": "Varian dari 'dynamic logic'." },
  { "en": "Perusahaan 'foundry' semikonduktor terbesar di dunia?", "id": "TSMC (Taiwan Semiconductor Manufacturing Company)." },
  { "en": "Singkatan TSMC (Taiwan Semiconductor Manufacturing Company)?", "id": "Taiwan Semiconductor Manufacturing Company." },
  { "en": "Siapa yang ikut menemukan sirkuit terpadu?", "id": "Jack Kilby dan Robert Noyce." },
  { "en": "Kapan transistor pertama kali didemonstrasikan?", "id": "1947 di Bell Labs." },
  { "en": "Tiga penemu transistor?", "id": "Bardeen, Brattain, dan Shockley." },
  { "en": "Efek 'hot carrier' disebabkan oleh?", "id": "Medan listrik tinggi dekat drain." },
  { "en": "Crossover distortion disebabkan oleh?", "id": "Tegangan 'turn-on' dioda basis-emitor." },
  { "en": "Penurunan performa akibat 'IR drop' disebut?", "id": "Kegagalan timing atau fungsional." },
  { "en": "Apa itu 'parasitic extraction'?", "id": "Proses menghitung R/C parasitik dari layout." },
  { "en": "Apa itu 'back-annotation'?", "id": "Memasukkan informasi parasitik ke simulasi." },
  { "en": "Apa itu 'static timing analysis' (STA)?", "id": "Analisis timing tanpa simulasi input." },
  { "en": "Singkatan STA (Static Timing Analysis)?", "id": "Static Timing Analysis." },
  { "en": "Apa itu 'standard cell'?", "id": "Blok logika pre-designed (misal: AND, OR)." },
  { "en": "Apa itu 'standard cell library'?", "id": "Kumpulan dari 'standard cell'." },
  { "en": "Apa itu 'resistor ladder'?", "id": "Jaringan resistor seri." },
  { "en": "Aplikasi 'resistor ladder'?", "id": "DAC, pembagi tegangan." },
  { "en": "Apa itu 'Kelvin connection'?", "id": "Metode pengukuran resistansi 4-kawat." },
  { "en": "Tujuan 'Kelvin connection'?", "id": "Menghilangkan error akibat resistansi kabel." },
  { "en": "Apa itu 'diode-connected transistor'?", "id": "Transistor yang dikonfigurasi sebagai dioda." },
  { "en": "Cara membuat 'diode-connected' BJT (Bipolar Junction Transistor)?", "id": "Menghubungkan basis ke kolektor." },
  { "en": "Cara membuat 'diode-connected' MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor)?", "id": "Menghubungkan gerbang ke drain." },
  { "en": "Apa itu 'active resistor'?", "id": "Transistor yang digunakan sebagai resistor." },
  { "en": "Apa itu 'varactor diode'?", "id": "Dioda yang kapasitansinya dikontrol tegangan." },
  { "en": "Nama lain 'varactor diode'?", "id": "Varicap." },
  { "en": "Aplikasi 'varactor diode'?", "id": "Tuning frekuensi pada VCO dan radio." },
  { "en": "Apa itu 'PIN diode'?", "id": "Dioda dengan lapisan intrinsik di tengah." },
  { "en": "Aplikasi 'PIN diode'?", "id": "Saklar RF dan attenuator." },
  { "en": "Apa itu 'Gunn diode'?", "id": "Dioda yang digunakan untuk osilator microwave." },
  { "en": "Apa itu 'IMPATT diode'?", "id": "Dioda lain untuk osilator frekuensi sangat tinggi." },
  { "en": "Apa itu 'photolithography mask'?", "id": "Sinonim untuk photomask." },
  { "en": "Bahan dasar 'photomask'?", "id": "Kaca kuarsa dengan pola kromium." },
  { "en": "Apa itu 'wafer probing'?", "id": "Pengetesan chip saat masih di wafer." },
  { "en": "Alat untuk 'wafer probing'?", "id": "Probe card." },
  { "en": "Apa itu 'burn-in' test?", "id": "Pengetesan komponen pada suhu tinggi." },
  { "en": "Tujuan 'burn-in' test?", "id": "Mendeteksi kegagalan dini (infant mortality)." },
  { "en": "Apa itu 'bathtub curve'?", "id": "Grafik laju kegagalan terhadap waktu." },
  { "en": "Tiga bagian 'bathtub curve'?", "id": "Infant mortality, useful life, wear-out." },
  { "en": "Apa itu 'accelerated life test'?", "id": "Pengetesan dipercepat untuk prediksi umur." },
  { "en": "Apa itu 'Mean Time To Failure' (MTTF)?", "id": "Waktu rata-rata hingga terjadi kegagalan." },
  { "en": "Singkatan MTTF (Mean Time To Failure)?", "id": "Mean Time To Failure." },
  { "en": "Apa itu 'Mean Time Between Failures' (MTBF)?", "id": "Waktu rata-rata antar kegagalan." },
  { "en": "Singkatan MTBF (Mean Time Between Failures)?", "id": "Mean Time Between Failures." },
  { "en": "MTTF (Mean Time To Failure) untuk komponen?", "id": "Yang tidak bisa diperbaiki." },
  { "en": "MTBF (Mean Time Between Failures) untuk sistem?", "id": "Yang bisa diperbaiki." },
  { "en": "Apa itu 'silicon wafer'?", "id": "Sinonim untuk wafer silikon." },
  { "en": "Apa itu 'crystal orientation'?", "id": "Orientasi dari kisi kristal wafer." },
  { "en": "Orientasi wafer yang umum?", "id": "<100> dan <111>." },
  { "en": "Apa itu 'wafer flat' atau 'notch'?", "id": "Tanda untuk menentukan orientasi kristal." },
  { "en": "Apa itu 'getter' dalam tabung vakum?", "id": "Material untuk menyerap sisa gas." },
  { "en": "Apa itu 'phosphor'?", "id": "Material yang berpendar saat terkena elektron." },
  { "en": "Aplikasi 'phosphor'?", "id": "Layar CRT dan lampu fluoresen." },
  { "en": "Singkatan CRT (Cathode-Ray Tube)?", "id": "Cathode-Ray Tube." },
  { "en": "Apa itu 'thermionic emission'?", "id": "Emisi elektron dari permukaan panas." },
  { "en": "Prinsip dasar kerja tabung vakum?", "id": "Thermionic emission." },
  { "en": "Apa itu 'semiconductor foundry'?", "id": "Sinonim untuk foundry." },
  { "en": "Apa itu 'semiconductor node' (misal: 7nm)?", "id": "Nama untuk generasi teknologi fabrikasi." },
  { "en": "Angka pada 'node' merujuk pada?", "id": "Dimensi kritis transistor (secara historis)." },
  { "en": "Apakah 'node' 7nm berarti panjang gerbang 7nm?", "id": "Tidak lagi, sekarang hanya nama marketing." },
  { "en": "Hukum Moore pertama kali dipublikasikan pada?", "id": "Tahun 1965." },
  { "en": "Siapa Gordon Moore?", "id": "Salah satu pendiri Intel." },
  { "en": "Apa itu 'wafer scale integration'?", "id": "Membuat satu chip raksasa seukuran wafer." },
  { "en": "Apa itu 'die stacking'?", "id": "Menumpuk beberapa die secara vertikal." },
  { "en": "Teknologi di balik 'die stacking'?", "id": "TSV (Through-Silicon Via)." },
  { "en": "Apa itu 'chiplet'?", "id": "Die kecil yang digabungkan dalam satu package." },
  { "en": "Keuntungan arsitektur 'chiplet'?", "id": "Meningkatkan yield dan fleksibilitas desain." },
  { "en": "Apa itu 'heterogeneous integration'?", "id": "Menggabungkan chiplet dari proses berbeda." },
  { "en": "Apa itu 'photonic integrated circuit' (PIC)?", "id": "Sirkuit terpadu yang memanipulasi cahaya." },
  { "en": "Singkatan PIC (Photonic Integrated Circuit)?", "id": "Photonic Integrated Circuit." },
  { "en": "Bahan dasar PIC (Photonic Integrated Circuit)?", "id": "Silicon photonics, Indium phosphide." },
  { "en": "Apa itu 'quantum computing'?", "id": "Komputasi menggunakan fenomena kuantum." },
  { "en": "Unit dasar 'quantum computing'?", "id": "Qubit (quantum bit)." },
  { "en": "Sifat 'qubit'?", "id": "Superposisi dan 'entanglement'." },
  { "en": "Apa itu 'conversion gain'?", "id": "Ukuran penguatan pada mixer." },
  { "en": "Apa itu 'local oscillator' (LO)?", "id": "Osilator lokal untuk proses mixing." },
  { "en": "Singkatan LO (Local Oscillator)?", "id": "Local Oscillator." },
  { "en": "Apa itu 'feedthrough' LO (Local Oscillator)?", "id": "Kebocoran sinyal LO ke output." },
  { "en": "Apa itu 'image-reject mixer'?", "id": "Mixer yang menekan frekuensi bayangan." },
  { "en": "Apa itu 'single-balanced mixer'?", "id": "Jenis mixer dengan penolakan port." },
  { "en": "Apa itu 'double-balanced mixer'?", "id": "Mixer dengan penolakan port lebih baik." },
  { "en": "Apa itu 'pipeline' ADC (Analog-to-Digital Converter)?", "id": "ADC kecepatan tinggi dengan latensi." },
  { "en": "Struktur 'pipeline' ADC (Analog-to-Digital Converter) terdiri dari?", "id": "Beberapa tahap yang identik." },
  { "en": "Apa itu 'subranging' ADC (Analog-to-Digital Converter)?", "id": "Arsitektur ADC dua tahap." },
  { "en": "Apa itu 'string' DAC (Digital-to-Analog Converter)?", "id": "DAC yang menggunakan rantai pembagi tegangan." },
  { "en": "Apa itu 'segmented' DAC (Digital-to-Analog Converter)?", "id": "Kombinasi beberapa arsitektur DAC." },
  { "en": "Apa itu 'glitch energy' pada DAC?", "id": "Energi dari pulsa glitch transien." },
  { "en": "Apa itu 'formal verification'?", "id": "Verifikasi desain menggunakan pembuktian matematis." },
  { "en": "Apa itu 'logic equivalence checking' (LEC)?", "id": "Membandingkan dua netlist secara fungsional." },
  { "en": "Singkatan LEC (Logic Equivalence Checking)?", "id": "Logic Equivalence Checking." },
  { "en": "Apa itu 'synthesis constraints'?", "id": "Batasan (timing, area) untuk proses sintesis." },
  { "en": "Contoh 'synthesis constraint'?", "id": "Frekuensi clock, delay maksimum." },
  { "en": "Apa itu 'static timing analysis' (STA)?", "id": "Sinonim untuk analisis timing statis." },
  { "en": "Apa itu 'false path'?", "id": "Jalur logika yang secara fungsional tidak mungkin." },
  { "en": "Apa itu 'multicycle path'?", "id": "Jalur yang diizinkan memiliki beberapa siklus." },
  { "en": "Apa itu 'clock tree synthesis' (CTS)?", "id": "Proses membangun jaringan distribusi clock." },
  { "en": "Singkatan CTS (Clock Tree Synthesis)?", "id": "Clock Tree Synthesis." },
  { "en": "Tujuan 'clock tree synthesis' (CTS)?", "id": "Meminimalkan clock skew dan delay." },
  { "en": "Apa itu 'drift-diffusion model'?", "id": "Model fisika untuk transport pembawa muatan." },
  { "en": "Dua komponen arus dalam model ini?", "id": "Arus drift dan arus difusi." },
  { "en": "Apa itu 'Shockley-Read-Hall (SRH) recombination'?", "id": "Rekombinasi melalui 'trap' atau cacat kristal." },
  { "en": "Singkatan SRH (Shockley-Read-Hall)?", "id": "Shockley-Read-Hall." },
  { "en": "Apa itu 'Auger recombination'?", "id": "Rekombinasi tiga partikel non-radiatif." },
  { "en": "Dominan pada konsentrasi pembawa tinggi?", "id": "Auger recombination." },
  { "en": "Apa itu 'surface recombination'?", "id": "Rekombinasi yang terjadi di permukaan semikonduktor." },
  { "en": "Apa itu 'passivation'?", "id": "Proses melindungi permukaan semikonduktor." },
  { "en": "Tujuan 'passivation'?", "id": "Mengurangi rekombinasi permukaan." },
  { "en": "Proses memindahkan pola ke wafer disebut?", "id": "Fotolitografi." },
  { "en": "Sirkuit yang menghasilkan gelombang periodik?", "id": "Osilator." },
  { "en": "Sirkuit pemilih data digital?", "id": "Multiplexer (MUX)." },
  { "en": "Perangkat yang mengubah cahaya ke listrik?", "id": "Fotodetektor (misal: fotodioda)." },
  { "en": "Apa itu 'avalanche photodiode' (APD)?", "id": "Fotodioda dengan penguatan internal." },
  { "en": "Singkatan APD (Avalanche Photodiode)?", "id": "Avalanche Photodiode." },
  { "en": "Proses penguatan pada APD (Avalanche Photodiode)?", "id": "Avalanche multiplication." },
  { "en": "Apa itu 'quantum efficiency' (QE)?", "id": "Ukuran efisiensi konversi foton-elektron." },
  { "en": "Singkatan QE (Quantum Efficiency)?", "id": "Quantum Efficiency." },
  { "en": "Apa itu 'responsivity'?", "id": "Ukuran lain dari sensitivitas fotodetektor." },
  { "en": "Apa itu 'fill factor' sel surya?", "id": "Ukuran kualitas dari sel surya." },
  { "en": "Apa itu 'open-circuit voltage' (Voc)?", "id": "Tegangan sel surya tanpa beban." },
  { "en": "Singkatan Voc (Voltage, open-circuit)?", "id": "Voltage, open-circuit." },
  { "en": "Apa itu 'short-circuit current' (Isc)?", "id": "Arus sel surya saat dihubung singkat." },
  { "en": "Singkatan Isc (Current, short-circuit)?", "id": "Current, short-circuit." },
  { "en": "Titik daya maksimum (MPP) sel surya?", "id": "Titik operasi dengan daya output maksimal." },
  { "en": "Singkatan MPP (Maximum Power Point)?", "id": "Maximum Power Point." },
  { "en": "Apa itu 'MPPT (Maximum Power Point Tracking)'?", "id": "Teknik untuk beroperasi di MPP." },
  { "en": "Singkatan MPPT (Maximum Power Point Tracking)?", "id": "Maximum Power Point Tracking." },
  { "en": "Apa itu 'gate driver'?", "id": "Sirkuit untuk mengemudikan gerbang transistor daya." },
  { "en": "Tujuan 'gate driver'?", "id": "Menyediakan arus besar untuk switching cepat." },
  { "en": "Apa itu 'bootstrap power supply'?", "id": "Teknik untuk menyediakan daya ke gate driver." },
  { "en": "Apa itu 'snubber circuit'?", "id": "Sirkuit untuk menekan lonjakan tegangan." },
  { "en": "Aplikasi 'snubber circuit'?", "id": "Melindungi saklar elektronik (transistor)." },
  { "en": "Apa itu 'freewheeling diode'?", "id": "Dioda untuk melindungi dari tegangan induktif." },
  { "en": "Aplikasi 'freewheeling diode'?", "id": "Pada beban induktif seperti motor." },
  { "en": "Apa itu 'semiconductor strain gauge'?", "id": "Sensor regangan berbasis semikonduktor." },
  { "en": "Keuntungan 'semiconductor strain gauge'?", "id": "Sensitivitas sangat tinggi." },
  { "en": "Apa itu 'Hall effect sensor'?", "id": "Sensor untuk mendeteksi medan magnet." },
  { "en": "Prinsip dasar 'Hall effect'?", "id": "Pembelokan pembawa muatan oleh medan magnet." },
  { "en": "Apa itu 'thermocouple'?", "id": "Sensor suhu berbasis efek Seebeck." },
  { "en": "Apa itu 'thermistor'?", "id": "Resistor yang resistansinya sensitif suhu." },
  { "en": "Jenis 'thermistor'?", "id": "PTC dan NTC." },
  { "en": "Singkatan PTC (Positive Temperature Coefficient)?", "id": "Positive Temperature Coefficient." },
  { "en": "Singkatan NTC (Negative Temperature Coefficient)?", "id": "Negative Temperature Coefficient." },
  { "en": "Apa itu 'Resistance Temperature Detector' (RTD)?", "id": "Sensor suhu berbasis resistansi logam." },
  { "en": "Singkatan RTD (Resistance Temperature Detector)?", "id": "Resistance Temperature Detector." },
  { "en": "Material yang umum untuk RTD (Resistance Temperature Detector)?", "id": "Platinum (platina)." },
  { "en": "Apa itu 'integrated sensor'?", "id": "Sensor yang terintegrasi dengan sirkuit pemroses." },
  { "en": "Apa itu 'smart sensor'?", "id": "Sensor dengan kemampuan komputasi onboard." },
  { "en": "Apa itu 'wafer-level packaging' (WLP)?", "id": "Proses packaging dilakukan di level wafer." },
  { "en": "Singkatan WLP (Wafer-Level Packaging)?", "id": "Wafer-Level Packaging." },
  { "en": "Apa itu 'fan-out wafer-level packaging' (FOWLP)?", "id": "Varian dari WLP untuk pin lebih banyak." },
  { "en": "Singkatan FOWLP (Fan-Out Wafer-Level Packaging)?", "id": "Fan-Out Wafer-Level Packaging." },
  { "en": "Apa itu 'thermal interface material' (TIM)?", "id": "Material untuk transfer panas." },
  { "en": "Singkatan TIM (Thermal Interface Material)?", "id": "Thermal Interface Material." },
  { "en": "Aplikasi TIM (Thermal Interface Material)?", "id": "Antara chip dan heat sink." },
  { "en": "Apa itu 'wire bonding'?", "id": "Sinonim untuk ikatan kawat." },
  { "en": "Material kawat untuk 'wire bonding'?", "id": "Emas (Au) atau Aluminium (Al)." },
  { "en": "Apa itu 'ball bonding'?", "id": "Jenis wire bonding yang umum." },
  { "en": "Apa itu 'wedge bonding'?", "id": "Jenis wire bonding yang lain." },
  { "en": "Apa itu 'lead frame'?", "id": "Kerangka logam tempat die dipasang." },
  { "en": "Apa itu 'substrate' dalam packaging?", "id": "Papan interposer untuk koneksi." },
  { "en": "Apa itu 'ball grid array' (BGA)?", "id": "Jenis kemasan dengan bola solder." },
  { "en": "Singkatan BGA (Ball Grid Array)?", "id": "Ball Grid Array." },
  { "en": "Apa itu 'quad flat package' (QFP)?", "id": "Kemasan dengan pin di keempat sisi." },
  { "en": "Singkatan QFP (Quad Flat Package)?", "id": "Quad Flat Package." },
  { "en": "Apa itu 'small-outline integrated circuit' (SOIC)?", "id": "Jenis kemasan surface-mount." },
  { "en": "Singkatan SOIC (Small-Outline Integrated Circuit)?", "id": "Small-Outline Integrated Circuit." },
  { "en": "Apa itu 'dual in-line package' (DIP)?", "id": "Kemasan through-hole dengan dua baris pin." },
  { "en": "Singkatan DIP (Dual In-line Package)?", "id": "Dual In-line Package." },
  { "en": "Apa itu 'surface-mount technology' (SMT)?", "id": "Teknologi pemasangan komponen di permukaan." },
  { "en": "Singkatan SMT (Surface-Mount Technology)?", "id": "Surface-Mount Technology." },
  { "en": "Apa itu 'through-hole technology' (THT)?", "id": "Teknologi pemasangan komponen menembus PCB." },
  { "en": "Singkatan THT (Through-Hole Technology)?", "id": "Through-Hole Technology." },
  { "en": "Apa itu 'Widlar current source'?", "id": "Cermin arus dengan resistor emitor." },
  { "en": "Tujuan 'Widlar current source'?", "id": "Menghasilkan arus output yang kecil." },
  { "en": "Apa itu 'Wilson current mirror'?", "id": "Cermin arus dengan impedansi output tinggi." },
  { "en": "Keuntungan 'Wilson current mirror'?", "id": "Matching arus yang lebih baik." },
  { "en": "Apa itu penguat 'folded cascode'?", "id": "Varian cascode untuk rentang tegangan lebar." },
  { "en": "Apa itu 'current-steering' DAC (Digital-to-Analog Converter)?", "id": "DAC yang menjumlahkan sumber arus." },
  { "en": "Keuntungan 'current-steering' DAC (Digital-to-Analog Converter)?", "id": "Kecepatan konversi sangat tinggi." },
  { "en": "Apa itu 'antenna effect' dalam fabrikasi?", "id": "Penumpukan muatan yang merusak gerbang." },
  { "en": "Penyebab 'antenna effect'?", "id": "Proses etsa plasma." },
  { "en": "Solusi untuk 'antenna effect'?", "id": "Menambahkan 'antenna diode' atau 'jumper'." },
  { "en": "Apa itu 'power grid' (jaringan daya) pada chip?", "id": "Jaringan distribusi VDD dan VSS/GND." },
  { "en": "Apa itu 'voltage droop'?", "id": "Sinonim untuk 'IR drop'." },
  { "en": "Perbedaan Flash NAND dan NOR?", "id": "Arsitektur sel dan antarmuka." },
  { "en": "Flash NAND cocok untuk?", "id": "Penyimpanan data massal (SSD, SD card)." },
  { "en": "Flash NOR cocok untuk?", "id": "Eksekusi kode langsung (firmware)." },
  { "en": "Mana yang lebih cepat untuk tulis/hapus?", "id": "Flash NAND." },
  { "en": "Mana yang lebih cepat untuk baca acak?", "id": "Flash NOR." },
  { "en": "Apa itu 'access time' memori?", "id": "Waktu dari permintaan hingga data tersedia." },
  { "en": "Apa itu 'cycle time' memori?", "id": "Waktu minimum antara dua akses." },
  { "en": "Apa itu 'latency' memori?", "id": "Delay total untuk akses memori." },
  { "en": "Apa itu 'quality factor' (Q)?", "id": "Ukuran kualitas komponen atau sirkuit resonansi." },
  { "en": "Faktor Q yang tinggi berarti?", "id": "Kehilangan energi rendah, selektivitas baik." },
  { "en": "Apa itu sirkuit 'tank' LC?", "id": "Sirkuit resonan paralel induktor-kapasitor." },
  { "en": "Apa itu 'resonant frequency'?", "id": "Frekuensi osilasi natural sistem." },
  { "en": "Apa itu penguat RF 'Class E'?", "id": "Penguat switching efisiensi sangat tinggi." },
  { "en": "Apa itu penguat RF 'Class F'?", "id": "Varian penguat switching efisiensi tinggi lain." },
  { "en": "Mengapa NMOS (N-channel Metal-Oxide-Semiconductor) lebih cepat dari PMOS (P-channel Metal-Oxide-Semiconductor)?", "id": "Mobilitas elektron lebih tinggi dari hole." },
  { "en": "Apa trade-off gain dan bandwidth?", "id": "Gain tinggi, bandwidth cenderung turun." },
  { "en": "Produk dari gain dan bandwidth adalah?", "id": "Konstan (GBP)." },
  { "en": "Apa itu 'noise temperature'?", "id": "Representasi noise dalam satuan temperatur." },
  { "en": "Apa itu 'shot noise'?", "id": "Noise acak akibat sifat diskrit muatan." },
  { "en": "Apa itu 'thermal noise'?", "id": "Noise akibat agitasi termal elektron." },
  { "en": "Daya 'thermal noise' bergantung pada?", "id": "Temperatur dan bandwidth." },
  { "en": "Apa itu 'phase noise'?", "id": "Fluktuasi acak pada fasa sinyal." },
  { "en": "Efek 'phase noise' pada osilator?", "id": "Menyebabkan ketidakstabilan frekuensi." },
  { "en": "Apa itu 'vacuum tube' (tabung vakum)?", "id": "Komponen aktif sebelum era transistor." },
  { "en": "Jenis 'vacuum tube'?", "id": "Dioda, trioda, pentoda." },
  { "en": "Apa itu 'cathode'?", "id": "Elemen pemancar elektron pada tabung." },
  { "en": "Apa itu 'anode' (plate)?", "id": "Elemen pengumpul elektron pada tabung." },
  { "en": "Apa itu 'grid'?", "id": "Elemen kontrol pada tabung trioda." },
  { "en": "Apa itu MESFET (Metal-Semiconductor Field-Effect Transistor)?", "id": "FET yang menggunakan dioda Schottky." },
  { "en": "Singkatan MESFET (Metal-Semiconductor Field-Effect Transistor)?", "id": "Metal-Semiconductor Field-Effect Transistor." },
  { "en": "Material umum untuk MESFET (Metal-Semiconductor Field-Effect Transistor)?", "id": "Gallium Arsenide (GaAs)." },
  { "en": "Apa itu HEMT (High-Electron-Mobility Transistor)?", "id": "Transistor kecepatan sangat tinggi." },
  { "en": "Singkatan HEMT (High-Electron-Mobility Transistor)?", "id": "High-Electron-Mobility Transistor." },
  { "en": "Struktur HEMT (High-Electron-Mobility Transistor) disebut juga?", "id": "Heterojunction." },
  { "en": "Apa itu 'heterojunction'?", "id": "Sambungan antara dua semikonduktor berbeda." },
  { "en": "Apa itu 'quantum well'?", "id": "Lapisan tipis yang mengurung elektron." },
  { "en": "Elektron dalam 'quantum well' disebut?", "id": "2DEG (Two-Dimensional Electron Gas)." },
  { "en": "Singkatan 2DEG (Two-Dimensional Electron Gas)?", "id": "Two-Dimensional Electron Gas." },
  { "en": "Apa itu 'integrated passive device' (IPD)?", "id": "Komponen pasif (R,L,C) dalam satu chip." },
  { "en": "Singkatan IPD (Integrated Passive Device)?", "id": "Integrated Passive Device." },
  { "en": "Apa itu 'wafer bumping'?", "id": "Proses menambahkan bola solder ke wafer." },
  { "en": "Tujuan 'wafer bumping'?", "id": "Untuk koneksi flip-chip." },
  { "en": "Apa itu 'thermal management'?", "id": "Pengelolaan panas pada perangkat elektronik." },
  { "en": "Apa itu 'thermal resistance'?", "id": "Ukuran kesulitan panas mengalir." },
  { "en": "Satuan 'thermal resistance'?", "id": "Derajat Celsius per Watt (°C/W)." },
  { "en": "Apa itu 'junction temperature' (Tj)?", "id": "Suhu pada sambungan P-N semikonduktor." },
  { "en": "Apa itu 'ambient temperature' (Ta)?", "id": "Suhu lingkungan sekitar." },
  { "en": "Apa itu 'derating'?", "id": "Mengurangi rating daya pada suhu tinggi." },
  { "en": "Apa itu 'passivation layer'?", "id": "Lapisan pelindung akhir pada die." },
  { "en": "Material 'passivation layer'?", "id": "Silicon nitride atau silicon dioxide." },
  { "en": "Apa itu 'Fermi potential'?", "id": "Potensial yang berhubungan dengan level Fermi." },
  { "en": "Apa itu 'built-in potential'?", "id": "Sinonim untuk tegangan penghalang dioda." },
  { "en": "Apa itu 'Early voltage'?", "id": "Parameter yang memodelkan Early effect." },
  { "en": "Apa itu 'Gummel plot'?", "id": "Plot karakteristik arus BJT." },
  { "en": "Apa itu 'Gummel-Poon model'?", "id": "Model SPICE yang detail untuk BJT." },
  { "en": "Apa itu 'BSIM' model?", "id": "Model SPICE standar industri untuk MOSFET." },
  { "en": "Singkatan BSIM (Berkeley Short-channel IGFET Model)?", "id": "Berkeley Short-channel IGFET Model." },
  { "en": "Apa itu 'process design kit' (PDK)?", "id": "Kumpulan file data dari foundry." },
  { "en": "Singkatan PDK (Process Design Kit)?", "id": "Process Design Kit." },
  { "en": "Isi dari PDK (Process Design Kit)?", "id": "Model SPICE, design rules, layout." },
  { "en": "Apa itu 'electronic design automation' (EDA)?", "id": "Software untuk mendesain sirkuit elektronik." },
  { "en": "Singkatan EDA (Electronic Design Automation)?", "id": "Electronic Design Automation." },
  { "en": "Contoh perusahaan EDA (Electronic Design Automation)?", "id": "Cadence, Synopsys, Mentor Graphics." },
  { "en": "Apa itu 'GDSII'?", "id": "Format file standar untuk layout IC." },
  { "en": "Apa itu 'Oasis'?", "id": "Format file layout yang lebih baru." },
  { "en": "Apa itu 'logical effort'?", "id": "Metode untuk analisis delay gerbang logika." },
  { "en": "Apa itu 'parasitic capacitance'?", "id": "Kapasitansi tak diinginkan antar konduktor." },
  { "en": "Apa itu 'parasitic resistance'?", "id": "Resistansi tak diinginkan pada jalur." },
  { "en": "Apa itu 'parasitic inductance'?", "id": "Induktansi tak diinginkan pada jalur." },
  { "en": "Efek parasitik dominan pada frekuensi tinggi?", "id": "Induktansi." },
  { "en": "Apa itu 'skin effect'?", "id": "Arus cenderung mengalir di permukaan." },
  { "en": "Kapan 'skin effect' signifikan?", "id": "Pada frekuensi sangat tinggi (RF)." },
  { "en": "Apa itu 'dielectric loss'?", "id": "Kehilangan energi pada material isolator." },
  { "en": "Apa itu 'tangent loss'?", "id": "Parameter yang mengukur 'dielectric loss'." },
  { "en": "Apa itu 'microstrip'?", "id": "Jenis 'transmission line' pada PCB." },
  { "en": "Apa itu 'stripline'?", "id": "Jenis 'transmission line' lain." },
  { "en": "Apa itu 'coplanar waveguide'?", "id": "Jenis 'transmission line' lain lagi." },
  { "en": "Apa itu 'power divider'?", "id": "Sirkuit untuk membagi sinyal RF." },
  { "en": "Apa itu 'directional coupler'?", "id": "Mengambil sampel sinyal tanpa mengganggu." },
  { "en": "Apa itu 'circulator'?", "id": "Perangkat RF tiga port non-resiprokal." },
  { "en": "Apa itu 'isolator'?", "id": "Perangkat RF yang melewatkan sinyal searah." },
  { "en": "Apa itu 'spintronics'?", "id": "Elektronik yang memanfaatkan spin elektron." },
  { "en": "Aplikasi umum Schmitt trigger?", "id": "Debouncing saklar dan osilator relaksasi." },
  { "en": "Apa itu 'debouncing'?", "id": "Menghilangkan pantulan sinyal dari saklar mekanik." },
  { "en": "Apa itu 'crowbar circuit'?", "id": "Sirkuit proteksi terhadap tegangan berlebih." },
  { "en": "Cara kerja 'crowbar circuit'?", "id": "Menghubung singkat catu daya saat overvoltage." },
  { "en": "Apa itu 'voltage reference'?", "id": "Sirkuit penghasil tegangan DC yang sangat stabil." },
  { "en": "Jenis 'voltage reference' selain bandgap?", "id": "Referensi berbasis dioda Zener." },
  { "en": "Apa itu 'temperature coefficient'?", "id": "Ukuran perubahan parameter terhadap suhu." },
  { "en": "Voltage reference yang baik memiliki?", "id": "Koefisien temperatur mendekati nol." },
  { "en": "Apa itu 'wet etching'?", "id": "Proses etsa menggunakan bahan kimia cair." },
  { "en": "Apa itu 'dry etching'?", "id": "Proses etsa menggunakan plasma atau gas." },
  { "en": "Contoh 'dry etching'?", "id": "Reactive Ion Etching (RIE)." },
  { "en": "Singkatan RIE (Reactive Ion Etching)?", "id": "Reactive Ion Etching." },
  { "en": "Apa itu etsa 'isotropic'?", "id": "Etsa ke segala arah dengan laju sama." },
  { "en": "Apa itu etsa 'anisotropic'?", "id": "Etsa pada arah vertikal lebih cepat." },
  { "en": "Etsa 'anisotropic' penting untuk?", "id": "Membentuk fitur dengan dinding curam." },
  { "en": "Apa itu 'dopant'?", "id": "Atom pengotor yang ditambahkan saat doping." },
  { "en": "Apa itu 'clock recovery'?", "id": "Proses mengekstrak sinyal clock dari data." },
  { "en": "Nama lain 'clock recovery'?", "id": "Timing recovery." },
  { "en": "Apa itu 'data slicer'?", "id": "Komparator untuk meregenerasi sinyal digital." },
  { "en": "Apa itu 'settling time' pada op-amp?", "id": "Waktu output masuk dalam rentang error." },
  { "en": "Apa itu 'overshoot' pada op-amp?", "id": "Output melebihi nilai steady-state sesaat." },
  { "en": "Alat untuk mengukur kurva I-V transistor?", "id": "Curve tracer atau SMU." },
  { "en": "Singkatan SMU (Source Measure Unit)?", "id": "Source Measure Unit." },
  { "en": "Apa itu 'four-point probe'?", "id": "Metode mengukur resistivitas lembaran (sheet resistance)." },
  { "en": "Apa itu 'ellipsometry'?", "id": "Teknik optik mengukur ketebalan lapisan tipis." },
  { "en": "Apa itu 'profilometer'?", "id": "Mengukur topografi permukaan atau ketinggian step." },
  { "en": "Impedansi input penguat common-emitter?", "id": "Sedang (medium)." },
  { "en": "Impedansi input penguat common-collector?", "id": "Tinggi." },
  { "en": "Impedansi output penguat common-collector?", "id": "Rendah." },
  { "en": "Impedansi input penguat common-base?", "id": "Rendah." },
  { "en": "Impedansi output penguat common-base?", "id": "Tinggi." },
  { "en": "Penyebab utama disipasi daya statis CMOS?", "id": "Arus bocor (leakage current)." },
  { "en": "Penyebab utama disipasi daya dinamis CMOS?", "id": "Pengisian dan pengosongan kapasitor." },
  { "en": "Apa itu 'short-circuit power'?", "id": "Daya terdisipasi saat transisi PMOS-NMOS." },
  { "en": "Apa itu 'valence electron'?", "id": "Elektron di kulit terluar atom." },
  { "en": "Ikatan kimia dalam kristal silikon?", "id": "Ikatan kovalen." },
  { "en": "Berapa elektron valensi atom silikon?", "id": "Empat." },
  { "en": "Apa itu 'lattice' (kisi)?", "id": "Susunan atom yang teratur dalam kristal." },
  { "en": "Apa itu 'unit cell'?", "id": "Blok bangunan terkecil dari kisi kristal." },
  { "en": "Struktur kristal silikon dan germanium?", "id": "Diamond cubic." },
  { "en": "Apa itu 'polycrystalline silicon' (polisilikon)?", "id": "Silikon yang terdiri dari banyak kristal kecil." },
  { "en": "Aplikasi polisilikon?", "id": "Material gerbang (gate) pada MOSFET." },
  { "en": "Apa itu 'amorphous silicon'?", "id": "Silikon tanpa struktur kristal teratur." },
  { "en": "Aplikasi 'amorphous silicon'?", "id": "Sel surya film tipis dan LCD." },
  { "en": "Singkatan LCD (Liquid Crystal Display)?", "id": "Liquid Crystal Display." },
  { "en": "Apa itu 'negative differential resistance' (NDR)?", "id": "Fenomena di mana arus turun saat tegangan naik." },
  { "en": "Singkatan NDR (Negative Differential Resistance)?", "id": "Negative Differential Resistance." },
  { "en": "Perangkat yang menunjukkan NDR (Negative Differential Resistance)?", "id": "Tunnel diode, Gunn diode." },
  { "en": "Apa itu 'doping concentration'?", "id": "Jumlah atom dopan per volume." },
  { "en": "Satuan 'doping concentration'?", "id": "atom per cm³." },
  { "en": "Apa itu 'heavily doped'?", "id": "Semikonduktor dengan konsentrasi doping tinggi." },
  { "en": "Notasi untuk semikonduktor 'heavily doped'?", "id": "n+ atau p+." },
  { "en": "Apa itu 'lightly doped'?", "id": "Semikonduktor dengan konsentrasi doping rendah." },
  { "en": "Apa itu 'depletion approximation'?", "id": "Asumsi untuk analisis sambungan P-N." },
  { "en": "Apa itu 'minority carrier injection'?", "id": "Injeksi minoritas saat dioda bias maju." },
  { "en": "Apa itu 'diffusion capacitance'?", "id": "Kapasitansi pada dioda bias maju." },
  { "en": "Apa itu 'junction capacitance'?", "id": "Kapasitansi pada dioda bias mundur." },
  { "en": "Nama lain 'junction capacitance'?", "id": "Depletion capacitance." },
  { "en": "Apa itu 'saturasi' pada BJT?", "id": "Kondisi di mana BJT fully on." },
  { "en": "Tegangan V_CE(sat) pada BJT?", "id": "Sangat rendah (sekitar 0.2V)." },
  { "en": "Daerah 'cut-off' pada BJT?", "id": "Kondisi di mana BJT fully off." },
  { "en": "Daerah aktif pada BJT?", "id": "Daerah operasi untuk penguatan." },
  { "en": "Apa itu 'base-width modulation'?", "id": "Sinonim untuk Early Effect." },
  { "en": "Apa itu 'Ebers-Moll model'?", "id": "Model matematis skala besar untuk BJT." },
  { "en": "Apa itu 'hybrid-pi model'?", "id": "Model small-signal populer untuk BJT." },
  { "en": "Parameter r_π dalam model hybrid-pi?", "id": "Resistansi input basis-emitor." },
  { "en": "Apa itu 'channel' pada MOSFET?", "id": "Daerah konduktif di bawah gerbang." },
  { "en": "Apa itu 'pinch-off'?", "id": "Kondisi awal saturasi pada FET." },
  { "en": "Apa itu 'bulk' atau 'substrate'?", "id": "Terminal keempat pada MOSFET." },
  { "en": "Apa itu 'inverter' CMOS?", "id": "Gerbang NOT yang dibuat dari PMOS dan NMOS." },
  { "en": "Apa itu 'voltage transfer characteristic' (VTC)?", "id": "Kurva V_out vs V_in." },
  { "en": "Singkatan VTC (Voltage Transfer Characteristic)?", "id": "Voltage Transfer Characteristic." },
  { "en": "Bentuk VTC (Voltage Transfer Characteristic) inverter ideal?", "id": "Sangat curam." },
  { "en": "Apa itu 'noise margin high' (NMH)?", "id": "Toleransi noise untuk level logika tinggi." },
  { "en": "Singkatan NMH (Noise Margin High)?", "id": "Noise Margin High." },
  { "en": "Apa itu 'noise margin low' (NML)?", "id": "Toleransi noise untuk level logika rendah." },
  { "en": "Singkatan NML (Noise Margin Low)?", "id": "Noise Margin Low." },
  { "en": "Apa itu 'power-delay product' (PDP)?", "id": "Ukuran efisiensi energi gerbang logika." },
  { "en": "Singkatan PDP (Power-Delay Product)?", "id": "Power-Delay Product." },
  { "en": "Nilai PDP (Power-Delay Product) yang baik?", "id": "Sekecil mungkin." },
  { "en": "Apa itu 'energy-delay product' (EDP)?", "id": "Ukuran efisiensi lain yang lebih baik." },
  { "en": "Singkatan EDP (Energy-Delay Product)?", "id": "Energy-Delay Product." },
  { "en": "Apa itu 'pull-up network' (PUN)?", "id": "Jaringan transistor yang terhubung ke VDD." },
  { "en": "Singkatan PUN (Pull-Up Network)?", "id": "Pull-Up Network." },
  { "en": "Pada inverter CMOS, PUN adalah?", "id": "Transistor PMOS." },
  { "en": "Apa itu 'pull-down network' (PDN)?", "id": "Jaringan transistor yang terhubung ke Ground." },
  { "en": "Singkatan PDN (Pull-Down Network)?", "id": "Pull-Down Network." },
  { "en": "Pada inverter CMOS, PDN adalah?", "id": "Transistor NMOS." },
  { "en": "Apa itu 'ratioless logic'?", "id": "Logika yang fungsinya tidak bergantung rasio W/L." },
  { "en": "Logika CMOS (Complementary Metal-Oxide-Semiconductor) bersifat?", "id": "Ratioless." },
  { "en": "Apa itu 'ratioed logic'?", "id": "Logika yang bergantung pada rasio W/L." },
  { "en": "Contoh 'ratioed logic'?", "id": "Logika pseudo-NMOS." },
  { "en": "Apa itu 'body bias effect'?", "id": "Sinonim untuk body effect." },
  { "en": "Apa itu 'barrel shifter'?", "id": "Sirkuit untuk pergeseran bit variabel." },
  { "en": "Apa itu 'Wallace tree multiplier'?", "id": "Arsitektur multiplier digital kecepatan tinggi." },
  { "en": "Apa itu 'carry-select adder'?", "id": "Adder cepat yang menggunakan multiplexer." },
  { "en": "Apa itu 'carry-save adder'?", "id": "Adder yang menunda propagasi carry." },
  { "en": "Apa itu 'one-hot encoding'?", "id": "Representasi state dengan satu bit aktif." },
  { "en": "Keuntungan 'one-hot encoding'?", "id": "Logika dekode yang sederhana dan cepat." },
  { "en": "Apa itu 'gray code'?", "id": "Kode biner di mana transisi hanya mengubah satu bit." },
  { "en": "Tujuan 'gray code'?", "id": "Mengurangi error pada sistem sekuensial." },
  { "en": "Apa itu 'input offset voltage' op-amp?", "id": "Tegangan diferensial untuk membuat output nol." },
  { "en": "Penyebab 'input offset voltage'?", "id": "Mismatch pada pasangan transistor input." },
  { "en": "Apa itu 'input bias current' op-amp?", "id": "Arus DC kecil yang mengalir ke input." },
  { "en": "Apa itu 'input offset current'?", "id": "Selisih antara dua 'input bias current'." },
  { "en": "Singkatan PSRR (Power Supply Rejection Ratio)?", "id": "Power Supply Rejection Ratio." },
  { "en": "PSRR (Power Supply Rejection Ratio) mengukur?", "id": "Kemampuan menolak noise dari catu daya." },
  { "en": "Nilai PSRR (Power Supply Rejection Ratio) yang baik?", "id": "Sangat tinggi." },
  { "en": "Apa itu 'common-mode input range'?", "id": "Rentang tegangan input common-mode." },
  { "en": "Apa itu 'output voltage swing'?", "id": "Rentang tegangan output maksimum." },
  { "en": "Apa itu 'blind via' pada PCB?", "id": "Menghubungkan lapisan luar ke dalam." },
  { "en": "Apa itu 'buried via' pada PCB?", "id": "Menghubungkan dua lapisan dalam." },
  { "en": "Apa itu 'through-hole via'?", "id": "Via yang menembus semua lapisan." },
  { "en": "Apa itu 'backgrinding'?", "id": "Proses menipiskan wafer dari belakang." },
  { "en": "Tujuan 'backgrinding'?", "id": "Untuk aplikasi die stacking atau kemasan tipis." },
  { "en": "Garis resistansi konstan pada Smith Chart?", "id": "Berbentuk lingkaran." },
  { "en": "Garis reaktansi konstan pada Smith Chart?", "id": "Berbentuk busur lingkaran." },
  { "en": "Titik tengah Smith Chart merepresentasikan?", "id": "Impedansi yang cocok (matched)." },
  { "en": "Apa itu 'noise factor' (F)?", "id": "Rasio SNR input terhadap SNR output." },
  { "en": "Hubungan 'noise figure' (NF) dan 'noise factor' (F)?", "id": "NF adalah F dalam desibel (dB)." },
  { "en": "NF (Noise Figure) kaskade dipengaruhi paling besar oleh?", "id": "Stage pertama." },
  { "en": "Rumus ini dikenal sebagai?", "id": "Formula Friis." },
  { "en": "Apa itu 'phase margin'?", "id": "Ukuran stabilitas pada sistem umpan balik." },
  { "en": "Phase margin yang baik biasanya?", "id": "Di atas 45 derajat." },
  { "en": "Apa itu 'gain margin'?", "id": "Ukuran stabilitas lain pada sistem umpan balik." },
  { "en": "Apa itu 'serialization'?", "id": "Mengubah data paralel menjadi serial." },
  { "en": "Apa itu 'deserialization'?", "id": "Mengubah data serial menjadi paralel." },
  { "en": "Sirkuit untuk serialisasi/deserialisasi?", "id": "SerDes." },
  { "en": "Singkatan SerDes (Serializer/Deserializer)?", "id": "Serializer/Deserializer." },
  { "en": "Apa itu 'avalanche noise'?", "id": "Noise pada dioda avalanche." },
  { "en": "Apa itu 'burst noise' (popcorn noise)?", "id": "Transisi level DC yang acak dan tiba-tiba." },
  { "en": "Apa itu 'Barkhausen noise'?", "id": "Noise pada material feromagnetik." },
  { "en": "Apa itu 'Flicker noise'?", "id": "Sinonim untuk 1/f noise." },
  { "en": "Apa itu 'phase noise'?", "id": "Sinonim untuk noise fasa." },
  { "en": "Apa itu 'quantization noise'?", "id": "Sinonim untuk error kuantisasi." },
  { "en": "Apa itu 'aliasing noise'?", "id": "Noise akibat sampling di bawah laju Nyquist." },
  { "en": "Apa itu 'Miller capacitance'?", "id": "Kapasitansi efektif akibat Miller effect." },
  { "en": "Apa itu 'Gunn effect'?", "id": "Efek yang mendasari dioda Gunn." },
  { "en": "Apa itu 'Zener effect'?", "id": "Mekanisme breakdown pada dioda Zener." },
  { "en": "Apa itu 'avalanche effect'?", "id": "Mekanisme breakdown pada dioda avalanche." },
  { "en": "Apa itu 'photoconductive effect'?", "id": "Peningkatan konduktivitas akibat cahaya." },
  { "en": "Apa itu 'photovoltaic effect'?", "id": "Pembangkitan tegangan akibat cahaya." },
  { "en": "Apa itu 'photoelectric effect'?", "id": "Emisi elektron akibat foton." },
  { "en": "Apa itu 'Seebeck effect'?", "id": "Pembangkitan tegangan akibat perbedaan suhu." },
  { "en": "Prinsip dasar 'thermocouple'?", "id": "Seebeck effect." },
  { "en": "Apa itu 'Peltier effect'?", "id": "Pembangkitan perbedaan suhu akibat arus." },
  { "en": "Aplikasi 'Peltier effect'?", "id": "Pendingin termoelektrik." },
  { "en": "Apa itu 'Thomson effect'?", "id": "Efek termoelektrik lain." },
  { "en": "Apa itu 'piezoresistive effect'?", "id": "Perubahan resistivitas akibat stres mekanis." },
  { "en": "Prinsip dasar 'strain gauge' semikonduktor?", "id": "Piezoresistive effect." },
  { "en": "Apa itu 'integrated optics'?", "id": "Sinonim untuk 'photonic integrated circuit' (PIC)." },
  { "en": "Apa itu 'silicon photonics'?", "id": "Teknologi PIC berbasis silikon." },
  { "en": "Apa itu 'Mach-Zehnder modulator'?", "id": "Modulator optik kecepatan tinggi." },
  { "en": "Apa itu 'ring resonator'?", "id": "Komponen optik untuk filtering atau sensing." },
  { "en": "Apa itu 'vertical-cavity surface-emitting laser' (VCSEL)?", "id": "Jenis laser dioda semikonduktor." },
  { "en": "Singkatan VCSEL (Vertical-Cavity Surface-Emitting Laser)?", "id": "Vertical-Cavity Surface-Emitting Laser." },
  { "en": "Keuntungan VCSEL (Vertical-Cavity Surface-Emitting Laser)?", "id": "Mudah dites dan dibuat dalam array." },
  { "en": "Aplikasi VCSEL (Vertical-Cavity Surface-Emitting Laser)?", "id": "Komunikasi data, sensor 3D (Face ID)." },
  { "en": "Apa itu 'quantum dot'?", "id": "Partikel nano semikonduktor." },
  { "en": "Sifat 'quantum dot' ditentukan oleh?", "id": "Ukurannya." },
  { "en": "Aplikasi 'quantum dot'?", "id": "Layar QLED, pencitraan biomedis." },
  { "en": "Singkatan QLED (Quantum Dot Light Emitting Diode)?", "id": "Quantum Dot Light Emitting Diode." },
  { "en": "Apa itu 'graphene'?", "id": "Lapisan tunggal atom karbon." },
  { "en": "Struktur 'graphene'?", "id": "Kisi heksagonal (sarang lebah)." },
  { "en": "Potensi 'graphene' dalam elektronika?", "id": "Transistor ultra-cepat." },
  { "en": "Apa itu 'carbon nanotube' (CNT)?", "id": "Lembaran graphene yang digulung." },
  { "en": "Singkatan CNT (Carbon Nanotube)?", "id": "Carbon Nanotube." },
  { "en": "Apa itu 'memristor'?", "id": "Elemen sirkuit pasif keempat." },
  { "en": "Properti 'memristor'?", "id": "Resistansinya bergantung pada sejarah arus." },
  { "en": "Potensi aplikasi 'memristor'?", "id": "Memori non-volatile, komputasi neuromorfik." },
  { "en": "Apa itu 'neuromorphic computing'?", "id": "Komputasi yang meniru arsitektur otak." },
  { "en": "Apa itu 'gate-all-around' (GAA) FET?", "id": "Arsitektur transistor penerus FinFET." },
  { "en": "Singkatan GAA (Gate-All-Around)?", "id": "Gate-All-Around." },
  { "en": "Keuntungan GAA (Gate-All-Around) FET?", "id": "Kontrol gerbang yang lebih baik." },
  { "en": "Apa itu 'wafer fabrication'?", "id": "Sinonim untuk proses fabrikasi wafer." },
  { "en": "Apa itu 'front-end-of-line' (FEOL)?", "id": "Tahap awal fabrikasi (pembuatan transistor)." },
  { "en": "Singkatan FEOL (Front-End-of-Line)?", "id": "Front-End-of-Line." },
  { "en": "Apa itu 'back-end-of-line' (BEOL)?", "id": "Tahap akhir fabrikasi (interconnect)." },
  { "en": "Singkatan BEOL (Back-End-of-Line)?", "id": "Back-End-of-Line." },
  { "en": "Apa itu 'Moore's Law'?", "id": "Observasi tentang peningkatan kepadatan transistor." },
  { "en": "Apa itu 'Dennard scaling'?", "id": "Aturan scaling untuk transistor MOSFET." },
  { "en": "Apa itu 'Rent's rule'?", "id": "Hubungan antara jumlah pin dan gerbang." },
  { "en": "Apa itu 'Amdahl's law'?", "id": "Batas peningkatan kecepatan dari paralelisasi." },
  { "en": "Apa itu 'Gantt chart'?", "id": "Diagram untuk penjadwalan proyek." },
  { "en": "Apa itu 'critical path method' (CPM)?", "id": "Metode manajemen proyek." },
  { "en": "Singkatan CPM (Critical Path Method)?", "id": "Critical Path Method." },
  { "en": "Apa itu 'state assignment' pada FSM (Finite State Machine)?", "id": "Proses mengkodekan state menjadi biner." },
  { "en": "Singkatan FSM (Finite State Machine)?", "id": "Finite State Machine." },
  { "en": "Apa itu 'state minimization'?", "id": "Proses mengurangi jumlah state pada FSM." },
  { "en": "Apa itu 'dual-port RAM'?", "id": "RAM dengan dua port akses independen." },
  { "en": "Aplikasi 'dual-port RAM'?", "id": "Komunikasi antar domain clock." },
  { "en": "Apa itu 'FIFO (First-In, First-Out)' buffer?", "id": "Memori antrian, data pertama masuk pertama keluar." },
  { "en": "Singkatan FIFO (First-In, First-Out)?", "id": "First-In, First-Out." },
  { "en": "Apa itu 'LIFO (Last-In, First-Out)' buffer?", "id": "Memori tumpukan (stack)." },
  { "en": "Singkatan LIFO (Last-In, First-Out)?", "id": "Last-In, First-Out." },
  { "en": "Apa itu 'total harmonic distortion plus noise' (THD+N)?", "id": "Ukuran kualitas sinyal audio." },
  { "en": "Singkatan THD+N (Total Harmonic Distortion plus Noise)?", "id": "Total Harmonic Distortion plus Noise." },
  { "en": "Apa itu 'signal-to-noise and distortion' (SINAD)?", "id": "Ukuran lain dari kualitas sinyal." },
  { "en": "Singkatan SINAD (Signal-to-Noise and Distortion)?", "id": "Signal-to-Noise and Distortion." },
  { "en": "Apa itu 'slew-induced distortion'?", "id": "Distorsi akibat slew rate yang terbatas." },
  { "en": "Apa itu 'crosstalk'?", "id": "Gangguan antara dua sinyal berdekatan." },
  { "en": "Apa itu 'silicon cycle'?", "id": "Siklus 'boom-bust' industri semikonduktor." },
  { "en": "Apa itu 'second sourcing'?", "id": "Memiliki lebih dari satu pemasok komponen." },
  { "en": "Tujuan 'second sourcing'?", "id": "Mengurangi risiko rantai pasokan." },
  { "en": "Apa itu 'end-of-life' (EOL) komponen?", "id": "Komponen yang tidak lagi diproduksi." },
  { "en": "Singkatan EOL (End-of-Life)?", "id": "End-of-Life." },
  { "en": "Apa itu 'bill of materials' (BOM)?", "id": "Daftar semua komponen dalam sebuah produk." },
  { "en": "Singkatan BOM (Bill of Materials)?", "id": "Bill of Materials." },
  { "en": "Apa itu 'Pockels effect'?", "id": "Perubahan indeks bias akibat medan listrik." },
  { "en": "Apa itu 'Kerr effect'?", "id": "Efek Pockels orde kedua (kuadratik)." },
  { "en": "Apa itu 'Faraday effect'?", "id": "Rotasi polarisasi cahaya oleh medan magnet." },
  { "en": "Aplikasi 'Faraday effect'?", "id": "Isolator optik." },
  { "en": "Apa itu 'photorefractive effect'?", "id": "Perubahan indeks bias optik akibat cahaya." },
  { "en": "Apa itu 'thermo-optic effect'?", "id": "Perubahan indeks bias akibat suhu." },
  { "en": "Apa itu 'hysteresis'?", "id": "Ketergantungan output pada sejarah input." },
  { "en": "Contoh sirkuit dengan 'hysteresis'?", "id": "Schmitt trigger." },
  { "en": "Apa itu 'soft error'?", "id": "Error bit sementara akibat radiasi." },
  { "en": "Apa itu 'hard error'?", "id": "Kerusakan fisik permanen pada sel memori." },
  { "en": "Apa itu 'wafer sort' atau 'probe'?", "id": "Pengetesan fungsional di tingkat wafer." },
  { "en": "Apa itu 'final test'?", "id": "Pengetesan setelah chip dikemas." },
  { "en": "Apa itu 'guard banding'?", "id": "Memperketat batas tes untuk menjamin spesifikasi." },
  { "en": "Apa itu 'characterization'?", "id": "Proses mengukur performa chip di berbagai kondisi." },
  { "en": "Apa itu 'validation' dalam desain IC?", "id": "Memastikan chip berfungsi sesuai tujuan." },
  { "en": "Apa itu 'emulation'?", "id": "Meniru perilaku satu sistem dengan sistem lain." },
  { "en": "Sistem 'emulation' menggunakan?", "id": "FPGA besar untuk meniru ASIC." },
  { "en": "Apa itu 'assertion-based verification' (ABV)?", "id": "Metodologi verifikasi formal." },
  { "en": "Singkatan ABV (Assertion-Based Verification)?", "id": "Assertion-Based Verification." },
  { "en": "Apa itu 'Universal Verification Methodology' (UVM)?", "id": "Standar metodologi verifikasi." },
  { "en": "Singkatan UVM (Universal Verification Methodology)?", "id": "Universal Verification Methodology." },
  { "en": "Apa itu 'code coverage'?", "id": "Metrik seberapa banyak kode yang dites." },
  { "en": "Apa itu 'functional coverage'?", "id": "Metrik seberapa banyak fitur yang dites." },
  { "en": "Apa itu 'constrained random verification'?", "id": "Menghasilkan stimulus tes secara acak." },
  { "en": "Apa itu 'regression testing'?", "id": "Menjalankan ulang semua tes setelah perubahan." },
  { "en": "Apa itu 'bug tracking system'?", "id": "Sistem untuk melaporkan dan melacak bug." },
  { "en": "Apa itu 'transistor sizing'?", "id": "Menentukan lebar dan panjang transistor." },
  { "en": "Tujuan 'transistor sizing'?", "id": "Optimisasi kecepatan, daya, dan area." },
  { "en": "Apa itu 'logical effort'?", "id": "Metode untuk estimasi delay pada logika CMOS." },
  { "en": "Apa itu 'self-heating effect'?", "id": "Peningkatan suhu transistor akibat disipasi daya." },
  { "en": "Efek 'self-heating' signifikan pada teknologi?", "id": "SOI (Silicon on Insulator) dan FinFET." },
  { "en": "Apa itu 'random dopant fluctuation'?", "id": "Variasi jumlah atom dopan di kanal." },
  { "en": "Efek 'random dopant fluctuation'?", "id": "Menyebabkan variasi V_T antar transistor." },
  { "en": "Apa itu 'line edge roughness' (LER)?", "id": "Ketidaksempurnaan pada tepi garis hasil litografi." },
  { "en": "Singkatan LER (Line Edge roughness)?", "id": "Line Edge roughness." },
  { "en": "Apa itu 'phase noise'?", "id": "Fluktuasi acak pada fasa sinyal." },
  { "en": "Phase noise penting pada sirkuit?", "id": "Osilator dan PLL." },
  { "en": "Apa itu 'jitter'?", "id": "Fluktuasi acak pada timing sinyal." },
  { "en": "Jitter adalah manifestasi dari?", "id": "Phase noise di domain waktu." },
  { "en": "Jenis 'jitter'?", "id": "Random dan deterministic." },
  { "en": "Apa itu 'random jitter'?", "id": "Jitter tanpa pola yang bisa diprediksi." },
  { "en": "Apa itu 'deterministic jitter'?", "id": "Jitter yang bisa diprediksi polanya." },
  { "en": "Apa itu 'period jitter'?", "id": "Variasi pada durasi satu siklus clock." },
  { "en": "Apa itu 'cycle-to-cycle jitter'?", "id": "Variasi periode antar siklus berurutan." },
  { "en": "Apa itu 'bit-stuffing'?", "id": "Menyisipkan bit untuk sinkronisasi." },
  { "en": "Apa itu 'Manchester encoding'?", "id": "Skema encoding yang menyatukan data dan clock." },
  { "en": "Kelebihan 'Manchester encoding'?", "id": "Tidak ada komponen DC, clock recovery mudah." },
  { "en": "Kelemahan 'Manchester encoding'?", "id": "Membutuhkan bandwidth dua kali lipat." },
  { "en": "Apa itu 'non-return-to-zero' (NRZ)?", "id": "Skema encoding level biner sederhana." },
  { "en": "Singkatan NRZ (Non-Return-to-Zero)?", "id": "Non-Return-to-Zero." },
  { "en": "Apa itu 'non-return-to-zero inverted' (NRZI)?", "id": "Varian dari NRZ." },
  { "en": "Singkatan NRZI (Non-Return-to-Zero Inverted)?", "id": "Non-Return-to-Zero Inverted." },
  { "en": "Apa itu 'resistor-transistor logic' (RTL)?", "id": "Keluarga logika usang." },
  { "en": "Singkatan RTL (Resistor-Transistor Logic)?", "id": "Resistor-Transistor Logic." },
  { "en": "Apa itu 'diode-transistor logic' (DTL)?", "id": "Keluarga logika usang lain." },
  { "en": "Singkatan DTL (Diode-Transistor Logic)?", "id": "Diode-Transistor Logic." },
  { "en": "TTL (Transistor-Transistor Logic) adalah pengembangan dari?", "id": "DTL (Diode-Transistor Logic)." },
  { "en": "Apa itu 'integrated injection logic' (IIL)?", "id": "Keluarga logika BJT kepadatan tinggi." },
  { "en": "Singkatan IIL (Integrated Injection Logic)?", "id": "Integrated Injection Logic." },
  { "en": "Apa itu 'avalanche breakdown'?", "id": "Mekanisme breakdown akibat tumbukan ionisasi." },
  { "en": "Apa itu 'Zener breakdown'?", "id": "Mekanisme breakdown akibat quantum tunneling." },
  { "en": "Breakdown mana yang dominan pada tegangan tinggi?", "id": "Avalanche breakdown." },
  { "en": "Breakdown mana yang dominan pada doping tinggi?", "id": "Zener breakdown." },
  { "en": "Apa itu 'reach-through' atau 'punchthrough'?", "id": "Kondisi di mana daerah deplesi bertemu." },
  { "en": "Apa itu 'kirk effect'?", "id": "Efek pembesaran basis pada BJT." },
  { "en": "Apa itu 'crystal oscillator'?", "id": "Sinonim untuk osilator kristal." },
  { "en": "Apa itu 'relaxation oscillator'?", "id": "Osilator non-linear (contoh: multivibrator astable)." },
  { "en": "Apa itu 'ring oscillator'?", "id": "Osilator dari inverter yang dirangkai loop." },
  { "en": "Aplikasi 'ring oscillator'?", "id": "Pengujian performa proses fabrikasi." },
  { "en": "Apa itu 'voltage-controlled oscillator' (VCO)?", "id": "Sinonim untuk VCO." },
  { "en": "Apa itu 'digitally-controlled oscillator' (DCO)?", "id": "Osilator yang frekuensinya dikontrol digital." },
  { "en": "Singkatan DCO (Digitally-Controlled Oscillator)?", "id": "Digitally-Controlled Oscillator." },
  { "en": "Apa itu 'oven-controlled crystal oscillator' (OCXO)?", "id": "Osilator kristal dengan stabilitas sangat tinggi." },
  { "en": "Singkatan OCXO (Oven-Controlled Crystal Oscillator)?", "id": "Oven-Controlled Crystal Oscillator." },
  { "en": "Cara kerja OCXO (Oven-Controlled Crystal Oscillator)?", "id": "Menjaga suhu kristal tetap konstan." },
  { "en": "Arus saturasi MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) sebanding dengan?", "id": "Kuadrat dari (V_GS - V_T)." },
  { "en": "Kapasitansi deplesi berbanding terbalik dengan?", "id": "Akar kuadrat dari tegangan bias." },
  { "en": "Daya disipasi dinamis CMOS (Complementary Metal-Oxide-Semiconductor) sebanding dengan?", "id": "Frekuensi clock dan kuadrat tegangan." },
  { "en": "Hukum Moore memprediksi peningkatan eksponensial dari?", "id": "Kepadatan transistor." },
  { "en": "Apa itu 'cascode current mirror'?", "id": "Cermin arus dengan impedansi output tinggi." },
  { "en": "Apa itu 'regulated cascode'?", "id": "Cascode dengan umpan balik untuk gain tinggi." },
  { "en": "Apa itu 'chopper amplifier'?", "id": "Amplifier untuk mengurangi offset dan 1/f noise." },
  { "en": "Prinsip dasar 'chopper amplifier'?", "id": "Modulasi-demodulasi sinyal DC/frekuensi rendah." },
  { "en": "Apa itu 'auto-zeroing amplifier'?", "id": "Amplifier yang mengkalibrasi offsetnya sendiri." },
  { "en": "Apa itu 'flying capacitor'?", "id": "Teknik kapasitor untuk transfer muatan." },
  { "en": "Apa itu 'antenna ratio'?", "id": "Rasio area konduktor terhadap area gerbang." },
  { "en": "Tujuan aturan 'antenna ratio'?", "id": "Mencegah kerusakan gerbang saat fabrikasi." },
  { "en": "Apa itu 'electromigration check'?", "id": "Verifikasi keandalan jalur interconnect." },
  { "en": "Apa itu 'power integrity analysis'?", "id": "Menganalisis jaringan distribusi daya (PDN)." },
  { "en": "Apa itu 'pixel' (picture element)?", "id": "Titik terkecil pada layar digital." },
  { "en": "Komponen warna sub-pixel?", "id": "Merah, Hijau, Biru (RGB)." },
  { "en": "Singkatan RGB (Red, Green, Blue)?", "id": "Red, Green, Blue." },
  { "en": "Apa itu 'active-matrix' display?", "id": "Setiap piksel dikontrol transistor sendiri." },
  { "en": "Apa itu 'passive-matrix' display?", "id": "Piksel dialamati berdasarkan baris dan kolom." },
  { "en": "Singkatan OLED (Organic Light-Emitting Diode)?", "id": "Organic Light-Emitting Diode." },
  { "en": "Kelebihan OLED (Organic Light-Emitting Diode) dari LCD (Liquid Crystal Display)?", "id": "Kontras tak hingga, tidak perlu backlight." },
  { "en": "Material aktif pada OLED (Organic Light-Emitting Diode)?", "id": "Senyawa organik." },
  { "en": "Apa itu 'backlight'?", "id": "Sumber cahaya di belakang layar LCD." },
  { "en": "Apa itu 'thin-film transistor' (TFT)?", "id": "Transistor yang digunakan pada layar 'active-matrix'." },
  { "en": "Singkatan TFT (Thin-Film Transistor)?", "id": "Thin-Film Transistor." },
  { "en": "Apa itu 'burn-in' pada layar?", "id": "Degradasi piksel yang tidak seragam." },
  { "en": "Layar yang rentan 'burn-in'?", "id": "OLED dan Plasma." },
  { "en": "Apa itu 'refresh rate'?", "id": "Berapa kali layar diperbarui per detik." },
  { "en": "Satuan 'refresh rate'?", "id": "Hertz (Hz)." },
  { "en": "Apa itu 'response time' piksel?", "id": "Waktu piksel berubah dari satu warna ke warna lain." },
  { "en": "Efek 'response time' yang lambat?", "id": "Motion blur atau 'ghosting'." },
  { "en": "Apa itu 'leakage current'?", "id": "Arus yang mengalir saat transistor mati." },
  { "en": "Jenis-jenis 'leakage current'?", "id": "Subthreshold, gate, junction leakage." },
  { "en": "Apa itu 'static power dissipation'?", "id": "Disipasi daya akibat leakage current." },
  { "en": "Apa itu 'standby power'?", "id": "Daya yang dikonsumsi saat perangkat idle." },
  { "en": "Apa itu 'dark silicon'?", "id": "Bagian chip yang dimatikan untuk hemat daya." },
  { "en": "Apa itu 'race condition' dalam logika?", "id": "Output bergantung pada kecepatan sinyal." },
  { "en": "Apa itu 'hazard' dalam logika digital?", "id": "Potensi glitch akibat delay." },
  { "en": "Jenis 'hazard'?", "id": "Statis dan dinamis." },
  { "en": "Apa itu 'glitch'?", "id": "Transisi sinyal sesaat yang salah." },
  { "en": "Apa itu 'asynchronous circuit'?", "id": "Sirkuit digital tanpa clock global." },
  { "en": "Keuntungan sirkuit asinkron?", "id": "Konsumsi daya rendah, tidak ada clock skew." },
  { "en": "Kelemahan sirkuit asinkron?", "id": "Desain yang lebih sulit." },
  { "en": "Apa itu 'handshaking'?", "id": "Protokol komunikasi antar modul asinkron." },
  { "en": "Apa itu 'arbiter'?", "id": "Sirkuit untuk menyelesaikan permintaan akses bersamaan." },
  { "en": "Apa itu 'metastability hardener'?", "id": "Sirkuit untuk mengurangi kemungkinan metastabilitas." },
  { "en": "Contoh 'metastability hardener'?", "id": "Sinkronisasi dua flip-flop." },
  { "en": "Apa itu 'logic synthesis'?", "id": "Sinonim untuk sintesis logika." },
  { "en": "Apa itu 'technology mapping'?", "id": "Tahap sintesis memetakan ke sel standar." },
  { "en": "Apa itu 'boolean satisfiability' (SAT)?", "id": "Masalah penentuan pemenuhan formula boolean." },
  { "en": "Singkatan SAT (Boolean Satisfiability)?", "id": "Boolean Satisfiability." },
  { "en": "Aplikasi SAT (Boolean Satisfiability) dalam EDA (Electronic Design Automation)?", "id": "Verifikasi formal dan testing." },
  { "en": "Apa itu 'binary decision diagram' (BDD)?", "id": "Representasi grafis dari fungsi boolean." },
  { "en": "Singkatan BDD (Binary Decision Diagram)?", "id": "Binary Decision Diagram." },
  { "en": "Apa itu 'noise floor'?", "id": "Level noise dasar dari sebuah sistem." },
  { "en": "Apa itu 'dynamic range'?", "id": "Rasio antara sinyal terkuat dan terlemah." },
  { "en": "Apa itu 'gain flatness'?", "id": "Variasi gain pada rentang frekuensi." },
  { "en": "Apa itu 'group delay variation'?", "id": "Variasi group delay pada rentang frekuensi." },
  { "en": "Penyebab 'group delay variation'?", "id": "Respon fasa yang non-linear." },
  { "en": "Apa itu 'return loss'?", "id": "Ukuran seberapa baik impedansi cocok." },
  { "en": "Return loss tinggi berarti?", "id": "Impedansi cocok dengan baik." },
  { "en": "Apa itu 'insertion loss'?", "id": "Kehilangan sinyal saat komponen disisipkan." },
  { "en": "Apa itu 'isolation'?", "id": "Ukuran kebocoran sinyal antar port." },
  { "en": "Apa itu 'orthogonality' dalam RF?", "id": "Sinyal yang tidak saling mengganggu." },
  { "en": "Contoh sinyal ortogonal?", "id": "Sinus dan kosinus pada frekuensi sama." },
  { "en": "Apa itu 'quadrature signal'?", "id": "Sinyal In-phase (I) dan Quadrature (Q)." },
  { "en": "Beda fasa antara sinyal I dan Q?", "id": "90 derajat." },
  { "en": "Apa itu 'balun'?", "id": "Sirkuit pengubah balanced ke unbalanced." },
  { "en": "Fungsi 'balun'?", "id": "Menghubungkan antena dipol ke kabel koaksial." },
  { "en": "Apa itu 'balanced signal'?", "id": "Sinyal yang dibawa pada dua konduktor." },
  { "en": "Keuntungan 'balanced signal'?", "id": "Tahan terhadap noise common-mode." },
  { "en": "Apa itu 'unbalanced signal'?", "id": "Sinyal dengan satu konduktor dan ground." },
  { "en": "Apa itu 'differential signaling'?", "id": "Menggunakan sinyal balanced untuk transmisi." },
  { "en": "Aplikasi 'differential signaling'?", "id": "USB, Ethernet, HDMI." },
  { "en": "Singkatan USB (Universal Serial Bus)?", "id": "Universal Serial Bus." },
  { "en": "Singkatan HDMI (High-Definition Multimedia Interface)?", "id": "High-Definition Multimedia Interface." },
  { "en": "Apa itu 'common-mode choke'?", "id": "Induktor untuk menekan noise common-mode." },
  { "en": "Apa itu 'Faraday cage'?", "id": "Selungkup untuk memblokir medan elektromagnetik." },
  { "en": "Apa itu 'electromagnetic interference' (EMI)?", "id": "Gangguan akibat radiasi elektromagnetik." },
  { "en": "Singkatan EMI (Electromagnetic Interference)?", "id": "Electromagnetic Interference." },
  { "en": "Apa itu 'electromagnetic compatibility' (EMC)?", "id": "Kemampuan perangkat berfungsi tanpa EMI." },
  { "en": "Singkatan EMC (Electromagnetic Compatibility)?", "id": "Electromagnetic Compatibility." },
  { "en": "Apa itu 'shielding'?", "id": "Menggunakan lapisan konduktif untuk memblokir EMI." },
  { "en": "Apa itu 'grounding'?", "id": "Menyediakan jalur referensi tegangan nol." },
  { "en": "Apa itu 'thermal shutdown'?", "id": "Fitur proteksi yang mematikan chip saat panas." },
  { "en": "Apa itu 'undervoltage lockout' (UVLO)?", "id": "Mematikan sirkuit saat tegangan suplai rendah." },
  { "en": "Singkatan UVLO (Undervoltage Lockout)?", "id": "Undervoltage Lockout." },
  { "en": "Apa itu 'soft start'?", "id": "Fitur untuk menaikkan daya secara perlahan." },
  { "en": "Tujuan 'soft start'?", "id": "Membatasi 'inrush current'." },
  { "en": "Apa itu 'inrush current'?", "id": "Arus masuk yang tinggi saat power-on." },
  { "en": "Apa itu 'hot swapping'?", "id": "Mencabut/memasang komponen tanpa mematikan daya." },
  { "en": "Apa itu 'cache coherence'?", "id": "Menjaga konsistensi data di beberapa cache." },
  { "en": "Apa itu 'pipeline hazard'?", "id": "Konflik dalam eksekusi instruksi pipeline." },
  { "en": "Sebutkan jenis 'pipeline hazard'.", "id": "Struktural, data, dan kontrol." },
  { "en": "Apa itu 'data hazard'?", "id": "Instruksi bergantung pada hasil instruksi sebelumnya." },
  { "en": "Solusi untuk 'data hazard'?", "id": "Forwarding atau stalling." },
  { "en": "Apa itu 'control hazard' (branch hazard)?", "id": "Ketidakpastian pada instruksi percabangan." },
  { "en": "Solusi untuk 'control hazard'?", "id": "Branch prediction." },
  { "en": "Apa itu 'branch prediction'?", "id": "Teknik menebak hasil percabangan." },
  { "en": "Apa itu 'speculative execution'?", "id": "Eksekusi instruksi sebelum percabangan selesai." },
  { "en": "Apa itu 'out-of-order execution'?", "id": "Eksekusi instruksi tidak sesuai urutan program." },
  { "en": "Apa itu 'superscalar processor'?", "id": "Prosesor dengan beberapa pipeline eksekusi." },
  { "en": "Apa itu 'chopper stabilization'?", "id": "Teknik mengurangi offset dan drift op-amp." },
  { "en": "Apa itu 'correlated double sampling' (CDS)?", "id": "Teknik mengurangi noise pada sensor gambar." },
  { "en": "Singkatan CDS (Correlated Double Sampling)?", "id": "Correlated Double Sampling." },
  { "en": "Apa itu 'decapsulation'?", "id": "Proses membuka kemasan IC untuk analisis." },
  { "en": "Tujuan 'decapsulation'?", "id": "Analisis kegagalan (failure analysis)." },
  { "en": "Apa itu 'Focused Ion Beam' (FIB)?", "id": "Alat untuk memotong dan mengedit sirkuit." },
  { "en": "Singkatan FIB (Focused Ion Beam)?", "id": "Focused Ion Beam." },
  { "en": "Apa itu 'Scanning Electron Microscope' (SEM)?", "id": "Mikroskop untuk melihat topografi permukaan." },
  { "en": "Singkatan SEM (Scanning Electron Microscope)?", "id": "Scanning Electron Microscope." },
  { "en": "Apa itu 'Transmission Electron Microscope' (TEM)?", "id": "Mikroskop untuk melihat struktur internal." },
  { "en": "Singkatan TEM (Transmission Electron Microscope)?", "id": "Transmission Electron Microscope." },
  { "en": "Apa itu 'Atomic Force Microscope' (AFM)?", "id": "Mikroskop resolusi sangat tinggi." },
  { "en": "Singkatan AFM (Atomic Force Microscope)?", "id": "Atomic Force Microscope." },
  { "en": "Apa itu 'X-ray lithography'?", "id": "Litografi menggunakan sinar-X." },
  { "en": "Apa itu 'electron-beam lithography'?", "id": "Litografi tanpa masker menggunakan e-beam." },
  { "en": "Kelebihan 'electron-beam lithography'?", "id": "Resolusi sangat tinggi." },
  { "en": "Kelemahan 'electron-beam lithography'?", "id": "Throughput sangat rendah (lambat)." },
  { "en": "Organisasi standardisasi semikonduktor?", "id": "JEDEC." },
  { "en": "Singkatan JEDEC (Joint Electron Device Engineering Council)?", "id": "Joint Electron Device Engineering Council." },
  { "en": "JEDEC (Joint Electron Device Engineering Council) menetapkan standar untuk?", "id": "Memori (DDR), kemasan, ESD." },
  { "en": "Apa itu 'channel hot electron' (CHE) injection?", "id": "Mekanisme penulisan pada memori Flash." },
  { "en": "Singkatan CHE (Channel Hot Electron)?", "id": "Channel Hot Electron." },
  { "en": "Apa itu 'Fowler-Nordheim tunneling'?", "id": "Mekanisme lain untuk penulisan/penghapusan." },
  { "en": "Apa itu 'endurance' pada memori Flash?", "id": "Jumlah siklus tulis/hapus maksimum." },
  { "en": "Apa itu 'data retention'?", "id": "Kemampuan memori menyimpan data dari waktu." },
  { "en": "Apa itu 'single-level cell' (SLC) Flash?", "id": "Menyimpan satu bit per sel." },
  { "en": "Singkatan SLC (Single-Level Cell)?", "id": "Single-Level Cell." },
  { "en": "Apa itu 'multi-level cell' (MLC) Flash?", "id": "Menyimpan dua bit per sel." },
  { "en": "Singkatan MLC (Multi-Level Cell)?", "id": "Multi-Level Cell." },
  { "en": "Apa itu 'triple-level cell' (TLC) Flash?", "id": "Menyimpan tiga bit per sel." },
  { "en": "Singkatan TLC (Triple-Level Cell)?", "id": "Triple-Level Cell." },
  { "en": "Trade-off SLC/MLC/TLC?", "id": "Kecepatan dan ketahanan vs kepadatan." },
  { "en": "Mana yang paling awet, SLC, MLC, atau TLC?", "id": "SLC." },
  { "en": "Mana yang paling padat/murah, SLC, MLC, atau TLC?", "id": "TLC." },
  { "en": "Apa itu 'Gilbert cell mixer'?", "id": "Sinonim untuk Gilbert cell." },
  { "en": "Apa itu 'cascode amplifier'?", "id": "Sinonim untuk penguat cascode." },
  { "en": "Apa itu 'Darlington transistor'?", "id": "Sinonim untuk Darlington pair." },
  { "en": "Apa itu 'push-pull amplifier'?", "id": "Topologi output pada amplifier Class B/AB." },
  { "en": "Apa itu 'long-tailed pair'?", "id": "Nama lain untuk tahap input penguat diferensial." },
  { "en": "Apa itu 'constant-gm biasing'?", "id": "Sirkuit bias yang menjaga transkonduktansi konstan." },
  { "en": "Apa itu 'current source'?", "id": "Sirkuit yang menyediakan arus konstan." },
  { "en": "Apa itu 'current sink'?", "id": "Sirkuit yang menyerap arus konstan." },
  { "en": "Apa itu 'level shifter'?", "id": "Sirkuit untuk mengubah level tegangan logika." },
  { "en": "Apa itu 'bootstrap' circuit?", "id": "Sirkuit yang menggunakan outputnya untuk meningkatkan performa." },
  { "en": "Apa itu 'charge sharing'?", "id": "Masalah pada logika dinamik." },
  { "en": "Apa itu 'keeper' transistor?", "id": "Transistor lemah untuk menjaga state." },
  { "en": "Aplikasi 'keeper' transistor?", "id": "Logika dinamik dan sel memori SRAM." },
  { "en": "Apa itu 'sense amplifier'?", "id": "Penguat kecil untuk membaca bit memori." },
  { "en": "Mengapa DRAM (Dynamic Random-Access Memory) butuh 'sense amplifier'?", "id": "Karena sinyal dari kapasitor sangat kecil." },
  { "en": "Apa itu 'precharge'?", "id": "Tahap dalam logika dinamik atau pembacaan DRAM." },
  { "en": "Apa itu 'write driver'?", "id": "Sirkuit untuk menulis data ke sel memori." },
  { "en": "Apa itu 'column decoder'?", "id": "Memilih kolom bit line pada array memori." },
  { "en": "Apa itu 'row decoder'?", "id": "Memilih baris word line pada array memori." },
  { "en": "Apa itu 'redundancy' pada chip memori?", "id": "Baris/kolom cadangan untuk perbaikan." },
  { "en": "Bagaimana 'redundancy' diaktifkan?", "id": "Dengan memotong 'fuse' menggunakan laser." },
  { "en": "Apa itu 'direct memory access' (DMA)?", "id": "Transfer data tanpa melibatkan CPU." },
  { "en": "Singkatan DMA (Direct Memory Access)?", "id": "Direct Memory Access." },
  { "en": "Apa itu 'interrupt'?", "id": "Sinyal yang meminta perhatian prosesor." },
  { "en": "Apa itu 'interrupt service routine' (ISR)?", "id": "Kode yang dijalankan saat interrupt terjadi." },
  { "en": "Singkatan ISR (Interrupt Service Routine)?", "id": "Interrupt Service Routine." },
  { "en": "Apa itu 'polling'?", "id": "Proses memeriksa status perangkat secara berulang." },
  { "en": "Apa itu 'watchdog timer'?", "id": "Timer untuk mereset sistem jika hang." },
  { "en": "Apa itu 'brown-out detection'?", "id": "Mendeteksi jika tegangan catu daya turun." },
  { "en": "Apa itu 'instruction set architecture' (ISA)?", "id": "Bagian dari arsitektur komputer." },
  { "en": "Singkatan ISA (Instruction Set Architecture)?", "id": "Instruction Set Architecture." },
  { "en": "Contoh ISA (Instruction Set Architecture)?", "id": "x86, ARM, RISC-V." },
  { "en": "Apa itu 'Reduced Instruction Set Computer' (RISC)?", "id": "Strategi desain ISA." },
  { "en": "Singkatan RISC (Reduced Instruction Set Computer)?", "id": "Reduced Instruction Set Computer." },
  { "en": "Apa itu 'Complex Instruction Set Computer' (CISC)?", "id": "Strategi desain ISA lain." },
  { "en": "Singkatan CISC (Complex Instruction Set Computer)?", "id": "Complex Instruction Set Computer." },
  { "en": "Intel x86 adalah contoh arsitektur?", "id": "CISC." },
  { "en": "ARM adalah contoh arsitektur?", "id": "RISC." },
  { "en": "Apa itu 'microcode'?", "id": "Layer interpretasi instruksi dalam CPU CISC." },
  { "en": "Apa itu 'firmware'?", "id": "Software permanen yang disimpan di ROM." },
  { "en": "Contoh 'firmware'?", "id": "BIOS atau UEFI pada komputer." },
  { "en": "Singkatan BIOS (Basic Input/Output System)?", "id": "Basic Input/Output System." },
  { "en": "Singkatan UEFI (Unified Extensible Firmware Interface)?", "id": "Unified Extensible Firmware Interface." },
  { "en": "Apa itu 'volatile memory'?", "id": "Memori yang butuh daya untuk simpan data." },
  { "en": "Contoh 'volatile memory'?", "id": "SRAM, DRAM." },
  { "en": "Apa itu 'non-volatile memory' (NVM)?", "id": "Memori yang tetap simpan data tanpa daya." },
  { "en": "Singkatan NVM (Non-Volatile Memory)?", "id": "Non-Volatile Memory." },
  { "en": "Contoh 'non-volatile memory' (NVM)?", "id": "Flash, ROM, MRAM." },
  { "en": "Apa itu 'Dennard scaling'?", "id": "Aturan scaling transistor MOSFET." },
  { "en": "Apa itu 'Koomey's law'?", "id": "Observasi tentang efisiensi energi komputasi." },
  { "en": "Perangkat yang perilakunya dikontrol arus?", "id": "BJT (Bipolar Junction Transistor)." },
  { "en": "Perangkat yang perilakunya dikontrol tegangan?", "id": "FET (Field-Effect Transistor)." },
  { "en": "Proses menumbuhkan lapisan oksida?", "id": "Oksidasi." },
  { "en": "Proses menghilangkan material secara selektif?", "id": "Etsa (etching)." },
  { "en": "Apa itu 'channel' pada FET (Field-Effect Transistor)?", "id": "Daerah konduksi antara source dan drain." },
  { "en": "Apa itu 'base' pada BJT (Bipolar Junction Transistor)?", "id": "Terminal kontrol arus." },
  { "en": "Apa itu 'gate' pada FET (Field-Effect Transistor)?", "id": "Terminal kontrol tegangan." },
  { "en": "Apa itu 'polysilicon'?", "id": "Sinonim untuk 'polycrystalline silicon'." },
  { "en": "Apa itu 'silicon dioxide' (SiO2)?", "id": "Material isolator gerbang yang umum." },
  { "en": "Apa itu 'silicon nitride' (Si3N4)?", "id": "Material untuk isolasi dan 'passivation'." },
  { "en": "Material 'interconnect' sebelum tembaga?", "id": "Aluminium (Al)." },
  { "en": "Apa itu 'silicide' (salicide)?", "id": "Senyawa silikon dan logam." },
  { "en": "Tujuan pembentukan 'silicide'?", "id": "Mengurangi resistansi kontak dan gerbang." },
  { "en": "Apa itu 'cascode' BJT (Bipolar Junction Transistor)?", "id": "Konfigurasi common-emitter diikuti common-base." },
  { "en": "Apa itu 'cascode' FET (Field-Effect Transistor)?", "id": "Konfigurasi common-source diikuti common-gate." },
  { "en": "Apa itu 'common-mode voltage'?", "id": "Tegangan rata-rata pada input diferensial." },
  { "en": "Apa itu 'virtual short' pada op-amp?", "id": "Tegangan input diferensial mendekati nol." },
  { "en": "Prinsip 'virtual short' berlaku saat?", "id": "Umpan balik negatif digunakan." },
  { "en": "Apa itu 'pipeline flushing'?", "id": "Mengosongkan pipeline akibat salah prediksi." },
  { "en": "Apa itu 'cache miss'?", "id": "Data yang diminta tidak ditemukan di cache." },
  { "en": "Apa itu 'cache hit'?", "id": "Data yang diminta ditemukan di cache." },
  { "en": "Apa itu 'cache line' (atau block)?", "id": "Unit transfer data terkecil cache." },
  { "en": "Apa itu 'write-through cache'?", "id": "Menulis ke cache dan memori utama sekaligus." },
  { "en": "Apa itu 'write-back cache'?", "id": "Menulis ke memori saat data diusir." },
  { "en": "Apa itu 'cache eviction policy'?", "id": "Aturan untuk mengusir data dari cache." },
  { "en": "Contoh 'eviction policy'?", "id": "LRU (Least Recently Used)." },
  { "en": "Singkatan LRU (Least Recently Used)?", "id": "Least Recently Used." },
  { "en": "Apa itu 'thermal design power' (TDP)?", "id": "Daya panas maksimum yang dihasilkan chip." },
  { "en": "Singkatan TDP (Thermal Design Power)?", "id": "Thermal Design Power." },
  { "en": "Apa itu 'wafer bumping'?", "id": "Sinonim untuk proses wafer bumping." },
  { "en": "Apa itu 'die preparation'?", "id": "Persiapan die setelah dicing." },
  { "en": "Apa itu 'die attach'?", "id": "Proses menempelkan die ke substrat." },
  { "en": "Apa itu 'epoxy'?", "id": "Bahan perekat untuk 'die attach'." },
  { "en": "Apa itu 'molding'?", "id": "Proses membungkus chip dengan plastik/keramik." },
  { "en": "Apa itu 'trimming'?", "id": "Menyesuaikan nilai komponen (misal: resistor)." },
  { "en": "Metode 'trimming'?", "id": "Laser trimming atau fuse trimming." },
  { "en": "Apa itu 'Schottky effect'?", "id": "Penurunan 'work function' akibat medan listrik." },
  { "en": "Dioda Schottky bekerja berdasarkan?", "id": "Sambungan logam-semikonduktor." },
  { "en": "Apa itu 'Ohmic region'?", "id": "Sinonim untuk daerah triode MOSFET." },
  { "en": "Apa itu 'pinch-off voltage'?", "id": "Tegangan drain-source awal saturasi FET." },
  { "en": "Apa itu 'leakage power'?", "id": "Sinonim untuk 'static power dissipation'." },
  { "en": "Apa itu 'switching power'?", "id": "Sinonim untuk 'dynamic power dissipation'." },
  { "en": "Komponen yang berperilaku seperti saklar terkontrol?", "id": "Transistor." },
  { "en": "Komponen yang menyimpan energi di medan listrik?", "id": "Kapasitor." },
  { "en": "Komponen yang menyimpan energi di medan magnet?", "id": "Induktor." },
  { "en": "Komponen yang mendisipasi energi?", "id": "Resistor." },
  { "en": "Hukum yang menghubungkan tegangan, arus, resistansi?", "id": "Hukum Ohm." },
  { "en": "Hukum Kirchhoff tentang arus?", "id": "Jumlah arus masuk = jumlah arus keluar." },
  { "en": "Hukum Kirchhoff tentang tegangan?", "id": "Jumlah tegangan dalam satu loop = nol." },
  { "en": "Apa itu 'superposition theorem'?", "id": "Teorema untuk analisis sirkuit linear." },
  { "en": "Apa itu 'Thevenin's theorem'?", "id": "Menyederhanakan sirkuit menjadi sumber tegangan." },
  { "en": "Apa itu 'Norton's theorem'?", "id": "Menyederhanakan sirkuit menjadi sumber arus." },
  { "en": "Ekuivalen Thevenin terdiri dari?", "id": "Sumber tegangan dan resistor seri." },
  { "en": "Ekuivalen Norton terdiri dari?", "id": "Sumber arus dan resistor paralel." },
  { "en": "Apa itu 'maximum power transfer theorem'?", "id": "Teorema transfer daya maksimum." },
  { "en": "Syarat transfer daya maksimum?", "id": "Impedansi beban = konjugat impedansi sumber." },
  { "en": "Apa itu 'passivity'?", "id": "Sifat sirkuit yang tidak menghasilkan energi." },
  { "en": "Apa itu 'reciprocity'?", "id": "Sifat sirkuit dua port." },
  { "en": "Apa itu 'duality' dalam sirkuit?", "id": "Hubungan dual antara R-G, L-C, V-I." },
  { "en": "Apa itu 'SPICE deck' atau 'netlist'?", "id": "File input untuk simulator SPICE." },
  { "en": "Analisis '.OP' pada SPICE?", "id": "Analisis titik operasi DC." },
  { "en": "Analisis '.DC' pada SPICE?", "id": "Analisis sapuan tegangan/arus DC." },
  { "en": "Analisis '.AC' pada SPICE?", "id": "Analisis respon frekuensi small-signal." },
  { "en": "Analisis '.TRAN' pada SPICE?", "id": "Analisis transien di domain waktu." },
  { "en": "Apa itu 'convergence problem' dalam SPICE?", "id": "Simulator gagal menemukan solusi." },
  { "en": "Penyebab 'convergence problem'?", "id": "Non-linearitas tinggi atau model buruk." },
  { "en": "Apa itu 'electromotive force' (EMF)?", "id": "Gaya gerak listrik (GGL)." },
  { "en": "Singkatan EMF (Electromotive Force)?", "id": "Electromotive Force." },
  { "en": "Satuan EMF (Electromotive Force)?", "id": "Volt." },
  { "en": "Apa itu 'permittivity'?", "id": "Ukuran kemampuan material membentuk medan listrik." },
  { "en": "Apa itu 'permeability'?", "id": "Ukuran kemampuan material membentuk medan magnet." },
  { "en": "Material dengan permeabilitas sangat tinggi?", "id": "Material feromagnetik." },
  { "en": "Apa itu 'hysteresis' magnetik?", "id": "Ketergantungan magnetisasi pada sejarah medan." },
  { "en": "Apa itu 'retentivity'?", "id": "Sisa magnetisasi setelah medan dihilangkan." },
  { "en": "Apa itu 'coercivity'?", "id": "Kekuatan medan untuk menghilangkan magnetisasi." },
  { "en": "Apa itu 'doping profile'?", "id": "Distribusi konsentrasi dopan terhadap kedalaman." },
  { "en": "Jenis 'doping profile'?", "id": "Uniform dan non-uniform (misal: Gaussian)." },
  { "en": "Apa itu 'ion channeling'?", "id": "Masalah pada proses implantasi ion." },
  { "en": "Apa itu 'rapid thermal annealing' (RTA)?", "id": "Proses annealing yang sangat cepat." },
  { "en": "Singkatan RTA (Rapid Thermal Annealing)?", "id": "Rapid Thermal Annealing." },
  { "en": "Apa itu 'gettering'?", "id": "Proses menghilangkan pengotor tak diinginkan." },
  { "en": "Apa itu 'class' cleanroom (misal: Class 10)?", "id": "Jumlah partikel maksimum per kaki kubik." },
  { "en": "Semakin rendah 'class' cleanroom?", "id": "Semakin bersih ruangannya." },
  { "en": "Sumber utama kontaminasi di cleanroom?", "id": "Manusia." },
  { "en": "Pakaian yang dipakai di cleanroom?", "id": "Bunny suit." },
  { "en": "Aliran udara di cleanroom?", "id": "Aliran laminar." },
  { "en": "Apa itu 'high-efficiency particulate air' (HEPA) filter?", "id": "Filter udara yang sangat efisien." },
  { "en": "Singkatan HEPA (High-Efficiency Particulate Air)?", "id": "High-Efficiency Particulate Air." },
  { "en": "Apa itu 'ultra-pure water' (UPW)?", "id": "Air yang sangat murni untuk fabrikasi." },
  { "en": "Singkatan UPW (Ultra-Pure Water)?", "id": "Ultra-Pure Water." },
  { "en": "Apa itu 'slurry' dalam proses CMP?", "id": "Cairan abrasif untuk pemolesan." },
  { "en": "Apa itu 'aspect ratio' dalam etsa?", "id": "Rasio kedalaman terhadap lebar." },
  { "en": "Tantangan etsa 'high aspect ratio'?", "id": "Sulit dan lambat." }



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
