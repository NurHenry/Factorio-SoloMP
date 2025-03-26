# Factorio-SoloMP Setup Guide

This guide explains how to set up a Factorio multiplayer session where each player operates independently by managing their own factory, research, and production. The game is configured so that although every player is in their own force, map exploration is shared and alliances are in place. This lets you enjoy a cooperative yet independent experience.

---

## What is a Force?

In Factorio, a **force** represents a distinct faction or team. Each force has its own research, technology tree, and production capabilities. By assigning players to separate forces:
- **Individual Research & Production:** Each player manages their own research and building progress.
- **Shared Map Visibility:** Forces can be configured to share map information so that each team can see what others have explored.
- **Team Alliances:** Even though the factories are independent, forces can be set to be friendly (cease-fire and friend status), allowing cooperative gameplay without interference.

---

## Creating Forces

To start, you need to create separate forces. Replace the names below with your preferred force names (e.g., "FactoryOne", "FactoryTwo", etc.):

```lua
/c game.create_force("FactoryOne")
/c game.create_force("FactoryTwo")
/c game.create_force("FactoryThree")
/c game.create_force("FactoryFour")
/c game.create_force("FactoryFive")
```

---

## Assigning Players to Forces

Once the forces have been created, players need to be assigned to their respective forces. Replace `"PlayerName"` with the actual in-game player name and `"FactoryOne"` with the corresponding force:

```lua
/c game.get_player("PlayerName1").force = game.forces["FactoryOne"]
/c game.get_player("PlayerName2").force = game.forces["FactoryTwo"]
/c game.get_player("PlayerName3").force = game.forces["FactoryThree"]
```

---

## Setting Up Alliances

Once the forces have been created, you can set them up to share their map views and become allies. This ensures that all teams can see the explored map areas of their friends, but still maintain separate research trees and bases.

```lua
/c local teams = {"FactoryOne", "FactoryTwo", "FactoryThree", "FactoryFour"}
for _, t1 in pairs(teams) do
  local force1 = game.forces[t1]
  if force1 then
    force1.share_chart = true
    for _, t2 in pairs(teams) do
      if t1 ~= t2 then
        local force2 = game.forces[t2]
        if force2 then
          force1.set_cease_fire(force2, true)
          force1.set_friend(force2, true)
        end
      end
    end
  end
end
game.print("All forces are now allied and share their map view!")
```

---

## Teleporting Players

You can teleport players to their starting locations or any designated area on the map. Replace `"PlayerName"` with the actual in-game name of the player:

```lua
/c game.get_player("PlayerName").teleport({x=100, y=100})
```

For example, to teleport a player named `"Alex"`:

```lua
/c game.get_player("Alex").teleport({x=100, y=100})
```

---

## Setting Spawn Positions

To set a spawn point for a force (so that when a player dies, they reappear at their factory), use the following command. Replace `"FactoryOne"` with the appropriate force name, and adjust the coordinates as needed:

```lua
/c game.forces["FactoryOne"].set_spawn_position({x=0, y=0}, game.surfaces[1])
```

---

## Configuring Biter (Enemy) Behavior

### Making Biters Peaceful

If you want to disable biter attacks during your setup, you can set the game to peaceful mode and clear any existing enemy units:

```lua
/c game.forces["enemy"].kill_all_units()
/c game.surfaces[1].peaceful_mode = true
```

### Reactivating Biter Aggression

To re-enable biter aggression when you’re ready for challenges, simply set peaceful mode to false:

```lua
/c game.surfaces[1].peaceful_mode = false
```

---

## Conclusion

Feel free to open issues or contribute improvements to this documentation on GitHub.

---

Happy building and exploring in Factorio!
