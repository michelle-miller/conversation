---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:table: .aria-labeledby="caption"}

# Dialog beta features
{: #dialog-overview}

Use this information as you try out beta features of dialog.
{: shortdesc}

**BETA** The features described in this documentation are beta features that have been made available for your evaluation. Beta features might be unstable, might change frequently, and might be discontinued with short notice. Beta features also might not provide the same level of performance or compatibility that generally available features provide and are not intended for use in a production environment.

## Rich responses
{: #multimedia}

You can return responses with multimedia or interactive elements such as images or clickable buttons to simplify the interaction model of your application and enhance the user experience.

In addition to the default response type of **Text**, for which you specify the text to return to the user as a response, the following response types are supported:

- **Connect to human agent**: The dialog calls a service that you designate, typically a service that manages human agent support ticket queues, to pass off the conversation to a person. You can optionally include a message that sends information that was collected by the dialog up to that point to the human agent. It is the responsibility of the external service to display a message telling the user that the conversation is being transferred. The dialog does not manage that communication. The dialog transfer does not occur when you are testing nodes with this response type in the "Try it out" pane. You must access a node that uses this response type from a test deployment to see how your users will experience it.
- **Image**: Embeds an image into the response. The source image file must be hosted somewhere and have a URL that you can use to reference it.
- **Option**: Adds a list of one or more options. When a user clicks one of the options, an associated user input value is sent to the service. How options are rendered can differ depending on where you deploy the dialog. For example, in one integration channel the options might be displayed as clickable buttons, but in another they might be displayed as a list of links.

- **Pause**: Forces the application to wait for a specified number of milliseconds before continuing with processing. You can choose to show an indicator that the dialog is working on typing a response. Use this response type if you need to perform an action that might take some time. For example, a parent node makes a Cloud Function call and displays the result in a child node. You could use this response type as the response for the parent node to give the programmatic call time to complete, and then jump to the child node to show the result. This response type does not render in the "Try it out" pane. You must access a node that uses this response type from a test deployment to see how your users will experience it.

### Deploying dialogs with rich responses
{: #deploying-multimedia}

If you are integrating the dialog with Slack or Facebook Messenger by using the {{site.data.keyword.conversationshort}} connector, these rich responses are supported automatically. However, there might be slight differences in how the resulting responses are displayed in each channel. Always test your dialog from the deployment channels you plan to support to make sure the responses behave as you expect them to.

If you are building a custom application, you need to know when a rich response type is returned so you can render it properly in your client application. This is the format of the the API /message response that is returned for each response type:

- **Image**

  ```json
  {
    "response_type": "image",
    "source": "http://www.example.com/images/house.jpg"
  }
  ```

- **Option**

  ```json
  {
    "response_type": "option",
    "options": [
      {
        "label": "Car",
        "value": { 
           "input" : { "text" : "car insurance"}
        }
      },
      {
        "label": "Home",
        "value": { 
           "input" : { "text" : "home insurance"}
        }
      },
      {
        "label": "Life",
        "value": { 
           "input" : { "text" : "life insurance"}
        }
      }
    ]
  }
  ```

- **Pause**

  ```json
  {
    "response_type": "pause",
    "time": 8000,
    "typing": true
  }
  ```

- **Text**

  ```json
  {
    "response_type": "text",
    "text": "We can find the optimal insurance plan for you."
  }
  ```

### Adding rich responses
{: #add-multimedia}

To add a rich response, complete the following steps:

1.  Click the drop-down menu in the response field to choose a response type, and then provide any required information:

    - **Connect to agent**. Optionally add a message for the human agent to whom the conversation is transferred.

        **Note**: This response type only works properly at runtime if you configure a human agent service to work with {{site.data.keyword.conversationshort}}.
    - **Image**. Add the full URL to the hosted image file into the **Image source** field. The image must be in .jpg, .gif, or .png format. If you want to display an image title and description above the embedded image in the response, then add them in the fields provided. DOES DESCRIPTION GET USED AS ALT-TEXT VALUE?
    - **Option**. Complete the following steps:

      1.  Click **Add option**.
      1.  In the **List label** field, enter the option to display in the list. In the corresponding **Value** field, enter the user input to pass to the service when this option is selected. The value can be either a string (for simple responses) or an object containing multiple fields.
      1.  Repeat the previous steps to add more options to the list.
      1.  Optionally, add a list introduction in the **Title** field and additional information in the **Description** field. These values are displayed above the option list.

    - **Pause**. Add the length of time for the pause to last as a number of milliseconds (ms) to the **Duration** field. The value cannot exceed 10,000 ms. Users are typically willing to wait about 8 seconds (8,000 ms) for someone to enter a response. To prevent a typing indicator from being displayed during the pause, choose **Off**.
    - **Text**. Add the text to return to the user in the text field.

1.  Click **Add response** to add another response type to the current response.

    You might want to add more than one response type if you want to warn users about an upcoming pause. You could display a text response type with the warning and then the pause response type.

    Or you might want to return a text response that displays on two separate lines, so effectively includes a carriage return. To do so, add more than one text response type to the response. The text you specify for each text response will be displayed on a separate line, one after another, in the order in which you listed them.

1.  If you added more than one response type, click the **Move** up or down arrows to arrange the response types in the order you want the service to process them.
