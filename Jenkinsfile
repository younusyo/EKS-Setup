pipeline {
    agent any

    environment {
        TF_IN_AUTOMATION = 'true'
        PATH = "${WORKSPACE}/bin:${env.PATH}"
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
                    mkdir -p bin

                    # Install Terraform
                    curl -fsSL -o terraform.zip https://releases.hashicorp.com/terraform/1.9.8/terraform_1.9.8_linux_amd64.zip
                    unzip -o terraform.zip -d bin
                    chmod +x bin/terraform
                    ./bin/terraform version

                    # Install TFLint
                    curl -s https://raw.githubusercontent.com/terraform-linters/tflint/master/install_linux.sh | bash -s -- -b bin
                    ./bin/tflint --version
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
