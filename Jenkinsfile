pipeline {
    agent {
        label 'shared-services-aws'
    }
    stages {
        stage('Ansible Lint') {
            steps {
                sh """
                    ansible-lint main.yml
                """
            }
        }
    }
} 