# Motivation

As backend systems grow more complex, the need for asynchronous processing has become essential. Users expect fast responses, but many operations take time from sending mail, processing payments to generating reports.

> ### Background workers
Here you have task that needs to be processed, but you don't want to handle it inside the reques-response cycle because it would take too long. So instead you push the task to a queue and let a separate service (a worker) process it asynchronously.

Typical examples include: 
- Sending emails
- Processing payments
- Generating PDFs
- Calling thyird-party APIs
> Notes: The task itself is not something you care about long-term

> ### Event-Driven Processing 
Event-driven processing is different in both scale and intent.

Event-driven architectures usually involve multiple independent processes reacting to the same event.

FOr example, when a user completes a payment, you may want to trigger several actions:
- Service A - Send a confirmation email
- Service B - Update the payment service
- Service C - Update the order service
...

> Event-driven does not always mean real-time.

Even though the system is event-driven, not all consumers process the event immediately. Some servicers react in real time, while others (like analytics or reporting) may process the same event hours or days later.

=> Background workers fonus on getting work done while event-driven system focusing on reacting to events.
# Rabbit MQ 

Rabbit MQ is built for background processing. It is designed to deliver a task to a consumer, ensure it gets processed, and then remove it from the queue.

# Kafka

Kafka is built for event-driven systems. It is built as a distributed log. When an event is written to KAfka, it stays there for a configured retention period. Comsumers don't consume and delete messages instead they read from the log at their own pace.


# When to choose what???

RabbitMQ excels when you're building traditional background worker system.

- You need a task processed once and you're done: sending welcome emails, processing uploaded images, calling a payment API.
- Operational simplicity matters
- You want built-in worker patterns

Kafka  is the right choice when you're building event-driven systems with specific requirements:
- Multiple services need to react to the same event.
- You need replay capability.
- Scale is a concern.