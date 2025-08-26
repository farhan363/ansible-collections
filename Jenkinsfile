pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/farhan363/ansible-collections.git'
            }
        }

        stage('Install Collections') {
            steps {
                sh 'ansible-galaxy collection install -r requirements.yml'
            }
        }

        stage('Run Playbook') {
            steps {
                withCredentials([
                    string(credentialsId: 'ansible-vault-pass', variable: 'ANSIBLE_VAULT_PASS'),
                    sshUserPrivateKey(credentialsId: 'Master-Server', keyFileVariable: 'SSH_KEY')
                ]) {
                    sh '''
                        # Write the Vault password into a temporary file
                        echo "$ANSIBLE_VAULT_PASS" > vault_pass.txt

                        # Run the playbook using the SSH private key
                        ansible-playbook site.yml --private-key $SSH_KEY --vault-password-file vault_pass.txt

                        # Remove the temporary file for security
                        rm -f vault_pass.txt
                    '''
                }
            }
        }
    }
}
