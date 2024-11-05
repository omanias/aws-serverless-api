# AWS SERVERLESS API

---
This project provides a basic REST API to store information about books in a DynamoDB database using AWS API Gateway and Lambda. The API exposes a single endpoint, which receives data from a book via a POST method and stores it in DynamoDB.

The main elements of the implementation are:

#### AWS API Gateway:
It acts as an entry point for API requests.
#### AWS Lambda:
It processes the requests, extracts the provided data and stores it in DynamoDB.
#### AWS DynamoDB:
NoSQL database that stores the ledger records.

##### This architecture ensures a scalable and serverless system, ideal for handling basic data storage with high availability and low maintenance cost.

### Requirements
* AWS CLI configured.
* Necessary permissions in IAM for API Gateway, Lambda and DynamoDB.
* Node.js or Python to develop the Lambda function.

### Estructura del Proyecto
* API Gateway: Configured to expose a public endpoint (POST) that receives the book data.
* Lambda Function: Function that receives the HTTP request, extracts the book information and stores it in DynamoDB.
* DynamoDB: Table configured to store the book records.

### Endpoint de la API
Method: POST
Path: /api/v1/books
Request Body: JSON with book details (e.g. title, author, year of publication, genre).
Response: confirmation message and success or error status.
Example Request:

POST /api/v1/libros
```json
{
  "title": "Book Example",
  "author": "Author Example",
  "publication_year": 2023,
  "genre": "Fiction"
}
```

### Configuration and Implementation
1) Create a new user from AWS IAM
![alt text](image.png)
![alt text](image-1.png)
![alt text](image-2.png)
![alt text](image-3.png)
![alt text](image-4.png)
![alt text](image-5.png)
![alt text](image-6.png)

2) Create access keys
![alt text](image-7.png)
![alt text](image-8.png)
![alt text](image-9.png)
![alt text](image-10.png)

3) Install VS Code extension
![alt text](image-11.png)
![alt text](image-14.png)

4) Create or modify user credentials
![alt text](image-13.png)

5) Download & install AWS CLI
descargar e instalar el AWS CLI y AWS SAM (Serverless Application Model)
https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-sam-cli.html#install-sam-cli-instructions

6) Start a new project
![alt text](image-12.png)
![alt text](image-15.png)
![alt text](image-16.png)
![alt text](image-17.png)
![alt text](image-18.png)
![alt text](image-19.png)

7) Connect AWS services
![alt text](image-20.png)

Your “api.yml” and “Infrastructure composer” files are synchronized.

8) Clean the src and change the file extension “index.js” to “index.mjs”.
![alt text](image-21.png)

9) Paste the following code in “index.mjs”.

```javascript
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
import { DynamoDBDocumentClient, PutCommand } from "@aws-sdk/lib-dynamodb";

const ddbDocClient = DynamoDBDocumentClient.from(new DynamoDBClient({}));

export const handler = async (event) => {
  try {
    const book = JSON.parse(event.body);

    const newBook = {
      ...book,
      id: book.id ?? crypto.randomUUID(),
    };
    await ddbDocClient.send(
      new PutCommand({
        TableName: process.env.BOOKS_TABLE_NAME,
        Item: newBook,
      })
    );

    return {
      statusCode: 201,
      body: JSON.stringify(newBook),
    };
  } catch (error) {
    console.error(error);
    return {
      statusCode: 500,
      body: JSON.stringify({ message: error.message }),
    };
  }
};
```

10) Synchronize services for deployment
![alt text](image-22.png)

Set time zone parameters
![alt text](image-23.png)

Stack name
![alt text](image-24.png)

S3 bucket name
![alt text](image-25.png)

"OK" to finish
![alt text](image-26.png)

You can create the bucket manually
![alt text](image-28.png)

Successfully created
![alt text](image-29.png)

Infra sync completed
![alt text](image-30.png)

11) Copy query url
![alt text](image-31.png)

From Postman we can prove it
![alt text](image-32.png)

In DynamoDB we can observe the results
![alt text](image-33.png)
