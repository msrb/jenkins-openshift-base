#!/usr/bin/groovy
@Library('github.com/fabric8io/fabric8-pipeline-library@master')
def name = 'jenkins-openshift-base'
def org = 'fabric8io'
dockerTemplate{
    goNode{
        git "https://github.com/${org}/${name}.git"
        if (env.BRANCH_NAME.startsWith('PR-')) {
            echo 'Running CI pipeline'
            container('go') {
                sh 'make build VERSION=2'
            }
        } else if (env.BRANCH_NAME.equals('master')) {
            echo 'Running CD pipeline'
            def newVersion = getNewVersion {}

            stage 'build'
            container('go') {
                sh 'make build VERSION=2'
            }
            
            
            stage 'push to dockerhub'
            container('docker') {
                sh "docker tag openshift/jenkins-2-centos7:latest fabric8/jenkins-openshift-base:${newVersion}"
                sh "docker push fabric8/jenkins-openshift-base:${newVersion}"
            }
            
            // pushPomPropertyChangePR {
            //     propertyName = 'jenkins-openshift.version'
            //     projects = [
            //             'fabric8io/fabric8-team-components'
            //     ]
            //     version = newVersion
            //     containerName = 's2i'
            // }
        }
    }
}
