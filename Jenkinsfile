node{
    def mvn = tool 'maven'
    stage('GIT SCM'){
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'LV_git', url: 'https://github.com/virdilokeshna/game-of-life.git']]])
    }
    stage('cleanInstall'){
        withMaven(maven: 'maven') {
            echo '***************INITIALISING*************'
            sh 'mvn -f pom.xml clean compile'
        }
    }
    stage('run sonarscan'){
            withMaven(maven: 'maven'){
            
                echo '***************INITIALISING*************'
                sh 'mvn -f pom.xml sonar:sonar'
                }
        }
   
}