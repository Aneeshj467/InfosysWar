pipeline {
    agent any
    tools {
        maven 'maven' 
    }
    stages {
        stage('Stage 1: Build with Maven') {
            steps {
                echo 'Building the project using Maven'
                sh 'mvn clean package'
            }
            post {
                success { echo 'Build successful!' }
                failure { echo 'Build failed!' }
                always  { echo 'Build process completed.' }
            }
        }
        stage('Build Docker Image') {
    steps {
        echo 'Building Docker Image'
        sh '''
            echo "Current directory:"
            pwd
            echo "Files in workspace:"
            ls -l
            echo "Files in target folder:"
            ls -l target || echo "target folder not found"
            echo "Contents of Dockerfile:"
            cat Dockerfile
        '''
        sh 'docker build -t varinfoservices .'
    }
}


        stage('Stage 2: Build Docker Image') {
            steps {
                echo 'Building Docker Image'
                sh 'docker build -t varinfoservices .'
            }
            post {
                success { echo 'Docker Image built successfully!' }
                failure { echo 'Docker Image build failed!' }
                always  { echo 'Docker build process completed.' }
            }
        }

        stage('Stage 3: Tag Docker Image') {
            steps {
                echo 'Tagging Docker Image'
                sh 'docker tag varinfoservices aneesh467/infosys:latest'
            }
            post {
                success { echo 'Docker Image tagged successfully!' }
                failure { echo 'Docker Image tagging failed!' }
                always  { echo 'Docker tagging process completed.' }
            }
        }

        stage('Stage 4: Docker Login') {
            steps {
                echo 'Logging into Docker Hub'
                withCredentials([usernamePassword(
                    credentialsId: 'cdf3e9b2-23f9-46e2-872a-c7631004bc76', 
                    usernameVariable: 'DOCKER_HUB_USERNAME', 
                    passwordVariable: 'DOCKER_HUB_PASSWORD'
                )]) {
                    sh '''
                        echo $DOCKER_HUB_PASSWORD | docker login -u $DOCKER_HUB_USERNAME --password-stdin
                    '''
                }
            }
            post {
                success { echo 'Logged into Docker Hub successfully!' }
                failure { echo 'Docker Hub login failed!' }
                always  { echo 'Docker login process completed.' }
            }
        }

        stage('Stage 5: Push Docker Image to Docker Hub') {
            steps {
                echo 'Pushing Docker Image to Docker Hub'
                sh 'docker push aneesh467/infosys:latest'
            }
            post {
                success { echo 'Docker Image pushed successfully!' }
                failure { echo 'Docker Image push failed!' }
                always  { echo 'Docker push process completed.' }
            }
        }

        stage('Stage 6: Run Docker Container') {
            steps {
                echo 'Running Docker Container'
                sh 'docker run -d -p 8084:8084 --name varinfoservices_container varinfoservices'
            }
            post {
                success { echo 'Docker Container is running successfully!' }
                failure { echo 'Docker Container failed to start!' }
                always  { echo 'Docker run process completed.' }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
    }
}
