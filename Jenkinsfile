node {
    stage ('GIT CheckOut') {
        git 'https://github.com/nwanki/javaprojrepo.git'
    }
    stage ('Build Artifact') {
        dir('demoweb') {
            def MAVEN_HOME = tool name: 'maven-3', type: 'maven'
            def MAVEN_CMD = "${MAVEN_HOME}/bin/mvn"
            sh "${MAVEN_CMD} clean package"
        }
    }
    stage("Docker Build"){
        dir('demoweb') {
            sh 'docker build -t wankineba/demoweb .'
        }
    }
    stage("Docker Push") {
        withVault(configuration: [timeout: 60, vaultCredentialId: 'vault-token', vaultUrl: 'http://18.217.63.49:8200'], vaultSecrets: [[path: 'kv/dockerhub', secretValues: [[vaultKey: 'username'], [vaultKey: 'password']]]]) {
            sh 'docker login -u $username -p $password'
        }
        sh 'docker push wankineba/demoweb'
    }
}
