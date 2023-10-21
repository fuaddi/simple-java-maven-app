node {
    // Alur kerja akan dieksekusi pada slave/agent Jenkins

    // Tahap 'Build'
    stage('Build') {
        // Tahap ini akan menggunakan Docker untuk menjalankan langkah-langkah build
        docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2') {
            sh 'mvn -B -DskipTests clean package'
        }
    }

    // Tahap 'Test'
    stage('Test') {
        // Tahap ini juga akan menggunakan Docker untuk menjalankan langkah tes
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
        steps {
            sh './jenkins/scripts/deliver.sh'
        }
    }
}
