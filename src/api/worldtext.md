---
id: worldtext
name: WorldText
title: WorldText
tags:
    - API
---

# WorldText

WorldText is an in-world text CoreObject.

## Properties

| Property Name | Return Type | Description | Tags |
| -------- | ----------- | ----------- | ---- |
| `text` | `string` | The text being displayed by this object. | Read-Write |

## Functions

| Function Name | Return Type | Description | Tags |
| -------- | ----------- | ----------- | ---- |
| `GetColor()` | [`Color`](color.md) | The color of the Text. | None |
| `SetColor(Color)` | `None` | The color of the Text. | None |
| `SetFont(string fontId)` | `None` | Sets the text to use the specified font asset. | None |

## Examples

Example using:

### `GetColor`

### `SetColor`

In this example, a WorldText object that is placed in the scene changes color gradually from white to black. The script expects to be a child of the WorldText. Notice that if you run this in multiplayer mode, the color changes will not be as smooth as in single-player preview. To fix that place the WorldText + Script hierarchy under a Client Context.

```lua
local nameTextObject = script.parent

function Tick(deltaTime)
    local c = nameTextObject:GetColor()

    if c.r < 0.03 then
        -- Start over from white (x3 so it stays on white for a bit longer)
        c = Color.WHITE * 3
    else
        c = Color.Lerp(c, Color.BLACK, deltaTime * 2)
    end

    nameTextObject:SetColor(c)
end
```

See also: [CoreObject.parent](coreobject.md) | [Color.Lerp](color.md)

---

Example using:

### `SetFont`

In this example, we change the font on a World Text at runtime. Each second we alternate between two different fonts. Note that not every font works with World Text objects. If an unsupported font is assigned with `SetFont()` the World Text resets to its default font.

```lua
local WORLD_TEXT = script:GetCustomProperty("WorldText"):WaitForObject()
local FONT_1 = script:GetCustomProperty("Font1")
local FONT_2 = script:GetCustomProperty("Font2")

while true do
    Task.Wait(1)
    WORLD_TEXT:SetFont(FONT_1)
    
    Task.Wait(1)
    WORLD_TEXT:SetFont(FONT_2)
end
```

See also: [CoreObject.GetCustomProperty](coreobject.md) | [CoreObjectReference.WaitForObject](coreobjectreference.md) | [Task.Wait](task.md)

---

Example using:

### `text`

Change the contents of a WorldText object with the `text` property. In this example, when a new player joins the game their name is written to the WorldText. It's also demonstrated that `<br>` can be used to insert line breaks. This script expects to be the child of a WorldText object that is placed in the scene.

```lua
local nameTextObject = script.parent

Game.playerJoinedEvent:Connect(function (player)
    nameTextObject.text = player.name .. "<br>has joined the game!<br>GLHF!"
end)
```

See also: [CoreObject.parent](coreobject.md) | [Game.playerJoinedEvent](game.md) | [Player.name](player.md) | [Event.Connect](event.md)

---
