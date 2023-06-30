pipeline{
    agent any

    stages {
      
        stage("CI"){
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus_credentials',usernameVariable: 'NEXUS_USERNAME',passwordVariable: 'NEXUS_PASS')]){
                  sh 'docker build -t 10.96.38.38:5000/repository/docker-repo/simple-node-app:latest .'
                  sh 'docker login 10.96.38.38:5000 -u ${NEXUS_USERNAME}  -p ${NEXUS_PASS}'
                  sh 'docker push 10.96.38.38:5000/repository/docker-repo/simple-node-app:latest'
                }
            }
        }
        stage("CD") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus_credentials',usernameVariable: 'NEXUS_USERNAME',passwordVariable: 'NEXUS_PASS')]){
                  ah 'kubectl create secret docker-registry regcred   --namespace=dev   --docker-server=10.97.178.100:5000   --docker-username=${NEXUS_USERNAME}   --docker-password=${NEXUS_PASS}   --docker-email=admin@example.org'
                  sh 'docker login 10.96.38.38:5000 -u ${NEXUS_USERNAME}  -p ${NEXUS_PASS}'
                  sh 'kubectl apply -f sql-secret.yaml'
                  sh 'kubectl apply -f sql-deployment.yaml'
                  sh 'kubectl apply -f sql-svc.yaml'
                  sh 'kubectl apply -f node-configmap.yaml'
                  sh 'kubectl apply -f node-deployment.yaml'
                  sh 'kubectl apply -f node-svc.yaml '
                }
            }
        }
    }
}