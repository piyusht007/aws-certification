# Amazon SQS:
* Fully managed message queue service by Amazon.
* Offers 2 types of queues:
    * **Standard Queues**:
        * Maximum throughput
        * Best-effort ordering (Sequencing information can be attached to each message and re-order them upon reception)
        * At-least-once delivery
    * **FIFO Queues**:
        * Exactly once semantics
        * In-order
        * Throughput:
            * Batching: 
                * 3000 transactions/second/API Method (`SendMessageBatch`, `ReceiveMessage` or `DeleteMessageBatch`)
                * 3000 transactions = 300 API Calls (Each API call have a batch of 10 messages)
            * Without Batching:
                * Upto 300 transactions/second/API Method (`SendMessage`, `ReceiveMessage`, or `DeleteMessage`)           
 * **Message Size**: 
    * **Min**: `1 B`
    * **Max**: `256 KB`
    * Extended client library can be used to reference a higher payload size of upto 2 GB stored in Amazon S3.
 * **Message Batch**:
    * A single message batch request can have a maximum of `10` messages.      
 * **Message Retention Period**:
    * **Default**: `4 days`
    * **Min**: `60 Seconds`
    * **Max**: `14 Days` 
 * **Message Visibility Timeout**: The time for which the message will be hidden to other consumers while the message is being consumed. 
    * **Default**: `30 Seconds`
    * **Min**: `0`
    * **Max**: `12 Hours`
 * **Delaying a message** (Making it invisible to the consumers for a specified time) using **Delay Queue** and **Message Timer**:
    * Delay queues let you postpone the delivery of new messages to a queue for a number of seconds. It can be defined at queue or message level.
    * On queue level it is configured as **Delay queue**.
    * On message level it is configured as **Message timer**. _This has given priority over the value define at queue level._
    * **Default**, **Min**: `0` Seconds
    * **Max**: `15` Minutes
 * **Polling**:
    * By default, queues use **short polling** i.e the ReceiveMessage API call with wait time parameter set as 0 seconds.
    * Wait Time > 0 in ReceiveMessage API call is Long Polling. 
    * **Long Polling Wait Time**: 20 Seconds (Maximum)
 * **DELETING a message from queue**: To DELETE a message from queue you must have to receive that message first. Upon message reception you get a receipt handle which should be used to call the DELETE API.
 * [**Temporary Queues (Virtual Queues)**](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-temporary-queues.html):
    * Local data structure within a standard queue. 
    * Commonly used to implement Request-response messaging pattern.  
 * **SQS Quotas**: [here](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-quotas.html) 
 * **FAQs**: [here](https://aws.amazon.com/sqs/faqs/)