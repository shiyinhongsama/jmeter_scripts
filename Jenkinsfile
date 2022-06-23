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
DIR=`date "+%Y%m%d%H%M%S"`
mkdir $DIR
jmeter -n -t SimpleTestPlan.jmx -l result.jtl -e -o $DIR
ls -l $DIR
cp -r $DIR /report/'''
        stash(name: 'reports', includes: '/report/**')
      }
    }

    stage('deploy reports') {
      agent any
      steps {
        unstash 'reports'
        sh 'cp -r /report/* /root/docker_data/nginx/data/'
      }
    }

  }
}