pipeline {
  agent any
  stages {
    stage('Comment on PR') {
      steps {
        script {
        def comment = pullRequest.comment("Open Jenkins job: [${env.BUILD_URL}](${env.BUILD_URL})")}
      }
    }
    stage('Static Code Analysis') {
      steps {
        script {
          publishChecks name: 'Static Code Analysis',
          summary: 'Running Static Code Analysis tools...',
          text: "Open Jenkins job: [${env.BUILD_URL}](${env.BUILD_URL})"
          echo 'Running Static Code Analysis tools...'
        }
      }
    }
    stage('Linode provisioning stage') {
      steps {
        script {
          publishChecks name: 'Linode provisioning stage',
          summary: 'Provisioning Linode'
          def userInput = input(
            message: 'Provision your Linode',
            parameters: [
              [$class: 'ChoiceParameterDefinition', choices: ['nano', 'micro', 'mini'].join('\n'), name: 'size', description: 'Select Linode size'],
              [$class: 'ChoiceParameterDefinition', choices: ['se-sto', 'us-ord', 'jp-osa'].join('\n'), name: 'region', description: 'Select Linode region']
            ]
          )
          echo "Provisioning Linode of size: ${userInput['size']} in region: ${userInput['region']}"
        }
      }
    }
    stage('App deployment stage') {
      steps {
        script {
          publishChecks name: 'App deployment stage',
          summary: 'Deploying the app'
          def deployment = input(
            message: 'Select App deployment method:',
            parameters: [
              [$class: 'ChoiceParameterDefinition', choices: ['jenkins', 'docker', 'MySQL'].join('\n'), name: 'deployment_method']
            ]
          )
          echo "Deploying the app with: ${deployment}"
        }
      }
    }
    stage('Deployment Tests') {
      steps {
        script {
          publishChecks name: 'Deployment Tests',
          summary: 'Running Deployment Tests...'
          echo 'Running tests...'
        }
      }
    }
    stage('Linode deletion stage') {
      steps {
        script {
          publishChecks name: 'Linode deletion stage',
          summary: 'Deleting the Linode'
          def confirm = input(
            message: 'Do you want to delete the Linode?',
            parameters: [
              [$class: 'ChoiceParameterDefinition', choices: ['Yes', 'No'].join('\n'), name: 'confirm_delete']
            ]
          )
          if (confirm == 'Yes') {
            echo 'Deleting the Linode...'
          } else {
            echo 'Deletion cancelled by user.'
          }
        }
      }
    }
    stage('Report stage') {
      steps {
        script {
          publishChecks name: 'Report stage',
          summary: 'Sending report...'
          echo 'Sending report...'
        }
      }
    }
  }
}