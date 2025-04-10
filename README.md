# 🚗 Vehicle Insurance Prediction – End-to-End MLOps Project

Welcome to the **Vehicle Insurance Prediction MLOps Project**! This project is designed to **impress recruiters, engineers, and MLOps enthusiasts** by showcasing an end-to-end production-grade ML pipeline. Leveraging tools like **Docker, GitHub Actions, MongoDB Atlas, AWS (ECR, EC2, S3), and FastAPI**, the project seamlessly covers everything from data ingestion to real-time deployment.

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
conda create -n vehicle python=3.10 -y
conda activate vehicle
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

## 📂 Project Structure (Simplified)

```
├── src/
│   ├── components/               # Feature modules
│   ├── configuration/            # AWS/Mongo configs
│   ├── data_access/              # MongoDB data fetch
│   ├── entity/                   # Pipeline entities
│   ├── pipeline/                 # Training pipeline
│   └── aws_storage/              # S3 push/pull
├── templates/                    # FastAPI HTML templates
├── static/                       # CSS/JS assets
├── notebooks/                    # EDA & Mongo demo
├── app.py                        # FastAPI app
├── demo.py                       # Logger test
├── Dockerfile                    # Docker config
├── .github/workflows/aws.yaml   # GitHub Actions
├── setup.py                      # Local imports
├── pyproject.toml                # Build meta
└── requirements.txt              # Dependencies
```

---

## 🙌 Summary

This project is an **industry-grade, cloud-integrated, CI/CD-enabled ML pipeline**, perfect for learning or showcasing your **MLOps and ML deployment skills**. Whether you're aiming to understand MLOps architecture or to deploy real-world ML systems, this repo gives you everything from zero to production.

---


