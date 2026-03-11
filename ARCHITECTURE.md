# BP_Task Architecture (Blueprint Overview)

Project root: `C:\Users\User\Documents\Unreal Projects\BP_Task`

This document is based on actual assets and config currently present in the project.

## 1) Runtime entry points

- Default map: `/Game/ThirdPerson/Lvl_ThirdPerson`
- Default GameMode: `/Game/ThirdPerson/Blueprints/BP_ThirdPersonGameMode`
- GameInstance: `/Game/StructuralAssets/BP_GI`
- Input system: Enhanced Input (`EnhancedPlayerInput`, `EnhancedInputComponent`)

Source: `Config/DefaultEngine.ini`, `Config/DefaultInput.ini`.

## 2) Blueprint inventory (all found)

### Core gameplay

- `/Game/ThirdPerson/Blueprints/BP_ThirdPersonCharacter`
- `/Game/ThirdPerson/Blueprints/BP_ThirdPersonGameMode`
- `/Game/ThirdPerson/Blueprints/BP_ThirdPersonPlayerController`
- `/Game/StructuralAssets/BP_GI` (GameInstance)
- `/Game/StructuralAssets/BP_MyLibrary` (Blueprint Function Library)

### Interfaces

- `/Game/Blueprints/Interfaces/BPI_ForCharacter`
- `/Game/Input/Touch/BPI_TouchInterface`

### Tasks / OOP / mechanics

- `/Game/Blueprints/CoreTasks/BP_Crouched_Zone`
- `/Game/OOP/BP_Overlap_Base`
- `/Game/LevelPrototyping/Interactable/Door/BP_DoorFrame`
- `/Game/LevelPrototyping/Interactable/JumpPad/BP_JumpPad`
- `/Game/LevelPrototyping/Interactable/Target/BP_WobbleTarget`

### Main menu/UI

- `/Game/MainMenu/BP_MainMenuGM`
- `/Game/MainMenu/BP_MainMenuPC`
- `/Game/MainMenu/BP_MainMenuHud`
- `/Game/MainMenu/WBP_MainMenu`
- `/Game/MainMenu/WBP_NewGame`

### Save system

- `/Game/SaveGame/BP_SaveGame`
- `/Game/SaveGame/BP_MainSG`

### Animation Blueprints

- `/Game/Characters/Mannequins/Anims/Unarmed/ABP_Unarmed`
- `/Game/Mixamo/animation/ABP_Rosales`

## 3) Input architecture (current)

Input Mapping Context assets:

- `/Game/Input/IMC_Default`
- `/Game/Input/IMC_MouseLook`

Input Actions currently present:

- `/Game/Input/Actions/IA_Move`
- `/Game/Input/Actions/IA_Look`
- `/Game/Input/Actions/IA_Jump`
- `/Game/Input/Actions/IA_MouseLook`

Important note:

- There is no `IA_Crouch` asset in `Content/Input/Actions`.

## 4) High-level dependency graph

- `BP_GI` initializes global runtime state and is always alive across level loads.
- `BP_ThirdPersonGameMode` governs rules/spawn flow on `Lvl_ThirdPerson`.
- `BP_ThirdPersonPlayerController` manages player input routing to pawn/character.
- `BP_ThirdPersonCharacter` handles movement/gameplay actions.
- Character animation is delegated to AnimBP (`ABP_Unarmed` and/or `ABP_Rosales`, depending on mesh setup).
- Interactable actors (`BP_DoorFrame`, `BP_JumpPad`, `BP_WobbleTarget`, `BP_Crouched_Zone`) provide world mechanics.
- Shared contracts are exposed via `BPI_ForCharacter` and `BPI_TouchInterface`.
- Save/load logic is encapsulated in `BP_SaveGame` + `BP_MainSG`.
- Main menu flow is isolated in `MainMenu` blueprints and widgets.

## 5) Known architecture risks from current snapshot

- Crouch pipeline is incomplete at input layer: no `IA_Crouch` action found.
- Because BP assets are binary `.uasset`, exact graph-level call chains (event nodes/functions/variables) are not inspectable from filesystem text only.

## 6) What to give your programmer next

- Open these first in Unreal Editor:
  - `BP_ThirdPersonCharacter`
  - `BP_ThirdPersonPlayerController`
  - `IMC_Default`
  - `ABP_Rosales` or `ABP_Unarmed` (whichever is assigned to active mesh)
- Validate the full crouch chain:
  - Input Action exists (`IA_Crouch`)
  - Mapping exists in active IMC
  - Event binding exists in character/controller
  - `Can Crouch` enabled on CharacterMovement
  - AnimBP has crouch state transitions (`IsCrouched`)

## 7) Asset paths (absolute, for quick navigation)

- `C:\Users\User\Documents\Unreal Projects\BP_Task\Content\ThirdPerson\Blueprints\BP_ThirdPersonCharacter.uasset`
- `C:\Users\User\Documents\Unreal Projects\BP_Task\Content\ThirdPerson\Blueprints\BP_ThirdPersonGameMode.uasset`
- `C:\Users\User\Documents\Unreal Projects\BP_Task\Content\ThirdPerson\Blueprints\BP_ThirdPersonPlayerController.uasset`
- `C:\Users\User\Documents\Unreal Projects\BP_Task\Content\StructuralAssets\BP_GI.uasset`
- `C:\Users\User\Documents\Unreal Projects\BP_Task\Content\StructuralAssets\BP_MyLibrary.uasset`
- `C:\Users\User\Documents\Unreal Projects\BP_Task\Content\Blueprints\Interfaces\BPI_ForCharacter.uasset`
- `C:\Users\User\Documents\Unreal Projects\BP_Task\Content\Blueprints\CoreTasks\BP_Crouched_Zone.uasset`
- `C:\Users\User\Documents\Unreal Projects\BP_Task\Content\OOP\BP_Overlap_Base.uasset`
- `C:\Users\User\Documents\Unreal Projects\BP_Task\Content\LevelPrototyping\Interactable\Door\BP_DoorFrame.uasset`
- `C:\Users\User\Documents\Unreal Projects\BP_Task\Content\LevelPrototyping\Interactable\JumpPad\BP_JumpPad.uasset`
- `C:\Users\User\Documents\Unreal Projects\BP_Task\Content\LevelPrototyping\Interactable\Target\BP_WobbleTarget.uasset`
- `C:\Users\User\Documents\Unreal Projects\BP_Task\Content\MainMenu\BP_MainMenuGM.uasset`
- `C:\Users\User\Documents\Unreal Projects\BP_Task\Content\MainMenu\BP_MainMenuPC.uasset`
- `C:\Users\User\Documents\Unreal Projects\BP_Task\Content\MainMenu\BP_MainMenuHud.uasset`
- `C:\Users\User\Documents\Unreal Projects\BP_Task\Content\MainMenu\WBP_MainMenu.uasset`
- `C:\Users\User\Documents\Unreal Projects\BP_Task\Content\MainMenu\WBP_NewGame.uasset`
- `C:\Users\User\Documents\Unreal Projects\BP_Task\Content\SaveGame\BP_SaveGame.uasset`
- `C:\Users\User\Documents\Unreal Projects\BP_Task\Content\SaveGame\BP_MainSG.uasset`
- `C:\Users\User\Documents\Unreal Projects\BP_Task\Content\Characters\Mannequins\Anims\Unarmed\ABP_Unarmed.uasset`
- `C:\Users\User\Documents\Unreal Projects\BP_Task\Content\Mixamo\animation\ABP_Rosales.uasset`
