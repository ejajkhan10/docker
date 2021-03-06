pipeline {
    agent any 
    stages {
        stage('Compile and Clean') { 
            steps {

                sh "mvn clean compile"
            }
        }
        stage('Test') { 
            steps {
                sh "mvn test site"
            }
            
             post {
                always {
                    junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'   
                }
            }     
        }

        stage('deploy') { 
            steps {
                sh "mvn package"
            }
        }


        stage('Build Docker image'){
            steps {
                sh 'docker build -t ejajkhan10/dockerrepo:${BUILD_NUMBER} .'
            }
        }

        stage('Docker Login'){
            
            steps {
               withCredentials([string(credentialsId: '', variable: 'dockerhub')]) {
                    sh "docker login -u ejajkhan10 -p ${dockerhub}"
                }
            }                
        }

        stage('Docker Push'){
            steps {
                sh 'docker push ejajkhan10/dockerrepo:${BUILD_NUMBER}'
            }
        }
        
        stage('Docker deploy'){
            steps {
                sh 'docker run -itd -p 8081:8080 ejajkhan10/springboot:0.0.3'
            }
        }

        
        stage('Archving') { 
            steps {
                 archiveArtifacts '**/target/*.jar'
            }
        }
    }
}


