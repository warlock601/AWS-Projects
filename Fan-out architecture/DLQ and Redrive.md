1. DLQ is a queue that holds os store messages that applications could not consume or that could not be delivered to their intended recipients. Basically other queues can target DLQ for messages that can't be processed successfully, they're useful for debuggiing applications or messaging systems.<br/>
2. Messages can't be processed because of a variety of possible issues, such as erroneous conditions within the producer or consumer application or an unexpected state change that causes an issue with your application code. For example, if a user places a web order with a particular product ID, but the product ID is deleted, the web store's code fails and displays an error, and the message with the order request is sent to a dead-letter queue.
3. Benifits:
   - Configure an alarm for any messages moved to a dead-letter queue.

   - Examine logs for exceptions that might have caused messages to be moved to a dead-letter queue.

   - Analyze the contents of messages moved to a dead-letter queue to diagnose software or the producer's or consumer's hardware issues.

   - Determine whether you have given your consumer sufficient time to process messages.
  
Note: The dead-letter queue of a FIFO queue must also be a FIFO queue. Similarly, the dead-letter queue of a standard queue must also be a standard queue.
4. Standard queues keep processing messages until the expiration of the retention period. 
5. FIFO queues provide exactly-once processing by consuming messages in sequence from a message group. Thus, although the consumer can continue to retrieve ordered messages from another message group, the first message group remains unavailable until the message blocking the queue is processed successfully or moved to a dead-letter queue.
6.  Don't use a dead-letter queue with standard queues when you want to be able to keep retrying the transmission of a message indefinitely. For example, don't use a dead-letter queue if your program must wait for a dependent process to become active or available.
7. Don't use a dead-letter queue with a FIFO queue if you don't want to break the exact order of messages or operations.

### DLQ Redrive: 
Redrive messages from a dead-letter queue to the selected destination queue. Let say our queue cannot process some messages, and we've a dead letter queue created for that queue, so all the unprocessed messages will be sent to the DLQ and we can then redrive those to either the source queue or a custom destination.

## Hands-on: "How to use DLQs & message redrive"
1. Create two Standard queues namely "main_queue" & "dlq_main_queue"
2. Open main_queue > Dead-letter queue > Edit > Enable Dead-letter queue & select dlq_main_queue as the DLQ, set maximum receives=1 (for hands-on purpose)
3. open main_queue > Send and receive messages > type message in the message body > Send. Then there'll be a message available
4. Now we'll create a situation where the message will be Polled but not processesed and because it is not processesed, it will go into DLQ.
5. Click on Poll and after timeout expires, again click on Poll and as we've set the maximum receives=1 so if no one acknowledges the message next time, it goes into DLQ.
![Screenshot 2023-11-22 123137](https://github.com/warlock601/AWS-Projects/assets/32487715/86e9a6ff-fa87-42a6-a3c1-4130e4ca2a1a)
6. Then we can send the message back to the original queue using DLQ Redrive.
![Screenshot 2023-11-22 123441](https://github.com/warlock601/AWS-Projects/assets/32487715/f2694cd3-86f7-4e15-b83e-fb3502143688)


Message will be again avaible in the original queue.

![Screenshot 2023-11-22 123605](https://github.com/warlock601/AWS-Projects/assets/32487715/ee135107-80fa-4d1b-9524-7267cb0ae408)
