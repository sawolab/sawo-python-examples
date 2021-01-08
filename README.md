# Sawo

Passwordless and OTP-less Authentication for your website. [Check the documentaion here.](https://docs.sawolabs.com/sawo/v/0.0.2/)

## Installing

A step by step series of examples that tell you how to get a development env running. These instructions will let you render the form in your speicified container, and allow you to attach successful login callback for futher actions.

#### Install the sawo package

```
pip install sawo
```
#### Import from Sawo createTemplate, getContext and verifyToken method from the sawo package

**The getContext method is used to create sawo dict object from data gain from DB in Django**
```python
from sawo import createTemplate,getContext,verifyToken
```

#### API Key
* Login to sawo [dev console.](dev.sawolabs.com)
* Create a new project
    * Set Project Name
    * Set Project Host
        *  For dev: point to localhost
        *  For prod: point to your domain.
*  Copy the API key

#### Setup
Getting started Creating template for sawo passwordless and OTP-less Authentication for your website.

###### Django
```python
    createTemplate("<filepath>")

    #example
    createTemplate("templates/partials")
```
###### Flask
```python
    createTemplate("./<filepath>",flask=True)

    #example
    createTemplate("./templates/partials",flask=True)
```

This will create a _sawo.html file in the filepath provided.

##### Using _sawo.html in your html files

###### Django

```html
    {% include "<filepath>/_sawo.html" %}

    #example
    {% include "partials/_sawo.html" %}
```

###### Flask
```html
    {% include "./<filepath>/_sawo.html" %}

    #example
    {% include "./partials/_sawo.html" %}
```


##### Sending data required by _sawo.html
The variable name used in **_sawo.html** template are **sawo.auth_key**, **sawo.identifier** and **sawo.to** so to send that data we create a json.

**Note**
1. The "to" route should be a post route which can recive posted data
2. If you dont know how data is passed to templates in Django or Flask, We will suggest looking into it first. 

###### Django
##### Method 1. Sending static data

```python
    context = {
            "sawo": {
                "auth_key" : "<api_key>",
                "identifier":"email | phone_number_sms",
                "to":"<route>" #the route where you will recive the payload sent by sdk
                }
            }

    #exapmple
    context = {
            "sawo": {
                "auth_key" : "785ha-hdjsdsd-799-ss345",
                "identifier":"phone_number_sms",
                "to":"login" 
                }
            }
```

##### Method 2. Using admin and database to save config for sawo
Step 1. Creating fields for sawo api_key and identifier to set it from admin dashboard

**copy this code in models of your app**

```python
class Config(models.Model):
    api_key = models.CharField(max_length=200)
    identifier = models.CharField(max_length=100, choices=[("email","Email"),("phone_number_sms","Phone")])
```

Step 2. Setting up view.py of app

**Note: Route should be the reciving end where you can handle post request**
```python
from .models import Config
from sawo import getContext

def <yourfuntion>(request):
    config = Config.objects.order_by('-api_key')[:1]
    context = {
        "sawo":getContext(config,<route>)  #the route where you will recive the payload sent by sdk 
    }

#example
def index(request):
    config = Config.objects.order_by('-api_key')[:1]
    context = {
        "sawo":getContext(config,"login")
    }
```

###### Falsk
```python
    sawo = {
            "auth_key" : "<api_key>",
            "identifier":"email | phone_number_sms",
            "to":"<route>" #the route where you will recive the payload sent by sdk
        }
    
    #exapmple
    sawo = {
            "auth_key" : "785ha-hdjsdsd-799-ss345",
            "identifier":"phone_number_sms",
            "to":"login" 
        }
```
#### Verifying Token
When he login is done on the webpage it sends the data to backend as payload to verify user you can use verifyToken function , it returns a boolen.

##### Backend **Code is ame for Django and Flask**
```python
    from sawo import verifyToken

    #use the methods provided by flask and django to recive data from post request
    #then use this funtion it will return True or False depending on user status
    verifyToken(payload)

    //example
    payload = <data from POST request>
    if(verifyToken(payload)):
        #do somthing
    else:
        #do somthing else
```

## Tutorials

###### **Django Tutorial** - [See Tutorial](https://github.com/sawolab/sawo-python-examples)
###### **Flask Tutorial** - [See Tutorial](https://github.com/sawolab/sawo-python-examples) 

## Versioning
We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). Thank

## Authors
**SAWO Labs** - [GitHub](https://github.com/sawolab)

## License

This project is licensed under the MIT License