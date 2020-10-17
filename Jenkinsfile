pipeline{
   agent any
   stages {
      stage('Build'){
         echo 'Running build automation'
         sh './gradlew build --no-daemon'
         archiveArtifats artifacts: 'dist/trainSchedule.zip'
      }
   }

   stage('Build Docker Image') {
       when {
           branch 'master'
       }
       steps {
          script {
              app = docker.build("prsomash/train-schedule")
              app.inside{
                  sh 'echo $(curl localhost:8080)'
              }
          }
       }
       stage('Push Docker Image') {
           when {
               branch 'master'
           }
           steps {
               script {
                   docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                         app.push("${env.BUILD_NUMBER}")
                         app.push("latest")
                    }
               }
           }
       }
    }
}
