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
                sh 'sudo docker build -t jayash1845/myproject:latest .'
            }
        }
        stage('Push Docker Image') {
            steps {
                echo "Pushing Docker Image to Registry"
                sh '''
                sudo docker login -u jayash1845 -p Kira@1845
                sudo docker push jayash1845/myproject:latest
                '''
            }
        }
        stage('Deploy Docker Container') {
            steps {
                echo "Deploying Docker Container"
                sh '''
                sudo docker pull jayash1845/myproject:latest
                sudo docker stop myproject || true
                sudo docker rm myproject || true
                sudo docker run -d --name myproject -p 8081:8081 jayash1845/myproject:latest
                '''
            }
        }
    }
}
