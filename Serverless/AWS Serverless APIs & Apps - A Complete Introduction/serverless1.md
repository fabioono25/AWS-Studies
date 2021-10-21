## AWS Serverless APIs & Apps - A Complete Introduction

The idea of [this introductory course](https://www.udemy.com/course/aws-serverless-a-complete-introduction) is to provide the fundamental serverless concepts in a hands-on mode, presenting some interesting services, like API Gateway, Lambda Functions, DynamoDB, Cognito. Bellow some of the notes:

## API Gateway

API Gateway serves as an entrypoint for the application. It provides way to create a well-defined model for validation, we can configure/transform the input and output, and we can integrate with external services (like Lambda Functions.  It is possible authorize access too, and scale the service, if necessary.

### Some Features
They are some elements presents in the API Gateway service:
![](https://github.com/fabioono25/AWS-Studies/blob/main/Serverless/AWS%20Serverless%20APIs%20%26%20Apps%20-%20A%20Complete%20Introduction/images/api_gateway1.png)

 - **Resources**: they are the paths that will compose part of your endpoint (e.g. https://test.com/compare-yourself).
 - **Methods**: methods are defined under resources and complement the URI with an action of what needs to be done for this endpoint (e.g. GET, POST, DELETE).
 - **Stages**: you can take snapshots of your endpoints and publish into different environments, or stages. Each state will provide their own URI, and it will be possible to define a log, firewall, throttling, SDK, documentation, and other features.
 **Authorizers**: integration with Cognito and Lambda Functions, where you can define access to a specific Cognito User Pool or Lambda Function.
 **Models**: define a shape for input/output data. It helps when you want to add validation for the input.
  **API Keys**: where you can add some keys to allow access for your customers. You can bound to a specific usage plan and stage.

### Method Execution

When you define a method for a resource, it creates a local where you can configure the characteristics and behavior of this specific method:
![](https://github.com/fabioono25/AWS-Studies/blob/main/Serverless/AWS%20Serverless%20APIs%20%26%20Apps%20-%20A%20Complete%20Introduction/images/api_gateway2.png)
You can visualize and manage each step of execution here:

 1. **Client**: it is a representation of the client, where you can test your API, sending parameters or an entire json body, if necessary.
 2. **Method Request**: it works as a *gatekeeper* for your API. Here you can define authorization, add a specific model to validate the required information, define request headers.
 3. **Integration Request**: here is the entry point of integration with external services. They can be from a simple mock, call another HTTP, a different AWS Service or a Lambda Function (as in the example). There is a feature called Mapping Body Templates that it's pretty useful, because you can transform/manipulate the information in way that the Lambda function of this example receives not the entire payload, but just the information it needs (providing the separation of responsibilities between the services involved). Here is an example:
 ![](https://github.com/fabioono25/AWS-Studies/blob/main/Serverless/AWS%20Serverless%20APIs%20%26%20Apps%20-%20A%20Complete%20Introduction/images/api_gateway3.png)
 4. **Lambda**: here is the representation of the external service (in this case,  cy-store-data). Example of the Lambda Function:

```cs
exports.handler = (event, context, callback) => {
    // asdasd
    
    console.log(event);
    console.log('falai Rogerio, tudo bom?');
    
    const age = event.age;
    
    callback(null, event.age * 10);
};  
```

 5. **Integration Response**: based on the pre-defined method response status, it is possible adding the headers (pre-defined as well) and transform/manipulate the response body (same way we did before for integration request). With this approach, we can move out only the needed information.
 6. **Method Response**: in the method response we define a kind of *contract* of what the client can see. Here is the definition of the response status and headers. 
 
 Side note: it is important not forget the CORS configuration for your API. It can be easily configured (for testing) in *Actions -> Enable CORS*.

### Testing using a client

After publishing your API, you can test it via a script, like this example:

```
var xhr = new XMLHttpRequest();
xhr.open('POST', 'https://API_ID.execute-api.API_REGION.amazonaws.com/STAGE/');
xhr.onreadystatechange = function (event) {
  console.log(event.target.response);
}
xhr.setRequestHeader('Content-Type', 'application/json');
xhr.send(JSON.stringify({name: 'John', age: 12}));
```

Back to the [index](https://github.com/fabioono25/AWS-Studies#readme).
