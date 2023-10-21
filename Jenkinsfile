node {
    // Tahap 'Build'
    stage('Build') {
        // Menggunakan Docker sebagai agen
        docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2') {
            sh 'mvn -B -DskipTests clean package'
        }
    }

    // Tahap 'Test'
    stage('Test') {
        docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2') {
            sh 'mvn test'
        }
        post {
            always {
                junit 'target/surefire-reports/*.xml'
            }
        }
    }

    // Tahap 'Deliver'
    stage('Deliver') {
        docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2') {
            sh './jenkins/scripts/deliver.sh'
        }
    }
}
