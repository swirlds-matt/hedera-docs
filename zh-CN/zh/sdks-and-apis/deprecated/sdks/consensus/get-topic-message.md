# Get topic messages

Subscribe to a topic ID's messages from a mirror node. You will receive all messages for the specified topic or within the defined start and end time.

**Query Fees**

* Please see the transaction and query [fees](../../../../networks/mainnet/fees/#transaction-and-query-fees) table for base transaction fee
* Please use the [Hedera fee estimator](https://hedera.com/fees) to estimate your query fee cost

{% tabs %}
{% tab title="V1" %}
| Constructor                       | Description                                      |
| --------------------------------- | ------------------------------------------------ |
| `new MirrorConsensusTopicQuery()` | Initializes the MirrorConsensusTopicQuery object |

```java
new MirrorConsensusTopicQuery()
```

#### Methods

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
      <code>subscribe(&lt;mirrorClient, onNext onError)</code>
    </td>
    
    <td>
      MirrorClient, Consumer \<MirrorConsensusTopicResponse>, Consumer\<Throwable>
    </td>
    
    <td>
      Subscribe and get the messages for a topic
    </td>
    
    <td>
      Required
    </td>
  </tr>
</table>

{% code title="Java" %}
```java
new MirrorConsensusTopicQuery()
    .setTopicId(topicId)
    .subscribe(mirrorClient, resp -> {
                String messageAsString = new String(resp.message, StandardCharsets.UTF_8);

                System.out.println(resp.consensusTimestamp + " received topic message: " + messageAsString);
            },
            // On gRPC error, print the stack trace
            Throwable::printStackTrace);
//v1.3.2
```
{% endcode %}

{% code title="JavaScript" %}
```javascript
new MirrorConsensusTopicQuery()
    .setTopicId(topicId)
    .subscribe(
        consensusClient,
        (message) => console.log(message.toString()),
        (error) => console.log(`Error: ${error}`)
    );
```
{% endcode %}
{% endtab %}
{% endtabs %}
