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
        sh 'echo "static analysis"'
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
            sh './mvnw spring-boot:run </dev/null &>/dev/null &'
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