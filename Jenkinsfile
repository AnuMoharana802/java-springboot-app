pipeline {
    agent {
        node {
            label 'jenkins-slave-node'
        }
    }
    
    environment {
        PATH = "/opt/apache-maven-3.9.6/bin:$PATH"
    }

    stages {
        stage("Build") {
            steps {
                echo "----------- Build Started ----------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo "----------- Build Completed ----------"
            }
        }

        /*
        Uncomment and configure the following stages as needed:

        stage('SonarQube analysis') {
            // ...
        }

        stage("Quality Gate") {
            // ...
        }

        stage("Artifact Publish") {
            // ...
        }
        */

        stage("Create Docker Image") {
            steps {
                script {
                    echo '-------------- Docker Build Started -------------'
                    def app = docker.build("projectpotal.jfrog.io/meportal-docker-local/myapp:1.0")
                    echo '-------------- Docker Build Ended -------------'
                }
            }
        }

        stage("Docker Publish") {
            steps {
                script {
                    echo '---------- Docker Publish Started ---------'  
                    docker.withRegistry("https://projectpotal.jfrog.io", 'jforg-cred') {
                        app.push()
                    }
                    echo '------------ Docker Publish Ended ---------'
                }
            }
        }
    }
}
