pipeline {
    agent any

    parameters {
        string(
            name: 'IMAGE_TAG',
            defaultValue: 'latest',
            description: 'Enter the Docker image tag to deploy'
        )
    }

    environment {
        IMAGE_TAG = "${params.IMAGE_TAG}"
        AWS_DEFAULT_REGION = 'us-east-1'
    }

    stages {
        stage('Checkout Repo B') {
            steps {
                echo "Checking out infrastructure repository..."
                checkout scm
            }
        }

        stage('Show Selected Tag') {
            steps {
                echo "Selected Docker image tag: ${params.IMAGE_TAG}"
            }
        }

        stage('Provision EC2') {
            steps {
                echo "Provisioning new EC2 instance..."
                sh '''
                ansible-playbook playbooks/provision.yml
                '''
            }
        }

        stage('Configure and Deploy App') {
            steps {
                echo "Installing Docker and deploying selected image..."
                sh '''
                ansible-playbook playbooks/configure.yml --extra-vars "image_tag=$IMAGE_TAG"
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                echo "Verification is handled in configure.yml"
            }
        }
    }
pipeline {
    agent any

    parameters {
        string(
            name: 'IMAGE_TAG',
            defaultValue: 'latest',
            description: 'Enter the Docker image tag to deploy'
        )
    }

    environment {
        IMAGE_TAG = "${params.IMAGE_TAG}"
        AWS_DEFAULT_REGION = 'us-east-1'
    }

    stages {
        stage('Checkout Repo B') {
            steps {
                echo "Checking out infrastructure repository..."
                checkout scm
            }
        }

        stage('Show Selected Tag') {
            steps {
                echo "Selected Docker image tag: ${params.IMAGE_TAG}"
            }
        }

        stage('Provision EC2') {
            steps {
                echo "Provisioning new EC2 instance..."

                withCredentials([
                    string(credentialsId: 'aws-access-key-id', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'aws-secret-access-key', variable: 'AWS_SECRET_ACCESS_KEY')
                ]) {
                    sh '''
                    ansible-playbook playbooks/provision.yml
                    '''
                }
            }
        }

        stage('Configure and Deploy App') {
            steps {
                echo "Installing Docker and deploying selected image..."

                withCredentials([
                    string(credentialsId: 'aws-access-key-id', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'aws-secret-access-key', variable: 'AWS_SECRET_ACCESS_KEY')
                ]) {
                    sh '''
                    ansible-playbook playbooks/configure.yml --extra-vars "image_tag=$IMAGE_TAG"
                    '''
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                echo "Verification is handled in configure.yml"
            }
        }
    }

    post {
        success {
            echo "CD pipeline completed successfully."
        }
        failure {
            echo "CD pipeline failed."
        }
    }
}
    post {
        success {
            echo "CD pipeline completed successfully."
        }
        failure {
            echo "CD pipeline failed."
        }
    }
}