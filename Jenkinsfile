pipeline {
    agent any
    triggers {
        issueCommentTrigger('.*start build.*')
        githubPush()
    }
    stages {
        stage('Get methods') {
            steps {
                script {
                    sh 'env|sort'
                    if(env.CHANGE_ID) {
                        echo pullRequest.title
                        echo pullRequest.body
                        echo pullRequest.state
                        echo pullRequest.labels[0]
                        pullRequest.comment("Ttes Commenv " + env.BUILD_URL)
                        //pullRequest.comment("start build from Pipeline")
                    }
                    withCredentials([usernamePassword(credentialsId: 'github-sunjoo-park', passwordVariable: 'GITHUB_PASSWORD', usernameVariable: 'GITHUB_USER')]) {
                        //echo "User: ${GITHUB_USER} , Password: ${GITHUB_PASSWORD}"
                        def message = "comment by Jenkinspipeline"
                        sh 'curl -u ${GITHUB_USER}:${GITHUB_PASSWORD} -X POST -d \'{"body": "' + message + '"}\' -H "Accept: application/vnd.github.v3+json" https://api.github.com/repos/sunjoo-org/search-orchestration-demo/issues/${CHANGE_ID}/comments'
                    }
                }
            }
        }
    }
    post {
        success {
            script {
                pullRequest.createStatus(status: 'success',
                                            description: 'Checkekd',
                                            targetUrl: "${env.BUILD_URL}")
                pullRequest.createStatus(status: 'success',
                                            context: 'static analysis',
                                            description: 'Checkekd',
                                            targetUrl: "${env.BUILD_URL}")
                echo "post: success"
                if(env.CHANGE_ID) {
                    /*
                        pullRequest.merge(commitTitle: 'Merge from test action', commitMessage: 'Merge Test', mergeMethod: 'squash')
                    */
                    pullRequest.comment("Post Section")
                    for (review in pullRequest.reviews) {
                        echo "${review.user} has a review in ${review.state} state for Pull Request. Review body: ${review.body}"
                    }
                    // merge_method	string	body	Merge method to use. Possible values are merge, squash or rebase. Default is merge.
                }
            }
        }

    }
}
