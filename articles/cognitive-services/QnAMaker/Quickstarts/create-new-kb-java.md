---
title: "Quickstart: Create knowledge base - REST, Java - QnA Maker"
description: This Java REST-based quickstart walks you through creating a sample QnA Maker knowledge base, programmatically, that will appear in your Azure Dashboard of your Cognitive Services API account.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.date: 12/16/2019
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27, devx-track-java
ms.topic: how-to
---

# Quickstart: Create a knowledge base in QnA Maker using Java

This quickstart walks you through programmatically creating a sample QnA Maker knowledge base. QnA Maker automatically extracts questions and answers from semi-structured content, like FAQs, from [data sources](../index.yml). The model for the knowledge base is defined in the JSON sent in the body of the API request.

This quickstart calls QnA Maker APIs:
* [Create KB](/rest/api/cognitiveservices/qnamaker/knowledgebase/create)
* [Get Operation Details](/rest/api/cognitiveservices/qnamaker/operations/getdetails)

[Reference documentation](/rest/api/cognitiveservices/qnamaker/knowledgebase) | [Java Sample](https://github.com/Azure-Samples/cognitive-services-qnamaker-java/blob/master/documentation-samples/quickstarts/create-knowledge-base/CreateKB.java)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## Prerequisites

* [Go 1.10.1](https://golang.org/dl/)
* You must have a [QnA Maker service](../How-To/set-up-qnamaker-service-azure.md). To retrieve your key and endpoint (which includes the resource name), select **Quickstart** for your resource in the Azure portal.

The [sample code](https://github.com/Azure-Samples/cognitive-services-qnamaker-java/blob/master/documentation-samples/quickstarts/create-knowledge-base/CreateKB.java) is available on the GitHub repo for QnA Maker with Java.

## Create a knowledge base file

Create a file named `CreateKB.java`

## Add the required dependencies

At the top of `CreateKB.java`, add the following lines to add necessary dependencies to the project:

:::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/CreateKB.java" id="dependencies":::

## Add the required constants
After the preceding required dependencies, add the required constants to the `CreateKB` class to access QnA Maker.

You must have a [QnA Maker service](../How-To/set-up-qnamaker-service-azure.md). To retrieve your key and resource name, select **Quickstart** in the Azure portal for your QnA Maker resource.

Set the following values:

* `<your-qna-maker-subscription-key>` - The **key** is a 32 character string and is available in the Azure portal, on the QnA Maker resource, on the Quickstart page. This key is not the same as the prediction endpoint key.
* `<your-resource-name>` - Your **resource name** is used to construct the authoring endpoint URL for authoring, in the format of `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com`. This resource name is not the same as the one used to query the prediction endpoint.

You do not need to add the final curly bracket to end the class; it is in the final code snippet at the end of this quickstart.

:::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/CreateKB.java" id="constants":::


## Add the KB model definition classes
After the constants, add the following classes and functions inside the `CreateKB` class to serialize the model definition object into JSON.

:::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/CreateKB.java" id="model":::

## Add supporting functions

Next, add the following supporting functions inside the `CreateKB` class.

1. Add the following function to print out JSON in a readable format:

    :::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/CreateKB.java" id="pretty":::

2. Add the following class to manage the HTTP response:

    :::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/CreateKB.java" id="response":::

3. Add the following method to make a POST request to the QnA Maker APIs. The `Ocp-Apim-Subscription-Key` is the QnA Maker service key, used for authentication.

    :::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/CreateKB.java" id="post":::

4. Add the following method to make a GET request to the QnA Maker APIs.

    :::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/CreateKB.java" id="get":::

## Add a method to create the KB
Add the following method to create the KB by calling into the Post method.

:::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/CreateKB.java" id="create_kb":::

This API call returns a JSON response that includes the operation ID. Use the operation ID to determine if the KB is successfully created.

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:19:01Z",
  "lastActionTimestamp": "2018-09-26T05:19:01Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "8dfb6a82-ae58-4bcb-95b7-d1239ae25681"
}
```

## Add a method to get status
Add the following method to check the creation status.

:::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/CreateKB.java" id="get_status":::

Repeat the call until success or failure:

```JSON
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:23:08Z",
  "resourceLocation": "/knowledgebases/XXX7892b-10cf-47e2-a3ae-e40683adb714",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

## Add a main method
The main method creates the KB, then polls for the status. The operation ID is returned in the POST response header field **Location**, then used as part of the route in the GET request. The `while` loop retries the status if it is not completed.

:::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/CreateKB.java" id="main":::

## Compile and run the program

1. Make sure the gson library is in the `./libs` directory. At the command line, compile the file `CreateKB.java`:

    ```bash
    javac -cp ".;libs/*" CreateKB.java
    ```

2. Enter the following command at a command line to run the program. It will send the request to the QnA Maker API to create the KB, then it will poll for the results every 30 seconds. Each response is printed to the console window.

    ```bash
    java -cp ",;libs/*" CreateKB
    ```

Once your knowledge base is created, you can view it in your QnA Maker Portal, [My knowledge bases](https://www.qnamaker.ai/Home/MyServices) page.

[!INCLUDE [Clean up files and KB](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## Next steps

> [!div class="nextstepaction"]
> [QnA Maker (V4) REST API Reference](/rest/api/cognitiveservices/qnamaker4.0/knowledgebase)