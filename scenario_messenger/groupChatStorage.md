- [One-to-One recent contact storage](#one-to-one-recent-contact-storage)
  - [Storage requirements](#storage-requirements)
  - [Initial schema](#initial-schema)
  - [Improved schema: Decouple msg content from sender and receiver](#improved-schema-decouple-msg-content-from-sender-and-receiver)
- [Group recent contact storage](#group-recent-contact-storage)
  - [Storage requirements](#storage-requirements-1)
  - [Storage model](#storage-model)
  - [Read amplification](#read-amplification)
  - [Write amplication](#write-amplication)
    - [Improve by only storing message id](#improve-by-only-storing-message-id)
    - [Improve by only storing last message id](#improve-by-only-storing-last-message-id)

# One-to-One recent contact storage
## Storage requirements
* Requirement1: Query all 1-on-1 conversations a user participates in after a given timestamp.
* Requirement2: For each conversation, load all messages within that conversation created later than a given timestamp.

## Initial schema

![](../.gitbook/assets/im_groupchat_recentContact_one_to_one.png)

## Improved schema: Decouple msg content from sender and receiver
* Intuition:
  * Even if sender A deletes the message on his machine, the receiver B should still be able to see it
  * Create a message\_content table and message\_index table

![](../.gitbook/assets/im_groupchat_recentContact_1to1_decouple.png)

# Group recent contact storage
## Storage requirements
* Requirement1: Query all group conversations a user participates in after a given timestamp.
* Requirement2: For each conversation, load all messages within that conversation created later than a given timestamp.
* Requirement3: For each conversation, load all participates inside it. 

## Storage model
* In group chat scenario (Except for those extremely big 100K+ groups -_-), typically read write ratio will be 100:1. So the storage model should better balance this factor to something like 50:50 or more evenly. So typically write amplification is used. 

![](../.gitbook/assets/im_groupchat_recentContact_storageModel.png)

## Read amplification

![](../.gitbook/assets/im_groupchat_recentContact_group.png)

## Write amplication

![](../.gitbook/assets/im_groupchat_recentContact_group_message_primarykey.png)

### Improve by only storing message id

![](../.gitbook/assets/im_groupchat_recentContact_group_last_message_id.png)

### Improve by only storing last message id

![](../.gitbook/assets/im_groupchat_recentContact_group_onlyStore_last_message_id.png)