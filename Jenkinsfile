pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        
        stage('Checkout'){
           steps {
                git credentialsId: '79725ebd-f5ef-4229-bd28-23486f9cf35d', 
                url: 'https://github.com/Maggiyerra/cicd-end-to-end',
                branch: 'main'
           }
        }

        stage('Build Docker'){
            steps{
                script{
                    sh '''
                    echo 'Buid Docker Image'
                    docker build -t maggi19/cicd-e2e:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        /*stage('Push the artifacts'){
           steps{
                script{
                    sh '''
                    echo 'Push to Repo'
                    docker push maggi19/cicd-e2e:${BUILD_NUMBER}
                    '''
                }
            }
        }     
        stage('Checkout K8S manifest SCM'){
            steps {
                git credentialsId: 'f87a34a8-0e09-45e7-b9cf-6dc68feac670', 
                url: 'https://github.com/Maggiyerra/manifest-kube-repo',
                branch: 'main'
            }
        }*/
        
        stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                    withCredentials([usernamePassword(credentialsId: '20cd6345-9783-40c7-ba6d-414f7a11aca9', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        sh '''
                        cat deploy.yaml
                        sed -i '' "s/32/${BUILD_NUMBER}/g" deploy.yaml
                        cat deploy.yaml
                        git add deploy.yaml
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push  https://github.com/Maggiyerra/manifest-kube-repo HEAD:main
                        '''                        
                    }
                }
            }
        }
    }
}
