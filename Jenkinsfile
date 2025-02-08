pipeline{
  agent{
    label 'master'
  }
  stages{
    stage('build app'){
      steps{
        dir('backend') {
    sh "docker build -t maro4299311/nodejs:$BUILD_NUMBER ."
}
        dir('frontend') {
    sh "docker build -t maro4299311/angular:$BUILD_NUMBER ."
}
        
      }
    }


   stage('push docker image'){
     steps{
       sh '''
        docker push maro4299311/nodejs:$BUILD_NUMBER && docker push maro4299311/angular:$BUILD_NUMBER
       '''
     }
   }

    stage('deploy'){
      steps{
       
        dir('k8s') {
          sh '''
            sed -i "s|image: .*|image: maro4299311/nodejs:$BUILD_NUMBER|g" backend.yml
            sed -i "s/image:.*/image: maro4299311/angular:$BUILD_NUMBER/g" frontend.yml
            kubectl apply -f backend.yml && kubectl apply -f frontend.yml
          '''
        }

     }
    }
  }
}
