# Credit Score Classifier

Aplikasi Streamlit untuk klasifikasi credit score melalui endpoint AWS SageMaker. Pengguna mengisi profil dan data keuangan, lalu aplikasi menampilkan kategori kredit serta distribusi probabilitas dari respons model.

## Features

- Form profil nasabah, informasi kredit, dan perilaku pembayaran.
- Encoding kategori pekerjaan, credit mix, pembayaran minimum, dan payment behaviour sebelum inferensi.
- Kategori hasil `Good`, `Standard`, atau `Poor`.
- Bar chart probabilitas masing-masing kategori.

## Architecture

```text
Streamlit form → JSON payload → SageMaker Runtime → label + probabilities → UI
```

Inferensi berlangsung di AWS; repository ini berisi client aplikasi, bukan kode training atau artefak model.

## Project structure

| File | Fungsi |
| --- | --- |
| [app_cloud.py](app_cloud.py) | UI, preprocessing input, dan pemanggilan endpoint |
| [requirements.txt](requirements.txt) | Streamlit, Boto3, dan Pandas |

## Getting started

```bash
git clone https://github.com/hoseaoc/-Credit-Score-Classifier.git
cd ./-Credit-Score-Classifier
python -m venv .venv
```

Aktifkan virtual environment (`source .venv/bin/activate` pada macOS/Linux atau `.venv\Scripts\Activate.ps1` pada PowerShell), lalu:

```bash
python -m pip install -r requirements.txt
```

### AWS configuration

Kode saat ini memakai endpoint `credit-score-endpoint` di region `us-east-1`. Endpoint tersebut harus tersedia dan memiliki kontrak input sesuai payload di `app_cloud.py`. Identitas AWS yang digunakan harus diizinkan melakukan `sagemaker:InvokeEndpoint`.

Buat `.streamlit/secrets.toml` secara lokal:

```toml
[aws]
aws_access_key_id = "YOUR_ACCESS_KEY_ID"
aws_secret_access_key = "YOUR_SECRET_ACCESS_KEY"
aws_session_token = "YOUR_SESSION_TOKEN"
```

Gunakan nilai credential milikmu; jangan commit file ini ke GitHub. Ketiga key dibaca langsung oleh kode. Credential sementara perlu diperbarui setelah kedaluwarsa.

Respons endpoint yang diharapkan berbentuk:

```json
{
  "label": "Good",
  "probabilities": {"Good": 0.7, "Standard": 0.2, "Poor": 0.1}
}
```

Contoh di atas hanya menunjukkan format respons, bukan hasil evaluasi model.

```bash
python -m streamlit run app_cloud.py
```

## Scope & limitations

- Tidak dapat melakukan prediksi offline tanpa endpoint SageMaker.
- Dataset, training pipeline, model server, dan metrik evaluasi tidak disertakan.
- Urutan dan encoding input harus cocok dengan model yang di-deploy.
- Proyek ini mendokumentasikan prototipe inferensi; hasilnya bukan keputusan kelayakan kredit.

## Author

[Hosea Christian](https://github.com/hoseaoc) · [Portfolio](https://hosea-christian-portfolio.vercel.app)
