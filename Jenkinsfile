pipeline {
    agent any
    stages {
        stage ('Docker image build and push to docker hub') {
            steps {
                script{
                    sh 'docker build . -t franckabdullah/laravel10:$BUILD_NUMBER'
                    withCredentials([string(credentialsId: 'docker-hub', variable: 'docker-hub-cred')]) {
                        sh '''
                        docker login -u franckabdullah -p $docker-hub-cred
                        docker push franckabdullah/laravel10:$BUILD_NUMBER
                        '''
                    } 
                }
            }
        }
    }
}