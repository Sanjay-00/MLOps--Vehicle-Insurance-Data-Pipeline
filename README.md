# 🚗 Vehicle Insurance Prediction - MLOps End-to-End Project

Welcome to the **Vehicle Insurance Prediction MLOps Project**! This project is built to impress—crafted using production-ready architecture and cutting-edge tools like **Docker, GitHub Actions, MongoDB, AWS ECR/EC2/S3**, and more. It's designed to show your MLOps knowledge in action—from development to deployment.

---

## 🌟 Key Highlights

- ✅ Fully Modular Architecture
- 🔁 End-to-End MLOps Lifecycle
- ☁️ Cloud-Integrated: MongoDB Atlas, AWS S3, EC2, ECR
- 📦 CI/CD: GitHub Actions + Self-hosted EC2 Runner
- 📈 ML Workflow: Data Ingestion to Model Deployment
- 📊 Realtime Prediction Interface using FastAPI
- 🐳 Dockerized Deployment to AWS EC2

---

## 🛠️ Project Setup Steps

### 1. 🚧 Project Initialization
- Run `template.py` to create the project folder structure.
- Add local package import support using `setup.py` and `pyproject.toml`. ([Learn more](./crashcourse.txt))

```bash
conda create -n vehicle python=3.10 -y
conda activate vehicle
pip install -r requirements.txt
pip list  # To verify packages
```

---

## 📦 MongoDB Setup (Atlas)
1. Sign up at MongoDB Atlas and create a new project.
2. Create a free M0 cluster.
3. Set up username & password, allow IP access (0.0.0.0/0).
4. Copy connection string (Python 3.6+ driver).
5. Create `notebook/mongoDB_demo.ipynb` and push dataset to MongoDB.
6. Verify upload on Atlas → Database → Browse Collection.

---

## 🧪 Logging, Exception Handling, and EDA
- Logger and exception handler implemented and tested via `demo.py`.
- Performed EDA and feature engineering notebooks added to `notebooks/`.

---

## 📥 Data Ingestion Module
- MongoDB connection setup in `configuration.mongo_db_connection.py`
- Create pipeline entities (`config_entity.py`, `artifact_entity.py`)
- Fetch and transform data using `data_access/proj1_data.py`
- Setup ingestion in `components.data_ingestion.py`
- Environment variable setup:

```bash
# For bash
export MONGODB_URL="your_mongodb_connection_string"
echo $MONGODB_URL

# For Windows
setx MONGODB_URL "your_mongodb_connection_string"
```

---

## ✅ Data Validation, Transformation, and Model Training
- Schema defined in `config/schema.yaml`
- Validation logic in `components.data_validation.py`
- Transformation logic in `components.data_transformation.py`
- Model training in `components/model_trainer.py`

---

## ☁️ AWS Setup for Model Registry
1. Create IAM user (`firstproj`) with `AdministratorAccess`.
2. Create and download Access Keys (store securely).
3. Set environment variables:

```bash
export AWS_ACCESS_KEY_ID="..."
export AWS_SECRET_ACCESS_KEY="..."
```

4. Create S3 Bucket: `my-model-mlopsproj` (region: us-east-1).
5. Configure S3 connection in `configuration.aws_connection.py`.
6. Define constants:
```python
MODEL_EVALUATION_CHANGED_THRESHOLD_SCORE = 0.02
MODEL_BUCKET_NAME = "my-model-mlopsproj"
MODEL_PUSHER_S3_KEY = "model-registry"
```

---

## 📊 Model Evaluation and Model Pusher
- Evaluate performance against previous model.
- Push trained model to AWS S3.

---

## 🧠 Prediction Pipeline + FastAPI
1. Create `app.py` for FastAPI interface.
2. Add `static/` and `template/` folders for web UI.

---

## 🔄 CI/CD Pipeline (Docker + GitHub Actions + EC2)
### Step-by-step:
1. Create Dockerfile and `.dockerignore`
2. Create `.github/workflows/aws.yaml`
3. Create IAM user: `usvisa-user`, download credentials
4. Create ECR repository: `vehicleproj`
5. Launch EC2 Ubuntu Instance: `vehicledata-machine`
6. Install Docker on EC2:
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
newgrp docker
```
7. Setup GitHub self-hosted runner on EC2.
8. Add GitHub secrets:
   - `AWS_ACCESS_KEY_ID`
   - `AWS_SECRET_ACCESS_KEY`
   - `AWS_DEFAULT_REGION`
   - `ECR_REPO`

9. Open EC2 port 5080:
   - EC2 > Security Group > Inbound Rules > Add TCP rule for port 5080

---

## 🚀 Deploy & Access
- Push code to GitHub → triggers CI/CD
- Docker image is built, pushed to ECR
- EC2 pulls image and runs container
- Access app at `http://<EC2-PUBLIC-IP>:5080`
- Trigger training at: `http://<EC2-PUBLIC-IP>:5080/training`

---

## 📁 Directory Structure (Simplified)
```bash
├── src/
│   ├── components/
│   ├── configuration/
│   ├── data_access/
│   ├── entity/
│   ├── pipeline/
│   ├── aws_storage/
├── templates/
├── static/
├── notebooks/
├── app.py
├── demo.py
├── Dockerfile
├── .github/workflows/aws.yaml
├── setup.py
├── pyproject.toml
├── requirements.txt
```

---

## 🙌 Final Notes
This project demonstrates an industry-grade, modular MLOps pipeline—covering every stage from raw data to deployment with monitoring, versioning, and CI/CD integration. Perfect for showcasing your full-stack ML/DevOps capabilities!

---

> ⚡ Made with passion, Python, and a lot of Docker containers.

---

Ready to deploy or learn? Fork it, run it, or make it your own. 💡

