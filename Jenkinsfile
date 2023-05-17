
// Code related to docker block

stage ('Docker build') {
    steps {
        script{
            sh 'cp -r ../web-app@2/target .'
            sh 'docker build . -t sudarshantevari/web-app:$BUILD_NUMBER'
            withCredentials([string(credentialsId: 'docker-hub', variable: 'docker-hub')]) {
                sh '''
                docker login -u sudarshantevari -p $docker_password
                docker push sudarshantevari/web-app:$BUILD_NUMBER
                '''
            } 
        }
    }
}
