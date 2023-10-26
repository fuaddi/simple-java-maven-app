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
        
	stage('Manual Aproval') {
            steps {
                script {
                    // Meminta masukan dari pengguna
                    def userInput = input(
                        id: 'userInput',
                        message: 'Lanjut ke tahap deploy?',
                        parameters: [choice(name: 'Pilihan', choices: 'Ya,Tidak')]
                    )

                    if (userInput == 'Ya') {
                        echo 'Melanjutkan ke tahap deployment'
                    } else {
                        echo 'Menghentikan alur kerja...'
                        currentBuild.result = 'ABORTED'
                    }
                }
            }
        }
	
	stage('Deploy') {
            steps {
                sh './jenkins/scripts/deliver.sh'
		sleep time : 1, unit :'MINUTES'		
            }
        }
    }
}
