`pipeline {
    agent any

    environment {
        TF_IN_AUTOMATION = 'true'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup Terraform & Tools') {
            steps {
                sh '''
                    # Install Terraform
                    curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
                    echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
                    sudo apt-get update && sudo apt-get install -y terraform

                    # Install tflint
                    curl -s https://raw.githubusercontent.com/terraform-linters/tflint/master/install_linux.sh | bash
                '''
            }
        }

        stage('Terraform Format Check') {
            steps {
                sh 'terraform fmt -check -recursive'
            }
        }

        stage('Terraform Validate') {
            steps {
                sh '''
                    terraform init -backend=false
                    terraform validate
                '''
            }
        }

        stage('TFLint') {
            steps {
                sh 'tflint --recursive'
            }
        }
    }

    post {
        always {
            echo "Terraform lint pipeline finished"
        }
        failure {
            echo "Lint checks failed. Please fix the issues."
        }
    }
}
