properties([
    parameters([
        gitParameter(branch: '',
                     branchFilter: 'origin/(.*)',
                     defaultValue: 'master',
                     description: '',
                     name: 'BRANCH',
                     quickFilterEnabled: false,
                     selectedValue: 'NONE',
                     sortMode: 'NONE',
                     tagFilter: '*',
                     type: 'PT_BRANCH')
    ])
])

pipeline {
    agent any
    
    environment {
        SERVER_ID = 'artifactory'
    }

    stages {
        stage("Checkout") {
            steps {
                git branch: "${params.BRANCH}", url: 'https://github.com/sergiosamu/blog-pipelines.git'
            }
        }
        stage("Build") {
            steps {
                warnError("Fallo pruebas unitarias") {
                    withMaven(maven: "Maven363") {
                        sh "mvn package"
                    }
                }
            }    
        }
        stage("Publish artifact") {
            steps {
                rtUpload (
                    serverId: "$SERVER_ID",
                    spec: '''{
                          "files": [
                            {
                              "pattern": "target/blog-pipelines*.jar",
                              "target": "libs-snapshot-local/com/sergiosanchez/pipelines/"
                            }
                         ]
                    }'''
                )
            }
        }
    }
}
