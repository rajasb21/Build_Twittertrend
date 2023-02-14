pipeline{
    agent {
        node {
            label "Slave2"
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
                sh 'mvn clean install'
            }
        }
    }
}
