pipeline{
    agent {
        node {
            label "Slave-Node-2"
        }
    }
    environment {
        PATH = "/opt/apache-maven-3.9.0/bin:$PATH"
    }
    
    stages {
        stage("git clone") {
            steps{
                git branch: 'main', credentialsId: '00e0c593-5358-42f8-9280-d37679fc6ea4', url: 'https://github.com/rajasb21/Build_Twittertrend.git'
            }
        }    
            
        stage("Build") {
            steps{
                sh mvn clean install
            }
        }    
        
    }   
    
 }
    
