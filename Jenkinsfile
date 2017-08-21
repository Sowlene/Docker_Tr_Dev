pipeline {
  agent any
  stages {
    stage('Welcome') {
      steps {
        parallel(
          "First": {
            readFile 'Dockerfile'
            sh 'cat Dockerfile'
            
          },
          "Situation": {
            sh 'pwd && who && ls -Shal #l not pass'
            
          }
        )
      }
    }
    stage('Build') {
      steps {
        parallel(
          "Build": {
            echo 'We are trying to build'
            
          },
          "ImageBuild": {
            sh '''ls -lhS /var/run/ | grep docker.sock
'''
            sh 'docker build -t solene/installtv2 .'
            
          }
        )
      }
    }
    stage('ContainersStarting') {
      steps {
        echo 'Container are going to start'
      }
    }
    stage('Test') {
      steps {
        parallel(
          "TestWithPostgresql": {
            sh 'docker run -e DATABASE_TYPE=postgresql -e DATABASE_USER=tracimuser -e DATABASE_PASSWORD=tracimpassword -e DATABASE_HOST=192.168.1.73 -e DATABASE_NAME=tracimdb solene/installtv2'
            
          },
          "TestWithsqlite": {
            sh 'docker run -e DATABASE_TYPE=sqlite -e DATABASE_USER=tracimuser -e DATABASE_PASSWORD=tracimpassword -e DATABASE_HOST=192.168.1.73 -e DATABASE_NAME=tracimdb solene/installtv2'
            
          },
          "TestWithMysql": {
            sh 'docker run -e DATABASE_TYPE=mysql -e DATABASE_USER=tracimuser -e DATABASE_PASSWORD=tracimpassword -e DATABASE_HOST=192.168.1.73 -e DATABASE_NAME=tracimdb solene/installtv2'
            
          }
        )
      }
    }
    stage('End') {
      steps {
        echo 'I will check containers'
        sh 'docker ps -n 3'
      }
    }
  }
}