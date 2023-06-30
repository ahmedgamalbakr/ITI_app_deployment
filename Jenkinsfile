<<<<<<< HEAD
pipeline {
    agent any

    stages {
        
        stage('ci') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'DOCKERPASS', usernameVariable: 'DOCKERENAME')]) {
                    sh """ 
                    docker build . -t iti/simple-node-app
                    docker login nexus-svc.tools -u ${DOCKERENAME} -p ${DOCKERPASS}
                    docker push iti/simple-web-app
                    """
                }
            }
        }
        stage('cd') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'DOCKERPASS', usernameVariable: 'DOCKERENAME')]) {
                    sh """
                    docker login nexus-svc.tools -u ${DOCKERENAME} -p ${DOCKERPASS}
                    kubectl apply -f deployment.yaml
                    kubectl apply -f svc.yaml
                    """
=======
pipeline{
    agent any

    stages {
      
        stage("CI"){
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus_credentials',usernameVariable: 'NEXUS_USERNAME',passwordVariable: 'NEXUS_PASS')]){
                  sh 'docker build -t 10.107.200.194:5000/repository/docker-repo/simple-node-app:latest .'
                  sh 'docker login 10.107.200.194:5000 -u ${NEXUS_USERNAME}  -p ${NEXUS_PASS}'
                  sh 'docker push 10.107.200.194:5000/repository/docker-repo/simple-node-app:latest'
                }
            }
        }
        stage("CD") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus_credentials',usernameVariable: 'NEXUS_USERNAME',passwordVariable: 'NEXUS_PASS')]){
                  sh 'kubectl create secret docker-registry regcred --namespace=dev --docker-server=10.107.200.194:5000 --docker-username=${NEXUS_USERNAME}  --docker-password=${NEXUS_PASS} --docker-email=admin@example.org'
                  sh 'docker login 10.107.200.194:5000 -u ${NEXUS_USERNAME}  -p ${NEXUS_PASS}'
                  sh 'kubectl apply -f app-deployment.yaml'
                  sh 'kubectl apply -f app-svc.yaml'
>>>>>>> ecaeee6a8fd754e62e79f4281f564a3d70adc203
                }
            }
        }
    }
}
