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
                }
            }
        }
    }
}
