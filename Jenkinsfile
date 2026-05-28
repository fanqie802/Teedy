pipeline {
    agent any
    tools {
        maven 'Maven3'
        jdk 'JDK17'
    }
    stages {
        stage('Maven Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('PMD Code Check') {
            steps {
                sh 'mvn pmd:pmd'
            }
            post {
                always { pmd pattern: 'target/pmd.xml' }
            }
        }
        stage('Run Tests') {
            steps {
                sh 'mvn test'
            }
            post {
                always { junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml' }
            }
        }
        stage('Generate JavaDoc') {
            steps {
                sh 'mvn javadoc:jar'
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'target/*.jar, target/site/**, target/*-javadoc.jar', fingerprint: true
        }
    }
}
