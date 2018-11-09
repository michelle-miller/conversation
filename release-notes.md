---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-08"
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

# Release notes

## Service API Versioning
{: shortdesc}

API requests require a version parameter that takes a date in the format `version=YYYY-MM-DD`. Whenever we change the API in a backwards-incompatible way, we release a new minor version of the API.

Send the version parameter with every API request. The service uses the API version for the date you specify, or the most recent version before that date. Don't default to the current date. Instead, specify a date that matches a version that is compatible with your app, and don't change it until your app is ready for a later version.

The current version is `2018-09-20`.

The "Try it out" pane in the {{site.data.keyword.conversationshort}} tooling is using version `2018-07-10`.

## Beta features

IBM releases services, features, and language support for your evaluation that are classified as beta. These features might be unstable, might change frequently, and might be discontinued with short notice. Beta features also might not provide the same level of performance or compatibility that generally available features provide and are not intended for use in a production environment. Beta features are supported only on the [IBM Developer Answers ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/topics/watson-conversation/){: new_window}.

## Updated models
{: #updated-models}

The {{site.data.keyword.conversationshort}} algorithms may be periodically refined and updated based on feedback, scientific enhancements, and additional factors, in order to continuously enhance the performance. Updates to the model will be communicated in these release notes.

Existing models that you have trained will not be immediately impacted, but expired models will be updated to the current model, if you have not already done so, after 60 days of a new model becoming available.

**Note:** This updating statement applies to Generally Available (GA) languages and features only.

## Changes
{: #change-log}

The following new features and changes to the service are available.

### 8 November 2018
{: #7November2018}

- **Japanese data center**: You can now create {{site.data.keyword.conversationshort}} service instances that are hosted in the Tokyo data center. See [Data centers](services-information.html#regions) for more details.

### 30 October 2018
{: #30October2018}

- **New API authentication process**: The {{site.data.keyword.conversationshort}} service transitioned from using Cloud Foundry to using token-based Identity and Access Management (IAM) authentication in the following regions:

  - Dallas (us-south)
  - Frankfurt (eu-de)

  For new service instances, you use IAM for authentication. You can pass either a bearer token or an API key. Tokens support authenticated requests without embedding service credentials in every call. API keys use basic authentication.

  For all existing service instances, you continue to use service credentials (`{username}:{password}`) for authentication.

  See [Authenticating API calls](services-information.html#authenticate-api-calls) for more information.

### 25 October 2018
{: #25October2018}

- **Entity synonym recommendations are available in more languages**: Synonym recommendation support was added for the French, Japanese, and Spanish languages.

### 26 September 2018
{: #26September2018}

- **{{site.data.keyword.conversationfull}} is available in {{site.data.keyword.icpfull}}**: See the [{{site.data.keyword.icpfull}} documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.3/featured_applications/watson_assistant.html) for more information.

### 21 September 2018
{: #21September2018}

- **New API version**: The current API version is now `2018-09-20`. In this version, the `errors[].path` attribute of the error object that is returned by the API is expressed as a [JSON Pointer ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://tools.ietf.org/html/rfc6901) instead of in dot notation form.
- **Web actions support**: You can now call {{site.data.keyword.openwhisk_short}} web actions from a dialog node. See [Making programmatic calls from a dialog node](dialog-actions.html) for more details.

### 15 August 2018
{: #15August2018}

- **Entity fuzzy matching support improvements**: Fuzzy matching is fully supported for English entities, and the misspelling feature is no longer a Beta-only feature for many other languages. See [Supported languages](lang-support.html) for details.

### 6 August 2018
{: #6August2018}

- **Intent conflict resolution ![Premium plan only](images/premium0.png)**: The tool can now help you to resolve conflicts when two or more user examples in separate intents are similar to one another. Non-distinct user examples can weaken the training data and make it harder for the service to map user input to the appropriate intent at run time. See [Resolving intent conflicts](intents.html#conflict-intents) for details.

- **Disambiguation** ![Premium plan only](images/premium0.png): Enable disambiguation to allow your assistant to ask the user for help when it needs to decide between two or more viable dialog nodes to process for a response. See [Disambiguation](dialog-runtime.html#disambiguation) for more details.

- **Jump-to fix**: Fixed a bug in the Dialogs tool which prevented you from being able to configure a jump-to that targets the response of a node with the `anything_else` special condition.

- **Digression return message**: You can now specify text to display when the user returns to a node after a digression. The user will have seen the prompt for the node already. You can change the message slightly to let users know they are returning to where they left off. For example, specify a response like, `Where were we? Oh, yes...` See [Digressions](dialog-runtime.html#digressions) for more details.

### 12 July 2018
{: #12July2018}

- **Rich response types**: You can now add rich responses that include elements such as images or buttons in addition to text, to your dialog. See [Rich responses](dialog-overview.html#multimedia) for more information.

- **Contextual entities (Beta)**: Contextual entities are entities that you define by labeling mentions of the entity type that occur in intent user examples. These entity types teach the service not only terms of interest, but also the context in which terms of interest typically appear in user utterances, enabling the service to recognize never-seen-before entity mentions based solely on how they are referenced in user input. For example, if you annotate the intent user example, "I want a flight to Boston" by labeling "Boston" as a @destination entity, then the service can recognize "Chicago" as a @destination in a user input that says, "I want a flight to Chicago." This feature is currently available for English only. See [Defining contextual entities](entities.html#defining-contextual-entities) for more information.

  **Note**: When you access the tool with an Internet Explorer web browser, you cannot label entity mentions in intent user examples nor edit user example text.

- **Entity recommendations**: The service can now recommend synonyms for your entity values. The recommender finds related synonyms based on contextual similarity extracted from a vast body of existing information, including large sources of written text, and uses natural language processing techniques to identify words similar to the existing synonyms in your entity value. For more information see [Synonyms](entities.html#synonyms).

- **New API version**: The current API version is now `2018-07-10`. This version introduces the following changes:

  - The content of the /message `output` object changed from being a `text` JSON object to being a `generic` array that supports multiple rich response types, including `image`, `option`, `pause`, and `text`.
  - Support for contextual entities was added.

- **Overview page date filter**: Use the new date filters to choose the period for which data is displayed. These filters affect all data shown on the page: not just the number of conversations displayed in the graph, but also the statistics displayed along with the graph, and the lists of top intents and entities. See [Controls](logs_oview.html#controls) for more information.

- **Pattern limit expanded**: When using the **Patterns** field to [define specific patterns for an entity value](entities.html#patterns), the pattern (regular expression) is now limited to 512 characters.

### 2 July 2018
{: #2July2018}

- **Jump-tos from conditional responses**: You can now configure a conditional response to jump directly to another node. See [Conditional responses](dialog-overview.html#multiple) for more details.

### 21 June 2018
{: #21June2018}

- **Language updates for system entities**: Dutch and Simplified Chinese language support are now generally available. Dutch language support includes fuzzy matching for misspellings. Traditional Chinese language support includes the availability of [system entities](system-entities.html) in beta release. See [Supported languages](lang-support.html) for details.

### 14 June 2018
{: #14June2018}

- **Washington, DC data center opens**: You can now create {{site.data.keyword.conversationshort}} service instances that are hosted in the Washington, DC data center. See [Data centers](services-information.html#regions) for more details.

- **New API authentication process**: The {{site.data.keyword.conversationshort}} service has a new API authentication process for service instances that are hosted in the following regions:

  - Washington, DC (us-east) as of 14 June 2018
  - Sydney, Australia (au-syd) as of 7 May 2018

  {{site.data.keyword.cloud_notm}} is migrating to token-based Identity and Access Management (IAM) authentication.

  For new service instances in the regions listed above, you use IAM for authentication. You can pass either a bearer token or an API key. Tokens support authenticated requests without embedding service credentials in every call. API keys use basic authentication.

  For all new and existing service instances in other regions, you continue to use service credentials (`{username}:{password}`) for authentication.

  When you use any of the Watson SDKs, you can pass the API key and let the SDK manage the lifecycle of the tokens. For more information and examples, see [Authentication ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#authentication){: new_window} in the API reference.

  If you are not sure which type of authentication to use, view the service credentials by clicking the service instance on the [Dashboard ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/dashboard/apps?category=ai){: new_window}.

### 25 May 2018
{: #25May2018}

- **New sample workspace**: The sample workspace that is provided for you to explore or to use as a starting point for your own workspace has changed. The **Car Dashboard** sample was replaced by a **Customer Service** sample. The new sample showcases how to use content catalog intents and other newer features to build a bot. It can answer common questions, such as inquiries about store hours and locations, and illustrates how to use a node with slots to schedule in-store appointments.

- **HTML rendering was added to Try it out**: The "Try it out" pane now renders HTML formatting that is included in response text. Previously, if you included a hypertext link as an HTML anchor tag in a text response, you would see the HTML source in the "Try it out" pane during testing. It used to look like this:

  `Contact us at <a href="https://www.ibm.com">ibm.com</a>.`

  Now, the hypertext link is rendered as if on a web page. It is displayed like this:

  `Contact us at` [ibm.com](https://www.ibm.com){: new_window}.

    **Note**: As before, you must use the appropriate type of syntax in your responses for the client application to which you will deploy the conversation. Only use HTML syntax if your client application can interpret it properly. Other integration channels might expect other formats.

- **Deployment changes**: The **Test in Slack** option was removed.

### 11 May 2018
{: #11May2018}

- **Information security**: The documentation includes some new details about data privacy. Read more in [Information security](information-security.html).

### 7 May 2018
{: #7May2018}

- **Sydney, Australia data center opens**: You can now create {{site.data.keyword.conversationshort}} service instances that are hosted in the Sydney, Australia data center. See [IBM Cloud global data centers ![External link icon](../../icons/launch-glyph.svg "External link icon"](https://www.ibm.com/cloud/data-centers/) for more details.

### 4 April 2018
{: #4April2018}

- **Search dialogs**: You can now [search dialog nodes](dialog-build.html#search) for a given word or phrase.

### 15 March 2018
{: #15March2018}

- **Introducing {{site.data.keyword.conversationfull}}**: {{site.data.keyword.ibmwatson}} Conversation has been renamed. It is now called {{site.data.keyword.conversationfull}}. The name change reflects the fact that the service is expanding to provide prebuilt content and tools that help you more easily share the virtual assistants you build. Read [this blog post ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/watson/2018/03/the-future-of-watson-conversation-watson-assistant/) for more details.

- **New REST APIs and SDKs are available for Watson Assistant**: The new APIs are functionally identical to the existing Conversation APIs, which continue to be supported. For more information about the Watson Assistant APIs, see the [API Reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/assistant/api/v1/){: new_window}.

- **Dialog enhancements**: The following features were added to the dialog tool:

  - Simple variable name and value fields are now available that you can use to add context variables or update context variable values. You do not need to open the JSON editor unless you want to. See [Defining a context variable](dialog-runtime.html#context-var-define) for more details.
  - Organize your dialog by using folders to group together related dialog nodes. See [Organizing the dialog with folders](dialog-build.html#folders) for more details.
  - Support was added for customizing how each dialog node participates in user-initiated digressions away from the designated dialog flow. See [Digressions](dialog-runtime.html#digressions) for more details.

- **Search intents and entities**: A new search feature has been added that allows you to [search intents](intents.html#searching-intents) for user examples, intent names, or descriptions, or to [search entity](entities.html#searching-entities) values and synonyms.

- **Content catalogs**: The new [content catalogs](catalog.html#using-content-catalogs) contain a single category of prebuilt common intents and entities that you can add to your application. For example, most applications require a general #greeting-type intent that starts a dialog with the user. You can add it from the content catalog rather than building your own.

- **Enhanced user metrics**: The [Improve component](logs.html#about-the-improve-component) has been enhanced with additional user metrics and logging statistics. For example, the [Overview page](logs_oview.html#the-overview-page) includes several new, detailed graphs that summarize interactions between users and your application, the amount of traffic for a given time period, and the intents and entities that were recognized most often in user conversations.

### 12 March 2018
{: #12March2018}

- **New date and time methods**: Methods were added that make it easier to perform date calculations from the dialog. See [Date and time calculations](dialog-methods.html#date-and-time-calculations) for more details.

### 16 February 2018
{: #16February2018}

- **Dialog node tracing**: When you use the "Try it out" pane to test a dialog, a location icon is displayed next to each response. You can click the icon to highlight the path that the service traversed through the dialog tree to arrive at the response. See [Building a dialog](dialog-build.html#test) for details.

- **New API version**: The current API version is now `2018-02-16`. This version introduces the following changes:

  - A new `include_audit` parameter is now supported on most GET requests. This is an optional boolean parameter that specifies whether the response should include the audit properties (`created` and `updated` timestamps). The default value is `false`. (If you are using an API version earlier than `2018-02-16`, the default value is `true`.) For more information, see the [API Reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.

  - Responses from API calls using the new version include only properties with non-`null` values.

  - The `output.nodes_visited` and `output.nodes_visited_details` properties of message responses now include nodes with the following types, which were previously omitted:

    - Nodes with `type`=`response_condition`
    - Nodes with `type`=`event_handler` and `event_name`=`input`

### 9 February 2018
{: #9February2018}

- **Dutch system entities (Beta)**: Dutch language support has been enhanced to include the availability of [System entities](system-entities.html) in beta release. See [Supported languages](lang-support.html) for details.

### 29 January 2018
{: #29January2018}

- The {{site.data.keyword.conversationshort}} REST API now supports new request parameters:
  - Use the `append` parameter when updating a workspace to indicate whether the new workspace data should be added to the existing data, rather than replacing it. For more information, see [Update workspace ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#update_workspace){: new_window}.
  - Use the `nodes_visited_details` parameter when sending a message to indicate whether the response should include additional diagnostic information about the nodes that were visited during processing of the message. For more information, see [Send message ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#send_message){: new_window}.

### 23 January 2018
{: #23January2018}

- **Unable to retrieve list of workspaces**: If you see this or similar error messages when working in the tooling, it might mean that your session has expired. Log out by choosing **Log out** from the **User information** icon ![User information icon menu](images/user-icon.png), and then log back in.

### 8 December 2017
{: #8December2017}

- **Log data access across instances (Premium users only)**: If you are a {{site.data.keyword.conversationshort}} Premium user, your premium instances can optionally be configured to allow access to log data from workspaces across your different premium instances.

- **Copy nodes**: You can now duplicate a node to make a copy of it and its children. This feature is helpful if you build a node with useful logic that you want to reuse elsewhere in your dialog. See [Copying a dialog node](dialog-build.html#copy-node) for more information.

- **Capture groups in pattern entities**: You can identify groups in the regular expression pattern that you define for an entity. Identifying groups is useful if you want to be able to refer to a subsection of the pattern later. For example, your entity might have a regex pattern that captures US phone numbers. If you identify the area code segment of the number pattern as a group, then you can subsequently refer to that group to access just the area code segment of a phone number. See [Defining entities](entities.html#creating-entities) for more information.

### 6 December 2017
{: #6December2017}

- **{{site.data.keyword.openwhisk}} integration (Beta)**: Call {{site.data.keyword.openwhisk}} (formerly IBM OpenWhisk) actions directly from a dialog node. This feature enables you to, for example, call an action to retrieve weather information from within a dialog node, and then condition on the returned information in the dialog response. Currently, you can call an action from a {{site.data.keyword.openwhisk_short}} instance that is hosted in the US South region from {{site.data.keyword.conversationshort}} instances that are hosted in the US South region. See [Making programmatic calls from a dialog node](dialog-actions.html) for more details.

### 5 December 2017
{: #5December2017}

- **Redesigned UI for Intents and Entities**: The `Intents` and `Entities` tabs have been redesigned to provide an easier, more efficient workflow when creating and editing entities and intents. See [Defining intents](intents.html#defining-intents) and [Defining entities](entities.html#defining-entities) for information about working with these tabs.

### 30 November 2017
{: #30November2017}

- **Eastern Arabic numeral support**: Eastern Arabic numerals are now supported in Arabic system entities.

### 29 November 2017
{: #29November2017}

- **Improving understanding of user input across workspaces**: You can now improve a workspace with utterances that were sent to other workspaces within your instance. For example, you might have multiple versions of production workspaces and development workspaces; you can use the same utterance data to improve any of these workspaces. See [Improving across workspaces](logs.html#deploy_id) and [Selecting a data source](logs_convo.html#selecting-a-data-source) for additional information.

### 20 November 2017
{: #20November2017}

- **GB18030 compliance**: GB18030 is a Chinese standard that specifies an extended code page for use in the Chinese market. This code page standard is important for the software industry because the China National Information Technology Standardization Technical Committee has mandated that any software application that is released for the Chinese market after September 1, 2001, be enabled for GB18030. The {{site.data.keyword.conversationshort}} service supports this encoding, and is certified GB18030-compliant.

### 9 November 2017
{: #9November2017}

- **Intent examples can directly reference entities**: You can now specify an entity reference directly in an intent example. That entity reference, along with all its values or synonyms, is used by the {{site.data.keyword.conversationshort}} service classifier for training the intent. For more information, see [*Entity as example*](intents.html#entity-as-example) in the [Intents](intents.html) topic.

  **Note**: Currently, you can only directly reference closed entities that you define. You cannot directly reference [pattern entities](entities.html#pattern-entities) or [system entities](system-entities.html).

### 8 November 2017
{: #8November2017}

- **{{site.data.keyword.conversationshort}} connector**: You can use the new {{site.data.keyword.conversationshort}} connector tool to connect your workspace to a Slack or Facebook Messenger app that you own, making it available as a chat bot that Slack or Facebook Messenger users can interact with. This tool is available only for the {{site.data.keyword.Bluemix_notm}} US South region. For more information, see [Deploying to a channel with the {{site.data.keyword.conversationshort}} connector](conversation-connector.html).

### 3 November 2017
{: #3November2017}

- **Dialog updates**: The following updates make is easier for you to build a dialog. (See [Building a dialog](dialog-build.html) for details.)

    - You can add a condition to a slot to make it required only if certain conditions are met. For example, you can make a slot that asks for the name of a spouse required only if a previous (required) slot that asks for marital status indicates that the user is married.

    - You can now choose **Skip user input** as the next step for a node. When you choose this option, after processing the current node, the service jumps directly to the first child node of the current node. This option is similar to the existing *Jump to* next step option, except that it allows for more flexibility. You do not need to specify the exact node to jump to. At run time, the service always jumps to whichever node is the first child node, even if the child nodes are reordered or new nodes are added after the next step behavior is defined.

    - You can add conditional responses for slots. For both Found and Not found responses, you can customize how the service responds based on whether certain conditions are met. This feature enables you to check for possible misinterpretations and correct them before saving the value provided by the user in the slot's context variable. For example, if the slot saves the user's age, and uses `@sys-number` in the *Check for* field to capture it, you can add a condition that checks for numbers over 100, and responds with something like, *Please provide a valid age in years.* See [Adding conditions to Found and Not found responses](dialog-slots.html#slot-handler-next-steps) for more details.

    - The interface you use to add conditional responses to a node has been redesigned to make it easier to list each condition and its response. To add node-level conditional responses, click **Customize**, and then enable the **Multiple responses** option.

     **Note**: The **Multiple responses** toggle sets the feature on or off for the node-level response only. It does not control the ability to define conditional responses for a slot. The slot multiple response setting is controlled separately.

    - To keep the page where you edit a slot simple, you now select menu options to a.) add a condition that must be met for the slot to be processed, and b.) add conditional responses for the Found and Not found conditions for a slot. Unless you choose to add this extra functionality, the slot condition and multiple responses fields are not displayed, which declutters the page and makes it easier to use.

### 25 October 2017
{: #25October2017}

- **Updates to Simplified Chinese**: Language support has been enhanced for Simplified Chinese. This includes intent classification improvements using character-level word embeddings, and the availability of system entities. Note that the {{site.data.keyword.conversationshort}} service learning models may have been updated as part of this enhancement, and when you retrain your model any changes will be applied; see [Updated models](release-notes.html#updated-models) for more information.
- **Updates to Spanish** - Improvements have been made to Spanish intent classification, for very large datasets.

### 11 October 2017
{: #11October2017}

- **Updates to Korean**: Language support has been enhanced for Korean. Note that the {{site.data.keyword.conversationshort}} service learning models may have been updated as part of this enhancement, and when you retrain your model any changes will be applied; see [Updated models](release-notes.html#updated-models) for more information.

### 3 October 2017
{: #3October2017}

- **Pattern-defined entities (Beta)**: You can now define specific patterns for an entity, using regular expressions. This can help you identify entities that follow a defined pattern, for example SKU or part numbers, phone numbers, or email addresses. See [Pattern-defined entities](entities.html#pattern-entities) for additional details.
    - You can add either synonyms or patterns for a single entity value; you cannot add both.
    - For each entity value, there can be a maximum of up to 5 patterns.
    - Each pattern (regular expression) is limited to 128 characters.
    - Importing or exporting via a CSV file does not currently support patterns.
    - The REST API does not support direct access to patterns, but you can retrieve or modify patterns using the `/values` endpoint.

- **Fuzzy matching filtered by dictionary (English only)**: An improved version of fuzzy matching for entities is now available, for English. This improvement prevents the capturing of some common, valid English words as fuzzy matches for a given entity. For example, fuzzy matching will not match the entity value `like` to `hike` or `bike`, which are valid English words, but will continue to match examples such as `lkie` or `oike`.

### 27 September 2017
{: #27September2017}

- **Condition builder updates**: The control that is displayed to help you define a condition in a dialog node has been updated. Enhancements include support for listing available context variable names after you enter the $ to begin adding a context variable.

### 31 August 2017
{: #31August2017}

- **Improve section rollback**: The median conversation time metric, and corresponding filters, are being temporarily removed from the Overview page of the Improve section. This removal will prevent the calculation of certain metrics from causing the median conversation time metric, and the conversations over time graph, to display inaccurate information. IBM regrets removing functionality from the tool, but is committed to ensuring that we are communicating accurate information to users.
- **Dialog node names**: You can now assign any name to a dialog node; it does not need to be unique. And you can subsequently change the node name without impacting how the node is referenced internally. The name you specify is saved as a title attribute of the node in the workspace JSON file and the system uses a unique ID that is stored in the name attribute to reference the node.

### 23 August 2017
{: #23August2017}

- **Updates to Korean, Japanese, and Italian**: Language support has been enhanced for Korean, Japanese, and Italian. Note that the {{site.data.keyword.conversationshort}} service learning models may have been updated as part of this enhancement, and when you retrain your model any changes will be applied; see [Updated models](release-notes.html#updated-models) for more information.

### 10 August 2017
{: #10August2017}

- **Accent normalization**: In a conversational setting, users may or may not use accents while interacting with the {{site.data.keyword.conversationshort}} service. As such, an update has been made to the algorithm so that accented and non-accented versions of words are treated the same for intent detection and entity recognition.

  However for some languages, like Spanish, some accents can alter the meaning of the entity. Thus, for entity detection, although the original entity may implicitly have an accent, the service can also match the non-accented version of the same entity, but with a slightly lower confidence score.

  For example, for the word `barrió`, which has an accent and corresponds to the past tense of the verb `barrer` (to sweep), the service can also match the word `barrio` (neighborhood), but with a slightly lower confidence.

  The system will provide the highest confidence scores in entities with exact matches. For example, `barrio` will not be detected if `barrió` is in the training set; and `barrió` will not be detected if `barrio` is in the training set.

  You are expected to train the system with the proper characters and accents. For example, if you are expecting `barrió` as a response, then you should put `barrió` into the training set.

  Although not an accent mark, the same applies to words using, for example, the Spanish letter `ñ` vs. the letter `n`, such as `uña` vs. `una`. In this case the letter `ñ` is not simply an `n` with an accent; it is a unique, Spanish-specific letter.

  You can enable fuzzy matching if you think your customers will not use the appropriate accents, or misspell words (including, for example, putting a `n` instead of a `ñ`), or you can explicitly include them in the training examples.

  **Note:** Accent normalization is enabled for Portuguese, Spanish, French, and Czech.

- **Workspace opt-out flag**: The {{site.data.keyword.conversationshort}} REST API now supports an opt-out flag for workspaces. This flag indicates that workspace training data such as intents and entities are not to be used by IBM for general service improvements. For more information, see the [API Reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#data-collection){: new_window}

### 7 August 2017
{: #7August2017}

- **`Next` and `last` date interpretation**: The {{site.data.keyword.conversationshort}} service treats `last` and `next` dates as referring to the most immediate last or next day referenced, which may be in either the same or a previous week. See the [system entities](system-entities.html#sys-datetime) topic for additional information.

### 3 August 2017
{: #3August2017}

- **Fuzzy matching for additional languages (Beta)**: Fuzzy matching for entities is now available for additional languages, as noted in the [Supported languages](lang-support.html) topic.
- **Partial match (Beta - English only)**: Fuzzy matching will now automatically suggest substring-based synonyms present in user-defined entities, and assign a lower confidence score as compared to the exact entity match. See [Fuzzy matching](entities.html#fuzzy-matching) for details.

### 28 July 2017
{: #28July2017}

- When you set bidirectional preferences for the tooling, you can now specify the graphical user interface direction.
- The color scheme of the tooling was updated to be consistent with other Watson services and products.

### 19 July 2017
{: #19July2017}

- The {{site.data.keyword.conversationshort}} REST API now supports access to dialog nodes. For more information, see the [API Reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#dialog_nodes){: new_window}.

### 14 July 2017
{: #14July2017}

- The slots functionality of dialogs was enhanced. For example, a *slot_in_focus* property was added that you can use to define a condition that applies to a single slot only. See [Gathering information with slots](/docs/services/conversation/dialog-slots.html) for details.

### 12 July 2017
{: #12July2017}

- **Support for Czech**: Czech language support has been introduced; please see the [Supported languages](lang-support.html) topic for additional details.

### 11 July 2017
{: #11July2017}

- **Test in Slack**: You can use the new **Test in Slack** tool to quickly deploy your workspace as a Slack bot user for testing purposes. This tool is available only for the {{site.data.keyword.Bluemix_notm}} US South region. For more information, see [Testing in Slack](test-deploy.html).
- **Updates to Arabic**: Arabic language support has been enhanced to include absolute scoring per intent, and the ability to mark intents as irrelevant; please see the [Supported languages](lang-support.html) topic for additional details. Note that the {{site.data.keyword.conversationshort}} service learning models may have been updated as part of this enhancement, and when you retrain your model any changes will be applied; see [Updated models](release-notes.html#updated-models) for more information.

### 23 June 2017
{: #23June2017}

- **Updates to Korean**: Korean language support has been enhanced; please see the [Supported languages](lang-support.html) topic for additional details. Note that the {{site.data.keyword.conversationshort}} service learning models may have been updated as part of this enhancement, and when you retrain your model any changes will be applied; see [Updated models](release-notes.html#updated-models) for more information.

### 22 June 2017
{: #22June2017}

- **Introducing slots**: It is now easier to collect multiple pieces of information from a user in a single node by adding slots. Previously, you had to create several dialog nodes to cover all the possible combinations of ways that users might provide the information. With slots, you can configure a single node that saves any information that the user provides, and prompts for any required details that the user does not. See [Gathering information with slots](dialog-slots.html) for more details.
- **Simplified dialog tree** - The dialog tree has been redesigned to improve its usability. The tree view is more compact so it is easier to see where you are within it. And the links between nodes are represented in a way that makes it easier to understand the relationships between the nodes.

### 21 June 2017
{: #21June2017}

- **Arabic support**: Language support for Arabic is now generally available. For details, see [Configuring bi-directional languages](lang-support.html#configuring-bi-directional).
- **Language updates**: The {{site.data.keyword.conversationshort}} service algorithms have been updated to improve overall language support. See the [Supported languages](lang-support.html) topic for details.

### 16 June 2017
{: #16June2017}

- **Recommendations (Beta - Premium users only)**: The Improve panel also includes a **Recommendations** page that recommends ways to improve your system by analyzing the conversations that users have with your chatbot, and taking into account your system's current training data and response certainty.

### 14 June 2017
{: #14June2017}

- **Fuzzy matching for additional languages (Beta)**: Fuzzy matching for entities is now available for additional languages, as noted in the [Supported languages](lang-support.html) topic. You can turn on fuzzy matching per entity to improve the ability of the service to recognize terms in user input with syntax that is similar to the entity, without requiring an exact match. The feature is able to map user input to the appropriate corresponding entity despite the presence of misspellings or slight syntactical differences. For example, if you define giraffe as a synonym for an animal entity, and the user input contains the terms giraffes or girafe, the fuzzy match is able to map the term to the animal entity correctly. See [Fuzzy matching](entities.html#fuzzy-matching) for details.

### 13 June 2017
{: #13June2017}

- **User conversations**: The Improve panel now includes a **User conversations** page, which provides a list of user interactions with your chatbot that can be filtered by keyword, intent, entity, or number of days. You can open individual conversations to correct intents, or to add entity values or synonyms.
- **Regex change**: The regular expressions that are supported by SpEL functions like find, matches, extract, replaceFirst, replaceAll and split have changed. A group of regular expression constructs are no longer allowed, including look-ahead, look-behind, possessive repetition and backreference constructs. This change was necessary to avoid a security exposure in the original regular expression library.

### 12 June 2017
{: #12June2017}

- The maximum number of workspaces that you can create with the **Lite** plan (formerly named the Free plan) changed from 3 to 5.
- You can now assign any name to a dialog node; it does not need to be unique. And you can subsequently change the node name without impacting how the node is referenced internally. The name you specify is treated as an alias and the system uses its own internal identifier to reference the node.
- You can no longer change the language of a workspace after you create it by editing the workspace details. If you need to change the language, you can export the workspace as a JSON file, update the language property, and then import the JSON file as a new workspace.

### 6 June 2017
{: #6June2017}

- **Learn**: A new *Learn about {{site.data.keyword.conversationfull}}* page is available that provides getting started information and links to service documentation and other useful resources. To open the page, click the ![i for information.](images/info.png) icon in the page header.
- **Bulk export and delete**: You can now simultaneously export a number of intents or entities to a CSV file, so you can then import and reuse them for another {{site.data.keyword.conversationshort}} application. You can also simultaneously select a number of entities or intents for deletion in bulk.
- **Updates to Korean**: Korean tokenizers have been updated to address informal language support. IBM continues to work on improvements to entity recognition and classification.
- **Emoji support**: Emojis added to intent examples, or as entity values, will now be correctly classified/extracted. **Note**: Only emojis that are included in your training data will be correctly and consistently identified; emoji support may not correctly classify similar emojis with different color tones or other variations.
- **Entity stemming (Beta - English only)**: The fuzzy matching beta feature recognizes entities and matches them based on the stem form of the entity value. For example, this feature correctly recognizes 'bananas' as being similar to 'banana', and 'run' being similar to 'running' as they share a common stem form. For more information, see [Fuzzy matching](entities.html#fuzzy-matching).
- **Workspace import progress**: When you import a workspace from a JSON file, a tile for the workspace is displayed immediately, in which information about the progress of the import is displayed.
- **Reduced training time**: Multiple models are now trained in parallel, which noticeably reduces the training time for large workspaces.

### 26 May 2017
{: #26May2017}

- The current API version is now `2017-05-26`. This version introduces the following changes:
    - The schema of ErrorResponse objects has changed. This change affects all endpoints and methods. For more information, see the [API Reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.
    - The internal schema used to represent dialog nodes in exported workspace JSON has changed. If you use the `2017-05-26` API to import a workspace that was exported using an earlier version, some dialog nodes might not import correctly. For best results, always import a workspace using the same version that was used to export it.

### 25 May 2017
{: #25May2017}

- You can now manage context variables in the "Try it out" pane. Click the **Manage context** link to open a new pane where you can set and check the values of context variables as you test the dialog. See [Testing your dialog](dialog-build.html#test) for more information.

### 16 May 2017
{: #16May2017}

- A **Car Dashboard** sample workspace is now available when you open the tool. To use the sample as a starting point for your own workspace, edit the workspace. If you want to use it for multiple workspaces, then duplicate it instead. The sample workspace does not count toward your subscription workspace total unless you use it.
- It is now easier to navigate the tool. The navigation menu options are available from the side of the main page instead of the top. At the top of the page, Breadcrumb links display that show you where you are. You can now switch between service instances from the Workspaces page. To get there quickly, click **Back to workspaces** from the navigation menu. If you have multiple service instances, the name of the current instance is displayed. You can click the **Change** link beside it to choose another instance.
- When you create a dialog, two nodes are now added to it for you: 1) a **Welcome** node at the top of the dialog tree that contains the greeting to display to the user and 2) an **Anything else** node at the bottom of the tree that catches any user inquiries that are not recognized by other nodes in the dialog and responds to them. See [Creating a dialog](dialog-build.html) for more details.
- When you are testing a dialog in the "Try it out" pane, you can now find and resubmit a recent test utterance by pressing the Up key to cycle through your previous inputs.
- Experimental Korean language support for 5 system entities (`@sys-date`, `@sys-time`, `@sys-currency`, `@sys-number`, `@sys-percentage`) is now available. There are known issues for some of the numeric entities, and limited support for informal language input.
- An Overview page is available from the Improve tab. The page provides a summary of interactions with your bot. You can view the amount of traffic for a given time period, as well as the intents and entities that were recognized most often in user conversations. For additional information, see [Using the Overview page](logs_oview.html).

### 27 April 2017
{: #27April2017}

- The following system entities are now available as beta features in English only:
    - sys-location: Recognizes references to locations, such as towns, cities, and countries, in user utterances.
    - sys-person: Recognizes references to people's names, first and last, in user utterances.

    For more information, see the [System entities reference](system-entities.html).
- Fuzzy matching for entities is a beta feature that is now available in English. You can turn on fuzzy matching per entity to improve the ability of the service to recognize terms in user input with syntax that is similar to the entity, without requiring an exact match. The feature is able to map user input to the appropriate corresponding entity despite the presence of misspellings or slight syntactical differences. For examples, if you define **giraffe** as a synonym for an animal entity, and the user input contains the terms *giraffes* or *girafe*, the fuzzy match is able to map the term to the animal entity correctly. See [Defining entities](entities.html#fuzzy-matching) and search for `Fuzzy Matching` for details.

### 18 April 2017
{: #18April2017}

- The {{site.data.keyword.conversationshort}} REST API now supports access to the following resources:
    - entities
    - entity values
    - entity value synonyms
    - logs

    For more information, see the [API Reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.
- The behavior of the /messages `POST` method has changed the handling of entities and intents specified as part of the message input:
    - If you specify intents on input, the service uses the intents you specify, but uses natural language processing to detect entities in the user input.
    - If you specify entities on input, the service uses the entities you specify, but uses natural language processing to detect intents in the user input.

        The behavior has not changed for messages that specify both intents and entities, or for messages that specify neither.
- The option to mark user input as irrelevant is now available for all supported languages. This is a beta feature.
- A new Credentials tab provides a single place where you can find all of the information you need for connecting your application to a workspace (such as the service credentials and workspace ID), as well as other deployment options. To access the Credentials tab for your workspace, click the ![Menu](images/Menu_16.png) icon and select **Credentials**.

### 9 March 2017
{: #9March2017}

The {{site.data.keyword.conversationshort}} REST API now supports access to the following resources:

- workspaces
- intents
- examples
- counterexamples

For more information, see the [API Reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.

### 7 March 2017
{: #7March2017}

- The use of `.` or `..` as an intent name causes problems and is no longer supported.

    You cannot rename or delete an intent with this name; to change the name, export your intents to a file, rename the intent in the file, and import the updated file into your workspace.

    Paying customers can contact support for a database change.

### 1 March 2017
{: #1March2017}

- System entities are now enabled in German.

### 22 February 2017
{: #22February2017}

- Messages are now limited to 2,048 characters.

### 3 February 2017
{: #3February2017}

- We changed how intents are scored and added the ability to mark input as irrelevant to your application. For details, see [Defining intents](intents.html) and search for `Mark as irrelevant`.

- This release introduced a major change to the workspace. To benefit from the changes, you must manually upgrade your workspace. To do so, complete the following steps:

  1.  [Duplicate your workspace](configure-workspace.html#exporting-and-copying-workspaces).
  1.  Upgrade the duplicate workspace by clicking the upgrade icon (![upgrade icon](images/upgrade.png)).
  1.  Test the upgraded workspace.
  1.  When testing is done and things are working as expected, apply the upgrade to your application by changing the message API call to use **2017-02-03** or later.

- The processing of **Jump to** actions changed to prevent loops that can occur under certain conditions. Previously, if you jumped to the condition of a node and neither that node nor any of its peer nodes had a condition that was evaluated as true, the system would jump to the root-level node and look for a node whose condition matched the input. In some situations this processing created a loop, which prevented the dialog from progressing.

  Under the new process, if neither the target node nor its peers is evaluated as true, the dialog turn is ended. To reimplement the old model, add a final peer node with a condition of `true`. In the response, use a **Jump to** action that targets the condition of the first node at the root level of your dialog tree.

### 11 January 2017
{: #11January2017}

- In this release, you can customize node titles in dialog.

### 22 December 2016
{: #22December2016}

- In this release, dialog nodes display a new section for `node title`. The ability to customize the `node title` is not available. When collapsed, the `node title` displays the `node condition` of the dialog node. If there is not a `node condition`, "Untitled Node" is displayed as the title.

### 19 December 2016
{: #19December2016}

Several changes make the dialog builder easier and more intuitive to use:

- A larger editing view makes it easier to view all the details of a node as you work on it.
- A node can contain multiple responses, each triggered by a separate condition. For more information see [Multiple responses](dialog-overview.html#responses).

### 5 December 2016
{: #5December2016}

- New languages are supported, all in Experimental mode: German, Traditional Chinese, Simplified Chinese, and Dutch.
- Two new system entities are available: @sys-date and @sys-time. For details, see [System entities](system-entities.html).

### 21 October 2016
{: #21October2016}

- The {{site.data.keyword.conversationshort}} service now provides system entities, which are common entities that can be used across any use case. For details, see [Defining entities](entities.html) and search for `Enabling system entities`.
- You can now view a history of conversations with users on the Improve page. You can use this to understand your bot's behavior. For details, see [Improving understanding](logs.html).
- You can now import entities from a comma-separated value (CSV) file, which helps with when you have a large number of entities. For details, see [Defining entities](entities.html) and search for `Importing entities`.

### 20 September 2016
{: #20September2016}

**New version**: 2016-09-20

To take advantage of the changes in a new version, change the value of the `version` parameter to the new date. If you're not ready to update to this version, don't change your version date.

- version **2016-09-20**: `dialog_stack` changed from an array of strings to an array of JSON objects.

### 29 August 2016
{: #29August2016}

- You can move dialog nodes from one branch to another, as siblings or peers. For details, see [Moving a dialog node](dialog-build.html#move-node).
- You can expand the JSON editor window.
- You can view chat logs of your bot's conversations to help you understand it's behavior. You can filter by intents, entities, date, and time. For details, see [Improving understanding](logs.html)

### 11 July 2016
{: #21July2016}

- This General Availability release enables you to work with entities and dialogs to create a fully functioning bot.

### 18 May 2016
{: #18May2016}

- This Experimental release of the {{site.data.keyword.conversationshort}} introduces the user interface and enables you to work with workspaces, intents, and examples.
