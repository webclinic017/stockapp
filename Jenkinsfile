pipeline {
  agent any
  stages {
    stage('hello world') {
      steps {
        echo 'Hello world'
      }
    }

    stage('Credentials') {
      parallel {
        stage('Credentials') {
          steps {
            withCredentials(bindings: [[$class:  'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'bear1', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
              sh """



                                                                                             mkdir -p ~/.aws
                                                                                             echo "[default]" >~/.aws/credentials
                                                                                             echo "[default]" >~/.boto
                                                                                             echo "aws_access_key_id = ${AWS_ACCESS_KEY_ID}" >>~/.boto
                                                                                             echo "aws_secret_access_key = ${AWS_SECRET_ACCESS_KEY}" >>~/.boto
                                                                                             echo "aws_access_key_id = ${AWS_ACCESS_KEY_ID}" >>~/.aws/credentials
                                                                                             echo "aws_secret_access_key = ${AWS_SECRET_ACCESS_KEY}" >>~/.aws/credentials
                                                                                               """
            }

          }
        }

        stage('fix') {
          steps {
            sh '''@{HOMEDIRS}+=/foo/bar/
sudo apparmor_parser -r /var/lib/snapd/apparmor/profiles/*
mount options=(rw rbind) /foo/bar/ -> /tmp/snap.rootfs_*/home/,
sudo apparmor_parser -r /etc/apparmor.d/*snap-confine*'''
          }
        }

      }
    }

    stage('docker login') {
      steps {
        withCredentials(bindings: [usernamePassword(credentialsId: 'docker', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
          sh 'docker login --username $USER --password $PASS'
        }

      }
    }

    stage('push docker') {
      steps {
        sh './push_docker.sh'
      }
    }

    stage('access') {
      steps {
        sh 'kubectl config use-context minikube'
      }
    }

  }
}