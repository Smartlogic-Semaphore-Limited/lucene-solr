@Library('smartlogic-common@v2') _
smartlogic([
  docker: 'smartlogic/custom/centos7/solr.json:master-474',
  parameters: {getParameters()},
  settings: [
    blackduck: [scan: [args: "--detect.excluded.detector.types='pip,gradle'"]],
  ],
  builder: smartlogic.simpleBuilder({
    def solrBuildParameters = getSolrBuildParameters();
    sh "(cd solr && ant package ${solrBuildParameters})"
    if(!params.SKIP_TESTS) {
      stage("Run tests") {
        sh "(cd solr && ant test)"
      }
    }
  }),
  archive: {archive()},
  release: [skip: true]
])

def getParameters() {[
  string(
    name: 'SOLR_VERSION',
    defaultValue: "",
    description: 'Version of SOLR to build'
  ),
  booleanParam(
    name: 'SKIP_TESTS',
    defaultValue: false,
    description: 'Skip running tests'
  )
]}

def getSolrBuildParameters() {
  if(params.SOLR_VERSION) {
    return "-Dversion.base=${params.SOLR_VERSION} -Dversion=${params.SOLR_VERSION}"
  }
  ""
}

def archive() {
  archiveArtifacts 'solr/package/*.zip,solr/package/*.tgz,solr/package/*.sha512'
}
