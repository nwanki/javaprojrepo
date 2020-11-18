node {
    stage ('GIT CheckOut') {
        git 'https://github.com/VamsiTechTuts/javaprojrepo.git'
    }
    stage ('Build Artifact') {
        dir('demoweb') {
            def MAVEN_HOME = tool name: 'maven3', type: 'maven'
            def MAVEN_CMD = "${MAVEN_HOME}/bin/mvn"
            sh "${MAVEN_CMD} clean package"
        }
    }
    stage("Docker Build"){
        dir('demoweb') {
            sh 'docker build -t anushj200/demoweb .'
        }
    }
    stage("Docker Push") {
        withVault(configuration: [timeout: 60, vaultCredentialId: 'vault-token', vaultUrl: 'http://3.14.134.57:8200'], vaultSecrets: [[path: 'kv/dockerhub', secretValues: [[vaultKey: 'username'], [vaultKey: 'password']]]]) {
            sh 'docker login -u $username -p $password'
        }
        sh 'docker push anushj200/demoweb'
    }
}
