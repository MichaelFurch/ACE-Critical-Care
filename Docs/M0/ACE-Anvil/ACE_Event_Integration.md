# ACE Event Integration

## Breathing System Touchpoint Mapping

### ACE Medical Vitals Component

Class: `ACE_Medical_VitalsComponent`

Verified state access:
- GetRespiratoryRate()
- GetSpO2()
- GetPalvO2()
- GetPalvCO2()
- GetCvenCO2()
- GetPneumothoraxScale()
- HasTensionPneumothorax()
- IsAirwayObstructed()
- IsAirwayOccluded()
- CanBreath()

Preferred use:
ACECC-owned systems may read existing ACE breathing state through the vitals component.

### Pneumothorax State Changed Event

Verified hook:
`ScriptInvoker<float, bool> GetOnPneumothoraxStateChanged()`

Event data:
- Pneumothorax scale
- Tension pneumothorax state

The event is triggered through replicated-property callbacks and direct state changes.

Integration classification:
- Status: Verified Hook
- Method: Existing ScriptInvoker
- Compatibility risk: Low
- Preferred integration method: Yes

### Airway Obstruction and Occlusion

Verified replicated state:
`[RplProp()] protected bool m_bIsAirwayObstructed;`
`[RplProp()] protected bool m_bIsAirwayOccluded;`

No dedicated state-changed event was identified during the initial inspection.

Integration classification:
- Status: Existing state, no verified event hook
- Read access: Verified
- Event access: Not found
- Prototype validation required: Yes

### Respiratory Rate and Oxygenation Values

The inspected vitals component contains getters and setters for respiratory rate, SpO2, and various pressures. These values are not marked as RplProp in the inspected class.

ACE Medical Network Component exposes notification data for:
- ACE_MEDICAL_BREATHING_RESULT
- ACE_MEDICAL_SPO2_RESULT

Integration classification:
- Direct replication behavior: Not verified
- Notification access: Verified
- Monitor synchronization strategy: Requires later validation

### Breathing Calculation Pipeline

Verified central calculation class: `ACE_Medical_IVitalState`
Relevant methods: OnUpdate, ComputeRespiratoryRate, UpdateOxygenMetabolism, UpdateVentilation, UpdatePerfusion, ComputePalvO2, ComputeSpO2, ComputeCvenCO2, ComputePalvCO2.

Integration classification:
- Status: Verified calculation extension point
- Method: modded-class method extension
- Compatibility risk: High
- Preferred integration method: No, unless no safer extension path exists

### Lifecycle and Damage Touchpoints

Verified extension points include:
- SCR_CharacterControllerComponent.ACE_Medical_OnUnconsciousPoseChanged()
- SCR_CharacterControllerComponent.OnLifeStateChanged(...)
- SCR_CharacterDamageManagerComponent.ACE_Medical_OnKilled()
- SCR_CharacterDamageManagerComponent.ACE_Medical_UpdateResilienceRegenScale()
- ACE_Medical_CharacterChestHitZone.OnDamage(...)

Integration classification:
- Status: Verified extension points
- Method: modded-class overrides
- Compatibility risk: Medium to high
- Preferred only when no event-driven alternative exists

## Breathing Touchpoint Result

Preferred integration paths:
1. Subscribe to GetOnPneumothoraxStateChanged() where pneumothorax changes are relevant.
2. Read existing breathing state through ACE_Medical_VitalsComponent.
3. Use existing ACE notification paths for breathing and SpO2 results where applicable.
4. Avoid overriding the central breathing calculation pipeline unless required and explicitly validated.

## Open Questions

- How should ACECC observe airway obstruction and occlusion changes without a dedicated event?
- How are respiratory rate and SpO2 synchronized for remote clients?
- Which ACE medical actions trigger breathing and SpO2 result notifications?
- Can ACECC add additional breathing-related notifications without replacing ACE behavior?