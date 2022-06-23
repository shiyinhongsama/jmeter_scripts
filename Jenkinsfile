pipeline {
  agent none
  stages {
    stage('performance test') {
      agent {
        docker {
          image 'zm/jmeter:0.0.1'
        }

      }
      steps {
        sh '''echo "$PWD"
DIR=`date "+%Y%m%d%H%M%S"`
mkdir $DIR
jmeter -n -t SimpleTestPlan.jmx -l result.jtl -e -o $DIR
ls -l $DIR'''
      }
    }

  }
}