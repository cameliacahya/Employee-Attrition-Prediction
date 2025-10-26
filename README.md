# How to Run
## Setup Environment
Versi Python
```
Python==3.10.11
```
1. **Buat virtual environment**  
    ```bash
    py -3.10 -m venv .venv
    ```
2. **Aktifkan virtual environment**  
    - Windows:
      ```bash
      .venv\Scripts\activate
      ```
    - macOS/Linux:
      ```bash
      source .venv/bin/activate
      ```
## Install Library
3. **Install dependencies dari `requirements.txt`**  
    ```bash
    py -m pip install -r requirements.txt
    ```
## Menjalankan Project
4. **Jalankan notebook utama**  

## Catatan
- Pastikan Python 3.10.11 sudah terinstall di komputer Anda.
  Cara mengeceknya dengan
  ```bash
  py --list
  ```
- Untuk keluar dari environment, gunakan `deactivate`.
---

# Employee Attrition Prediction
## Description
Proyek ini bertujuan untuk membangun model prediktif yang mampu memperkirakan kemungkinan seorang karyawan keluar dari perusahaan (*attrition*). Tingkat attrition yang tinggi dapat menyebabkan peningkatan biaya rekrutmen, penurunan produktivitas, serta berdampak pada moral dan kinerja organisasi. Oleh karena itu, deteksi dini terhadap karyawan yang berisiko keluar menjadi sangat penting bagi manajemen sumber daya manusia.

Dataset berisi informasi demografis, karakteristik pekerjaan, kepuasan kerja, beban kerja, serta status attrition. Model dianalisis dan dievaluasi menggunakan beberapa pendekatan machine learning dengan mempertimbangkan isu **class imbalance** dan metrik evaluasi yang relevan.

## Data Understanding
### Sumber Data
Dataset yang digunakan dalam proyek ini berasal dari kompetisi [Kaggle](https://www.kaggle.com/competitions/tugas-1-sml-a-2025). Dataset ini berisi profil karyawan serta informasi lingkungan kerja yang berkaitan dengan kemungkinan seorang karyawan keluar dari perusahaan (attrition). Dataset mencakup 1.470 observasi karyawan dengan 35 fitur. Datset terbagi menjadi train (1176 observasi) dan test (294 observasi).
### Deskripsi Fitur 
| Nama Fitur                | Deskripsi                                                                                             | Tipe Data    |
|---------------------------|-------------------------------------------------------------------------------------------------------|--------------|
| `id`                      | ID unik karyawan untuk identifikasi.                                                                  | `object`     |
| `Age`                     | Usia karyawan                                                                                         | `int64`      |
| `BusinessTravel`          | Frekuensi perjalanan dinas karyawan (contoh: `Non-Travel`, `Travel_Rarely`, `Travel_Frequently)`.     | `object`     |
| `DailyRate`               | Gaji harian                                                                                           | `int64`      |
| `Department`              | Departemen tempat karyawan bekerja (contoh: `Sales`, `Research & Development`, `Human Resources)`.    | `object`     |
| `DistanceFromHome`        | Jarak tempat tinggal karyawan ke kantor.                                                              | `int64`      |
| `Education`               | Tingkat pendidikan terakhir: 1 = Below College, 2 = College, 3 = Bachelor, 4 = Master, 5 = Doctor.    | `int64`      |
| `EducationField`          | Bidang studi terakhir karyawan (contoh: `Life Sciences`, `Medical`, `Marketing`).                     | `object`     |
| `EmployeeCount`           | Jumlah karyawan (selalu 1 dalam dataset).                                                             | `int64`      |
| `EmployeeNumber`          | Nomor unik karyawan dalam sistem HR.                                                                  | `int64`      |
| `EnvironmentSatisfaction` | Tingkat kepuasan terhadap lingkungan kerja: 1 = Low, 2 = Medium, 3 = High, 4 = Very High.             | `int64`      |
| `Gender`                  | Jenis kelamin karyawan (`Male`/`Female`).                                                             | `object`     |
| `HourlyRate`              | Upah per jam.                                                                                         | `int64`      |
| `JobInvolvement`          | Tingkat keterlibatan pekerjaan: 1 = Low, 2 = Medium, 3 = High, 4 = Very High.                         | `int64`      |
| `JobLevel`                | Level jabatan karyawan.                                                                               | `int64`      |
| `JobRole`                 | Posisi/jabatan spesifik karyawan (contoh: `Sales Executive`, `Research Scientist`).                   | `object`     |
| `JobSatisfaction`         | Tingkat kepuasan pekerjaan: 1 = Low, 2 = Medium, 3 = High, 4 = Very High.                             | `int64`      |
| `MaritalStatus`           | Status pernikahan karyawan (`Single`, `Married`, `Divorced`).                                         | `object`     |
| `MonthlyIncome`           | Gaji bulanan karyawan.                                                                                | `int64`      |
| `MonthlyRate`             | Tarif bulanan karyawan.                                                                               | `int64`      |
| `NumCompaniesWorked`      | Jumlah perusahaan tempat karyawan pernah bekerja sebelumnya.                                          | `int64`      |
| `Over18`                  | Status usia di atas 18 tahun (selalu `Y` dalam dataset).                                              | `object`     |
| `OverTime`                | Apakah karyawan sering lembur (`Yes`/`No`).                                                           | `object`     |
| `PercentSalaryHike`       | Persentase kenaikan gaji tahunan terakhir.                                                            | `int64`      |
| `RelationshipSatisfaction`| Tingkat kepuasan terhadap hubungan kerja: 1 = Low, 2 = Medium, 3 = High, 4 = Very High.               | `int64`      |
| `StandardHours`           | Jam kerja standar (selalu 80 dalam dataset).                                                          | `int64`      |
| `StockOptionLevel`        | Level kepemilikan saham perusahaan.                                                                   | `int64`      |
| `TotalWorkingYears`       | Total tahun pengalaman kerja.                                                                         | `int64`      |
| `TrainingTimesLastYear`   | Jumlah pelatihan yang diikuti dalam setahun terakhir.                                                 | `int64`      |
| `WorkLifeBalance`         | Tingkat keseimbangan kerja–hidup: 1 = Bad, 2 = Good, 3 = Better, 4 = Best.                            | `object`     |
| `YearsAtCompany`          | Total tahun bekerja di perusahaan saat ini.                                                           | `int64`      |
| `YearsInCurrentRole`      | Total tahun di posisi/jabatan saat ini.                                                               | `int64`      |
| `YearsSinceLastPromotion` | Tahun sejak promosi terakhir.                                                                         | `int64`      |
| `YearsWithCurrManager`    | Tahun bekerja dengan manajer saat ini.                                                                | `int64`      |
| `Attrition`               | Target: apakah karyawan keluar dari perusahaan (1 = `Yes`/ 0 = `No`).                                 | `int64`      |

## Exploratory Data Analysis (EDA)
Pada tahap ini, analisis eksploratori dilakukan khusus pada data train karena dataset tersebut digunakan dalam proses pelatihan dan validasi model. EDA dilakukan untuk memperoleh pemahaman awal mengenai profil karyawan, hubungan antar variabel, serta faktor yang berpotensi memengaruhi keputusan karyawan untuk keluar dari perusahaan.
### Deskripsi Variabel
| Fitur                       | Count   | Mean         | Std          | Min       | 25%          | 50%          | 75%          | Max          |
|-----------------------------|---------|--------------|--------------|-----------|--------------|--------------|--------------|--------------|
| `Age`	                      | 1176.0	| 36.998299	   | 9.178142	  | 18.0	  | 30.00	     | 36.0	        | 43.00        | 60.0         |
| `DailyRate`    	          | 1176.0	| 803.991497   | 401.339423	  | 103.0	  | 467.75	     | 799.5	    | 1157.00	   | 1499.0       |
| `DistanceFromHome`	      | 1176.0	| 9.357993	   | 8.179803	  | 1.0	      | 2.00	     | 7.0	        | 14.00	       | 29.0         |
| `Education`	              | 1176.0	| 2.906463	   | 1.027996	  | 1.0	      | 2.00	     | 3.0	        | 4.00	       | 5.0          |
| `EmployeeCount`	          | 1176.0	| 1.000000	   | 0.000000	  | 1.0	      | 1.00	     | 1.0	        | 1.00	       | 1.0          |
| `EmployeeNumber`	          | 1176.0	| 1015.830782  | 599.657438	  | 1.0	      | 487.75	     | 1004.5	    | 1547.25	   | 2062.0       |
| `EnvironmentSatisfaction`	  | 1176.0	| 2.716837	   | 1.088707	  | 1.0	      | 2.00	     | 3.0	        | 4.00	       | 4.0          |
| `HourlyRate`	              | 1176.0	| 65.500000	   | 20.373324	  | 30.0	  | 48.00	     | 66.0	        | 83.00	       | 100.0        |
| `JobInvolvement`	          | 1176.0	| 2.737245	   | 0.703673	  | 1.0	      | 2.00	     | 3.0	        | 3.00	       | 4.0          |
| `JobLevel`	              | 1176.0	| 2.076531	   | 1.091987	  | 1.0	      | 1.00	     | 2.0	        | 3.00	       | 5.0          |
| `JobSatisfaction`	          | 1176.0	| 2.719388	   | 1.110644	  | 1.0	      | 2.00	     | 3.0	        | 4.00	       | 4.0          |
| `MonthlyIncome`             | 1176.0	| 6544.024660  | 4653.743955  | 1009.0	  | 2948.00	     | 5004.5	    | 8420.50	   | 19973.0      |
| `MonthlyRate`	              | 1176.0	| 14390.239796 | 7192.834394  | 2094.0	  | 8051.00	     | 14373.0	    | 20770.75	   | 26999.0      | 
| `NumCompaniesWorked`	      | 1176.0	| 2.693027	   | 2.486077	  | 0.0	      | 1.00	     | 2.0	        | 4.00	       | 9.0          |
| `PercentSalaryHike`	      | 1176.0	| 15.239796	   | 3.679081	  | 11.0	  | 12.00	     | 14.0	        | 18.00	       | 25.0         |
| `PerformanceRating`	      | 1176.0	| 3.157313	   | 0.364250	  | 3.0	      | 3.00	     | 3.0	        | 3.00	       | 4.0          |
| `RelationshipSatisfaction`  | 1176.0	| 2.738946	   | 1.087201	  | 1.0	      | 2.00	     | 3.0	        | 4.00	       | 4.0          |
| `StandardHours`	          | 1176.0	| 80.000000    | 0.000000	  | 80.0	  | 80.00	     | 80.0     	| 80.00	       | 80.0         |
| `StockOptionLevel`	      | 1176.0	| 0.790816	   | 0.845786	  | 0.0	      | 0.00	     | 1.0	        | 1.00	       | 3.0          |
| `TotalWorkingYears`	      | 1176.0	| 11.364796	   | 7.801391	  | 0.0	      | 6.00	     | 10.0	        | 15.00	       | 40.0         |
| `TrainingTimesLastYear`	  | 1176.0	| 2.760204	   | 1.256262	  | 0.0	      | 2.00	     | 3.0	        | 3.00	       | 6.0          |
| `WorkLifeBalance`	          | 1176.0	| 2.757653	   | 0.718113	  | 1.0	      | 2.00	     | 3.0	        | 3.00	       | 4.0          |
| `YearsAtCompany`	          | 1176.0	| 7.050170	   | 6.086612	  | 0.0	      | 3.00	     | 5.0	        | 10.00	       | 37.0         |
| `YearsInCurrentRole`	      | 1176.0	| 4.231293	   | 3.569503	  | 0.0	      | 2.00	     | 3.0	        | 7.00	       | 17.0         | 
| `YearsSinceLastPromotion`	  | 1176.0	| 2.182823	   | 3.215348	  | 0.0  	  | 0.00	     | 1.0	        | 3.00	       | 15.0         | 
| `YearsWithCurrManager`	  | 1176.0	| 4.196429	   | 3.564795	  | 0.0	      | 2.00	     | 3.0	        | 7.00	       | 17.0         | 
| `Attrition`	              | 1176.0	| 0.161565	   | 0.368208	  | 0.0	      | 0.00	     | 0.0	        | 0.00	       | 1.0          |

Dataset ini memuat profil 1.176 karyawan dengan usia rata-rata 37 tahun dan pengalaman kerja rata-rata 11 tahun. Mayoritas berada di level jabatan 1–3 (staff hingga supervisi) dengan masa kerja di perusahaan sekitar 7 tahun. Pendapatan bulanan bervariasi (rata-rata 6.544) dan kenaikan gaji terbaru sekitar 15%. Tingkat kepuasan kerja rata-rata sedang hingga tinggi, meski beberapa karyawan memiliki kepuasan rendah, berpotensi meningkatkan risiko attrition. Secara keseluruhan, populasi karyawan relatif muda dengan variasi pengalaman dan kompensasi, yang menjadi faktor penting dalam prediksi risiko turnover.

### Menangani Missing Value 
Dalam tahap awal pembersihan data, dilakukan pengecekan terhadap duplikasi data dan missing value. Hasilnya menunjukkan bahwa tidak terdapat duplikasi data maupun missing value di seluruh kolom fitur maupun target. Hal ini mengindikasikan bahwa dataset sudah lengkap dan tidak memerlukan teknik imputasi lebih lanjut.
| Nama Fitur                | Jumlah Missing Value   | 
|---------------------------|------------------------|
| `id`                      | 0                      |
| `Age`                     | 0                      |
| `BusinessTravel`          | 0                      |
| `DailyRate`               | 0                      |
| `Department`              | 0                      |
| `DistanceFromHome`        | 0                      |
| `Education`               | 0                      |
| `EducationField`          | 0                      |
| `EmployeeCount`           | 0                      |
| `EmployeeNumber`          | 0                      |
| `EnvironmentSatisfaction` | 0                      |
| `Gender`                  | 0                      |
| `HourlyRate`              | 0                      |
| `JobInvolvement`          | 0                      |
| `JobLevel`                | 0                      |
| `JobRole`                 | 0                      |
| `JobSatisfaction`         | 0                      |
| `MaritalStatus`           | 0                      |
| `MonthlyIncome`           | 0                      |
| `MonthlyRate`             | 0                      |
| `NumCompaniesWorked`      | 0                      |
| `Over18`                  | 0                      |
| `OverTime`                | 0                      |
| `PercentSalaryHike`       | 0                      |
| `RelationshipSatisfaction`| 0                      |
| `StandardHours`           | 0                      |
| `StockOptionLevel`        | 0                      |
| `TotalWorkingYears`       | 0                      |
| `TrainingTimesLastYear`   | 0                      |
| `WorkLifeBalance`         | 0                      |
| `YearsAtCompany`          | 0                      |
| `YearsInCurrentRole`      | 0                      |
| `YearsSinceLastPromotion` | 0                      |
| `YearsWithCurrManager`    | 0                      |
| `Attrition`               | 0                      |

### Menangani Outliers
Deteksi outlier dilakukan menggunakan metode Interquartile Range (IQR) untuk semua fitur numerik. Hasil analisis menunjukkan bahwa beberapa fitur memiliki jumlah outlier yang cukup signifikan, seperti **MonthlyIncome (86 outlier)**, **NumCompaniesWorked (36 outlier)**, **PerformanceRating (185 outlier)**, **StockOptionLevel (66 outlier)**, **TotalWorkingYears (52 outlier)**, **TrainingTimesLastYear (174 outlier)**, **YearsAtCompany (52 outlier)**, **YearsInCurrentRole (16 outlier)**, **YearsSinceLastPromotion (85 outlier)**, dan **YearsWithCurrManager (10 outlier)**. Keberadaan outlier pada fitur-fitur tersebut mengindikasikan adanya variasi ekstrem dalam karakteristik karyawan, misalnya dalam hal pendapatan, pengalaman kerja, atau frekuensi promosi dan pelatihan.
| Fitur                     | Jumlah Outlier   |
|---------------------------|------------------|
| `Age`                     | 0                |
| `DailyRate`               | 0                |
| `DistanceFromHome`        | 0                |
| `Education`               | 0                |
| `EmployeeCount`           | 0                |
| `EmployeeNumber`          | 0                |
| `EnvironmentSatisfaction` | 0                |
| `HourlyRate`              | 0                |
| `JobInvolvement`          | 0                |
| `JobLevel`                | 0                |
| `JobSatisfaction`         | 0                |
| `MonthlyIncome`           | 86               |
| `MonthlyRate`             | 0                |
| `NumCompaniesWorked`      | 36               |
| `PercentSalaryHike`       | 0                |
| `PerformanceRating`       | 185              |
| `RelationshipSatisfaction`| 0                |
| `StandardHours`           | 0                |
| `StockOptionLevel`        | 66               |
| `TotalWorkingYears`       | 52               |
| `TrainingTimesLastYear`   | 174              |
| `WorkLifeBalance`         | 0                |       
| `YearsAtCompany`          | 52               |
| `YearsInCurrentRole`      | 16               |
| `YearsSinceLastPromotion` | 85               |
| `YearsWithCurrManager`    | 10               |

<img width="1489" height="4490" alt="image" src="https://github.com/user-attachments/assets/11ddb227-4e44-4a83-b0f5-a760ff4a1614" />
Outlier ini tidak dihapus agar informasi penting mengenai variasi karakteristik karyawan tetap terjaga, yang dapat berpengaruh pada analisis risiko attrition.

### Distribusi Fitur Numerik
Berdasarkan histogram yang ditampilkan di bawah :

- **Age**: Distribusi usia cenderung sedikit miring ke kanan (positively skewed), menunjukkan bahwa sebagian besar karyawan berusia di bawah 40 tahun, dengan puncaknya berada di sekitar 30-an. Ada juga sejumlah kecil karyawan yang lebih tua, hingga usia 60 tahun.
- **DailyRate**: Distribusi DailyRate terlihat cukup merata (uniform distribution), meskipun ada sedikit fluktuasi. Ini menunjukkan bahwa DailyRate tersebar relatif proporsional di seluruh rentang nilainya.
- **DistanceFromHome**: Distribusi DistanceFromHome sangat miring ke kanan. Ini berarti sebagian besar karyawan tinggal cukup dekat dengan tempat kerja (jarak kurang dari 10), sementara ada beberapa karyawan yang tinggal sangat jauh.
- **MonthlyIncome**: Distribusi MonthlyIncome sangat miring ke kanan. Sebagian besar karyawan memiliki pendapatan bulanan yang relatif rendah, dengan puncaknya berada di bawah 5000. Ada segelintir karyawan dengan pendapatan yang jauh lebih tinggi.
- **TotalWorkingYears**: Distribusi TotalWorkingYears juga miring ke kanan. Mayoritas karyawan memiliki pengalaman kerja total kurang dari 10 tahun, dengan beberapa karyawan yang memiliki pengalaman kerja yang sangat panjang.
- **YearsAtCompany**: Distribusi YearsAtCompany sangat miring ke kanan, mirip dengan TotalWorkingYears. Ini menunjukkan bahwa sebagian besar karyawan baru bekerja di perusahaan ini selama beberapa tahun, dengan segelintir yang telah bekerja untuk waktu yang lama.

Secara umum, banyak variabel numerik menunjukkan distribusi yang miring (skewed), terutama miring ke kanan. Ini menunjukkan adanya nilai-nilai ekstrim yang tinggi pada variabel-variabel tersebut (seperti MonthlyIncome, DistanceFromHome, TotalWorkingYears, YearsAtCompany). Informasi ini penting untuk dipahami karena dapat mempengaruhi pilihan model machine learning dan kebutuhan akan feature scaling atau transformasi data.
<img width="1987" height="1572" alt="image" src="https://github.com/user-attachments/assets/78317d50-66e2-4c08-bbdc-168684faef67" />

### Distribusi Fitur Kategorik
Berdasarkan plot distribusi di bawah :

- **BusinessTravel**: Mayoritas karyawan (Travel_Rarely) melakukan perjalanan bisnis sesekali, diikuti oleh Travel_Frequently, dan sebagian kecil Non-Travel. Ini menunjukkan bahwa sebagian besar karyawan tidak melakukan perjalanan bisnis secara intensif.
- **Department**: Departemen Research & Development memiliki jumlah karyawan terbanyak, diikuti oleh Sales, dan paling sedikit adalah Human Resources. Ini mengindikasikan struktur organisasi perusahaan dengan fokus yang lebih besar pada R&D dan Penjualan.
- **EducationField**: Bidang pendidikan yang paling umum adalah Life Sciences dan Medical, diikuti oleh Marketing dan Technical Degree. Bidang Other dan Human Resources memiliki jumlah yang paling sedikit.
- **Gender**: Terdapat lebih banyak karyawan Male dibandingkan Female. Ini menunjukkan ketidakseimbangan gender dalam dataset.
- **JobRole**: Peran pekerjaan (JobRole) yang paling banyak adalah Sales Executive, diikuti oleh Research Scientist, dan Laboratory Technician. Beberapa peran seperti Human Resources dan Sales Representative memiliki jumlah yang paling sedikit.
- **MaritalStatus**: Status pernikahan yang paling umum adalah Married, diikuti oleh Single, dan paling sedikit adalah Divorced.
- **OverTime**: Sebagian besar karyawan (No) tidak melakukan kerja lembur, sementara sebagian kecil (Yes) melakukan kerja lembur. Ini menunjukkan bahwa kerja lembur bukanlah praktik yang sangat umum di perusahaan ini, atau setidaknya tidak dicatat untuk mayoritas karyawan.

Secara keseluruhan, kita melihat adanya ketidakseimbangan yang signifikan pada beberapa variabel kategorik seperti Gender dan OverTime. Variabel lain seperti Department, EducationField, dan JobRole memiliki beberapa kategori dominan. Memahami distribusi ini penting untuk langkah preprocessing selanjutnya, seperti encoding variabel kategorik, dan juga untuk mempertimbangkan strategi penanganan ketidakseimbangan data jika target (Attrition) juga tidak seimbang terkait dengan variabel kategorik ini.
<img width="1790" height="985" alt="image" src="https://github.com/user-attachments/assets/0fae0edf-21e7-4ee3-a39a-b65517633f54" />

## Data Preparation
### 1. Drop Fitur yang Tidak Digunakan
ada tahap data preparation, dilakukan proses pembersihan data yang diawali dengan penghapusan fitur yang tidak memberikan kontribusi terhadap analisis maupun pemodelan. Beberapa variabel seperti `id`, `EmployeeCount`, `EmployeeNumber`, `StandardHours`, dan `Over18` dihapus dari kedua dataset (train dan test) karena seluruhnya bersifat konstan atau tidak relevan dalam memprediksi attrition, sehingga keberadaannya berpotensi menambah noise pada model.
### 2. Splitting Data
Setelah itu dilakukan pemisahan data menjadi fitur prediktor dan variabel target, di mana variabel `Attrition` ditetapkan sebagai **target**, sedangkan variabel lainnya digunakan sebagai fitur pada dataset training. 

Proses pembagian dataset dilakukan menggunakan metode train-test split dengan proporsi **80% data sebagai data train** dan **20% sebagai data validasi**, serta menerapkan parameter stratify = y untuk menjaga keseimbangan proporsi kelas target pada kedua subset sehingga model dapat dievaluasi secara konsisten terhadap fenomena attrition.
### 3. Preprocessing
**Fitur numerik** diproses melalui **Standardization** menggunakan `StandardScaler` agar seluruh variabel memiliki skala yang sebanding dan tidak mempengaruhi bobot model secara tidak proporsional. Sementara itu, **fitur kategorik** dikodekan menggunakan `One-Hot Encoding` dengan handle_unknown='ignore' untuk mengantisipasi kemunculan kategori baru pada data pengujian tanpa menimbulkan error saat proses prediksi. 

Seluruh proses transformasi ini diintegrasikan dalam sebuah ColumnTransformer sehingga baik scaling maupun encoding dapat diterapkan secara otomatis dan konsisten pada pipeline pemodelan yang digunakan pada tahap analisis berikutnya.
```python
num_transform = Pipeline(steps=[
    ('scaler', StandardScaler())
])

cat_transform = Pipeline(steps=[
    ('onehot', OneHotEncoder(handle_unknown='ignore'))
])

preprocessor = ColumnTransformer(
    transformers=[
        ('num', num_transform, num_col),
        ('cat', cat_transform, cat_col)
    ]
)
```
## Model Training, Selection, and Tuning
### 1. Model Training
Pada tahap pengembangan model, digunakan lima algoritma klasifikasi berbasis **linear, tree-based,** dan **ensemble/boosting**. Model yang digunakan meliputi **Logistic Regression (LR), Random Forest (RF), Extreme Gradient Boosting (XGB), LightGBM (LGBM),** dan **Support Vector Classifier (SVC).** Pemilihan kelima model ini didasarkan pada keberagaman kemampuan dalam menangani data kategorik maupun numerik hasil preprocessing One-Hot Encoding, serta untuk membandingkan pendekatan _linear vs tree-based vs boosting_ pada permasalahan prediksi attrition.

#### Logistic Regression (LR)
```python
from sklearn.linear_model import LogisticRegression

logreg_model = LogisticRegression(max_iter=1000, random_state=42)
logreg_model.fit(X_train, y_train)
```
[Logistic Regression](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html) merupakan model linear yang banyak digunakan sebagai baseline dalam klasifikasi biner, termasuk prediksi attrition. Model ini menghitung probabilitas keanggotaan kelas menggunakan fungsi logit serta memberikan interpretasi yang baik melalui odds ratio pada setiap fitur prediktor.

#### Random Forest (RF)
```python
from sklearn.ensemble import RandomForestClassifier

rf_model = RandomForestClassifier(random_state=42)
rf_model.fit(X_train, y_train)
```
[Random Forest](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html) adalah metode bagging ensemble yang menggabungkan beberapa pohon keputusan untuk memperbaiki stabilitas dan akurasi prediksi. Dengan melakukan random sampling terhadap data dan fitur, model ini mampu mengurangi overfitting serta bekerja baik pada hubungan non-linear antar fitur.

#### Extreme Gradient Boosting (XGB)
```python
from xgboost import XGBClassifier

xgb_model = XGBClassifier(
    use_label_encoder=False,
    eval_metric='logloss',
    random_state=42
)
xgb_model.fit(X_train, y_train)
```
XGBClassifier merupakan algoritma boosting berbasis gradien yang mengoptimalkan model secara bertahap. Model ini terkenal efisien dalam menangkap pola kompleks dan interaksi antar fitur, sehingga sering digunakan pada kompetisi data science karena akurasinya yang tinggi.

#### Light Gradient Boosting Machine (LGBM)
```python
from lightgbm import LGBMClassifier

lgbm_model = LGBMClassifier(random_state=42, verbose=-1)
lgbm_model.fit(X_train, y_train)
```
LightGBM adalah algoritma boosting yang melakukan pembentukan pohon secara leaf-wise. Keunggulannya terletak pada kecepatan pelatihan, efisiensi memori, serta kemampuan menangani dataset besar dan fitur kategori dalam jumlah banyak.

#### Support Vector Classifier (SVC)
```python
from sklearn.svm import SVC

svc_model = SVC(probability=True, random_state=42)
svc_model.fit(X_train, y_train)
```
[Support Vector Classifier](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html) berusaha mencari optimal hyperplane yang memisahkan dua kelas dengan margin terbesar. SVC cocok untuk dataset dengan batas pemisahan yang kompleks, serta dapat bekerja baik melalui penggunaan kernel non-linear.

#### Gradient Boosting
```python
from sklearn.ensemble import GradientBoostingClassifier

gb_model = GradientBoostingClassifier(random_state=42)
gb_model.fit(X_train, y_train)
```
[Gradient Boosting Classifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.GradientBoostingClassifier.html) merupakan metode boosting yang membangun model secara bertahap dengan meminimalkan kesalahan prediksi menggunakan gradient descent. Model ini efektif dalam menangani hubungan non-linear antar fitur dan mampu memberikan performa prediksi yang baik, meskipun membutuhkan waktu pelatihan lebih lama dibandingkan Random Forest karena sifat pembelajaran yang sekuensial.

Kelima model ini digunakan dengan pengaturan parameter awal sebagai percobaan dasar

- Pada langkah ini, membandingkan kinerja model yang berbeda dengan menggunakan **_stratified k-fold cross validation_** untuk melatih masing-masing model dan mengevaluasi skor ROC-AUC. Stratified k-fold cross validation akan mempertahankan proporsi target pada setiap fold, menangani target yang tidak seimbang.

### 2. Model Comparison
Evaluasi performa model dilakukan menggunakan **K-Fold Cross Validation** dengan metrik **ROC-AUC**, sehingga skor yang ditampilkan merupakan **rata-rata performa generalisasi model** pada berbagai subset data. Berdasarkan hasil pengujian pada tabel, **Logistic Regression** memperoleh nilai ROC-AUC tertinggi sebesar **0.8241**, disusul oleh **SVC** dengan skor **0.8218**. Model berbasis tree boosting seperti **XGBClassifier (0.7965)** dan **LGBMClassifier (0.7978)** menunjukkan performa yang cukup baik, namun masih berada di bawah model linear dan kernel-based. Sementara itu, Random Forest memiliki performa paling rendah (0.7862), yang mengindikasikan bahwa model ini kurang mampu menangkap pola secara optimal dalam dataset ini.
| Model                 | ROC-AUC Score |
|-----------------------|---------------|
| LogisticRegression    | 0.8241        |
| RandomForest          | 0.7862        |
| XGBClassifier         | 0.7965        |
| LGBMClassifier        | 0.7978        |
| SVC                   | 0.8218        |

Model **Logistic Regression** dan **SVC** ini menunjukkan kemampuan paling baik dalam membedakan kelas Attrition, terutama mengingat kondisi dataset yang mengalami ketidakseimbangan kelas. Sehingga, kedua model ini kemudian dilanjutkan ke tahap Hyperparameter Tuning untuk meningkatkan performa prediksi secara lebih optimal.

### 3. Hyperparameter Tuning
Tahap selanjutnya adalah melakukan optimasi hiperparameter untuk meningkatkan performa model terbaik. Dua model yang dipilih berdasarkan hasil K-Fold ROC-AUC sebelumnya, yaitu **Logistic Regression** dan **Support Vector Classifier (SVC)**, dilakukan proses tuning menggunakan **GridSearchCV** dengan skema **Stratified K-Fold (k = 5)**. Pendekatan ini memastikan distribusi kelas tetap seimbang pada setiap fold, sehingga evaluasi performa lebih stabil dan tidak bias terhadap kelas mayoritas.

Proses tuning dilakukan dalam satu pipeline yang mencakup preprocessing data (scaling dan encoding) untuk mencegah kebocoran data (data leakage) antara train dan validation split pada CV.
#### Logistic Regression
Parameter yang dioptimasi:
| Parameter | Pilihan                          | Tujuan                                                  |
| --------- | -------------------------------- | ------------------------------------------------------- |
| `penalty` | `['l1', 'l2']`                   | Memilih jenis regularisasi agar model tidak overfitting |
| `C`       | `[0.001, 0.01, 0.1, 1, 10, 100]` | Mengatur strength regularisasi                          |
| `solver`  | `['liblinear', 'lbfgs']`         | Menyesuaikan solver yang kompatibel dgn penalty         |

Selain itu, digunakan class_weight = ‘balanced’ untuk mengatasi ketidakseimbangan kelas pada target attrition.

Setelah proses pencarian kombinasi terbaik, diperoleh hasil berupa:
- **Best AUC Score :** 0.8280
- **Best Parameters :** {'model__C': 0.1, 'model__penalty': 'l2', 'model__solver': 'liblinear'}

#### Support Vector Classifier (SVC)
Parameter yang dioptimasi:
| Parameter | Pilihan                     | Tujuan                                                 |
| --------- | --------------------------- | ------------------------------------------------------ |
| `C`       | `[0.1, 1, 10, 100]`         | Mengontrol margin dan penalti kesalahan                |
| `kernel`  | `['linear', 'rbf', 'poly']` | Menangkap pola linear maupun non-linear                |
| `gamma`   | `['scale', 'auto', 0.1, 1]` | Mengatur kompleksitas boundary (untuk kernel RBF/Poly) |
| `degree`  | `[2, 3, 4]`                 | Khusus kernel polynomial untuk kontrol derajat polinom |

Penggunaan class_weight = ‘balanced’ juga diterapkan agar prediksi tidak bias terhadap kelas mayoritas.

Setelah proses pencarian selesai, diperoleh:
- **Best AUC Score :** 0.8306
- **Best Parameters :** {'model__C': 1, 'model__degree': 2, 'model__gamma': 'auto', 'model__kernel': 'rbf'}

#### Kesimpulan Tuning
Dengan menerapkan GridSearchCV pada kedua model, diperoleh konfigurasi hiperparameter terbaik yang memberikan performa ROC-AUC maksimal pada data pelatihan ter-validasi. Model terbaik dari tahap ini kemudian akan digunakan pada proses evaluasi akhir terhadap validation/test set untuk memastikan performanya benar-benar generalizable.

### 4. Model Selection
Berdasarkan hasil hyperparameter tuning, model Logistic Regression dan SVC terpilih sebagai model terbaik. Untuk meningkatkan performa lebih lanjut, dilakukan pendekatan **Stacking Ensemble**, yaitu teknik yang menggabungkan beberapa model sehingga prediksi akhir menjadi lebih kuat dan stabil dibandingkan model tunggal.

Pada metode stacking ini, tiga model dijadikan base learners:
- Logistic Regression 
- SVC 
- Gradient Boosting Classifier (sebagai model tambahan berbasis boosting)

Ketiga model tersebut menghasilkan prediksi masing-masing, kemudian digabungkan dan digunakan sebagai input bagi **final estimator**, yaitu Logistic Regression, untuk menghasilkan keputusan akhir. Selain itu, proses pelatihan menggunakan **Stratified K-Fold** untuk menjaga keseimbangan kelas di setiap fold dan menghindari overfitting.

<img width="608" height="252" alt="image" src="https://github.com/user-attachments/assets/25bb59a9-96ca-450a-8caf-44cd1d6d7a38" />
  
Pendekatan ini memungkinkan model untuk saling melengkapi:
- Logistic Regression menangkap pola linear
- SVC dan Gradient Boosting mempelajari pola non-linear dan interaksi fitur

Setelah ketiga model dasar (Logistic Regression, SVC, dan Gradient Boosting) digabungkan dalam arsitektur Stacking Ensemble, model kemudian diuji pada data validasi. Hasil evaluasi menunjukkan bahwa model ini memberikan performa terbaik dibandingkan model individual. Berdasarkan hasil pengujian, diperoleh Accuracy sebesar **0.9521** dan ROC-AUC sebesar **0.9686**, yang menunjukkan bahwa model memiliki kemampuan sangat baik dalam membedakan karyawan yang akan tetap bekerja dan yang berpotensi keluar.

|                   Kelas | Precision | Recall |  F1-Score  | Support |
| ----------------------: | :-------: | :----: | :--------: | ------: |
| **0** (Tidak Attrition) |    0.95   |  0.99  |    0.97    |     262 |
|       **1** (Attrition) |    0.95   |  0.75  |    0.84    |      51 |
|            **Accuracy** |           |        | **0.9521** |     313 |
|           **Macro Avg** |    0.95   |  0.87  |    0.90    |     313 |
|        **Weighted Avg** |    0.95   |  0.95  |    0.95    |     313 |

Selain itu, dari classification report diketahui bahwa kelas mayoritas (Attrition = 0) memiliki **recall sangat tinggi sebesar 0.99**, menandakan model hampir selalu benar dalam mengidentifikasi karyawan yang bertahan. Pada kelas minoritas (Attrition = 1), model menghasilkan **precision 0.95 dan recall 0.75**, yang menunjukkan bahwa meskipun kasus resign lebih sulit diprediksi karena ketidakseimbangan data, model tetap cukup efektif mendeteksinya. Nilai **macro average F1-score sebesar 0.90** dan **weighted average F1-score sebesar 0.95** semakin menegaskan bahwa pendekatan stacking mampu mengatasi tantangan imbalance dan memberikan generalisasi yang baik.

## Model Testing and Evaluation
Setelah dilakukan pelatihan dan validasi model, model terbaik yaitu **Stacking Ensemble** diaplikasikan pada data test untuk menghasilkan prediksi risiko attrition setiap karyawan. Karena data test tidak memiliki label sebenarnya (y_test), maka evaluasi performa hanya mengacu pada hasil validasi. Output pada tahap ini berupa probabilitas terjadinya attrition pada tiap observasi di test set, yang kemudian disimpan sebagai file submission untuk keperluan analisis lanjutan atau penilaian eksternal.

## Interpretasi 
Hasil prediksi pada data test disajikan dalam bentuk probabilitas Attrition untuk setiap karyawan, yang menunjukkan seberapa besar kemungkinan individu tersebut akan keluar dari perusahaan. Nilai ini berada pada rentang 0 hingga 1, di mana semakin mendekati 1 berarti risiko karyawan untuk resign semakin tinggi. Misalnya, seorang karyawan dengan probabilitas 0.15 memiliki peluang sekitar 15% untuk keluar dari perusahaan, sementara nilai sangat kecil seperti 0.03 menunjukkan kemungkinan yang sangat rendah untuk attrition.

| id    | Attrition    |
|-------|--------------|
| CM617	| 0.137938663  |
| PJ010	| 0.030281862  |
| GJ831	| 0.151824062  |
| JD352	| 0.032564569  |
| :    	| :            |
| MQ920	| 0.031431933  |

Karena data test tidak memiliki label sebenarnya, hasil prediksi ini sepenuhnya menggambarkan estimasi risiko berdasarkan pola yang telah dipelajari model dari data training. Probabilitas tersebut dapat digunakan oleh perusahaan sebagai dasar pengambilan keputusan strategis, seperti menentukan prioritas dalam program retensi atau memberikan perhatian khusus pada karyawan dengan tingkat risiko yang lebih tinggi. Apabila diperlukan klasifikasi biner (keluar atau tidak), maka nilai probabilitas dapat dikonversi menjadi label dengan menetapkan threshold tertentu, misalnya 0.30, sehingga karyawan dengan probabilitas di atas ambang tersebut dikategorikan berpotensi tinggi untuk keluar.
