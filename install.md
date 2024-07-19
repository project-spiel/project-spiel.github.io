---
layout: default
title: Installing Spiel
---

Spiel consists of several components. You will need a speech provider, some voices that are supported by the provider, and of course an app that actually uses Spiel to do something useful.

We conviniently have all that set up as Flatpaks. You are jusr a few steps away from a talking app.

## Spiel Installer

<picture class="screenshot">
  <!-- User prefers light mode: -->
  <source srcset="https://raw.githubusercontent.com/project-spiel/spiel-installer/main/data/screenshot-light.png" media="(prefers-color-scheme: light)"/>

  <!-- User prefers dark mode: -->
  <source srcset="https://raw.githubusercontent.com/project-spiel/spiel-installer/main/data/screenshot-dark.png"  media="(prefers-color-scheme: dark)"/>

  <!-- User prefers high contrast: -->
  <source srcset="https://raw.githubusercontent.com/project-spiel/spiel-installer/main/data/screenshot-hc.png"  media="(prefers-contrast: more)"/>

  <!-- User has no color preference: -->
  <img src="https://raw.githubusercontent.com/project-spiel/spiel-installer/main/data/screenshot-light.png"/>
</picture>

The process of retrieving the voice packages and the speech provider packages they rely on is simplified with a voice installer application. This application will retrieve all the Flatpak dependencies and restart any running speech providers so the voices will be available immediately.

Install the [Spiel Installer Flatpak](https://project-spiel.org/flatpaks/spiel-installer.flatpakref) and browse for the voices you want.

You can filter voices by language or speech provider, you can then install or remove voice packages.

## Command Line Installation

You can also install Spiel speech providers and their voices with a few Flatpak commands.

### Add the Spiel Flatpak Repository

To begin you will need to add the Spiel flatpak repository to your system:

```sh
flatpak remote-add --user spiel https://project-spiel.org/flatpaks/spiel-repo.flatpakrepo
```

### Installing a Speech Provider

Once you have the repository configured, you can install a speech provider:

```sh
flatpak install ai.piper.Speech.Provider
```

### Installing a Voice

The voices for each provider are bundled separately as Flatpak extensions. This allows user to choose which voices they want installed.

```sh
flatpak install ai.piper.Speech.Provider.Voice.en_US-amy-medium
```

### Installing a Talking Application

Now that you went through the effort of installing a Spiel voice, you need something to speak with it. Orca has [experimental Spiel support](https://gitlab.gnome.org/GNOME/orca/#experimental-features). You can also try a small demo app called Spiel It:

```sh
flatpak install org.project_spiel.SpielIt
```
