pipeline {
    agent any
    tools{
        maven 'maven-tool'
    }
    stages {
        stage('Build') {
            steps {
                echo "build"
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
        stage('build') {
            steps {
                sh "docker build -t keerthanabk/java-demo:${env.BUILD_NUMBER} ."
            }
        }
        stage('Push image to Docker') {
            steps{
                withCredentials([string(credentialsId: 'jenkins-pat-keerthana', variable: 'PASSWORD')]) {
                    sh 'docker login -u keerthanabk -p $PASSWORD' 
                    sh "docker push keerthanabk/java-demo:${env.BUILD_NUMBER}"
                    
                }
            }
        }
    }   
}
