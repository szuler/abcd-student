pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
    }
    stages {
        stage('Code checkout from GitHub') {
            steps {
                script {
                    cleanWs()
                    git credentialsId: 'abcjenkins', url: 'https://github.com/szuler/abcd-student', branch: 'main'
                }
            }
        }
        stage('[Semgrep]') {
            steps {
                sh 'mkdir -p results/'                                
                sh 'semgrep --config auto --output results/semgrep_output.json --json'
            }            
            post {
                always {
                    defectDojoPublisher(artifact: '${WORKSPACE}/results/semgrep_output.json', 
                    productName: 'Juice Shop', 
                    scanType: 'Semgrep JSON Report', 
                    engagementName: 'szymon.urbanski@gmail.com')
                }
            }
        }   
        // stage('[TruffleHog] Filesystem scan') {
        //     steps {
        //         sh 'mkdir -p results/'                
        //         sh 'trufflehog git file://. --only-verified --json > ${WORKSPACE}/results/trufflehog_result.json'                            
        //     }            
        //     post {
        //         always {
        //             defectDojoPublisher(artifact: '${WORKSPACE}/results/trufflehog_result.json', 
        //             productName: 'Juice Shop', 
        //             scanType: 'Trufflehog Scan', 
        //             engagementName: 'szymon.urbanski@gmail.com')
        //         }
        //     }
        // }
        // stage('[ZAP] Baseline passive-scan') {
        //     steps {
        //         sh 'mkdir -p results/'
        //         sh '''
        //             docker run --name juice-shop -d --rm \
        //                 -p 3000:3000 \
        //                 bkimminich/juice-shop
        //             sleep 5
        //         '''
        //         sh '''
        //             docker run --name zap \
        //                 --add-host=host.docker.internal:host-gateway \
        //                 -v /mnt/c/projects/tut/abcdevsecops/abcd-student/.zap:/zap/wrk/:rw \
        //                 -t ghcr.io/zaproxy/zaproxy:stable bash -c \
        //                 "zap.sh -cmd -addonupdate; zap.sh -cmd -addoninstall communityScripts -addoninstall pscanrulesAlpha -addoninstall pscanrulesBeta -autorun /zap/wrk/passive.yaml" \
        //                 || true
        //         '''
        //     }
        //     post {
        //         always {
        //             sh '''
        //                 docker cp zap:/zap/wrk/reports/zap_html_report.html ${WORKSPACE}/results/zap_html_report.html
        //                 docker cp zap:/zap/wrk/reports/zap_xml_report.xml ${WORKSPACE}/results/zap_xml_report.xml
        //                 docker stop zap juice-shop
        //                 docker rm zap
        //             '''
        //             defectDojoPublisher(artifact: '${WORKSPACE}/results/zap_xml_report.xml', 
        //             productName: 'Juice Shop', 
        //             scanType: 'ZAP Scan', 
        //             engagementName: 'szymon.urbanski@gmail.com')
        //         }
        //     }
        // }
    //     stage('SCA scan') {
    //     steps {
    //         sh 'mkdir -p results/'  
    //         script {  
    //             try {        
    //                     OSV_SCAN_VALUE = sh (
    //                         script: 'osv-scanner scan --lockfile package-lock.json --format json --output results/sca-osv-scanner.json',
    //                         returnStdout: true
    //                         )
    //                     echo 'OsvScan value = ${OSV_SCAN_VALUE}'
    //                 } catch (Exception e) {
    //                     echo 'Exception occurred: ' + e.toString()                        
    //                 }
    //         }
    //         sh 'osv-scanner scan --lockfile package-lock.json --format table'            
    //         cat "${WORKSPACE}/results/sca-osv-scanner.json"
    //     }
    //     // post {
    //     //     always {                   
    //     //         defectDojoPublisher(artifact: '${WORKSPACE}/results/sca-osv-scanner.json', 
    //     //         productName: 'Juice Shop', 
    //     //         scanType: 'OSV Scan', 
    //     //         engagementName: 'szymon.urbanski@gmail.com')
    //     //     }
    //     // }
    // }      
    //     stage('[ZAP] Baseline passive-scan') {
    //         steps {
    //             sh 'mkdir -p results/'
    //             sh '''
    //                 docker run --name juice-shop -d --rm \
    //                     -p 3000:3000 \
    //                     bkimminich/juice-shop
    //                 sleep 5
    //             '''
    //             sh '''
    //                 docker run --name zap \
    //                     --add-host=host.docker.internal:host-gateway \
    //                     -v /mnt/c/projects/tut/abcdevsecops/abcd-student/.zap:/zap/wrk/:rw \
    //                     -t ghcr.io/zaproxy/zaproxy:stable bash -c \
    //                     "zap.sh -cmd -addonupdate; zap.sh -cmd -addoninstall communityScripts -addoninstall pscanrulesAlpha -addoninstall pscanrulesBeta -autorun /zap/wrk/passive.yaml" \
    //                     || true
    //             '''
    //         }
    //         // post {
    //         //     always {
    //         //         sh '''
    //         //             docker cp zap:/zap/wrk/reports/zap_html_report.html ${WORKSPACE}/results/zap_html_report.html
    //         //             docker cp zap:/zap/wrk/reports/zap_xml_report.xml ${WORKSPACE}/results/zap_xml_report.xml
    //         //             docker stop zap juice-shop
    //         //             docker rm zap
    //         //         '''
    //         //         defectDojoPublisher(artifact: '${WORKSPACE}/results/zap_xml_report.xml', 
    //         //         productName: 'Juice Shop', 
    //         //         scanType: 'ZAP Scan', 
    //         //         engagementName: 'szymon.urbanski@gmail.com')
    //         //     }
    //         // }
    //     }     
    }    
}