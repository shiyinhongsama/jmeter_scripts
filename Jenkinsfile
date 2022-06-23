pipeline {
  agent none
  stages {
    stage('performance test') {
      agent {
        docker {
          args '-v /root/docker_data/nginx/data:/report'
          image 'zhangmin/jmeter:0.0.1'
        }

      }
      steps {
        sh '''echo "$PWD"
export DIR=`date "+%Y%m%d%H%M%S"`
DIR=$PWD/report/$DIR
mkdir -p $DIR
jmeter -n -t SimpleTestPlan.jmx -l result.jtl -e -o $DIR
ls -l $DIR'''
        stash 'reports'
      }
    }

    stage('deploy reports') {
      agent any
      steps {
        unstash 'reports'
        sh '''echo $PWD
ls -la'''
      }
    }

  }
}