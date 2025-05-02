pipeline {
    agent any

    environment {
        FUNCTION_NAME = 'hello-world-lambda'
        REGION = 'us-east-1'
        ZIP_FILE = 'lambda.zip'
    }

    stages {
        stage('Prepare') {
            steps {
                script {
                    sh 'zip -r lambda.zip lambda_function.py'
                }
            }
        }

        stage('Deploy Lambda') {
            steps {
                script {
                    sh '''
                        aws lambda create-function \
                        --function-name $FUNCTION_NAME \
                        --runtime python3.12 \
                        --role arn:aws:iam::<your-account-id>:role/<lambda-execution-role> \
                        --handler lambda_function.lambda_handler \
                        --zip-file fileb://$ZIP_FILE \
                        --region $REGION || true

                        aws lambda update-function-code \
                        --function-name $FUNCTION_NAME \
                        --zip-file fileb://$ZIP_FILE \
                        --region $REGION
                    '''
                }
            }
        }

        stage('Setup CloudWatch Schedule') {
            steps {
                script {
                    sh '''
                        aws events put-rule \
                        --schedule-expression "rate(15 minutes)" \
                        --name hello-schedule \
                        --region $REGION

                        aws lambda add-permission \
                        --function-name $FUNCTION_NAME \
                        --statement-id hello-event \
                        --action 'lambda:InvokeFunction' \
                        --principal events.amazonaws.com \
                        --source-arn arn:aws:events:$REGION:<your-account-id>:rule/hello-schedule \
                        --region $REGION || true

                        aws events put-targets \
                        --rule hello-schedule \
                        --targets "Id"="1","Arn"="arn:aws:lambda:$REGION:<your-account-id>:function:$FUNCTION_NAME"
                    '''
                }
            }
        }
    }
}
