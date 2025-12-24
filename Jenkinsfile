pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')   
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    credentialsId: 'git-creds',
                    url: 'https://github.com/Nikitakank/CICD-pipeline.git'
            }
        }

        stage('Deploy to Server') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'technohertz-creds',
                        usernameVariable: 'SSH_USER',
                        passwordVariable: 'SSH_PASS'
                    )
                ]) {
                    sh '''
                    echo "Deploying to server..."

                    sshpass -p "$SSH_PASS" ssh -o StrictHostKeyChecking=no 
                    $SSH_USER@148.72.215.184 
                    "cd /home/technohertz/War/Demo && chmod +x deploy_demo.sh && ./deploy_demo.sh"

                    echo "Deployment completed"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "CI/CD PIPELINE SUCCESS"
        }
        failure {
            echo "CI/CD PIPELINE FAILED"
        }
    }
}
