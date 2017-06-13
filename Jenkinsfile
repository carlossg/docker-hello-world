/**
 * This pipeline will deploy to Kubernetes
 */

library identifier: 'fabric8-pipeline-library@v2.2.311', retriever: modernSCM(
  github(repoOwner: 'fabric8io', repository: 'fabric8-pipeline-library')
)

podTemplate(label: 'deploy') {

  stage('deployment') {
    node('deploy') {
      checkout scm
      //git 'https://github.com/jenkinsci/kubernetes-plugin.git'
      kubernetesApply(file: readFile('kubernetes-hello-world-service.yaml'), environment: 'kubernetes-plugin')
      kubernetesApply(file: readFile('kubernetes-hello-world-v1.yaml'), environment: 'kubernetes-plugin')
    }
  }

  stage('upgrade') {
    timeout(time:1, unit:'DAYS') {
      input message:'Approve upgrade?'
    }
    node('deploy') {
      checkout scm
      //git 'https://github.com/jenkinsci/kubernetes-plugin.git'
      kubernetesApply(file: readFile('kubernetes-hello-world-v2.yaml'), environment: 'kubernetes-plugin')
    }
  }
}
