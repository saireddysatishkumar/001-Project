pipeline {
    
    agent {
        node {
            label 'python'
        }
    }

    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
       /*  
        stage('Checkout'){
           steps {
               git credentialsId: 'xyz', 
                url: 'https://github.com/saireddysatishkumar/001-Project.git',
                branch: 'main'
           }
        }
*/
        stage('Build Docker'){
            environment {
            DOCKER_IMAGE = "saireddysatishkumar/todo-app:${BUILD_NUMBER}"
            REGISTRY_CREDENTIALS = credentials('docker-cred')
      }
            steps{
                script{
                    sh '''
                    echo 'Buid Docker Image'
                    docker build -t saireddysatishkumar/todo-app:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        stage('Push the artifacts'){
           steps{
                script{
                    sh '''
                    echo 'Push to Repo'
                    docker push saireddysatishkumar/todo-app:${BUILD_NUMBER}
                    '''
                }
            }
        }

        stage('Checkout K8S manifest SCM and commit changes'){
            stage('Checkout K8S manifests'){
                steps {
                    git credentialsId: 'github', 
                    url: 'https://github.com/saireddysatishkumar/ArgoCD.git',
                    branch: 'main'
                }    
            }
/*            stage('Update Build number and commit changes'){ */
                steps {
                    withCredentials([string(credentialsId: 'github', variable: 'GITHUB_TOKEN')]) {
                        sh '''
                        git config user.email "satish.kumar@gmail.com"
                        git config user.name "satish kumar"
                        BUILD_NUMBER=${BUILD_NUMBER}
                        cp deploy-manifests/dev/todo-app/src/*yaml deploy-manifests/dev/todo-app/
                        sed -i "s/replaceImageTag/${BUILD_NUMBER}/g" deploy-manifests/dev/todo-app/1-deployment.yaml
                        git add deploy-manifests/dev/todo-app/1-deployment.yaml
                        git commit -m "Updated the deploy yaml for build '${BUILD_NUMBER}'"
                        git remote -v
                        git push https://${GITHUB_TOKEN}@github.com/saireddysatishkumar/ArgoCD.git HEAD:main
                        '''                        
                    }
                }    
        /*    } */
        }

        stage('Update deploy number in index.html & push to Repo'){
            steps {
                script{
                    withCredentials([string(credentialsId: 'github', variable: 'GITHUB_TOKEN')]) {
                        sh '''
                        git config user.email "satish.kumar@gmail.com"
                        git config user.name "satish kumar"
                        BUILD_NUMBER=${BUILD_NUMBER}
                        cp deploy/src/*yaml deploy/
                        cp todos/templates/todos/index.html.template  todos/templates/todos/index.html
                        sed -i "s/replaceImageTag/${BUILD_NUMBER}/g" deploy/deploy.yaml
                        sed -i "s/replaceImageTag/${BUILD_NUMBER}/g" todos/templates/todos/index.html
                        git add deploy/deploy.yaml todos/templates/todos/index.html
                        git commit -m "Updated the deploy yaml for build '${BUILD_NUMBER}'"
                        git remote -v
                        git push https://${GITHUB_TOKEN}@github.com/saireddysatishkumar/001-Project.git HEAD:main
                        '''                        
                    }
                }
            }
        }
    }
}