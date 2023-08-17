podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: go
        image: openjdk:11-jre-slim
        command:
        - sleep
        args:
        - 99d
      restartPolicy: Never
''') {
  node(POD_LABEL) {
    stage('Get the project') {
      git url: 'https://github.com/Hardcorelevelingwarrior/chap3', branch: 'main'
    }
    stage("Perform SAST with Sonarqube") {
      def scannerHome = tool name: 'sonarqube', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
      def jdkHome = tool name: 'Java 17', type: 'hudson.model.JDK'
      withSonarQubeEnv('sonarqube') {
        withEnv(["JAVA_HOME=${jdkHome}", "PATH+JDK=${jdkHome}/bin"]) {
          sh "${scannerHome}/bin/sonar-scanner"
        }
      }
    }
    stage("Dockerizing the app"){
        script {
                    def dockerImage = docker.build("my-image:${env.BUILD_ID}", ".")
    }
  }
}
}
