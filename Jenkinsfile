node('maven') {
    def mvnHome = tool name: 'M3', type: 'maven'
    env.PATH = "${mvnHome}/bin:${env.PATH}"

    stage('Checkout') {
        checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'CheckoutOption'], [$class: 'LocalBranch', localBranch: 'master']], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'c5ae61df-e297-4c82-af51-133feb1f6ca4', url: 'git@github.com:dempj/spring-boot-openshift-hello-world.git']]]
    }

    stage('Build'){
        sh 'mvn -f pom.xml clean install -DskipTests'
    }

    stage('Test'){
        sh 'mvn -f pom.xml test'
    }

    stage('Package'){
        sh 'mvn -f pom.xml deploy'
    }

    stage('Apply definitions'){
        sh 'oc apply -f definitions/deployment/'
    }

    stage('Deploy'){
      sh "mkdir -p target/deploy"
      sh "cp Dockerfile target/deploy"
      sh "cp target/ROOT.war target/deploy"
      sh "oc start-build goop-wildfly-build  --from-dir  target/deploy"
    }

}
