node {
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')), disableConcurrentBuilds(), [$class: 'GithubProjectProperty', displayName: '', projectUrlStr: 'https://github.com/zubairRiyaz/PetClinic.git/'], pipelineTriggers([githubPush()])])
    
    def MvnHome = tool name: 'MAVEN', type: 'maven'
    def MvnCli = "${MvnHome}/bin/mvn"

    stage('Checkout Git'){
        sh 'echo "Welcome to jenkins"'
        git changelog: false, credentialsId: 'github_creds', poll: false, url: 'https://github.com/zubairRiyaz/PetClinic.git'
    }
    
    stage('Read Maven POM'){
        readpom = readMavenPom file: '';
        def art_version = readpom.version;
        echo "${art_version}"
    }
    
    stage('Maven test'){
        sh "${MvnCli} test"
    }
    
    stage('Maven Package'){
        sh "${MvnCli} package -Dmaven.skip.test=true"
    }
    
    stage('Archive Artifacts'){
        archiveArtifacts artifacts: '**/spring-petclinic-*.war', onlyIfSuccessful: true
    }
    
    stage('Archive Junit'){
        junit '**/surefire-reports/*.xml'
    }
    
    
    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "java.yaml", kubeconfigId: "mykubeconfigfile")
        }
      }
    }
    
}
