pipeline {
    agent any
    stages {
        stage('Lint HTML') {
            steps {
                sh 'tidy -q -e *.html'
            }
        }
        stage('Build image') {
            steps {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
                    sh '''
                            sudo -n docker build -t duodev/ucdncapstonecluster .
                    '''
                }
            }
        }
        stage('Push Image To Dockerhub') {
            steps {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
                    sh '''
                            sudo docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
                            sudo docker push duodev/ucdncapstonecluster
                    '''
                }
            }
        }
        stage('Set current kubectl context') {
            steps {
                withAWS(region:'eu-central-1',credentials:'udacity-capstone') {
                    sh '''
                        sudo -s
			echo "start set context"
			aws eks --region eu-central-1 update-kubeconfig --name ucdncapstonecluster
			kubectl config get-contexts    
                        kubectl config use-context arn:aws:eks:eu-central-1:862214991036:cluster/ucdncapstonecluster
                    	echo "finish set context"
		    '''
                }
            }
        }
	stage('deployment') {
            steps {
                withAWS(region:'eu-central-1',credentials:'udacity-capstone') {
                    sh '''
                        sudo -s
                        echo "start deployment"
                        kubectl apply -f capstone/deployments/rolling.yaml
			kubectl get nodes
			kubectl get deployment
			kubectl get pod -o wide
			kubectl get service/ucdncapstonecluster
                        echo "finish deployments"
                    '''
                }
            }
        }
    }
}

