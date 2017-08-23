/**
 * This pipeline will deploy to Kubernetes
 */

// JENKINS-45953 Loading libraries by tag no longer works in github-branch-source-plugin 2.2.3 :(
// @Library("github.com/fabric8io/fabric8-pipeline-library@v2.2.311") _
@Library("github.com/fabric8io/fabric8-pipeline-library@master") _

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
