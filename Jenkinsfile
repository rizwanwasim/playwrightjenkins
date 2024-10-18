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
                script {
                    // Run tests with the defined configuration and generate the report
                    bat 'npx playwright test --report=html'
                }
            }
            post {
                success {
                    // Archive the HTML report folder
                    archiveArtifacts artifacts: 'playwright-report/**', followSymlinks: false
                }
                always {
                    // Optionally clean up the workspace
                    bat 'del /q playwright-report\\*'  // Delete contents of the report folder on Windows
                }
            }
        }

        stage('Publish Report') {
            steps {
                // Publish the HTML report
                publishHTML([
                    reportDir: 'playwright-report', // Directory containing the report
                    reportFiles: 'index.html', // Main report file
                    reportName: 'Playwright Test Report', // Name of the report
                    keepAll: true, // Keep all reports
                    alwaysLinkToLastBuild: true // Always link to the last build
                ])
            }
        }
    }
}


// stages {
//         stage('Checkout SCM') {
//             steps {
//                 script {
//                     echo "Checking out source code"
//                     checkout scm
//                 }
//             }
//         }

//         stage('Install Node.js') {
//             steps {
//                 script {
//                     echo "Installing Node.js version ${NODE_VERSION}"
//                     // Use the NodeJS plugin for installation
//                     nodejs('Node') {
//                         echo "Node.js environment is set up"
//                         // Verify Node.js installation
//                         sh 'node -v'
//                         sh 'npm -v'
//                     }
//                 }
//             }
//         }

//         stage('Install Project Dependencies') {
//             steps {
//                 script {
//                     echo "Installing project dependencies"
//                     sh 'npm ci'
//                     echo "Dependencies installation completed successfully"
//                 }
//             }
//         }

//         stage('Install xmlstarlet') {
//             steps {
//                 script {
//                     echo "Installing xmlstarlet"
//                     if (isUnix()) {
//                         sh 'sudo apt-get update && sudo apt-get install -y xmlstarlet'
//                     } else {
//                         bat 'choco install xmlstarlet -y'
//                     }
//                 }
//             }
//         }

//         stage('Install Playwright Browser (Chromium)') {
//             steps {
//                 script {
//                     echo "Installing Playwright dependencies and Chromium"
//                     sh 'npx playwright install-deps chromium'
//                     sh 'npx playwright install chromium'
//                 }
//             }
//         }

//         stage('Run Playwright Tests') {
//             steps {
//                 script {
//                     echo "Running Playwright tests"
//                     def testResult = sh(script: 'npx playwright test', returnStatus: true)
//                     if (testResult != 0) {
//                         error("Playwright tests failed with exit code: ${testResult}")
//                     }
//                     echo "Playwright tests completed successfully"
//                 }
//             }
//         }

//         stage('Save Test Results') {
//             steps {
//                 script {
//                     echo "Saving test results"
//                     sh 'cp junit-report/results.xml results.xml'
//                 }
//             }
//         }

//         stage('Read XML and Send Teams Notification') {
//             steps {
//                 script {
//                     echo "Reading XML report and preparing Teams notification"
//                     def totalTests = sh(script: "xmlstarlet sel -t -v '/testsuites/@tests' results.xml", returnStdout: true).trim()
//                     def failedTests = sh(script: "xmlstarlet sel -t -v '/testsuites/@failures' results.xml", returnStdout: true).trim()
//                     def skippedTests = sh(script: "xmlstarlet sel -t -v '/testsuites/@skipped' results.xml", returnStdout: true).trim()
//                     def passedTests = (totalTests.toInteger() - (failedTests.toInteger() + skippedTests.toInteger()))

//                     def status = failedTests.toInteger() == 0 ? 'passed' : 'failed'
//                     def icon = status == 'passed' ? '✔️' : '❌'
//                     def color = status == 'passed' ? '00FF00' : 'FF0000'

//                     def payload = """
//                     {
//                         "@type": "MessageCard",
//                         "@context": "http://schema.org/extensions",
//                         "summary": "Daily Run",
//                         "themeColor": "${color}",
//                         "sections": [
//                             {
//                                 "activityTitle": "New Daily Run",
//                                 "facts": [
//                                     {
//                                         "name": "Repository",
//                                         "value": "${env.GIT_URL}"
//                                     },
//                                     {
//                                         "name": "Status",
//                                         "value": "${icon} ${status}"
//                                     },
//                                     {
//                                         "name": "Total Tests",
//                                         "value": "${totalTests}"
//                                     },
//                                     {
//                                         "name": "Passed Tests",
//                                         "value": "${passedTests}"
//                                     },
//                                     {
//                                         "name": "Failed Tests",
//                                         "value": "${failedTests}"
//                                     },
//                                     {
//                                         "name": "Skipped Tests",
//                                         "value": "${skippedTests}"
//                                     },
//                                     {
//                                         "name": "Report Download Url",
//                                         "value": "[Click to Download Report](results.xml)"
//                                     },
//                                     {
//                                         "name": "Project Name",
//                                         "value": "Auto QA Dynamics"
//                                     }
//                                 ],
//                                 "markdown": true
//                             }
//                         ]
//                     }
//                     """

//                     echo "Payload prepared for Teams notification"

//                     // Send payload to Microsoft Teams webhook URL
//                     try {
//                         sh "curl -X POST -H 'Content-type: application/json' --data '${payload}' '${WEBHOOK_URL}'"
//                         echo "Teams notification sent successfully"
//                     } catch (Exception e) {
//                         error("Failed to send Teams notification: ${e.message}")
//                     }
//                 }
//             }
//         }
//     }

//     post {
//         always {
//             echo "Cleaning up workspace"
//             cleanWs()
//         }
//     }
// }