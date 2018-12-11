---

copyright:
  years: 2015, 2018
lastupdated: "2018-07-12"

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

# Expression language methods

You can process values extracted from user utterances that you want to reference in a context variable, condition, or elsewhere in the response.
{: shortdesc}

This version of the {{site.data.keyword.conversationshort}} documentation is deprecated. Go [here](https://console.bluemix.net/docs/services/assistant/dialog-methods.html) instead.
{: deprecated}

## Evaluation syntax

To expand variable values inside other variables, or apply methods to output text or context variables, use the `<? expression ?>` expression syntax. For example:

- **Incrementing a numeric property**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **Invoking a method on an object**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`

The following sections describe methods you can use to process values. They are organized by data type:

- [Arrays](dialog-methods.html#arrays)
- [Date and Time](dialog-methods.html#date-time)
- [Numbers](dialog-methods.html#numbers)
- [Objects](dialog-methods.html#objects)
- [Strings](dialog-methods.html#strings)

## Arrays
{: #arrays}

You cannot use these methods to check for a value in an array in a node condition or response condition within the same node in which you set the array values.

### JSONArray.append(object)

This method appends a new value to the JSONArray and returns the modified JSONArray.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Make this update:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.append('ketchup', 'tomatoes') ?>"
  }
}
```
{: codeblock}

Result:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ketchup", "tomatoes"]
  }
}
```
{: codeblock}

### JSONArray.clear()

This method clears all values from the array and returns null.

Use the following expression in the output to define a field that clears an array that you saved to a context variable ($toppings_array) of its values.

```json
{
  "output": {
    "array_eraser": "<? $toppings_array.clear() ?>"
  }
}
```
{: codeblock}

If you subsequently reference the $toppings_array context variable, it returns '[]' only.

### JSONArray.contains(object value)

This method returns true if the input JSONArray contains the input value.

For this Dialog runtime context which is set by a previous node or external application:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Dialog node or response condition:

```json
$toppings_array.contains('ham')
```
{: codeblock}

Result: `True` because the array contains the element ham.

### JSONArray.get(integer)

This method returns the input index from the JSONArray.

For this Dialog runtime context which is set by a previous node or external application:

```json
{
  "context": {
    "name": "John",
    "nested": {
      "array": [ "one", "two" ]
    }
  }
}
```
{: codeblock}

Dialog node or response condition:

```json
$nested.array.get(0).getAsString().contains('one')
```
{: codeblock}

Result:
`True` because the nested array contains `one` as a value.

Response:

```json
"output": {
  "generic" : [
    {
      "values": [
        {
        "text" : "The first item in the array is <?$nested.array.get(0)?>"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
```
{: codeblock}

### JSONArray.getRandomItem()

This method returns a random item from the input JSONArray.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Dialog node output:

```json
{
  "output": {
  "generic" : [
    {
      "values": [
        {
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
}
```
{: codeblock}

Result: `"ham is a great choice!"` or `"onion is a great choice!"` or `"olives is a great choice!"`

**Note:** The resulting output text is randomly chosen.

### JSONArray.join(string delimiter)

This method joins all values in this array to a string. Values are converted to string and delimited by the input delimiter.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Dialog node output:

```json
{
  "output": {
  "generic" : [
    {
      "values": [
        {
    "text": "This is the array: <? $toppings_array.join(';') ?>"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
}
```
{: codeblock}

Result:

```json
This is the array: onion;olives;ham;
```
{: codeblock}

If you define a variable that stores multiple values in a JSON array, you can return a subset of values from the array, and then use the join() method to format them properly.

A `collection projection` SpEL expression extracts a subcollection from an array. The syntax for a collection projection is `array_that_contains_value_sets.![value_of_interest]`.

For example, the following context variable defines a JSON array that stores flight information. There are two data points per flight, the time and flight code.

```json
"flights_found": [
  {
    "time": "10:00",
    "flight_code": "OK123"
  },
  {
    "time": "12:30",
    "flight_code": "LH421"
  },
  {
    "time": "16:15",
    "flight_code": "TS4156"
  }
]
```
{: codeblock}

To return the flight codes only, you can create a collection project expression by using the following syntax:

```
<? $flights_found.![flight_code] ?>
```

This expression returns an array of the `flight_code` values as `["OK123","LH421","TS4156"]`. See the [SpEL Collection projection documentation](https://docs.spring.io/spring/docs/3.0.x/reference/expressions.html) for more details.

If you apply the `join()` method to the values in the returned array, the flight codes are displayed in a comma-separated list. For example, you can use the following syntax in a response:

```
The flights that fit your criteria are:
  <? T(String).join(",", $flights_found.![flight_code]) ?>.
```
{: codeblock}

Result: `The flights that match your criteria are: OK123,LH421,TS4156.`

### JSONArray.remove(integer)

This method removes the element in the index position from the JSONArray and returns the updated JSONArray.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Make this update:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.remove(0) ?>"
  }
}
```
{: codeblock}

Result:

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: codeblock}

### JSONArray.removeValue(object)

This method removes the first occurrence of the value from the JSONArray and returns the updated JSONArray.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Make this update:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.removeValue('onion') ?>"
  }
}
```
{: codeblock}

Result:

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: codeblock}

### JSONArray.set(integer index, object value)

This method sets the input index of the JSONArray to the input value and returns the modified JSONArray.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Dialog node output:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.set(1,'ketchup')?>"
  }
}
```
{: codeblock}

Result:

```json
{
  "context": {
    "toppings_array": ["onion", "ketchup", "ham"]
  }
}
```
{: codeblock}

### JSONArray.size()

This method returns the size of the JSONArray as an integer.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Make this update:

```json
{
  "context": {
    "toppings_array_size": "<? $toppings_array.size() ?>"
  }
}
```
{: codeblock}

Result:

```json
{
  "context": {
    "toppings_array_size": 2
  }
}
```
{: codeblock}

### JSONArray split(String regexp)

This method splits the input string by using the input regular expression. The result is a JSONArray of strings.

For this input:

```
"bananas;apples;pears"
```
{: codeblock}

This syntax:

```json
{
  "context": {
    "array": "<?input.text.split(";")?>
  }
}
```
{: codeblock}

Results in this output:

```json
{
  "context": {
    "array": [ "bananas", "apples", "pears" ]
  }
}
```
{: codeblock}

### com.google.gson.JsonArray support
{: #com.google.gson.JsonArray}

In addition to the built-in methods, you can use standard methods of the `com.google.gson.JsonArray` class.

#### New array

new JsonArray().append('value')

To define a new array that will be filled in with values that are provided by users, you can instantiate an array. You must also add a placeholder value to the array when you instantiate it. You can use the following syntax to do so:

```json
{
  "context":{
    "answer": "<? output.answer?:new JsonArray().append('temp_value') ?>"
  }
```
{: codeblock}

## Date and Time
{: #date-time}

Several methods are available to work with date and time.

For information about how to recognize and extract date and time information from user input, see [@sys-date and @sys-time entities](system-entities.html#sys-datetime).

### .after(String date or time)

Determines whether the date/time value is after the date/time argument.

### .before(String date or time)
Determines whether the date/time value is before the date/time argument.

For example:
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')

- If comparing different items, such as `time vs. date`, `date vs. time`, and `time vs. date and time`, the method returns false and an exception is printed in the response JSON log `output.log_messages`.

  For example, `@sys-date.before(@sys-time)`.
- If comparing `date and time vs. time` the method ignores the date and only compares times.

### now()

Returns a string with the current date and time in the format `yyyy-MM-dd HH:mm:ss`.

- Static function.
- The other date/time methods can be invoked on date-time values that are returned by this function and it can be passed in as their argument.
- If the context variable `$timezone` is set, this function returns dates and times in the client's time zone.

Example of a dialog node with `now()` used in the output field:

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "<? now() ?>"
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Example of `now()` in node's conditions (to decide if it is still morning):

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
      "generic": [
        {
        "values": [
          {
          "text": "Good morning!"
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
        }
      ]
  }
}
```
{: codeblock}

### .reformatDateTime(String format)

Formats date and time strings to the format desired for user output.

Returns a formatted string according to the specified format:

- `MM/dd/yyyy` for 12/31/2016
- `h a` for 10pm

To return the day of the week:

- `E` for Tuesday
- `u` for day index (1 = Monday, ..., 7 = Sunday)

For example, this context variable definition creates a $time variable that saves the value 17:30:00 as *5:30 PM*.

```json
{
  "context": {
    "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

Format follows the Java [SimpleDateFormat ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html){: new_window} rules.

**Note**: When trying to format time only, the date is treated as `1970-01-01`.

### .sameMoment(String date/time)

- Determines whether the date/time value is the same as the date/time argument.

### .sameOrAfter(String date/time)

- Determines whether the date/time value is after or the same as the date/time argument.
- Analogous to `.after()`.

### .sameOrBefore(String date/time)

- Determines whether the date/time value is before or the same as the date/time argument.

### today()

Returns a string with the current date in the format `yyyy-MM-dd`.

- Static function.
- The other date methods can be invoked on date values that are returned by this function and it can be passed in as their argument.
- If the context variable `$timezone` is set, this function returns dates in the client's time zone. Otherwise, the `GMT` time zone is used.

Example of a dialog node with `today()` used in the output field:

```json
{
  "conditions": "#what_day_is_it",
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "Today's date is <? today() ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Result: `Today's date is 2018-03-09.`

## Date and time calculations

Use the following methods to calculate a date.

| Method                  | Description |
|-------------------------|-------------|
| `<date>.minusDays(n)`   | Returns the date of the day n number of days before the specified date. |
| `<date>.minusMonths(n)` | Returns the date of the day n number of months before the specified date. |
| `<date>.minusYears(n)`  | Returns the date of the day n number of years before the specified date. |
| `<date>.plusDays(n)`   | Returns the date of the day n number of days after the specified date. |
| `<date>.plusMonths(n)` | Returns the date of the day n number of months after the specified date. |
| `<date>.plusYears(n)`  | Returns the date of the day n number of years after the specified date. |

where `<date>` is specified in the format `yyyy-MM-dd` or `yyyy-MM-dd HH:mm:ss`.

To get tomorrow's date, specify the following expression:

```json
{
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "Tomorrow's date is <? today().plusDays(1) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Result if today is March 9, 2018: `Tomorrow's date is 2018-03-10.`

To get the date for the day a week from today, specify the following expression:

```json
{
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "Next week's date is <? @sys-date.plusDays(7) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Result if the date captured by the @sys-date entity is today's date, March 9, 2018: `Next week's date is 2018-03-16.`

To get last month's date, specify the following expression:

```json
{
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "Last month the date was <? today().minusMonths(1) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Result if today is March 9, 2018: `Last month the date was 2018-02-9.`

Use the following methods to calculate time.

| Method                  | Description |
|-------------------------|-------------|
| `<time>.minusHours(n)`   | Returns the time n hours before the specified time. |
| `<time>.minusMinutes(n)` | Returns the time n minutes before the specified time. |
| `<time>.minusSeconds(n)`  | Returns the time n seconds before the specified time. |
| `<time>.plusHours(n)`   | Returns the time n hours after the specified time. |
| `<time>.plusMinutes(n)` | Returns the time n minutes after the specified time. |
| `<time>.plusSeconds(n)`  | Returns the time n secons after the specified time. |

where `<time>` is specified in the format `HH:mm:ss`.

To get the time an hour from now, specify the following expression:

```json
{
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "One hour from now is <? now().plusHours(1) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Result if it is 8 AM: `One hour from now is 09:00:00.`

To get the time 30 minutes ago, specify the following expression:

```json
{
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "A half hour before @sys-time is <? @sys-time.minusMinutes(30) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Result if the time captured by the @sys-time entity is 8 AM: `A half hour before 08:00:00 is 07:30:00.`

To reformat the time that is returned, you can use the following expression:

```json
{
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "6 hours ago was <? now().minusHours(6).reformatDateTime('h:mm a') ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Result if it is 2:19 PM: `6 hours ago was 8:19 AM.`

### Working with time spans
{: #time-spans}

To show a response based on whether today's date falls within a certain time frame, you can use a combination of time-related methods. For example, if you run a special offer during the holiday season every year, you can check whether today's date falls between November 25 and December 24 of this year. First, define the dates of interest as context variables.

In the following start and end date context variable expressions, the date is being constructed by concatenating the dynamically-derived current year value with hard-coded month and day values.

```json
"context": {
   "end_date": "<? now().reformatDateTime('Y') + '-12-24' ?>",
   "start_date": "<? now().reformatDateTime('Y') + '-11-25' ?>"
 }
```

In the response condition, you can indicate that you want to show the response only if the current date falls between the start and end dates that you defined as context variables.

`now().after($start_date) && now().before($end_date)`

### java.util.Date support
{: #java.util.Date}

In addition to the built-in methods, you can use standard methods of the `java.util.Date` class.

To get the date of the day that falls a week from today, you can use the following syntax.

```json
{
  "context": {
    "week_from_today": "<? new Date(new Date().getTime() +
      (7 * (24*60*60*1000L))) ?>"
  }
}
```
{: codeblock}

This expression first gets the current date in milliseconds (since January 1, 1970, 00:00:00 GMT). It also calculates the number of milliseconds in 7 days. (The `(24*60*60*1000L)` represents one day in milliseconds.) It then adds 7 days to the current date. The result is the full date of the day that falls a week from today. For example, `Fri Jan 26 16:30:37 UTC 2018`. Note that the time is in the UTC time zone. You can always change the 7 to a variable (`$number_of_days`, for example) that you can pass in. Just be sure that its value gets set before this expression is evaluated.

If you want to be able to subsequently compare the date with another date that is generated by the service, then you must reformat the date. System entities (`@sys-date`) and other built-in methods (`now()`) convert dates to the `yyyy-MM-dd` format.

```json
{
  "context": {
    "week_from_today": "<? new Date(new Date().getTime() +
      (7 * (24*60*60*1000L))).format('yyyy-MM-dd') ?>"
  }
}
```
{: codeblock}

After reformatting the date, the result is `2018-01-26`. Now, you can use an expression like `@sys-date.after($week_from_today)` in a response condition to compare a date specified in user input to the date saved in the context variable.

The following expression calculates the time 3 hours from now.

```json
{
  "context": {
    "future_time": "<? new Date(new Date().getTime() + (3 * (60*60*1000L)) -
      (5 * (60*60*1000L))).format('h:mm a') ?>"
  }
}
```
{: codeblock}

The `(60*60*1000L)` value represents an hour in milliseconds. This expression adds 3 hours to the current time. It then recalculates the time from a UTC time zone to EST time zone by subtracting 5 hours from it. It also reformats the date values to include hours and minutes AM or PM.

## Numbers
{: #numbers}

These methods help you get and reformat number values.

For information about system entities that can recognize and extract numbers from user input, see [@sys-number entity](system-entities.html#sys-number).

If you want the service to recognize specific number formats in user input, such as order number references, consider creating a pattern entity to capture it. See [Creating entities](entities.html#creating-entities) for more details.

If you want to change the decimal placement for a number, to reformat a number as a currency value, for example, see the [String format() method](dialog-methods.html#java.lang.String).

### toDouble()

  Converts the object or field to the Double number type. You can call this method on any object or field. If the conversion fails, *null* is returned.

### toInt()

  Converts the object or field to the Integer number type. You can call this method on any object or field. If the conversion fails, *null* is returned.

### toLong()

  Converts the object or field to the Long number type. You can call this method on any object or field. If the conversion fails, *null* is returned.

  If you specify a Long number type in a SpEL expression, you must append an `L` to the number to identify it as such. For example, `5000000000L`. This syntax is required for any numbers that do not fit into the 32-bit Integer type. For example, numbers that are greater than 2^31 (2,147,483,648) or lower than -2^31 (-2,147,483,648) are considered Long number types. Long number types have a minimum value of -2^63 and a maximum value of 2^63-1.

### Java number support
{: #java.lang.Number}

### java.lang.Math()

Performs basic numeric operations.

You can use the the Class methods, including these:

- max()

```json
{
  "context": {
    "bigger_number": "<? T(Math).max($number1,$number2) ?>"
  },
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "The bigger number is $bigger_number."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

- min()

```json
{
  "context": {
    "smaller_number": "<? T(Math).min($number1,$number2) ?>"
  },
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "The smaller number is $smaller_number."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

- pow()

```json
{
  "context": {
    "power_of_two": "<? T(Math).pow($base.toDouble(),2.toDouble()) ?>"
  },
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "Your number $base to the second power is $power_of_two."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

See the [java.lang.Math reference documentation](https://docs.oracle.com/javase/7/docs/api/java/lang/Math.html) for information about other methods.

### java.util.Random()

Returns a random number. You can use one of the following syntax options:

- To return a random boolean value (true or false), use `<?new Random().nextBoolean()?>`.
- To return a random double number between 0 (included) and 1 (excluded), use `<?new Random().nextDouble()?>`
- To return a random integer between 0 (included) and a number you specify, use `<?new Random().nextInt(n)?>`  where n is the top of the number range you want + 1.
  For example, if you want to return a random number between 0 and 10, specify `<?new Random().nextInt(11)?>`.
- To return a random integer from the full Integer value range (-2147483648 to 2147483648), use `<?new Random().nextInt()?>`.

For example, you might create a dialog node that is triggered by the #random_number intent. The first response condition might look like this:

```json
Condition = @sys-number
{
  "context": {
    "answer": "<? new Random().nextInt(@sys-number.numeric_value + 1) ?>"
  },
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "Here's a random number between 0 and @sys-number.literal: $answer."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

See the [java.util.Random reference documentation](https://docs.oracle.com/javase/7/docs/api/java/util/Random.html) for information about other methods.

You can use standard methods of the following classes also:

- `java.lang.Byte`
- `java.lang.Integer`
- `java.lang.Long`
- `java.lang.Double`
- `java.lang.Short`
- `java.lang.Float`

## Objects
{: #objects}

### JSONObject.clear()

This method clears all values from the JSON object and returns null.

For example, you want to clear the current values from the $user context variable.

```json
{
  "context": {
    "user": {
      "first_name":"John",
      "last_name":"Snow"
    }
  }
}
```
{: codeblock}

Use the following expression in the output to define a field that clears the object of its values.

```json
{
  "output": {
    "object_eraser": "<? $user.clear() ?>"
  }
}
```
{: codeblock}

If you subsequently reference the $user context variable, it returns `{}` only.

You can use the `clear()` method on the `context` or `output` JSON objects in the body of the API `/message` call.

#### Clearing context
{: #clearing_context}

When you use the `clear()` method to clear the `context` object, it clears **all** variables except these ones:

 - `context.conversation_id`
 - `context.timezone`
 - `context.system`

**Warning**: All context variable values means:

  - All default values that were set for variables in nodes that have been triggered during the current session.
  - Any updates made to the default values with information provided by the user or external services during the current session.

To use the method, you can specify it in an expression in a variable that you define in the output object. For example:

```json
{
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "Response for this node."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ],
    "context_eraser": "<? context.clear() ?>"
  }
}

```

#### Clearing output
{: #clearing_output}

When you use the `clear()` method to clear the `output` object, it clears all variables except the one you use to clear the output object and any text responses that you define in the current node. It also does not clear these variables:

- `output.nodes_visited`
- `output.nodes_visited_details`

To use the method, you can specify it in an expression in a variable that you define in the output object. For example:

```json
{
  "output": {
    "generic": [
      {
        "values": [
          {
          "text": "Have a great day!"
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ],
    "output_eraser": "<? output.clear() ?>"
  }
}
```

If a node earlier in the tree defines a text response of `I'm happy to help.` and then jumps to a node with the JSON output object defined above, then  only `Have a great day.` is displayed as the response. The `I'm happy to help.` output is not displayed, because it is cleared and replaced with the text response from the node that is calling the `clear()` method.

### JSONObject.has(string)

This method returns true if the complex JSONObject has a property of the input name.

For this Dialog runtime context:

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

Dialog node output:

```json
{
  "conditions": "$user.has('first_name')"
}
```
{: codeblock}

Result: The condition is true because the user object contains the property `first_name`.

### JSONObject.remove(string)

This method removes a property of the name from the input `JSONObject`. The `JSONElement` that is returned by this method is the `JSONElement` that is being removed.

For this Dialog runtime context:

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

Dialog node output:

```json
{
  "context": {
    "attribute_removed": "<? $user.remove('first_name') ?>"
  }
}
```
{: codeblock}

Result:

```json
{
  "context": {
    "user": {
      "last_name": "Snow"
    },
    "attribute_removed": {
      "first_name": "John"
    }
  }
}
```
{: codeblock}

### com.google.gson.JsonObject support
{: #com.google.gson.JsonObject}

In addition to the built-in methods, you can use standard methods of the `com.google.gson.JsonObject` class.

## Strings
{: #strings}

There methods help you work with text.

For information about how to recognize and extract certain types of Strings, such as people names and locations, from user input, see [System entities](system-entities.html).

**Note:** For methods that involve regular expressions, see [RE2 Syntax reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/google/re2/wiki/Syntax){: new_window} for details about the syntax to use when you specify the regular expression.

### String.append(object)

This method appends an input object to the string as a string and returns a modified string.

For this Dialog runtime context:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

This syntax:

```json
{
  "context": {
    "my_text": "<? $my_text.append(' More text.') ?>"
  }
}
```
{: codeblock}

Results in this output:

```json
{
  "context": {
    "my_text": "This is a text. More text."
  }
}
```
{: codeblock}

### String.contains(string)

This method returns true if the string contains the input substring.

Input: "Yes, I'd like to go."

This syntax:

```json
{
  "conditions": "input.text.contains('Yes')"
}
```
{: codeblock}

Results: The condition is `true`.

### String.endsWith(string)

This method returns true if the string ends with the input substring.

For this input:

```
"What is your name?".
```
{: codeblock}

This syntax:

```json
{
  "conditions": "input.text.endsWith('?')"
}
```
{: codeblock}

Results: The condition is `true`.

### String.extract(String regexp, Integer groupIndex)

This method returns a string extracted by specified group index of the input regular expression.

For this input:

```
"Hello 123456".
```
{: codeblock}

This syntax:

```json
{
  "context": {
    "number_extract": "<? input.text.extract('[\\d]+',0) ?>"
  }
}
```
{: codeblock}

  **Important:** To process `\\d` as the regular expression, you have to escape both the backslashes by adding another `\\`: `\\\\d`

Result:

```json
{
  "context": {
    "number_extract": "123456"
  }
}
```
{: codeblock}

### String.find(string regexp)

This method returns true if any segment of the string matches the input regular expression.  You can call this method against a JSONArray or JSONObject element, and it will convert the array or object to a string before making the comparison.

For this input:

```
"Hello 123456".
```
{: codeblock}

This syntax:

```json
{
  "conditions": "input.text.find('^[^\d]*[\d]{6}[^\d]*$')"
}
```
{: codeblock}

Result: The condition is true because the numeric portion of the input text matches the regular expression `^[^\d]*[\d]{6}[^\d]*$`.

### String.isEmpty()

This method returns true if the string is an empty string, but not null.

For this Dialog runtime context:

```json
{
  "context": {
    "my_text_variable": ""
  }
}
```
{: codeblock}

This syntax:

```json
{
  "conditions": "$my_text_variable.isEmpty()"
}
```
{: codeblock}

Results: The condition is `true`.

### String.length()

This method returns the character length of the string.

For this input:

```
"Hello"
```
{: codeblock}

This syntax:

```json
{
  "context": {
    "input_length": "<? input.text.length() ?>"
  }
}
```
{: codeblock}

Results in this output:

```json
{
  "context": {
    "input_length": 5
  }
}
```
{: codeblock}

### String.matches(string regexp)

This method returns true if the string matches the input regular expression.

For this input:

```
"Hello".
```
{: codeblock}

This syntax:

```json
{
  "conditions": "input.text.matches('^Hello$')"
}
```
{: codeblock}

Result: The condition is true because the input text matches the regular expression `\^Hello\$`.

### String.startsWith(string)

This method returns true if the string starts with the input substring.

For this input:

```
"What is your name?".
```
{: codeblock}

This syntax:

```json
{
  "conditions": "input.text.startsWith('What')"
}
```
{: codeblock}

Results: The condition is `true`.

### String.substring(int beginIndex, int endIndex)

This method gets a substring with the character at `beginIndex` and the last character set to index before `endIndex`.
The endIndex character is not included.

For this Dialog runtime context:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

This syntax:

```json
{
  "context": {
    "my_text": "<? $my_text.substring(5, $my_text.length()) ?>"
  }
}
```
{: codeblock}

Results in this output:

```json
{
  "context": {
    "my_text": "is a text."
  }
}
```
{: codeblock}

### String.toLowerCase()

This method returns the original String converted to lowercase letters.

For this input:

```
"This is A DOG!"
```
{: codeblock}

This syntax:

```json
{
  "context": {
    "input_lower_case": "<? input.text.toLowerCase() ?>"
  }
}
```
{: codeblock}

Results in this output:

```json
{
  "context": {
    "input_upper_case": "this is a dog!"
  }
}
```
{: codeblock}

### String.toUpperCase()

This method returns the original String converted to upper case letters.

For this input:

```
"hi there".
```
{: codeblock}

This syntax:

```json
{
  "context": {
    "input_upper_case": "<? input.text.toUpperCase() ?>"
  }
}
```
{: codeblock}

Results in this output:

```json
{
  "context": {
    "input_upper_case": "HI THERE"
  }
}
```
{: codeblock}

### String.trim()

This method trims any spaces at the beginning and the end of the string and returns the modified string.

For this Dialog runtime context:

```json
{
  "context": {
    "my_text": "   something is here    "
  }
}
```
{: codeblock}

This syntax:

```json
{
  "context": {
    "my_text": "<? $my_text.trim() ?>"
  }
}
```
{: codeblock}

Results in this output:

```json
{
  "context": {
    "my_text": "something is here"
  }
}
```
{: codeblock}

### java.lang.String support
{: #java.lang.String}

In addition to the built-in methods, you can use standard methods of the `java.lang.String` class.

#### java.lang.String.format()

You can apply the standard Java String `format()` method to text. See [java.util.formatter reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://docs.oracle.com/javase/7/docs/api/java/util/Formatter.html#syntax){: new_window} for information about the syntax to use to specify the format details.

For example, the following expression takes three decimal integers (1, 1, and 2) and adds them to a sentence.

```json
{
  "formatted String": "<? T(java.lang.String).format('%d + %d equals %d', 1, 1, 2) ?>"
}
```
{: codeblock}

Result: `1 + 1 equals 2`.

To change the decimal placement for a number, use the following syntax:

```
{
  <? T(String).format('%.2f',<number to format>) ?>
}
```
{: codeblock}

For example, if the $number variable that needs to be formatted in US dollars is `4.5`, then a response such as, `Your total is $<? T(String).format('%.2f',$number) ?>` returns `Your total is $4.50.`

## Indirect data type conversion

When you include an expression within text, as part of a node response, for example, the value is rendered as a String. If you want the expression to be rendered in its original data type, then do not surround it with text.

For example, you can add this expression to a dialog node response to return the entities that are recognized in the user input in String format:

```json
  The entities are <? entities ?>.
```
{: codeblock}

If the user specifies *Hello now* as the input, then the @sys-date and @sys-time entities are triggered by the `now` reference. The entities object is an array, but because the expression is included in text, the entities are returned in String format, like this:

```json
  The entities are 2018-02-02, 14:34:56.
```
{: codeblock}

If you do not include text in the response, then an array is returned instead. For example, if the response is specified as an expression only, not surrounded by text.

```
  <? entities ?>
```
{: codeblock}

The entity information is returned in its native data type, as an array.

```json
[
  {
    "entity":"sys-date","location":[6,9],"value":"2018-02-02","confidence":1,"metadata":{"calendar_type":"GREGORIAN","timezone":"America/New_York"}
  },
  {
    "entity":"sys-time","location":[6,9],"value":"14:33:22","confidence":1,"metadata":{"calendar_type":"GREGORIAN","timezone":"America/New_York"}
  }
  ]
```
{: codeblock}

As another example, the following $array context variable is an array, but the $string_array context variable is a string.

```json
{
  "context": {
    "array": [
      "one",
      "two"
    ],
    "array_in_string": "this is my array: $array"
  }
}
```
{: codeblock}

If you check the values of these context variables in the Try it out pane, you will see their values specified as follows:

**$array** : `["one","two"]`

**$array_in_string** : `"this is my array: [\"one\",\"two\"]"`

You can subsequently perform array methods on the $array variable, such as `<? $array.removeValue('two') ?>`,but not the $array_in_string variable.
