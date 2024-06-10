pipeline {
  environment {
    imagename = "khayrullinii/app-nginx"
  }
  
  agent any
  stages {
    stage('Building image') {
      steps{
        script {
          sh 'docker build . -t $imagename:0.$BUILD_NUMBER'
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd',usernameVariable: 'dockerhubname', passwordVariable: 'dockerhubpwd')]) {
                   sh 'docker login -u ${dockerhubname} -p ${dockerhubpwd} cr.yandex'}
          sh 'docker push $imagename:0.$BUILD_NUMBER'
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $imagename:0.$BUILD_NUMBER"
 
      }
    }
    stage('Deploy') {
            when { tag "release-*" }
            steps {
                echo 'Deploying only because this commit is tagged...'
                sh "kubectl apply -f deploy.yaml"
            }
        }
  }
}
