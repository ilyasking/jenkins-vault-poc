pipeline {
  agent any

  options {
    // Keep logs shorter on first runs
    timestamps()
  }

  stages {
    stage('Check tools') {
      steps {
        sh 'echo "Git version:"; git --version || true'
        sh 'echo "Curl version:"; curl --version | head -n 1 || true'
      }
    }

    stage('Read secret from Vault') {
      steps {
        withVault(vaultSecrets: [[
          path: 'kv/jenkins/example', engineVersion: 2,
          secretValues: [
            [envVar: 'VAULT_USERNAME', vaultKey: 'username'],
            [envVar: 'VAULT_PASSWORD', vaultKey: 'password']
          ]
        ]]) {
          sh '''
            echo "Fetched from Vault:"
            echo "  VAULT_USERNAME=$VAULT_USERNAME"
            echo "  VAULT_PASSWORD length: ${#VAULT_PASSWORD}"
          '''
        }
      }
    }

    stage('Do something using secrets') {
      environment {
        // (Example) build args, API calls, etc., using the env vars
      }
      steps {
        sh 'echo "Pretend we build & deploy here..."'
      }
    }
  }
}
