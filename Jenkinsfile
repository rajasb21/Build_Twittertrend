pipeline{
    agent {
        node {
            label "Slave1"
        }
    }    
    environment {
        PATH = "/opt/apache-maven-3.9.0/bin:$PATH"
    }    
    stages{
        stage("git clone") {
            steps {
                git branch: 'main', credentialsId: '4309365a-da2e-4ef7-b13f-23443ee79697', url: 'https://github.com/rajasb21/Build_Twittertrend.git'
            }
        }
        stage("build") {
            steps {
                sh 'mvn clean install'
            }
        }
        
        stage("sonarqube analysis") {
            environment {
                scannerHome = tool 'SONARQUBE_SCANNER'
            }
            steps {
                echo '<--------------Sonar Analysis Started-------------->'
                withSonarQubeEnv('SONARQUBE_SERVER') {
                    sh "${scannerHome}/bin/sonar-scanner"
                echo '<--------------Sonar Analysis Stopped-------------->'    
                }
            }
        }
    }
}    

