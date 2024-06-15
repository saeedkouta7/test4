pipeline {
    agent any

    environment {
        TERRAFORM_DIR = 'terraform'
        BACKEND_DIR = 'terraform/modules/backend'
        ANSIBLE_DIR = 'ansible-roles'
        INVENTORY_FILE = 'inventory'
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        AWS_REGION = 'us-east-1'
    }

    stages {
        
        stage('Run Ansible Playbook') {
            steps {
                dir("${ANSIBLE_DIR}") {
                    withCredentials([sshUserPrivateKey(credentialsId: 'ivolve_private_key', keyFileVariable: 'SSH_PRIVATE_KEY')]) {
                        script {
                            // Ensure the key has the correct permissions
                            sh 'chmod 400 $SSH_PRIVATE_KEY'
                            // Run the Ansible playbook
                            sh '''
                            ansible-playbook -K -i inventory playbook.yml --private-key $SSH_PRIVATE_KEY
                            '''
                        }
                    }     
                }
            }
        }
    }
}
