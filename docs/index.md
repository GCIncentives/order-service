### 8 December 2017
## Method: Create(ExternalOrder)
## Versions 
* V2 http://business2.giftcertificates.com/2/0/WSOrder.asmx
* V1 https://business1.giftcertificates.com/webservice/wsorder.asmx

## Enhanced Result when submitting an order.
EnhancedStatus entity now shows more detail about the current status of an order during processing.  

Previous OrderCreateResult:


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
* OrderID - Client generated order identifier
* ConfirmationNumber - Giftcertificates generated order number


## Enhanced OrderCreateResult:  
No modifications have been made to the existing fields that werer previously part of the OrderCreateResult - OrderCreateResultCode, OrderID and ConfirmationNumber and all messages and status' remain as in previous versions

Enhanced properties:
    * EnhancedStatusMessage - additional information to help with understanding the actual state of an order
    * EnhancedStatusCode - Status codes to follow the model of HTTP status codes.  This will be the direction of the GC API set moving forward


```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <soap:Body>
        <CreateResponse xmlns="http://www.giftcertificates.com/WebService/">
            <CreateResult>
                <OrderCreateResultCode>Success</OrderCreateResultCode>
                <ExternalEnhancedCreateResult>Unassigned</ExternalEnhancedCreateResult>
                <OrderID>3ff7ba6c-5991-4df5-8921-7f2427708bf5</OrderID>
                <ConfirmationNumber>3421701372247</ConfirmationNumber>
                <AcknowledgementIdentifier>170c8394-f80a-46dd-9d21-8e27e433ca4d</AcknowledgementIdentifier>
                <EnhancedStatusMessage>The order has been Accepted, currently it's in Submitted state. Order processing will be accomplished within 2 minutes of submission</EnhancedStatusMessage>
                <EnhancedStatusCode>202</EnhancedStatusCode>
                <EnhanceCacheIdentifier>829cfc9e-9e24-43c1-ae4f-b44edb9022ab</EnhanceCacheIdentifier>
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



