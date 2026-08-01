pipeline {
    agent any
        stages {

        stage('Checkout') {
            steps {
                echo 'Source code checked out successfully.'
            }
        }
            
        stage('Build') {
            steps {
                sh 'docker build -t my-app:latest .'
            }
        }

    stage('Docker Login') {
    steps {
        withCredentials([
            usernamePassword(
                credentialsId: 'dockerhub-login',
                usernameVariable: 'DOCKER_USER',
                passwordVariable: 'DOCKER_PASS'
            )
        ]) {
            sh '''
            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
            '''
        }
    }
}

    stage('Docker Tag') {
    steps {
        sh 'docker tag my-app:latest nur64/my-app:latest'
    }
}

            stage('Docker Push') {
    steps {
        sh 'docker push nur64/my-app:latest'
    }
}

        stage('Run') {
            steps {
                sh 'docker run --rm my-app:latest'
            }
        }

    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }

        failure {
            echo 'Pipeline failed.'
        }
    }
}
