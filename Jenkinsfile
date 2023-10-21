node {
    stage('Build') {
        try {
            // Container Docker dengan gambar 'maven:3.9.0'
            def dockerImage = docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2') {
                // Jalankan perintah Maven untuk build
                sh 'mvn -B -DskipTests clean package'
            }
        } finally {
            // Jika terjadi kesalahan, bersihkan kontainer
            dockerImage.stop()
        }
    }

    // Stage 'Test'
    stage('Test') {
        // Jalankan perintah Maven untuk menjalankan tes
        sh 'mvn test'

        // Post-build: Gunakan plugin JUnit untuk melaporkan hasil tes
        junit 'target/surefire-reports/*.xml'
    }

    // Stage 'Deliver'
    stage('Deliver') {
        // Jalankan skrip pengiriman yang sesuai dengan proyek Anda
        sh './jenkins/scripts/deliver.sh'
    }
}
