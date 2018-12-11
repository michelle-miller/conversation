---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-29"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:deprecated: .deprecated}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# About the Improve component

The Improve component of {{site.data.keyword.conversationshort}} provides a history of interactions with users of your application. You can use this history to improve your application's understanding of users' inputs.
{: shortdesc}

This version of the {{site.data.keyword.conversationshort}} documentation is deprecated. Go [here](https://console.bluemix.net/docs/services/assistant/logs_intro.html) instead.
{: deprecated}

Sections of the **Improve** panel:

* [Overview](logs_oview.html): A summary of conversations that users had with your application.
* [User conversations](logs_convo.html): A list of individual user messages. You can update intents and entities while viewing an individual user message. **Note**: A single user conversation may consist of multiple messages.
* [Recommendations](logs_recommend.html): Ways to improve your system. Available only to Premium users.

To understand how to use message data to make improvements across applications, it is helpful to review the following definitions associated with the {{site.data.keyword.conversationshort}} service:

* ***Instance***: Your deployment of {{site.data.keyword.conversationshort}}, accessible with unique credentials. A {{site.data.keyword.conversationshort}} instance may be made up of multiple applications.
* ***Application/Bot***: An application - often referred to as a 'bot' - is one model of your {{site.data.keyword.conversationshort}} content.
* ***User***: A user is anyone who interacts with your application; often these are your customers.
* ***User ID***: A unique label that is used to track data for a specific user.
* ***Message***: A message is a single utterance a user sends to the application.
* ***Conversation***: A set of messages consisting of the messages that an individual user sends to your application, and the messages your application responds with.
* ***Conversation ID***: A unique identifier used to link related exchanges. This identifier is sent using the `/message` API, by including the Conversation ID inside the metadata object in your [context ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#message)
* ***Workspace ID***: The unique identifier of an application
* ***Deployment ID***: Unique labels that are passed with user messages to help identify the deployment environment that the messages come from.
* ***Customer ID***: A unique label that can be used to delete data for a specific customer.

**Important**: The **User ID** property is *not* equivalent to the **Customer ID** property, though both may be passed in the `/message` API header. The **User ID** field is used to track levels of usage, whereas the **Customer ID** field is used to support subsequent deletion of messages associated with end users. To understand the difference between the two, as an example, if a customer service representative (CSR) interacts with a {{site.data.keyword.conversationshort}} application in reference to a client, then **Userd ID** and **Customer ID** may both be set, but with different values. In this case, the direct interaction with {{site.data.keyword.conversationshort}} is through the CSR, so you would probably want to set `user_id=CSR`. However, since the CSR is relaying information about their client, you would probably set `customer_id=client`.

## Enabling user metrics
{: #user_id}

User metrics allow you to see, for example, the number of unique users who have engaged with your application/bot, or the average number of conversations per user over a given time interval on the [Overview page](logs_oview.html). User metrics are enabled by using a unique `User ID` parameter.

To specify the `User ID` for a message sent using the `/message` API, include the `user_id` property inside the metadata object in your [context ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#message){: new_window}, as in this example::

```json
"context" : {
  "metadata" : {
       "user_id": "{UserID}"
  }
}
```
{: codeblock}

## Improving across applications
{: #deploy_id}

Creating an application is a very iterative process. While you develop your application, you use the *Try it out* pane to verify that your application recognizes the correct intents and entities in test inputs, and to make corrections as needed.

In the **Improve** panel, you can view information about actual application interactions with your users, and make similar corrections to improve the accuracy with which intents and entities are recognized by your application. It is difficult to know exactly *how* your users will ask questions, or what random messages they may input, so it is important to frequently visit the **Improve** panel, in order to improve your applications.

For a {{site.data.keyword.conversationshort}} instance that includes multiple applications, there may be times when it is useful to use message data from one application to improve another application within that same instance. **Note**: If you are a {{site.data.keyword.conversationshort}} Premium user, your premium instances can optionally be configured to allow access to log data from applications across your different premium instances.

As an example, say you have a {{site.data.keyword.conversationshort}} instance named *HelpDesk*. You may have both a Production application and a Development application in your HelpDesk instance. When working in the Development application, you can filter messages based on the `Deployment ID` for the Production application, so that you are using Production application messages to improve your Development application.

![Data source link](images/data_source_1.png)

Any edits you then make within the Development application will only affect the Development application, even if you’re using Production application messages. See [Selecting a data source](logs_convo.html#selecting-a-data-source) for additional information.

To specify the deployment ID for a message sent using the `/message` API, include the deployment property inside the metadata object in your [context ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#message){: new_window}, as in this example:

```json
"context" : {
  "metadata" : {
       "deployment": "HelpDesk-Production"
  }
}
```
{: codeblock}

## Associating message data with a user for deletion
{: #customer_id}

There might come a time when you want to completely remove a set of your user's data from a {{site.data.keyword.conversationshort}} instance. When the delete feature is used, then the Overview metrics will no longer reflect those deleted messages; for example, they will have fewer Total Conversations.

### Before you begin
To delete messages for one or more individuals, you first need to associate a message with a unique **Customer ID** for each individual. To specify the **Customer ID** for any message sent using the `/message` API, include the `X-Watson-Metadata: customer_id` property in your header. You can pass multiple **Customer ID** entries with semicolon separated `field=value` pairs, using `customer_id`, as in the following example:

```
curl -X POST -u "apikey:3Df... ...Y7Pc9"
 --header
   'Content-Type: application/json'
   'X-Watson-Metadata: customer_id={first-customer-ID};customer_id={second-customer-ID}'
 --data '{"input":{"text":"hello"}}' 'https://gateway-us-south.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/message?version=2018-09-20'
```
{: codeblock}

**Note**: The `customer_id` string cannot include the semicolon (`;`) or equal sign (`=`) characters. You are responsible for ensuring that each `Customer ID` parameter is unique across your customers.

### Querying user data
{: #query_customer_id}

Use the `/logs` API `filter` parameter to search an application log for specific user data. For example, to search for data specific to a `customer_id` that matches `my_best_customer`, the query might be:

``` curl
curl -X GET -u "apikey:3Df... ...Y7Pc9"
'https://gateway-us-south.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/logs?version=2018-09-20&filter=customer_id::my_best_customer'
```
{: codeblock}

See the [Filter query reference](filter-reference.html) for additional details.

### Deleting data
To delete any message log data associated with a specific user that the service might have stored, use the `DELETE /user_data` API endpoint. Specify the customer ID of the user by passing a `customer_id` parameter with the request.

Only data that was added by using the `POST /message` API endpoint with an associated customer ID can be deleted using this delete method. Data that was added by other methods cannot be deleted based on customer ID. For example, entities and intents that were added from customer conversations, cannot be deleted in this way. Personal Data is not supported for those methods.

**IMPORTANT**: Specifying a `customer_id` will delete *all* messages with that `customer_id` that were received before the delete request, across your entire {{site.data.keyword.conversationshort}} instance, not just within one workspace.

As an example, to delete any message data associated with a user that has the customer ID `abc` from your {{site.data.keyword.conversationshort}} instance, send the following cURL command:

``` curl
curl -X DELETE -u "apikey:3Df... ...Y7Pc9"
 'https://gateway-us-south.watsonplatform.net/assistant/api/v1/user_data?customer_id=abc&version=2018-09-20'
```
{: codeblock}

An empty JSON object `{}` is returned.

For more information, see the [API reference](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-user-data).

**Note:** Delete requests are processed in batches and may take up to 24 hours to complete.
