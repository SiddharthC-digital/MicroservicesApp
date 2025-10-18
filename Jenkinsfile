pipeline {
    agent {
        label 'ec2'
    }

    stages {
        stage('Deploy To Kubernetes') {
            steps {
                script {
                    // Retrieve DockerHub credentials (username + password)
                    withCredentials([usernamePassword(credentialsId: 'docker-cred', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    // Replace ${DOCKER_USER} in deployment YAML
                        sh """
                            envsubst < deployment-service.yml > temp.yml && mv temp.yml deployment-service.yml
                        """
                        // Apply the dynamically updated YAML to Kubernetes
                       
                            sh "kubectl apply -f deployment-service.yml"
                        
                    }
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                
                    sh "kubectl get svc -n webapps"
                
            }
        }
    }
}