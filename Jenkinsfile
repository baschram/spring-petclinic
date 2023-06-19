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
        sh '''./mvnw sonar:sonar 
\\   -Dsonar.projectKey=spring-petclinic 
  
\\ -Dsonar.host.url=http://172.31.6.113:9000/ 
  \\ -Dsonar.token=sqa_3deabbfd8cb8ecf1a2b70f35fdb2c9917f8cba89'''
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
            junit '**/target/surefire-reports/'
            perfReport '**/target/jmeter/results/*'
          }
        }

      }
    }

  }
}