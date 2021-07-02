pipeline {
  agent any 
  tools {
    maven 'Maven'
  }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }
    
    stage ('Check-Git-Secrets') {
     steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/HemantTomar/webapp.git > trufflehog'
        sh 'cat trufflehog'
      }
   }
    
   stage ('Source Composition Analysis') {
      steps {
        sh 'rm owasp* || true'
        sh 'wget "https://raw.githubusercontent.com/HemantTomar/webapp/master/owasp-dependency-check.sh" '
        sh 'chmod +x owasp-dependency-check.sh'
        sh 'bash owasp-dependency-check.sh'
        sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
        
     }
    }
    
    //stage ('SAST') {
    //  steps {
     //   withSonarQubeEnv('sonar') {
       //   sh 'mvn sonar:sonar'
        //  sh 'cat target/sonar/report-task.txt'
       // }
      //}
  //  }
    
    stage ('Build') {
      steps {
      sh 'mvn clean package'
       }
    }
    
    stage('Deploy to Tomcat'){
      steps {
        deploy adapters: [tomcat9(credentialsId: 'fae0c4b2-1012-4b06-be38-119b54a9e4c9', path: '', url: 'http://18.207.98.119:8080/')], contextPath: null, onFailure: false, war: '**/*.war'
    //  sshagent(['tomcat']) {
         //sh """
         //  scp  target/*.war ubuntu@18.207.98.119:/home/ubuntu
        //   ssh  ubuntu@18.207.98.119 'cp -r /home/ubuntu/*.war /home/ubuntu/prod/apache-tomcat-10.0.7/webapps'
      //   """
      //}
   }
    }
   // stage ('DAST') {
   //   steps {
    //    sshagent(['zap']) {
      //   sh 'ssh -o  StrictHostKeyChecking=no ubuntu@3.235.105.127 "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://18.207.98.119:8080/webapp/" || true'
     //   }
     // }
   // }
    
  }
}
