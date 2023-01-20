pipeline {
    agent any
	tools {
		maven 'Maven'
	}
	
	environment {
		PROJECT_ID = 'devops-374608'
        CLUSTER_NAME = 'dev-cluster'
        LOCATION = 'us-central1'
        CREDENTIALS_ID = 'kubernetes'	
	}           
	
    stages {
		stage('Test') {
		    steps {
			    echo "Testing..."
			    sh 'mvn test'
		    }
	    }
	    stage('Build') {
		    steps {
			    sh 'mvn clean package'
		    }
	    }
	    stage('Build Docker Image') {
		    
			    sh 'whoami'
				
				sh 'pwd'
			    dockerImage = docker build("masudd11/javapp:1")
			
		}
	    
	    stage("Push Docker Image") {
		    
				                withDockerRegistry(credentialsId: 'dockersecret', url: 'masudd11/javaproject') {
                                // some block
				                dockerImage.push()
                
			}
		}
	    
	    stage('Deploy to K8s') {
		    steps{
			    echo "Deployment started ..."
			    sh 'ls -ltr'
			    sh 'pwd'
			    sh "sed -i 's/tagversion/${BUILD_NUMBER}/g' serviceLB.yaml"
				sh "sed -i 's/tagversion/${BUILD_NUMBER}/g' deployment.yaml"
			    echo "Start deployment of serviceLB.yaml"
			    step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'serviceLB.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
				echo "Start deployment of deployment.yaml"
				step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
			    echo "Deployment Finished ..."
		    }
	    }
    }
}
