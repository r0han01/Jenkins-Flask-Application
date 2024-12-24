# Jenkins Flask Application

This is a simple Flask web application that displays a "Hello from Jenkins!" message. The app is containerized using Docker and can be easily deployed in Jenkins pipelines.

## Project Structure
```
project_folder/
├── app.py              # Flask application
├── Dockerfile          # Docker configuration
├── requirements.txt    # Python dependencies
└── templates/          # HTML files
    └── index.html      # Main HTML file
```
## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/jenkins-flask-app.git
cd jenkins-flask-app
```
2. Build the Docker Image
```bash
docker build -t flask-jenkins-app .
```
3. Run the Flask App
```bash
docker run -p 9000:9000 flask-jenkins-app
```
`Access the app at http://localhost:9000.`
