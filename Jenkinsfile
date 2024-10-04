def sendNewRelicChangeNotification() {
    def newRelicUrl = "https://api.newrelic.com/v2/applications/${env.NEW_RELIC_APP_ID}/deployments.json"
 
    //descriptionを取得
    def revision = sh(script: "git tag --sort=-creatordate | head -n 1", returnStdout: true).trim()
    echo "0.revision:${revision}"
    
    def description = sh(script: "git rev-list -n 1 ${revision} | cut -c 1-6", returnStdout: true).trim()
    echo "1.description:${description}"

    //userを取得
    def user = sh(script: "git show ${revision} --format='%an' --no-patch", returnStdout: true).trim()
    echo "2.user:${user}"

    // changelogを取得
    def changelog = sh(script: "git log -n 1 --merges --format=%s ${revision}", returnStdout: true).trim()
    echo "3.changelog:${changelog}"
    
    def requestBody = """
    {
      "deployment": {
        "revision": "${revision}",
        "changelog": "${changelog}",
        "description": "${description}",
        "user": "${user}"
      }
    }
    """
    def response = httpRequest(
        httpMode: 'POST',
        url: newRelicUrl,
        customHeaders: [
            [name: 'X-Api-Key', value: env.NEW_RELIC_API_KEY],
            [name: 'Content-Type', value: 'application/json; charset=utf-8']
        ],
        requestBody: requestBody,
        validResponseCodes: '200:299'
    )
}
pipeline {
    agent any
    environment {
            NEW_RELIC_API_KEY = 'NRAK-VSF0X62BE2VQJJE1VN2SY9WFN2T' 
            NEW_RELIC_APP_ID = '1326011399'
            JAVA_OPTS = '-Dfile.encoding=UTF-8'
    }
    stages {
        stage('Deploy') {
            steps {
                echo 'Deploy......'
                 script { 
                     try {
                   
                        //if (revision != null && revision.trim() != '') {
                            //if (env.GIT_BRANCH == "master") {
                            sh(script: "git log -n 1 ")
                            sh(script: "git tag --sort=-creatordate ")
                            
                            //echo "10.revision:${revision}"
                            //sendNewRelicChangeNotification()
                            //}
                       // }
                    } catch (Exception e) {
                        echo "New Relicの本番リリースディプロイの通知送信に失敗しました: ${e.message}"
                       
                    }
                    
                }
                script {
                    echo("~~~~~~~~~11~~~~~~~~~~~~~~~~~~~~~~~~~~~~")
                    sh(script: "pwd ~")
                }
                script {
                    echo("~~~~~~~~~22~~~~~~~~~~~~~~~~~~~~~~~~~~~~")
                    sh(script: "ls ~/1.txt")
                }
            }
        }
    }
}


