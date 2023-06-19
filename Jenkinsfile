pipeline {
  agent any
  stages {
    stage('Compile') {
      steps {
        sh './mvnw clean compile'
      }
    }

    stage('Static Analysis') {
      steps {
        sh '''mvn clean verify sonar:sonar \\




  -Dsonar.projectKey=Petclinic \\
  -Dsonar.projectName=\'Petclinic\' \\
  -Dsonar.host.url=http://172.31.6.113:9000 \\
  -Dsonar.token=sqp_b54fc4a820a0650feb6970e8a850ee16b20b9969'''
      }
    }

  }
}