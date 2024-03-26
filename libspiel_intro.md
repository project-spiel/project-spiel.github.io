---
layout: default
title: Getting Started with Talking Apps
---

Spiel provides a client library that is supported in any language that can use GObject Introspection.

```python
>>> import gi
>>> gi.require_version("Spiel", "1.0")
>>> from gi.repository import Spiel
>>> speaker = Spiel.Speaker.new_sync()
```

The client library automatically discovers all the speech providers installed in the session and makes them available.

```python
>>> for provider in speaker.get_providers():
...     print([provider.get_well_known_name(), provider.get_name()])
... 
['ai.mimic3.Speech.Provider', 'Mimic 3']
['ai.piper.Speech.Provider', 'Piper']
>>>
```

Available voices can be retrieved from their provider, or a collated list of all voices from the `Speaker` object.

```python
>>> for provider in speaker.get_providers():
...     voice = provider.get_voices()[0]
...     print([voice.get_name(), voice.get_languages()])
... 
['apope_low - English (United Kingdom)', ['en-GB']]
['lessac (English)', ['en-US']]
>>>
>>> print(["Total available voices", len(speaker.get_voices())])
['Total available voices', 7]
```

In order to synthesize speech, an event loop is needed. Here is a complete Hello World example:

```python
import gi
gi.require_version("Spiel", "1.0")
from gi.repository import GLib, Spiel

loop = GLib.MainLoop()

def _notify_speaking_cb(synth, val):
    if not synth.props.speaking:
        loop.quit()

speaker = Spiel.Speaker.new_sync()
speaker.connect("notify::speaking", _notify_speaking_cb)

utterances = [
  Spiel.Utterance(text="Hello world.", language="en"),
  Spiel.Utterance(text="Bonjour le monde.", language="fr"),
  Spiel.Utterance(text="مرحبا بالعالم", language="ar")
]

for utterance in utterances:
  speaker.speak(utterance)

loop.run()
```

For a full review of features, please review libspiel's [API reference](/libspiel).