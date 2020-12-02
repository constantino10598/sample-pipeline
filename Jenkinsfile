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
        def filename = 'orders.yaml'
        def data = readYaml file: filename
        data.task.each{ entry ->
            entry.each{entries ->
                echo entries.value
            }
        }                
    }
}

