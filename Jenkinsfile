node {
    gitProjectURL = 'https://github.com/ozanbozkurtt/simple-spring-boot.git'
    containerName = 'simple-java'
    latestTag = 'latest'
    branchName = 'main'
    dockerProjectURL = 'https://github.com/ozanbozkurtt/dockerfiles-demo.git'
    dockerBranchName = 'main'
    dockerProjectName = 'Dockerfile'
    sonarProjectKey = 'simple-java'
    sonarLoginToken = 'sqp_18edf432e534ae4652cf09a17c6bbca952ae901d'
    sonarHostUrl = 'http://192.168.1.2:9000'
    stage('Get Dockerfile repository') {
        fileOperations([folderCreateOperation('mytmp')])
        dir('mytmp') {
            checkout([$class: 'GitSCM', branches: [[name: "${dockerBranchName}"]], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[ url: "${dockerProjectURL}"]]])
        }
    stage('Clone repository') {
        checkout([$class: 'GitSCM', branches: [[name: "${branchName}"]], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[ url: "${gitProjectURL}"]]])        
    }        
        sh("cp mytmp/${dockerProjectName} .")
        
        fileOperations([folderDeleteOperation('mytmp')])
    }
    stage('Build & register') {
         def dockerHome = tool 'docker'
         env.PATH = "${dockerHome}/bin:${env.PATH}"
            
        
            docker.withRegistry('http://192.168.1.2:5000') {
                def customImage = docker.build("${containerName}")
                customImage.push("${latestTag}")
            
        }
    }
}
