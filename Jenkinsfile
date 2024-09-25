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
                        def revision = sh(script: "git tag --sort=-creatordate | head -n 1", returnStdout: true).trim()
                        if (revision != null && revision.trim() != '') {
                            sendNewRelicChangeNotification(revision)
                        }
                    } catch (Exception e) {
                        echo "New Relicの本番リリースディプロイの通知送信に失敗しました: ${e.message}"
                       
                    }
                    
                }
            }
        }
    }
}

def sendNewRelicChangeNotification(revision) {
    def newRelicUrl = "https://api.newrelic.com/v2/applications/${env.NEW_RELIC_APP_ID}/deployments.json"
    echo "#####${revision}"
    //description
    def description = sh(script: "git rev-list -n 1 ${revision}", returnStdout: true).trim()

    //userを取得
    def user = sh(script: "git show ${revision} --format='%an' --no-patch", returnStdout: true).trim()

    // changelog
    def changelog = sh(script: "git log -n 1 --merges --format=%s ${revision}", returnStdout: true).trim()
    
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
