
pipeline {

  agent any
  parameters{
    string(defaultValue: 'DEV_STAGING_K8S_CONFIG',
      description : '',
      name        : 'DEV_STAGING_K8S_CONFIG')
  }
  options {
    buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '1', numToKeepStr: '1')
    disableConcurrentBuilds()
    timestamps()

  }

  environment{
    GIT_COMMIT_SHORT = sh(
                script: "printf \$(git rev-parse --short ${GIT_COMMIT})",
                returnStdout: true
    )
  }

  stages {
    stage('Prepare') {
      steps {
        script {
           k8s_config = "/var/jenkins_home/dev.config"
           k8s_file = "1"
        }
      }
    }
    stage('Hello') {

      steps {

        sh '''
        if [ 1 -eq 1 ]; then
          echo "hello"
          tee ${k8s_config}.base64 <<-EOF > /dev/null
          "${params.DEV_STAGING_K8S_CONFIG}"
EOF
          echo "hi!"
        fi
        '''

      }

    }
    stage('cat README: POST') {

      when {

        branch "fix-*"

      }

      steps {

        sh '''

          cat README.md

        '''

      }

    }
  }
  post { 
      success { 
          echo "success"
      }
      failure { 
          echo "failure"
      }
      aborted { 
          echo "aborted"
      }
  }

}
