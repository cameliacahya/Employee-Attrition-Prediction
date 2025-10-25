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
Deteksi outlier dilakukan menggunakan metode Interquartile Range (IQR) untuk semua fitur numerik. Hasil analisis menunjukkan bahwa beberapa fitur memiliki jumlah outlier yang cukup signifikan, seperti MonthlyIncome (86 outlier), NumCompaniesWorked (36 outlier), PerformanceRating (185 outlier), StockOptionLevel (66 outlier), TotalWorkingYears (52 outlier), TrainingTimesLastYear (174 outlier), YearsAtCompany (52 outlier), YearsInCurrentRole (16 outlier), YearsSinceLastPromotion (85 outlier), dan YearsWithCurrManager (10 outlier). Keberadaan outlier pada fitur-fitur tersebut mengindikasikan adanya variasi ekstrem dalam karakteristik karyawan, misalnya dalam hal pendapatan, pengalaman kerja, atau frekuensi promosi dan pelatihan.
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
