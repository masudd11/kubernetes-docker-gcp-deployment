pipeline {
    agent any
	tools {
		maven 'Maven'
	}
	
	environment {
		PROJECT_ID = 'devops-374608'
                CLUSTER_NAME = 'devops'
                LOCATION = 'us-central1'
                CREDENTIALS_ID = 'kubernetes'
				build = '1'	
	}           
	
    stages {
	    stage('Scm Checkout') {
		    steps {
			    checkout scm
		    }
	    }
	    
	    stage('Build') {
		    steps {
			    sh 'mvn clean package'
		    }
	    }
	    
	    stage('Test') {
		    steps {
			    echo "Testing..."
			    sh 'mvn test'
		    }
	    }
	    
	    stage('deploy') {
		    steps {
			   sshagent(['myjavaid']) {
                           // some block
				   sh 'scp - -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/project/temp/app.war ubuntu@3.108.66.159:/opt/tomcat10/webapps/'
                           }
// 	    stage('Build Docker Image') {
// 		    steps {
// 			    sh 'whoami'
// 		            script {
//  				    def myimage = docker.build("masudd11/devops:${BUILD_ID}")
// 				    myimage.push()
// 			    }
// 			    sh 'sudo docker image build -t masudd11/devops:${BUILD_ID} .'
// 		    }
// 	    }
	    
// 	    stage("Push Docker Image") {
// 		    steps {
// 			    script {
// 				    echo "Push Docker Image"
// 				    withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) {
//             				sh "docker login -u masudd11 -p ${dockerhub}"
// 				    }
// 				        myimage.push("${env.BUILD_ID}")
				    
// 			    }
// 		    }
// 	    }
	    
// 	    stage('Deploy to K8s') {
// 		    steps{
// 			    echo "Deployment started ..."
// 			    sh 'ls -ltr'
// 			    sh 'pwd'
// 			    sh "sed -i 's/tagversion/${env.BUILD_ID}/g' serviceLB.yaml"
// 				sh "sed -i 's/tagversion/${env.BUILD_ID}/g' deployment.yaml"
// 			    echo "Start deployment of serviceLB.yaml"
// 			    step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'serviceLB.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
// 				echo "Start deployment of deployment.yaml"
// 				step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
// 			    echo "Deployment Finished ..."
// 		    }
// 	    }
    }
}
