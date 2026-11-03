# CloudPoll: AWS-Based Student Feedback and Polling System

An academic prototype using HTML, CSS, JavaScript, Amazon S3, API Gateway, AWS Lambda, DynamoDB, IAM and CloudWatch.

## Features
- Display available polls
- Submit student responses
- Store responses in DynamoDB
- Retrieve aggregated poll results

## Project structure
```text
CloudPoll/
├── frontend/ (HTML, CSS and JavaScript)
├── backend/
│   ├── getPolls/index.js
│   ├── submitResponse/index.js
│   ├── getResults/index.js
│   └── package.json
├── screenshots/
├── README.md
└── .gitignore
```

## DynamoDB schema
Create table `CloudPoll` with partition key `pollId` (String) and sort key `responseId` (String).

Insert a poll item:
```json
{
  "pollId": "P001",
  "responseId": "POLL",
  "recordType": "POLL",
  "question": "Which programming language do you prefer?",
  "options": ["Python", "Java", "C++", "JavaScript"]
}
```
Poll items use `responseId=POLL`; response items use `responseId=RESP#<unique-id>`.

## AWS deployment summary
1. Create the DynamoDB table and sample poll item.
2. Create three Node.js Lambda functions from the matching backend handler files.
3. Set `TABLE_NAME=CloudPoll` on each function.
4. Package each function separately with the AWS SDK dependencies. Set handler to `index.handler`.
5. Give each Lambda role table-scoped permissions: getPolls: Scan; submitResponse: GetItem and PutItem; getResults: GetItem and Query. Attach basic Lambda logging permissions.
6. Create an API Gateway HTTP API: GET /polls, POST /responses, GET /results, each integrated with its matching Lambda.
7. Configure CORS for the S3 website origin, GET and POST, and Content-Type.
8. Deploy the API and paste its invoke URL into `frontend/config.js`.
9. Upload the contents of `frontend/` to an S3 static website bucket.

## Important
This is an educational prototype. The results endpoint is not authenticated and must not be used for private student data. Restrict CORS and add authentication before real use. Never commit AWS keys or secrets. S3 website endpoints use HTTP; use CloudFront if HTTPS is required.

## Author
Krishiv Taneja  
23BCE0848  
B.Tech Computer Science and Engineering  
4th Year  
Vellore Institute of Technology
