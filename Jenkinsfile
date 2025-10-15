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
                credentialsId: 'githubcreds',
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
