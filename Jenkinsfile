// A Jenkins Declarative Pipeline script.
pipeline {
    // The 'agent' section specifies where the pipeline will run.
    // 'any' means Jenkins will allocate an agent for the job.
    agent any

    // The 'stages' block defines the logical steps of the pipeline.
    stages {
        // This stage checks out the code from the Git repository using the scmGit step.
        stage('Checkout Code') {
            steps {
                // The scmGit step provides a more detailed way to configure Git checkout.
                // It specifies the branch to checkout and the credentials to use for authentication.
                checkout scmGit(
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[
                        // credentialsId: '',
                        url: 'https://github.com/pmeharg/nodegoat-reachability.git'
                    ]]
                )
            }
        }
        // Second stage: Performs the Sonatype Lifecycle evaluation.
        stage('Sonatype Lifecycle Evaluation') {
            steps {
                // The 'sh' step executes a shell command on the Jenkins agent.
                // This command assumes you have the Sonatype Lifecycle plugin and/or
                // the Sonatype CLI configured and available in your Jenkins environment.
                // The exact command may vary based on your specific setup (e.g., using Maven or Gradle).
                // This example uses a placeholder command that you would replace with your actual command.
                nexusPolicyEvaluation (
                    iqApplication: selectedApplication('nodegoat-reachability'), 
                    iqInstanceId: 'NexusIQServer', 
                    iqStage: 'build', 
                    iqScanPatterns: [[scanPattern: '**/*.js'], [scanPattern: '**/*.zip'], [scanPattern:"**/package*.json"]],
                    reachability: [ 
                        logLevel: 'DEBUG',
                        jsAnalysis:[ 
                            enable: true,
                            packageJsonFile: 'package.json',
                            sourceFiles: [
                                [pattern: 'app/**/*'], [pattern: 'artifacts/**/*'], [pattern: 'config/**/*']
                            ],
                            excludeFiles:[
                                [pattern: 'node_modules/**/*']
                            ]
                        ]
                    ]
                )
            }
        }
        
    }
}
