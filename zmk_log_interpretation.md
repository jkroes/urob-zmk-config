# ZMK Debug Log Interpretation Guide

## Log Structure
Each log line follows this format:
```
[timestamp] <level> module: message
```
- **timestamp**: `[HH:MM:SS.milliseconds]`
- **level**: `<dbg>` (debug), `<inf>` (info), `<wrn>` (warning), `<err>` (error)
- **module**: Usually `zmk`

## Key Processing Flow

### 1. Physical Key Detection
```
<dbg> zmk: kscan_matrix_read: Sending event at 3,4 state on
```
- **What it means**: Physical key press detected
- **Format**: `Row,Column state on/off`
- **Example**: Row 3, Column 4 pressed

### 2. Position Mapping
```
<dbg> zmk: zmk_physical_layouts_kscan_process_msgq: Row: 3, col: 4, position: 38, pressed: true
```
- **What it means**: Matrix position converted to keymap position
- **position**: The keymap position number (0-based index)

### 3. Hold-Tap Processing

#### New Hold-Tap
```
<dbg> zmk: on_hold_tap_binding_pressed: 38 new undecided hold_tap
```
- **What it means**: A hold-tap behavior (like home row mods) was triggered
- **"undecided"**: ZMK doesn't know yet if it's a tap or hold

#### Hold-Tap Decision
```
<dbg> zmk: decide_hold_tap: 38 decided tap (balanced decision moment key-up)
```
- **Decision types**:
  - `decided tap`: Will send the tap behavior
  - `decided hold`: Will send the hold behavior
- **Decision moments**:
  - `key-up`: Decided when key was released (quick tap)
  - `other-key-down`: Decided when another key was pressed while holding
  - `timeout`: Decided after tapping-term timeout

#### Hold-Tap Cleanup
```
<dbg> zmk: on_hold_tap_binding_released: 38 cleaning up hold-tap
```
- **What it means**: Hold-tap processing is complete

### 4. Combo Processing

#### Combo Capture
```
<dbg> zmk: position_state_down: combo: capturing position event 38
```
- **What it means**: Combo system is evaluating this keypress

#### Combo Filtering
```
<dbg> zmk: filter_timed_out_candidates: after filtering out timed out combo candidates: remaining_candidates=0
```
- **remaining_candidates=0**: No combos matched
- **remaining_candidates=N**: N potential combos still being evaluated

#### Combo Release
```
<dbg> zmk: release_pressed_keys: combo: releasing position event 38
```
- **What it means**: No combo matched, releasing the key for normal processing

### 5. Key Binding Execution

#### Key Press
```
<dbg> zmk: on_keymap_binding_pressed: position 38 keycode 0x70009
```
- **keycode**: HID usage code (see keycode reference below)

#### Key Release
```
<dbg> zmk: on_keymap_binding_released: position 38 keycode 0x70009
```

### 6. HID Report Generation

#### HID Key Press
```
<dbg> zmk: hid_listener_keycode_pressed: usage_page 0x07 keycode 0x09 implicit_mods 0x00 explicit_mods 0x00
```
- **usage_page 0x07**: Keyboard/keypad usage page
- **keycode**: The actual key being sent
- **implicit_mods**: Modifiers from layer/behavior
- **explicit_mods**: Modifiers from explicit mod behaviors

#### HID Report Sent
```
<dbg> zmk: zmk_endpoints_send_report: usage page 0x07
```
- **What it means**: HID report sent to host computer

## Common Keycode Reference

### Letters (0x04-0x1D)
- `0x04` = A, `0x05` = B, `0x06` = C, `0x07` = D, `0x08` = E, `0x09` = F
- And so on... (A=0x04, B=0x05, etc.)

### Numbers (0x1E-0x27)
- `0x1E` = 1, `0x1F` = 2, `0x20` = 3, `0x21` = 4, `0x22` = 5
- `0x23` = 6, `0x24` = 7, `0x25` = 8, `0x26` = 9, `0x27` = 0

### Common Keys
- `0x28` = Enter/Return
- `0x29` = Escape
- `0x2A` = Backspace
- `0x2B` = Tab
- `0x2C` = Space
- `0x2D` = Minus/Underscore
- `0x2E` = Equals/Plus

### Modifiers (shown as mods values)
- `0x01` = Left Ctrl
- `0x02` = Left Shift  
- `0x04` = Left Alt
- `0x08` = Left GUI (Cmd/Win)
- `0x10` = Right Ctrl
- `0x20` = Right Shift
- `0x40` = Right Alt
- `0x80` = Right GUI

## Troubleshooting Patterns

### Hold-Tap Not Working
Look for:
- Missing `new undecided hold_tap` message
- Wrong decision (`decided tap` when you wanted `decided hold`)
- Check your tapping-term and other hold-tap settings

### Combo Not Triggering
Look for:
- `remaining_candidates=0` too quickly
- Missing keys in the combo sequence
- Timing issues with combo timeout

### Keys Not Registering
Look for:
- Missing `kscan_matrix_read` events (hardware issue)
- Missing `on_keymap_binding_pressed` (keymap issue)
- Missing HID reports (USB/Bluetooth issue)

## Tips
- Use timestamps to measure timing issues
- Compare working vs non-working key sequences
- Look for patterns in decision moments for hold-tap tuning