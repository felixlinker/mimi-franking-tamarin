# MIMI Message Franking - Preliminary Analysis

This repo contains a preliminary, formal analysis of the message franking mechanism as specified in the [MIMI protocol](https://ietf-wg-mimi.github.io/mimi-protocol/draft-ietf-mimi-protocol.html#name-message-franking).
The model is modelled for the Tamarin prover.

Assumptions and abstractions:
  - Participants have a secure channel to the hub.
    The channel is authenticated and provides replay protection.
    No encryption of the MIMI protocol is modelled.
  - The adversary has access to all messages sent to a room.
  - Any participant may become compromised.
    When that happens, the adversary can send messages in that participant's name.
  - The hub key and room secrets are modelled as long-term secrets.

Under the assumptions from above, I was able to prove a "correctness" property.
That is, whenever a hub receives an abuse report and successfully verifies it, the accused sender did indeed send the respective message.
