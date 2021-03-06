pipeline {
  agent any
  stages {
    stage('compile') {
      steps {
        git 'https://github.com/lerndevops/samplejavaapp.git'
        sh '/usr/share/maven/bin/mvn compile'
        sleep 10
      }
    }

    stage('codereview-pmd') {
      
      steps {
        sh '/usr/share/maven/bin/mvn -P metrics pmd:pmd'
      }
    }

    stage('unit-test') {
      post {
        success {
          junit 'target/surefire-reports/*.xml'
        }

      }
      steps {
        sh '/usr/share/maven/bin/mvn test'
      }
    }

    stage('codecoverage') {
      post {
        success {
          cobertura(autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'target/site/cobertura/coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false)
        }

      }
      steps {
        sh '/usr/share/maven/bin/mvn cobertura:cobertura -Dcobertura.report.format=xml'
      }
    }

    stage('package') {
      steps {
        sh '/usr/share/naven/bin/mvn clean package'
      }
    }

  }
}
