pipeline{
    agent any

    stages {
      
        stage("CI"){
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus_credentials',usernameVariable: 'NEXUS_USERNAME',passwordVariable: 'NEXUS_PASS')]){
                  sh 'docker build . -t nexuspath/simple-node-app'
                  sh 'docker login IP:PORT -u ${NEXUS_USERNAME}  -p ${NEXUS_PASS}'
                  sh 'docker push nexuspath/simple-node-app'
                }
            }
        }
        stage("CD") {
            steps {
                
                        sh """ 
                            kubectl apply -f app-deployment.yaml
                            kubectl apply -f app-svc.yaml
                        """
                    
                

            }
        }
    }
}