# StatusEffectBase

## Properties

| Type | Name | Default Value |
| -------- | -------- | -------- |
| bool | [alwaysCheckedForNPCs](#bool-alwayscheckedfornpcs--false) | false |
| bool | [alwaysCheckedForPlayer](#bool-alwayscheckedforplayer--false) | false |
| BaseCharacter | [character](#basecharacter-character) | |
| String | [id](#string-id) | |
| bool | [isBattleOnly](#bool-isbattleonly--false) | false |
| bool | [isSexEngineOnly](#bool-issexengineonly--false) | false |
| int | [priorityDuringChecking](#int-priorityduringchecking) | 0 |
| bool | [removedOnDungeonStart](#bool-removedondungeonstart--false) | false |
| bool | [subscribeCheckOnFightStart](#bool-subscribecheckonfightstart--false) | false |
| int | [turns](#int-turns---1) | -1 |

## Methods

| Return Type | Name |
| -------- | -------- |
| Buff | [buff](#buff-buff-buffid-args--) |
| bool | [checkAvoidedBuff](#bool-checkavoidedbuff-_npc--false) |
| Array | [checkOnFightStart](#array-checkonfightstart-_npc-_contextdictionary) |
| void | [combine](#void-combine-_newargs--) |
| BaseCharacter | [contextGetEnemy](#basecharacter-contextgetenemy-_contextdictionary) |
| String | [contextGetEnemyID](#string-contextgetenemyid-_contextdictionary) |
| float | [getAccuracyMod](#float-getaccuracymod----00) |
| Array | [getBuffs](#array-getbuffs--) |
| float | [getDamageMultiplierMod](#float-getdamagemultipliermod-_damagetype--00) |
| float | [getDodgeMod](#float-getdodgemod----00) |
| String | [getEffectDesc](#string-geteffectdesc--) |
| String | [getEffectImage](#string-geteffectimage--) |
| String | [getEffectName](#string-geteffectname--) |
| Color | [getIconColor](#color-geticoncolor----iconcolorblue) |
| float | [getRecievedDamageMod](#float-getrecieveddamagemod-_damagetype--00) |
| String | [getVisibleDescription](#string-getvisibledescription--) |
| void | [_init](#void-_init--) |
| void | [initArgs](#void-initargs-_args--) |
| bool | [isDrugEffect](#bool-isdrugeffect----false) |
| void | [loadData](#void-loaddata-_data) |
| void | [onFightEnd](#void-onfightend-_contex--) |
| void | [onFightStart](#void-onfightstart-_contex--) |
| void | [onSleeping](#void-onsleeping--) |
| void | [onSexEnded](#void-onsexended-_contex--) |
| void | [onSexEvent](#void-onsexevent-_eventsexevent) |
| void | [onSexStarted](#void-onsexstarted-_contex--) |
| void | [processBattleTurn](#void-processbattleturn--) |
| void | [processBattleTurnContex](#void-processbattleturncontex-_contex--) |
| void | [processSexTurn](#void-processsexturn--) |
| void | [processSexTurnContex](#void-processsexturncontex-_contex--) |
| void | [processTime](#void-processtime-_secondspassed-int) |
| Dictionary | [saveData](#dictionary-savedata--) |
| void | [setCharacter](#void-setcharacter-c) |
| bool | [shouldApplyTo](#bool-shouldapplyto-_npc--false) |
| bool | [shouldHaveWideTooltip](#bool-shouldhavewidetooltip----false) |
| void | [stop](#void-stop--) |

## Property Descriptions

### *bool* alwaysCheckedForNPCs = false
When true, the status effect is checked for all NPCs every time the game scene is updated. With each update, the [shouldApplyTo](#bool-shouldapplyto-_npc--false) method is called to check whether the effect should be applied or removed.

This property is useful for status effects triggered by character properties, such as "InHeat" or "Pregnancy". It's also used for status effects applied by clothing or bondage items, such as "BlindfoldedStatus" and "ArmsBoundStatus".

### *bool* alwaysCheckedForPlayer = false
When true, the status effect is checked for the player character every time the game scene is updated. With each update, the [shouldApplyTo](#bool-shouldapplyto-_npc--false) method is called to check whether the effect should be applied or removed.

Like [alwaysCheckedForNPCs](#bool-alwayscheckedfornpcs--false), this property is useful for status effects triggered by character properties and clothing items. Examples of player-exclusive status effects include "Intoxicated", since NPCs don't have an intoxication property, and "NakedStatus", since naked NPCs don't experience the same buffs and debuffs as naked player characters.

### *BaseCharacter* character
When an instance of a status effect is created and applied to a character, this property stores a reference to said character, allowing you to easily access their own properties and methods.

### *String* id
The unique reference for the status effect in the code. Not to be confused with the status effect's visible name, which is returned by [getEffectName](#string-geteffectname--).

### *bool* isBattleOnly = false
When true, this effect will be removed at the end of battles.

Used for status effects exclusively used in battles, such as when a character is stunned, bleeding, or collapsed on the ground.

### *bool* isSexEngineOnly = false
When true, this effect will be removed at the end of dynamic sex encounters.

Used for status effects which only apply during dynamic sex. These typically manipulate properties exclusive to the sex engine, such as arousal, dom anger/sub fear, resistance/forced obedience, etc.

### *int* priorityDuringChecking
The greater this property's value, the earlier it will be updated every update,  relative to status effects with lower priorities.

### *bool* removedOnDungeonStart = false
When true, this effect will be removed whenever the player starts a drug den run.

Since drug den gameplay follows roguelike conventions in which players start without any perks or equipment, beneficial status effects should also be cleared when starting a new run. For example, the benefits of doing yoga and lifting weights at the gym are cleared upon starting a new drug den run.

### *bool* subscribeCheckOnFightStart = false
When true, at the start of a battle, the [checkOnFightStart](#array-checkonfightstart-_npc-_contextdictionary) method is called. This allows you to conditionally apply the status effect to a character at the beginning of a fight.

### *int* turns = -1
Typically used to track the remaining duration of the effect.

When the effect is first applied, the base duration should be set with the _init method, and then decremented as time passes with the appropriate processing method:

* For battle-only status effects, this property can be used to measure the effect's duration by battle turns, as the name suggests; in this case, the property is decremented with the [processBattleTurn](#void-processbattleturn--) method. For example, the Bleeding status effect lasts for three turns by default, so turns = 3.

* Alternatively, duration can be measured by game seconds, according to the in-game clock; in this case the property is decremented with the [processTime](#void-processtime-_secondspassed-int) method. For example, the Yoga status effect lasts for twelve hours, so turns = 12\*60\*60.

> [!IMPORTANT]
> Don't forget to configure the [saveData](#dictionary-savedata--) and [loadData](#void-loaddata-_data) methods to write this property's value to save data. Otherwise, if the player saves and loads the game before the status effect expires, the status effect will bug out and might cause some serious issues.


## Method Descriptions

### *Buff* buff (buffid, args = [])
Core functionality of the base status effect class to initialize the buffs applied to a character by the [getBuffs](#array-getbuffs--) method.

### *bool* checkAvoidedBuff (_npc) = false
When this method returns true, an attempt to apply the status effect to a character fails: they avoid the status effect.

Alternatively, apply a StatusEffectImmunityBuff to the target.

### *Array* checkOnFightStart (_npc, _context:Dictionary)
When the [subscribeCheckOnFightStart](#bool-subscribecheckonfightstart--false) property is true, this method is called at the start of a battle. Subsequently, when this method returns [true], the status effect is applied to the character at the start of the fight. If true, the second element can hold another array for any arguments you want to pass to the status effect being applied.

For example, refer to how the Aura of Dominance checks if it should be applied at the start of a fight:

```
func checkOnFightStart(_npc, _context:Dictionary) -> Array:
	var theEnemyID:String = contextGetEnemyID(_context)
	
	if(theEnemyID == "pc"):
		var ourID:String = _npc.getID()
		
		var theSpecialRelationship:SpecialRelationshipBase = GM.main.RS.getSpecialRelationship(ourID)
		if(theSpecialRelationship && theSpecialRelationship.shouldHaveAuraOfDominance()):
			return [true, []]
	
	return [false]
```

> [!IMPORTANT]
> Pay close attention to this method's return type: the bool needs to be returned as **the first element of an array**.


### *void* combine (_newArgs = [])
When the status effect is added to a character, if that character already has the same kind of status effect, the arguments are passed to this function to modify it.

This is often used to extend the effect's duration by incrementing the [turns](#int-turns---1) property, although it can also be used to update the status effect in other ways, such as adding new body writings to a character.

### *BaseCharacter* contextGetEnemy (_context:Dictionary)
Calls [contextGetEnemyID](#string-contextgetenemyid-_contextdictionary) and returns a reference to the character.

### *String* contextGetEnemyID (_context:Dictionary)
Returns the character ID of the opponent at the start of a battle. Call this method within another method that grabs fight scene context, such as [onFightStart](#void-onfightstart-_contex--), for this to work properly.

### *float* getAccuracyMod ( ) = 0.0
Deprecated method: use [getBuffs](#array-getbuffs--) to apply AccuracyBuff instead.

### *Array* getBuffs ( )
Returns an array of buffs that the status effect applies to the character while it is active.

For example, the Light Workout status effect grants a +2 bonus to strength, a 10-point physical damage reduction, and a 50% boost to combat skill experience gained. Its getBuffs method looks like this:
```
func getBuffs():
	return [
		buff(Buff.StatBuff, [Stat.Strength, 2]),
		buff(Buff.PhysicalArmorBuff, [10]),
		buff(Buff.SkillExperienceBuff, [Skill.Combat, 50.0])
	]
```

### *float* getDamageMultiplierMod (_damageType) = 0.0
Deprecated method: use [getBuffs](#array-getbuffs--) to apply PhysicalDamageBuff or LustDamageBuff instead.

### *float* getDodgeMod ( ) = 0.0
Deprecated method: use [getBuffs](#array-getbuffs--) to apply DodgeChanceBuff instead.

### *String* getEffectDesc ( )
Returns a brief description of the status effect.

### *String* getEffectImage ( )
Returns the resource path of the icon representing the status effect on the character panel.

### *String* getEffectName ( )
Returns the name of the status effect that is visible to the player, displayed when hovering over its icon in the character panel.

### *Color* getIconColor ( ) = IconColorBlue
Returns the color of the icon representing the status effect on the character panel.

While this can be any color you choose, there are a number of constants provided that you can choose from. The most commonly used colors are IconColorRed, typically used for negative effects; IconColorGreen, typically used for positive effects; and IconColorBlue, which is used for drug effects and also serves as the default icon color.

### *float* getRecievedDamageMod (_damageType) = 0.0
Deprecated method: use [getBuffs](#array-getbuffs--) to apply ReceivedPhysicalDamageBuff or ReceivedLustDamageBuff instead.

### *String* getVisibleDescription ( )
Returns the fully formatted description for the status effect presented by its tooltip when hovering over its icon in the character panel. Includes the description from [getEffectDesc](#string-geteffectdesc--) and the list of buffs from [getBuffs](#array-getbuffs--).

### *void* _init ( )
Initializes the status effect. Configure the static properties of the status effect here.

### *void* initArgs (_args = [])
Processes the arguments passed to an effect being added to a character. Typically initializes dynamic properties, such as [turns](#int-turns---1).

### *bool* isDrugEffect ( ) = false
When this method returns true, the status effect is registered as an effect from a drug. Examples include the cumulative lust gain from heat pills during sex, the stamina boost from energy drinks during battle, and the fertility debuffs from birth control pills.

This method's value is measured by two BaseCharacter methods:
* **getDrugsInfluenceAmount()** returns an integer count of how many active drug effects a character currently has. Whenever a character cums while under the influence of drugs, their drug use fetish increases: this method's value serves as a multiplier for the amount their fetish score is incremented by.
* **isUnderDrugsInfluence()** returns true if the character has any active drug effects.

### *void* loadData (_data)
When a save file is loaded, this method reinitializes the properties with their respective values written to the file.

Each property is reinitialized as such:
```
property = SAVE.loadVar(_data, "propertyKey", defaultValue)
```
where _data is the save file being loaded, "propertyKey" is the key in the [saveData](#dictionary-savedata--)) dictionary corresponding to the property, and defaultValue is the value the property should be assigned if it doesn't have an entry in the saveData dictionary.

For example, the "Blocking" status effect's loadData method looks like this:
```
func loadData(_data):
	turns = SAVE.loadVar(_data, "turns", 3)
	howMuchBlock = SAVE.loadVar(_data, "howMuchBlock", 80)
``` 

### *void* onFightEnd (_contex = {})
Called at the end of a fight scene.

### *void* onFightStart (_contex = {})
Called at the start of a fight scene.

### *void* onSleeping ( )
Called when the player character sleeps each night.

### *void* onSexEnded (_contex = {})
Called at the end of a dynamic sex scene.

### *void* onSexEvent (_event:SexEvent)
Called when sex events are sent by certain activities. By comparing the _event argument, the status effect can respond to specific sex events, such as SexEvent.Orgasmed or SexEvent.PainInflicted.

### *void* onSexStarted (_contex = {})
Called at the beginning of a dynamic sex scene.

### *void* processBattleTurn ( )
Called at the start of every player action during each turn of battle.

When decrementing [turns](#turns-int---1) during battle, the method usually looks like this:

```
func processBattleTurn():
	turns -= 1
	if(turns <= 0):
		stop()
```

### *void* processBattleTurnContex (_contex = {})
Advanced version of [processBattleTurnContex](#void-processbattleturncontex-_contex--). The contex is a two key dictionary:

* whoID: The ID of the current character
* enemyID: The ID of the character's opponent

If either key refers to the player character, its value will simply be "pc".

### *void* processSexTurn ( )
Called at the beginning of dynamic sex, and with every subsequent turn.

### *void* processSexTurnContex (_contex = {})
Advanced version of [processSexTurn](#void-processsexturn--). The contex is a two key dictionary:

* sexEngine: a reference to the active sex engine
* isDom: a bool set to true if the character is the active dom, false if they're the sub

### *void* processTime (_secondsPassed: int)
Called whenever the game clock is updated; in other words, whenever time passes in-game.

When tracking the duration of an effect with [turns](#turns-int---1), this method tends to look like this:

```
func processTime(_secondsPassed: int):
	turns -= _secondsPassed
	if(turns <= 0):
		stop()
```

### *Dictionary* saveData ( )
Returns a dictionary of the status effect's properties and their current values, in order to write them to save data. When a save is loaded, the [loadData](#void-loaddata-_data) method uses this dictionary to reinitialize the effect.

For example, the "Blocking" status effect has two key properties: "turns" (the remaining duration of the effect) and "howMuchBlock" (the amount of damage to negate). Its saveData method looks like this:
```
func saveData():
	return {
		"turns": turns,
		"howMuchBlock": howMuchBlock,
	}
```

> [!TIP]
> While it's useful for the keys and values in the saveData dictionary to have the same name, the key names are arbitrary. If you wish, you can shorten the key names to *slightly* optimize save file size.

### *void* setCharacter (c)
Sets the [character](#basecharacter-character) property of the status effect when a new instance of a status effect is applied to a character.

### *bool* shouldApplyTo (_npc) = false
When either the [alwaysCheckedForNPCs](#bool-alwayscheckedfornpcs--false) or [alwaysCheckedForPlayer](#bool-alwayscheckedforplayer--false) properties are true, this method is called every time the game scene is updated to check whether or not the status effect should be applied, kept, or removed. When the method returns true, the status effect is applied or kept; when it returns false, it is removed.

For example, the "ExhaustedStatus" status effect is applied to player characters when they run out of stamina. Its shouldApplyTo method looks like this:
```
func shouldApplyTo(_npc):
	if(_npc.getStamina() <= 0):
		return true
	return false
```

### *bool* shouldHaveWideTooltip ( ) = false
When true, this method doubles the width of the tooltip displayed when hovering over the status effect on the character panel.

As of version 0.1.12, this is only used for the Fetishes display in character panels (named SexEngineLikes in the game files), where the tabulated list of fetishes requires the extra width to render properly.

### *void* stop ( )
When this method is called, the effect is removed from the character. Typically called by the [processBattleTurn](#void-processbattleturn--), [processTime](#void-processtime-_secondspassed-int), or [onSleeping](#void-onsleeping--) when conditions are met to end the status effect.