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
git clone https://github.com/r0han01/Jenkins-Flask-Application.git
cd Jenkins-Flask-Application
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

# Jenkins Setup on EC2 Instance

This guide walks you through setting up Jenkins on an EC2 instance, troubleshooting common issues, and accessing the Jenkins dashboard.

## Prerequisites

- AWS EC2 instance running Ubuntu.
- A valid PEM key to access the instance.
- Basic knowledge of working with the Linux terminal.

---

## **Step 1: Launch EC2 Instance**

1. **Log into AWS Management Console** and launch a new EC2 instance (e.g., t2.micro).
2. Ensure that the **Security Group** allows access to Jenkins on port `8080` (or your custom port).
3. **Download the PEM key** associated with your instance to connect via SSH.

---

## **Step 2: Connect to the EC2 Instance**

To connect to your EC2 instance, follow these steps:

1. **Locate the PEM Key** file:
ECE.pem

2. **Set the correct permissions** for the PEM file:
```bash
chmod 400 ECE.pem
```
## SSH into the EC2 instance:
```bash
ssh -i "ECE.pem" ubuntu@<Public-DNS-Name>
```
Example:
```bash
ssh -i "ECE.pem" ubuntu@ec2-3-6-160-36.ap-south-1.compute.amazonaws.com
```
### Step 3: Install Java and Jenkins
Jenkins requires Java 11 or Java 17. Follow these commands to install them:

Update package list:

```bash
sudo apt update
```
- Install Java 17 (or Java 11):

```bash
sudo apt install openjdk-17-jre
```
- Verify Java installation:

```bash
java -version
```
- Add the Jenkins repository and GPG key:

```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```
### Update and install Jenkins:

```bash
sudo apt-get update
sudo apt-get install jenkins
```
### Step 4: Start Jenkins
- Start Jenkins service:

```bash
sudo systemctl start jenkins
```
- Check Jenkins service status:

```bash
sudo systemctl status jenkins
```
- If Jenkins does not start, troubleshoot by checking the logs:

```bash
sudo journalctl -xeu jenkins.service
```
### Step 5: Access Jenkins
- Once Jenkins is running, access it through your browser using the EC2 public IP and port 8080 (or custom port if changed).

Example:

```vbnet
http://<Public-IP>:8080
```
### Step 6: Retrieve the Jenkins Administrator Password
- During the initial setup of Jenkins, you will be prompted to provide an administrator password. This password is stored in the file /var/lib/jenkins/secrets/initialAdminPassword on your EC2 instance.

- SSH into the EC2 instance.

- Retrieve the password by running:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
- Copy the password and paste it into the Jenkins setup page to unlock Jenkins.

### Step 7: Configure Jenkins
- Install recommended plugins during the setup.
- Create the first admin user as part of the setup process.
- Complete the setup and start using Jenkins!
## Troubleshooting
- If Jenkins is not starting, consider the following:

- Check permissions on Jenkins files:

```bash
sudo chown -R jenkins:jenkins /var/lib/jenkins
sudo chown -R jenkins:jenkins /var/cache/jenkins
sudo chown -R jenkins:jenkins /var/log/jenkins
```
- Check for port conflicts (default port is 8080):

```bash
sudo netstat -tuln | grep 8080
```
- If needed, change the port in the Jenkins configuration:

```bash
sudo nano /etc/default/jenkins
```
- Modify the HTTP_PORT variable to your desired port:

```bash
HTTP_PORT=8090
```
- Restart Jenkins after changes:

```bash
sudo systemctl restart jenkins
```
### Security Group Configuration
##### Ensure that the Security Group attached to your EC2 instance allows inbound traffic on port 8080 (or your custom port).

- Go to AWS EC2 Console.
- Select the Security Group linked to your instance.
- Edit Inbound Rules:
- Type: Custom TCP Rule
- Port Range: 8080 (or your custom port)
- Source: My IP (for security purposes)
- Save the rule and try accessing Jenkins again.

####### After following these steps, Jenkins should be up and running on your EC2 instance. You can start configuring Jenkins jobs and pipelines for your CI/CD needs!

- For further details, refer to the official Jenkins documentation.

```markdown

---

### **How to Use this Markdown:**
1. **Copy** the content above.
2. **Create a new file** in your project directory named `README.md`.
3. **Paste** the content into this file.
4. **Commit** the changes to your GitHub repository.
```


### Step 8: Create Jenkins Administrator User
##### After accessing Jenkins for the first time, you will be prompted to create a new administrator user. This step is required to complete the Jenkins setup process.

- Provide the following details to create the user:

- Username: Choose a username for the administrator account (e.g., admin).
- Password: Choose a secure password for the administrator account.
- Full Name: Enter the full name of the administrator (e.g., Jenkins Admin).
- Email Address: Enter the email address associated with the administrator (e.g., admin@example.com).
- Click "Save and Finish" to complete the setup.

You can now access Jenkins and start configuring jobs and pipelines as an administrator.


### 1. Verify your local Jenkins image
- Run the following command to check if the Jenkins image exists on your local system:

```bash
docker images
```
This will list all available images. Look for your Jenkins image (it should show something like jenkins in the REPOSITORY column). If you don't see it, you will need to build it again.

### 2. Build your Jenkins image (if it's not available)
- If the Jenkins image doesn't exist locally, you will need to build it. Assuming you have a Dockerfile for your Jenkins image in your current directory, you can build the image using:

```bash
docker build -t jenkins .
```
- This command will build the image using the Dockerfile in the current directory (.) and tag it as jenkins.

### 3. Run the Jenkins container
- Once the image is built (or if it's already available), you can run it:

```bash
docker run --name jenkins -d -p 9000:9000 jenkins
```
This command will run your custom Jenkins container and map port 9000 on your machine to port 9000 in the container.

### 4. Verify the container is running
- You can check if your Jenkins container is running with:

```bash
docker ps
```
- This will list all running containers. You should see the jenkins container listed there with the port 9000 exposed.

### Step 9: Running Jenkins with Docker (Optional)
- If you're using Docker to run Jenkins, here’s how to verify, build, and run the Jenkins image:

### 1. Verify your local Jenkins image
- Run the following command to check if the Jenkins image exists on your local system:

```bash
docker images
```
- This will list all available images. Look for your Jenkins image (it should show something like jenkins in the REPOSITORY column). If you don't see it, you will need to build it again.

### 2. Build your Jenkins image (if it's not available)
- If the Jenkins image doesn't exist locally, you will need to build it. Assuming you have a Dockerfile for your Jenkins image in your current directory, you can build the image using:

```bash
docker build -t jenkins .
```
- This command will build the image using the Dockerfile in the current directory (.) and tag it as jenkins.

### 3. Run the Jenkins container
- Once the image is built (or if it's already available), you can run it:

```bash
docker run --name jenkins -d -p 9000:9000 jenkins
```
- This command will run your custom Jenkins container and map port 9000 on your machine to port 9000 in the container.

### 4. Verify the container is running
- You can check if your Jenkins container is running with:

```bash
docker ps
```
- This will list all running containers. You should see the Jenkins container listed there with port 9000 exposed.

### Step 10: Create a Jenkins Job Using Executable Shell
- To create a Jenkins job and include commands to run via an executable shell, follow these steps:

-From the Jenkins dashboard, click New Item.
- Choose Freestyle project and give it a name.
- Under Build, select Execute shell and add your shell commands. For example:
```bash
docker run --rm -d -p 8080:8080 jenkins
```
- This allows Jenkins to run Docker commands to launch containers directly within a job, making it easier to manage Jenkins environments without manually running them.

### Step 11: Schedule Jobs Using Cron Expressions
- you can schedule Jenkins jobs using cron expressions, which allow you to automate when the jobs will run. Here’s how to use the Cron expression generator from Cronhub:

# Cron Expression Format
The cron expression consists of five fields:

```scss
* * * * *
| | | | |
| | | | +---- Day of the week (0 - 6) (Sunday=0)
| | | +------ Month (1 - 12)
| | +-------- Day of the month (1 - 31)
| +---------- Hour (0 - 23)
+------------ Minute (0 - 59)
Example Cron Expressions
*/5 * * * *: Every 5 minutes
* * * * *: Every minute
0 * * * *: Every hour
0 0 * * *: Every day at 12:00 AM
0 0 * * FRI: At 12:00 AM, only on Friday
0 0 1 * *: At 12:00 AM, on day 1 of the month
This cron expression generator can help you schedule jobs more efficiently without worrying about infrastructure setup. Simply use the expression that fits your needs!
```

### Step 12: Simplifying Job Creation with Docker and GitHub Integration
- To create a simple Jenkins job for a containerized application (e.g., a Flask web app), follow these steps:

### Create a New Job:

- From the Jenkins dashboard, click New Item.
- Choose Freestyle project and give it a name.
- Configure Source Code Management:

-Under Source Code Management, choose Git.
- Enter the GitHub repository URL: https://github.com/r0han01/Jenkins-Flask-Application.git.
- Set the Branch Specifier to */main (or your desired branch).
- Build Triggers:

- Under Build Triggers, enable Poll SCM.
- Enter the cron expression * * * * * to check for changes every minute (adjust as necessary).
- Build Steps:

Under Build Steps, select Execute shell.
Enter the following commands to build and run the Docker container:
```bash
docker stop jenkins
docker rm jenkins
docker build -t jenkins .
docker run --name jenkins -d -p 9000:9000 jenkins
```
- Save and Run:

- Click Save to save your job.
- You can now trigger the job manually or automatically based on the configured schedule.
- This approach integrates GitHub and Docker with Jenkins, making it easier to deploy containerized applications within Jenkins pipelines.

#### Step 13: Update Application Every Minute Using Cron Job
- To ensure your application is updated automatically every minute using the cron technique in Jenkins, follow these steps:

- Modify the Jenkins Job Cron Schedule:
- In your Jenkins job configuration, go to the Build Triggers section.
- Enable Poll SCM and set the cron expression to * * * * * to run the job every minute. This will ensure that the job checks for changes every minute.
- Update Your Application HTML for Real-Time Changes:
- Modify the HTML code of your application to trigger a refresh whenever the page is loaded. This can be done by adding the following code to ensure the page reloads every time it is accessed:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello from Jenkins</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(45deg, #3498db, #9b59b6, #e74c3c, #2ecc71, #f39c12);
            background-size: 400% 400%;
            animation: gradientAnimation 15s ease infinite;
            color: #fff;
            text-align: center;
            padding: 50px;
            margin: 0;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }

        @keyframes gradientAnimation {
            0% { background-position: 0% 50%; }
            25% { background-position: 50% 50%; }
            50% { background-position: 100% 50%; }
            75% { background-position: 50% 50%; }
            100% { background-position: 0% 50%; }
        }

        .content {
            background-color: rgba(0, 0, 0, 0.8);
            border-radius: 20px;
            padding: 40px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.7);
            position: relative;
            display: inline-block;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
            transform: scale(1);
        }

        .content:hover {
            transform: scale(1.05);
            box-shadow: 0 8px 30px rgba(0, 0, 0, 0.8);
        }

        .text {
            font-size: 3.5em;
            font-weight: bold;
            color: #f1c40f;
            margin: 20px 0;
            animation: typing 3s steps(30) 1s forwards, bounce 2s ease-in-out infinite;
        }

        @keyframes typing {
            from { width: 0; }
            to { width: 100%; }
        }

        @keyframes bounce {
            0%, 100% {
                transform: translateY(0);
            }
            50% {
                transform: translateY(-20px);
            }
        }

        .musk {
            font-size: 3em;
            font-weight: bold;
            color: #e74c3c;
            cursor: pointer;
            transition: transform 0.3s ease, color 0.3s ease, text-shadow 0.3s ease, transform 0.3s ease;
        }

        .musk:hover {
            transform: scale(1.3);
            color: #16a085;
            text-shadow: 0 0 20px rgba(23, 191, 191, 0.8);
        }

        .button {
            padding: 18px 40px;
            font-size: 1.3em;
            color: #fff;
            background-color: #3498db;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: background-color 0.3s ease, transform 0.3s ease;
            margin-top: 30px;
        }

        .button:hover {
            background-color: #2980b9;
            transform: scale(1.1);
        }

        .popup {
            display: none;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: rgba(0, 0, 0, 0.85);
            padding: 40px;
            border-radius: 15px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.5);
            backdrop-filter: blur(10px);
            transition: opacity 0.3s ease, transform 0.3s ease;
        }

        .popup.show {
            display: block;
            opacity: 1;
            transform: translate(-50%, -50%) scale(1);
        }

        .popup h2 {
            color: #ecf0f1;
            font-size: 2.2em;
            margin-bottom: 20px;
            animation: fadeIn 1s ease-out;
        }

        @keyframes fadeIn {
            0% { opacity: 0; transform: translateY(-20px); }
            100% { opacity: 1; transform: translateY(0); }
        }

        .close-popup {
            position: absolute;
            top: 10px;
            right: 10px;
            color: #fff;
            background-color: #e74c3c;
            border: none;
            padding: 10px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1.5em;
            transition: background-color 0.3s ease;
        }

        .close-popup:hover {
            background-color: #c0392b;
        }

    </style>
</head>
<body>
    <div class="content">
        <div class="text">Hello from Jenkins!</div>
        <div class="musk" onclick="showPopup()">Click Musk</div>
        <div class="popup" id="popup">
            <button class="close-popup" onclick="closePopup()">X</button>
            <h2>Welcome to Jenkins powered by Musk!</h2>
            <p>It's super cool, isn't it?</p>
        </div>
        <button class="button" onclick="window.location.reload()">Reload Page</button>
    </div>

    <script>
        function showPopup() {
            document.getElementById('popup').classList.add('show');
        }

        function closePopup() {
            document.getElementById('popup').classList.remove('show');
        }

        // Reload page every minute to reflect updates
        setTimeout(function(){
            window.location.reload();
        }, 60000); // 60000 milliseconds = 1 minute
    </script>
</body>
</html>
```
`This script will reload the page every minute (as configured by the Jenkins cron job) to ensure that the application updates itself with the latest changes made in the repository.`

`Run Jenkins Job:
Once the cron job is set, Jenkins will trigger the build every minute, updating the application with the latest changes from the repository. You can manually trigger or let Jenkins handle it automatically based on your setup.`

# Conclusion
######### By following these steps, we’ve successfully implemented a Continuous Integration and Continuous Deployment (CI/CD) pipeline using Jenkins. With automated updates running every minute, our application is now equipped to efficiently reflect the latest changes from the repository, reducing manual intervention and ensuring faster development cycles. This pipeline helps streamline our workflow, improves reliability, and ensures the application is always up-to-date and ready for production. With Jenkins managing the build, test, and deployment processes, we’ve laid the foundation for a robust, automated development lifecycle that enables faster and more efficient software delivery. This marks the end of our setup, but the beginning of an improved and more efficient development process.
