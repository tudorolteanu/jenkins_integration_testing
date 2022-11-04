def gv

pipeline {
    agent any
    parameters {
        choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'], description: 'Choose version to deploy')
        booleanParam(name: 'executeTests', defaultValue: true, description: 'Run performance script?')
    }
    stages {
        stage("init") {
            steps {
                script {
                   gv = load "script.groovy" 
                }
            }
        }
        stage("build") {
            steps {
                script {
                    gv.buildApp()
                }
            }
        }
        stage("test") {
            when {
                expression {
                    params.executeTests
                }
            }
            steps {
                script {
                    bat '''cd C:\\BPS\\APACHE\\apache-jmeter-5.5\\bin
                        jmeter -n -t C:\\TESTING\\PerformanceScripts\\jmeter.jmx -l C:\\TESTING\\PerformanceScripts\\result\\TestResult1.jtl'''
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    gv.deployApp()
                }
            }
        }
    }   
}
