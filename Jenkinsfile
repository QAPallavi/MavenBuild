node {

    // SonarScanner tool (optional, uncomment if using SonarQube)
    // def sonarHome = tool name: 'SonarScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'

    stage('Code Checkout') {
        checkout changelog: false, poll: false, scm: [
            $class: 'GitSCM',
            branches: [[name: '*/master']],
            doGenerateSubmoduleConfigurations: false,
            extensions: [],
            userRemoteConfigs: [[
                credentialsId: 'QAPallavi',
                url: 'https://github.com/QAPallavi/MavenBuild'
            ]]
        ]
    }

    stage('Build Automation') {
    def mvnHome = tool name: 'Maven3', type: 'maven'
    bat """
        echo Listing workspace files:
        dir
        echo Running Maven build:
        "${mvnHome}\\bin\\mvn" clean install
        echo Listing target directory:
        dir target
        """
    }

    stage('Code Scan') {
        /*
        withSonarQubeEnv(credentialsId: 'SonarQubeCreds') {
            bat "\"${sonarHome}\\bin\\sonar-scanner.bat\""
        }
        */
    }

    stage('Code Coverage') {
        // PowerShell version for Windows (optional)
        // bat """
        // powershell -Command "
        //   \$coverage = Invoke-RestMethod -Uri 'http://35.154.151.174:9000/sonar/api/measures/component?componentKey=com.java.example:java-example&metricKeys=coverage';
        //   if (\$coverage.component.measures.value -lt 50) { exit 1 }
        // "
        // """
    }

    stage('Code Deployment') {
     //   deploy adapters: [
     //       tomcat9(
     //           credentialsId: 'tomcatcreds',
     //           path: '',
     //           url: 'http://localhost:9090/'
     //       )
     //   ],
     //   contextPath: 'Planview',
     //   onFailure: false,
     //   war: 'target/*.war'
    }
}

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
                bat '"C:\\ECIPSE\\apache-maven-3.6.3-bin\\apache-maven-3.6.3\\bin\\mvn" clean install'
            }
        }

        stage('Test Execution with JUnit') {
            steps {
                echo 'Running JUnit tests...'
                bat '"C:\\ECIPSE\\apache-maven-3.6.3-bin\\apache-maven-3.6.3\\bin\\mvn" test'
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
