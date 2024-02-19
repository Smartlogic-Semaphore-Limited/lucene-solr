@Library('smartlogic-common@v2') _
smartlogic([
  docker: 'smartlogic/custom/centos7/solr.json:master-474',
  parameters: {getParameters()},
  settings: [
    blackduck: [scan: [args: "--detect.excluded.detector.types='pip,gradle'"]],
  ],
  builder: smartlogic.simpleBuilder({
    sh "ant validate"
    def solrBuildParameters = getSolrBuildParameters();
    sh "(cd solr && ant package ${solrBuildParameters})"
    if(!params.SKIP_TESTS) {
      stage("Run tests") {
        sh "(cd solr && ant test ${params.TESTS_PARAMS})"
      }
    }
  }),
  archive: {archive()},
  release: [skip: true]
])

def getParameters() {[
  string(
    name: 'SOLR_BASE_VERSION',
    defaultValue: "",
    description: 'Base version of SOLR to build, must be "x.y.z" (major, minor, bugfix) or "x.y.z.1/2" (+ prerelease) and numeric only'
  ),
  string(
    name: 'SOLR_VERSION',
    defaultValue: "",
    description: 'Version of SOLR to build. If you pass it to set a release version, it must match SOLR_BASE_VERSION, optionally followed by a suffix (e.g., "-SNAPSHOT").'
  ),
  booleanParam(
    name: 'SKIP_TESTS',
    defaultValue: false,
    description: 'Skip running tests'
  ),
  string(
    name: 'TESTS_PARAMS',
    defaultValue: "",
    description: 'Parameters to pass to the SOLR "ant test" command, e.g. "-Dtestcase=TestFieldCacheTermsFilter".'
  ),
  booleanParam(
    name: 'ARCHIVE',
    defaultValue: false,
    description: 'Archive build artifacts'
  )
]}

def getSolrBuildParameters() {
  def solrBuildParams = "";
  if(params.SOLR_BASE_VERSION) {
    solrBuildParams += "-Dversion.base=${params.SOLR_BASE_VERSION} "
  }
  if(params.SOLR_VERSION) {
    solrBuildParams += "-Dversion=${params.SOLR_VERSION}"
  }
  solrBuildParams
}

def archive() {
  if(params.ARCHIVE) {
    archiveArtifacts 'solr/package/*.zip,solr/package/*.tgz,solr/package/*.sha512'
  }
}
