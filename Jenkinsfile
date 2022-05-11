pipeline {
    agent any

    stages {
        stage('Begin'){
            steps {
                sh 'echo Begin'
            }
        }
        stage('Midle'){
            steps {
                sh 'echo Midle'
                sh 'echo Midle again'
            }
        }
        stage('End'){
            steps {
                sleep(5)
                sh 'echo End'
            }
        }
    }
}