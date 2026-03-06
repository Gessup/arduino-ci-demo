pipeline {
	agent any

	environment {
		//zorg dat we een lokale in PTAH hebben voor aarding-cli
		PATH = "$HOME/bin:$PATH"
}

	stages {
		stage('checkout') {
			steps {
				git "git@github.com:gessup/arduino-ci-demo.git"

				// Pas aan: zet je juiste branchenaam indien anders
				git branch: 'main', url: 'git@github.com:gessup/arduino-ci-demo.git'
			}
}
	stage('Arduino CLI') {
		steps {
			sh '''
			set -e

			# lokale bin-map voor tools
			mkdir -p "$HOME/bin"

			#installeer arduino-cli in de user space als die nog niet bestaat
			if ! command -v arduino-cli >/dev/null 2>&1; then
				echo "[INFO] Installing arduino CLI locally in $HOME/bin ..."
				curl -fsSL https://raw.githubusercontent.com/arduino/arduino-cli/master/install.sh | sh
				mv bin/arduino-cli "$HOME/bin/arduino-cli"
				fi

				arduino-cli version

			#Config en cores
			arduino-cli config init || true
			arduino-cli core update-index
			arduino-cli core install arduino:avr
			'''
}
}
	stage('Build') {
		steps {
			//compileer de sketch in de repo root (manaam == .ino naam)
			sh 'arduino-cli compile -- fqbn arduino:avr:uno .'
}
}
}
}
