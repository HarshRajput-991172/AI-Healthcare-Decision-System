# 🏥 AI Healthcare Decision System

> An AI-powered Clinical Decision Support System (CDSS) for patient survival prediction, built with Streamlit, Scikit-learn, and Groq LLM.

---

## 📌 Overview

The **AI Healthcare Decision System** is an end-to-end machine learning web application designed to assist clinicians and healthcare professionals by predicting a patient's survival probability based on clinical and demographic data. The system is trained on the **SUPPORT2 (Study to Understand Prognoses and Preferences for Outcomes and Risks of Treatments)** dataset and powered by a tuned **Gradient Boosting Classifier**.

> ⚠️ **Disclaimer:** This system is an educational and research prototype. It must **not** replace professional medical judgment, diagnosis, or treatment decisions.

---

## ✨ Key Features

| Feature | Description |
|---|---|
| 🔐 **Secure Login** | Role-based access with admin authentication |
| 🩺 **Patient Survival Prediction** | ML model predicts survival/death probability |
| 🧠 **SHAP Explainability** | Visual explanation of model predictions using SHAP values |
| 🤖 **Groq AI Integration** | AI-generated clinical interpretation, precautions & diet plans |
| 💬 **AI Medical Chatbot** | Context-aware Q&A assistant powered by Groq LLM |
| 📄 **PDF Report Generation** | Downloadable patient report with prediction & AI insights |
| 📚 **Patient History** | MySQL-backed storage, search, and export of patient records |
| 📊 **Analytics Dashboard** | Dataset-level insights including age distribution & death rates |
| 📈 **Model Performance** | Display of accuracy, precision, recall, and F1 score |
| 🚨 **Clinical Alert System** | Automatic flags for abnormal vitals (BP, glucose, temperature, etc.) |
| 🩺 **Health Score** | Composite health score calculated from vital signs |

---

## 🖥️ Application Pages

```
🏠 Home           — System overview and connection status
🩺 Predict Patient — Input patient data and get survival prediction
📚 Patient History — Browse, search, download, and delete records
📊 Analytics       — Dataset-level visualizations
📈 Model Performance — ML model evaluation metrics
ℹ  About          — Project description and technology stack
```

---

## 🧠 ML Pipeline (`main.py`)

The training pipeline covers:

1. **Data Loading** — SUPPORT2 dataset (`support2_dataset.csv`)
2. **Exploratory Data Analysis (EDA)** — Distribution plots, boxplots, correlation heatmap, pair plots
3. **Missing Value Handling** — Median imputation (numerical), mode imputation (categorical)
4. **Feature Engineering** — Label encoding of categorical variables, removal of data-leakage columns
5. **Model Comparison** — 7 classifiers evaluated:
   - Logistic Regression
   - Decision Tree
   - Random Forest
   - Extra Trees
   - Gradient Boosting ✅ *(selected)*
   - AdaBoost
   - Hist Gradient Boosting
6. **Hyperparameter Tuning** — `GridSearchCV` with 5-fold cross-validation
7. **Model Evaluation** — Confusion matrix, ROC curve, AUC, classification report
8. **Model Saving** — Pickled model, label encoders, and feature columns

---

## 📊 Model Performance

| Metric | Score |
|---|---|
| ✅ Accuracy | **92.8%** |
| 🎯 Precision | **91.9%** |
| 📡 Recall | **93.5%** |
| ⚖️ F1 Score | **92.7%** |

---

## 🗂️ Project Structure

```
AI-Healthcare-Decision-System/
│
├── app.py                      # Main Streamlit application
├── main.py                     # ML training pipeline
├── database.py                 # MySQL database operations
├── model_loader.py             # Model loading utility
├── style.css                   # Custom UI styling
│
├── patient_survival_model.pkl  # Trained Gradient Boosting model
├── label_encoders.pkl          # Fitted label encoders
├── feature_columns.pkl         # Feature column order
│
├── support2_dataset.csv        # SUPPORT2 training dataset
├── hospital.db                 # SQLite fallback (local)
├── Patient_Report.pdf          # Last generated patient report
│
├── requirements.txt            # Python dependencies
├── .env                        # Environment variables (not committed)
├── .gitignore
└── README.md
```

---

## ⚙️ Installation & Setup

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/AI-Healthcare-Decision-System.git
cd AI-Healthcare-Decision-System
```

### 2. Create a Virtual Environment

```bash
python -m venv venv

# Windows
venv\Scripts\activate

# macOS / Linux
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure Environment Variables

Create a `.env` file in the project root:

```env
# Groq API Key (required)
GROQ_API_KEY=gsk_your_groq_api_key_here

# MySQL Database (required for patient history)
MYSQL_HOST=localhost
MYSQL_PORT=3306
MYSQL_USER=root
MYSQL_PASSWORD=your_mysql_password
MYSQL_DATABASE=hospital
```

> 💡 Get a free Groq API key at [console.groq.com](https://console.groq.com)

### 5. Set Up MySQL Database

Ensure MySQL is running and create the database:

```sql
CREATE DATABASE hospital;
```

The application will automatically create the `patient_history` table on first launch.

### 6. Run the Application

```bash
streamlit run app.py
```

Open your browser at **http://localhost:8501**

---

## 🔐 Default Login Credentials

| Field | Value |
|---|---|
| Username | `admin` |
| Password | `admin123` |

> ⚠️ Change these credentials before deploying to production.

---

## 🧪 Input Features

The prediction form accepts the following patient data:

| Category | Fields |
|---|---|
| **Patient Info** | Name, Age, Sex, Race, Income, Education, Comorbidities |
| **Disease Info** | Disease Group, Disease Class, Cancer, Diabetes, Dementia, DNR status |
| **Vital Signs** | Mean Blood Pressure, Heart Rate, Respiratory Rate, Temperature, WBC |
| **Lab Tests** | PaO2/FiO2, Albumin, Bilirubin, Creatinine, Sodium, pH, Glucose, BUN, Urine Output |
| **Clinical Scores** | APS, SPS, APACHE/TISS, Coma Score, Hospital Days, Hospital Death |
| **Functional Status** | ADL Physical, ADL Score, ADL Change Score, Functional Status (SFDM2) |

---

## 🤖 AI Features (Groq LLM)

After a prediction is made, the system can generate:

- **Clinical Interpretation** — Plain-language explanation of prediction results and vital signs
- **Safety Precautions** — 8 monitoring/safety recommendations tailored to the patient
- **Diet Plan** — General one-day meal plan example considering the patient's condition

Additionally, the **AI Medical Chatbot** lets clinicians ask free-form questions about the patient in context.

> All AI outputs explicitly state they are **not diagnoses** and recommend professional medical review.

---

## 🚨 Clinical Alert Thresholds

| Parameter | Alert Condition |
|---|---|
| Mean Blood Pressure | `< 60 mmHg` → 🔴 Low BP |
| Blood Glucose | `> 250 mg/dL` → 🔴 Hyperglycemia |
| Temperature | `> 39 C` → 🔴 High Fever |
| Respiratory Rate | `> 30 /min` → 🔴 Tachypnea |
| WBC Count | `> 15 x10^3/uL` → 🟡 Elevated |

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | Streamlit |
| **ML Framework** | Scikit-learn |
| **Explainability** | SHAP |
| **AI / LLM** | Groq API |
| **Visualization** | Plotly, Matplotlib, Seaborn |
| **Database** | MySQL (`mysql-connector-python`) |
| **PDF Generation** | ReportLab |
| **Data Processing** | Pandas, NumPy |
| **Environment** | python-dotenv |

---

## 📦 Dependencies

```
streamlit
pandas
numpy
scikit-learn
plotly
matplotlib
shap
reportlab
Pillow
sqlalchemy
streamlit-option-menu
streamlit-lottie
streamlit-extras
python-dotenv
joblib
openpyxl
seaborn
mysql-connector-python
groq
```

Install all with:
```bash
pip install -r requirements.txt
```

---

## ☁️ Deploying to Streamlit Community Cloud

1. Push your code to a GitHub repository (ensure `.env` is in `.gitignore`).
2. Go to [share.streamlit.io](https://share.streamlit.io) and create a new app.
3. Set **Secrets** in the Streamlit dashboard (replaces `.env`):

```toml
GROQ_API_KEY = "gsk_your_key"
MYSQL_HOST = "your_host"
MYSQL_PORT = "3306"
MYSQL_USER = "your_user"
MYSQL_PASSWORD = "your_password"
MYSQL_DATABASE = "hospital"
```

---

## 📋 Dataset

**SUPPORT2 — Study to Understand Prognoses and Preferences for Outcomes and Risks of Treatments**

- Source: Publicly available clinical research dataset
- Records: ~9,000+ critically ill hospitalized patients
- Target variable: `death` (binary — 0: survived, 1: died)
- Features: Demographics, vital signs, lab values, clinical scores, functional status

Leakage columns removed during training: `surv2m`, `surv6m`, `prg2m`, `prg6m`, `dnrday`, `totmcst`, `totcst`, `charges`

---

## 🔒 Security Notes

- The `.env` file is excluded from version control via `.gitignore`
- The default admin credentials (`admin` / `admin123`) should be updated before production deployment
- All AI outputs are clearly labeled as non-diagnostic educational content

---

## 🤝 Contributing

Contributions are welcome! Please open an issue or submit a pull request.

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m 'Add your feature'`
4. Push to the branch: `git push origin feature/your-feature`
5. Open a Pull Request

---

## 📄 License

This project is for **educational and research purposes only**.

---

## 👩‍💻 Author

Developed as part of a Machine Learning & Generative AI project.

> *"Empowering clinical decisions with AI — responsibly."*
