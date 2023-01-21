pipeline {
    agent any
	tools {
		maven 'Maven'
	}
	
	environment {
		PROJECT_ID = 'devops-374608'
        CLUSTER_NAME = 'cluster-1'
        LOCATION = 'us-central-1c'
        CREDENTIALS_ID = 'devops'	
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
		    steps {
			    sh 'whoami'
				
				sh 'pwd'
			    // sh 'docker build -t masudd11/javaproject:${BUILD_NUMBER} .'
				sh 'docker build -t gcr.io/devops-374608/javaproject:${BUILD_NUMBER} .'
			}
		}
	    
	    stage("Push Docker Image") {
		    steps {
				// withDockerRegistry(credentialsId: 'dockerid', url: '') {
    			withCredentials([file(credentialsId: 'filesecret', variable: 'filesecret', url: "https://gcr.io")]) {
				sh 'gcloud auth configure-docker'	
                sh  'docker push gcr.io/devops-374608/javaproject:${BUILD_NUMBER}'
			    // sh 'docker login -u masudd11 -p ${pass}' 
				// sh 'docker push masudd11/javaproject:${BUILD_NUMBER}'
		        }
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
