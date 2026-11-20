# Godot Game Architecture

[한국어](README.ko.md)

Decide which scenes own the rooms, actors, and state in a Godot game.

```text
$godot-game-architecture A player picks up a key, leaves the room, and returns. Which scene knows the key is gone? Show how to run the room scene on its own and test reentry.
```

### G1: Shop and wave reentry

```text
$godot-game-architecture In a shop, start a run with 10G and buy one 3G tower. Visit a wave and reopen the shop three times. Show the balance, tower count, signal lifetime, new-run reset, and whether two towers made from one Resource share mutable values.
```

First inspect the project's main scene, signal connections, Autoloads, and save path. Give one run owner the balance and owned-tower state; the shop button sends one purchase request to it. The owner checks the price and commits the 7G balance and one tower together. Connect the button when the shop scene becomes active and end that connection with the scene. On every same-run reentry, the UI reads the owner’s current 7G and one tower; reopening the shop twice more does not repeat the purchase. Starting a new run creates or resets run state to 10G and zero towers. Tower definitions can share a Resource, while mutable runtime values belong to each tower instance: changing one leaves the other and the original definition unchanged. For a standalone shop scene, inject test run state with these values instead of requiring the whole game to boot.

## Install

[Download the skill ZIP](https://github.com/Ddrubok/godot-game-architecture/releases/latest/download/godot-game-architecture.zip) and extract it into your Codex skills directory, `$CODEX_HOME/skills` or the default `~/.codex/skills`. The ZIP contains the `godot-game-architecture` folder. After extraction, open `godot-game-architecture/SKILL.md` to read the instructions.

You can use this skill on its own. If you already use the general skill, it can fetch this specialist during a design task. [Software Architecture](https://github.com/Ddrubok/software-architecture)

Direct invocation needs no general skill installation. Read `VERSION` as the declared tag; if release contents have not been verified, report only “declared v1.1.0 (unverified),” not a verified release version.
