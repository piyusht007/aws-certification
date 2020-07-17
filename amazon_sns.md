# Amazon SNS:
* Push based publish-subscribe(Pub-Sub) messaging service.
* Works on the Topic abstraction (**1 Lakh** Topics per AWS account). A Topic can have multiple subscriptions (**1.25 Million** Subscriptions per Topic).
* **Subscribers** can be:
    * Amazon SQS Queue
    * Amazon Lambda Function
    * HTTP/HTTPS endpoint
    * SMS
    * EMAIL
    * MOBILE PUSH BASED SERVICES (ADM, APNs, BAIDU, FMC etc..)
* Commonly used to: 
    * `Fanout` messages to multiple systems.
    * Send application & system alerts.
    * Push email & text messaging.
    * Mobile push notifications.
* **Notification**:
    * Messages sent by SNS will be a JSON object with following fields :
        * **MessageId**: A Universally Unique Identifier, unique for each notification published.
        * **Timestamp**: The time (in GMT) at which the notification was published.
        * **TopicArn**: The topic to which this message was published
        * **Type**: The type of the delivery message. 
            * `Notification` - Notification deliveries. 
            * `SubscriptionConfirmation` 
            * `UnsubscribeConfirmation` 
        * **UnsubscribeURL**: A link to unsubscribe the end-point from this topic, and prevent receiving any further notifications.
        * **Message**: The payload (body) of the message, as received from the publisher.
        * **Subject**: The Subject field – if one was included as an optional parameter to the publish API call along with the message.
        * **Signature**: Base64-encoded “SHA1withRSA” signature of the Message, MessageId, Subject (if present), Type, Timestamp, and Topic values.
        * **SignatureVersion**: Version of the Amazon SNS signature used.
        * **Note**: Notification messages sent over the “Email” transport only contain the payload (message body) as received from the publisher
    * **Note**: Notification messages sent over the `Email` transport only contain the payload (message body) as received from the publisher.
* **TTL (Time-To-Live)**:
    * **Default**: `4` weeks.
    * TTL can also be specified at message attribute and body level.
    * The precedence order is as follows:
        * Message attribute TTL - (**Highest**) 
        * Message body TTL
        * Push notification service default TTL (varies per service)
        * Amazon SNS default TTL (4 weeks) - (**Lowest**)
* **Message Attributes**:
    * Key-value pairs to be used as metadata for the message.
    * Allowed Types:
        * String
        * String.Array
        * Number
        * Binary
    * A message can have upto `10` attributes (key-value pairs).
    * These attributes also contributes to the message size limit i.e. `256` KB 
    * <u>Message attributes are sent only when the message structure is String, not JSON</u>.
* **Message Size**:
    * Excluding SMS, SNS messages can be of size upto `256 KB`.
    * Message formats can be `JSON`, `XML` and `Unformatted text`. 
* **Billing:**
    * A chunk of `64 KB` is considered as `1` request. So a single API call with `256 KB` payload/data will be billed as `4` requests.
* **Message Filters**:
    *  By default, `200` filters (filter policies) can created for an AWS account.
* **Message Delivery Failures**:
    * **Client-Side Errors**: No retries.
    * **Server-Side Errors**: Retries as configured in the retry policy.
        * For **AWS managed endpoints (SQS, Lambda)**,  Amazon SNS retries delivery up to `100,015` times, over `23` days
        * For **Customer managed endpoints**, Amazon SNS sets an internal delivery retry policy to `50` times over `6` hours.
    * **DLQ (Dead-Letter-Queue)** can be configured at the `Subscribers` end to get the failed messages to further processing.
        * **Amazon SQS FIFO Queues** can't be used as a dead-letter queue.   
* **Example**: [Publishing a message to SNS Topic using Python Boto3](https://gist.github.com/piyusht007/28b1990b47c09f39e5df4e4d20d897a7).
* **SNS Quotas**: [here](https://docs.aws.amazon.com/general/latest/gr/sns.html)
* **FAQs**: [here](https://aws.amazon.com/sns/faqs/)