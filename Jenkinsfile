pipeline {
    agent any

    stages {
        stage('Docker Push') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'DockerHub-lbq16756', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh '''
                            SOCAT_IP=$(docker inspect socat --format '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}')
                            export DOCKER_HOST=tcp://${SOCAT_IP}:2375
                            docker login -u $USERNAME -p $PASSWORD
                            docker push lbq16756/node-web-app
                        '''
                    }
                }
            }
        }
        stage('Trigger Render Deployment') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'RenderDeployKey', variable: 'KEY')]) {
                        sh "curl https://api.render.com/deploy/$KEY"
                    }
                }
            }
        }        
    }
}