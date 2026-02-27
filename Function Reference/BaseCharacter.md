# Functions
### getID (String)
Returns a string representing the character's ID. This is used to identify them in-game, and is used for pretty much every function that has to do with characters.

### addPain, addLust, addStamina (void)
(\_p : int)
Increases the respective stat of the character by *\_p*, emitting the *pain_changed* (or *lust_changed*, or *stamina_changed*) signal if the final stat value is different than what it was before the function was called.
The stat value is clamped between 0 and the value returned by the stat's respective [**threshold function**](#painThreshold,%20lustThreshold,%20getMaxStamina%20(int)) (inclusive). It will always emit the *stat_changed* signal.

### getPain, getLust, getStamina (int)
Returns the current value of the respective stat for this character.

### getName (String)
Returns this character's name. You should probably edit this when making your own characters.

### getSmallDescription (String)
Returns a short description for this character.

### getSmallDescriptionWithRelationship (String)
Returns the value of [**getSmallDescription**](#getSmallDescription%20(String)) with the character's special relationship string added at the end in color. You should probably not edit this.

### getBasePainThreshold, getBaseLustThreshold, getBaseMaxStamina (int)
Returns the base value for this character's respective stat threshold. This is then used to calculate the final value in the respective [**threshold functions**](#painThreshold,%20lustThreshold,%20getMaxStamina%20(int)).

### painThreshold, lustThreshold, getMaxStamina (int)
Returns the respective stat's final threshold value, based on its respective [**base threshold function**](#getBasePainThreshold,%20getBaseLustThreshold%20getBaseMaxStamina%20(int)), with a minimum value of 10 for pain and lust, and 0 for stamina.
## getPainLevel, getLustLevel, getStaminaLevel (float)
Returns this character's respective stat percentage, from 0.0 to 1.0 (inclusive).

### getAmbientPain, getAmbientLust (int)
Returns the ambient pain for this character, from their *buffsHolder* variable.

### addEffect (bool)
(effectID : String, args : Array = \[])
Tries adding the status effect with the *effectID* ID to the character, returning *true* if it's applied and *false* otherwise.
The percentage chance to not apply it is calculated from **getStatusEffectImmunity(effectID)** for this character, whose value is clamped between 0.0 and 1.0 (inclusive), multiplied by 100 (final chance 0% to 100%, inclusive). If it's applied, the *args* are passed onto the status effect.

### hasEffect (bool)
(effectID : String)
Returns *true* if the character has this status effect, otherwise it returns *false*.

### getEffect() (StatusEffectBase)
(effectID : String)
Returns this character's *effectID* status effect if they're afflicted by it, otherwise returns *null*.

### removeEffect (void)
(effectID : String)
Removes the status effect with the *effectID* ID from this character if they're afflicted by it, does nothing otherwise.

### canStandUpCombat (bool)
Returns whether this character can currently stand up in combat, affected by the *Collapsed* status effect and the perk *CombatBetterGetUp*.
Note that if the character doesn't have the *Collapsed* status effect, this will return *false*.

### getStatusEffects (Dictionary\[String, StatusEffectBase])
Returns the *statusEffects* dictionary, with the effects' IDs as keys and their objects (extending StatusEffectBase) as values.

### saveStatusEffectsData (Dictionary\[String, Dictionary])
Returns a dictionary with the character's status effects' IDs as keys and the result of their **saveData()** function as the values.

### loadStatusEffectsData (void)
(data : Dictionary\[String, Dictionary])
Removes every current status effect from this character using **removeEffect()** and then adds the status effects with IDs corresponding to *data*'s keys and, if the effect can be created, loads the effect's data using the value of the respective key.

### updateEffectPanel (void)
(panel : StatusEffectsPanel)
Updates the *panel* based on current status effects.

### processTimedBuffs (void)
(\_seconds : int)
Processes this character's *timedBuffs* by decrementing their duration by the *\_seconds* value, and sets the *timedBuffs* to only the ones that have a duration higher than 0.

### \_getAttacks (Array\[String])
Returns an array with the IDs of this character's attacks for all generic battles.

### \_getAttacksForBattle (Array\[String] or null)
(\_battlename : String)
Returns this character's attacks for a specific battle (based on *\_battlename*), returns *null* by default.
This function's return value will replace the character's attacks if it's different than *null* and the battle has the name \_battlename, otherwise the default attacks will be used (from [**\_getAttacks()**](#_getAttacks%20(Array%20[String]))).

### isDodging, isBlocking, isDefocusing (bool)
Returns *true* if the character is currently doing the respective action in combat.

### setFightingStateNormal, setFightingStateDodging, setFightingStateBlocking, setFightingStateDefocusing (void)
Sets this character's current fighting state to the specified state.

### getGender (Gender)
Returns this character's gender, which affects their char color by default.

### getPronounGender (Gender)
Returns this character's pronoun gender, which are derived from the [**getGender()**](#getGender%20(Gender)) function by default.
Gender affects pronouns as follows:
- Other: it/its
- Androgynous: they/them
- Female: she/her
- Male: he/him

### getChatColor (String)
Returns the chat color for this character, defaulting to one according to the value returned by the [**getGender()**](#getGender%20(Gender)) function, or red if the gender is invalid.
Return value can be a hex color code (eg. \#FFFFFF, \#f776ff), or a named color (see Godot's Color constants).
Default gender colors are the following:
- Other: \#77D86C
- Androgynous: \#BA82FF
- Female: \#FF837A
- Male: \#5696EA

### formatSay (String)
(text : String)
Returns the *text* string colored (using BBCode tags) to match this character's chat color (from [**getChatColor()**](#getChatColor%20(String))), with their speech modifiers applied.

### Various get\* functions (Various object types)
Returns the character's respective variable, with acceptable suffixes (and return object types) being: LustInterests, Inventory, SkillsHolder, BuffsHolder, FetishHolder, Personality.

### addExperience (void)
(newexp : int)
Adds *newexp* general experience to the character.
Emits the character's skillsHolder's *experienceChanged* signal.

### addSkillExperience (void)
(skillID : String, amount : int, activityID : null or String)
Adds a value of experience equal to *amount* to this character's skill with ID *skillID*.
If *activityID* is not null or an empty string, the experience will be added only once, and any further calls of this function with the same *skillID* and *activityID* will do nothing.
Emits the skill's *experienceChanged* (skill extends from SkillBase class) signal if experience is added.
If the character has no such skill, a new one will be created and assigned to them.

### hasPerk (bool)
(perkID : String)
Returns *true* if this character has the perk with ID *perkID*.
### getStat (int)
(statID : String)
Returns this character's stat value with ID *statID*.
Valid stats are Vitality, Agility, Sexiness and Strength in the base game (can be found as constants in the Stat class).