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
                git branch: 'main', credentialsId: 'd7e87e01-8f7c-4e27-8206-bec0ccc24af6', url: 'https://github.com/rajasb21/Build_Twittertrend.git'
            }
        }
        stage("Build") {
            steps {
                sh 'mvn clean deploy'
            }
        }
        stage("Code Analysis") {
            environment {
                scannerHome = tool 'SONARQUBE_SCANNER'
            }
            steps {
                echo '<--------------Sonar Analysis Started------------------->'
                withSonarQubeEnv('SONARQUBE_SERVER') {
                    sh "${scannerHome}/bin/sonar-scanner"
                echo '<--------------Sonar Analysis Stopped------------------->'
                }
            }
        }
        stage("Quality Gate") {
            steps {
                script {
                    echo '<---------------------Quality Gate Analysis Started------------------------->'
                    timeout(time: 1, unit: 'HOURS'){
                        def qg = waitForQualityGate()
                        if(qg.status !='OK') {
                            error "Pipeline failed due to quality gate failures: ${qg.status}"
                        }
                    }
                }
            }
        }
        
    }
}    



