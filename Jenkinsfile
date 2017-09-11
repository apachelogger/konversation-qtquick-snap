env.DIST = 'xenial'
env.TYPE = 'unstable'
env.DOCKER_ENV_WHITELIST = 'APPNAME'

cleanNode('8core-amd64 && cloud') {
  stage('clone') {
    checkout scm
  }
  stage('snapcraft') {
    sh '~/tooling/nci/contain.rb rake snapcraft'
    stash includes: '**', name: 'snapcraft'
  }
}

cleanNode('master') {
  stage 'snapcraft push'
  unstash 'snaps'
  // Temporary workspace during pipeline execution can't be accessed via UI, so
  // this should be save.
  // Even so we should move to a contain.rb which forward mounts the snapcraft
  // dir as volume into the container.
  sh 'cp ~/.config/snapcraft/snapcraft.cfg snapcraft.cfg'
  sh '~/tooling/nci/contain.rb rake publish'
}

def cleanNode(label = null, body) {
  node(label) {
    deleteDir()
    try {
      wrap([$class: 'AnsiColorBuildWrapper', colorMapName: 'xterm']) {
        wrap([$class: 'TimestamperBuildWrapper']) {
          body()
        }
      }
    } finally {
      step([$class: 'WsCleanup', cleanWhenFailure: true])
    }
  }
}
