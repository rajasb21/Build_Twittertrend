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
		git branch: 'main', credentialsId: '4309365a-da2e-4ef7-b13f-23443ee79697', url: 'https://github.com/rajasb21/Build_Twittertrend.git'
            }
        }    
            
        stage("Build") {
            steps{
                sh 'mvn clean install'
            }
        }    
        
    }   
    
 }
    
