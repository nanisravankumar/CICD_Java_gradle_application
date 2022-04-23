pipeline{
    agent any
    environment {
        VERSION = "${env.BUILD_ID}"
    }
    stages {
        stage("sonar qube code quality analysis"){
            agent {
                docker {
                    image = 'openjdk:8'
                }
            }
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube'
                    }
                }
            }
        }
        stage("docker build & push"){
            steps {
                script {
                    withCredentials([string(credentialsId: 'docker_pass', variable: 'docker-password')]) {
                        sh '''
                            docker build -t 54.208.44.84:8083/springapp:$VERSION .
                            docker login -u admin -p $docker-password 54.208.44.84:8083
                            docker push 54.208.44.84:8083/springapp:${VERSION} .
                            docker rmi 54.208.44.84:8083/springapp:${VERSION}
                        '''
                    }
                }
            }
        }

    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}