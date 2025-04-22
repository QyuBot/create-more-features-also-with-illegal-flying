# Create More Features Also With Illegal Flying (1.20.1)
- [Readme in Korean](/docs/ko/README.md)

## Before You Begin
This repository covers a modified version of [Create: More Features](https://www.curseforge.com/minecraft/mc-mods/create-more-features).

The original mod file is `create_more_features-0.8.0-forge-1.20.1.jar`, And this repository contains the modified version of the mod file.

Because I'm not a pro or mod developer, Additional bug fixes or technical support may not be provided beyond the mod file and the information in this document.

If you encounter any bugs or other issues, please open an issue so others can be aware.

## What Is This?
This is a modified version of the original mod (Create: More Features) that fixes an issue where an item called *Gravitron* disables creative-style zero-gravity flight.

## What’s the Issue?
When you are flying with Gravitron, If the Gravitron is out of your hand, flight mode will be disabled. 

To do this, check every time if you have a Gravitron in your right hand, and disable flight mode if not.

As a result, items that provide creative-like flight (such as the Mechanism Mekasuit Gravity Unit) cannot flight because flight mode gets immediately disabled in every time.

This cannot be turned off through the config.

## What Should I Do?
Just remove the Create: More Features mod or replace it with the modified version in [Releases](/release).

There are two versions available. Choose the one you want and replace the original mod with it.

### OPG (Over Powered Gravitron) Version
- Remove the code that disables flight mode when the Gravitron is out of your hand at every tick.
- After flying once with the Gravitron, you can continue flying even if the item is no longer held, until you return to the main menu.
- Other items will work fine.

### ULG (Useless Gravitron) Version
- All the Gravitron’s code has been removed.
- The item becomes completely useless.

## How Was It Fixed?
Using the bytecode editing feature of [Recaf](https://recaf.coley.software/home.html), we directly modified the `GravitronprocedureProcedure.class` file.

## Fix It Yourself
If you have knowledge of Minecraft modding and programming (assembly), you can follow the [modification guide](/docs/ko/howTo.md)(written in Korean) to do it yourself.