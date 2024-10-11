---
layout: home
title: A Speech Framework for the Free Desktop and Beyond
---

<section markdown=1>

<article markdown=1>

## Make Your App Speak

### Familiar API
The libspiel client library offers an ergonomic speech interface that is inspired by the standard Web Speech API. The library is available to many languages through GObject Introspection. Any app can start speaking with minimal effort.

### Integrated with GStreamer
The speech generated with the client library will automatically be sent to the default audio device. It is also possible to construct your own GStreamer pipeline for custom use cases.

### Sensible defaults and easy configurability
libspiel uses global settings a user may set per-language to determine which voice to use for each utterance. Applications also have finer granularity and use their own settings or heuristics to choose a proper voice.

</article>

<article markdown=1>

## Provide a Voice

### Simple Interface
Creating a speech provider is easy. Write a D-Bus service with a single `Synthesize()` method and a few properties. The synthesized audio frames are written to a file descriptor, and Spiel does the rest.

### Discoverable
By using a well-known suffix in the D-Bus service name, the speech provider automatically becomes available to consumer applications. The service can be automatically spawned and does not need to be running in order to be discovered.

### Easy Distribution
Use Flatpak or Snap to give a one-click install for users. The speech provider can be fully contained and hide any complexity from host systems.

</article>

</section>

<iframe width="560" height="315" src="https://www.youtube.com/embed/xseIsaxrlXo?si=QdBWK0Z4E7vRfUTT" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" style="margin: 1em auto; display: block;" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
