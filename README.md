# Project Carnation: A Demo Step Function with Map State and Error Retry

This is a sample project to use map state in a state machine to loop through a list of events and invoke a Lambda to process tham and store the erros events in a SQS queue. In the subsequest run the Lambda receives the error events from the SQS to retry the processing.

## Description

This sample state machine consists of two Lambdas, one SQS, one SNS and CloudWatch alarms for Lambda. The first Lambda generates some sample data with sequence number, random strings and generated Unix time stamp. The map state loops through the list generated by the first Lambda and processes them by inserting the records into a DynamoDB table. If any insertion fails then they are collected in a list and pushed into a SQS queue and the state machine fails. If all the generated data gets processed successfully the state machine ends in a success state. In the subsequent run of the state machine, the first lambda pulls the error events from the SQS (if any) and reprocesses them.

![Project Carnation - Design Diagram](https://blog.subhamay.com/wp-content/uploads/2023/04/63_Carnation_1_1_Architecture_Diagram-1024x567.png)

![Project Carnation - Services Used](https://blog.subhamay.com/wp-content/uploads/2023/04/63_Carnation_1_2_Services_Used-1024x568.png)

![Project Carnation - State Machine](https://blog.subhamay.com/wp-content/uploads/2023/04/63_Carnation_1_3_State_Machine-1024x942.png)

## Getting Started

### Dependencies

* Create a Customer Managed KMS Key in the region where you want to create the stack.
* Modify the KMS Key Policy to let the IAM user encrypt / decrypt using any resource using the created KMS Key.

### Installing

* Clone the repository.
* Create a S3 bucket to store the CFT and python code zip file.
* Create the folders - carnation/cft/nested-stacks, carnation/code/state-machine, carnation/code/python
* Upload the following YAML templates to carnation/cft/nested-stacks/
    * cloudwatch-stack.yaml
    * dynamodb-stack.yaml
    * lambda-function-stack.yaml
    * sqs-stack.yaml
    * iam-role-stack.yaml
    * sns-stack.yaml
* Upload the following YAML templates to carnation/cft/
    * carnation-root-stack.yaml
* Upload state machine ASL to the folder cft/state-machine/
* Zip and Upload the Python files  to carnation/code/python
* Create the entire using by using the root stack template carnation-root-stack.yaml by providing the required parameters.

### Executing program

* Follow the blog to execute the state machine.

## Help

Post message in my blog (https://subhamay.blog)


## Authors

Contributors names and contact info

Subhamay Bhattacharyya  - [subhamoyb@yahoo.com](https://subhamay.blog)

## Version History

* 0.2
    * Various bug fixes and optimizations
    * See [commit change]() or See [release history]()
* 0.1
    * Initial Release

## License

This project is licensed under Subhamay Bhattacharyya. All Rights Reserved.

## Acknowledgments

Inspiration, code snippets, etc.
* [Stephane Maarek ](https://www.linkedin.com/in/stephanemaarek/)
* [Neal Davis](https://www.linkedin.com/in/nealkdavis/)
* [Adrian Cantrill](https://www.linkedin.com/in/adriancantrill/)
