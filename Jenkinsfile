pipeline {
  agent any
  stages {
    stage('1. Static Code Analysis') {
      steps {
        script {
          def jobUrl = env.BUILD_URL.replaceFirst(/\/\d+\/?$/, '/')
          def comment = pullRequest.comment("Jenkins job link: [${jobUrl}](${jobUrl})")
          publishChecks name: '1. Static Code Analysis',
          summary: 'Running Static Code Analysis tools...',
          text: "Open Jenkins job: [${env.BUILD_URL}](${env.BUILD_URL})"
          echo 'Running Static Code Analysis tools...'
        }
      }
    }
    stage('2. Linode provisioning stage') {
      steps {
        script {
          publishChecks name: '2. Linode provisioning stage',
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
    stage('3. App deployment stage') {
      steps {
        script {
          publishChecks name: '3. App deployment stage',
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
    stage('4. Deployment Tests') {
      steps {
        script {
          publishChecks name: '4. Deployment Tests',
          summary: 'Running Deployment Tests...'
          echo 'Running tests...'
        }
      }
    }
    stage('5. Linode deletion stage') {
      steps {
        script {
          publishChecks name: '5. Linode deletion stage',
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
    stage('6. Report stage') {
      steps {
        script {
          publishChecks name: '6. Report stage',
          summary: 'Sending report...'
          echo 'Sending report...'
        }
      }
    }
  }
}