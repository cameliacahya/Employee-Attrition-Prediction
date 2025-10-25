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
Dataset yang digunakan dalam proyek ini berasal dari kompetisi [Kaggle](https://www.kaggle.com/competitions/tugas-1-sml-a-2025). Dataset ini berisi profil karyawan serta informasi lingkungan kerja yang berkaitan dengan kemungkinan seorang karyawan keluar dari perusahaan (attrition). Dataset mencakup 1.470 observasi karyawan dengan 35 fitur.
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
| `JobRole`                 |  Posisi/jabatan spesifik karyawan (contoh: `Sales Executive`, `Research Scientist`).                  | `object`     |
| `JobSatisfaction`         | Tingkat kepuasan pekerjaan: 1 = Low, 2 = Medium, 3 = High, 4 = Very High.                             | `int64`      |
| `MaritalStatus`           | Status pernikahan karyawan (`Single`, `Married`, `Divorced`).                                         | `object`     |
| `MonthlyIncome`           | Gaji bulanan karyawan.                                                                                | `int64`      |
| `MonthlyRate`             | Tarif bulanan karyawan.                                                                               | `int64`      |
| `NumCompaniesWorked`      | Jumlah perusahaan tempat karyawan pernah bekerja sebelumnya.                                          | `int64`      |
| `Over18`                  | Status usia di atas 18 tahun (selalu `Y` dalam dataset).                                              | `object`     |
| `OverTime`                |  Apakah karyawan sering lembur (`Yes`/`No`).                                                          | `object`     |
| `PercentSalaryHike`       | Persentase kenaikan gaji tahunan terakhir.                                                            | `int64`      |
| `RelationshipSatisfaction`| Tingkat kepuasan terhadap hubungan kerja: 1 = Low, 2 = Medium, 3 = High, 4 = Very High.               | `int64`      |
| `StandardHours`           | Jam kerja standar (selalu 80 dalam dataset).                                                          | `int64`      |
| `StockOptionLevel`        | Level kepemilikan saham perusahaan.                                                                   | `int64`      |
| `TotalWorkingYears`       |  Total tahun pengalaman kerja.                                                                        | `int64`      |
| `TrainingTimesLastYear`   | Jumlah pelatihan yang diikuti dalam setahun terakhir.                                                 | `int64`      |
| `WorkLifeBalance`         | Tingkat keseimbangan kerja–hidup: 1 = Bad, 2 = Good, 3 = Better, 4 = Best.                            | `object`     |
| `YearsAtCompany`          | Total tahun bekerja di perusahaan saat ini.                                                           | `int64`      |
| `YearsInCurrentRole`      | Total tahun di posisi/jabatan saat ini.                                                               | `int64`      |
| `YearsSinceLastPromotion` | Tahun sejak promosi terakhir.                                                                         | `int64`      |
| `YearsWithCurrManager`    | Tahun bekerja dengan manajer saat ini.                                                                | `int64`      |
| `Attrition`               | Target: apakah karyawan keluar dari perusahaan (1 = `Yes`/ 0 = `No`).                                 | `int64`     |
