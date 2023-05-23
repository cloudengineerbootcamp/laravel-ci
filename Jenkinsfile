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

        // Code related to updating deployment file block

        stage ('update deployment file for GitOps') {
            environment {
                GITHUB_USER = "cloudengineerbootcamp"
            }

            steps {
                withCredentials([string(credentialsId: 'laravel-ci-token', variable: 'GITHUB_TOKEN')]) {
                    sh '''
                    cat Deployment.yaml
                    sed -i "s/docker_tag/$BUILD_NUMBER/" Deployment.yaml
                    cat Deployment.yaml

                    git clone https://github.com/cloudengineerbootcamp/laravel-cd.git
                    cp Deployment.yaml laravel-cd/

                    cd laravel-cd

                    git config user.name ${GITHUB_USER}
                    git config user.email ${GITHUB_USER}@gmail.com

                    git add Deployment.yaml
                    git commit -m "updated Deployment file with ${BUILD_NUMBER}"
                    git push https://${GITHUB_TOKEN}@github.com/${GITHUB_USER}/laravel-cd HEAD:main
                    cd ../ && rm -rf laravel-cd
                    '''
                }
            }
        }
    }
}