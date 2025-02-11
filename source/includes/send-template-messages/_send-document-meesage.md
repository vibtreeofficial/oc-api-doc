## Send Document Message
```shell
curl --location --globoff 'https://api.sobot.in/message/' \
--header 'x-api-key: yourapiaccesskey' \
--header 'x-api-secret: yourapisecretkey' \
--header 'content-type': 'application/json' \
--data '{
          "template_name": "hello_world",
          "template_lang": "en",
          "customer_number":"919970XXXXXX",
          "customer_name": "John Doe",
           "params":[
            {
                "type": "header",
                "parameters":[
                  {
                    "type": "document",
                    "document": {
                      "link": "https://example.com/documents/20230625_003239.pdf",
                      "filename" : "20230625_003239.pdf"
                    }
                  }
                ]
            }
          ]
}'
```

```python
import requests
import json
url = "https://api.sobot.in/message/"

payload = json.dumps(
  {
    "template_name": "hello_world",
    "template_lang": "en",
    "customer_number": "919970XXXXXX",
    "customer_name": "John Doe",
    "params":[
            {
                "type": "header",
                "parameters":[
                  {
                    "type": "document",
                    "document": {
                        "link": "https://example.com/documents/20230625_003239.pdf",
                        "filename" : "20230625_003239.pdf"
                    }
                  }
                ]
            }
          ]
}
)
headers = {
  'x-api-key': 'yourapiaccesskey',
  'x-api-secret': 'yourapisecretkey',
  'content-type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)
```

```php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://api.sobot.in/message/');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'x-api-key' => 'yourapiaccesskey',
  'x-api-secret' => 'yourapisecretkey',
  'content-type' => 'application/json'
));
$request->setBody('{
            "template_name": "hello_world",
            "template_lang": "en",
            "customer_number":"919970XXXXXX",
            "customer_name": "John Doe",
            "params":[
            {
                "type": "header",
                "parameters":[
                  {
                    "type": "document",
                    "document": {
                        "link": "https://example.com/documents/20230625_003239.pdf",
                        "filename" : "20230625_003239.pdf"
                      }
                  }
                ]
            }
          ]
            }'
);
try {
  $response = $request->send();
  if ($response->getStatus() == 200) {
    echo $response->getBody();
  }
  else {
    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .
    $response->getReasonPhrase();
  }
}
catch(HTTP_Request2_Exception $e) {
  echo 'Error: ' . $e->getMessage();
}

```

> The above API call returns JSON response like this:

```json
{
    "data": {
        "id": "wamid.HBgMOTE5OTcwNTg1NTU2FQIAERgSQjhGRDQ2REFBQjAxMjJBM0Q1AA==",
        "status": "submitted"
    },
    "message": "Message sent successfully.",
    "code": 200
}
```
This API allows you to send Template messages with Document to customers using the Sobot platform, enabling seamless communication with your customer.


### HTTP Request

`POST https://api.sobot.in/message/`

### Body Parameters

Parameter | Required | Description |
--------- | ------- | ----------- | 
template_name | Yes | Specify the name of the template. You can get the name of template from template ```GET``` API. | 
template_lang | Yes | Specify the lang_code of the template that you want to send. e.g. en, en_US, en_GB | -
customer_number | Yes| WhatsApp phone number of the customer you want to send a message to. <br /> <br />No Plus signs (+), hyphens (-), parenthesis ((,)), and spaces are supported in customer phone number. <br /><br /> We highly recommend that you **include country calling code** when sending a message to a customer. If the country calling code is omitted, your business phone number's country calling code is prepended to the customer's phone number. This can result in undelivered or misdelivered messages. | 
customer_name | Yes | Name of a customer for future reference | 
params | Yes | This is a JSON array accepts the single JSON object to specify document as a header for document template. JSON object has two keys: <ul><li>type: In this case ```header```</li> <li>parameters: A nested JSON array holds single object which has following keys: <ul><li>type: for document header specify ```document``` as a type.</li><li>A public URL of document. Please see below table for supported Media types.</li></ul>

### Supported Media Types

| Media     | Supported Types| Size Limit                                       |
|-----------|----------------|-----------|
| document | text/plain, application/pdf, application/vnd.ms-powerpoint, application/msword, application/vnd.ms-excel, application/vnd.openxmlformats-officedocument.wordprocessingml.document, application/vnd.openxmlformats-officedocument.presentationml.presentation, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet | 100MB |


### Response
In response of successfully sent messages, you will get ```id``` & ```status``` under ```data``` key. Messages are identified by a unique ID (```wamid```). You can track message status in the Webhooks through its ```wamid```  (Check Webhook Documentation for recieving Status events of sent messages on your webhook). This ```wamid``` can have a maximum length of up to 128 characters.



