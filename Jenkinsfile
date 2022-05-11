pipeline {
    agent any

    stages {
        stage('Begin'){
            steps {
                sh 'echo Begin'
            }
        }
        stage('Meio'){
            steps {
                sh 'echo Midle'
                sh 'echo Midle again'
            }
        }
        stage('Fim'){
            steps {
                sleep(5)
                sh 'echo End'
            }
        }
    }
}