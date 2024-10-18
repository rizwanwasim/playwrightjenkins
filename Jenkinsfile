// pipeline {
//   agent { 
//     docker { 
//       image 'mcr.microsoft.com/playwright:v1.17.2-focal'
//     } 
//   }
//   stages {
//     stage('install playwright') {
//       steps {
//         sh '''
//           npm i -D @playwright/test
//           npx playwright install
//         '''
//       }
//     }
//     stage('help') {
//       steps {
//         sh 'npx playwright test --help'
//       }
//     }
//     stage('test') {
//       steps {
//         sh '''
//           npx playwright test --list
//           npx playwright test
//         '''
//       }
//       post {
//         success {
//           archiveArtifacts(artifacts: 'homepage-*.png', followSymlinks: false)
//           sh 'rm -rf *.png'
//         }
//       }
//     }
//   }
// }
// --------------------------------------------------------
// pipeline {
//   agent {
//     docker {
//       image 'mcr.microsoft.com/playwright:v1.17.2-focal'
//       reuseNode true  // Reuse the Docker container for subsequent stages if possible
//     }
//   }
//   stages {
//     stage('install playwright') {
//       steps {
//         sh '''
//           npm i -D @playwright/test
//           npx playwright install
//         '''
//       }
//     }
//     stage('help') {
//       steps {
//         sh 'npx playwright test --help'
//       }
//     }
//     stage('test') {
//       steps {
//         sh '''
//           npx playwright test --list
//           npx playwright test
//         '''
//       }
//       post {
//         success {
//           archiveArtifacts(artifacts: 'homepage-*.png', followSymlinks: false)
//           sh 'rm -rf *.png'
//         }
//       }
//     }
//   }
// }
// -------------------------------------------------------------------------------------------------
pipeline {
    agent any  // Use any available Jenkins agent
    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    // Install dependencies
                    sh 'npm i -D @playwright/test'
                    sh 'npx playwright install'
                }
            }
        }
        stage('Help') {
            steps {
                sh 'npx playwright test --help'
            }
        }
        stage('Run Tests') {
            steps {
                // Run tests with a single worker
                sh 'npx playwright test --workers=1'
            }
            post {
                success {
                    archiveArtifacts(artifacts: 'homepage-*.png', followSymlinks: false)
                    sh 'rm -rf *.png'
                }
            }
        }
    }
}

