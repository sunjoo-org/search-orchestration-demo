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
                        //pullRequest.comment("Ttes Commenv " + env.JENKINS_URL)
                    }
                }
            }
        }
    }
    post {
        success {
            script {
            /*
                    pullRequest.createStatus(status: 'error',
                         description: 'Failed',
                         targetUrl: "${env.BUILD_URL}")
                         */
                echo "post: success"
                if(env.CHANGE_ID) {
                    /*
                    pullRequest.comment("Post Section")
                    pullRequest.merge(commitTitle: 'Merge from test action', commitMessage: 'Merge Test', mergeMethod: 'squash')
                    */
                    for (review in pullRequest.reviews) {
                        echo "${review.user} has a review in ${review.state} state for Pull Request. Review body: ${review.body}"
                    }
                    // merge_method	string	body	Merge method to use. Possible values are merge, squash or rebase. Default is merge.
                }
            }
        }

    }
}
