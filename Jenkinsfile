#!/usr/bin/env groovy

pipeline {
	 
    agent any
    
    environment {
        imgtag="hemantkbajaj/helloworld:${env.BUILD_NUMBER}"
    }   	    
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
		sh("docker -H 10.3.20.117:5555 build --build-arg http_proxy=http://web-proxy.corp.hpecorp.net:8080 --build-arg https_proxy=http://web-proxy.corp.hpecorp.net:8080 -t ${imgtag} .")
            }
        }
        }
	stage('Test') {
           steps {
               echo 'Testing hello world'
            }
	}
	stage('Push') {
           steps {
               echo 'Pushing Image to Docker hub'
	       sh("docker -H :5555 login -u hemantkbajaj -p hunny2744")
	       sh("docker -H :5555 push ${imgtag}")	       
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying in kubectl !!!'
		sh("sed -i.bak 's#paulbouwer/hello-kubernetes:1.8#${imgtag}#' ./yaml/hello-kubernetes.custom-message.yaml")
		sh("kubectl apply -f ./yaml/hello-kubernetes.custom-message.yaml")		
            }
        }
    }
}
