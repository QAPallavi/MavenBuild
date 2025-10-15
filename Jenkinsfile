pipeline {
    agent any

    tools {
        maven 'Maven3' // Configure this name under Jenkins > Global Tool Configuration
        jdk 'JDK21'     // Configure your JDK under Jenkins > Global Tool Configuration
    }

    stages {
        stage('Checkout from Git') {
            steps {
                echo 'Cloning source code from GitHub...'
                git branch: 'master', url: 'https://github.com/QAPallavi/MavenBuild.git'
            }
        }

        stage('Build with Maven') {
            steps {
                echo 'Running Maven clean install...'
                bat '"C:\\ECIPSE\\apache-maven-3.9.9-bin\\apache-maven-3.9.9\\bin\\mvn" clean install'
            }
        }

        stage('Test Execution with JUnit') {
            steps {
                echo 'Running JUnit tests...'
                bat '"C:\\ECIPSE\\apache-maven-3.9.9-bin\\apache-maven-3.9.9\\bin\\mvn" test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build completed successfully!'
        }
        failure {
            echo '❌ Build failed. Check logs for details.'
        }
    }
}
