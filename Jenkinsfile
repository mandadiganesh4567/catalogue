@Library('Jenkins-shared-library') _ {
    def configMap [
        PROJECT = "roboshop",
        COMPONENT = "catalogue"
    ]
    if ( ! env.BRANCH_NAME.equalsIgnoreCase('main')){
        nodeJSEKSPipeline(configMap)
    }
}