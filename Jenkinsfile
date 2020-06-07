pipeline {
  agent any
  stages {
    stage ('Initialize') {
       steps {
          sh '''
              echo "PATH = ${PATH}"
              echo "M2_HOME = ${M2_HOME}"

              '''
            }
        }
    stage('cleaning package') {
      steps {
        sh 'mvn clean install package'
      }
    }
    stage('building docker image from docker file by tagging') {
      steps {
        sh 'docker build -t phanirudra9/phani9-devops:$BUILD_NUMBER .'
      }   
    }
    stage('logging into docker hub') {
      steps {
        sh 'docker login --username="phanirudra9" --password="9eb876d4@A"'
      }   
    }
    stage('pushing docker image to the docker hub with build number') {
      steps {
        sh 'docker push phanirudra9/phani9-devops:$BUILD_NUMBER'
      }   
    }
    stage('deploying the docker image into EC2 instance and run the container') {
      steps {
        sh 'ansible-playbook deploy.yml --extra-vars="buildNumber=$BUILD_NUMBER"'
      }   
    }
    stage("Email"){
      steps{
        emailext (to: '${DEFAULT_RECIPIENTS}', replyTo: '${DEFAULT_RECIPIENTS}', subject: "Email Report from - '${env.JOB_NAME}' ", body: "Job report - '${env.BUILD_CONTENT}'", mimeType: 'text/html');
     }
   }
}
}
