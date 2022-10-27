@Library('smartlogic-common@v2') _
smartlogic([
  settings: [
    blackduck: [scan: [args: "--detect.excluded.detector.types='pip,gradle' detect.blackduck.signature.scanner.dry.run=true --detect.blackduck.signature.scanner.arguments='--dryRunWriteDir=/root/blackduck/dryrun'"]],
  ],
  archive: {archive()},
  buildWrapper: {
    sh "chown -R root ./"
    it()
  }
])

def archive() {
  sh 'mkdir blackduck-dryrun && cp -r /root/blackduck/dryrun/* blackduck-dryrun/'
  archiveArtifacts 'blackduck-dryrun/*'
  sh 'rm -rf blackduck-dryrun'
}
