pipeline {
    agent any
    environment {
        registry = "80190363/ibmace"
        registryCredential = 'dockerhub'
    }
    stages {
        
        stage('Build App Connect Enterprise Docker Image') {
            when { 
                branch 'master'
            }
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                    
                }    
            }
        }
        
        stage('Push App Connect Enterprise Docker Image to Registry') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry( '', 'dockerhub' ) {
                        dockerImage.push()
                    }
                }
            }
        }   
        stage('Deploy App Connect Enterprise Openshift Container Platform') {
            when {
                branch 'master'
            }
            steps {
                script {
                    sh 'oc login --token=a2n8jceVJODcydaXR2q8z-FPhG5aKR0qb4BRaU9IMcM --server=https://c107-e.us-south.containers.cloud.ibm.com:32172'
                    sh 'oc project appconnectenterprise'    
                    sh 'oc apply -f ace-openshift-deployment-bancamovil2.yaml'
                        }  
                }
            }
        }  
}
