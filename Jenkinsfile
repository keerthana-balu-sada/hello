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
                sh "docker build -t ${env.IMAGE_NAME}:${env.BUILD_NUMBER} ."
            }
        }
        stage('push') {
            steps {
                sh "docker push keerthanabk/java-demo:latest"
            }
        }
      
    }
}
