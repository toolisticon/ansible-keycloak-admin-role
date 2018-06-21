properties properties: [
        [$class: 'BuildDiscarderProperty', strategy: [$class: 'LogRotator', artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '30', numToKeepStr: '10']],
        disableConcurrentBuilds()
]

@Library('holisticon-build-library')
def build = new de.holisticon.ci.jenkins.Build()
def notify = new de.holisticon.ci.jenkins.Notify()

node {
    def buildNumber = env.BUILD_NUMBER
    def branchName = env.BRANCH_NAME
    def workspace = env.WORKSPACE
    def buildUrl = env.BUILD_URL
    def image

    // PRINT ENVIRONMENT TO JOB
    echo "workspace directory is $workspace"
    echo "build URL is $buildUrl"
    echo "build Number is $buildNumber"
    echo "branch name is $branchName"
    echo "PATH is $env.PATH"

    try {
        stage('Checkout') {
            deleteDir()
            checkout scm
        }

        stage('Build') {
            sh "" // TODO
        }

        stage('Test') {
            sh "" // TODO
        }

        stage('Integration-Test') {
            sh returnStatus: true, script: "docker-compose build"
            sh "docker-compose build"
            sh "docker-compose up -d"
            sh "ansible-playbook test_end2end.yml"
            sh "docker-compose down"
        }

    } catch (e) {
        notify.buildMessage(currentBuild, true, 'holi-oss', 'Error with recent changes: ' + build.summarizeBuild(currentBuild))
        throw e
    }

}
