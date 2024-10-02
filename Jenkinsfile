pipeline{
    agent {label 'yash'}
    
    stages{
       stage('pull the code'){
           steps{
               git branch: 'compose', url: 'https://github.com/Yashgoswami14/containerized_mern_application'
           }
       }
       
       stage('building the image'){
           steps{
               sh '''cd mern/backend/
                    docker build -t backend:latest .

                    cd ../frontend/
                    docker build -t frontend:latest .
                '''
           }
       }
       
       stage('pushing image to dockerhub'){
           steps{
               withCredentials([usernamePassword(credentialsId:"dockerHubCred",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
               sh 'docker login -u ${dockerHubUser} -p ${dockerHubPass}'
               sh 'docker tag backend:latest yashgoswami14/backend:latest'
               sh 'docker tag frontend:latest yashgoswami14/frontend:latest'
               sh 'docker push yashgoswami14/backend:latest'
               sh 'docker push yashgoswami14/frontend:latest'
           }
           }
       }
       
       stage('deploying the code'){
           steps{
               sh 'docker compose up -d'
           }
       }
    }
}
