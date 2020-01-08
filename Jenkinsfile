pipeline {
    environment {
        registry = "comdata456/maven-jdk-11-slim"
        registryCredential = 'docker-hub-credentials'
    }

    agent {
        docker {
            image 'docker' 
            args '-v $HOME/.m2:/root/.m2 -v /root/.ssh:/root/.ssh -v /run/docker.sock:/run/docker.sock' 
        }
    }



    stages {

        
        stage('Make Container') {

            steps {
                sh "docker build -t comdata456/maven-jdk-11-slim:${env.BUILD_ID} jdk-11-slim"
                sh "docker tag comdata456/maven-jdk-11-slim:${env.BUILD_ID} comdata456/maven-jdk-11-slim:latest"
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'target/**/*.jar', fingerprint: true
        }
        success {
            withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh "/usr/bin/docker login -u ${USERNAME} -p ${PASSWORD}"
                sh "/usr/bin/docker push comdata456/maven-jdk-11-slim:${env.BUILD_ID}"
                sh "/usr/bin/docker push comdata456/maven-jdk-11-slim:latest"
            }
        }
    }
}
