# Using advanced Out of Band features

> This section assumes the following items:
>
> 1. A [valid enviroment](../getting-started/installation) for development
>
> 1. A basic understanding of how to create an agent and make a connection

### Shortened URLs

The first topic to discuss is that of using a shortened URL to encode an invitation. This can be done for many purposes but mostly because the length of encoded URL's with complex invitations can quickly become very large. 
The finer details of why URL shortening might want to be done and how it should be done can be found here [`Out of Band RFC`](https://github.com/hyperledger/aries-rfcs/tree/main/features/0434-outofband#url-shortening)

The primary way to handle a invitation from a shortened URL is to use the same method as receiving an invitation from an URL

```typescript showLineNumbers 
const receiveInvitation = async (agent: Agent invitationUrl: string) => {
  const { outOfBandRecord } = await agent.oob.receiveInvitationFromUrl(invitationUrl)

    return outOfBandRecord
}
```
This method has the functionality built in to check whether or not the provided URL is a shortened URL and process it accordingly, or in the case that it is a normal URL to still process it correctly. Notably this function also supports processing of legacy `connectionInvitationMessage` as part of the Out of Band module inside of AFJ.

Within this same module there are two other methods that relate to shortened URLs. `ParseInvitation(string: URL)` and `async parseInvitationFromUrl(string: URL)`. Both of these function return an `OutOfBandInvitation`. The first method will only return if the provided URL is NOT a shortened URL, otherwise it will throw an error, the second method will process either a shortened URL or a normal URL encoded invitation. Finally it should be noted that a badly formatted invitation will cause all of these methods to throw errors.

### Connection Reuse

The second advanced topic is that of connection reuse. This is is done through `recieveInvitation()` and `recieveInvitationFromUrl()` in the `agent.oob` module. This is done by including a config object that contains a field for connection reuse, and by insureing the DID that is being connected to is a public DID, more on this later.
```typescript showLineNumbers
    const config = {
        reuseConnection: True,
    }
    const invitation = await recieveInvitationFromUrl('exampleURL', config)
```
Passing a valid config object with the `reuseConnection` property set to true will cause AFJ to reuse connections internally when accepting a provided invitation from an existing connection. 

NOTE: the config object does not need to be fully constructed with all fields available but it should be noted that additional fields exist providing more functionality.

### Conecting with a Public DID

Lastly we will talk about how to connect using a public DID. This is another feature that is handled internally in AFJ, suffice it to say that as long as the DID type that is being used to connect is supported by AFJ it will be able to make a connection when it. 

The two public DIDs that are supported by AFJ are DID:sov and DID:web. Currently AFJ does not have a way to create either of these public DIDs and they must be obtained elsewhere.