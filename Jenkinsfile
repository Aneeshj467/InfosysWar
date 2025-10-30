// Method 2  - Terminal run these commands to give jenkins user docker access

// -------------------

// sudo usermod -aG docker jenkins
// sudo systemctl restart jenkins






// pipeline{
//     agent any
//     tools{
//         maven 'maven' 
//     }
//     environment {
//         DOCKERHUB_CREDENTIALS_ID = 'crud_login' 
//         DOCKERHUB_USERNAME       = 'aneesh467'
//         IMAGE_NAME               = "${env.DOCKERHUB_USERNAME}/infosys"
//         CONTAINER_NAME           = "infosys_container"
//     }
//     stages{

//         stage('Build stage'){
//             steps{
//                 echo 'Building with Maven...'
//                 sh 'mvn clean package'
//             }
//         }

//         stage('Build Docker Image') {
//             steps {
//                 echo "Building Docker image: ${IMAGE_NAME}:${BUILD_NUMBER}"
//                 sh "sudo docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ."
//             }
//         }

//         stage('Login to Docker Hub') {
//             steps {
//                 echo 'Logging in to Docker Hub...'
//                 withCredentials([usernamePassword(credentialsId: env.DOCKERHUB_CREDENTIALS_ID, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
//                     sh 'echo $DOCKER_PASS | sudo docker login -u $DOCKER_USER --password-stdin'
//                 }
//             }
//         }

//         stage('Tag and Push Docker Image') {
//             steps {
//                 script {
//                     echo "Pushing image: ${IMAGE_NAME}:${BUILD_NUMBER}"
//                     sh "sudo docker push ${IMAGE_NAME}:${BUILD_NUMBER}"
                    
//                     echo "Tagging as 'latest'..."
//                     sh "sudo docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${IMAGE_NAME}:latest"
                    
//                     echo "Pushing 'latest' tag..."
//                     sh "sudo docker push ${IMAGE_NAME}:latest"
//                 }
//             }
//         }

//         stage('Remove Local Docker Image') {
//             steps {
//                 echo "Removing local image: ${IMAGE_NAME}:${BUILD_NUMBER}"
//                 sh "sudo docker rmi ${IMAGE_NAME}:${BUILD_NUMBER}"
//             }
//         }

//         stage('Run Container') {
//             steps {
//                 echo "Running new container ${CONTAINER_NAME} on port 8084..."
//                 sh "sudo docker stop ${CONTAINER_NAME} || true"
//                 sh "sudo docker rm ${CONTAINER_NAME} || true"
//                 sh "sudo docker run -d -p 8084:8080 --name ${CONTAINER_NAME} ${IMAGE_NAME}:latest"
//             }
//         }
//     }
//     post {
//         always {
//             echo 'This will always run after the stages are complete.'
//         }
//         success {
//             echo 'This will run only if the pipeline succeeds.'
//         }
//         failure {
//             echo 'This will run only if the pipeline fails.'
//         }
//     }
// }




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
                    credentialsId: 'crud_login', 
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
                sh 'docker run -d -p 8084:8080 --name varinfoservices_container varinfoservices'
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



