# Custom Blueprint NPCs: quick guide

> Summary of Epic's official documentation. Source material: [Using the NPC Spawner with Animations](https://dev.epicgames.com/documentation/fortnite/using-the-npc-spawner-with-animations-in-unreal-editor-for-fortnite) (Fortnite Documentation, Epic Developer Community). The feature is in **Early Access**: islands can be published, but future updates may break them and require your intervention.

## What a Custom Blueprint NPC is

An NPC is defined by an **NPC Character Definition** and placed in the level with an **NPC Spawner** device. When the definition uses a **custom Blueprint** instead of a plain character, Sequencer treats it differently:

- Most NPC types bind in Sequencer as a simple **skeletal mesh**.
- A custom Blueprint NPC binds as the **whole Blueprint**, so its extra components (for example Niagara VFX) are exposed and can be keyed in Sequencer. Epic's example animates a Niagara system to make the NPC's head explode.

## Bringing the NPC into a sequence

An NPC Spawner used in a sequence needs a **Binding Lifetime** track (click **+** next to the NPC Spawner in the track list, then **Binding Lifetime**). Epic notes it cannot be added retroactively to sequences created before UEFN 31.00, and islands using an NPC Spawner in a sequence must be republished after the track is added.

There are two ways to bind an NPC, both created from the NPC Character Definition:

1. **Spawnable NPC binding**: drag the NPC Character Definition into Sequencer. The sequence spawns the NPC itself and you animate it like any skeletal mesh (add an animation or emote with **+ Animation**, move it and keyframe positions). No extra setup is needed to play it.
2. **Replaceable NPC binding**: create a spawnable binding, right click it and choose **Convert selected binding to > Replaceable NPC Character**. The sequence takes control of an NPC already spawned in the world. While bound, its behavior, perception and path following are paused. When unbound, they resume and the NPC goes back to its original location.
   - Add the **Sequencer modifier** to the NPC Character Definition, otherwise validation fails.
   - The modifier's **Unique Identifier** (default: the definition name) is used to find the NPC in game. Definitions sharing the same identifier can all be bound.
   - An NPC Spawner using that definition must exist in the level. If no NPC is found, the client log shows `LogFortNPCMovieSceneBindings: Warning: Could not bind to a pawn using NPC Character Definition ...`.
   - If the NPC spawns at the same moment the sequence should start, use a spawnable binding, or connect the spawner's **On Spawned** event to the Cinematic Sequence device's **Play**.

Play the sequence in game with a **Cinematic Sequence** device.

## Animating in Sequencer

1. Create a **Level Sequence** in the Content Browser and open it.
2. **+Track > Actor to Sequencer > NPC Spawner** (or drag the spawner from the Outliner).
3. Click **+** next to the spawner and pick **Control Rig > Control Rig Classes > FK Control Rig** to key individual bones, or **Animation** to add an imported FBX animation sequence.
4. Key the bones over the timeline, preview, then right click the spawner and choose **Bake Animation Sequence**. Check that limbs do not clip through the body.

Animations imported from Unreal Engine (MetaHumans included) may need **retargeting** to the Fortnite skeleton. MetaHumans are memory heavy, so use them sparingly.

## Known restrictions (replaceable bindings)

- The Cinematic Sequence device must use **Visibility: Everyone**.
- Rideable or tameable wildlife NPCs fail validation.
- **Force Keep State** in the Finish Completion State Override option fails validation.
- The NPC snaps into place when bound, and latency can cause brief visual glitches on bind and unbind. Hide it with off screen spawns, screen fades, VFX or the visibility track.

## Playing animations from Verse

Custom animations must be exposed to Verse through **asset reflection** so they appear in `Assets.digest.verse`. Then, from an `npc_behavior`:

- Get the `play_animation_controller` with `GetPlayAnimationController()`.
- `PlayAndAwait(Animation)` plays asynchronously and returns `play_animation_result` (`Completed`, `Interrupted`, `Error`).
- `Play(Animation)` returns a `playing_animation_instance` with `GetState()`, `Stop()`, `Await()` and the events `CompletedEvent`, `InterruptedEvent`, `BlendedInEvent`, `BlendingOutEvent`.
- Optional parameters: `PlayRate` (default 1.0), `BlendInTime`, `BlendOutTime`, `StartPositionSeconds`.

---


# UEFN NPC Animations preset (Escape Lama UEFN Map)

## ESCAPE LAMA [HORROR] HALLOWEEN LLAMA

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1b3a1402-5435-4a3c-8041-55ff4d651a37" />

Developed by [Klian](https://x.com/kliansenpai) and [Engineers.FN](https://x.com/engineers_fn)

# CODE: [3891-5899-6094](https://www.fortnite.com/play/island/3891-5899-6094?lang=en-US)

## HOW TO:

The files in this folder are meant to be used **inside an NPC Character Definition**. The NPC Character Definition is the asset that defines the look and details of an NPC spawned by the **NPC Spawner** device.

To give an NPC a custom skin:

1. **Create a Blueprint Actor with a Skeletal Mesh Component inside it.** That skeletal mesh becomes the NPC's new skin.
2. **Create an Anim Preset** with the movement animations and the right settings for a good result in game.
3. **Assign a Cosmetic NPC Modifier** in the NPC Character Definition, choose **Custom**, and set the two files you just created (the Blueprint Actor and the Anim Preset).

The plugin includes working examples for all of these steps, built on the mannequin that already ships with the Animation Template provided by Epic Games.

<img width="1999" height="1164" alt="image" src="https://github.com/user-attachments/assets/4d4db859-5b23-4f0f-a449-223bd5f13956" />

<img width="931" height="830" alt="image" src="https://github.com/user-attachments/assets/1454da21-f01e-45a0-859a-2447d0f8cb5d" />

-----

### Locate assets
<img width="787" height="387" alt="image" src="https://github.com/user-attachments/assets/fe8caad7-9b1b-4dbf-b8c9-26d7cd1d6064" />

-----

### Examples
<img width="331" height="185" alt="image" src="https://github.com/user-attachments/assets/367193cc-05ea-4050-81b4-f916af18ad0d" />

-----

### Ignore
<img width="836" height="348" alt="image" src="https://github.com/user-attachments/assets/650f8148-989b-44bd-aafe-f62ae356463b" />

-----

### NPC Spawner
<img width="1293" height="1062" alt="image" src="https://github.com/user-attachments/assets/9f50fe2e-3d5c-4409-bf19-20da97cbe2fe" />

-----

### Result
<img width="1910" height="1071" alt="image" src="https://github.com/user-attachments/assets/93a9e752-4a6b-45f3-be8f-9e1b7a47dd12" />
