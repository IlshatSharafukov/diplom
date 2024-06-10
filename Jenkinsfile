pipeline {
  environment {
    imagename = "cr.yandex/crpasim1ibnrr04p09uq/app-nginx"
  }
  
  agent any
  stages {
    stage('Building image') {
      steps{
        script {
          sh 'id'
          sh 'sudo docker build . -t $imagename:latest'
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', usernameVariable: 'dockerhubname', passwordVariable: 'dockerhubpwd')]) {
                   sh 'docker login -u ${dockerhubname} -p ${dockerhubpwd} cr.yandex'}
          sh 'docker push $imagename:latest'
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $imagename:latest"
 
      }
    }
    stage('Deploy') {
            steps {
                echo 'Deploying only because this commit is tagged...'
                sh "kubectl apply -f deploy.yaml"
            }
        }
  }
}
