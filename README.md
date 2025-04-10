# 🚗 Vehicle Insurance Prediction – End-to-End MLOps Project

Welcome to the **Vehicle Insurance Prediction MLOps Project**! This project is designed to **predict whether a customer will opt for vehicle insurance** based on their personal and vehicle-related details using a machine learning model.

In addition to delivering accurate predictions, this project is built to **impress recruiters, engineers, and MLOps enthusiasts** by showcasing an **end-to-end production-grade ML pipeline**. Leveraging tools like **Docker, GitHub Actions, MongoDB Atlas, AWS (ECR, EC2, S3), and FastAPI**, the project seamlessly covers everything from data ingestion to real-time deployment.

---

## 🌟 Key Highlights

✅ **ML Problem Solving**: Predict customer vehicle insurance uptake based on input features like age, gender, region, vehicle age, and driving license history.  
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

- Implemented **custom logging** to track pipeline behavior and trace errors (`demo.py`)  
- Developed **exception handling** for robust debugging  
- Performed **Exploratory Data Analysis (EDA)** in Jupyter notebooks to:
  - Understand feature distributions
  - Engineer new features
  - Visualize class imbalance

---

## 📅 Data Ingestion

🔹 MongoDB connection handled in: `configuration/mongo_db_connection.py`  
🔹 Pipeline entities created:
   - `entity/config_entity.py`: Configuration schemas used throughout the pipeline
   - `entity/artifact_entity.py`: Standardized structure for artifacts produced at each pipeline stage
🔹 Data access logic:
   - `data_access/proj1_data.py`: Handles reading from and writing to MongoDB
🔹 Ingestion logic:
   - `components/data_ingestion.py`: Downloads data from MongoDB and stores it locally for further use

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

📁 Schema file: `config/schema.yaml` – defines expected columns and datatypes

📌 Core Components:

- `components/data_validation.py`: Validates the dataset schema and structure against the YAML configuration
- `components/data_transformation.py`: Encodes categorical variables, handles missing values, and scales numeric features
- `components/model_trainer.py`: Trains a machine learning classifier (e.g., RandomForest or XGBoost) using transformed data and saves the model

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

- `components/model_evaluation.py`: Compares current trained model against the existing production model (if any) using evaluation metrics like accuracy or AUC.
- If model performance exceeds threshold improvement, the new model is uploaded to AWS S3 using `components/model_pusher.py`.

---

## 🧠 FastAPI-based Realtime Prediction

1. `app.py`: Implements a FastAPI backend to expose the model via REST API
2. UI Components:
   - `templates/`: Jinja HTML files for frontend
   - `static/`: CSS and JavaScript assets

📌 Routes:
- `/`: Renders the home page with form inputs
- `/predict`: Receives input and returns predicted class (1: Will opt for insurance, 0: Will not)
- `/training`: Allows retraining the pipeline on demand

---

## 🔄 CI/CD Pipeline – Docker, GitHub Actions, EC2

### 🧱 Setup Steps:

1. **Dockerize App**:
   - `Dockerfile`: Defines how to containerize the FastAPI app
   - `.dockerignore`: Ignores unnecessary files during Docker build

2. **CI/CD Workflow**:
   - `.github/workflows/aws.yaml`: Automates Docker build, test, and push to ECR

3. **GitHub → AWS Integration**:
   - IAM User → Generate Access Keys
   - GitHub Secrets:
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
   - Configure using GitHub Actions → Settings → Runners

8. **Allow Port 5080 on EC2**:
   - Security Groups → Inbound Rules → Add TCP rule for Port 5080

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

