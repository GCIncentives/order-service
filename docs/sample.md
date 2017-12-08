# Sample code
## Language: C#
## Method: Create(ExternalOrder)
### Applies to Version http://cte2.giftcertificates.com/2/0/WSOrder.asmx and http://business2.giftcertificates.com/2/0/WSOrder.asmx

```
 static void Main(string[] args)
        {

              // Create api credentials
            Data_WebServiceCredentials credentials = new Data_WebServiceCredentials();
           
            // add api client id
            credentials.ApiClientId = 1; 
            credentials.ApiClientPassword = "API Client password";

           
            int orderMemberId = 123456;
            OrderService.WSOrder orderService = new OrderService.WSOrder();
            orderService.WebServiceCredentials = credentials;
    
            // Create External Order entity
            ExternalOrder externalOrder = new ExternalOrder();
            externalOrder.RemainingBudget = 0;
            externalOrder.Status = OrderService.OrderStatus.Unset;
            externalOrder.OrderID = Guid.NewGuid().ToString();
            externalOrder.PhysicalLogoFileID = 0;
            externalOrder.DigitalLogoFileID = 0;
            externalOrder.OrderEmailAddress ="Orderinguseremailaddress@client.com";
            externalOrder.GcMemberID = orderMemberId;
            externalOrder.EcardFileID = 1;
            externalOrder.ProgramID = 0;
            externalOrder.CartID = 0;
            externalOrder.ServiceFeeAmount = 0.0;
            externalOrder.ServiceFeePercent = 0.0;
            externalOrder.ClientIP = "Add local ip address";
            externalOrder.QuotedTotal = 0;

            // Create External ShipTo Entity
            ExternalShipTo externalShipTo = new ExternalShipTo();
            externalShipTo.FirstName = "Receiver First Name";
            externalShipTo.LastName = "Receiver Last Name";
            externalShipTo.Email = "Receiver Email Address";
            externalShipTo.ShipMethodID = 0;
            externalShipTo.OneTimeUseTemplate = new ExternalEmailTemplate();
            externalShipTo.OneTimeUseTemplate.DigitalTemplateID = INSERT TEMPLATE ID;
            externalShipTo.OneTimeUseTemplate.Subject = "Subject";

            ExternalShipTo[] externalShipTos = new ExternalShipTo[1];
            externalShipTos[0] = externalShipTo;

        
            // Create ExternalShipToRecipient
            ExternalShipToRecipient externalShipToRecipient = new ExternalShipToRecipient();
            externalShipToRecipient.ItemType = ItemTypes.Catalog;
            externalShipToRecipient.FirstName = "AwardRecipientFirstName";
            externalShipToRecipient.LastName = "AwardRecipientLastName";
            externalShipToRecipient.RecipientEmail = "AwardRecipientEmailAddress";
            externalShipToRecipient.NotificationRequested = false;
            externalShipTos[0].ShipToRecipients = new ExternalShipToRecipient[1];
            externalShipTos[0].ShipToRecipients[0] = externalShipToRecipient;


            // Create SKUS                                                
            ExternalShipToRecipientSku[] bothSkus = new ExternalShipToRecipientSku[2];
            ExternalShipToRecipientSku externalShipToRecipientSku = new ExternalShipToRecipientSku();
            externalShipToRecipientSku.Quantity = 1;
            externalShipToRecipientSku.SkuNum = "SKU OF ITEM";
            externalShipToRecipientSku.ActualCost = 0;
            externalShipToRecipientSku.TotalPoints = 0;
            externalShipToRecipientSku.ItemTypeID = 0;
            externalShipToRecipientSku.OccasionId = 0;
            externalShipToRecipientSku.PhysicalTemplateId = 0;


            ExternalShipToRecipientSku externalShipToRecipientMessageSheetSku = new ExternalShipToRecipientSku();
            externalShipToRecipientMessageSheetSku.Quantity = 1;
            externalShipToRecipientMessageSheetSku.SkuNum = "MSHEET_D01";
            externalShipToRecipientMessageSheetSku.ActualCost = 0;
            externalShipToRecipientMessageSheetSku.TotalPoints = 0;
            externalShipToRecipientMessageSheetSku.PhysicalTemplateId = 0;

            externalShipToRecipientMessageSheetSku.ShipToRecipientSkuMessages = new ExternalShipToRecipientSkuMessage[3];
            externalShipToRecipientMessageSheetSku.ShipToRecipientSkuMessages[0] = new ExternalShipToRecipientSkuMessage();
            externalShipToRecipientMessageSheetSku.ShipToRecipientSkuMessages[0].Message = "Opening Message";
            externalShipToRecipientMessageSheetSku.ShipToRecipientSkuMessages[0].MessageType = ShipToRecipientSkuMessageTypes.Opening;

            externalShipToRecipientMessageSheetSku.ShipToRecipientSkuMessages[1] = new ExternalShipToRecipientSkuMessage();
            externalShipToRecipientMessageSheetSku.ShipToRecipientSkuMessages[1].Message = "SubjectMessage";
            externalShipToRecipientMessageSheetSku.ShipToRecipientSkuMessages[1].MessageType = ShipToRecipientSkuMessageTypes.Body;

            externalShipToRecipientMessageSheetSku.ShipToRecipientSkuMessages[2] = new ExternalShipToRecipientSkuMessage();
            externalShipToRecipientMessageSheetSku.ShipToRecipientSkuMessages[2].Message = "ClosingMessage";
            externalShipToRecipientMessageSheetSku.ShipToRecipientSkuMessages[2].MessageType = ShipToRecipientSkuMessageTypes.Closing;
            bothSkus[0] = externalShipToRecipientSku;
            bothSkus[1] = externalShipToRecipientMessageSheetSku;
            externalShipToRecipient.ShipToRecipientSkus = bothSkus;

            externalOrder.ShipTos = externalShipTos;

            externalOrder.PaymentMethods = new ExternalPaymentMethod[1];

            // Create Payment Methods
            ExternalPaymentMethod externalPaymentMethod = new ExternalPaymentMethod();
            externalPaymentMethod.IsSaveCardInfo = false;
            externalPaymentMethod.PaymentMethodTypeID = 0000;

            externalOrder.PaymentMethods[0] = externalPaymentMethod;
                                           
            // Submit Order;
            OrderCreateResult result = orderService.Create(externalOrder);
            // existing properties
            // Success or failure of order process
            //result.OrderCreateResultCode;

            // Client provided order identifier
            //result.OrderID;

            // Informational text 
            //result.ResultMessage;

            // GC generated order number
            //result.ConfirmationNumber;

            // Enhanced addtional fields

            // Acknowledgement Identifier indicating receipt of message
            //result.EnhancedAcknowledgementIdentifier;

            // Identifier representing cache id for guaranteed delivery of message
            //result.EnhancedCacheIdentifier;

            // updated status information indicating actual internal state of an order
            //result.EnhancedStatus;

            // updated textual information indicating details of order
            //result.EnhancedStatusMessage;
        }
```
