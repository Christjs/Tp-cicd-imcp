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
                // Vérification de la syntaxe du Playbook Ansible sans l'exécuter
                sh 'ansible-playbook -i ansible/hosts.ini ansible/deploy.yml --syntax-check'
            }
        }

        stage('3. Déploiement via Ansible') {
            steps {
                // Exécution du déploiement automatisé
                sh 'ansible-playbook -i ansible/hosts.ini ansible/deploy.yml'
            }
        }

        stage('4. Healthcheck Applicatif') {
            steps {
                // Validation que Nginx répond correctement dans le conteneur cible
                  sh 'sshpass -p "password123" ssh -o StrictHostKeyChecking=no deploy@target-server -p 2222 sudo docker exec my-web-app-container curl -i http://localhost:80'            }
        }
    }

    post {
        success {
            echo '✅ Déploiement CI/CD réussi avec succès !'
        }
        failure {
            echo '❌ Échec du pipeline CI/CD.'
        }
    }
}