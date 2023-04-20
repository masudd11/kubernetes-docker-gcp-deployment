pipeline {
    agent any
	tools {
		maven 'Maven'
	}
	
	environment {
	PROJECT_ID = 'kubernetes-gcp-383911'
        CLUSTER_NAME = 'my-gke-cluster'
        LOCATION = 'us-central1-b'
        CREDENTIALS_ID = 'kubernetes-gcp-383911'	
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
// 			    sh 'docker build -t masudd11/gcp-kubernetes:${BUILD_NUMBER} .'
			    sh 'docker build -t gcr.io/${PROJECT_ID}/javaproject:${BUILD_NUMBER} .'
			}
		}
	    
	    stage("Push Docker Image") {
		    steps {
				withDockerRegistry(credentialsId: 'gcr:kubernetes-gcp-383911', url: 'https://gcr.io') { 					sh  'docker push gcr.io/${PROJECT_ID}/javaproject:${BUILD_NUMBER}'
			           sh 'docker push gcr.io/${PROJECT_ID}/javaproject:${BUILD_NUMBER}'
 		        }
// 			    withDockerRegistry(credentialsId:'dockerid', url: '') {
// 				    sh 'docker push masudd11/gcp-kubernetes:${BUILD_NUMBER}'
// 			}
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
