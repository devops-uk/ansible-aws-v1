pipeline {
    agent any

    environment {
        ANSIBLE_CONFIG = "${WORKSPACE}/ansible.cfg"
    }

    stages {

        stage('Create EC2 using Ansible') {
            steps {
                sh '''
                /usr/bin/ansible-playbook ec2-create.yml
                '''
            }
        }

        stage('Wait for EC2 to be reachable') {
            steps {
                sh 'sleep 60'
            }
        }

        stage('Verify Dynamic Inventory') {
            steps {
                sh '''
                /usr/bin/ansible-inventory --graph
                '''
            }
        }

        stage('Deploy Apache using Dynamic Inventory') {
            steps {
                sh '''
                /usr/bin/ansible-playbook deploy_apache.yml
                '''
            }
        }
    }

    post {
        success {
            echo 'EC2 created and Apache deployed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}

