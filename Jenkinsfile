node {
    // Alur kerja akan dieksekusi pada Jenkins agent

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
        // Post-build action: JUnit test results
        junit 'target/surefire-reports/*.xml'
    }

    // Tahap 'Deliver'
    stage('Deliver') {
        // Tahap ini menjalankan skrip deliver.sh dalam direktori saat ini
        sh './jenkins/scripts/deliver.sh'
    }
}
