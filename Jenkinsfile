pipeline {
  agent {
    kubernetes {
      yamlFile 'build-agent.yaml'
      defaultContainer 'maven'
      idleMinutes 1
    }
  }
  stages {
    stage('Build') {
      parallel {
        stage('Compile') {
          steps {
            container('maven') {
              sh 'mvn compile'
            }
          }
        }
      }
    }
    stage('Package') {
      parallel {
        stage('Create Jarfile') {
          steps {
            container('maven') {
              sh 'mvn package -DskipTests'
            }
           }
          } // stage
        stage('Docker BnP') {
            steps {
              container('kaniko') {
                sh '/kaniko/executor -f `pwd`/Dockerfile -c `pwd` --force --insecure --skip-tls-verify --cache=true --destination=docker.io/zkube/dso-demo'
            } 
          }
         } // stage
        } // parallel
       } // stage
    stage('Deploy to Dev') {
      steps {
        // TODO
        sh "echo done"
      }
     } // stage
  } // stages
} // pipeline
