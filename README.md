# 🚗 Vehicle Insurance Prediction – End-to-End MLOps Project

Welcome to the **Vehicle Insurance Prediction MLOps Project**! This project is designed to **predict whether a customer will opt for vehicle insurance** based on their personal and vehicle-related details using a machine learning model.

In addition to delivering accurate predictions, this project is built to **impress recruiters, engineers, and MLOps enthusiasts** by showcasing an **end-to-end production-grade ML pipeline**. Leveraging tools like **Docker, GitHub Actions, MongoDB Atlas, AWS (ECR, EC2, S3), and FastAPI**, the project seamlessly covers everything from data ingestion to real-time deployment.

---


---

## 🌟 Key Highlights

✅ **Modular Architecture**  
🔁 **Complete MLOps Lifecycle**  
☁️ **Cloud Integration**: MongoDB Atlas, AWS S3, EC2, ECR  
🚀 **CI/CD Automation**: GitHub Actions + Self-hosted EC2 Runner  
📊 **ML Pipeline**: Ingestion → Validation → Transformation → Training → Evaluation → Deployment  
⚡ **FastAPI Interface**: Realtime prediction UI  
🐳 **Dockerized Deployment** on AWS EC2  

---

## 🏗️ Project Setup

### 1️⃣ Environment Setup

```bash
python -m venv vehicle-env
source vehicle-env/bin/activate 
pip install -r requirements.txt
pip list  # Verify installed packages
```

- Create the project folder structure.
- Add local package support using `setup.py` and `pyproject.toml`. 

---

## 📦 MongoDB Atlas Integration

1. Create a **MongoDB Atlas** account.
2. Launch a free **M0 cluster**.
3. Add your **username/password** and set IP access to `0.0.0.0/0`.
4. Copy your **Python connection string**.
5. Create and run `notebooks/mongoDB_demo.ipynb` to upload your dataset.
6. Confirm upload via: *Clusters → Collections → Browse Data*.

---

## 🧪 Logging, Exception Handling, & EDA

- Implemented custom logging and exception handling (`demo.py`)
- Performed **Exploratory Data Analysis (EDA)** and feature engineering in `notebooks/`

---

## 📅 Data Ingestion

🔹 MongoDB connection handled in: `configuration/mongo_db_connection.py`  
🔹 Created pipeline entities:  
   - `entity/config_entity.py`  
   - `entity/artifact_entity.py`  
🔹 Used `data_access/proj1_data.py` for DB read/write  
🔹 Ingestion logic in: `components/data_ingestion.py`

#### 🔐 Environment Variables Setup

```bash
# Linux/macOS
export MONGODB_URL="your_connection_string"
echo $MONGODB_URL

# Windows
setx MONGODB_URL "your_connection_string"
```

---

## ✅ Data Validation, Transformation & Model Training

📁 Schema: `config/schema.yaml`  
📌 Modules:  
- Validation: `components/data_validation.py`  
- Transformation: `components/data_transformation.py`  
- Model Training: `components/model_trainer.py`

---

## ☁️ AWS S3 for Model Registry

1. Create an **IAM User** (`firstproj`) with `AdministratorAccess`.
2. Download and securely store **Access Keys**.
3. Export keys as environment variables:

```bash
export AWS_ACCESS_KEY_ID="your_key"
export AWS_SECRET_ACCESS_KEY="your_secret"
```

4. Create S3 Bucket: `my-model-mlopsproj` (region: `us-east-1`)
5. Setup S3 connection in: `configuration/aws_connection.py`

📌 Constants to define:

```python
MODEL_EVALUATION_CHANGED_THRESHOLD_SCORE = 0.02
MODEL_BUCKET_NAME = "my-model-mlopsproj"
MODEL_PUSHER_S3_KEY = "model-registry"
```

---

## 📊 Model Evaluation & Pushing to S3

- Compares performance with previous model.
- Pushes the best-performing model to AWS S3 for production use.

---

## 🧠 FastAPI-based Realtime Prediction

1. Build FastAPI interface in `app.py`
2. Create web UI using:

```
templates/
static/
```

- Endpoints:
  - `/`: Home Page
  - `/predict`: Make predictions
  - `/training`: Trigger training

---

## 🔄 CI/CD Pipeline – Docker, GitHub Actions, EC2

### 🧱 Setup Steps:

1. **Dockerize App**:
   - `Dockerfile`
   - `.dockerignore`

2. **CI/CD Workflow**:
   - `.github/workflows/aws.yaml`

3. **GitHub → AWS Integration**:
   - Goto IAM User
   - Generate Access Keys
   - Add secrets to GitHub:
     - `AWS_ACCESS_KEY_ID`
     - `AWS_SECRET_ACCESS_KEY`
     - `AWS_DEFAULT_REGION`
     - `ECR_REPO`

4. **Create ECR Repo**: `vehicleproj`

5. **Launch EC2 Instance**: `vehicledata-machine` (Ubuntu)

6. **Install Docker on EC2**:

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
newgrp docker
```

7. **Add Self-Hosted Runner on EC2**:
   - Configure using GitHub Actions Settings → Runners

8. **Allow Port 5080 on EC2**:
   - Go to EC2 → Security Groups → Inbound Rules → Add TCP rule for **Port 5080**

---

## 🚀 Deploy & Access

✅ Push code → GitHub Actions triggers CI/CD  
✅ Docker image built → pushed to ECR  
✅ EC2 pulls image → launches container  

🌐 **Access the app** at:  
```bash
http://<EC2-PUBLIC-IP>:5080
```

🔁 **Trigger model training**:  
```bash
http://<EC2-PUBLIC-IP>:5080/training
```

---

---

## 📂 Project Structure (Simplified)

```
├── src/
│   ├── components/               # Core pipeline logic (ingestion, validation, training, etc.)
│   ├── configuration/            # AWS and MongoDB configurations
│   ├── data_access/              # Handles DB CRUD operations
│   ├── entity/                   # Defines structured entities for configs and artifacts
│   ├── pipeline/                 # Training orchestration logic
│   └── aws_storage/              # AWS S3 utility functions
├── templates/                    # FastAPI HTML templates
├── static/                       # CSS/JS assets
├── notebooks/                    # EDA & MongoDB upload demos
├── app.py                        # FastAPI backend server
├── demo.py                       # Logger and error handler test script
├── Dockerfile                    # Docker configuration
├── .github/workflows/aws.yaml   # GitHub Actions CI/CD pipeline
├── setup.py                      # Local module installer
├── pyproject.toml                # Metadata and packaging config
└── requirements.txt              # Python dependencies
```

---

## 🙌 Summary

This project is an **industry-grade, cloud-integrated, CI/CD-enabled ML pipeline** that solves a **real-world classification problem**: predicting whether a user will opt for vehicle insurance. It's a complete ML system packaged with training, monitoring, deployment, and a realtime prediction interface — perfect for showcasing your **ML engineering and MLOps expertise**.

---





