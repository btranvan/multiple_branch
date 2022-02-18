
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

        sh """
          java -version
          eFile="1"
          if true;
          then
          tee ${k8s_config}.base64 <<-EOF > /dev/null
  ${params.DEV_STAGING_K8S_CONFIG}
  EOF
          rm -rf ${k8s_config}
          cat ${k8s_config}.base64 | base64 -d > ${k8s_config} EOF
          else echo \"Unsupported branch: ${env.BRANCH_NAME}\" 
          EOF
          fi
        """

      }

    }
    stage('cat README') {

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

}
