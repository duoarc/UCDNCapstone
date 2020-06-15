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
                        kubectl config use-context arn:aws:eks:eu-central-1:862214991036:cluster/ucdncapstonecluster
                    	echo "finish set context"
		    '''
                }
            }
        }
    }
}

