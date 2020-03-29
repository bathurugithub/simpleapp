node{

    stage('SCM Checkout'){
        git url: 'https://github.com/bathurugithub/panda.git',branch: 'master'
    }

    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
    }

    stage('Build Docker Image'){
        sh 'docker build -t bathurudocker/simpleapp:${buildNumber} .'
    }

    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'dockerHubPwd', variable: 'dockerpwd')]) {
          sh "docker login -u bathurudocker -p ${dockerpwd}"
        }
        sh 'docker push bathurudocker/simpleapp:${buildNumber}'
     }

      stage('Run Docker Image In Dev Server'){
        sh  'docker run  -d -p 8010:8080 --name simpleapp bathurudocker/simpleapp:${buildNumber}'
       }
}
