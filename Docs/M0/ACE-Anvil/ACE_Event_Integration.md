# ACE Event Integration Document

## Document Status

**Status:** Draft – WP-M0-07 verified baseline  
**Workpackage:** WP-M0-07 Event & Hook Mapping  
**Project:** ACE Critical Care  
**Base Framework:** ACE Anvil Medical System  

## 1. Purpose

This document defines the verified ACE Anvil touchpoints that ACE Critical Care may use during later implementation milestones.

It documents:

- existing ACE medical state access points
- existing replicated state
- existing callbacks and `ScriptInvoker` hooks
- available method-extension points
- available medical action patterns
- authority and synchronization behavior observed in the inspected ACE code
- integration risks
- missing hooks
- required future prototypes

This document does not authorize direct ACE source modification.

ACE Critical Care must remain a separate addon and extend ACE Anvil through the least invasive verified integration path available.

---

## 2. Evidence Boundary

The findings in this document are based on inspected code from the loaded ACE Anvil medical modules, including:

- ACE Medical Core
- ACE Medical Breathing
- ACE Medical Circulation
- ACE Medical Bleeding
- relevant ACE Medical Pain behavior inspected during WP-M0-06
- existing medical user actions, consumable effects, damage effects and networking components

Only behavior directly visible in the inspected code is classified as verified.

Where no hook or replication mechanism was found, this document records:

- `No Hook Found`
- `Not Verified`
- `Prototype Required`

Absence from the inspected code does not prove that no implementation exists elsewhere in ACE Anvil.

---

## 3. Integration Classification System

| Classification | Meaning |
|---|---|
| **Verified Hook** | A subscribable callback or `ScriptInvoker` was found and its use is visible in existing ACE code. |
| **Verified State Access** | Existing state can be read through a verified getter or component. |
| **Verified Mutation Point** | An existing setter or authoritative mutation method was found. |
| **Verified Method Extension Point** | A method is already extended through `modded class` and may technically be extended again. |
| **Verified Action Pattern** | A reusable medical action structure was identified. |
| **Verified Replication** | A property is marked with `RplProp` or an explicit replication update is visible. |
| **Verified Server Authority** | The inspected code explicitly limits logic to server or authority. |
| **No Hook Found** | No dedicated subscribable event or `ScriptInvoker` was found in the inspected code. |
| **Prototype Required** | Runtime behavior must be validated before implementation. |
| **High Risk** | The integration point touches central ACE state, simulation, damage or state-machine logic. |

---

## 4. Global Integration Rules

### 4.1 Preferred Integration Order

ACE Critical Care should select integration methods in this order:

1. Existing `ScriptInvoker` or event hook
2. Read existing ACE state through public getters
3. Add ACECC-owned components, adapters or systems
4. Use existing ACE action and consumable-effect patterns
5. Use a carefully scoped `modded class` extension
6. Avoid replacing central ACE simulation, state machines or damage pipelines

### 4.2 Mandatory Rules

- ACECC must not directly edit ACE Anvil source files.
- ACECC must not replace the ACE vital-state machine.
- ACECC must not replace the ACE bleeding pipeline.
- ACECC must not independently own baseline unconsciousness, damage or bleeding truth.
- Critical medical mutations must remain server-authoritative.
- Client-side UI and effects must not become authoritative gameplay logic.
- Every `modded class` extension must call `super` unless a verified reason explicitly requires otherwise.
- Missing hooks must be recorded instead of silently worked around.

---

## 5. Touchpoint Overview Matrix

| Touchpoint | Verified Hook | Verified State Access | Verified Mutation | Recommended Integration | Risk |
|---|---|---|---|---|---|
| Breathing | Pneumothorax `ScriptInvoker` | Yes | Yes | Read ACE vitals and subscribe to existing pneumothorax event | Low to Medium |
| Bleeding | No dedicated bleeding event found | Yes | Internal mutation points exist | Read ACE bleeding state; avoid pipeline replacement | High for mutation |
| Unconsciousness | No `ScriptInvoker`; replicated callback exists | Yes | Authoritative reposition method exists | Read pose; extend callback only when required | Medium |
| Damage Pipeline | No general damage event found | Yes | Damage effects and hit-zone methods exist | Use targeted damage effects or scoped extensions | Medium to High |
| Medical Actions | Multiple action patterns verified | Yes | Through actions/effects | Create ACECC-owned actions following verified patterns | Medium |
| Patient State | No vital-state changed event found | Yes | Replicated setter exists | Read ACE state; use adapter/prototype for reactions | High for FSM changes |

---

# 6. Breathing Systems Touchpoint

## 6.1 ACE-Owned Responsibility

The inspected ACE Breathing module owns and updates:

- airway obstruction state
- airway occlusion state
- pneumothorax scale
- tension pneumothorax state
- respiratory rate
- SpO2
- alveolar oxygen pressure
- alveolar carbon dioxide pressure
- venous carbon dioxide concentration
- breathing-related physiological calculations
- breathing-related damage and deterioration behavior

ACECC must treat these as ACE-owned baseline breathing states.

---

## 6.2 Central Component

### Class

```c
ACE_Medical_VitalsComponent
```

### Verified State Access

```c
GetRespiratoryRate()
GetSpO2()
GetPalvO2()
GetPalvCO2()
GetCvenCO2()
GetPneumothoraxScale()
HasTensionPneumothorax()
IsAirwayObstructed()
IsAirwayOccluded()
CanBreath()
GetTidalVolume()
GetCapacityVolume()
```

### Classification

- Verified State Access
- Preferred read integration point
- Low risk when used read-only

ACECC-owned systems may read these values as inputs. Reading these values does not authorize ACECC to replace the underlying ACE calculations.

---

## 6.3 Replicated Breathing State

The inspected class contains:

```c
[RplProp()]
protected bool m_bIsAirwayObstructed;

[RplProp()]
protected bool m_bIsAirwayOccluded;

[RplProp(onRplName: "OnPneumothoraxStateChanged")]
protected float m_fPneumothoraxScale;

[RplProp(onRplName: "OnPneumothoraxStateChanged")]
protected bool m_bHasTensionPneumothorax;
```

### Verified Replication Behavior

Mutation methods call `Replication.BumpMe()`:

```c
SetIsAirwayObstructed(...)
SetIsAirwayOccluded(...)
SetPneumothoraxScale(...)
SetHasTensionPneumothorax(...)
```

### Classification

- Airway obstruction: Verified Replication
- Airway occlusion: Verified Replication
- Pneumothorax scale: Verified Replication
- Tension pneumothorax: Verified Replication

Respiratory rate, SpO2, PalvO2, PalvCO2 and CvenCO2 were not marked as `RplProp` in the inspected class.

---

## 6.4 Verified Pneumothorax Hook

### Hook

```c
ScriptInvoker<float, bool> GetOnPneumothoraxStateChanged()
```

### Event Payload

- `float scale`
- `bool hasTension`

### Existing Use Pattern

Existing ACE UI code subscribes and unsubscribes through:

```c
vitals.GetOnPneumothoraxStateChanged().Insert(...)
vitals.GetOnPneumothoraxStateChanged().Remove(...)
```

### Trigger Behavior

`OnPneumothoraxStateChanged()` is called:

- directly after state mutation
- through `RplProp` callbacks when the replicated pneumothorax state changes

### Classification

- Verified Hook
- Preferred event-driven integration point
- Low compatibility risk

### ACECC Decision

ACECC systems requiring pneumothorax updates should subscribe to this existing `ScriptInvoker` instead of polling or overriding central pneumothorax logic.

---

## 6.5 Airway Obstruction and Occlusion

### Verified Mutation Points

```c
SetIsAirwayObstructed(bool isObstructed)
SetIsAirwayOccluded(bool isOccluded)
```

Both methods:

- update replicated state
- call `Replication.BumpMe()`
- call `OnUpdateCanBreathServer()`

### Missing Hook

No dedicated airway-obstruction or airway-occlusion `ScriptInvoker` was found.

The `RplProp` declarations do not use `onRplName`.

### Classification

- Verified State Access
- Verified Mutation Point
- Verified Replication
- No Hook Found
- Prototype Required for event-driven ACECC reaction

### ACECC Decision

ACECC may read obstruction and occlusion state.

ACECC must not assume a state-change callback exists.

A later prototype must determine whether ACECC needs:

- a dedicated ACECC adapter
- a carefully scoped method extension
- or another supported observation mechanism

---

## 6.6 Central Breathing Calculation Pipeline

### Class

```c
ACE_Medical_IVitalState
```

### Verified Calculation Methods

```c
OnUpdate(...)
ComputeRespiratoryRate(...)
UpdateOxygenMetabolism(...)
UpdateVentilation(...)
UpdatePerfusion(...)
ComputePalvO2(...)
ComputeSpO2(...)
ComputeCvenCO2(...)
ComputePalvCO2(...)
```

### Classification

- Verified Method Extension Point
- Central simulation pipeline
- High Risk

### ACECC Decision

ACECC should not override the central breathing calculation pipeline by default.

Any later override requires:

- explicit feature requirement
- technical prototype
- multiplayer validation
- compatibility review
- documented reason why no safer adapter or external system is sufficient

---

## 6.7 Breathing Lifecycle and Damage Touchpoints

Verified methods include:

```c
SCR_CharacterControllerComponent.ACE_Medical_OnUnconsciousPoseChanged()
SCR_CharacterControllerComponent.OnLifeStateChanged(...)
SCR_CharacterDamageManagerComponent.ACE_Medical_OnKilled()
SCR_CharacterDamageManagerComponent.ACE_Medical_UpdateResilienceRegenScale()
ACE_Medical_CharacterChestHitZone.OnDamage(...)
```

These methods connect breathing with:

- unconscious pose
- vomiting registration
- death
- resilience regeneration
- chest trauma
- pneumothorax generation

### Classification

- Verified Method Extension Points
- Medium to High risk
- Not preferred when a dedicated hook exists

---

## 6.8 Breathing Result Notifications

### Verified Notification Types

```c
ENotification.ACE_MEDICAL_BREATHING_RESULT
ENotification.ACE_MEDICAL_SPO2_RESULT
```

### Verified Data Source

```c
ACE_Medical_NetworkComponent.GetPatientStateNotificationData(...)
```

Values are read from:

```c
patientContext.m_pVitals.GetRespiratoryRate()
patientContext.m_pVitals.GetSpO2()
```

### Classification

- Notification output path: Verified
- Continuous replication behavior: Not Verified
- Triggering interaction path: Partially Verified through `ACE_Medical_CheckVitalsUserAction`
- Prototype Required for monitor synchronization

### ACECC Decision

ACECC must not assume that all breathing values are continuously available on all clients.

Monitoring implementation requires a later networking prototype.

---

# 7. Bleeding Systems Touchpoint

## 7.1 ACE-Owned Responsibility

The inspected ACE Bleeding module owns:

- centralized blood-loss application
- total bleeding-rate calculation
- bleeding effect lifecycle
- tourniquet influence on bleeding
- bleed-out evaluation
- blood-hit-zone damage caused by bleeding

ACECC must not replace this pipeline.

---

## 7.2 Central Blood-Loss Pipeline

### Central Effect

```c
ACE_Medical_BloodLossDamageEffect
```

This effect:

- redirects blood loss to the blood hit zone
- calculates effective total blood loss
- applies centralized bleeding DOT

### Supporting Effect Extension

```c
modded class SCR_BleedingDamageEffect
```

Verified behavior:

- starts `ACE_Medical_BloodLossDamageEffect` when needed
- updates total bleeding amount when effects are added
- updates total bleeding amount when effects are removed
- updates total bleeding amount when an existing effect is hijacked
- disables original per-effect DOT processing because central blood-loss handling replaces it

### Classification

- Central ACE pipeline
- High Risk
- Do not replace

---

## 7.3 Verified Bleeding State Access

### Class

```c
SCR_CharacterBloodHitZone
```

### Getter

```c
override float GetTotalBleedingAmount()
```

### Internal Recalculation Method

```c
ACE_Medical_UpdateTotalBleedingAmount()
```

### Classification

- `GetTotalBleedingAmount()`: Verified State Access
- `ACE_Medical_UpdateTotalBleedingAmount()`: Internal Mutation/Recalculation Point
- Read access preferred
- Direct mutation not preferred

---

## 7.4 Bleeding Calculation Extension

### Method

```c
SCR_CharacterHitZone.ACE_Medical_CalculateBleedingRate()
```

### Current Calculation

The inspected implementation calculates bleeding rate from:

- hit-zone health scale
- maximum bleeding rate

### Classification

- Verified Method Extension Point
- High Risk if overridden
- Changes affect central bleeding behavior

### ACECC Decision

ACECC must not override bleeding-rate calculation without explicit later scope and prototype validation.

---

## 7.5 Tourniquet Touchpoint

### Method

```c
SCR_CharacterDamageManagerComponent.SetTourniquettedGroup(...)
```

After the base implementation, ACE recalculates total bleeding amount.

### Classification

- Verified Method Extension Point
- Existing bleeding lifecycle touchpoint
- Medium risk

---

## 7.6 Bleed-Out Authority

### Method

```c
SCR_CharacterBloodHitZone.OnDamageStateChanged(...)
```

The inspected implementation only performs bleed-out death handling when:

```c
Replication.IsServer()
```

and the blood hit zone becomes `DESTROYED`.

### Classification

- Verified Server Authority
- Critical outcome
- ACECC must not perform client-authoritative bleed-out decisions

---

## 7.7 Missing Bleeding Event

No dedicated bleeding-state `ScriptInvoker` or event was found in the inspected bleeding code.

No dedicated hook such as the following was found:

```c
GetOnBleedingChanged()
OnTotalBleedingAmountChanged()
```

The total bleeding amount was not marked as `RplProp` in the inspected class.

### Classification

- No Hook Found
- Direct total-rate replication: Not Verified
- Prototype Required if ACECC requires event-driven bleeding updates

### ACECC Decision

ACECC should read existing bleeding state when required.

ACECC must not replace the bleeding pipeline to obtain notifications.

---

# 8. Unconsciousness Touchpoint

## 8.1 Replicated Unconscious Pose

### State

```c
[RplProp(onRplName: "ACE_Medical_OnUnconsciousPoseChanged")]
protected ACE_Medical_EUnconsciousPose m_eACE_Medical_UnconsciousPose;
```

### Verified States

```c
NONE
BACK
BELLY
BELLY_UP
LEFT_SIDE
RIGHT_SIDE
```

### Classification

- Verified Replication
- Verified State Access
- Replicated callback exists

---

## 8.2 Pose Read and Mutation

### Getter

```c
ACE_Medical_GetUnconsciousPose()
```

### Mutation Method

```c
ACE_Medical_Reposition(ACE_Medical_EUnconsciousPose pose)
```

The mutation method:

- updates the pose
- calls `Replication.BumpMe()`
- calls `ACE_Medical_OnUnconsciousPoseChanged()`

### Classification

- Getter: Verified State Access
- Reposition method: Verified Mutation Point
- Mutation must remain authoritative

---

## 8.3 Pose-Changed Callback

### Callback

```c
ACE_Medical_OnUnconsciousPoseChanged()
```

The base callback updates the animation variable only on:

- owner
- authority

The Breathing module already extends this callback through `modded class` and calls `super`.

### Classification

- Verified Method Extension Point
- Not a subscribable `ScriptInvoker`
- Medium compatibility risk

### ACECC Decision

ACECC should read pose through the getter.

ACECC may extend `ACE_Medical_OnUnconsciousPoseChanged()` only when a dedicated reaction is required and no cleaner adapter is available.

Every extension must call `super`.

---

## 8.4 Animation Event Authority

### Method

```c
OnAnimationEvent(...)
```

The inspected implementation only handles the unconscious-pose animation event on:

```c
RplRole.Authority
```

It then calls `ACE_Medical_Reposition(...)`.

### Classification

- Verified Authority
- Pose changes are authoritative
- Clients must not independently finalize pose state

---

## 8.5 Reposition User Action

### Class

```c
ACE_Medical_RepositionUserAction
```

Verified behavior:

- validates user and target state
- checks vehicles, swimming, falling and ragdoll state
- revalidates critical conditions during `PerformAction`
- disables client execution through `CanBroadcastScript() == false`
- calls the authoritative reposition method

### Classification

- Verified Action Pattern
- Server-side state mutation pattern
- Suitable reference for non-item medical interactions

---

## 8.6 Life-State Touchpoint

### Method

```c
OnLifeStateChanged(...)
```

Multiple ACE medical modules extend this method for:

- vomit-system registration
- revive-history cleanup
- resilience regeneration updates
- second-chance behavior

The inspected medical logic performs server checks before critical state changes.

### Classification

- Verified Method Extension Point
- No dedicated ACE life-state `ScriptInvoker` found
- Medium to High compatibility risk due to multiple extensions

### ACECC Decision

ACECC should avoid adding broad logic directly to `OnLifeStateChanged(...)` unless required.

Where possible, ACECC should use an ACECC-owned adapter that reacts only to the specific state transition needed.

---

## 8.7 Unconsciousness Ownership Decision

ACE and Reforger retain ownership of:

- life state
- unconsciousness
- unconscious pose
- pose replication
- pose-related animation update

ACECC may:

- read the current pose
- react to pose or life-state changes through carefully scoped extensions
- request authoritative repositioning through supported action flows

ACECC must not replace the baseline unconsciousness pipeline.

---

# 9. Damage Pipeline Touchpoint

## 9.1 Verified General Entry Points

```c
SCR_CharacterDamageManagerComponent.OnDamage(...)
SCR_CharacterDamageManagerComponent.OnDamageStateChanged(...)
SCR_CharacterBloodHitZone.OnDamageStateChanged(...)
ACE_Medical_CharacterChestHitZone.OnDamage(...)
SCR_BleedingDamageEffect.OnEffectAdded(...)
SCR_BleedingDamageEffect.OnEffectRemoved(...)
SCR_BleedingDamageEffect.HijackDamageEffect(...)
SCR_InstantDamageEffect.OnEffectAdded(...)
SCR_ConsumableEffectHealthItems.ApplyEffect(...)
```

### Classification

- Verified Method Extension Points
- No general subscribable damage event found
- Medium to High risk depending on method

---

## 9.2 Server-Authoritative Damage Decisions

Verified server checks exist in critical medical damage handling, including:

- last struck physical hit-zone processing
- killed-state handling
- bleed-out death handling
- pneumothorax generation from chest damage
- brain-hit-zone death handling
- ACE-specific resilience damage and regeneration scaling

### ACECC Decision

ACECC must not produce critical medical outcomes from client-only damage processing.

Any ACECC damage extension must preserve server authority.

---

## 9.3 Damage-Effect Pattern

ACE uses damage-effect classes to represent and apply treatment or injury consequences.

Verified examples include:

```c
ACE_Medical_BloodLossDamageEffect
ACE_Medical_ChestSealDamageEffect
ACE_Medical_LaryngealTubeDamageEffect
```

### Classification

- Verified integration pattern
- Suitable for scoped treatment or injury consequences
- Safer than replacing central damage-manager logic when applicable

### ACECC Decision

Where an ACECC intervention can be represented as a scoped effect, an ACECC-owned damage/effect class should be preferred over broad damage-manager overrides.

---

## 9.4 Damage Pipeline Risk

The damage pipeline is already extended by several ACE modules.

Multiple `modded class` layers may depend on:

- call order
- correct `super` usage
- server-side execution
- existing ACE side effects

### Classification

- High compatibility sensitivity
- Prototype Required for every broad damage-pipeline extension

---

# 10. Medical Actions Touchpoint

## 10.1 Verified Action Pattern A: Consumable-Based Treatment

### Examples

```c
ACE_Medical_EpinephrineUserAction
ACE_Medical_ChestSealUserAction
ACE_Medical_AmmoniumCarbonateUserAction
```

### Flow

```text
UserAction
→ obtain consumable component
→ call ConsumableEffect.CanApplyEffect(...)
→ map fail reason to UI feedback
→ existing consumable infrastructure applies effect
```

### Verified Effect Classes

```c
ACE_Medical_ConsumableEpinephrine
ACE_Medical_ConsumableMorphine
ACE_Medical_ConsumableAmmoniumCarbonate
ACE_Medical_ConsumableMedication
```

### Classification

- Verified Action Pattern
- Preferred for item-based ACECC treatments
- Medium risk

### ACECC Decision

ACECC item-based medical interventions should use ACECC-owned consumable effects and actions following this verified pattern.

---

## 10.2 Verified Action Pattern B: Direct Scripted Medical Action

### Examples

```c
ACE_Medical_RepositionUserAction
ACE_Medical_ClearVomitAction
ACE_Medical_TiltHeadUserAction
```

### Flow

```text
CanBeShownScript(...)
→ validate visibility and basic interaction conditions

PerformAction(...)
→ obtain target component
→ revalidate critical target state
→ perform authoritative mutation

CanBroadcastScript() == false
→ clients do not independently execute the state mutation
```

### Classification

- Verified Action Pattern
- Suitable for non-item medical interventions
- Requires server-side revalidation

---

## 10.3 Verified Action Pattern C: Local Examination with Network Request

### Class

```c
ACE_Medical_CheckVitalsUserAction
```

### Flow

```text
local action
→ find ACE_Medical_NetworkComponent
→ RequestPatientStateNotification(...)
→ receive/display requested patient information
```

### Local Behavior

```c
HasLocalEffectOnlyScript() == true
```

### Classification

- Verified Action Pattern
- Suitable reference for examination and requested patient-data display
- Does not prove continuous vital replication

---

## 10.4 Verified Action Pattern D: Complex Server-Side Action with Animation

### Class

```c
ACE_Medical_CPRUserAction
```

Verified behavior includes:

- visibility checks
- additional performability checks
- patient-pose validation
- obstruction trace
- server-side revalidation
- helper-compartment animation
- prevention of duplicate CPR
- disabled client execution

### Classification

- Verified Action Pattern
- Suitable reference for complex synchronized interventions
- Higher implementation complexity

---

## 10.5 Action Validation Rules for ACECC

Every ACECC action must separate:

### Visibility Checks

Used for whether the action should be shown.

### Performability Checks

Used for current interaction validity and user-facing failure reasons.

### Server-Side Revalidation

Used before authoritative state mutation.

### Effect Execution

Implemented through the appropriate ACECC-owned effect, component or authoritative system.

### Networking Behavior

Selected based on whether the action is:

- server-authoritative
- local examination only
- synchronized animation
- item-based treatment

---

# 11. Patient State Touchpoint

## 11.1 Central Component

### Class

```c
ACE_Medical_VitalsComponent
```

This is the central inspected ACE patient-vitals component.

### Verified Replicated State

```c
[RplProp()]
protected ACE_Medical_EVitalStateID m_eVitalStateID;

[RplProp()]
protected bool m_bIsCPRPerformed;
```

### Classification

- Verified State Access
- Verified Replication

---

## 11.2 Vital State Values

```c
STABLE
UNSTABLE
CRITICAL
RESUSCITATION
CARDIAC_ARREST
```

### Getter

```c
GetVitalStateID()
```

The inspected code explicitly states that clients may call this getter.

### Classification

- Verified State Access
- Preferred read integration point

---

## 11.3 Vital-State Mutation

### Method

```c
SetVitalStateID(ACE_Medical_EVitalStateID newStateID)
```

The method:

- exits when state is unchanged
- stores the new state
- calls `Replication.BumpMe()`

The method itself does not contain an explicit server check.

### Classification

- Verified Mutation Point
- Verified Replication
- Unsafe for unrestricted client use
- High risk when called outside existing ACE flows

### ACECC Decision

ACECC must not directly force ACE vital-state changes without a specifically validated integration requirement.

---

## 11.4 Server-Side Vital-State System

### Classes

```c
ACE_Medical_VitalStateMachine
ACE_Medical_VitalStatesSystem
```

The inspected world system is configured with:

```c
SetLocation(WorldSystemLocation.Server)
```

The system creates and runs the ACE state machine and transitions.

### Verified State-Machine Flow

```text
STABLE
↔ UNSTABLE
↔ CRITICAL
→ CARDIAC_ARREST
↔ RESUSCITATION
```

### Classification

- Verified Server Authority
- Central ACE state machine
- High Risk

### ACECC Decision

ACECC must not replace the ACE vital-state machine.

ACECC-owned state must remain modular and use ACE state as input where required.

---

## 11.5 Missing Vital-State Event

The inspected `m_eVitalStateID` property has no `onRplName`.

No dedicated hook was found such as:

```c
GetOnVitalStateChanged()
OnVitalStateChanged()
```

### Classification

- No Hook Found
- State read and replication are available
- Prototype Required for event-driven ACECC reactions

### ACECC Decision

A later prototype must validate an ACECC-owned patient-state adapter if ACECC requires reliable change notifications.

---

## 11.6 Individual Vital Values

Verified getters include:

```c
GetHeartRate()
GetCardiacOutput()
GetSystemicVascularResistance()
GetMeanArterialPressure()
GetPulsePressure()
GetBloodPressures()
GetVitalStateID()
IsCPRPerformed()
WasRevived()
```

Most inspected individual vital values are not directly marked as `RplProp`.

The inspected code states:

```c
Updates to vitals are mostly server side right now.
Clients can request values for vitals via ACE_Medical_NetworkComponent.
```

### Classification

- Server-side vital calculation: Verified
- Direct replication of all vital values: Not Verified
- Request-based client access: Verified
- Prototype Required for ACECC monitor synchronization

---

# 12. Cross-System Integration Flows

## 12.1 Unconscious Pose to Airway Obstruction

```text
Authority changes unconscious pose
→ pose is replicated
→ ACE_Medical_OnUnconsciousPoseChanged()
→ Breathing extension evaluates obstruction chance
→ ACE_Medical_VitalsComponent.SetIsAirwayObstructed(...)
→ replicated airway state updates
→ breathing ability and resilience regeneration are updated
```

### ACECC Relevance

ACECC must preserve this flow when adding airway-related systems.

ACECC must not independently duplicate unconscious-pose obstruction logic.

---

## 12.2 Chest Damage to Pneumothorax

```text
Chest hit-zone receives qualifying server-side damage
→ ACE settings and probability are evaluated
→ pneumothorax or tension pneumothorax state changes
→ Replication.BumpMe()
→ pneumothorax callback and ScriptInvoker execute
→ UI or subscribed systems update
```

### ACECC Relevance

The existing pneumothorax `ScriptInvoker` is the preferred observation point.

---

## 12.3 Bleeding to Bleed-Out

```text
Bleeding effects are added/updated/removed
→ total bleeding amount is recalculated
→ central blood-loss effect applies damage to blood hit zone
→ blood hit zone reaches destroyed state
→ server validates bleed-out rules
→ server kills patient when permitted
```

### ACECC Relevance

ACECC may read bleeding state but must not replace the central pipeline.

---

## 12.4 Vital State to UI Effects

```text
ACE server-side vital-state machine updates vital state
→ vital state is replicated
→ client-side UI/screen effect reads GetVitalStateID()
→ local presentation changes
```

### ACECC Relevance

UI may read replicated state but must not own medical truth.

---

## 12.5 Local Examination to Networked Patient Data

```text
local user performs check-vitals action
→ action requests patient-state notification
→ ACE_Medical_NetworkComponent resolves requested data
→ result is displayed to requesting player
```

### ACECC Relevance

This is the verified baseline for request-based medical examination.

It is not sufficient evidence for continuous monitor synchronization.

---

# 13. ACECC Adapter Requirements

The following adapters are candidates for later implementation. They are not yet implemented or verified.

## 13.1 Patient-State Adapter

### Purpose

Provide ACECC-owned reactions to ACE vital-state changes without replacing the ACE state machine.

### Reason

No dedicated vital-state changed event was found.

### Required Prototype

- determine safe observation strategy
- verify server/client behavior
- verify JIP behavior
- avoid per-frame polling unless justified

---

## 13.2 Airway-State Adapter

### Purpose

Provide ACECC-owned reactions to airway obstruction and occlusion changes.

### Reason

The states are replicated, but no dedicated state-change hook was found.

### Required Prototype

- determine whether a scoped setter extension is safe
- determine whether an ACECC-owned callback layer is required
- verify remote-client behavior

---

## 13.3 Bleeding-State Adapter

### Purpose

Expose bleeding-state changes to ACECC systems without replacing the bleeding pipeline.

### Reason

No dedicated bleeding-state event was found.

### Required Prototype

- determine whether read-only periodic evaluation is sufficient
- determine whether a carefully scoped update-method extension is safe
- verify total bleeding visibility and authority

---

## 13.4 Monitor Networking Adapter

### Purpose

Provide synchronized monitor-visible patient data.

### Reason

Most individual vital values are calculated server-side and were not directly replicated in the inspected code.

### Required Prototype

- define which values require continuous or sampled synchronization
- determine visibility rules
- determine request versus persistent monitor stream
- validate bandwidth and update frequency
- validate JIP and device ownership

---

# 14. Risk Register

| Risk | Cause | Impact | Mitigation |
|---|---|---|---|
| ACE update breaks ACECC extension | Broad `modded class` dependency | Compile or runtime failure | Prefer hooks/getters; isolate extensions |
| Multiple mods extend same method | Shared `modded class` touchpoint | Order-dependent behavior | Always call `super`; minimize overrides |
| ACECC replaces medical truth | Independent state-machine or bleeding logic | Desync and incompatibility | Keep ACE baseline authoritative |
| Client mutates critical state | Unsafe direct setter use | Multiplayer desync/exploit | Server-side validation and authority rules |
| Monitor assumes local vital availability | Non-replicated server-side values | Incorrect or stale UI | Build dedicated networking prototype |
| Excessive polling | Missing events | Performance cost | Prototype adapters and event bridges |
| Duplicate physiology | ACECC recomputes ACE-owned values | Conflicting medical behavior | Read ACE state and own only ACECC extensions |
| Missing JIP restoration | Adapter/event-only state | Incorrect late-join state | Explicit JIP testing in later replication work |

---

# 15. Prototype Requirements

The following prototypes are required before production implementation:

| Prototype | Purpose | Planned Handover |
|---|---|---|
| Vital-State Change Adapter | React safely to ACE vital-state changes | M1 |
| Airway-State Change Adapter | React to obstruction/occlusion updates | M1/M2 |
| Bleeding Observation Adapter | Observe bleeding changes without replacing pipeline | M1/M4 |
| Monitor Networking Prototype | Synchronize monitor-visible vital values | M1/M3 |
| ACECC Medical Action Prototype | Validate one ACECC-owned server-authoritative action | M1 |
| Dedicated Server and JIP Validation | Confirm authority, replication and late join | M1 |

---

# 16. Handover to Later Milestones

## M1 – Core Medical Framework

Use this document to implement and validate:

- ACECC-owned adapter boundaries
- ACECC patient-state extension structure
- networking and replication prototypes
- one representative ACECC medical action
- dedicated-server and JIP tests

## M2 – Airway and Ventilation

Use:

- breathing state getters
- pneumothorax `ScriptInvoker`
- unconscious-pose integration
- verified airway action patterns
- validated airway-state adapter

## M3 – Monitoring and Defibrillation

Use:

- patient-state getters
- request-based ACE network behavior
- monitor networking prototype
- CPR-state replication
- server-authoritative cardiac state

## M4 – Trauma and Critical Care

Use:

- bleeding read access
- targeted damage-effect pattern
- damage-pipeline mapping
- bleeding observation adapter
- high-risk override restrictions

---

# 17. WP-M0-07 Result

The inspected ACE Anvil medical modules expose a mixture of:

- safe read access through existing components and getters
- selected replicated properties
- one verified breathing-related `ScriptInvoker`
- replicated callbacks
- established medical action patterns
- server-authoritative simulation systems
- multiple method-extension touchpoints
- central pipelines that must not be replaced

The preferred ACE Critical Care integration strategy is:

1. read ACE-owned baseline medical state
2. subscribe to existing events where available
3. add ACECC-owned modular adapters and systems
4. follow verified ACE action and effect patterns
5. use `modded class` extensions only when no safer path exists
6. preserve ACE server authority and replication ownership
7. validate missing-hook areas through focused prototypes before production implementation

---

## 18. Open Questions

The following questions remain intentionally unresolved:

- What is the safest runtime observation method for vital-state changes?
- What is the safest runtime observation method for airway obstruction and occlusion changes?
- How should total bleeding changes be exposed to ACECC without replacing ACE bleeding behavior?
- Which vital values must be synchronized continuously for monitor devices?
- Which values should be request-based instead?
- How does the selected adapter strategy behave for JIP clients?
- Which ACECC actions require shared animation state?
- Which future ACE updates may change the inspected extension points?

These questions require later technical prototypes and must not be answered through assumptions.
