pipeline {
    agent any
    options {
        ansiColor('xterm')
    }

    stages {
        stage('Build BackEnd'){
            steps {
                echo '\033[34mStart\033[0m \033[33mbuild\033[0m \033[35mBackend!\033[0m'
                sh 'mvn clean package -DskipTests=true'
            }
        }
        stage('Unit Tests'){
            steps {
                sh 'mvn test'
            }
        }
        // stage('Sonar Analysis'){
        //     environment{
        //         scannerHome = tool 'SONAR_SCANNER'
        //     }
        //     steps {
        //         withSonarQubeEnv('SONAR_LOCAL'){
        //             sh "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=12345123 -Dsonar.java.binaries=target -Dsonar.coverage=exclusions=**/.mvn/**,**/src/test/**,**/model/**"
        //         }
        //     }
        // }
        // stage('Quality Gate'){
        //     steps{
        //         sleep(10)
        //         timeout(time: 1, unit: 'MINUTES'){
        //             waitForQualityGate abortPipeline: true
        //         }
        //     }
        // }
        stage('Deploy BackEnd'){
            steps{
                deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }
        stage('API Tests'){
            steps{
                dir('api-test'){
                    git credentialsId: 'github_login', url: "https://github.com/amtrindade/tasks-api-test"
                    sh 'mvn test'
                }
            }
        }
        stage('Build and Deploy FrontEnd'){
            steps{
                dir('frontend'){
                    git credentialsId: 'github_login', url: "https://github.com/amtrindade/tasks-frontend"  
                    sh 'mvn clean package'
                    deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
                }
            }
        }
        stage('Functional Tests FrontEnd'){
            steps{
                dir('functional-test'){
                    git credentialsId: 'github_login', url: "https://github.com/amtrindade/tasks-functional-tests"
                    sh 'mvn test'
                }
            }
        }
        stage('Deploy Prod'){
            steps{
                sh 'docker-compose build'
                sh 'docker-compose up -d'
            }
        }
        stage('Health Check'){
            steps{
                sleep(10)
                dir('functional-test'){                    
                    sh 'mvn verify -Dskip.surefire.tests'
                }
            }
        }
    }

    post {
        always{
            junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml','api-test/target/surefire-reports/*.xml', 'functional-test/target/surefire-reports/*.xml', 'functional-test/target/failsafe-reports/*.xml'
        }
    }
}


