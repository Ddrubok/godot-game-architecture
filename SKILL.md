---
name: godot-game-architecture
description: Design or review the initial architecture of a Godot game around its first playable flow, scene boundaries, state ownership, and scene transitions. Use for new Godot projects or substantial structural changes (Godot architecture, 고도 게임 아키텍처, 씬 구조); use focused implementation guidance for a local script fix.
---

# Godot game architecture

On direct invocation, report the `VERSION` tag as declared only unless release contents have been verified. The general skill need not be installed.

Start with one playable path, such as title → room → collect an item → exit → reenter. Name the release scope and constraints that could change the structure. For an existing project, check its Godot version and trace that path through the current main scene, scene tree, autoloads, Resources, and save data before proposing changes. Keep useful existing boundaries.

## Decide ownership from the flow

- Sketch the scene tree for the path. Make a node a child when its lifetime should end with its parent. Give a reusable actor, room, or UI element its own scene when it can be instantiated and checked independently. A child scene should not depend on a hard-coded parent path; let its parent configure needed inputs and connect its outputs. [Scene organization](https://docs.godotengine.org/en/stable/tutorials/best_practices/scene_organization.html)
- For each changing value, name its authority and lifetime: node or scene instance, current room, run/session, or saved profile. Keep room-local state with the room; explicitly transfer only data that must survive a room change. State whether reentry restores the room, rebuilds it from session data, or resets it. Avoid two writers for the same value.
- Use a direct method call when the caller owns or is intentionally given the callee and needs a command or result. Use a signal for an event emitted by a reusable child without knowing who responds; connect it at the owning scene boundary and disconnect or let the connection end with that lifetime. Do not create a global event bus for local interactions. [Signals](https://docs.godotengine.org/en/stable/getting_started/step_by_step/signals.html)
- Use Resources for reusable configuration or intentionally shared data. A loaded Resource can be shared across scene instances; do not mutate it as though it were private actor state. Put per-instance mutable state on the instance, or use a scene-local Resource when that is the intended scope (`resource_local_to_scene`). Check nested mutable data when duplicating. [Resources](https://docs.godotengine.org/en/stable/tutorials/scripting/resources.html) · [Resource class](https://docs.godotengine.org/en/stable/classes/class_resource.html)
- Add an autoload only for a responsibility that must remain available across scenes, such as bounded session state or a transition service. Name the data it owns and its reset point. A scene-owned service remains easier to test in isolation when global lifetime is unnecessary. Autoloads remain in the tree during scene changes. [Autoloads versus regular nodes](https://docs.godotengine.org/en/stable/tutorials/best_practices/autoloads_versus_regular_nodes.html)

## Verify the boundary

For a shop and wave flow, compare same-run reentry, a new run, and a new instance against explicit expected values; see the [G1 worked example](README.md#g1-shop-and-wave-reentry).

For design requests, describe the following checks. When building or testing, run a representative reusable scene on its own and check that required inputs are explicit and missing context fails clearly. Then play the full path, change rooms, reenter, and restart the run. Check that the authoritative state survives or resets as specified, old room nodes and signal connections no longer act, and a new scene instance does not inherit accidental Resource mutations. On Godot 4 versions with these APIs, use `SceneTree.change_scene_to_file()` or `change_scene_to_packed()`; wait for `scene_changed` before accessing the replacement scene, since the old scene is removed and freed during the change. [SceneTree](https://docs.godotengine.org/en/stable/classes/class_scenetree.html)

Deliver a compact scene and state map, reasons for the chosen boundaries, the first playable implementation slice, and the specific isolation and reentry checks. Build only the structure needed by the confirmed flow; leave later systems as explicit extension points when their requirements are uncertain.
