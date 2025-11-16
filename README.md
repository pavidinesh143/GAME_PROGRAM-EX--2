# GAME_PROGRAM-EX--2

##  Create a player movement using character, collectable, player health and score

# AIM
Create a playable third-person character in Unreal Engine that can move and run, collect coin-like collectibles, track a Score and Player Health, and display both on-screen (UI).

#  Overview:

# 1. Player Character (BP_PlayerCharacter) __
      handles movement, input, health, and overlaps with collectables.

# 2. Collectable (BP_Collectable) __ 
    simple actor with collision that gives score (and optionally health) when overlapped.     

# 3. UI (WBP_HUD) __
    UMG Widget showing Score and Health values.

# 4. GameMode / PlayerState __
    (optional) hold persistent Score/HighScore across respawns.

# 5. Game flow __
    pickup increments Score, maybe plays sound/particle and destroys the collectable; health decreases on damage, and player dies or respawns when health ≤ 0.


##  Step-by-step Implementation (Blueprint-first)

# 1. Project & Input Setup

  . Create a Third Person Blueprint project (or use your existing ThirdPersonMap).
  . Open Project Settings → Input and ensure these mappings exist:

 . MoveForward (W / Up arrow)
 . MoveRight (A/D or Left/Right)
 . Turn / LookUp (mouse)
 . Jump (SpaceBar)
 . Run (Left Shift) — optional if you want sprint

 # 2. Player Character Blueprint (BP_PlayerCharacter)

1. Duplicate the existing ThirdPersonCharacter (or create a new Character blueprint) and name it BP_PlayerCharacter.

2. Variables to add (Expose where useful):

.Score (Integer) — default 0
.MaxHealth (Float) — e.g. 100.0
.Health (Float) — default equal to MaxHealth
.bIsRunning (Boolean) — if you want sprinting

3. Movement (in Event Graph):

.Use Add Movement Input hooked to MoveForward and MoveRight axis mappings.
.Use Turn and LookUp to rotate camera.
.If using Run: on Run Pressed set Max Walk Speed on the Character Movement component (e.g. 1200) .and reset on Released (600 default).

4. Health functions:

. Function: ApplyDamage(float DamageAmount)

.Subtract DamageAmount from Health.
.If Health <= 0 → call OnDeath event (disable input, play animation, respawn or show Game Over).
.Update HUD (call event to update widget binding).

.Function: AddHealth(float HealAmount)
.Add to Health but clamp to MaxHealth.
.Update HUD.

5. Score management:

.Function: AddScore(int Amount)

.Score = Score + Amount → update HUD.

# BP_Collectable → OnComponentBeginOverlap (Sphere)

. Other Actor → Cast To BP_PlayerCharacter

. Branch (if cast success)

.Call AddScore(ScoreValue) on Player Character
.If GiveHealth > 0 Call AddHealth(GiveHealth)
.Play Sound at Location
.Spawn Emitter at Location
.Destroy Actor

# BP_PlayerCharacter → AddScore (Custom Event)

.Input: Amount (int)
.Score = Score + Amount
.Call UpdateScoreDisplay on the HUD widget reference
.(Optional) Play pickup sound, animate, or show floating text

# BP_PlayerCharacter → ApplyDamage (Custom Event)

.Input: Damage (float)

.Health = Health - Damage

.If Health <= 0
  .Call OnDeath (Disable Input; show Game Over)
  
. Update HUD: Call UpdateHealthDisplay

# OUTPUT

<img width="516" height="215" alt="513752969-18cea467-7cfa-486a-9383-af7393d665ac" src="https://github.com/user-attachments/assets/07aab355-3f98-4dda-b072-90e0beb9df34" />


<img width="1920" height="1200" alt="513753065-dfbcdcee-830a-4fce-a8c9-9f3b3c44bcd0" src="https://github.com/user-attachments/assets/22758a0b-2dc5-4208-96c7-65df67a375e2" />


<img width="1920" height="1200" alt="513753171-bc79e6c7-58ed-4bb6-9a15-6547fd4babcf" src="https://github.com/user-attachments/assets/c549e363-9d42-41bc-b807-b7af5edb24eb" />

<img width="1182" height="572" alt="513754121-8eb0b872-8f46-4c87-bb16-f130fe48f3d3" src="https://github.com/user-attachments/assets/9412b39a-b8ad-4a58-926c-ac74961d76ff" />


# RESULT

The AI character successfully roams within the defined NavMesh area, choosing random destinations at intervals using the Behavior Tree logic.














