### Order Service Release Notes:

## 30 November 2017
* Adding of Enhanced Order information in Retrieve Order Status
When retrieving an order status there is now an EnhancedStatus entity that shows more detail about the current status of an order during processing.  
Previous version: 

```xml

<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <soap:Body>
        <CreateResponse xmlns="http://www.giftcertificates.com/WebService/">
            <CreateResult>
                <OrderCreateResultCode>Success</OrderCreateResultCode>
                <OrderID>3_INTER00392_a15b0454d5184e1f85b5af7a5f9c22b6</OrderID>
                <ConfirmationNumber>3341700182958</ConfirmationNumber>
            </CreateResult>
        </CreateResponse>
    </soap:Body>
</soap:Envelope>

```

New enahanced entity added and can be accessed if needed.  This is an addition and is not intended to break any existing clients.

```xml

<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <soap:Body>
        <CreateResponse xmlns="http://www.giftcertificates.com/WebService/">
            <CreateResult>
                <OrderCreateResultCode>Success</OrderCreateResultCode>
                <OrderID>3_INTER00392_a15b0454d5184e1f85b5af7a5f9c22b6</OrderID>
                <ConfirmationNumber>3341700182958</ConfirmationNumber>
            </CreateResult>
            <EnhancedStatus>
                  <acknowledgement></acknowledgement>
                  <Status></Status>
            </EnhancedStatus>
        </CreateResponse>
    </soap:Body>
</soap:Envelope>
```

