pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git branch: 'ZD-9990', url: 'https://github.com/pavel-snyk/jenkins-showcases'
      }
    }

    stage('Terraform Plan') {
      agent {
        docker {
          image 'hashicorp/terraform:0.12.25'
          args '--entrypoint="" -u root'
        }
      }

      steps {
        sh 'terraform init -no-color'
        sh 'terraform plan -no-color'
      }
    }

    stage('Snyk Scan') {
      agent {
        docker {
          image 'debian'
          args '-v /opt/jenkins/tools/:/opt/jenkins/tools/'
        }
      }

      steps {
        snykSecurity snykInstallation: 'snyk@latest',
                     snykTokenId: 'pavel-snyk-token',
                     monitorProjectOnBuild: false,
                     failOnIssues: false,
                     additionalArguments: '--iac --debug'

        sh 'cat snyk_report.json'
      }
    }
  }
}
