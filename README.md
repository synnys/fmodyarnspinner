🎭 FMOD & Yarn Spinner Integration for Unity

This repository provides a fully integrated FMOD & Yarn Spinner system for interactive dialogues with dynamic audio in Unity.
It supports multi-language voiceovers, FMOD event triggering, and camera-based audio listeners for a more immersive experience.

🚀 Features

FMOD Event Mapping to Yarn Lines

Assign English (EN) and Alternative (ALT) FMOD events to Yarn dialogue lines.

Automatically extracts Line IDs from Yarn scripts.

Dialogue-Driven FMOD Sound Triggering

Play FMOD events from Yarn scripts.

Adjust FMOD parameters directly from Yarn commands.

Multi-Language Support

Toggle between English & an alternative language with a UI button.

Each Yarn line can have a separate FMOD event for each language.

FMOD Camera-Based Listeners

Integrates with Cinemachine virtual cameras.

Ensures audio perception follows active cameras.

Fully Modular Setup

Drag-and-drop Yarn scripts into the Inspector.

Automatically populates Line IDs for easy FMOD event assignment.

📦 Installation & Setup

1️⃣ Import the Package

Clone this repository or download the Unity package.

Import it into your Unity project.

Ensure FMOD Unity Integration and Yarn Spinner are installed.

2️⃣ Set Up the Dialogue System

Attach Scripts to the Dialogue Runner

To centralize all functionality, attach these scripts to the Yarn Spinner Dialogue Runner:

FMODLineProvider.cs

FMODDialogueView.cs

YarnFMODTrigger.cs

FMODLanguageToggle.cs

FMODCameraListenerManager.cs (Attach to AudioManager GameObject)

3️⃣ Configure FMOD Event Assignments

Add the FMODLineProvider component to the Dialogue Runner.

Drag your Yarn script(s) into the Inspector under Yarn Script.

Click Update Lines to extract Line IDs.

Assign FMOD Events for English & ALT languages.

Toggle between EN/ALT using the UI button.

4️⃣ Enable FMOD Audio for Dialogue Lines

The FMODDialogueView script automatically plays FMOD events assigned to Yarn dialogue lines.

Uses the current selected language (EN or ALT).

Plays the event when the corresponding Yarn line is reached.

5️⃣ Trigger FMOD Events & Parameters from Yarn Scripts

Use commands inside Yarn scripts:

// Play an FMOD event manually
<<PlayFmodEvent "event:/EN_MALE1">>

// Adjust FMOD parameters
<<SetFmodParameter "event:/EN_MALE1" "Reverb" 0.5>>

6️⃣ FMOD Listener Follows Active Camera

The FMODCameraListenerManager script ensures the correct FMOD Studio Listener is enabled based on the active Cinemachine Virtual Camera.

Attach FMOD Studio Listeners to Cinemachine cameras.

Assign camera-listener pairs in the FMODCameraListenerManager.

🎮 Usage Guide

🔀 Switching Languages at Runtime

A UI button toggles between English & Alternative language voiceovers.

Assigned via FMODLanguageToggle.cs.

Dynamically updates the played FMOD event based on the selected language.

The Yarn text remains in English (subtitles) while audio changes.

📜 Using Multiple Yarn Scripts (Per NPC)

For projects with multiple NPCs, use separate Yarn scripts per character.

Add multiple Yarn scripts to the FMODLineProvider.

Assign FMOD events separately for each NPC’s lines.

The system automatically detects and plays the correct voiceover.

🛠 Debugging & Logging

✅ Check the Console Logs

Successful mapping: ✅ Mapped Yarn Line: line:xxxxxx → FMOD Event

Missing FMOD event: ⚠️ No FMOD event assigned for Yarn Line ID

Event playing: 🔊 Playing FMOD Event: event:/EN_MALE1

🎯 Future Improvements

🔹 More granular control over audio mixing (reverb, spatial effects)
🔹 Multiple alternative languages support
🔹 Live Yarn script reloading without restarting Unity

📝 Credits

Developed for interactive storytelling in Unity, integrating FMOD for immersive audio.
Built using Yarn Spinner to create dynamic and engaging dialogue-driven experiences.

⚡ License

This project is licensed under the MIT License. Feel free to modify and expand upon it!

🎵🎭 Happy game developing! 🚀
