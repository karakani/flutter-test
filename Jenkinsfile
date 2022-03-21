pipeline {
    agent none
    options {
        checkoutToSubdirectory "source"
    }

    stages {
        stage('Build') {
            parallel {
                stage('android') {
                    agent {
                        dockerfile {
                            label 'docker'
                            filename 'Dockerfile.android'
                        }
                    }
                    steps {
                        dir("flutter") {
                            checkout([
                                $class: 'GitSCM',
                                branches: [[name: 'refs/tags/2.10.3']],
                                extensions: [[$class: 'CloneOption', shallow: false, depth: 0, reference: '']],
                                userRemoteConfigs: [[
                                    credentialsId: scm.userRemoteConfigs[0].credentialsId,
                                    url: 'git@github.com:flutter/flutter.git'
                                    ]],
                                ])
                        }
                        dir("source") {
                            sh '../flutter/bin/flutter build apk --release'
                            
                            deleteDir()
                        }
                    }
                }

                stage('linux desktop') {
                    agent {
                        label "linux"
                    }
                    steps {
                        dir("flutter") {
                            checkout([
                                $class: 'GitSCM',
                                branches: [[name: 'refs/tags/2.10.3']],
                                extensions: [[$class: 'CloneOption', shallow: false, depth: 0, reference: '']],
                                userRemoteConfigs: [[
                                    credentialsId: scm.userRemoteConfigs[0].credentialsId,
                                    url: 'git@github.com:flutter/flutter.git'
                                    ]],
                                ])
                        }
                        dir("source") {
                            sh '../flutter/bin/flutter config --enable-linux-desktop'
                            sh '../flutter/bin/flutter build linux --release'
                            
                            deleteDir()
                        }
                    }
                }

                stage('macOS') {
                    agent {
                        label "macos"
                    }
                    steps {
                        dir("flutter") {
                            checkout([
                                $class: 'GitSCM',
                                branches: [[name: 'refs/tags/2.10.3']],
                                extensions: [[$class: 'CloneOption', shallow: false, depth: 0, reference: '']],
                                userRemoteConfigs: [[
                                    credentialsId: scm.userRemoteConfigs[0].credentialsId,
                                    url: 'git@github.com:flutter/flutter.git'
                                    ]],
                                ])
                        }
                        dir("source") {
                            sh '../flutter/bin/flutter config --enable-macos-desktop'
                            sh '../flutter/bin/flutter build macos --release'
                            
                            deleteDir()
                        }
                    }
                }

                stage('iOS') {
                    agent {
                        label "macos"
                    }
                    steps {
                        dir("flutter") {
                            checkout([
                                $class: 'GitSCM',
                                branches: [[name: 'refs/tags/2.10.3']],
                                extensions: [[$class: 'CloneOption', shallow: false, depth: 0, reference: '']],
                                userRemoteConfigs: [[
                                    credentialsId: scm.userRemoteConfigs[0].credentialsId,
                                    url: 'git@github.com:flutter/flutter.git'
                                    ]],
                                ])
                        }
                        dir("source") {
                            sh '../flutter/bin/flutter build ios --release --no-codesign'
                            
                            deleteDir()
                        }
                    }
                }
            }
        }
    }
}

