node {
    // Alur kerja akan dieksekusi pada slave/agent Jenkins

    // Tahap 'Build'
    stage('Build') {
        // Tahap ini akan menggunakan Docker untuk menjalankan langkah-langkah build
        def mavenImage = docker.image('maven:3.9.0').run("-v /root/.m2:/root/.m2")
        try {
            sh 'mvn -B -DskipTests clean package'
        } finally {
            mavenImage.stop()
        }
    }

    // Tahap 'Test'
    stage('Test') {
        // Tahap ini juga akan menggunakan Docker untuk menjalankan langkah tes
        def mavenImage = docker.image('maven:3.9.0').run("-v /root/.m2:/root/.m2")
        try {
            sh 'mvn test'
            archiveArtifacts allowEmptyArchive: true, artifacts: 'target/surefire-reports/*.xml'
        } finally {
            mavenImage.stop()
        }
    }

    // Tahap 'Deliver'
    stage('Deliver') {
        // Tahap ini menjalankan skrip deliver.sh
        sh './jenkins/scripts/deliver.sh'
    }
}
