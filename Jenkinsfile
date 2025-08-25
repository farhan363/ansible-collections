pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Umair1012/ansible-collections.git'
            }
        }

        stage('Install Collections') {
            steps {
                sh 'ansible-galaxy collection install -r requirements.yml'
            }
        }

        stage('Run Playbook') {
            steps {
                withCredentials([string(credentialsId: 'ansible-vault-pass', variable: 'ANSIBLE_VAULT_PASS')]) {
                    sh '''
                        ansible-playbook site.yml \
                        --vault-password-file <(echo $ANSIBLE_VAULT_PASS)
                    '''
                }
            }
        }
    }
}
