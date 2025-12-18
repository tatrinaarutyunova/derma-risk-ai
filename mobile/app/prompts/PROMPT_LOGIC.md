# AM / PM Cleanse Prompt Logic

## Purpose
Collect daily routine compliance data (face cleansing) in a low-friction way
to support proactive skin risk prediction.

---

## Prompt Types
- cleanse_am
- cleanse_pm

Each prompt can appear once per day.

---

## Time Windows

### Morning (AM)
- Default window: wake time to +2 hours
- Question: "Did you wash your face this morning?"

### Night (PM)
- Default window: 2 hours before bedtime to bedtime
- Question: "Did you wash your face tonight?"

---

## Greeting Rules
When a prompt is shown, display a time-based greeting:
- 5:00–11:59 → "Good morning"
- 12:00–16:59 → "Good afternoon"
- 17:00–21:59 → "Good evening"
- 22:00–4:59 → no greeting

Greeting is derived from device local time and does not affect logic.

---

## Prompt States
Each prompt has one of the following states:
- Not Asked
- Pending
- Confirmed
- Skipped

---

## Allowed Responses
- Yes
- Not yet
- Skipped

---

## Response Behavior

### Yes
- State → Confirmed
- Log event with value = true
- Close prompt for the day

### Not yet
- State → Pending
- Allow one soft reminder later
- No event logged yet

### Skipped
- State → Skipped
- Log event with value = false
- Close prompt for the day

---

## Reminder Rules
- Only one reminder per prompt
- Reminder only occurs if state is Pending
- If ignored, prompt expires silently

---

## Daily Reset
- At day boundary, prompt states reset to Not Asked
- Previous day responses are immutable

---

## Logged Event Format
Each confirmed or skipped prompt logs:

{
  user_id,
  timestamp,
  prompt_type,
  value: true | false
}
