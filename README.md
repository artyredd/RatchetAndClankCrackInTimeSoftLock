Ratchet and Clank Future: A Crack In Time
# Time Anomaly Planets (Time fixing mini-games) Soft Lock Skips and Workarounds

1. [Scope](#scope)
2. [Cause](#cause)
3. [Prerequisites](#prerequisites)
4. [Overview](#overview)
4. [First Planet (Kreeli Comet Room)](#first-planet-kreeli-comet-room)
5. [Second Planet(Planet Quantos) and Onwards](#second-planet-planet-quantos-and-onwards)
6. [Environment](#environment)

## Scope
These instructions provide limited work arounds in avoiding / defeating the soft lock associated with missing textures in the [Time fixing mini-games](https://ratchetandclank.fandom.com/wiki/Time_fixing_mini-game) &#128187;(External, wiki) (Planet Room) minigames within the Clank Subconcious levels of the [Great Clock](https://ratchetandclank.fandom.com/wiki/Great_Clock) &#128187;(External, wiki).

## Cause
When emulating Ratchet & Clank Future: A Crack In Time on RPCS3 some textures do not render, these include lasers, fire, and most notably the time anomolies within the Time fixing mini-game.

## Prerequisites
### Required Pograms:
- [Cheat Engine](https://cheatengine.org/) &#128187;(External)
> &#9940; **Caution** Installer contains bloatware  &#9940;  
<u>Select **Skip All** in installer to avoid bloatware</u>

## Overview
### Video Quick Links
- First Planet
  - [Locating the memory address](https://www.youtube.com/watch?v=bLkfLdcC5uQ)  &#128250; (Youtube)
  - [Skipping the Planet](https://www.youtube.com/watch?v=imwx-ocYBGQ) &#128250; (Youtube)
- Second Planet+
  - [Locating the memory address](https://www.youtube.com/watch?v=rcUSXrplIgo) &#128250; (Youtube)
  - [Modifying the assembly code](https://www.youtube.com/watch?v=UVKmdvAZTyo) &#128250; (Youtube)
### Estimated Times
**First Planet**
- Locating the memory address: **< 2 minutes**
- Skipping level: **< 1 minute**

**Second Planet+**
- Locating the memory address: **~ 5 minutes**
- Locating the assembly code: **< 1 minute**
- Modifying the assembly code: **< 2 minutes**
- Beating mini-game: **< 10 seconds**

## First Planet (Kreeli Comet Room)
### Context
In addition to the inability to see the anomolies on the planet - the Kreeli Comet room is intended to be a tutorial planet to introduce you to the concept of the minigame. 

The way it does this is by limiting the number of features, bonuses, and challenges and gradually introducing them with every planet you successfully complete.

This is a problem becuase without extensive work we have no way to track the number of anomolies, their position, or anything else to complete the minigame.

The only work around for this section of Clank's Subconcious is to trigger an area cutscene to skip the planet all together.

To trigger the cutscene we have to travel to the large rotating ring to the left of the planet minigame. We will do so by temporarily giving Clank infinite helicopter jumps to fly over to the large spinning ring.

### Workaround
1. Locate the memory address that `currentJumpCount` is stored.
2. Freeze the value at the memory address to `0`

#### Locating the memory address
Characteristics:  
- `currentJumpCount` is stored as a `byte`
- `currentJumpCount` has a default value of `0` and a maximum value of `3`
    - `0` represents a grounded state
    - `1` represents one helicopter jump has been used  
    - `2` represents two helicopter jumps have been used
    - `3` represents no additional helicopter jumps remain
    

Using the provided characteristics find the memory address associated with Clank's Helicopter jump (`currentJumpCount`).

[Locating the memory address](https://www.youtube.com/watch?v=bLkfLdcC5uQ)  &#128250; (Youtube)

I found it was best to locate the addresses by jumping to the max(`3`) and searching for `3`(`byte`) as my initial value - as all others provided too many results. I followed the pattern `3` -> `1` -> `3` -> `1` -> `3` -> `2` and so on until only 2 memory addresses were left in my search.

For me the addresses for `currentJumpCount` were
>     &346F94AB1
>     &446F94AB1

### Finishing Clank's Subconsciousness

[Skipping the Planet](https://www.youtube.com/watch?v=imwx-ocYBGQ) &#128250; (Youtube)

Freezing either address's value to `0` will provide infinite helicopter flight.

## Second Planet (Planet Quantos) and Onwards

### Context
After skipping the tutorial planet(Kreeli Comet) all other planets have Bombs, which will allow us to actually complete all of the planets.

Bombs, when detonated, repair any anomolies that they hit with their explosions. We can not see the anomolies, so therefore, we can use bombs to beat the mini-game for us.

Only a limited number of bombs can spawn until the current ones are destroyed. For this work around we are going to make unlimited bombs spawn, and multiply the number of bombs that spawn at once.

### Workaround
1. Locate the memory address where `bombCount` is stored
2. Modify the assembly code to enable unlimited bombs to spawn
3. Modify the assembly code to increase the number of bombs that spawn

### Locating the memory address
Characteristics Scope:  
These characteristics are specifically for the first wave of Planet Quantos (the first planet after the cutscene following the infinite helicopter skip). **The modifications that we apply using Planet Quantos will automatically apply to all other planets.**

Characteristics:
- `bombCount` is stored a `byte`
- `bombCount` is controlled by a `*pointer` and freezing it's value has no benefit
- It's default value is `0` and has a maximum value of `3`
    - `0` represents no bombs are present
    - `3` represents 3 bombs are present

In order to locate the addresses use the provided characteristics. Begin Planet Quantos, wait for the 3 initial bombs to spawn, destroy any number of them, and restart the planet afterwwards in order to narrow down the addresses.

[Locating the memory address](https://www.youtube.com/watch?v=rcUSXrplIgo) &#128250; (Youtube)

For me the addresses for `bombCount` were
>     &346F974B7
>     &446F974B7  

### Modifying The Assembly Code
1. Locate the address of `bombCount` (see above)
2. Use `Find out what writes to this address` to locate the assembly that modifies `bombCount`
3. Change the assembly to prevent incrementing `bombCount`
4. Change the assembly to allow many bombs to spawn at the start of a wave and subsequently instantly beating every wave

### Finding the code
Use `Find out what writes to this address` to locate the assembly that modifies `bombCount`. This should provide you with a list of two instructions that change `bombCount`. 

&#9940; **Do not use** the `Replace` button on the `Find out what writes to this address` form. &#9940;

The instruction we want to modify should end with `#####03AE`, the instruction for me was `163E203AE` but it is likely to be different for you. 

If you can't determine the instruction by name you want to look for the one that looks similar to: 
>     movebe [rax+rbx+00000134],ecx

Click on the instruction to select it. Click the `Show disassembler` on the right-hand side of the form to view the assembly code for the instruction.

### Assembly Modification #1
Look for the line
>     #####090 ... inc    rcx

This line is reponsible for incrementing `bombCount`. Right click this line and select `Replace with code that does nothing`. To allow infinite bombs to spawn on the planet.

[Modifying the assembly code](https://www.youtube.com/watch?v=UVKmdvAZTyo) &#128250; (Youtube)

### Assembly Modification #2
Return back to the same assembly code and look for the line
>     #####048 ... inc    [rbp+000000F8]

Double-click the line and in the `Single-line assembler` box that pops up enter the following line to replace the original.

>     add [rbp+000000F8], 16

This replacement will allow you to instantly win every wave. You can increase the number from `16` if you don't instantly win every wave on all planets, but 16 worked best in my testing.

[Modifying the assembly code](https://www.youtube.com/watch?v=UVKmdvAZTyo) &#128250; (Youtube)

Once these two modifications have been made all planets will be won automatically. **These changes will reset when the game is restarted.**

## Environment
These environment details are provided for purely academic purposes to allow replication of my findings on similar hardware. The environment specs listed here are unlikely to change the result or steps significantly.
### Hardware
- AMD Ryzen 3800X
- 32GB DDR4
- GTX 1060
- Windows 10 x64 AME
### Software
- RPCS3 0.0.16-12408-aaa20c04 Alpha
- RAMDisk
### Disk
- RPCS3 is running in a 4096MB **RAMDisk** with a junctioned save directory for persistent gamestates
- Game Source Files are on 512GB NVME m.2 SSD
- Cheat Engine is on the same 512GB NVME m.2 SSD
