#!/usr/bin/env groovy

node {
    stage('checkout') {
        checkout scm
    }

    stage('check java') {
        sh "java -version"
    }

    stage('clean') {
        sh "chmod +x gradlew"
        sh "./gradlew clean --no-daemon"
    }


    stage('backend tests') {
        try {
            sh "./gradlew test -PnodeInstall --no-daemon"
        } catch(err) {
            throw err
        } finally {
            junit '**/build/**/TEST-*.xml'
        }
    }

    stage('packaging') {
        sh "./gradlew bootRepackage -x test -Pprod -PnodeInstall --no-daemon"
        archiveArtifacts artifacts: '**/build/libs/*.war', fingerprint: true
    }
    
//    stage('deployment') {
//        sh "./gradlew deployHeroku --no-daemon"
//    }

    // ToDo Odkomentować przy dodaniu Sonara
//    stage('quality analysis') {
//        withSonarQubeEnv('Sonar') {
//            sh "./gradlew sonarqube --no-daemon"
//        }
//    }
}
