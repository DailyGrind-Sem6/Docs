# Messaging

Messaging is a way for backend services to communicate with each other. There are many patterns to choose from (some of which I mention in my research docucment called `Research Messaging`), but in this case I will be using the `Publish/Subscribe` pattern. This pattern is used when you want to broadcast messages to multiple consumers. This is useful when you have multiple services that need to know about a certain event.

More justifications for the messaging patterns can be found in the `Research Messaging` document.

## Scenario

The feature I used to implement messaging is one that keeps track of the comment count on a certain post. In my application, the main page will display cards of posts. Each card will display some extra information about the post other than the standard author, title and some of the content. These extra pieces of information can be the amount of comments on the post, the amount of likes, the amount of shares, etc.

I chose to go for the comment count, although it could've been any of the other options. The reason I want to keep track of this extra information in the posts database in the first place is because I believe it would help with performance. If I were to calculate the amount of comments on a post every time the main page is loaded, it would be a waste of resources. Instead, I can keep track of this information in the database and update it whenever a comment is added or removed. This would be better since the main page is loaded more often than comments are added or removed.

## Flow

The flow that happens when a comment is added or removed is as follows:

1. The API-Gateway receives a request to add or remove a comment.
2. The API-Gateway redirects that request to the Comment-Service.
3. The Comment-Service adds or removes the comment from the database.
4. The Comment-Service sends a message to the messaging broker on the topic "comment-created".
5. The Post-Service receives the message from the messaging broker.
6. The Post-Service updates the comment count in the database.

In the future, were I to need to add more service that need to run some logic when a comment is added or removed, I can easily add another service that listens to the messaging broker to the same topic.

The flow described above can be seen in the following diagram:

![commentcount-flow-diagram.png](commentcount-flow-diagram.png)

## Implementation

To implement this flow, I had to add a few things to the Comment-Service and the Post-Service. The Comment-Service needed to send a message to the messaging broker, and the Post-Service needed to listen to that message.

### Comment-Service

In the Comment-Service, I added a new class called `KafkaProducer`. This class is responsible for sending messages to the messaging broker. The method that sends the message looks as follows:

```C#
public async Task SendMessage(string value, string topic)
{
    await _producer.ProduceAsync(topic, new Message<Null, string>()
    {
        Value = value
    });
    _logger.LogInformation($"Message sent to Kafka: {value} - on topic: {topic}");
}
```

This method takes a value and a topic as parameters. The value is the message that will be sent to the messaging broker, and the topic is the topic on which the message will be sent. In this case, the topic is "comment-created".

### Post-Service

In the Post-Service, I added a new class called `KafkaCommentPostConsumer`. This class is responsible for listening to messages from the messaging broker for this specific topic. The method that listens to the messages looks as follows:

```C#
    public Task StartAsync(CancellationToken cancellationToken)
    {
        _consumer.Subscribe("comment-created");
        _logger.LogInformation("Subscribed to comment-created...");

        Task.Run(() =>
        {
            while (!cancellationToken.IsCancellationRequested)
            {
                try
                {
                    var consumeResult = _consumer.Consume(cancellationToken);

                    var message = consumeResult.Message.Value;

                    _logger.LogInformation("Received message...");
                    _logger.LogInformation($"Updating CommentCount of post: {message}");
                    
                    _postService.IncrementCommentCount(message).Wait();
                    _logger.LogInformation($"Incremented CommentCount of post: {message}");
                }
                catch (OperationCanceledException ex)
                {
                    _logger.LogError($"Operation was cancelled: {ex.Message}");
                }
                catch (Exception ex)
                {
                    _logger.LogError($"Error processing Kafka message: {ex.Message}");
                }
            }
        });

        return Task.CompletedTask;
    }
```