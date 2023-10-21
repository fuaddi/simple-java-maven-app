node {
    // Alur kerja akan dieksekusi pada slave/agent Jenkins

    // Tahap 'Checkout' dan cek Docker image
    stage('Checkout and Docker Image Check') { 
	// Mengecek apakah image Docker 'maven:3.9.0' sudah ada
        def dockerImageExists = sh(script: 'docker image inspect maven:3.9.0', returnStatus: true) == 0

        if (!dockerImageExists) {
            // Jika image tidak ada, unduh image Docker 'maven:3.9.0'
            sh 'docker pull maven:3.9.0'
        }
    }

    // Stage 'Build'
    stage('Build') {
        // Langkah ini akan menjalankan perintah build (Maven)
        sh 'mvn -B -DskipTests clean package'
    }

    // Stage 'Test'
    stage('Test') {
        // Langkah ini akan menjalankan tes unit
        sh 'mvn test'
    }

    // Stage 'Deliver'
    stage('Deliver') {
        // Langkah ini akan melakukan deploy aplikasi
        // Gantilah dengan perintah yang sesuai dengan proyek Anda
        sh './jenkins/scripts/deliver.sh'
    }
}
