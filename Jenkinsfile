pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/ahmedmota/ci-cd-react-app-docker-container.git'

                // Build the Docker image
                sh 'docker build -t ahmedhamid/ci-cd-react-app-docker-image .'
            }
        }

        stage('Deploy') {
            steps {
                // Log in to Docker Hub
                withCredentials([usernamePassword(credentialsId: 'ahmedhamid-dockerhub', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
                    sh 'echo $DOCKER_HUB_PASSWORD | docker login -u $DOCKER_HUB_USERNAME --password-stdin'
                }

                // Push the Docker image to Docker Hub
                sh 'docker push ahmedhamid/ci-cd-react-app-docker-image'

                // Pull the Docker image on your Docker server and run it
                sshagent(['your-credentials-id']) {
                    sh 'ssh -o StrictHostKeyChecking=no yourusername@your.docker.server "docker pull ahmedhamid/your-app && docker run -d -p 3000:3000 ahmedhamid/your-app"'
                }
            }
        }
    }
}
