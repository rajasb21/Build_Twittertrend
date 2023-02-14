pipeline{
    agent {
        node {
            label "Slave1"
        }
    }
    environment {
        PATH = "/opt/apache-maven/3.9.0/bin:$PATH"
    }
    stages{
        stage("git clone") {
            steps {
                git branch: 'main', credentialsId: 'ba5a22fa-1520-443c-b8e7-b84abeb74eee', url: 'https://github.com/rajasb21/Build_Twittertrend.git'
                
            }
        }
        stage("build") {
            steps{
                sh 'mvn clean install'
            }
        }
        stage("Code Analysis") {
            environment {
                scannerHome = tool 'SONARQUBE_SCANNER'
            }
            steps {
                echo '<-------------------Sonar Analysis Started------------------>'
                withSonarQubeEnv('SONARQUBE_SERVER') {
                    sh "${scannerHome}/bin/sonar-scanner"
                echo '<-------------------Sonar Analysis Styopped------------------>'    
                }
            }
        }
    }
}
