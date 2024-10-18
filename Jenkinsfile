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
    agent any 
    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    bat 'npm install'
                    bat 'npx playwright install'
                }
            }
        }
        stage('Help') {
            steps {
                bat 'npx playwright test --help'
            }
        }
        stage('Run Tests') {
            steps {
                bat 'npx playwright test'
            }
            post {
                success {}
                    archiveArtifacts(artifacts: 'playwright-report/**', followSymlinks: false)
                    bat 'del *.png'  // Use `del` for deleting files on Windows
                }
            }
        }
}
