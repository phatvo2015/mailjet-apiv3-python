[api_credential]: https://app.mailjet.com/account/api_keys
[doc]: http://dev.mailjet.com/guides/?python#
[api_doc]: https://github.com/mailjet/api-documentation
[smsDashboard]: https://app.mailjet.com/sms?_ga=2.81581655.1972348350.1522654521-1279766791.1506937572
[smsInfo]: https://app.mailjet.com/docs/transactional-sms?_ga=2.183303910.1972348350.1522654521-1279766791.1506937572#trans-sms-token

![alt text](https://www.mailjet.com/images/email/transac/logo_header.png "Mailjet")

# Official Mailjet Python Wrapper

[![Build Status](https://travis-ci.org/mailjet/mailjet-apiv3-python.svg?branch=master)](https://travis-ci.org/mailjet/mailjet-apiv3-python)

### API documentation

All code examples can be found on the [Mailjet Documentation][doc].

(Please refer to the [Mailjet Documentation Repository][api_doc] to contribute to the documentation examples)

## Installation

``` bash
(sudo) pip install mailjet_rest
```

## Getting Started

Grab your API and Secret Keys [here][api_credential]. You need them for authentication when using the Email API:

```bash
export MJ_APIKEY_PUBLIC='your api key'
export MJ_APIKEY_PRIVATE='your api secret'
```

## API Versioning

The Mailjet API is spread among three distinct versions:

- `v3` - The Email API
- `v3.1` - Email Send API v3.1, which is the latest version of our Send API
- `v4` - SMS API

Since most Email API endpoints are located under `v3`, it is set as the default one and does not need to be specified when making your request. For the others you need to specify the version using `version`. For example, if using Send API `v3.1`:

``` python
# import the mailjet wrapper
from mailjet_rest import Client
import os

# Get your environment Mailjet keys
API_KEY = os.environ['MJ_APIKEY_PUBLIC']
API_SECRET = os.environ['MJ_APIKEY_PRIVATE']

mailjet = Client(auth=(API_KEY, API_SECRET), version='v3.1')

```

For additional information refer to our [API Reference](https://dev.preprod.mailjet.com/reference/overview/versioning/).

## Make a `GET` request:
``` python
# get all contacts
result = mailjet.contact.get()
```

## `GET` request with filters:
``` python
# get the first 2 contacts
result = mailjet.contact.get(filters={'limit': 2})
```
## `POST` request
``` python
# Register a new sender email address
result = mailjet.sender.create(data={'email': 'test@mailjet.com'})
```

## Combine a resource with an action
``` python
# Get the contacts lists of contact #2
result = mailjet.contact_getcontactslists.get(id=2)
```

## Send an Email
``` python

from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret), version='v3.1')
data = {
  'Messages': [
                {
                        "From": {
                                "Email": "pilot@mailjet.com",
                                "Name": "Mailjet Pilot"
                        },
                        "To": [
                                {
                                        "Email": "passenger1@mailjet.com",
                                        "Name": "passenger 1"
                                }
                        ],
                        "Subject": "Your email flight plan!",
                        "TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
                        "HTMLPart": "<h3>Dear passenger 1, welcome to Mailjet!</h3><br />May the delivery force be with you!"
                }
        ]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()

```

## Create a new Contact
``` python

# wrapping the call inside a function
def new_contact(email):
	return mailjet.contact.create(data={'Email': email})

new_contact('mr@smith.com')
```
