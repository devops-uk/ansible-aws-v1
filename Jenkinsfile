pipeline {
    agent {label 'workernode2'}

    environment {
        PATH = "/usr/bin:/bin:/usr/local/bin"
        ANSIBLE_CONFIG = "${WORKSPACE}/ansible.cfg"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/devops-uk/ansible-aws-v1.git'
            }
        }

        stage('Create EC2 using Ansible') {
            steps {
                sh '''
                ansible-playbook ec2-create.yml
                '''
            }
        }

        stage('Wait for EC2 to be reachable') {
            steps {
                sh '''
                sleep 60
                '''
            }
        }

        stage('Verify Dynamic Inventory') {
            steps {
                sh '''
                ansible-inventory --graph
                '''
            }
        }

        stage('Deploy Apache using Dynamic Inventory') {
            steps {
                sh '''
                ansible-playbook -i aws_ec2.yml ubuntuapache.yml
                '''
            }
        }
    }

    post {
        success {
            echo 'EC2 created and Apache deployed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
