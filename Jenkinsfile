pipeline{
    agent any
    environment{
        PROJECT_ID = 'horizontal-ally-383421'
            CLUSTER_NAME = 'demo-gke'
            ZONE = 'us-central1'
            CREDENTIALS_ID = 'gcr-deploy'
    }
    stages {
        stage('Build') {
            steps{
                // Connect to Git
                git 'https://github.com/syahidhzblh/berkelana.github.io.git'
                // Build Docker image from Dockerfile
                sh 'docker build -t gcr.io/horizontal-ally-383421/berkelana:latest .'
                // Removing image with tag <none>
                sh 'docker image prune -f'
            }
        }
        stage('Push'){
            steps{
                sh '''
                docker push syahid188/berkelana:v2
                '''
            }
        }
        stage('Deploy'){
            steps{
                withCredentials([file(credentialsId:'gcr-deploy', variable:'gke_creds')]){
                    git 'https://github.com/syahidhzblh/berkelana.github.io.git'
                    sh '''
                        gcloud container clusters get-credentials $CLUSTER_NAME --zone $ZONE
                        kubectl apply -f deployment.yaml
                    '''
                }
            }
        }
    }
}