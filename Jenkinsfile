@Library('smartlogic-common@v2') _
smartlogic([
  settings: [
    blackduck: [scan: [args: "--detect.excluded.detector.types='pip,gradle' detect.blackduck.signature.scanner.dry.run=true --detect.blackduck.signature.scanner.arguments='--dryRunWriteDir=blackduck-dryrun'"]],
  ],
  archive: {archive()}
])

def archive() {
  archiveArtifacts(artifacts: "blackduck-dryrun/*", allowEmptyArchive: false)
}
