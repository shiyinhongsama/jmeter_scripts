pipeline {
  agent none
  parameters {
          string(name: 'SCRIPT_NAME', defaultValue: 'SimpleTestPlan.jmx', description: '测试脚本名称')
          booleanParam(name: 'SEND_MAIL', defaultValue: false, description: '性能测试结果是否发送邮件')
     }
  options{
    skipDefaultCheckout true
  }
  triggers {
     //秒 分 时 日 月 星期 年
     cron('0 0 0 ? * *')
    }
  stages {
    stage('clone code'){
      agent any
        steps{
            git credentialsId: "33bde891-d360-457d-b165-91dbb45ad22e", url: "https://gitee.com/shiyinhong/jmeter_scripts.git"
        }
    }
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
rm result.jtl
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
ls -la
cp -r report/* /root/docker_data/nginx/data'''
      }
    }

  }
}
