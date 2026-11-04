pipeline {
    agent any

    stages {
        stage('1. Récupération du Code') {
            steps {
                checkout scm
            }
        }

        stage('2. Validation Syntaxique') {
            steps {
                sh 'ansible-playbook -i ansible/hosts.ini ansible/deploy.yml --syntax-check'
            }
        }

        stage('3. Déploiement via Ansible') {
            steps {
                sh 'ansible-playbook -i ansible/hosts.ini ansible/deploy.yml'
            }
        }

        stage('4. Healthcheck Applicatif') {
            steps {
                sh 'sshpass -p "password123" ssh -o StrictHostKeyChecking=no deploy@target-server -p 2222 "echo password123 | sudo -S docker exec my-web-app-container curl -i http://localhost:80"'
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline exécuté avec succès !'
        }
        failure {
            echo '❌ Échec du pipeline CI/CD.'
        }
    }
}