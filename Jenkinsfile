
pipeline{

	agent any

	
	stages {

		stage('Build') {

			steps {
				sh 'sudo docker build -t patelsaheb/hellonodejs:eks .'
               
			}
		}
        
        stage('login') {

            steps {
        
		        withCredentials([string(credentialsId: 'DOCKER_PWD', variable: 'PASSWORD')]) {
                    sh 'sudo docker login -u patelsaheb -p $PASSWORD'
                }
            }
        }
		stage('Push') {

			steps {
				sh 'sudo docker push patelsaheb/hellonodejs:eks'
			}
		}

        stage('eks deploy') {

			steps {
				sh 'kubectl get -o yaml deploy/hello-world-nodejs > deploy.yaml'
                sh "sed -i 's/hellonodejs:latest/hellonodejs:eks/g' deploy.yaml"
                sh 'kubectl apply -f deploy.yaml'
                sh 'kubectl rollout restart deployment hello-world-nodejs'
			}
		}
	}

	post {
		always {

            		cleanWs()
			
            		echo "done"
		}
	}

}

