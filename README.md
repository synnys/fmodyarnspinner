# FMOD & Yarn Spinner Integration for Unity

Connect Yarn Spinner dialogue lines to FMOD voice-over events, with English and alternative-language audio mappings.

This package is designed for **Yarn Spinner 3**. The setup below keeps the dialogue audio components on your existing **Dialogue System** GameObject.

## Features

- Assign FMOD voice-over events to individual Yarn dialogue lines.
- See the dialogue text in each mapping's **Description** field.
- Assign separate English (`EN`) and alternative-language (`ALT`) events.
- Wait for voice-over playback to finish, with support for advancing past a line.
- Trigger additional FMOD events and change global parameters using Yarn commands.
- Optionally switch voice-over languages with a UI button.

## Requirements

- Unity with Yarn Spinner 3 installed. The updated presenter targets the API used by Yarn Spinner **3.2.8**.
- FMOD for Unity, configured for your FMOD Studio project.
- A working Yarn Project and Dialogue System.
- FMOD voice-over events assigned to banks that are built and available to Unity at runtime.

The updated `FMODDialogueView` uses `DialoguePresenterBase` and is not compatible with Yarn Spinner 2's `DialogueViewBase` API.

## 1. Import the package

1. Download `FMODYarnSpinner.unitypackage` from this repository.
2. In Unity, select **Assets → Import Package → Custom Package…**.
3. Select the downloaded package and import its contents.
4. Let Unity finish compiling and resolve any Console errors before continuing.

## 2. Add line tags to your Yarn scripts

Every dialogue line that needs a voice-over must have a unique line tag.

1. In the **Project** window, select your **Yarn Project** asset, such as `MyStoryProject`.
2. In its Inspector, click **Add Line Tags to Yarn Scripts**.

Your dialogue lines will have tags like this:

```yarn
Player: I need to get off this island. #line:0d19f8a
```

Keep these IDs once you assign audio. They connect the dialogue lines to their FMOD events.

## 3. Add the components to Dialogue System

Select your existing **Dialogue System** GameObject in the Hierarchy. Add these components using **Add Component**:

| Component | Required? | Purpose |
| --- | --- | --- |
| `FMODLineProvider` | Yes | Maps Yarn line IDs to FMOD voice-over events. |
| `FMODDialogueView` | Yes | Plays the mapped audio when Yarn presents a line. |
| `FMODLanguageToggle` | Optional | Connects a UI button to EN/ALT audio switching. |
| `YarnFmodTrigger` | Optional | Registers Yarn commands for FMOD events and global parameters. |

You do not need separate FMOD Manager or FMOD Dialogue View GameObjects for this setup.

## 4. Populate the line mappings

On the **FMOD Line Provider** component:

1. Drag your `.yarn` script, such as `MyStoryScript`, into **Yarn Script**. Use the script, not the Yarn Project asset.
2. Open the component's **⋮ menu** in its top-right corner.
3. Select **Update Lines**.
4. Expand **Line Event Mappings**.

Each entry contains:

| Field | What it contains |
| --- | --- |
| **Line ID** | The Yarn line tag, such as `line:0d19f8a`. |
| **Description** | The dialogue text, including the speaker when present, without trailing tags. |
| **Fmod Event EN** | The English voice-over event. |
| **Fmod Event ALT** | The alternative-language voice-over event. |

The foldout heading may still display the line ID. Expand the entry to read its **Description**.

> **Add Line Tags** and **Update Lines** do different jobs: Yarn adds IDs to your script; Update Lines reads those IDs into the FMOD component. Update Lines does not create tags or change the Yarn file. There is no separate Validate button.

The current provider accepts **one Yarn script**.

## 5. Assign the voice-over events

For each line you want voiced:

1. Read its **Description**.
2. Use the FMOD event picker to assign the matching event to **Fmod Event EN**.
3. If using a second language, assign the matching event to **Fmod Event ALT**.

For example:

```text
Line ID:       line:0d19f8a
Description:   Player: I need to get off this island.
Fmod Event EN: event:/VO/Player/ineedofftheisland
Fmod Event ALT: [your alternative-language event]
```

Audio defaults to **EN**. An empty event for the selected language produces no voice-over; the script does not automatically fall back to the other language.

When adding or editing dialogue later:

1. Save your Yarn script.
2. Use **Add Line Tags to Yarn Scripts** for new, untagged lines.
3. Run **Update Lines** again outside Play mode.
4. Assign events for the new lines and save your scene.

Update Lines refreshes descriptions and preserves existing audio assignments for unchanged line IDs.

## 6. Connect FMOD Dialogue View to its provider

On the **FMOD Dialogue View** component:

1. Find **Fmod Line Provider**.
2. Drag the **FMOD Line Provider component header** from the same GameObject into that field.

The reference should display:

```text
Dialogue System (FMOD Line Provider)
```

## 7. Register the FMOD dialogue presenter

**This step is essential. Adding the component to the GameObject alone does not make Yarn use it.**

On the **Dialogue Runner** component:

1. Expand **Dialogue Presenters**.
2. Click **+** to add an entry.
3. Drag the **FMOD Dialogue View component header** into the new slot.
4. Keep your existing presenters.

With Yarn's standard Dialogue System, the list should look like this:

```text
Element 0: Line Presenter
Element 1: Options Presenter
Element 2: Line Advancer
Element 3: Dialogue System (FMOD Dialogue View)
```

> Do not put `FMODLineProvider` in the Dialogue Runner's separate **Line Provider** field. That field is for Yarn's text/localisation provider. If you use Yarn's default setup, it can remain empty so Yarn creates its built-in localised line provider at runtime.

## 8. Add a language button — optional

1. Create a UI **Button** with a TextMeshPro label.
2. On **FMOD Language Toggle**, assign:
   - **Line Provider:** the FMOD Line Provider on Dialogue System.
   - **Toggle Button:** your UI Button component.
   - **Button Text:** the button's TextMeshProUGUI label.
3. Assign EN and ALT voice-over events for the lines you want voiced.

The script registers the button click automatically. Do not also add `ToggleLanguage` to the Button's **On Click** list, as this would toggle twice.

Switching language affects subsequent voice-over playback. It does not replace audio already playing or change Yarn's subtitle language. Configure text localisation separately in Yarn Spinner.

For English-only audio, leave out or disable **FMOD Language Toggle**.
