pipeline {
    agent any
    stages {
        stage('Clean Workspace') {
            steps {
                //cleanWs() // 清理工作区
                echo '清理工作区...'
            }
        }
        stage('初始化') {
            steps {
                echo '初始化阶段...'
                

            }
        }
        stage('构建') {
            steps {
                echo '构建阶段...'
                script{
                     sh 'git checkout master'
                     def branch = sh(script: 'git rev-parse --abbrev-ref HEAD', returnStdout: true).trim()
                        echo "当前分支: ${branch}" 
                }
                
                sh 'git status' 
                sh 'git log -5 --oneline' 
                
                
                
            }
        }
        stage('测试') {
            steps {

                sh 'pwd'
                sh 'ls -l'  // 列出当前目录中的文件
               
                echo '测试阶段...'
                sh 'git for-each-ref --format "\'%(taggername)\'" refs/tags/mytest1'
 
            }
        }
        stage('部署') {
            steps {
                echo '部署阶段...'
                 script {
                     sh 'rm -rf /var/lib/jenkins/workspace/git-pipline/*'
                    // 获取最近一次提交的标签名
                    def tagName = sh(script: "git tag --sort=-creatordate | head -n 1", returnStdout: true).trim()
                    echo "取得的标签名: ${tagName}"

                    // 获取 mytest1 标签的最后一次合并提交信息
                    def commitMessage = sh(script: "git log -n 1 --merges --format=%s ${tagName}", returnStdout: true).trim()
                    echo "最后一次合并提交的信息: ${commitMessage}"
                    
                     //作者
                    def tagInfo = sh(script: "git show ${tagName} --format='%an' --no-patch", returnStdout: true).trim()
                    echo "Tag 作者: ${tagInfo}"

                    //version
                   def version = sh(script: "git rev-list -n 1 ${tagName}", returnStdout: true).trim()
                   echo "Version: ${version}"
              
                }
        
            }
        }
    }
}
