pipeline {
  agent any
 
  environment {
    // Zorg dat we een lokale bin in PATH hebben voor arduino-cli
    PATH = "$HOME/bin:$PATH"
  }
 
  stages {
    stage('Prepare SSH known_hosts') {
      steps {
        sh '''
          set -e
          mkdir -p "$HOME/.ssh"
          chmod 700 "$HOME/.ssh"
 
          # Oude/dubbele entries voor github.com verwijderen (indien aanwezig)
          ssh-keygen -R github.com -f "$HOME/.ssh/known_hosts" || true
 
          # ED25519 host key van GitHub toevoegen (hashed vorm is prima)
          ssh-keyscan -H -t ed25519 github.com >> "$HOME/.ssh/known_hosts"
 
          chmod 600 "$HOME/.ssh/known_hosts"
 
          echo "[INFO] known_hosts content (github.com):"
          grep github.com "$HOME/.ssh/known_hosts" || true
        '''
      }
    }
 
    stage('Checkout') {
      steps {
        // PAS AAN: zet je juiste branch-naam indien anders
        git branch: 'main',
            url: 'git@github.com:gessup/arduino-ci-demo-git',
            credentialsId: 'git'
      }
    }
 
    stage('Setup Arduino CLI') {
      steps {
        sh '''
          set -e
 
          # Lokale bin-map voor tools
          mkdir -p "$HOME/bin"
 
          # Installeer arduino-cli in de user space als die nog niet bestaat
          if ! command -v arduino-cli >/dev/null 2>&1; then
            echo "[INFO] Installing Arduino CLI locally in $HOME/bin ..."
            curl -fsSL https://raw.githubusercontent.com/arduino/arduino-cli/master/install.sh | sh
            mv bin/arduino-cli "$HOME/bin/arduino-cli"
          fi
 
          arduino-cli version
 
          # Config en cores
          arduino-cli config init || true
          arduino-cli core update-index
          arduino-cli core install arduino:avr
        '''
      }
    }
 
    stage('Build') {
      steps {
        // Compileer de sketch in de repo root (mapnaam == .ino naam)
        sh 'arduino-cli compile --fqbn arduino:avr:uno .'
      }
    }
  }
}
 