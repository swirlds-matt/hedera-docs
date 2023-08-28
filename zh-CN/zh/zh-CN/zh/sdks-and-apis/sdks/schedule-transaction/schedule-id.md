# Schedule ID

The entity ID of a schedule transaction.

A `ScheduleId` is composed of a \<shardNum>.\<realmNum>.\<scheduleNum> (eg. 0.0.10).

* **shardNum** represents the shard number (`shardId`). It will default to 0 today, as Hedera only performs in one shard. It will default to 0 today, as Hedera only performs in one shard.
* **realmNum** represents the realm number (`realmId`). It will default to 0 today, as realms are not yet supported. It will default to 0 today, as realms are not yet supported.
* **scheduleNum** represents the schedule number (`scheduleId`)

Together these values make up your `ScheduleId`. Together these values make up your `ScheduleId`. When a `ScheduleId` is requested in a field, be sure enter all three values.

| **Constructor**                                                         |     **Type**     | **Description**                                                                                  |
| ----------------------------------------------------------------------- |:----------------:| ------------------------------------------------------------------------------------------------ |
| `new ScheduleId(<shardNum>,<realmNum>,<scheduleNum>)` | long, long, long | Constructs a `ScheduleId` with 0 for `shardNum` and `realmNum` (e.g., `0.0.<scheduleNum>`) |

### Example

Update the custom fees for a given token. If the token does not have a fee schedule, the network response returned will be `CUSTOM_SCHEDULE_ALREADY_HAS_NO_FEES`. You will need to sign the transaction with the fee schedule key to update the fee schedule for the token. If you do not have a fee schedule key set for the token, you will not be able to update the fee schedule.
{% tab title="Java" %}
```java
ScheduleId scheduleID = new ScheduleId(0,0,10); 
System.out.println(scheduleID)
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const scheduleID = new ScheduleId(0,0,10); 
console.log(scheduleID)
```
{% endtab %}
{% endtabs %}
