pipeline{
    agent any

    stages {
      
        stage("CI"){
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus_credentials',usernameVariable: 'NEXUS_USERNAME',passwordVariable: 'NEXUS_PASS')]){
                  sh 'docker build -t 10.98.11.64:5000/repository/docker-repo/simple-node-app:latest .'
                  sh 'docker login 10.98.11.64:5000 -u ${NEXUS_USERNAME}  -p ${NEXUS_PASS}'
                  sh 'docker push 10.98.11.64:5000/repository/docker-repo/simple-node-app:latest'
                }
            }
        }
        stage("CD") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus_credentials',usernameVariable: 'NEXUS_USERNAME',passwordVariable: 'NEXUS_PASS')]){
                  sh 'docker login 10.98.11.64:5000 -u ${NEXUS_USERNAME}  -p ${NEXUS_PASS}'
                  sh 'kubectl apply -f app-deployment.yaml'
                  sh 'kubectl apply -f app-svc.yaml'
                }
            }
        }
    }
}
