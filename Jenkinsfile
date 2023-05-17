pipeline {
    agent any
    stages {
        stage ('Docker image build and push to docker hub') {
            steps {
                script{
                    sh 'docker build . -t franckabdullah/laravel10:$BUILD_NUMBER'
                    withCredentials([string(credentialsId: 'docker-hub', variable: 'docker_hub_cred')]) {
                        sh '''
                        docker login -u franckabdullah -p $docker_hub_cred
                        docker push franckabdullah/laravel10:$BUILD_NUMBER
                        '''
                    } 
                }
            }
        }
    }
}