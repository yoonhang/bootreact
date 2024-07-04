스프링부트와 리액트 함께 쓰기

한코딩

https://www.youtube.com/watch?v=md7AdsH9RCA


application.properties
"proxy": "http://localhost:8080",


// react....
// build.gradle
def frontendDir = "$projectDir/src/main/reactfront"

sourceSets {
    main {
        resources { srcDirs = ["$projectDir/src/main/resources"]
        }
    }
}

processResources { dependsOn "copyReactBuildFiles" }

task installReact(type: Exec) {
    workingDir "$frontendDir"
    inputs.dir "$frontendDir"
    group = BasePlugin.BUILD_GROUP
    if (System.getProperty('os.name').toLowerCase(Locale.ROOT).contains('windows')) {
        commandLine "npm.cmd", "audit", "fix"
        commandLine 'npm.cmd', 'install' }
    else {
        commandLine "npm", "audit", "fix" commandLine 'npm', 'install'
    }
}

task buildReact(type: Exec) {
    dependsOn "installReact"
    workingDir "$frontendDir"
    inputs.dir "$frontendDir"
    group = BasePlugin.BUILD_GROUP
    if (System.getProperty('os.name').toLowerCase(Locale.ROOT).contains('windows')) {
        commandLine "npm.cmd", "run-script", "build"
    } else {
        commandLine "npm", "run-script", "build"
    }
}

task copyReactBuildFiles(type: Copy) {
    dependsOn "buildReact"
    from "$frontendDir/build"
    into "$projectDir/src/main/resources/static"
}
