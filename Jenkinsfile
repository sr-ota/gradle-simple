node {
    def server
    def rtGradle = Artifactory.newGradleBuild()
    def buildInfo = Artifactory.newBuildInfo()

    stage ('Artifactory configuration') {
        // Obtain an Artifactory server instance, defined in Jenkins --> Manage Jenkins --> Configure System:
        server = Artifactory.server "jfrogeval"
        rtGradle.tool = "latest"
        // rtGradle.deployer repo: ARTIFACTORY_LOCAL_SNAPSHOT_REPO, server: server
        // rtGradle.resolver repo: ARTIFACTORY_VIRTUAL_RELEASE_REPO, server: server
    }

    stage ('Config Build Info') {
        buildInfo.env.capture = true
        buildInfo.env.filter.addInclude ("*")
        // buildInfo.env.filter.addExclude ("DONT_COLLECT*")
    }

    //stage ('Extra gradle configurations') {
        // rtGradle.deployer.artifactDeploymentPatterns.addExclude("*.war")
        // tGradle.usesPlugin = true // Artifactory plugin already defined in build script
        // rtGradle.useWrapper = true
    //}

    stage ('Exec Gradle') {
        rtGradle.run rootDir: ".", tasks: 'clean build', buildInfo: buildInfo
    }

    stage ('Publish build info') {
        server.publishBuildInfo buildInfo
    }
}
