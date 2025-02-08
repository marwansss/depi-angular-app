  pipeline {
  agent any
  stages {
    stage('build docker image') {
      steps {
        dir('backend') {
          sh "docker build -t maro4299311/nodejs:$BUILD_NUMBER ."
        }
        dir('front-end') {
          sh "docker build -t maro4299311/angular:$BUILD_NUMBER ."
        }
      }
    }

    stage('push docker images'){
      steps{
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
    sh "docker login -u $USER -p $PASS"
    sh '''
      docker push maro4299311/nodejs:$BUILD_NUMBER
      docker push maro4299311/angular:$BUILD_NUMBER
    '''
}
      }
    }

    stage('deploy') {
      steps {
        sh "sed -i 's|image:.*|image: maro4299311/nodejs:$BUILD_NUMBER|g' k8s/backend.yml"
        sh "sed -i 's|image:.*|image: maro4299311/angular:$BUILD_NUMBER|g' k8s/frontend.yml"

        withCredentials([file(credentialsId: 'k8s', variable: 'k8s')]) {
    sh "kubectl --kubeconfig=$k8s apply -f k8s/backend.yml && kubectl --kubeconfig=$k8s apply -f k8s/frontend.yml"
}
        
      }
    }
  }
}
