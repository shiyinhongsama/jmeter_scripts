pipeline {
  agent any
  stages {
    stage('performance test') {
      agent {
        docker {
          image 'justb4/jmeter'
          args '-v  "/var/run/docker.sock:/var/run/docker.sock"'
        }

      }
      steps {
        sh '''DIR=`date "+%Y-%m-%d %H:%M:%S"`
mkdir $DIR
jmeter -n -t SimpleTestPlan.jmx -l result.jtl -e -o $DIR'''
      }
    }

  }
}