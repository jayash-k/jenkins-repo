pipeline {
    agent any

    stages {
        stage('Git Pull') {
            steps {
                git 'https://github.com/jayash-k/jenkins-repo.git'
            }
        }
        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                echo "Testing with SonarQube"
                sh '''
                mvn clean verify sonar:sonar \
                  -Dsonar.projectKey=myproject \
                  -Dsonar.host.url=http://54.87.37.180:9000 \
                  -Dsonar.login=sqp_324cbb78c2f55e24ac966b69c126158e7d06e03e
                '''
            }
        }
        stage('Build Docker Image') {
            steps {
                echo "Building Docker Image"
                sh 'cd /var/lib/jenkins/workspace/myjob && docker build -t jayash1845/myproject:latest .'
            }
        }
        stage('Push Docker Image') {
            steps {
                echo "Pushing Docker Image to Registry"
                sh '''
                docker login -u jayash1845 -p Kira@1845
                docker push jayash1845/myproject:latest
                '''
            }
        }
        stage('Deploy Docker Container') {
            steps {
                echo "Deploying Docker Container"
                sh '''
                docker pull jayash1845/myproject:latest
                docker stop myproject || true
                docker rm myproject || true
                docker run -d --name myproject -p 8081:8081 jayash1845/myproject:latest
                '''
            }
        }
    }
}
