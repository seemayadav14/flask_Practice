# Flask Student Management System

A Flask-based Student Management System with MongoDB integration, Docker containerization, automated testing, and CI/CD deployment using GitHub Actions and AWS.

## 📌 Project Overview

This project is a simple web application built using **Python Flask** that allows users to:

* Add students
* View students
* Update student information
* Delete students
* Check application health
* Store student data in MongoDB

The application is containerized using Docker and can be deployed on AWS EC2 using an image stored in Amazon ECR.

## 🛠️ Technologies Used

* Python 3.12+
* Flask
* MongoDB
* PyMongo / Flask-PyMongo
* HTML / CSS
* Docker
* GitHub Actions
* AWS EC2
* AWS ECR
* AWS IAM
* MongoDB Atlas
* Linux / Ubuntu

## 📂 Project Structure

```text
flask_Practice/
│
├── app.py
├── requirements.txt
├── test_app.py
│
├── templates/
│   ├── base.html
│   ├── index.html
│   ├── add_student.html
│   └── update_student.html
│
├── Dockerfile
├── .dockerignore
├── .env.example
│
├── .github/
│   └── workflows/
│       └── ci-cd.yml
│
├── README.md
└── README.pdf
```

## ⚙️ Application Features

### Student Management

The application provides CRUD functionality:

* **Create** – Add a new student
* **Read** – View all students
* **Update** – Modify student details
* **Delete** – Remove a student

### Health Check

The application provides a health endpoint:

```text
GET /health
```

A healthy response is returned when the Flask application can successfully communicate with MongoDB.

Example:

```json
{
  "status": "healthy"
}
```

## 🔐 Environment Variables

Create a `.env` file locally using `.env.example` as a reference.

Example:

```env
MONGO_URI=mongodb://localhost:27017/student_db
SECRET_KEY=your-secret-key
MONGO_TLS=false
```

For MongoDB Atlas, use your Atlas connection string:

```env
MONGO_URI=mongodb+srv://<username>:<password>@<cluster-url>/<database-name>
```

Do **not** commit real MongoDB credentials, passwords, AWS keys, or other secrets to GitHub.

## 💻 Run Locally

### 1. Clone the Repository

```bash
git clone https://github.com/seemayadav14/flask_Practice.git
cd flask_Practice
```

### 2. Create a Virtual Environment

On Windows PowerShell:

```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

### 3. Install Dependencies

```powershell
pip install -r requirements.txt
```

### 4. Configure Environment Variables

Create a `.env` file and add the required MongoDB connection details.

### 5. Run the Application

```powershell
python app.py
```

The application will be available at:

```text
http://localhost:5000
```

Health check:

```text
http://localhost:5000/health
```

## 🧪 Run Tests

Run the automated tests using:

```powershell
pytest -v
```

The tests verify the application's basic functionality and help ensure that changes do not break existing features.

## 🐳 Docker

### Build Docker Image

```powershell
docker build -t flask-practice .
```

### Run Container

```powershell
docker run -d `
  --name flask-app `
  -p 5000:5000 `
  -e MONGO_URI="YOUR_MONGO_URI" `
  -e SECRET_KEY="YOUR_SECRET_KEY" `
  -e MONGO_TLS="false" `
  flask-practice
```

### Check Running Containers

```powershell
docker ps
```

### View Container Logs

```powershell
docker logs flask-app
```

### Test Health Endpoint

```powershell
curl http://localhost:5000/health
```

### Stop Container

```powershell
docker stop flask-app
```

### Remove Container

```powershell
docker rm flask-app
```

## ☁️ AWS Deployment

The application can be deployed using the following AWS services:

```text
GitHub
   │
   ▼
GitHub Actions
   │
   ├── Run Tests
   │
   ├── Build Docker Image
   │
   └── Push Image
          │
          ▼
       AWS ECR
          │
          ▼
       AWS EC2
          │
          ▼
    Flask Container
          │
          ▼
    MongoDB Atlas
```

### AWS ECR

Create an ECR repository for the application.

Example repository:

```text
flask-practice
```

Authenticate Docker with ECR:

```bash
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin YOUR_AWS_ACCOUNT_ID.dkr.ecr.ap-south-1.amazonaws.com
```

Tag the Docker image:

```bash
docker tag flask-practice:latest YOUR_AWS_ACCOUNT_ID.dkr.ecr.ap-south-1.amazonaws.com/flask-practice:latest
```

Push the image:

```bash
docker push YOUR_AWS_ACCOUNT_ID.dkr.ecr.ap-south-1.amazonaws.com/flask-practice:latest
```

## 🖥️ AWS EC2 Deployment

Connect to the Ubuntu EC2 instance using SSH:

```bash
ssh -i "flask-cicd-key.pem" ubuntu@YOUR_EC2_PUBLIC_IP
```

Authenticate Docker with ECR:

```bash
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin YOUR_AWS_ACCOUNT_ID.dkr.ecr.ap-south-1.amazonaws.com
```

Pull the image:

```bash
docker pull YOUR_AWS_ACCOUNT_ID.dkr.ecr.ap-south-1.amazonaws.com/flask-practice:latest
```

Run the application:

```bash
docker run -d \
  --name flask-app \
  -p 5000:5000 \
  -e MONGO_URI="YOUR_MONGO_URI" \
  -e SECRET_KEY="YOUR_SECRET_KEY" \
  -e MONGO_TLS="false" \
  YOUR_AWS_ACCOUNT_ID.dkr.ecr.ap-south-1.amazonaws.com/flask-practice:latest
```

Check the container:

```bash
docker ps
```

Check application logs:

```bash
docker logs flask-app
```

Test:

```bash
curl http://localhost:5000/health
```

## 🔄 CI/CD Pipeline

GitHub Actions is used to automate the deployment process.

The pipeline performs the following steps:

1. Checkout source code
2. Set up Python
3. Install dependencies
4. Run automated tests
5. Build Docker image
6. Authenticate with AWS
7. Login to Amazon ECR
8. Push Docker image to ECR
9. Deploy the image to EC2
10. Verify the application health

## 🔑 GitHub Secrets

The following secrets can be configured in:

**GitHub → Repository → Settings → Secrets and variables → Actions**

Example secrets:

```text
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
AWS_REGION
ECR_REPOSITORY
EC2_HOST
EC2_USERNAME
EC2_SSH_KEY
MONGO_URI
SECRET_KEY
```

Use GitHub Secrets for sensitive information instead of hard-coding credentials in the workflow.

## 🔒 Security

The following security practices are recommended:

* Never commit `.env` files.
* Never commit AWS access keys.
* Never commit MongoDB passwords.
* Use GitHub Secrets for CI/CD credentials.
* Restrict EC2 security-group access where possible.
* Use IAM permissions according to least privilege.
* Use HTTPS in production.
* Use strong MongoDB credentials.
* Do not expose database credentials in Docker commands, logs, or source code.

## 🩺 Troubleshooting

### Check Docker Container

```bash
docker ps -a
```

### Check Logs

```bash
docker logs flask-app --tail 100
```

### Check Health

```bash
curl -i http://localhost:5000/health
```

### Check Environment Variables

```bash
docker inspect flask-app
```

### Restart Container

```bash
docker restart flask-app
```

### Remove Existing Container

If Docker reports a container-name conflict:

```bash
docker stop flask-app
docker rm flask-app
```

Then run the container again.

## 📊 Expected Deployment

After successful deployment, the application should be accessible through:

```text
http://YOUR_EC2_PUBLIC_IP:5000
```

Health endpoint:

```text
http://YOUR_EC2_PUBLIC_IP:5000/health
```

A successful health check should indicate that the Flask application and MongoDB connection are working correctly.


