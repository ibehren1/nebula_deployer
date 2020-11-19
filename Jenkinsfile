pipeline {
    agent {
        label 'shared-services-aws'
    }
    stages {
        stage('Lint YAML Config') {
            steps {
                sh """
                    python -c 'import yaml, sys; yaml.safe_load(sys.stdin)' < nebula_files/config.yml
                """
            }
        }
        stage('Lint Ansible Playbook') {
            steps {
                sh """
                    ansible-lint main.yml
                """
            }
        }
    }
} 