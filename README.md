# MIMI Message Franking - Preliminary Analysis

This repository contains a preliminary, formal analysis of the message franking mechanism as specified in the [MIMI protocol](https://ietf-wg-mimi.github.io/mimi-protocol/draft-ietf-mimi-protocol.html#name-message-franking).
The model is modelled for the Tamarin prover.

Assumptions and abstractions:
  - Participants have an authenticated channel to the hub.
    No encryption of the MIMI protocol is modelled.
  - The adversary has access to all messages sent in a room.
  - Any participant may become compromised.
    When that happens, the adversary can send messages in that participant's name.
  - The hub key and room secrets are modelled as long-term secrets.

Under the assumptions from above, I was able to prove a "correctness" property.
That is, whenever a hub receives an abuse report and successfully verifies it, and at least one honest client has processed the message, the accused sender did indeed send the respective message.

The assumption on honest-client processing is required because the following can happen.
1. A malicious client submits a message `m1` to the hub with a franking tag for message `m2`.
2. The hub accepts the message and relays it to clients because it does not verify the franking tag.
3. Honest clients will reject the message because the they cannot verify the franking tag.
4. Now, the initial, malicious client can be accused of having sent `m2`, which they have not.
They have sent `m1`.
This way, a malicious client can try denying having sent a message they were accused of having sent, but they can only do so by claiming they did not follow the protocol.

The above does not seem to be desirable, but also not a major concern.
