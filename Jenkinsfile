pipeline {
  agent any
  stages {
    stage('deploy start') {
      steps {
        slackSend(message: "Deploy ${env.BUILD_NUMBER} Started"
        , color: 'good', tokenCredentialId: 'slack-key')
      }
    }
    stage('git scm update') {
      steps {
        git url: 'https://github.com/JaE-master/GitOps.git', branch: 'main'
      }
    }
    stage('deploy kubernetes') {
      steps {
//      kubernetesDeploy(kubeconfigId: 'kubeconfig', configs: '*.yaml')
        script {
          try {
            sh '''
            kubectl delete -f qg-flask.yaml
            '''
          } catch (err) {
            echo err.getMessage()
          }
        }
        sh '''
        kubectl create -f qg-flask.yaml
        '''        
      }
    }
    stage('deploy end') {
      steps {
        slackSend(message: """${env.JOB_NAME} #${env.BUILD_NUMBER} End
        """, color: 'good', tokenCredentialId: 'slack-key')
      }
    }
  }
}
