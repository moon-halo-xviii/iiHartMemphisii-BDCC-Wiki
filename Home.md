Welcome to the BDCC modding wiki! Here you can learn how to make mods ^^

<ins>**BDCC is currently running on Godot 3.6.**</ins>

Reasons why the game isn\'t on Godot 4 is because moving to it would require major rewrites as the scripting language gdscript and 3D workflow was changed. Feel free to fork and port it to Godot 4 but don't expect getting your pull accepted.

## User Modding

If you aren't making a mod and only using them, please go to [User Support page.](User-Support)  

If you are on Android AND you can\'t import/export saves, [please see this guide](User's-Permission-Shenanigans)

## How to start modding

To start, please see this guide,  
[How to set up development environment](How-to-setup-development-environment)

Inside the page will have basic information and step by step on how to set everything up .

<!-- Old info in case of reverting - CIB | Just look at the git blame lol - future CIB
BDCC is running on Godot 3.x engine, usually the latest stable version. At the writing of this tutorial Godot 4.x is in beta but moving to it would require major rewrites as the scripting language gdscript was changed quite a bit. So, what do you need to start modding.

- Latest Godot 3 build. Get it here: https://godotengine.org/download Standard 64 bit version will do, mono is not used. Download godot 3! not 4!
- Game source files. Use git if you know how or just download the zip archive from https://github.com/Alexofp/BDCC Click the green button and press Download as zip. Git is still preferred though in case you will want to make a pull request. Easiest git client is GitHub Desktop

That should be it. Launch Godot, press import and point it to the `project.godot` file inside the game source files folder. Editor should open with BDCC project loaded. From there you can start exploring or just run the game by pressing the play button in the top right corner. Best way to learn how to do something is to check one of the existing scenes.
-->

## What things can be done through mods in BDCC

### Easy stuff:
- [Making Skins](Making-skins)
- [Your first mod. New item](Your-first-mod.-New-item)
- [What are modules](What-are-modules)
- [Writing scenes for the Scene Converter!](Writing-scenes-for-the-Scene-Converter)
- [Your first scene](Your-first-scene)
- [Image packs. Adding your character art](Image-packs.-Adding-your-character-art)
- Characters
- [Changing the core files of the game](Changing-the-core-files-of-the-game)

### Complicated stuff:
- [Bodyparts/hair, Species](Bodyparts,-Species)
- Events that can trigger new scenes
- Npc attacks
- Perks
- Status effects
- Arena fighters
- S\*x Activity
- [New floors](New-floors)
- [New rooms in existing floor without overwriting](Room-to-existing-floor)
- [Animations](AnimationStages)

Eventually each topic will have a dedicated tutorial


## How mods work in BDCC
Mods utilize godot's ability to load \'resource packs\'. 
If you wanna read more about it, check this page: 
https://docs.godotengine.org/en/3.6/tutorials/export/exporting_pcks.html

Unfortunately, due to how Godot currently works, it is impossible to load these resource packs while the game is running from the editor. That means that you can't load mods too. But you can still develop mods as part of the game and then isolating the files that you created into a mod. After that you can test your mod on a compiled version of the game. Tutorials will explain it ^^

## Contributing to modding wiki

0) skip to 4. if you already made a repo and its hosted on your git service of choice
1) `git clone https://github.com/Alexofp/BDCC.wiki.git`
2) make a new repo on any git service (gh preferred)
3) add new remote to the git service
4) pull from the original remote to get all changes before you try to make one
5) make changes
6) push
7) make issue (still needed gh account) titled `[WIKI CONTRIBUTION]` and append brief description in the title, include your remote repo inside the description. it would be nice if you also include merge instruction. rebase preferred.
