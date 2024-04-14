pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = "debatradevelop/train-schedule"
        KUBE_CONFIG = '/home/edureka/.kube/admin.conf'
        KUBE_CONFIG_ID = 'kubeconfig'
    }
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(env.DOCKER_IMAGE_NAME)
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker_hub_login') {
                        docker.image(env.DOCKER_IMAGE_NAME).push("${env.BUILD_NUMBER}")
                        docker.image(env.DOCKER_IMAGE_NAME).push("latest")
                    }
                }
            }
        }
        stage('Canary Deploy') {
            environment { 
                CANARY_REPLICAS = 1
            }
            steps {
                kubernetesDeploy(
                    kubeconfigId: env.KUBE_CONFIG_ID,
                    configs: 'train-schedule-kube-canary.yml',
                    enableConfigSubstitution: true
                )
            }
        }
        stage('Deploy to Production') {
            environment { 
                CANARY_REPLICAS = 0
            }
            steps {
                input 'Deploy to Production?'
                milestone(1)
                kubernetesDeploy(
                    kubeconfigId: env.KUBE_CONFIG_ID,
                    configs: 'train-schedule-kube.yml',
                    enableConfigSubstitution: true
                )
            }
        }
    }
}
