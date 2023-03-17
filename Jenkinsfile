def registry = 'https://testfrog21.jfrog.io'
def imageName = 'testfrog21.jfrog.io/mytwittertrend-docker/twittertrend'
def version = '2.0.2'
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
                git branch: 'main', credentialsId: '540e877e-667e-47d2-b7ef-695ea1aefe6d', url: 'https://github.com/rajasb21/Build_Twittertrend.git'
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
                echo '<--------------Code Analysis Started------------->'
                withSonarQubeEnv('SONARQUBE_SERVER') {
                    sh "${scannerHome}/bin/sonar-scanner"
                echo '<--------------Code Analysis Stopped------------->'    
                }
            }
        }
        stage("Quality Gate") {
            steps {
                script {
                    echo '<-----------------Quality Gate Started-------------->'
                    timeout(time: 1, unit: 'HOURS') {
                        def qg = waitForQualityGate()
                        if(qg.status !='OK') {
                            error "Pipeline failed due to quality gate failures: ${qg.status}"
                        }
                        
                    }
                }
            }
        }
        stage("jar publish") {
            steps {
                script {
                    echo '<--------------Jar Publish Started---------------->'
                    def server = Artifactory.newServer url:registry+"/artifactory" , credentialsId:"JFrog-Token"
                    def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}"
                    def uploadSpec = """{
                        "files": [
                          {
                            "pattern": "jarstaging/(*)",
                            "target": "twittertrend_build-libs-release-local/{1}",
                            "flat": "false",
                            "props": "${properties}",
                            "exclusions": ["*.sha1", "*.md5"]
                           }
                        ]
                    }"""
                    def buildInfo = server.upload(uploadSpec)
                    buildInfo.env.collect()
                    server.publishBuildInfo(buildInfo)
                    echo '<--------------Jar Publish Ended----------------->'
                }
            }
                
        }
        stage("Docker Build") {
            steps {
                script {
                    echo '<----------Docker Build Started------------>'
                    app = docker.build(imageName+":"+version)
                    echo '<----------Docker Build Ends---------------->'
                }
            }
        }
        stage("Docker Publish") {
            steps {
                script {
                    echo '<-------------Docker Publish Started-------------->'
                    docker.withRegistry(registry, 'JFrog-Token'){
                        app.push()
                        
                    }
                    echo '<--------------Docker Publish Ended---------------->'
                }
            }
        }
        stage("Permission") {
            steps {
                sh 'chmod +x deploy.sh'
            }
        }
        stage("Deploy") {
            steps {
                sh './deploy.sh'
            }
        }
            
    }
}
