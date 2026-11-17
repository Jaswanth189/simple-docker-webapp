pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/jaswanth189/simple-docker-webapp.git'
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
    steps {
        withSonarQubeEnv('SonarQube') {
            sh 'mvn sonar:sonar -Dsonar.projectKey=simple-docker-webapp'
        }
    }
}
        stage('Quality Gate') {
    steps {
        timeout(time: 5, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
        }
    }
}
       stage('Docker Build on Remote Server') {
    steps {
        sh '''
        ssh -i /var/lib/jenkins/.ssh/id_ed25519 \
        -o StrictHostKeyChecking=no \
        ubuntu@172.31.37.100 \
        'cd /home/ubuntu/app && sudo docker build -t simple-docker-webapp:latest .'
        '''
    }
}
            }
        }
