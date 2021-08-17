# WISH@IETF111

## Logistics

### Time

FRIDAY, July 30, 2021 @ 1900-2100

### Meeting Information

Meetecho (a/v and chat):
https://meetings.conf.meetecho.com/ietf111/?group=wish&short=&item=1

Audio (only):
http://mp3.conf.meetecho.com/ietf111/wish/1.m3u

CodiMD (notes):
https://codimd.ietf.org/notes-ietf-111-wish

## Agenda

[Slides](https://github.com/wish-wg/wg-materials/blob/main/ietf111/wish%40ietf111.pdf)

### Administrivia - Chairs - 10min

- Virtual Meeting Tips
- Note Well
- Note Taker
- Jabber Scribe
- Status

### draft-murillo-whip - Sergio Murillo - 45min

* Presenting changes between different versions: from draft-00 to draft-02
* WHIP session setup: WHIP resource URL returned on the location header of the response
* Trickle ICE and ICE restart: use an HTTP Patch request to communicate new candidates with mime type
* Session termination
* Protocol extensions: way to extend the protocol. Optional for WHIP clients and servers.
* TURN/STUN for the WHIP client: out of scope.
* CORS handling: two ways permitted. 

### Discussion - All - 5min

For draft-murillo-whip:

Around Trickle ICE:

    Adam Roach: ok having them to a bundle? Less things to have to touch is for the better.

    Julius Chroboczek: very important work. 
    Three comments: 1. don't agree with the fact that the server is allowed to re-use trickle. Make things as easy as possible for the client: client should be able to find before hand. Any wish server should be able to restart with trickle. 
    Murillos agrees on this.
    Lorenz should be the one to ask.
    Limited number of servers we are hoping for, those are features that can be ignored by the client but not the server.
    2 point: must have a reference client (will be helpful to server authors), which is the one we should use?  Must have clear instructions. We need a reference.
    3 point: most of the server authors have a web client that works, which means that the incentive to implement is low. Which is the killer feature of this (to implement WISH)? Modern browsers don't implement screen sharing, so this could be the killer feature for ipads and tablets.

    Murrillo: 
    For second point: When draft is more aproved, planning to run an interop hackton.
    For thrid point: Agreed.

    Jonathan Lennox:
    Concerned that the spec is underspecified (trickle ice): issue with the server, must have a transactional model of ice restarts. The HTTP patch contains a port?

    Nils Ohlmeier: 
    Mentioned ICE consent to detect a connection is dead or not. ICE consent by deafult only timeouts after 30s. Is it good enough or too slow? It is not a keep-alive.

    Murillo:
    Might be ok.

    Adam Roach:
    Motivation? There is a motivation to be able to use the capabilities of broadcast tools.

    Timothy Panton:
    Not agreed with the ICE front. We will adopt NICER.

    Murillo:
    Should we wait to have a mature draft of NICER?

    Timothy:
    ICE is more complex. All of these things will get eventually fixed.

    Murillo:
    We can revisit. 

    Jonathan Lennox:
    Don't want to gate on NICER. 

    Harald Alvestrand:
    Speaking as the author of NICER draft, it is nice but it is likely to change. So don't depend on it. 

Around TURN/STUN for the WHIP client:

    Julius Chroboczek: the user interface I will like: a WISH URL and it works. Different URLS for ICE config and endpoint? Must give two URLs? Would you consider having the initial message and answer as it is now, but with an extra message: one TURN for you? Piggieback the ICE configuration to the existant protocol?

    Murrillo: Not possible

    Juliusz Chroboczek: Only knows the endpoint. Can't it discover the WISH config without any extra? Asking the user to configure multiple URLs is not good. Maybe as a JSON object?

    Adam Roach: Ok with TCP. It will be what Julius want. 

    Murrillo: agrees.

    Juliusz Chroboczek: webRTC chooses the ports randomly, which is denied sometimes by firewalls. Put the port that you know is unblocked. Automatic TURN configuration is something that will make WISH successful.

    Adam Roach: Hesistant to put it on a normative level (the draft on the TURN/STUN slide). Maybe move the contents of the draft to WISH.

    Murrillo: agrees.

    Jonathan Lennox: Proposal is similar: a pre-flight request without the content type. Should be simple.

    Justin Uberti: O-Auth mechanism for connecting to TURN servers. Principles are the same. Must the simplest to use. 

    Murillo: Is it implemented? Is it in popular servers or webRTC stack? 

    Justin Uberti: might be worth it.

Around CORS:

    Adam Roach: Do the way it is done in the web. If something else is needed, point to an specification. 

    Juliusz: WISH is not important from browsers. If it is something required by web standards, please explain to us. 

    Adam Roach: There a handful of web broadcast tools where it does not apply. Issue for web tools must be an addition to the document.

Sean: adopt to the WG? Support or not, let us know!

Poll raised: most of the group supported adoption. 
