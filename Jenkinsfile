properties([ 
        [$class: 'JiraProjectProperty'],
        parameters([
                [$class      : 'GlobalVariableStringParameterDefinition',
                 defaultValue: '${DEV_STAGING_K8S_CONFIG}',
                 description : '',
                 name        : 'DEV_STAGING_K8S_CONFIG'],
                [$class      : 'GlobalVariableStringParameterDefinition',
                 defaultValue: '${PRODUCTION_K8S_CONFIG_2}',
                 description : '',
                 name        : 'PRODUCTION_K8S_CONFIG_2'],
                [$class: 'GlobalVariableStringParameterDefinition',
                 defaultValue: '${TEAMS_BUILD_NOTIFICATION_CHANNEL}',
                 description: '',
                 name: 'TEAMS_BUILD_NOTIFICATION_CHANNEL'],
                [$class: 'GlobalVariableStringParameterDefinition',
                  defaultValue: '${DOCKER_HUB_TOKEN}',
                  description: '',
                  name: 'DOCKER_HUB_TOKEN'],
                [$class: 'GlobalVariableStringParameterDefinition',
                  defaultValue: '${DOCKER_HUB_USERNAME}',
                  description: '',
                  name: 'DOCKER_HUB_USERNAME'],
        ])
])
pipeline {

  agent any

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
        }
      }
    }
    stage('Hello') {

      steps {

        sh '''

          java -version
          if[true];then
          tee ${k8s_config}.base64 <<-EOF > /dev/null
${DEV_STAGING_K8S_CONFIG}
EOF
          rm -rf ${k8s_config}
          cat ${k8s_config}.base64 | base64 -d > ${k8s_config}
          elif [ \"${env.BRANCH_NAME}\" = \"production\" ]; then 
          echo \"Deploying ${env.BRANCH_NAME} to namespace ${k8s_namespace} on kubernetes at ${k8s_server_host}\"
          fi
        '''

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
