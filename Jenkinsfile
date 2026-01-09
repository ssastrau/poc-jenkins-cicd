pipeline {
  agent any
  when {
    expression { return env.CHANGE_ID }
  }
  stages {
    stage('1. Static Code Analysis') {
      steps {
        script {
          def jobUrl = env.BUILD_URL.replaceFirst(/\/\d+\/?$/, '/')
          pullRequest.getComments().each { c ->
            if (c.body?.contains(jobUrl)) {
                c.delete()
            }
          }
          pullRequest.comment("Jenkins job link: [${jobUrl}](${jobUrl})")

          try {
            publishChecks(
              name: '1. Static Code Analysis',
              summary: 'Static code analysis is queued to run.',
              status: 'QUEUED'
            )
            publishChecks(
              name: '1. Static Code Analysis',
              summary: 'Static code analysis is running...',
              status: 'IN_PROGRESS'
            )
            echo 'Running Static Code Analysis tools...'
            publishChecks(
              name: '1. Static Code Analysis',
              summary: 'Static code analysis completed successfully!',
              status: 'COMPLETED',
              conclusion: 'SUCCESS'
            )
          } catch (e) {
            publishChecks(
              name: '1. Static Code Analysis',
              summary: "Static code analysis failed: ${e}",
              status: 'COMPLETED',
              conclusion: 'FAILURE'
            )
            throw e
          }
        }
      }
    }
    stage('2. Linode provisioning stage') {
      steps {
        script {
          try {
            publishChecks(
              name: '2. Linode provisioning stage',
              summary: 'Provisioning Linode (queued)',
              status: 'QUEUED'
            )
            publishChecks(
              name: '2. Linode provisioning stage',
              summary: 'Provisioning Linode (in progress)',
              status: 'IN_PROGRESS'
            )
            def userInput = input(
              message: 'Provision your Linode',
              parameters: [
                [
                  $class: 'ChoiceParameterDefinition',
                  choices: ['se-sto', 'us-ord', 'jp-osa'].join('\n'),
                  name: 'region',
                  description: 'Select Linode region'
                ],
                [
                  $class: 'ChoiceParameterDefinition',
                  choices: [
                    'g6-nanode-1',
                    'g6-standard-6',
                    'g6-dedicated-8',
                    'g7-highmem-1',
                    'g2-gpu-rtx4000a1-s'
                  ].join('\n'),
                  name: 'plan',
                  description: 'Select Linode Plan'
                ]
              ]
            )
            echo "Provisioning Linode: ${userInput.plan} in region: ${userInput.region}"
            publishChecks(
              name: '2. Linode provisioning stage',
              summary: 'Linode provisioned successfully!',
              status: 'COMPLETED',
              conclusion: 'SUCCESS'
            )
          } catch (e) {
            publishChecks(
              name: '2. Linode provisioning stage',
              summary: "Provisioning failed: ${e}",
              status: 'COMPLETED',
              conclusion: 'FAILURE'
            )
            throw e
          }
        }
      }
    }
    stage('3. App deployment stage') {
      steps {
        script {
          try {
            publishChecks(
              name: '3. App deployment stage',
              summary: 'App deployment queued.',
              status: 'QUEUED'
            )
            publishChecks(
              name: '3. App deployment stage',
              summary: 'App deployment in progress...',
              status: 'IN_PROGRESS'
            )
            def deployment = input(
              message: 'Select the app to deploy',
              parameters: [
                [
                  $class: 'ChoiceParameterDefinition',
                  choices: ['jenkins', 'docker', 'MySQL'].join('\n'),
                  name: 'app'
                ]
              ]
            )
            echo "Deploying the app: ${deployment}"
            publishChecks(
              name: '3. App deployment stage',
              summary: 'App deployed successfully!',
              status: 'COMPLETED',
              conclusion: 'SUCCESS'
            )
          } catch (e) {
            publishChecks(
              name: '3. App deployment stage',
              summary: "App deployment failed: ${e}",
              status: 'COMPLETED',
              conclusion: 'FAILURE'
            )
            throw e
          }
        }
      }
    }
    stage('4. Application Tests') {
      steps {
        script {
          try {
            publishChecks(
              name: '4. Application Tests',
              summary: 'Application tests queued.',
              status: 'QUEUED'
            )
            publishChecks(
              name: '4. Application Tests',
              summary: 'Application tests running...',
              status: 'IN_PROGRESS'
            )
            echo 'Running tests...'
            publishChecks(
              name: '4. Application Tests',
              summary: 'Application tests completed successfully!',
              status: 'COMPLETED',
              conclusion: 'SUCCESS'
            )
          } catch (e) {
            publishChecks(
              name: '4. Application Tests',
              summary: "Tests failed: ${e}",
              status: 'COMPLETED',
              conclusion: 'FAILURE'
            )
            throw e
          }
        }
      }
    }
    stage('5. Linode deletion stage') {
      steps {
        script {
          try {
            publishChecks(
              name: '5. Linode deletion stage',
              summary: 'Linode deletion queued.',
              status: 'QUEUED'
            )
            publishChecks(
              name: '5. Linode deletion stage',
              summary: 'Linode deletion in progress...',
              status: 'IN_PROGRESS'
            )
            def confirm = input(
              message: 'Do you want to delete the Linode?',
              parameters: [
                [
                  $class: 'ChoiceParameterDefinition',
                  choices: ['Yes', 'No'].join('\n'),
                  name: 'confirm_delete'
                ]
              ]
            )
            if (confirm == 'Yes') {
              echo 'Deleting the Linode...'
            } else {
              echo 'Deletion cancelled by user.'
            }
            publishChecks(
              name: '5. Linode deletion stage',
              summary: 'Linode deletion stage completed!',
              status: 'COMPLETED',
              conclusion: 'SUCCESS'
            )
          } catch (e) {
            publishChecks(
              name: '5. Linode deletion stage',
              summary: "Linode deletion failed: ${e}",
              status: 'COMPLETED',
              conclusion: 'FAILURE'
            )
            throw e
          }
        }
      }
    }
    stage('6. Report stage') {
      steps {
        script {
          try {
            publishChecks(
              name: '6. Report stage',
              summary: 'Report queued.',
              status: 'QUEUED'
            )
            publishChecks(
              name: '6. Report stage',
              summary: 'Sending report...',
              status: 'IN_PROGRESS'
            )
            echo 'Sending report...'
            publishChecks(
              name: '6. Report stage',
              summary: 'Report sent successfully!',
              status: 'COMPLETED',
              conclusion: 'SUCCESS'
            )
          } catch (e) {
            publishChecks(
              name: '6. Report stage',
              summary: "Report sending failed: ${e}",
              status: 'COMPLETED',
              conclusion: 'FAILURE'
            )
            throw e
          }
        }
      }
    }
  }
}