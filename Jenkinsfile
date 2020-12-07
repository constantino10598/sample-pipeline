node('master') {
    SCMStage()
    QueryStage()
}

def exec_script(script) {
    isUnix() ? sh(script) : bat(script)
    return this
}

def SCMStage() {
    stage('SCM Checkout') {
        cleanWs(patterns: [[pattern: 'target/*.*', type: 'INCLUDE']])
        checkout([
         $class: 'GitSCM',
         branches: scm.branches,
         doGenerateSubmoduleConfigurations: false,
         extensions: scm.extensions + [
                [$class: 'SubmoduleOption',
                recursiveSubmodules: true, // get the perf lib submodule
                trackingSubmodules: false]
            ],
         userRemoteConfigs: scm.userRemoteConfigs
        ])
    }
}

def QueryStage() {
    stage('Order Query') { 
        def sourceFile = "orders.yaml"
        if (fileExists(file: sourceFile)) {
            def newFile = "order-prev.yaml"
            writeFile(file: newFile, encoding: "UTF-8", text: readFile(file: sourceFile, encoding: "UTF-8"))
        }

        def content = readYaml file: sourceFile
        for (task in content.data){ 
            task.actual = task.prev
        }      
    }
}

