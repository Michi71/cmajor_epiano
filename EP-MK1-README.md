# EP-MK1 Cmajor Implementation

This is a Cmajor implementation of Mike Moreno's EP-MK1 physical modeled electric piano, originally created in Pure Data.

## Overview

The EP-MK1 is a detailed physical model of an electric piano (Rhodes/Wurlitzer style) that simulates:

- **Tine resonators** with two independently tunable partials
- **Tone bar** resonance with sustain pedal support
- **Electromagnetic pickup** with nonlinear characteristics
- **Hammer and damper** mechanical noise
- **Tremolo/vibrato** effect with shape morphing

## Files

- `ep-mk1.cmajor` - Main Cmajor implementation
- `ep-mk1.cmajorpatch` - Patch manifest file

## Parameters

### Tine Section
Physical model of the vibrating tine/reed

- **Ratio 1** (0-30): Frequency ratio for first partial (default: 7)
- **Ratio 2** (0-30): Frequency ratio for second partial (default: 20)
- **Highpass** (20-20000 Hz): High-pass filter cutoff (default: 1500 Hz)
- **Decay** (1-2000): Decay time in ms (default: 500)
- **Level** (-100-0 dB): Output level (default: -16 dB)
- **Send** (-100-24 dB): Send amount to pickup (default: -12 dB)

### Tone Bar Section
Resonating bar with sustain control

- **Decay** (1-2000): Decay time in ms (default: 1000)
- **Release** (1-2000): Release time in ms (default: 10)
- **Level** (-100-0 dB): Output level (default: -6 dB)
- **Sustain Pedal** (on/off): Sustain pedal control

### Pickup Section
Electromagnetic pickup simulation

- **Gain** (0-24 dB): Input gain (default: 13 dB)
- **Lowpass** (20-20000 Hz): Low-pass filter cutoff (default: 1000 Hz)
- **Symmetry** (0-24 dB): Pickup asymmetry/nonlinearity (default: 15 dB)
- **Level** (-100-6 dB): Output level (default: -12 dB)
- **Attack** (-100-30 dB): Attack noise level (default: 3 dB)
- **Buzz** (-100-0 dB): Buzz/bark effect level (default: -6 dB)
- **Buzz Phase** (on/off): Buzz phase inversion (default: on)

### Hammer Section
Mechanical hammer noise

- **Hammer Level** (-100-0 dB): Hammer strike noise (default: -24 dB)
- **Note-off Level** (-100-0 dB): Damper release noise (default: -30 dB)

### Tremolo Section
Vibrato/tremolo effect

- **Rate** (0-20 Hz): LFO rate (default: 3 Hz)
- **Shape** (0-127): Shape morphing sine→triangle (default: 0)
- **Depth** (-100-0 dB): Modulation depth (default: -9 dB)
- **On/Off**: Enable/disable effect (default: on)

### Master Section

- **Master Level** (-100-0 dB): Final output level (default: 0 dB)

### Tuning Section
*(Note: Full tuning system not yet implemented)*

- **Base Frequency** (100-20000 Hz): Reference frequency (default: 440 Hz)
- **Base MIDI Note** (0-127): MIDI note for base frequency (default: 69 = A4)
- **Num Divisions** (1-100): Equal divisions (default: 12)
- **Interval Divide** (0-20): Interval to divide (default: 2 = octave)
- **Use Text Scale** (on/off): Use text-based scale file (default: off)

## Architecture

The implementation follows the original Pure Data patch structure:

```
MIDI Input → Voice Allocator (32 voices)
                    ↓
            [Per-Voice Processing]
            - Tine Model (2 partials)
            - Tone Bar Model
            - Hammer Noise
            - Note-Off Noise
            - Voice Mixer
            - Pickup Model
                    ↓
            Master Gain
                    ↓
            Tremolo Effect
                    ↓
            Stereo Output
```

## Differences from Original PD Patch

1. **Polyphony**: Maintained 32 voices like the original
2. **Resonator Implementation**: Simplified from biquad filters to direct oscillators for clarity
3. **Tuning System**: Basic equal temperament implemented; text-based scale loading not yet implemented
4. **GUI**: Parameter controls are exposed as Cmajor endpoints rather than PD GUI objects

## Original Credits

- **Original Pure Data Patch**: Mike Moreno (2019)
- **Website**: https://mikemorenoaudio.wordpress.com/2019/01/10/ep-mk1-physical-modeled-electric-piano-vst/
- **GitHub**: https://github.com/MikeMorenoAudio

## Cmajor Port

Ported to Cmajor format maintaining the physical modeling approach and parameter set of the original Pure Data implementation.

## Usage

Load the `ep-mk1.cmajorpatch` file in a Cmajor-compatible host (e.g., Cmajor patch player, supported DAWs with Cmajor support).

## Future Enhancements

- Implement text-based scale file loading (like the PD patch)
- Add preset system
- Optimize resonator models with proper biquad filters
- Add additional voice stealing algorithms
