pipeline {
    agent {
        docker {
            image 'maven:3.9.0'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deploy') {
            steps {
		input message: 'Lanjutkan ke tahap Deploy? (Klik "Proceed" untuk melakukan deployment)'
                sh './jenkins/scripts/deliver.sh'
		sleep time : 1, unit :'MINUTES'		
            }
        }
    }
}
