#### Order Service Release Notes:

### 30 November 2017
## Method Create
## Versions 
* V2 http://business2.giftcertificates.com/2/0/WSOrder.asmx
* V1 https://business1.giftcertificates.com/webservice/wsorder.asmx

## Enhanced Result when submitting an order.
EnhancedStatus entity now shows more detail about the current status of an order during processing.  

Previous version: 

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <soap:Body>
        <CreateResponse xmlns="http://www.giftcertificates.com/WebService/">
            <CreateResult>
                <OrderCreateResultCode></OrderCreateResultCode>
                <OrderID></OrderID>
                <ConfirmationNumber></ConfirmationNumber>
            </CreateResult>
        </CreateResponse>
    </soap:Body>
</soap:Envelope>
```

* OrderCreateResultCode - String representing state of order
* OrderID - Client generated identifier
* ConfirmationNumber - Giftcertificates generated order number


New Version Result


```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <soap:Body>
        <CreateResponse xmlns="http://www.giftcertificates.com/WebService/">
            <CreateResult>
                <OrderCreateResultCode>Success</OrderCreateResultCode>
                <OrderID>3_INTER00392_a15b0454d5184e1f85b5af7a5f9c22b6</OrderID>
                <ConfirmationNumber>3341700182958</ConfirmationNumber>
                <EnhancedStatus>
                  <AcknowledgeMentId></acknowledgementId>
                  <StatusMessage></StatusMessage>
                  <StatusCode></StatusCode>
                </EnhancedStatus>
            </CreateResult>
        </CreateResponse>
    </soap:Body>
</soap:Envelope>
```

* AcknowledgeMentId - to be used in the future as a reference identifier
* StatusCode - Status code indicating status of order submission.  StatusCode will follow the guidelines layed out according to the [HTTP Specifcation status codes](https://tools.ietf.org/html/rfc2616).  For the Create method the following codes will apply:
    * 201 - Accepted
    * 400 - Bad Request - any issues related to system input information
    * 500 - Internal Server Error - internal errors and the AcknowledgeMentId can be used to relay information about the submitted     request
    
* StatusMessage - a helpful message to indicate any extra information about order submission.



