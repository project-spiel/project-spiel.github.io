---
layout: default
title: Create A Speech Provider
---

Code speaks best, so lets dive right in and create a fully functional speech provider that uses the `espeak-ng` command line progeam you probably already have installed.

```python
from dasbus.loop import EventLoop
from dasbus.connection import SessionMessageBus
from dasbus.unix import GLibServerUnix
from dasbus.server.interface import dbus_interface
from dasbus.typing import UnixFD, Str, Double, Bool, List, Tuple, UInt64
import os, subprocess

import gi

gi.require_version("SpeechProvider", "1.0")
from gi.repository import SpeechProvider


@dbus_interface("org.freedesktop.Speech.Provider")
class ExampleProvider(object):
    @property
    def Name(self) -> Str:
        """Return the human-readable name of the speech provider"""
        return "Example"

    @property
    def Voices(self) -> List[Tuple[Str, Str, Str, UInt64, List[Str]]]:
        """Return a list of available voices, their audio output format, features and supported languages"""
        # This audio format specifier is ultimately parsed by gstreamer.
        audio_format = "audio/x-raw,format=S16LE,channels=1,rate=22000"
        # eSpeakNG supports a subset of SSML, advertise which SSML features are supported for each voice.
        features = (
            SpeechProvider.VoiceFeature.SSML_SAY_AS_TELEPHONE
            | SpeechProvider.VoiceFeature.SSML_SAY_AS_CHARACTERS
            | SpeechProvider.VoiceFeature.SSML_SAY_AS_CHARACTERS_GLYPHS
            | SpeechProvider.VoiceFeature.SSML_BREAK
            | SpeechProvider.VoiceFeature.SSML_SUB
            | SpeechProvider.VoiceFeature.SSML_EMPHASIS
            | SpeechProvider.VoiceFeature.SSML_PROSODY
            | SpeechProvider.VoiceFeature.SSML_SENTENCE_PARAGRAPH
        )

        return [
            ["Charlie", "gmw/en-US", audio_format, features, ["en-US"]],
            ["Dominique", "roa/fr", audio_format, features, ["fr-FR"]],
        ]

    def Synthesize(
        self,
        fd: UnixFD,
        utterance: Str,
        voice_id: Str,
        pitch: Double,
        rate: Double,
        is_ssml: Bool,
        _language: Str,
    ):
        """Synthesize speech using the espeak-ng command and pipe output to provided file descriptor"""
        # The passed voice_id should be understood by espeak-ng
        cmd = ["espeak-ng", "--stdout", "-v", voice_id]
        # We get the pitch as a multiplier, so multiply it by the default espeak-ng value
        cmd += ["-p", str(pitch * 50)]
        # We get the rate as a multiplier, so multiply it by the default espeak-ng value
        cmd += ["-s", str(rate * 175)]
        if is_ssml:
            # The given utterance is indicated as ssml, so set the espeak-ng flag accordingly.
            cmd += ["-m"]
        cmd += [utterance]

        # Write the output to the fd, close it, and return.
        subprocess.run(cmd, stdout=fd)
        os.close(fd)


if __name__ == "__main__":
    mainloop = EventLoop()
    bus = SessionMessageBus()
    bus.publish_object(
        "/com/example/Speech/Provider",
        ExampleProvider(),
        server=GLibServerUnix,
    )
    bus.register_service("com.example.Speech.Provider")

    mainloop.run()
```

## Speech Events

The example above does not showcase a cool capability: speech events. Clients benefit from knowing what part of the given utterance's text is being spoken. This is achieved by sending audio frames interleaveed with events. The [StreamWriter class](https://project-spiel.org/libspeechprovider/class.StreamWriter.html) is used for this. And example of this being used can be seen in the [eSpeakNG speech provider](https://github.com/project-spiel/speech-provider-espeak/blob/main/src/main.rs).
