# Messaging

Messaging is a way for backend services to communicate with each other. There are many patterns to choose from (some of which I mention in my research docucment called `Research Messaging`), but in this case I will be using the `Publish/Subscribe` pattern. This pattern is used when you want to broadcast messages to multiple consumers. This is useful when you have multiple services that need to know about a certain event.

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