pipeline {
  agent any
  environment {
    dockerhub_username = 'lauresleven'
    img_name = 'spring-hello'
    img_tag = sh (returnStdout: true, script: 'git log -1 --pretty=%h').trim()
    username = 'user8'
  }
  stages {
    stage('Build') {
      steps {
        sh """
          docker build . -t ${dockerhub_username}/${img_name}:${img_tag}
        """
      }
    }
    stage('Push') {
      steps {
        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'user8', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
          sh """
            echo ${WORKSPACE}
            docker login -u ${USERNAME} -p ${PASSWORD}
            docker push ${dockerhub_username}/${img_name}:${img_tag}
            docker logout
          """
        }
      }
    }
//    stage('Deploy') {
//      steps {
//        sh """
//          helm upgrade --wait --install -f ${username}-backend.yaml --set image.tag=${img_tag} --namespace ${username} --tiller-namespace ${username} ${username}-employee-management-backend ../kubemania/employee-management-backend/
//        """
      }
    }
  }
}
