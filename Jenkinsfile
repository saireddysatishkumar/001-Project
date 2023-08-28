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
     /*   /* enable it only if deployment manifests are in other repo.
        stage('Checkout K8S manifest SCM'){
            steps {
                git credentialsId: 'f87a34a8-0e09-45e7-b9cf-6dc68feac670', 
                url: 'https://github.com/saireddysatishkumar/001-Project.git',
                branch: 'main'
            }
        }
       */  
        stage('Update K8S manifest & push to Repo'){
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
