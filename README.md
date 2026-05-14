# Panen yang Hilang: Panduan Riset Data & Analitik ENSO–Ketahanan Pangan 38 Provinsi Indonesia
**Untuk: Kompetisi Infografis Statistik (SIC) Tingkat Nasional**
*Sub-tema: Sustainability Through Data — Tackling Climate Change and Environmental Challenges*

***

## Executive Summary

Anomali iklim ENSO (El Niño/La Niña) adalah faktor paling dominan yang mendorong variabilitas produksi padi tahunan di Indonesia. Bukti empiris menunjukkan bahwa setiap kenaikan 1°C pada indeks Niño 3.4 (Agustus) mengakibatkan penurunan produksi gabah nasional rata-rata sebesar **1,4 juta ton**, dan pada tahun El Niño ekstrem, defisit produksi bisa mencapai **10% dari total produksi tahunan**. Dampak ini tidak merata — 38 provinsi menunjukkan pola kerentanan yang sangat berbeda berdasarkan konfigurasi ekosistem sawah (irigasi vs. tadah hujan), topografi, dan kapasitas infrastruktur.[^1]

Laporan ini menyajikan ekosistem data lengkap, metode statistik, insight non-obvious, dan pipeline analisis optimal untuk kompetisi 20 hari.

***

## Bagian 1: Katalog Dataset Resmi & Terpercaya

### 1.1 Dataset ENSO / Iklim Global

| # | Nama Dataset | Institusi | Cakupan Tahun | Variabel Kunci | Resolusi Geo | Format | Link Akses | Kredibilitas | Potensi Storytelling |
|---|---|---|---|---|---|---|---|---|---|
| 1 | **Niño 3.4 SST Index (ERSST V5)** | NOAA/CPC | 1950–kini | SST anomaly °C (bulanan) | Regional (5N-5S, 170-120W) | TXT/CSV | https://psl.noaa.gov/data/timeseries/month/Nino34_CPC/ | ★★★★★ | Backbone time-series utama; gunakan sebagai variabel X dalam semua regresi |
| 2 | **Niño 3.4 SST Index (HadISST1.1)** | NOAA/PSL | 1870–kini | SST anomaly °C (bulanan) | Regional | TXT/CSV | https://psl.noaa.gov/data/timeseries/month/Nino34/ | ★★★★★ | Seri lebih panjang; validasi lintas sumber |
| 3 | **Oceanic Niño Index (ONI)** | NOAA/CPC | 1950–kini | 3-bulan running avg SST anomaly | Regional | CSV (wide & long) | https://data.amerigeoss.org/dataset/monthly-oceanic-nino-index-oni | ★★★★★ | Standard resmi NOAA; threshold ±0.5°C untuk klasifikasi El Niño/La Niña[^2] |
| 4 | **ENSO Datasets Hub** | NOAA/PSL | 1870–kini | SOI, MEI, Niño 3, 3.4, 4 | Regional | Multi-format | https://psl.noaa.gov/enso/data.html | ★★★★★ | Multi-indeks untuk robustness check |
| 5 | **Nino 3.4 SST (Copernicus/Reanalysis)** | Mercator Ocean / EU | 1993–2023 | SST anomaly, threshold event | Regional | NetCDF-4 | https://data.marine.copernicus.eu/product/GLOBAL_OMI_CLIMVAR_enso_sst_area_averaged_anomalies | ★★★★☆ | Cocok untuk peta animasi periode 1993–2023[^3] |

### 1.2 Dataset Curah Hujan & Kekeringan Indonesia

| # | Nama Dataset | Institusi | Cakupan Tahun | Variabel Kunci | Resolusi Geo | Format | Link Akses | Kredibilitas | Potensi Storytelling |
|---|---|---|---|---|---|---|---|---|---|
| 6 | **Data Iklim Harian/Bulanan BMKG** | BMKG Indonesia | ~1980–kini | Curah hujan, suhu, kelembaban, kecepatan angin per stasiun | Stasiun (ratusan titik) | XLS/PDF | https://dataonline.bmkg.go.id/ | ★★★★★ | Data observasi resmi; butuh registrasi akun gratis[^4] |
| 7 | **Data Terbuka BMKG** | BMKG Indonesia | Variasi | Prakiraan cuaca, anomali curah hujan | Nasional–Provinsi | API/Open Data | https://data.bmkg.go.id | ★★★★★ | Akses terbuka tanpa login[^5] |
| 8 | **CHIRPS v2/v3 (Rainfall Estimates)** | UCSB/CHG | 1981–kini | Curah hujan harian/bulanan gridded | 0.05° (~5.5 km) | GeoTIFF/CSV via GEE | https://www.chc.ucsb.edu/data/chirps | ★★★★★ | Terbaik untuk spatial coverage seluruh Indonesia; accessible via Google Earth Engine[^6] |
| 9 | **CHIRPS Daily (Google Earth Engine)** | UCSB/CHG via Google | 1981–2026 | Presipitasi harian | 0.05° | GEE export to CSV | https://developers.google.com/earth-engine/datasets/catalog/UCSB-CHG_CHIRPS_DAILY | ★★★★★ | Download per provinsi sangat feasible dalam 20 hari[^7] |
| 10 | **Palmer Drought Severity Index (PDSI) Gridded** | NOAA/PSL | 1850–2014 | PDSI bulanan global | Gridded ~2.5° | NetCDF | https://psl.noaa.gov/data/gridded/data.pdsi.html | ★★★★☆ | Identifikasi drought events historis per wilayah[^8] |
| 11 | **PDSI Southeast Asia (Mendeley)** | Peneliti / Mendeley | 1980–2019 | PDSI bulanan per negara + gridded 10km | Nasional–Gridded 10km | CSV + Shapefile | https://data.mendeley.com/datasets/nv7jbmkynm/1 | ★★★★☆ | Spesifik impact on croplands SEA; ready-to-use untuk Indonesia[^9] |

### 1.3 Dataset Produksi Padi Indonesia

| # | Nama Dataset | Institusi | Cakupan Tahun | Variabel Kunci | Resolusi Geo | Format | Link Akses | Kredibilitas | Potensi Storytelling |
|---|---|---|---|---|---|---|---|---|---|
| 12 | **Luas Panen, Produksi & Produktivitas Padi per Provinsi** | BPS Indonesia | 1993–2024 | Luas panen (ha), produksi (ton), produktivitas (kw/ha) | Provinsi | XLS/Web table | https://www.bps.go.id/id/statistics-table/2/MTQ5OCMy/ | ★★★★★ | **Dataset utama kompetisi**; time-series 30+ tahun per 38 provinsi[^10][^11] |
| 13 | **Luas Panen, Produktivitas & Produksi Padi (data.go.id)** | BPS Aceh / Satu Data | Variasi per dataset | Luas panen (ha), produksi (ton), produktivitas per kab/kota | Kabupaten/Kota | CSV | https://data.go.id/dataset/dataset/luas-panen-produktivitas-dan-produksi-padi-bps | ★★★★★ | Resolusi sub-provinsi; berguna untuk drill-down[^12] |
| 14 | **FAOSTAT — Rice Production Indonesia** | FAO | 1961–2023 | Area harvested, yield, production value | Nasional | CSV/XLSX/API | https://www.fao.org/faostat/ | ★★★★★ | Cross-validation dengan BPS; data internasional standardized[^13] |
| 15 | **Agricultural Production Statistics 2010–2023** | FAO | 2010–2023 | Produksi padi per negara, komparasi global | Nasional | Report + Data | https://www.fao.org/statistics/highlights-archive | ★★★★★ | Kontekstualisasi posisi Indonesia secara global[^14] |
| 16 | **Rice Production Indonesia (UNdata/FAO)** | UN/FAO | Multi-dekade | Area, yield, production | Nasional | XML/CSV | https://data.un.org/Data.aspx?d=FAO&f=itemCode%3A27 | ★★★★☆ | Alternatif akses FAOSTAT via UN portal[^15] |

### 1.4 Dataset Ketahanan Pangan & Kerentanan Provinsi

| # | Nama Dataset | Institusi | Cakupan Tahun | Variabel Kunci | Resolusi Geo | Format | Link Akses | Kredibilitas | Potensi Storytelling |
|---|---|---|---|---|---|---|---|---|---|
| 17 | **Indeks Ketahanan Pangan (IKP) Provinsi 2023** | Badan Pangan Nasional | 2019–2023 | Skor IKP, ranking, kelompok kerentanan (1–6) | Provinsi | PDF + Dataset | https://data.go.id/dataset/dataset/indeks-ketahanan-pangan-ikp-provinsi-update-tahun-2024 | ★★★★★ | **Kunci untuk peta kerentanan**; 38 provinsi dengan skor, ranking & kategori[^16][^17] |
| 18 | **IKP Kabupaten/Kota 2023** | Badan Pangan Nasional | 2022–2023 | 9 indikator: kemiskinan, air bersih, stunting, harapan hidup, dll. | Kabupaten/Kota | PDF + XLS | https://data.bnpb.go.id/dataset/indeks-ketahanan-pangan-ikp-tahun-2022 | ★★★★★ | Sub-provincial detail; 21 kab Prioritas 1 di Papua[^18][^19] |
| 19 | **Food Security & Vulnerability Atlas (FSVA) 2024** | Bapanas + WFP | 2017–2024 | 9 indikator ketahanan pangan spatial | Kecamatan/Desa | Spatial + Tabular | https://fsva.badanpangan.go.id | ★★★★★ | Interactive atlas; 514 kab/kota; 6 kelas prioritas kerentanan[^20] |
| 20 | **Indikator Capaian Kinerja Urusan Pangan 2019–2023** | Dinas Ketahanan Pangan | 2019–2023 | Ketersediaan pangan utama, capaian tahunan per kab/kota | Kabupaten | XLSX | https://data.go.id/dataset/dataset/indikator-capaian-kinerja-urusan-pangan-tahun-2019-2023 | ★★★★☆ | Tracking performa ketahanan pangan daerah selama 5 tahun[^21] |
| 21 | **World Bank — Undernourishment Indonesia** | World Bank / FAO | 1991–2023 | % penduduk kurang gizi | Nasional | CSV/API | https://data.worldbank.org/indicator/SN.ITK.DEFC.ZS?locations=ID | ★★★★★ | Trend jangka panjang; kontekstualisasi dampak ENSO pada malnutrisi[^22] |
| 22 | **Global Hunger Index — Indonesia** | GHI | 1990–2025 | Skor kelaparan, undernutrition, child wasting, stunting | Nasional | Web/Report | https://www.globalhungerindex.org/indonesia.html | ★★★★★ | Indonesia skor 14.6 (2025), rank 70/123; konteks global[^23] |

***

## Bagian 2: Ringkasan Struktur Dataset untuk Pipeline

| Dataset | Fungsi dalam Analisis | Prioritas 20 Hari |
|---|---|---|
| NOAA ONI (CSV) | Variabel X ENSO; klasifikasi El Niño/La Niña per bulan/tahun | ★★★★★ **Mulai Hari 1** |
| BPS Luas Panen & Produksi Padi (XLS) | Variabel Y produksi; time-series 1993–2024 per 38 provinsi | ★★★★★ **Mulai Hari 1** |
| IKP Provinsi 2023 (PDF/CSV) | Baseline kerentanan ketahanan pangan saat ini | ★★★★★ **Mulai Hari 2** |
| CHIRPS (GEE export) | Anomali curah hujan per provinsi; intermediate variable | ★★★★☆ **Hari 3–5** |
| FSVA 2024 (fsva.badanpangan.go.id) | Peta spatial kerentanan; data visualisasi | ★★★★★ **Hari 3–4** |
| PDSI Mendeley / NOAA | Drought index; konfirmasi anomali kekeringan | ★★★☆☆ Opsional |
| FAOSTAT | Validasi silang dengan BPS; konteks global | ★★★★☆ **Hari 4** |

***

## Bagian 3: Metode Statistik untuk Infografis Kompetisi

### 3.1 Matriks Metode vs. Tujuan Storytelling

| Metode | Tujuan | Input Data | Output Visual | Kompleksitas | Nilai Storytelling |
|---|---|---|---|---|---|
| **Cross-Correlation Function (CCF)** | Mengukur lag effect ENSO→produksi padi (berapa bulan jeda dampaknya) | ONI monthly + produksi tahunan/trimester | Plot CCF dengan lag terbaik | ★★★☆☆ | ★★★★★ "ENSO merusak panen 6–9 bulan kemudian" |
| **Lag Analysis (OLS regresi dengan lagged X)** | Kuantifikasi: setiap +1°C ENSO → produksi turun berapa % | ONI (t-k) + produksi (t) | Tabel koefisien, scatter plot | ★★★★☆ | ★★★★★ Angka konkret untuk infografis |
| **K-Means Clustering Provinsi** | Mengelompokkan 38 provinsi berdasarkan pola kerentanan ENSO | Perubahan produksi di El Niño vs. La Niña per provinsi + IKP | Peta choropleth cluster | ★★★☆☆ | ★★★★★ Visualisasi peta Indonesia berwarna |
| **Correlation Matrix (Heatmap)** | Melihat hubungan simultan ENSO, curah hujan, luas panen, produksi, IKP | Semua variabel numerik | Heatmap 5×5 atau 6×6 | ★★☆☆☆ | ★★★★☆ Opening panel infografis |
| **Time-Series Visualization (Dual-axis)** | Menampilkan overlapping ONI vs. produksi padi nasional | ONI tahunan + produksi nasional | Line chart dual-axis dengan shading El Niño | ★★☆☆☆ | ★★★★★ Paling mudah dipahami juri |
| **Anomaly Detection (Z-score per provinsi)** | Identifikasi tahun-tahun outlier produksi yang bertepatan ENSO | Produksi per provinsi 1993–2024 | Barplot anomali per tahun | ★★☆☆☆ | ★★★★☆ "Tahun 1997, 2015: panen terburuk" |
| **First-difference regression** | Menghilangkan trend teknologi/kebijakan; isolasi efek ENSO murni | ΔProduksi vs. ΔONI | Scatter plot residual | ★★★★☆ | ★★★★☆ Validasi metodologi kuat |
| **Impulse Response (VAR/VECM)** | Dampak shock ENSO pada produksi dan harga beras | ONI + produksi bulanan + harga | IRF plot | ★★★★★ | ★★★★★ Akademik; cocok jika ada supervisor[^24] |

### 3.2 Pipeline Analisis Optimal 20 Hari

```
HARI 1–3:   Data Collection & Cleaning
            → Download ONI (NOAA), Produksi Padi BPS, IKP Provinsi
            → Merge ke satu spreadsheet: kolom [provinsi | tahun | ONI_Agustus | produksi | luas_panen | IKP]
            → Klasifikasi tahun: El Niño / La Niña / Netral berdasarkan ONI threshold ±0.5°C

HARI 4–6:   Exploratory & Correlation Analysis
            → Heatmap korelasi Pearson (ENSO, curah hujan, luas panen, produksi, IKP)
            → Plot time-series ONI vs. produksi nasional (1993–2023) dengan shading event
            → Identifikasi tahun anomali: 1997/98, 2002, 2009/10, 2015/16, 2020/22 Triple-Dip La Niña

HARI 7–10:  CCF & Lag Analysis
            → Cross-Correlation Function: lag 0 sampai lag 12 bulan antara ONI dan produksi
            → OLS regresi dengan ONI_Aug sebagai predictor terbaik (R² ~0.41–0.61 berdasarkan literatur)
            → Hitung first-difference untuk mengisolasi efek murni ENSO

HARI 11–14: Provincial Clustering
            → Hitung % perubahan produksi di tahun El Niño vs. normal untuk tiap provinsi
            → K-Means atau Ward clustering: 4 kelompok (Sangat Rentan, Rentan, Adaptif, Resilien)
            → Overlay dengan peta IKP untuk validasi

HARI 15–17: Visualization Design
            → Peta Indonesia choropleth: cluster kerentanan ENSO
            → Panel grafik: ONI time-series + produksi + event markers
            → Bar chart: Top 10 provinsi paling rentan vs. Top 10 resilien

HARI 18–20: Infographic Layout & Finalization
            → Integrate semua visual ke dalam layout infografis
            → Narrative flow: Hook → Data → Analisis → Insight → Call to Action
```

***

## Bagian 4: Identifikasi Provinsi — Kerentanan, Resiliensi & Lag

### 4.1 Provinsi Paling Rentan terhadap ENSO

Berdasarkan konvergensi bukti dari literatur akademik dan data IKP:[^25][^26][^1]

| Provinsi | Tipe Kerentanan | Mekanisme | IKP 2023 | Catatan |
|---|---|---|---|---|
| **Papua** | Ekstrem | Sawah rainfed dominan, infrastruktur rendah, isolasi logistik | 42,27 (Prioritas 1) | 17 kabupaten Prioritas 1 sangat rentan[^19] |
| **Papua Barat** | Sangat Tinggi | Sama dengan Papua; sawah minimal | 47,95 (Prioritas 1) | 4 kabupaten Prioritas 1 |
| **Nusa Tenggara Timur (NTT)** | Sangat Tinggi | Curah hujan sangat tergantung ENSO; musim kering panjang | 71,25 | Konsisten masuk daerah kerawanan pangan tinggi[^27] |
| **Maluku** | Tinggi | Kepulauan; akses logistik terbatas; sawah rainfed | 64,37 | IKP provinsi rendah[^19] |
| **Maluku Utara** | Tinggi | Sawah terbatas; distribusi pangan sangat bergantung impor antar pulau | 62,34 | |
| **Nusa Tenggara Barat (NTB)** | Tinggi | Sawah rainfed dominan di Sumbawa & Bima; El Niño → kekeringan parah | 76,51 | Sumbawa sangat rentan kekeringan |
| **Kalimantan Tengah** | Sedang-Tinggi | Lahan gambut; sawah pasang surut sensitif variabilitas curah hujan | 68,90 | |
| **Sulawesi Tengah** | Sedang | Topografi beragam; sawah rainfed di dataran tinggi | 75,83 | |
| **Aceh** | Sedang | El Niño → defisit curah hujan di pesisir utara | 72,96 | |
| **Bengkulu** | Sedang | Sawah rainfed; penelitian BPS-BMKG menunjukkan hubungan negatif suhu-produksi 2010–2023[^28] | 72,27 | |

### 4.2 Provinsi dengan Resiliensi Tidak Biasa (Non-Obvious Insights)

| Provinsi | Pola Resiliensi | Penjelasan |
|---|---|---|
| **Jawa Tengah** | IKP tertinggi kedua (84,80), produksi stabil | Irigasi teknis luas; "run-of-river" tapi manajemen air lebih baik; varietas unggul[^1] |
| **Sulawesi Selatan** | Produksi padi terjaga meski El Niño | Daerah Barru, Wajo, Soppeng punya irigasi primer handal; IKP 83,36[^29] |
| **Bali** | IKP tertinggi nasional (87,65) meski pulau kecil | Sistem irigasi Subak UNESCO; diversifikasi pangan; pariwisata menopang ekonomi |
| **Jawa Timur** | Produksi bertahan di El Niño sedang | Banyak waduk besar (Brantas, Sutami); reservoir buffering rainfall deficits[^1] |
| **DI Yogyakarta** | IKP 83,17 meski lahan terbatas | Keterjangkauan & pemanfaatan pangan sangat baik; safety net kuat |

### 4.3 Lag Effect ENSO → Produksi Padi

Berdasarkan studi CGIAR/Stanford (Naylor et al.) yang paling komprehensif:[^30][^1]

```
MEKANISME LAG:
Aug SSTA (ONI) → Sep–Dec Curah Hujan ↓ → Sep–Dec Tanam Tertunda → Jan–Apr Panen Tertunda/Berkurang

Lag Kritis:
• ENSO Agustus → Curah hujan Sep-Des:        lag ~1–4 bulan  (R² = 0.66)
• ENSO Agustus → Luas tanam Sep-Des:         lag ~1–4 bulan  (R² = 0.73)
• ENSO Agustus → Produksi Jan-Apr (wet):     lag ~5–8 bulan  (R² = 0.49–0.58)
• ENSO Agustus → Produksi annual (Sep-Aug):  lag ~0–12 bulan (R² = 0.61)

Temuan Kritis: Defisit musim hujan TIDAK terkompensasi di musim kering —
efeknya bersifat KUMULATIF selama satu tahun tanam penuh
```

Implikasi untuk CCF analysis: uji lag 0–12 bulan, dan harapkan peak correlation di lag 5–8 bulan untuk data trimester.

***

## Bagian 5: Critical Evaluation — Kelemahan, Confounding Variables & Risiko Interpretasi

### 5.1 Kelemahan Asumsi "ENSO Langsung → Turun Produksi"

| No | Kelemahan / Confounding Variable | Penjelasan | Cara Mitigasi dalam Infografis |
|---|---|---|---|
| 1 | **Teknologi & Varietas Unggul** | Tren peningkatan produktivitas akibat varietas baru (IR64, Ciherang, Inpari) bisa menutupi dampak ENSO[^31] | Gunakan first-difference data, bukan level |
| 2 | **Ekspansi Lahan** | Peningkatan produksi bisa dari perluasan lahan, bukan iklim | Pisahkan analisis luas panen vs. produktivitas (ton/ha) |
| 3 | **Irigasi & Infrastruktur** | Provinsi dengan irigasi teknis (Jawa) hampir imun terhadap ENSO sedang[^32][^1] | Stratifikasi analisis: irigasi vs. rainfed provinces |
| 4 | **Kebijakan Harga & Subsidi** | Program Bulog, subsidi pupuk, dan kebijakan impor mempengaruhi supply dan response petani | Akui sebagai limitation; tidak dapat dikontrol sepenuhnya |
| 5 | **Triple-Dip La Niña 2020–2022** | La Niña TIDAK selalu meningkatkan produksi — banjir di Pandeglang dan Lebak justru menurunkan produksi[^26][^33] | Sajikan sebagai nuance: "La Niña ≠ selalu baik untuk sawah banjir-prone" |
| 6 | **Indian Ocean Dipole (IOD)** | IOD positif (bersamaan El Niño) memperkuat kekeringan di Indonesia timur[^34] | Sebutkan IOD sebagai amplifier; gunakan sebagai advanced insight |
| 7 | **Spatial Heterogeneity** | Tangerang terbukti paling signifikan rentan kekeringan di Banten (β = -33,371 t/tahun), sementara kab. lain di provinsi sama tidak signifikan[^26] | Hindari generalisasi per provinsi; tekankan keberagaman intra-provinsi |
| 8 | **Korelasi vs. Kausalitas** | ENSO berkorelasi dengan produksi, tapi mekanisme intermediary (curah hujan, onset musim) yang sesungguhnya kausal | Jelaskan jalur kausal; jangan klaim "ENSO langsung membunuh panen" |
| 9 | **Data Kualitas BPS** | Data produksi padi BPS pre-2018 menggunakan metode ubinan manual; sejak 2018 menggunakan KSA (Kerangka Sampel Area)[^10] | Beri catatan metodologi; waspadai structural break di 2018 |
| 10 | **Krisis ekonomi 1997–1998** | El Niño 1997/98 bertepatan dengan krisis finansial Asia — dampak produksi mungkin overshooting karena faktor ekonomi[^1] | Pisahkan konfounding; sajikan tahun 1997/98 dengan asterisk |

### 5.2 Risiko Misleading Interpretation yang Harus Dihindari

- **Jangan** menampilkan peta "seluruh Indonesia merah saat El Niño" — Java dengan irigasi teknis relatif aman
- **Jangan** menyimpulkan "La Niña selalu baik" — banjir dari La Niña merusak sawah dataran rendah[^26]
- **Jangan** mengabaikan tren teknologi saat overlay data produksi — gunakan detrended/first-difference data
- **Jangan** mengklaim bahwa lag yang ditemukan di data Jawa berlaku universal untuk semua provinsi[^1]

***

## Bagian 6: Insight Non-Obvious untuk Storytelling Kuat

### 6.1 "The 93% Rule" — Fakta Historis Pemukau

93% dari seluruh kejadian kekeringan di Indonesia antara 1830–1953 terjadi di tahun El Niño. Ini adalah angka pembuka yang kuat untuk panel pertama infografis.[^1]

### 6.2 Produksi Padi Bervariasi 1,4 Juta Ton per 1°C ENSO

Studi Stanford/CGIAR yang paling dikutip menemukan bahwa setiap perubahan 1°C pada Niño 3.4 Agustus mengubah produksi gabah Indonesia rata-rata **1,4 juta ton/tahun**. Pada El Niño super (±3.5°C), defisit bisa mencapai **5 juta ton** — setara kebutuhan beras 33 juta orang selama satu tahun.[^1]

### 6.3 Luas Tanam, Bukan Hasil, yang Pertama Terdampak

Kontra-intuitif: **90% padi Jawa ditanam di lahan beririgasi**, namun tetap terdampak El Niño karena sebagian besar sistem irigasi adalah "run-of-river" yang tidak aktif sampai hujan tiba. Akibatnya, luas panen September–Desember di tahun El Niño rata-rata hanya **500.000 ha** dibandingkan **1.700.000 ha** di tahun La Niña — turun 70%.[^1]

### 6.4 Efek Kumulatif yang Tidak Tertebus

Keterlambatan tanam di musim hujan **tidak terkompensasi** di musim kering berikutnya. Petani yang terlambat menanam di Oktober–Desember juga terlambat menanam musim kering di Mei–Juli. Satu El Niño bisa merusak dua siklus tanam sekaligus.[^30][^1]

### 6.5 Papua: "Negara Dalam Negara" yang Terisolasi

IKP Papua 42,27 (2023) — terendah nasional — tidak semata-mata karena ENSO, melainkan kombinasi: infrastruktur nol, akses pasar sangat terbatas, dan ketergantungan pada sagu/umbi (bukan padi). 21 kabupaten Papua masuk Prioritas 1 (sangat rentan rawan pangan) bukan karena cuaca, tapi karena kemiskinan struktural. **Insight kritis untuk infografis**: ENSO adalah trigger, bukan akar masalah Papua.[^19]

### 6.6 "Triple-Dip La Niña 2020–2022" — Bencana dari Surplus Hujan

Anomali La Niña tiga tahun berturut-turut (2020–2023) di beberapa distrik Banten justru **menurunkan** produksi padi karena banjir di sawah dataran rendah seperti Pandeglang dan Lebak, menentang asumsi bahwa La Niña selalu menguntungkan pertanian. Ini adalah "counter-narrative" yang akan membedakan infografis dari pesaing.[^33][^26]

### 6.7 Agustus SSTA sebagai Early Warning 6 Bulan

Indeks ENSO bulan Agustus bisa **memprediksi** produksi padi musim hujan Januari–April dengan R² = 0.49–0.58. Artinya, data yang tersedia September 2024 sudah bisa memperkirakan kondisi panen April 2025 — implikasi besar untuk kebijakan cadangan pangan.[^1]

***

## Bagian 7: Rekomendasi Kombinasi Dataset Terbaik (20 Hari)

### Dataset Core Minimum (Wajib)

```
1. NOAA ONI CSV (1950–2024)
   → Klasifikasi El Niño / La Niña / Netral per tahun
   → Ambil nilai Agustus sebagai predictor terbaik

2. BPS — Produksi, Luas Panen & Produktivitas Padi per Provinsi (1993–2024)
   → Download tabel XLS dari bps.go.id
   → 38 provinsi × 31 tahun = ~1.200 data points
   
3. IKP Provinsi 2019–2023 (data.go.id / data.badanpangan.go.id)
   → Skor IKP + ranking + kelompok kerentanan per provinsi
   → Gabungkan sebagai variabel baseline kerentanan
```

### Dataset Pendukung (Hari 5–10)

```
4. CHIRPS Indonesia Monthly (via Google Earth Engine)
   → Ekstrak nilai curah hujan bulanan per provinsi (polygon averaging)
   → Hitung anomali vs. baseline 1981–2010

5. FSVA Interactive Atlas (fsva.badanpangan.go.id)
   → Screenshot/export peta kerentanan untuk visual panel
```

### Pipeline Analisis Paling Sederhana dengan Storytelling Terkuat

```
STEP 1: Merge ONI (Aug) + Produksi Padi Nasional → Time-series line chart
        Insight: "Panen turun saat El Niño, naik saat La Niña"
        
STEP 2: Classify years (El Niño / Netral / La Niña), hitung rata-rata produksi per kelompok
        Insight: Beda produksi El Niño vs. La Niña = X juta ton = kebutuhan Y juta orang
        
STEP 3: CCF analysis per provinsi → identify lag 6–9 bulan
        Insight: "ENSO bulan Agustus memberi sinyal 6 bulan sebelum panen berikutnya"
        
STEP 4: Scatter plot 38 provinsi: ENSO sensitivity vs. IKP score
        Insight: Provinsi paling sensitif ENSO justru provinsi dengan IKP terendah
        = "Double jeopardy" — rentan iklim DAN rentan pangan

STEP 5: K-Means 4 cluster → Peta Indonesia berwarna
        Cluster: [Sangat Rentan] [Rentan] [Adaptif] [Resilien]
        
STEP 6: Non-obvious findings panel:
        - Papua: kemiskinan struktural > ENSO
        - Jawa: irigasi sebagai buffer
        - La Niña 2020–2022: banjir merusak panen Banten
```

***

## Bagian 8: Possible Visualizations per Insight

| Insight | Tipe Visualisasi | Tools | Catatan |
|---|---|---|---|
| ENSO time-series vs. produksi nasional 1993–2023 | Dual-axis line chart + shaded El Niño bands | Excel / Python / R | Panel utama, paling impactful |
| Peta 38 provinsi: cluster kerentanan ENSO | Choropleth map | QGIS / Tableau / Python Geopandas | Harus menggunakan shapefile provinsi Indonesia resmi dari BIG/BPS |
| Correlation matrix: ENSO, curah hujan, panen, produksi, IKP | Heatmap Pearson correlation | Python (seaborn) / R | Panel kedua infografis |
| CCF plot: lag ENSO → produksi | Cross-correlation function barplot | Python statsmodels / R | Visualisasi lag; sangat academic-credible |
| Scatter: ENSO sensitivity vs. IKP per provinsi | Bubble chart (bubble = luas sawah) | R ggplot2 / Tableau | Identifikasi "double jeopardy" provinces |
| Bar chart: rata-rata perubahan produksi El Niño vs. La Niña per provinsi | Diverging bar chart | Python / Excel | Bandingkan 38 provinsi sekaligus |
| Top 10 provinsi rentan vs. resilien | Ranked horizontal bar | Excel / Canva | Mudah dipahami juri awam |
| Anomali produksi 1997–2023 timeline | Waterfall / annotated timeline | Python / Flourish | Highlight El Niño 1997/98, 2015/16, La Niña 2020/22 |
| Mekanisme kausal: diagram alir ENSO→panen | Sankey / flow diagram | Canva / Figma | Edukasi mekanisme; bukan data chart |

***

## Bagian 9: Referensi Jurnal Peer-Reviewed untuk Sitasi

| Studi | Temuan Kunci | Relevansi |
|---|---|---|
| Naylor et al. (2002) — CGIAR/Stanford | Setiap 1°C ENSO → 1,4 juta ton produksi padi Indonesia turun; R²=0.61[^1] | **Fundamental study; harus dikutip** |
| Keil et al. (2009) — Climate Research | Vulnerability smallholder farmers to ENSO-related drought, Central Sulawesi[^25] | Kerentanan petani kecil |
| Mulyaqin et al. (2026) — JAAST | Spatial heterogeneity ENSO impacts, Banten; Triple-Dip La Niña effect[^26][^33] | Temuan terbaru 2026 |
| Naylor et al. (2007) — PNAS | Risiko delay monsoon 30 hari meningkat dari 9–18% menjadi 30–40% di 2050[^35][^36] | Proyeksi masa depan |
| Hasudungan et al. — UGM Thesis | VAR/VECM: El Niño negatif terhadap produksi, positif terhadap harga beras[^24] | Model ekonometrik Indonesia |
| Nugroho et al. (2025) — IIETA | XGBoost ENSO + ONI untuk forecast harga beras; R²=0.83[^37] | Machine learning angle |
| Akhmad et al. (2025) — Jamba | Drought risk prevention trilogy, Indonesia[^38] | Risk management perspective |
| Bashir et al. (2019) — UMS | Faktor human capital, irigasi, harga mempengaruhi produksi padi[^31] | Confounding variables |

***

## Kesimpulan Analitik

Kombinasi paling kuat untuk kompetisi SIC 20 hari adalah **ONI + BPS Padi + IKP Provinsi**, dilengkapi CHIRPS untuk validasi spasial. Analisis yang paling membedakan adalah demonstrasi **"double jeopardy"** — bahwa provinsi dengan kerentanan iklim (ENSO sensitivity tinggi) hampir selalu bertepatan dengan provinsi kerentanan pangan (IKP rendah), menciptakan spiral deprivasi yang tidak bisa diselesaikan hanya dengan kebijakan produksi semata.

Narasi terkuat bukan sekadar "ENSO menurunkan panen", melainkan: **siapa yang paling menanggung beban anomali iklim, mengapa mereka tidak punya buffer, dan apa yang bisa diubah dengan data**. Itulah inti storytelling yang layak menang kompetisi nasional.

---

## References

1. [Using El Niño/Southern Oscillation climate data to improve ...](https://cgspace.cgiar.org/server/api/core/bitstreams/22498de6-0f41-4f12-be9e-66a1751be9f6/content)

2. [The Oceanic Niño Index - NASA Scientific Visualization Studio](https://svs.gsfc.nasa.gov/30847/) - Animated plot of the Oceanic Niño Index (ONI) from 1950-2023, with significant El Niño events labele...

3. [Nino 3.4 Sea Surface Temperature time series from Reanalysis](https://data.marine.copernicus.eu/product/GLOBAL_OMI_CLIMVAR_enso_sst_area_averaged_anomalies/description) - Monthly mean values are given here. The reference period is 1993-2014. El Nino or La Nina events are...

4. [BMKG Dataonline](https://dataonline.bmkg.go.id/dataonline-home) - BMKG Dataonline.

5. [Data Terbuka BMKG](https://data.bmkg.go.id) - Data BMKG yang tersedia dalam format terbuka dan mudah digunakan kembali, dengan tujuan untuk mening...

6. [CHIRPS: Rainfall Estimates from Rain Gauge and Satellite ...](https://www.chc.ucsb.edu/data/chirps) - CHIRPS is a 35+ year quasi-global rainfall data set. Spanning 50°S-50°N (and all longitudes) and ran...

7. [CHIRPS Precipitation Daily: Climate Hazards Center InfraRed Precipitation With Station Data (Version 2.0 Final) | Earth Engine Data Catalog | Google for Developers](https://developers.google.com/earth-engine/datasets/catalog/UCSB-CHG_CHIRPS_DAILY?hl=id) - Climate Hazards Center InfraRed Precipitation with Station data (CHIRPS) is a 30+ year quasi-global ...

8. [Palmer Drought Severity Index (PDSI) - Physical Sciences Laboratory](https://psl.noaa.gov/data/gridded/data.pdsi.html) - Palmer Drought Severity Index (PDSI). Monthly gridded global PDSI (Palmer Drought Severity Index) va...

9. [Dataset of Drought and Flood Impact on Croplands in Southeast ...](https://data.mendeley.com/datasets/nv7jbmkynm/1) - The first raw dataset folder contains monthly temporal Palmar drought Severity index (PDSI) by count...

10. [Luas Panen, Produksi, dan Produktivitas Padi Menurut Provinsi](https://www.bps.go.id/id/statistics-table/2/MTQ5OCMy/luas-panen-produksi-dan-produktivitas-padi-menurut-provinsi.html)

11. [Luas Panen, Produksi, dan Produktivitas Padi Menurut ...](https://www.bps.go.id/id/statistics-table/2/MTQ5OCMy/luas-panen--produksi--dan-produktivitas-padi-menurut-provinsi.html)

12. [Luas Panen, Produktivitas, dan Produksi Padi (BPS)](https://data.go.id/dataset/dataset/luas-panen-produktivitas-dan-produksi-padi-bps) - Dataset ini menyajikan data Luas Panen, Produktivitas, dan Produksi Padi dalam satuan hektare, kuint...

13. [FAOSTAT](https://www.fao.org/faostat/) - FAOSTAT provides free access to food and agriculture data for over 245 countries and territories and...

14. [Agricultural production statistics 2010–2023](https://www.fao.org/statistics/highlights-archive/highlights-detail/agricultural-production-statistics-2010-2023/en) - FAOSTAT released new data on agricultural production. The agricultural sector plays a key role in ac...

15. [record view | Rice](https://data.un.org/Data.aspx?d=FAO&f=itemCode%3A27) - FAOSTAT contains data for 200 countries and more than 200 primary products and inputs in its core da...

16. [Indeks Ketahanan Pangan (IKP) Provinsi Update Tahun 2024](https://data.go.id/dataset/dataset/indeks-ketahanan-pangan-ikp-provinsi-update-tahun-2024) - INFORMASI: Data berikut ini masih dalam proses pemenuhan Prinsip SDI. Indeks Ketahanan Pangan (IKP) ...

17. [Indeks Ketahanan Pangan (IKP) Provinsi Update Tahun 2024](https://data.badanpangan.go.id/datasetpublications/2dd/ikp-prov-2024) - Menurut Keputusan Kepala BPS: Angka yang menggambarkan kondisi terpenuhinya pangan bagi negara sampa...

18. [INDEKS KETAHANAN PANGAN (IKP) TAHUN 2022 - 2023 - Dataset](https://data.bnpb.go.id/dataset/indeks-ketahanan-pangan-ikp-tahun-2022) - IKP Nasional memiliki peran yang sangat strategis dalam mengukur capaian pembangunan ketahanan panga...

19. [[PDF] Indeks Ketahanan Pangan](https://data.badanpangan.go.id/nfs_storage/public/publication/documents/1728546912.pdf) - Badan Pangan Nasional pada tahun 2023 menyusun IKP Nasional dengan unit analisis tingkat kabupaten/k...

20. [FSVA](https://fsva.badanpangan.go.id) - Peta Ketahanan dan Kerentanan Pangan (Food Security and Vulnerability Atlas – FSVA) merupakan peta t...

21. [Indikator Capaian Kinerja Urusan Pangan Tahun 2019-2023](https://data.go.id/dataset/dataset/indikator-capaian-kinerja-urusan-pangan-tahun-2019-2023) - Pengenalan Dataset Dataset "Indikator Capaian Kinerja Urusan Pangan Tahun 2019-2023" ini diterbitkan...

22. [World Bank Open Data](https://data.worldbank.org/indicator/SN.ITK.DEFC.ZS?locations=ID) - Free and open access to global development data

23. [Indonesia](https://www.globalhungerindex.org/indonesia.html) - In the 2025 Global Hunger Index, Indonesia ranks 70th out of the 123 countries with sufficient data ...

24. [THE IMPACTS OF EL NINO-SOUTHERN OSCILLATION ON RICE ...](https://etd.repository.ugm.ac.id/penelitian/detail/203179) - The objectives of this paper are to empirically study the impact of ENSO on rice production and pric...

25. [Vulnerability of smallholder farmers to ENSO-related drought in ...](https://cgspace.cgiar.org/items/d8ca56ad-34c7-4ea0-85a0-8b678e104bd2) - El Niño causes comparatively dry conditions leading to substantial declines in crop yields, with sev...

26. [Spatial Heterogeneity of Rice Production Responses to ENSO ...](https://www.jaast.org/index.php/jaast/article/view/503) - This study investigates rice production responses to ENSO phases in four districts of Banten Provinc...

27. [Institutional framework for improving food security in crisis-prone ...](https://www.sciencedirect.com/science/article/pii/S2666154326001444) - Provinces such as Aceh, East Nusa Tenggara (NTT), and South Sulawesi are consistently identified as ...

28. [The Impact of El Niño and La Nina on Fluctuation of Rice Production ...](https://www.academia.edu/48776953/The_Impact_of_El_Ni%C3%B1o_and_La_Nina_on_Fluctuation_of_Rice_Production_in_Banten_Province) - Rice production in Indonesia is facing serious problem, in which the production is fluctuated causin...

29. [pola sebaran curah hujan di sulawesi selatan pada periode el-nino ...](https://repository.unhas.ac.id/id/eprint/50018/) - Faktor utama yang mempengaruhi pola curah hujan di wilayah ini adalah fenomena iklim El Niño Souther...

30. [[PDF] Using El Niño/Southern Oscillation climate data to improve food ...](https://cgspace.cgiar.org/bitstreams/22498de6-0f41-4f12-be9e-66a1751be9f6/download) - Indeed, rainfall early in the wet season tends to dictate rice planting patterns for the next 8–9 mo...

31. [Identifying factors influencing rice production and ...](https://journals.ums.ac.id/JEP/article/view/5939/0) - oleh A Bashir · Dirujuk 83 kali — Our findings indicate that rice production can be affected by huma...

32. [[PDF] Rice cultivation, Weather variation, and irrigation in indonesia](https://edepot.wur.nl/638139) - In other words, the rice cultivation in provinces with a larger irrigation program coverage a more s...

33. [Spatial Heterogeneity of Rice Production Responses to ENSO ...](http://www.jaast.org/index.php/jaast/article/view/503) - El Niño–Southern Oscillation (ENSO) anomalies are significant drivers of climate variability affecti...

34. [INVESTIGATING THE IMPACTS OF ENSO AND IOD ON RICE ...](http://proceedings.tiikmpublishing.com/index.php/agrico/article/view/1312) - oleh IODONRPIN SOUTH · 2023 — This research investigates the impact of both ENSO and IOD on rainfall...

35. [Assessing risks of climate variability and climate change for Indonesian rice agriculture | PNAS](https://www.pnas.org/doi/10.1073/pnas.0701825104) - El Niño events typically lead to delayed rainfall and decreased rice planting in Indonesia's main ri...

36. [Assessing risks of climate variability and climate change for ... - PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC1876519/) - El Niño events typically lead to delayed rainfall and decreased rice planting in Indonesia's main ri...

37. [Forecasting Rice Price Volatility in Indonesia: Integrating ENSO ...](https://www.iieta.org/journals/ijsdp/paper/10.18280/ijsdp.201109) - This study analyzes how climate anomalies, particularly El Niño and La Niña, influence rice price vo...

38. [http://www.jamba.org.za](https://jamba.org.za/index.php/jamba/article/download/1811/3495)

