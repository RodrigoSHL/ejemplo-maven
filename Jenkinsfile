pipeline {
    agent any

    stages {
        stage('Compile') {
            steps {
                script {
                    sh './mvnw clean compile -e'
                }
            }
        }
        stage('SonarQube analysis') {
            def scannerHome = tool 'sonar-scanner';
            withSonarQubeEnv('sonarqube-server') { // If you have configured more than one global server connection, you can specify its name
            sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=ejemplo-maven -Dsonar.sources=src -Dsonar.java.binaries=build"
            }
        }
        stage('Compile') {
            steps {
                script {
                    sh './mvnw clean compile -e'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    sh './mvnw clean test -e'
                }
            }
        }
        stage('Jar') {
            steps {
                script {
                    sh './mvnw clean package -e'
                }
            }
        }
        stage('Run') {
            steps {
                script {
                    sh 'nohup bash mvnw spring-boot:run &'
                    sleep 20
                }
            }
        }
        stage('TestApp') {
            steps {
                script {
                    sh "curl -X GET 'http://localhost:8081/rest/mscovid/test?msg=testing'"
                }
            }
        }
    }
}
