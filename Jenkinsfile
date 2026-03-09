pipeline {
  agent any
 
  stages {
    stage('Checkout OK') {
      steps {
        echo "Repo is binnengehaald door Jenkins."
        sh 'ls -la'
      }
    }
 
    stage('Install Arduino CLI') {
      steps {
        sh '''
          set -eux
          mkdir -p "$HOME/bin"
          if ! command -v arduino-cli >/dev/null 2>&1; then
            curl -fsSL https://raw.githubusercontent.com/arduino/arduino-cli/master/install.sh | sh
            mv bin/arduino-cli "$HOME/bin/arduino-cli"
          fi
          "$HOME/bin/arduino-cli" version
        '''
      }
    }
 
    stage('Build Firmware') {
      steps {
        sh '''
          "$HOME/bin/arduino-cli" core update-index
          "$HOME/bin/arduino-cli" core install arduino:avr
          "$HOME/bin/arduino-cli" compile --fqbn arduino:avr:uno .
        '''
      }
    }
  }
}
