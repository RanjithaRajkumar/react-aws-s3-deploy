pipeline {
  agent { any
  }
  environment {
    CI = 'true'
    HOME = '.'
    npm_config_cache = 'npm-cache'
  }
  stages {
    stage('Install Packages') {
      steps {
        sh 'npm install'
      }
    }
    stage('Test and Build') {
      parallel {
        stage('Run Tests') {
          steps {
            sh 'npm run test'
          }
        }
        stage('Create Build Artifacts') {
          steps {
            sh 'npm run build'
          }
        }
      }
    }
    stage('Deployment') {
      parallel {
        stage('Staging') {
          when {
            branch 'staging'
          }
          steps {
            withAWS(region:'us-east-2',credentials:'aws_s3_pipeline') {
              s3Delete(bucket: 'ranjitha123', path:'**/*')
              s3Upload(bucket: 'ranjitha123', workingDir:'build', includePathPattern:'**/*');
          
        }
      }
    }
  }
}
}
}
