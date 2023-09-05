pipeline {
    agent any

    stages {
        stage('Checkout Code from Git') {
            steps {
                // Checkout your code from a Git repository
                git branch: 'main', credentialsId: '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', url: 'https://github.com/Abhi01266/Jenkins_ArgoCD_Sonarcube_Java_Webapp_K8s.git'
            }
        }

        stage('Build and Test') {
            steps {
                // Build your project, run tests, etc.
                sh 'mvn clean package'
            }
        }

        stage('Docker Build and Push') {
            steps {
                // Build a Docker image
                script {
                    def dockerImage = docker.build("abhi-jenkins-docker-image:${env.BUILD_NUMBER}")
                }
                // Push the Docker image to Docker Hub
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'abhibondar', passwordVariable: 'Abhi321@123')]) {
                        docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {
                            dockerImage.push()
                        }
                    }
                }
            }
        }

        stage('Static Code Analysis with SonarQube') {
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    sh 'mvn sonar:sonar -Dsonar.login=$SONAR_AUTH_TOKEN -Dsonar.host.url=$SONAR_URL'
                }
            }
        }

        stage('Deploy to Production') {
            when {
                // Define conditions for when to deploy to production (e.g., after successful build and tests)
                expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            }
            steps {
                // Add deployment steps here
            }
        }
    }

    post {
        success {
            // Additional post-build actions for a successful build
            // e.g., notifications, artifact archiving, etc.
        }
        failure {
            // Additional actions to take on build failure
            // e.g., notifications, rollback, etc.
        }
    }
}
