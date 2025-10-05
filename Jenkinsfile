pipeline {
  agent any
  options { timestamps() }

  stages {
    stage('Check tools') {
      steps {
        sh 'echo "Git version:"; git --version || true'
        sh 'echo "Curl version:"; curl --version | head -n 1 || true'
      }
    }

    stage('Read & use secrets from Vault') {
      steps {
        withVault(
          configuration: [
            vaultUrl: 'http://vault:8200',
            vaultCredentialId: 'vault-token',
            timeout: 60
          ],
          vaultSecrets: [[
            path: 'kv/jenkins/example', engineVersion: 2,
            secretValues: [
              [envVar: 'VAULT_USERNAME', vaultKey: 'username'],
              [envVar: 'VAULT_PASSWORD', vaultKey: 'password']
            ]
          ]]
        ) {
          sh '''
            echo "Fetched from Vault:"
            echo "  VAULT_USERNAME=$VAULT_USERNAME"
            echo "  VAULT_PASSWORD length: ${#VAULT_PASSWORD}"
            echo "Build and deploy simulation successful."
          '''
        }
      }
    }
  }
}
