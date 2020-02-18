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

def SERVER_ID="artifactory"

node {
    stage("Checkout") {
        git branch: "${params.BRANCH}", url: 'https://github.com/sergiosamu/blog-pipelines.git'
    }
    stage("Build") {
        try {
            withMaven(maven: "Maven363") {
                sh "mvn package"
            }
        } catch (error) {
            currentBuild.result='UNSTABLE'
        }
    }
    stage("Publish artifact") {
        def server = Artifactory.server "$SERVER_ID"

        def uploadSpec = """{
          "files": [
            {
              "pattern": "target/blog-pipelines*.jar",
              "target": "libs-snapshot-local/com/sergiosanchez/pipelines/"
            }
         ]
        }"""
        
        server.upload(uploadSpec)
    }
    
}