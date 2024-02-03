pipeline {
    agent any

    parameters {
        string(name: 'DOCKER_IMAGE_NAME', defaultValue: 'testimage', description: 'Enter the Docker image name')
    }

    environment {
        dockerimagename = "${params.DOCKER_IMAGE_NAME}"
        registryCredential = 'docker'
    }

    stages {
        stage('Checkout Source') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/pragadees196/fampay-hodor']])
            }
        }

        stage('Build image') {
            steps {
                script {
                    withEnv(["PATH=/usr/local/bin:$env.PATH"]) {
                        dir('app') {
                            dockerImage = docker.build env.dockerimagename
                        }
                    }
                }
            }
        }

        stage('Tag image') {
            steps {
                script {
                    withEnv(["PATH=/usr/local/bin:$env.PATH"]){
                        sh "/usr/local/bin/docker tag ${env.dockerimagename}:latest pragadeesh196/${env.dockerimagename}:latest"
                    }
                }
            }
        }

        stage('Push image') {
            steps {
                script {
                    withEnv(["PATH=/usr/local/bin:$env.PATH"]){
                        sh "/usr/local/bin/docker login"
                        sh "/usr/local/bin/docker push pragadeesh196/${env.dockerimagename}:latest"
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withEnv(["PATH=/usr/local/bin:$env.PATH"]) {
                        dir('k8s'){
                            sh "/usr/local/bin/kubectx orbstack"
                            sh "/usr/local/bin/kubectl set image deployment hodor hodr1=pragadeesh196/${env.dockerimagename}:latest"
                        }
                    }
                }
            }
        }
    }
}
