pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/rk4027-N/nodejs-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t nani4027/nodejsapp .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-credentials', url: 'https://index.docker.io/v1/']) {
                    sh 'docker push nani4027/nodejsapp'
                }
            }
        }

        stage('Deploy via Ansible') {
            steps {
                sshPublisher(publishers: [
                    sshPublisherDesc(
                        configName: 'ansible',
                        transfers: [
                            sshTransfer(
                                sourceFiles: '',
                                execCommand: 'ansible-playbook ~/ansible-nodejs/deploy.yml'
                            )
                        ],
                        usePromotionTimestamp: false,
                        verbose: true
                    )
                ])
            }
        }
    }
}
