pipeline {
  agent any
  
  triggers {
    pollSCM '* * * * *'
  }
  stages {
    stage('Build') {
      steps {
        sh "docker build -t supaket/podinfo:${env.BUILD_NUMBER} ."
      }
    }
    stage('Test'){
       steps {
        sh "npm i"
        sh "npm run report-test"
      }
    }
    stage('Release') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh "docker push supaket/podinfo:${env.BUILD_NUMBER}"
          sh "docker rmi supaket/podinfo:${env.BUILD_NUMBER}"
        }
      }
    }
    stage('Deploy') {
      steps {
          withKubeConfig([credentialsId: 'kubeconfig']) {
          sh 'cat deployment.yaml | sed "s/{{BUILD_NUMBER}}/$BUILD_NUMBER/g" | kubectl apply -f -'
          sh 'kubectl apply -f service.yaml'
        }
      }
  }
}
}