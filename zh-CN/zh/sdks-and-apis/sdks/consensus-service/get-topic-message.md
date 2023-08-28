# Get topic messages

Subscribe to a topic ID's messages from a mirror node. You will receive all messages for the specified topic or within the defined start and end time.

**Query Fees**

* Please see the transaction and query [fees](../../../networks/mainnet/fees/#transaction-and-query-fees) table for the base transaction fee
* Please use the [Hedera fee estimator](https://hedera.com/fees) to estimate your query fee cost

### Methods

<table spaces-before="0">
  <tr>
    <th>
      Method
    </th>
    
    <th>
      Type
    </th>
    
    <th>
      Description
    </th>
    
    <th>
      Requirement
    </th>
  </tr>
  
  <tr>
    <td>
      <code>setTopicId(&lt;topicId&gt;)</code>
    </td>
    
    <td>
      TopicId
    </td>
    
    <td>
      The topic ID to subscribe to
    </td>
    
    <td>
      Required
    </td>
  </tr>
  
  <tr>
    <td>
      <code>setStartTime(&lt;startTime&gt;)</code>
    </td>
    
    <td>
      Instant
    </td>
    
    <td>
      The time to start subscribing to a topic's messages
    </td>
    
    <td>
      Optional
    </td>
  </tr>
  
  <tr>
    <td>
      <code>setEndTime(&lt;endTime&gt;)</code>
    </td>
    
    <td>
      Instant
    </td>
    
    <td>
      The time to stop subscribing to a topic's messages
    </td>
    
    <td>
      Optional
    </td>
  </tr>
  
  <tr>
    <td>
      <code>setLimit(&lt;limit&gt;)</code>
    </td>
    
    <td>
      long
    </td>
    
    <td>
      The number of messages to return
    </td>
    
    <td>
      Optional
    </td>
  </tr>
  
  <tr>
    <td>
      <code>subscribe(&lt;client, onNext)</code>
    </td>
    
    <td>
      SubscriptionHandle
    </td>
    
    <td>
      Client, Consumer\<TopicMessage>
    </td>
    
    <td>
      Required
    </td>
  </tr>
</table>

{% tabs %}
{% tab title="Java" %}
```java
//Create the query
new TopicMessageQuery()
    .setTopicId(newTopicId)
    .subscribe(client, topicMessage -> {
        System.out.println("at " + topicMessage.consensusTimestamp + " ( seq = " + topicMessage.sequenceNumber + " ) received topic message of " + topicMessage.contents.length + " bytes");
    });

//v2.0.0
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
//Create the query
new TopicMessageQuery()
        .setTopicId(topicId)
        .setStartTime(0)
        .subscribe(
            client,
            (message) => console.log(Buffer.from(message.contents, "utf8").toString())
        );
//v2.0.0
```
{% endtab %}

{% tab title="Go" %}
```java
//Create the query
_, err = hedera.NewTopicMessageQuery().
    SetTopicID(topicID).
    Subscribe(client, func(message hedera.TopicMessage) {
        if string(message.Contents) == content {
        wait = false
    }
})

if err != nil {
    panic(err)
}

//v2.0.0
```
{% endtab %}
{% endtabs %}
