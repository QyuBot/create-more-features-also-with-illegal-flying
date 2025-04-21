# Create More Features Also With Illegal Flying (1.20.1)
- Translated by GPT
- [한국어](/docs/ko/README.md)

## Before You Begin
This repository covers a modified version of [Create: More Features](https://www.curseforge.com/minecraft/mc-mods/create-more-features).

It corresponds to the file `create_more_features-0.8.0-forge-1.20.1.jar`.

No additional bug fixes or technical support will be provided beyond the mod file and information included in this document.

## What Is This?
This is a modified version of the original mod (Create: More Features) that fixes an issue where an item called *Gravitron* disables creative-style zero-gravity flight.

## What’s the Issue?
To enable the Gravitron’s functionality, there’s code that disables flying mode every tick if the player isn’t holding the Gravitron.

As a result, items that provide creative-like flight (such as the Mechanism Mekasuit Gravity Unit) cannot maintain flight mode, since it gets immediately disabled.

This behavior cannot be turned off through the config.

## How Was It Fixed?
Using the bytecode editing feature of [Recaf](https://recaf.coley.software/home.html), we directly modified the `GravitronprocedureProcedure.class` file.

## What Should I Use?
There are two versions available. Choose the one you want from the Releases section and replace the original mod with it.

### OPG (Over Powered Gravitron) Version
- Only the code checking if the Gravitron is held has been removed.
- After flying once with the Gravitron, you can continue flying even if the item is no longer held, until you return to the main menu.

### ULG (Useless Gravitron) Version
- All of the Gravitron’s code has been removed.
- The item becomes completely useless.

## Try It Yourself
If you have knowledge of Minecraft modding and programming (assembly), you can follow the [modification guide](/docs/ko/howTo.md)(written in Korean) to do it yourself.