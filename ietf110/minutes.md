# Wish IETF 110

Note taker: tim Panton
Reviewed by: Dr. Alex
TUESDAY, March 9, 2021 @ 1300-1500

# Administrivia (10 mn)

Chairs (10 mn)

what’s in/out of scope ()

Scope is Narrow

No modifications to WebRTC (or go to RTCWeb WG) esp. encryption (DTLS-SRTP) and ICE.

No video conferencing (2 ways)

No media (signalling)

Anything not in scope should go to DISPATCH

Actions Items from charter discussion

Dispatch highlighted extensibility need.

# WHIP Presentation: Original Proposal (Sergio M.)

Problem to solve

WebRTC is best media transport for low latency streaming ingest but others
protocols are currently used, e.g. RTMP because there is no standard for webRTC
signalling.

Cullen: Is transcoding at middlebox inscope?

Sergio: Not forbidden but SVC and Simulcast somewhat meet the requirement.

Cullen: SVC and Simulcast not yet RFC’d

Harald: Simulcast is now covered by recent RFCs

Adam : SVC and Simulcast isn’t manditory?

**Action Item-> Raise the question on list, for clarification.

Requirements:

Simple c.f. RTMP URI scheme

Specificly authenticated ingest from browsers and native HW Encoders.

No requirement for reneg.

Juliusz: ICE restarts needed in accademic networks. Use case: Online course at
university over university network. Media is one-way, one to many, and feedback
is achieved through chat. Practically: we can see up to 5 diferent IPs over the
lifetime of a student session, with WS session otherwise stable. We need to
reconnect!

**Action Item-> Raise the question of ICE restart specifically on the list.

Proposed solution:

HTTP post to intiate

DTLS/ICE to manage states

Standard HTTP auth

Load balance with standars HTTP load balanced.

Adam: Will discuss Auth. later in this IETF session.

Cullen: Do we need ICE? Discuss later.

Cullen: WebRTC compatibility - We may need extensions.

Harald: Needs Browser interop for success

Juliusz: Is signalling over websockets in scope?

Chair: websockets not in scope of charter.

Adam: Existing arch use REST.

Mike: - Can we discuss websockets? Incremental complexity isn’t big.

Tim: Make it more like RTMP

Sergio: +1 “more like RTMP”

**Action Item -> Discussion on mailing list. Articulate reasons for ruling out
websockets.

# Extensions Proposal (Adam R.)

Proposed Additonal Requirements:

Session management

Extensibilty

Heartbeats

2-ways

ICE management

Media Types

Constraints: BCP 56 + BCP 190 use of HTTP limit some of the ‘easy’ solutions

## Session Management:

There was consensus @ Dispatch that this was needed.

One of the obvious featuer impacted is load balancing.

Proposal:

Option 1: POST returns 201 to a resource you can DELETE to end the session

Option 2: POST with offer gets 200 with answer

Cullen: we should do what people use - REST.

Tim/Sergio: Choice depends on other decisions

## Extensibility:

There should be CORE set of feature, which are both necessary and critical, and
allow interopeability in all cases.

Then there should be Extention Points and an Extention mechanism. Extensions
should be pure enhancements.

Endpoint should be able to just ignore unknown extensions.

## Heartbeats:

Signalling path vs media path split. We need the media path to tell the
signalling path when it’s down.

HeartBeat and Websocket

Sergio: it only makes sense if you expect those to be different boxes. Some
SFUs (medooze, mediasoup, … ), combine those functionalities.

Chair: If you really have to have an heartbeat, Could websockets function as a signalling
layer heartbeat?

Adam: I’ll think about that.

**Action Item -> Adam to come back on ws/heartbeat.

Need of an additionnal Heartbeat?

Tim : not convinced about benefit of heartbeat at signalling layer. People
close their browsersm HB or WS would not help.

Juilisz: Detecting session
dropping is very useful in accademic environment.

Sergio: We already have a
media time out, and ICE disconnect anyhow, what is the benefit of an
additionnal layer of heartbeat?

Media session surviving signalling session?

Sergio: In production, we have an heartbeat on signalling, and sometimes, the
signalling path is down, but the media path is ok, and we ended up tearing the
media path down unecessarily by coupling both.

Adam: Proposal for Additional
requirement that the media session can survive the failure of the signalling
layer (if there is one).

**Action Item -> To add en entry for additionnal requirement

What is the signalling for?

Cullen: What is a signalling channel for ?

Adam: Comming in next few slides.

Adam: Server should be able to terminate a session if

heartbeat stops

heartbeat long poll with reply

Websocket or other channel

Cullen: Do we need ‘mid call signalling’?

Mike: Semantics first ? Perhaps look at status Quo? how much can we trim
complexity.

As Simple as RTMP

Tim: WebRTC puts off RTMP developers - for me that should be an audience.

Sergio: Yes, We cant be more complex than RTMP for the same use-case.

Mike: We do RTMP ingest and broadcast to webRTC - we would like to do WebRTC
ingest, but ffmpeg doesn’t do that.

MPEG-DASH?

Juliusz: 1 to many usecase video with chat back channel. What is our added
value over MPEG DASH?

Adam: difference No RTCP in MPEG DASH

Mike: Simple: Lower latency.

## 2-ways

Cullen: Notifications are not good over HTTP.

ICE management

Full ICE, ICE lite, trickly ICE?

Adam: Ingest behind NAT is likely requirement. Trickle ICE is needed.

Cullen: ICE has cost webRTC adoption. AWS services know their NAT. ICE lite
will be sufficient and easier to debug.

Tim: I don’t believe Trickle ICE is
needed. I don’t beleive TURN TLS plays to our benefits.

Sergio: In the cases we
are using TLS, maybe we should just use RTMP instead.

Luke: Don’t need Trickle
ICE - reuse the existing HTTP as TURN TCP ?

Lorenzo: Trickle not needed but
nice option because stats.

## ICE Restart

Cullen: ICE restart ? Will ‘reconnect’ do enough?

Jonathan: Ice Restart question. WHat is the story, what is the use case? We
need to look at failover cases to understand whether ICE restart is needed in
the first place.

Tim: 201 would give us a resource to reconnect so full restart
isn’t needed. some state can be preserved.

Juliusz: Accademic Networks and 4G are fragile. In practice - fall back to TCP
is useful. ICE restarts work.

**Action Item-> ** Add a Sub 2sec reconnect requirement.

## Media Types

Adam: How many streams? is 1 audio and 1 video suficient?

Tim: Ordinality is not our problem

Harald: Server whould reject anything it doesn’t want.

Juliusz: Multiple video streams are needed.

Adam: How does the ingest network know which stream is which?

Luke: Timed MetaData stream would be nice.

Sergio: Existing WebRTC’s SDP can support these requirements (many streams,
data, …).

Cullen: semantic taging streams is needed. Harald: Are we defining a
service or a component.

Adam: A service. Tim/HTA:We need to separate the
application/service use case and WISH use case. Example: 360 video, for WISH it
is just a video, the fact that the content needs to be interpreted and rendered
differentenly than your usual 2D+time video is not WHISH problem.

Sergio: We
have audio streams with different channels per language. It is not WISH nor
WebRTC problem to know that, it s up to the application to know and deal with
it. One way is tagging streams, another way is single ingest URL per media type
(like YT handles 360).
