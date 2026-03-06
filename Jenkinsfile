Pipeline {
	agent any

	stages {
		stage('checkout') {
			steps {
				git "git@github.com:gessup/arduino-ci-demo.git'
			}
}
	Stage('Build') {
		steps {
			sh '''
			arduino-cli core update-index
			arduino-cli core install Arduino:avr
			arduino-cli compile --fqbn Arduino:avr:uno .
			'''
}
}
}
}
