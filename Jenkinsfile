pipeline{
  agent any
  stages{
    stage('build docker image'){
      steps{
        dir('backend') {
    sh "docker build -t maro4299311/nodejs:$BUILD_NUMBER ."
}
       dir('frontend') {
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
      doker push maro4299311/angular:$BUILD_NUMBER
    '''
}
      }
    }

    stage('deploy'){
      steps{
      sh "sed -i 's|image:.*|image: maro4299311/angular:$BUILD_NUMBER|g' k8s/backend.yml"
      sh "sed -i 's|image:.*|image: maro4299311/angular:$BUILD_NUMBER|g' k8s/frontend.yml"

      sh "kubectl apply -f k8s/backend.yml && kubectl apply -f k8s/frontend.yml"
    }
  }
}
