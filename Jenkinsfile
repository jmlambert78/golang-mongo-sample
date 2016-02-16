#!/usr/bin/groovy
def envStage = "${env.JOB_NAME}-staging"

node ('kubernetes'){

  git 'https://github.com/jmlambert78/golang-mongo-sample'

  stage 'canary release'
    if (!fileExists ('Dockerfile')) {
      writeFile file: 'Dockerfile', text: 'FROM golang:onbuild'
    }

    def newVersion = performCanaryRelease {}

    def rc = getKubernetesJson {
      port = 8080
      label = 'golang'
      icon = 'https://cdn.rawgit.com/fabric8io/fabric8/dc05040/website/src/images/logos/gopher.png'
      version = newVersion
      imageName = clusterImageName
    }

  stage 'Rolling upgrade Staging'
    kubernetesApply(file: rc, environment: envStage)

}
