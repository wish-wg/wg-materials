WISH 112 Meeting
Friday, Nov 12, 2021
---

# Administrivia / Agenda
* Tim Panton: In addition to the other implementation reports, I have some experience I can contribute

# draft-ietf-wish-whip-01 status
* Sergio goes over updates from wish-whip-00 (identical to murillo-whip-02) -- see slides for details
* Cullen Jennings: When we get PATCH with new SDP, you can basically ignore the contents?
    * Sergio: If you have ICE light...
    * Cullen: I'm not talking about ICE light. When you get a PATCH with SDP, are you required to handle it?
    * Sergio: The PATCH can only contain ICE-related information, e.g. for trickle or restart
    * Cullen: Okay, so it can only be used for those two cases. That makes more sense.
* Adam Roach: Redirection can't work on the initial URL or the URL you get back from it?
    * Sergio: We found that some client libraries don't handle the 307 response to POST correctly, which means this needs to be handled by the client, adding complexity.
    * Adam: Okay, I'll have to think a bit more about this.

## Discussion of sdpfrag (from RFC 8840)
* There was a brief discussion about whether it makes sense to modify sdpfrag to send candidates at the session level (the format doesn't currently allow this)
* Additional conversation about whether we need trickle ICE at all
* Martin: If you want to move candidates to the session level, then define a new thing that does that
* Martin: With ICE restart, you generally have a new offer/answer exchange, and it seems you can do the same thing here to avoid PATCH in that context. If you can't, then define a new format rather than extending RFC 8840
* Sergio: To do an ice restart, a full O/A exchange doesn't add anything. All that needs to happen is updating the username/password on the ice candidates
* Martin: Okay, I just want to make sure that the changes from other approaches are well justified. If you can't use 8840 as-is, then define your own rather than extending it.
* Sergio: Currently, the draft uses 8840 as-is. We're proposing a minor tweak.
* Martin: okay, if you want to tweak it, then define a new format instead of changing 8840.
* Cullen: There's two paths here -- either use 8840 as-is, OR define a new format. I don't prefer one over the other, but those are the options.
* Tim: I think SDP fragments are the wrong approach. Our experience is that sending a diff makes more sense and is easier to implement. We should be defining our own format that looks more like what others
* Adam: Want to clarify that removing trickle ICE from the architecture, as Cullen seemed to be suggesting, makes the solution unsuitable for certain uses -- we need it for starting broadcasts as quickly as possible
* Juliusz: Agree that we need trickle. Separately, I believe that we need an indication at a layer much higher than the SDP parsing layer to indicate the difference between a trickled candidate and an ICE restart
    * Sergio: Currently, we're using the same signaling for these because RFC 8840 makes that easy. This is also related to request ordering.
    * Juliusz: Adding this information to the request at the HTTP layer is no extra work for the client, and makes the server's work much easier. Concede that it might not solve all problems, but it solves this one.
* Adam: Regarding ordering, HTTP does have the etag mechanism to address this -- we should just use this.

## TURN/STUN Configuration
* Martin: This seems like a reasonable approach.
* Proposal to remove OPTIONS from the TURN server configuration
    * Lorenzo: This doesn't work today -- I use gstreamer, and can't wait for this patch to deploy WHIP.
    * Sergio: What if we made returning credentials in the OPTIONS optional?
    * Adam: +1 to Lorenzo; also, we need to keep web browsers in mind, which may not support this any time soon. Finally, servers can pretty reasonably cache the credentials to avoid having to fetch them twice.

# Hackathon Interop Results
* Lorenzo summarizes hackathon results -- see slides for details
* Juliusz summarizes experience implementing WHIP in Galene -- see slides for details