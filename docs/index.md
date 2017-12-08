### 8 December 2017

## Versions 
* Production
    * V2 http://business2.giftcertificates.com/2/0/WSOrder.asmx
* Stage
    * V2 http://cte2.giftcertificates.com/2/0/WSOrder.asmx



## Caching of ExternaOrder upon request
### Method: Create(ExternalOrder)
### Enhancement Id: 10000 -7401

There have been numerous cases of orders that have been submitted that have been lost in transit when indicating that receipt of an order has succeeded.  The Create method has been enhanced to guarantee that orders will not be lost upon receipt and will cached to an external cache mechanism.  This will guarantee that all orders will be received and processing of an order should happen within 2 minutes of receipt.  

Two fields - EnhancedAcknowledgementIdentifier and EnhanceCacheIdentifier have been added to OrderCreateResult to help with processing of the order messages.  More on addition of Enhanced Fields in 

## Enhanced Result when submitting an order.
### Method: Create(ExternalOrder)
### Enhancement Id: 10001- 7401

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
    * EnhancedStatusCode - Status codes to follow the model of HTTP status codes.  This will be the direction of the GC API set moving  forward. StatusCode will follow the guidelines layed out according to the [HTTP Specifcation status codes](https://tools.ietf.org/html/rfc2616).  For the Create method the following codes will apply:
           * 201 - Accepted
           * 400 - Bad Request - any issues related to system input information
           * 500 - Internal Server Error - internal errors and the AcknowledgeMentId can be used to relay information about the submitted     request
    * EnhancedAcknowledgementIdentifier - Identifier that indicates receipt of request
    * EnhanceCacheIdentifier - Identifier that represents cache item of order



Successful Response to receiving an OrderMessage
```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <soap:Body>
        <CreateResponse xmlns="http://www.giftcertificates.com/WebService/">
            <CreateResult>
                <OrderCreateResultCode>Success</OrderCreateResultCode>
                <ExternalEnhancedCreateResult>Unassigned</ExternalEnhancedCreateResult>
                <OrderID>3ff7ba6c59914df589217f2427708bf5</OrderID>
                <ConfirmationNumber>0000000000000</ConfirmationNumber>
                <EnhancedAcknowledgementIdentifier>170c8394-f80a-46dd-9d21-8e27e433ca4d</EnhancedAcknowledgementIdentifier>
                <EnhancedStatusMessage>The order has been Accepted, currently it's in Submitted state. Order processing will be accomplished within 2 minutes of submission</EnhancedStatusMessage>
                <EnhancedStatusCode>202</EnhancedStatusCode>
                <EnhanceCacheIdentifier>829cfc9e-9e24-43c1-ae4f-b44edb9022ab</EnhanceCacheIdentifier>
            </CreateResult>
        </CreateResponse>
    </soap:Body>
</soap:Envelope>
```

If an error should go happen and an order cannot be cached the following response will be returned 

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <soap:Body>
        <CreateResponse xmlns="http://www.giftcertificates.com/WebService/">
            <CreateResult>
                <OrderCreateResultCode>Failure</OrderCreateResultCode>
                <ExternalEnhancedCreateResult>Unassigned</ExternalEnhancedCreateResult>
                <OrderID>3ff7ba6c59914df589217f2427708bf5</OrderID>
                <ConfirmationNumber>0000000000000</ConfirmationNumber>
                <EnhancedAcknowledgementIdentifier>170c8394-f80a-46dd-9d21-8e27e433ca4d</EnhancedAcknowledgementIdentifier>
                <EnhancedStatusMessage>An internal server error has occurred. Please contact support with the EnhancedAcknowledgementIdentifier</EnhancedStatusMessage>
                <EnhancedStatusCode>500</EnhancedStatusCode>
                <EnhanceCacheIdentifier>829cfc9e-9e24-43c1-ae4f-b44edb9022ab</EnhanceCacheIdentifier>
            </CreateResult>
        </CreateResponse>
    </soap:Body>
</soap:Envelope>
```

## Note about Duplicate Order exceptions.
### Method: Create(ExternalOrder)
The Create method will return an exception of 'Duplicate Order' when the Client order number has already been inserted into the system.  The current implementation does not allow for the duplication of the client order number.  

```xml
<?xml version="1.0" encoding="utf-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <soap:Body>
        <soap:Fault>
            <faultcode>soap:Server</faultcode>
            <faultstring>System.Web.Services.Protocols.SoapException: Server was unable to process request. ---&gt; System.Exception: Client Fault Certification Check Wrapper Exception - Please Look at Inner Exception for Details. ---&gt; System.Web.Services.Protocols.SoapException: Duplicate Order.
   at WebService.WSOrder.Create(ExternalOrder order) in C:\Development\GC-REPOS\src\WebService2.0\WebService\WSOrder.asmx.cs:line 921
   --- End of inner exception stack trace ---
   at WebService.WSOrder.ClientFaultCertificationCheck(Exception ex) in C:\Development\GC-REPOS\src\WebService2.0\WebService\WSOrder.asmx.cs:line 141
   at WebService.WSOrder.Create(ExternalOrder order) in C:\Development\GC-REPOS\src\WebService2.0\WebService\WSOrder.asmx.cs:line 932
   --- End of inner exception stack trace ---</faultstring>
            <detail />
        </soap:Fault>
    </soap:Body>
</soap:Envelope>
```



