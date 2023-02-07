pipeline {

  agent {
    label 'centos'
  }
  options {
    timeout(time: 12, unit: 'MINUTES')
    ansiColor('xterm')
  }

  stages {
    stage('Build') {
      steps {
        echo "Building....echo"
        sh 'echo "Building....sh"'
      }
    }
    stage('CloudSploit') {
      steps {
        echo 'Cloudsploit...'
        withCredentials([
          string(credentialsId: 'AQUA_KEY', variable: 'AQUA_KEY'),
          string(credentialsId: 'AQUA_SECRET', variable: 'AQUA_SECRET'),
          string(credentialsId: 'GITHUB_TOKEN', variable: 'GITHUB_TOKEN')
        ]) {
          sh '''
          export TRIVY_RUN_AS_PLUGIN=aqua
          export trivyVersion=0.34.0
          export AQUA_URL=https://api.asia-2.supply-chain.cloud.aquasec.com
          export CSPM_URL=https://asia-2.api.cloudsploit.com
          curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh -s -- -b . v${trivyVersion}
          ./trivy fs --security-checks config,vuln,secret .
          # Customizing which severities are scanned for is done by adding the following flag: --severity UNKNOWN,LOW,MEDIUM,HIGH,CRITICAL
          '''
        }
      }
    }
    stage('Test') {
      steps {
        echo 'Testing...'
        sh 'sudo docker-compose up --build --exit-code-from pytest'
      }
    }
    stage('Deploy') {
      steps {
        echo 'Deploying.....'
      }
    }
    stage('Build Information.....') {
      steps {
        echo 'BUILD INFORMATION:'
        echo "Build results can be found in here ${BUILD_URL}"
      }
    }
  }
}
