---
title: "How to make an Alexa Skill with Python"
date: 2017-08-05T11:51:01-07:00
draft: false
categories: [ "Tutorial" ]
tags: [ "Alexa", "Python"]
---


This guide will assume that you have already completed the [Alexa Python Tutorial](https://developer.amazon.com/alexa-skills-kit/alexa-skill-quick-start-tutorial) from Amazon. If you have done this then you should have a good handle on how to create new Alexa Skills and Lambda functions. But you probably still have no idea how to write code for an Alexa Skill. The code for “alexa-skills-kit-color-expert-python” is almost unreadable.

This tutorial will show you a template with some nice clean readable code. The full code for this template is on [GitHub](https://github.com/jrigden/alexa_template).

### JSON Everywhere

Almost everything you will be dealing with is JSON. Check out the “J[SON Interface Reference for Custom Skills](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/alexa-skills-kit-interface-reference)” as a reference. Those JSON objects will be represented by Python dictionaries. You will be seeing many dictionaries within dictionaries.

### The Lambda Function

Lambda functions start with `lambda_handler` function. Think of it like a `main()`.

<pre name="1caa" id="1caa" class="graf graf--pre graf-after--p">def lambda_handler(event, context):  
    if event['request']['type'] == "LaunchRequest":  
        return on_launch(event, context)</pre>

<pre name="377d" id="377d" class="graf graf--pre graf-after--pre">    elif event['request']['type'] == "IntentRequest":  
        return intent_router(event, context)</pre>

This function is given two arguments: `event`, `context`. `event` is a Python dictionary. First we check `event[‘request’][‘type’]`to see what type of request we have received. [Standard Request Types](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/custom-standard-request-types-reference) can be one of three values:

*   [LaunchRequest](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/custom-standard-request-types-reference#launchrequest)
*   [IntentRequest](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/custom-standard-request-types-reference#intentrequest)
*   [SessionEndedRequest](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/custom-standard-request-types-reference#sessionendedrequest)

### on_launch

This happens when the skill is invoked with no intent. When called, Alexa will say, “This is the body”. It will also create a card with the title, “This is the title”. In this function we see out first helper function from the template. The `statement` function.

<pre name="a7d2" id="a7d2" class="graf graf--pre graf-after--p">def on_launch(event, context):  
    return statement("This is the title", "This is the body")</pre>

### `statement`

This is a simple helper function that builds a response. It takes a `title` and a `body`. It uses these to build the [spoken output](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/alexa-skills-kit-interface-reference#outputspeech-object) and the [card output](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/alexa-skills-kit-interface-reference#card-object).

<pre name="e62a" id="e62a" class="graf graf--pre graf-after--p">def statement(title, body):  
    speechlet = {}  
    speechlet['outputSpeech'] = build_PlainSpeech(body)  
    speechlet['card'] = build_SimpleCard(title, body)  
    speechlet['shouldEndSession'] = True  
    return build_response(speechlet)</pre>

In this function we create a dictionary. Remember, these dictionaries will be turned into JSON objects. We give the key `outputSpeech` the value returned by `build_PlainSpeech`. We give the key `card` the value returned by `build_SimpleCard`. And we set the key `shouldEndSession` to `True`. Then we give that dictionary to `build_response` and return the result.

Let’s deconstruct some of the functions called here.

### build_PlainSpeech

This function builds the [OutputSpeech](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/alexa-skills-kit-interface-reference#outputspeech-object) object.

<pre name="e54f" id="e54f" class="graf graf--pre graf-after--p">def build_PlainSpeech(body):  
    speech = {}  
    speech['type'] = 'PlainText'  
    speech['text'] = body  
    return speech</pre>

### build_SimpleCard

This function builds the [Card](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/alexa-skills-kit-interface-reference#card-object) object. Cards need titles.

<pre name="003a" id="003a" class="graf graf--pre graf-after--p">def build_SimpleCard(title, body):  
    card = {}  
    card['type'] = 'Simple'  
    card['title'] = title  
    card['content'] = body  
    return card</pre>

### build_response

This function builds the [Response body](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/alexa-skills-kit-interface-reference#response-body-syntax).

<pre name="d9f5" id="d9f5" class="graf graf--pre graf-after--p">def build_response(message, session_attributes={}):  
    response = {}  
    response['version'] = '1.0'  
    response['sessionAttributes'] = session_attributes  
    response['response'] = message  
    return response</pre>

### intent_router

This happens when the skill is invoked with an intent. We need to do another check now. This time we need to route the right intent to the right function. I am assigning `event[‘request’][‘intent’][‘name’]` to `intent` just for readability. Those nested dictionaries can get hard to read.

<pre name="67c3" id="67c3" class="graf graf--pre graf-after--p">def intent_router(event, context):  
    intent = event['request']['intent']['name']</pre>

<pre name="5871" id="5871" class="graf graf--pre graf-after--pre">    # Custom Intents</pre>

<pre name="5ba0" id="5ba0" class="graf graf--pre graf-after--pre">    if intent == "SingIntent":  
        return sing_intent(event, context)</pre>

<pre name="1405" id="1405" class="graf graf--pre graf-after--pre">    if intent == "TripIntent":  
        return trip_intent(event, context)</pre>

<pre name="e221" id="e221" class="graf graf--pre graf-after--pre">    if intent == "CounterIntent":  
        return counter_intent(event, context)</pre>

<pre name="08a2" id="08a2" class="graf graf--pre graf-after--pre">    # Required Intents</pre>

<pre name="1eb8" id="1eb8" class="graf graf--pre graf-after--pre">    if intent == "AMAZON.CancelIntent":  
        return cancel_intent()</pre>

<pre name="e5ec" id="e5ec" class="graf graf--pre graf-after--pre">    if intent == "AMAZON.HelpIntent":  
        return help_intent()</pre>

<pre name="59e1" id="59e1" class="graf graf--pre graf-after--pre">    if intent == "AMAZON.StopIntent":  
        return stop_intent()</pre>

You may have noticed that I skipped over adding intents to the Alexa Skill Kit interface. This is a bit beyond the scope of this tutorial. The Skill Builder is complex. For now just open the Builder’s code editor and paste in the code from [intents.json](https://github.com/jrigden/alexa_template/blob/master/intents.json) from the GitHub repo. There are many good guides for the Skill Builder available online.

Let’s talk a look at some of these functions.

### sing_intent

This function gets called when we receive the `SingIntent`. It uses the `statement` function from earlier to sing a short song to the user.

<pre name="99f9" id="99f9" class="graf graf--pre graf-after--p">def sing_intent(event, context):  
    song = "Daisy, Daisy. Give me your answer, do. I'm half crazy all for the love of you"  
    return statement("daisy_bell_intent", song)</pre>

### trip_intent

This function gets called when we receive the `TripIntent`. This intent uses the [Dialog Model](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/ask-define-the-vui-with-gui#define-dialog). A dialog helps the collection multipart data by asking follow up questions. We need to return a [Delegate Directive](http://Delegate%20Directive) to continue the dialog.

We check the `dialogState`. If it is `STARTED` or `IN_PROGRESS` we call the `continue_dialog` function. When the `dialogState` is `COMPLETED`, we can access the information in the slots.

<pre name="1b45" id="1b45" class="graf graf--pre graf-after--p">def trip_intent(event, context):  
    dialog_state = event['request']['dialogState']</pre>

<pre name="e570" id="e570" class="graf graf--pre graf-after--pre">    if dialog_state in ("STARTED", "IN_PROGRESS"):  
        return continue_dialog()</pre>

<pre name="91f8" id="91f8" class="graf graf--pre graf-after--pre">    elif dialog_state == "COMPLETED":  
        return statement("trip_intent", "Have a good trip")</pre>

<pre name="a3bb" id="a3bb" class="graf graf--pre graf-after--pre">    else:  
        return statement("trip_intent", "No dialog")</pre>

### continue_dialog

This function creates and returns a `Dialog.Delegate`.

<pre name="20a4" id="20a4" class="graf graf--pre graf-after--p">def continue_dialog():  
    message = {}  
    message['shouldEndSession'] = False  
    message['directives'] = [{'type': 'Dialog.Delegate'}]  
    return build_response(message)</pre>

### counter_intent

This function gets called when we receive the `CounterIntent`. This function introduces the [Session object](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/alexa-skills-kit-interface-reference#session-object) and the template’s `conversation` function. The session allows us store key value pairs in `event[‘session’][‘attributes’]` . These key value pairs disappear at the end of a session.

In this function we are incrementing a counter in the session. Then we return the result of template’s `conversation` function.

<pre name="b02c" id="b02c" class="graf graf--pre graf-after--p">def counter_intent(event, context):  
    session_attributes = event['session']['attributes']  

    if "counter" in session_attributes:  
        session_attributes['counter'] += 1  

    else:  
        session_attributes['counter'] = 1  

    return conversation("counter_intent",  
                        session_attributes['counter'],  
                        session_attributes)</pre>

### conversation

This function is very similar to the `statement` function. It does two thing differently. First it sets the `shouldEndSession` to `False`, because we want to continue the session. It also sends back the session in the response. This maintains the information in the session.

<pre name="df8e" id="df8e" class="graf graf--pre graf-after--p">def conversation(title, body, session_attributes):  
    speechlet = {}  
    speechlet['outputSpeech'] = build_PlainSpeech(body)  
    speechlet['card'] = build_SimpleCard(title, body)  
    speechlet['shouldEndSession'] = False  
    return build_response(speechlet,  
                          session_attributes=session_attributes)</pre>

### **Conclusion**

I hope you found this useful. Although this was not an complete guide to making Alexa Skill with Python, it should get you started. Go out there and make awesome Alexa Skills! If you publish a skill send it to me. I would love to try it out.

Please send me any questions or suggestions on twitter [@mr_rigden](https://twitter.com/mr_rigden)
