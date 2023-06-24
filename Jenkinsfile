pipeline {
  agent {
    node {
      label 'test'
    }

  }
  stages {
    stage('Compile') {
      steps {
        sh './mvnw clean compile'
      }
    }

    stage('Static Analysis') {
      steps {
        sh '''./mvnw sonar:sonar \\
  -Dsonar.projectKey=Petclinic  \\
  -Dsonar.host.url=http://172.31.6.113:9000 \\
  -Dsonar.token=sqp_30c552da8a3aaa1c021d906b8d957ec05882f243'''
      }
    }

    stage('Unit Test') {
      steps {
        sh './mvnw "-Dtest=**/petclinic/*/*.java" test'
      }
    }

    stage('Package') {
      steps {
        sh './mvnw package -DskipTests=true'
      }
    }

    stage('QA') {
      parallel {
        stage('Deploy') {
          agent {
            node {
              label 'test'
            }

          }
          steps {
            sh './mvnw spring-boot:run < /dev/null'
          }
        }

        stage('Integration and Performance Tests') {
          agent {
            node {
              label 'test'
            }

          }
          steps {
            sh './mvnw verify'
            perfReport '**/target/jmeter/results/*'
            junit(testResults: '**/target/surefire-reports/*', allowEmptyResults: true)
          }
        }

      }
    }

  }
}