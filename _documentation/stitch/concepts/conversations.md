---
title: Conversations
description: Introduction to the concept of Conversations in Stitch.
navigation_weight: 3
---

# Conversations

A conversation is a shared core component that Nexmo APIs rely on. Conversations happen over multiple mediums and and can have associated Users through Memberships.

The Conversation is key to understanding Stitch. In order for users to communicate, they must connect to a Conversation, at which point they become Members of that Conversation. A Conversation is capable of supporting chat, audio or video (which includes audio). For textual communication a Conversation can be thought of as chat room. Users can be invited to join a Conversation and they can leave a conversation. Users can also join multiple Conversations. 

## Creating a Conversation

You can create a Conversation via the command line:

``` shell
$ nexmo conversation:create display_name="Nexmo Chat"
```

Returns a unique ID for the Conversation:

```
CON-35663e9d-687f-4e5c-bd37-91837294bd76
```

## Add a user to a Conversation

To add a User into a Conversation using the command line:

``` shell
$ nexmo member:add YOUR_CONVERSATION_ID action=join channel='{"type":"app"}' user_id=YOUR_USER_ID
```

## List Users associated with a Conversation

You can also list Users associated with a conversation via the command line:

``` shell
$ nexmo member:list CON-35663e9d-687f-4e5c-bd37-91837294bd76 -v
```

Returns something similar to:

``` shell
name  | user_id                                  | user_name | state  
----------------------------------------------------------------------
Tony  | USR-fa1acfbb-0087-4fa5-911d-045586470edc | Tony      | JOINED
alice | USR-cf0e4ff6-fd86-4d4a-9a90-282a59593480 | alice     | INVITED
```

## Getting a list of Conversations

You can use the REST API to get a list of Conversations:

``` shell
$ curl "https://api.nexmo.com/beta/conversations" \
     -H 'Authorization: Bearer eyJ0eXAiOiJKV1Q....VpwfOz2KGyJ14oL6gqCPfX24Mg' \
     -H 'Content-Type: application/json'
```
