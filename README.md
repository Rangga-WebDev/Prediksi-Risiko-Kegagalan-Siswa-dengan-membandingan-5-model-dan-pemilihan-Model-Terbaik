# Sistem Prediksi Risiko Kegagalan Siswa

## Deskripsi Proyek
Pipeline ini dirancang untuk mendeteksi dini siswa SMA berisiko gagal akademik dengan memadukan predictive modeling dan causal inference, serta memastikan intervensi yang adil dan dapat dijelaskan.

## Struktur Repository
- `Dataset/` – data mentah, hasil preprocessing, dataset split, prediksi tes.  
- `DatasetAsli/` – dataset original dari UCI (Math & Portuguese).  
- `models/` – model terlatih, scaler, encoder, artefak causal, summary.  
- `notebooks/` – Notebook part1 hingga part3 (data understanding → modeling → evaluation).  
- `.github/` – aturan keamanan Snyk dan workflow CI/CD.  
- `requirements.txt` – dependensi environment reproducible.  

## Data & Fitur
- **Sumber**: UCI Student Performance (nilai G1–G3, absensi, dukungan keluarga).  
- **Jenis data**: numerik (nilai, absensi, studytime), kategorikal (dukungan, gender, alamat), teks (alasan sekolah, pekerjaan orang tua).  
- **Fitur utama**: progression rate, total dukungan, lifestyle_score, family_stability, binary flags (high_absences, low_studytime).  
- **Target**: `risiko_gagal` (1 jika G3 < 10).  

## Pipeline
1. **Data Understanding & Preparation**
   - Gabungkan Math & Portuguese, bersihkan duplikat, tangani missing/outlier, engineering fitur.
   - Encoding kategorikal (binary, label, target), scaling dengan StandardScaler.
   - Simpan artifacts (scaler.pkl, encoders, feature_cols, dataset split).  
2. **Predictive Modeling**
   - Latih Logistic Regression, Decision Tree, Random Forest, XGBoost, LightGBM.
   - Evaluasi via AUC-ROC ≥ 0.85, Precision@80% Recall ≥ 0.70, confusion matrix, calibration.
   - Pilih model terbaik, simpan model + prediksi (best_predictive_model.pkl).  
3. **Causal Inference**
   - Definisikan treatment (dukungan sekolah/internet/keluarga), estimasi ATE via DR Learner, CATE via Causal Forest.
   - Identifikasi siswa high-benefit, simulasi alokasi budget (random vs optimal vs high-risk).
   - Simpan hasil causal (`causal_forest.pkl`, `ate_summary.json`, `causal_results.csv`).  
4. **Evaluation & Interpretability**
   - Metrics: AUC, precision, recall, F1, Brier score, calibration curve, fairness audit (gender/address).
   - SHAP untuk interpretasi global & individual, simpan sample values.

## Deployment & Operasional
- **Interface**: Streamlit dashboard + FastAPI prediction/explain endpoints (perlu ditambahkan).
- **Artifacts**: model + encoders + scaler di `models/`.
- **Monitoring**: observability stack untuk AUC, fairness gap, latency; CI via GitHub Actions.
- **Deployment**: containerize ke Azure App Service / Docker dengan environmental secrets di Key Vault.

## Cara Menjalankan
```bash
python -m venv .venv
.venv\Scripts\activate
pip install --upgrade pip
pip install -r requirements.txt

**AGUNG ADI RANGGA**
