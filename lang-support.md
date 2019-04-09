---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-08"

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

# Supported languages
The {{site.data.keyword.conversationshort}} service supports the languages listed. Individual features of the service are supported to a greater or lesser extent for each language.
{: shortdesc}

This version of the {{site.data.keyword.conversationshort}} documentation is deprecated. Go [here](https://cloud.ibm.com/docs/services/assistant?topic=assistant-language-support) instead.
{: deprecated}

In the following tables, the level of language and feature support is indicated by these codes:

- **GA** - The feature is generally available and supported for this language. Note that features may continue to be updated even after they are generally available.
- **Beta** - The feature is supported only as a Beta release, and is still undergoing testing before it is made generally available in this language.
- **NA** - Indicates that a feature is not available in this language.

The first table shows the level of support for all features, except those related to entities which are shown in the second table.

**Table 1. Feature support details**

| Language | **Defining [intents](intents.html), [entities](entities.html), and [dialog](dialog-build.html)** | **[Absolute scoring and 'Mark as irrelevant'](intents.html#mark-irrelevant)** | **Search** | **[Content Catalog](catalog.html)** |
|:---|:---:|:---:|:---:|:---:|
| **English (en)**                   | GA | GA | GA | GA |
| **Arabic (ar)**                    | GA | Beta | NA | NA |
| **Chinese (Simplified) (zh-cn)**   | GA | GA | Beta | NA |
| **Chinese (Traditional) (zh-tw)**  | Beta | Beta | Beta | NA |
| **Czech (cs)**                     | Beta | Beta | Beta | NA |
| **Dutch (nl)**                     | GA | GA | Beta | NA |
| **French (fr)**                    | GA | GA | Beta | GA |
| **German (de)**                    | GA | GA | Beta | GA |
| **Italian (it)**                   | GA | GA | Beta | GA |
| **Japanese (ja)**                  | GA | GA | Beta | GA |
| **Korean (ko)**                    | GA | GA | Beta | NA |
| **Portuguese (Brazilian) (pt-br)** | GA | GA | Beta | GA |
| **Spanish (es)**                   | GA | GA | Beta | GA |
{: caption="Feature support details" caption-side="top"}

**Table 2. Entity feature support details**

| Language | **System entities ([number](system-entities.html#sys-number), [currency](system-entities.html#sys-currency), [percentage](system-entities.html#sys-percentage), [date, time](system-entities.html#sys-datetime))** | **[Entity fuzzy matching](entities.html#fuzzy-matching)** | **[Contextual entities](entities.html#defining-contextual-entities)** | **[Synonym recommendations](entities.html#synonyms)** |
|:---:|:---:|:---:|:---:|:---:|
| **English (en)**                   | GA, Beta ([location](system-entities.html#sys-location), [person](system-entities.html#sys-person)) | GA (Stemming, misspelling, and partial match) | Beta | GA |
| **Arabic (ar)**                    | Beta | GA (Misspelling only) | NA | NA |
| **Chinese (Simplified) (zh-cn)**   | GA | NA | NA | NA |
| **Chinese (Traditional) (zh-tw)**  | Beta | NA | NA | NA |
| **Czech (cs)**                     | Beta | GA (Misspelling only) | NA | NA |
| **Dutch (nl)**                     | GA | GA (Misspelling only) | NA | NA |
| **French (fr)**                    | GA | GA (Misspelling only) | NA | GA |
| **German (de)**                    | GA | GA (Misspelling only) | NA | NA |
| **Italian (it)**                   | GA | GA (Misspelling only) | NA | NA |
| **Japanese (ja)**                  | GA | GA (Misspelling only) | NA | GA |
| **Korean (ko)**                    | GA | GA (Misspelling only) | NA | NA |
| **Portuguese (Brazilian) (pt-br)** | GA | GA (Misspelling only) | NA | NA |
| **Spanish (es)**                   | GA | GA (Misspelling only) | NA | GA |
{: caption="Entity feature support details" caption-side="top"}

**Note:** The {{site.data.keyword.conversationshort}} service supports multiple languages as noted, but the tooling interface itself (descriptions, labels, etc.) is in English. All supported languages can be input and trained through the English interface.

**GB18030 compliance**: GB18030 is a Chinese standard that specifies an extended code page for use in the Chinese market. This code page standard is important for the software industry because the China National Information Technology Standardization Technical Committee has mandated that any software application that is released for the Chinese market after September 1, 2001, be enabled for GB18030. The {{site.data.keyword.conversationshort}} service supports this encoding, and is certified GB18030-compliant

## Changing a workspace language

Once a workspace has been created, its language cannot be modified. If it is necessary to change the supported language of a workspace, the user should download the workspace. Then, edit the resulting JSON file in a text editor, searching for a JSON property called `language`.

The `language` property should be set to the original language of the workspace; for example, English would be `en`. Modify the value of this property, changing it to the desired language (`fr` for French, `de` for German, etc.). Save the changes to the JSON file, and import the modified file into your {{site.data.keyword.conversationshort}} service instance.

## Configuring bi-directional languages
{: #configuring-bi-directional}

For bi-directional languages, for example Arabic, you can change your workspace preferences accordingly. From your workspace tab, select the *Actions* drop-down menu, and select **Bidi preferences** (this option is only available for workspaces set to a bi-directional language):

![Bidi preferences](images/bidi_prefs.png)

Select from the following options for your workspace:

- **GUI Direction**: Specifies the layout direction of elements, such as buttons or menus, in the graphical user interface. Choose `LTR` (left-to-right) or `RTL` (right-to-left). If not specified, the tool follows the web browser GUI direction setting.
- **Text Direction**: Specifies the direction of typed text. Choose `LTR` (left-to-right) or `RTL` (right-to-left), or select `Auto` which will automatically choose the text direction based on your system settings. The `None` option will display left-to-right text.
- **Numeric Shaping**: Specifies which form of numerals to use when presenting regular digits. Choose from `Nominal`, `Arabic-Indic`, or `Arabic-European`. The `None` option will display Western numerals.
- **Calendar Type**: Specifies how you choose filtering dates in the workspace UI. Choose `Islamic-Civil`, `Islamic-Tabular`, `Islamic-Umm al-Qura`, or `Gregorian`. **Note**: This setting does not apply to the "Try it out" panel.

![Bidi options](images/bidi_opts.png)

When finished making selections, click **Update** to save and return to the workspace tab.

## Working with accented characters
{: #working-with-accents}

In a conversational setting, users may or may not use accents while interacting with the {{site.data.keyword.conversationshort}} service. As such, both accented and non-accented versions of words may be treated the same for intent detection and entity recognition.

However for some languages, like Spanish, some accents can alter the meaning of the entity. Thus, for entity detection, although the original entity may implicitly have an accent, the service can also match the non-accented version of the same entity, but with a slightly lower confidence score.

For example, for the word "barrió", which has an accent and corresponds to the past tense of the verb "barrer" (to sweep), the service can also match the word "barrio" (neighborhood), but with a slightly lower confidence.

The system will provide the highest confidence scores in entities with exact matches. For example, `barrio` will not be detected if `barrió` is in the training set; and `barrió` will not be detected if `barrio` is in the training set.

You are expected to train the system with the proper characters and accents. For example, if you are expecting `barrió` as a response, then you should put `barrió` into the training set.

Although not an accent mark, the same applies to words using, for example, the Spanish letter `ñ` vs. the letter `n`, such as "uña" vs. "una". In this case the letter `ñ` is not simply an `n` with an accent; it is a unique, Spanish-specific letter.

You can enable fuzzy matching if you think your customers will not use the appropriate accents, or misspell words (including, for example, putting a `n` instead of a `ñ`), or you can explicitly include them in the training examples.
