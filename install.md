---
layout: default
title: Installing Spiel
---

Spiel consists of several components. You will need a speech provider, some voices that are supported by the provider, and of course an app that actually uses Spiel to do something useful.

We conviniently have all that set up as Flatpaks. Hopefully there will be a more streamlined way to install voices in the future, but for now you are a few commands away from a talking app.

To begin you will need to add the Spiel flatpak repository to your system:

```sh
flatpak remote-add --user spiel https://project-spiel.org/spiel-repo.flatpakrepo
```

## Installing a Speech Provider

Once you have the repository configured, you can install a speech provider:

```sh
flatpak install ai.piper.Speech.Provider
```

## Installing a Voice

The voices for each provider are bundled separately as Flatpak extensions. This allows user to choose which voices they want installed.

```sh
flatpak install ai.piper.Speech.Provider.Voice.en_US-amy-medium
```

## Installing a Talking Application

Now that you went through the effort of installing a Spiel voice, you need something to speak with it. Orca has [experimental Spiel support](https://www.freelists.org/post/orca/Orca-now-has-Spiel-support). You can also try a small demo app called Spiel It:

```sh
flatpak install org.project_spiel.SpielIt
```
