import groovy.json.JsonSlurper;
 
node{
    
    slackSend (color: '#FFFF00', message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
    stage 'Build, Test and Package'
    git url: "https://github.com/capablaza/springboot-jenkins-demo.git"
    def commitid = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
    def workspacePath = pwd()
    sh "echo ${commitid} > ${workspacePath}/expectedCommitid.txt"
    sh "mvn clean package -Dcommitid=${commitid}"
}
 
node{
    stage 'Stop, Deploy and Start'

    sh 'curl -X POST http://localhost:9090/shutdown || true'

    sh 'echo "mvn spring-boot:run" | at now + 1 minutes'
}