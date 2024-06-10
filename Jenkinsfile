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
          sh 'sudo docker build . -t $imagename:0.$BUILD_NUMBER'
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          withCredentials([usernamePassword(usernameVariable: 'dockerhubname', passwordVariable: 'dockerhubpwd')]) {
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
