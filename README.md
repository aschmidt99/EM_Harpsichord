# Electromagnetic Harpsichord

![EM_Harpsichord](EM_Harpsichord.png)

The EM Harpsichord is an augmented harpsichord that uses optical sensors, audio power amplifier chips, and some simple signal processing to sustain the vibrations of its brass strings. This early prototype uses one circuit board for every string you wish to sustain. A work-in-progress performance/demonstration was given on October 2nd, 2023 at the 63rd Annual University of Michigan Organ Conference. For this demonstration, 3 strings had optical sustaining enabled and 8 strings had audio-playback enabled.

## Video Examples
Here is a [selection of audio examples](https://drive.google.com/drive/folders/1B2c4qz-7Rl1Y2wPH64m0Ud0_MTGL74a2?usp=sharing) of both sustainers and audio playback through the harpsichord strings.

## Working Principle
This simple board uses a ITR20403 Optical Photo-interrupt to sense the vibrations (position) of a harpsichord string. The derivative of this signal is amplified and sent to a 5-Watt audio amplifier chip (PAM8406) that is normally used to power small 4 or 8 ohm speakers. Rather than connect to a speaker, the output terminals of the speaker chip are connected to each end of one of the strings on a harpsichord. The amplified signal pushes current through the string, which generates an alternating magnetic field that interacts with a strong magnetic field from permanent neodymium magnets.

The working principle is similar to the more familiar eBow, but rather sensing and inducing vibrations with electromagnetic coils, we sense with an optical sensor and actuate with Lorentz force actuation.

## Simple System Diagram
![simpleSystemDiagram](simpleSystemDiagram.png)
This is the signal flow for the sustainer circuit in each board. You need one circuit board for each string you wish to sustain.

## The Circuit Board
![PCB](BoardRendering.png)
The circuitboard is thin enough to fit between the harpsichord's strings. We mounted them to an aluminum extrusion C Beam with slide nuts, M4 screws, and some small 3D printed brackets.

## How to order
### JLCPCB
If you wish to have this exact board, you can order these circuit boards from JLCPCB by uploading GERBER-EMHarpsichord_Sustainer+Playback.zip to JLCPCB. Also upload BOM-EMHarpsichord_Sustainer+Playback.csv and CPL-EMHarpsichord_Sustainer+Playback.csv to have JLCPCB assemble the board for you.
(Note: Double check the orientation of each IC in JLCPCB. They are often incorrect by default.)

### Reach out to me
Alternatively, reach out to me (schmia@umich.edu) in case I have some extra :)

### Other board house
If you are familiar with a different board house, feel free to export as you usually would from KiCad.

### Notes before ordering/assembling
- If you order based on my files, the optical sensor will be placed on the wrong side of the board. I found it easier to source the components from JLCPCB and desolder/resolder the ITR20403 to the other side of the board. If you can find them for cheap elsewhere, it would probably save you a small headache to order them separately and omit them from the board when ordering.

## How to use
### Wiring up
- A 3 Pin JST XH 2.54mm connector supplies 5V, Ground, and an auxiliary audio input. If you don't anticipate using audio playback, simply wire up the 5V and Ground and you are good to go. Chain several boards together to sustain multiple notes. We put a SPST switch between each board's ground and the power supply ground to allow us to select which strings were activated individually.
- The PAM8406 chip is advertised to support 4-8 ohm loads, though the data sheet notes that it can support loads down to 2 ohms. The data sheet also notes that outputs of the PAM8406 amplifier chip have built in short circuit protection, and the outputs will not activate if they detect less than 2 ohm load. Therefore, you must ensure that there are at least 2 ohms between the (+) and (-) terminal of each speaker output.
	- Most of the brass strings on our harpsichord measured much less than 1 Ohm, so we opted to put a 2-Ohm 10W Power Resistor between each string and one of the terminals the selected channels.

### Magnets
- To induce vibration, the strings must be exposed to a strong magnetic field. We have been using N52 neodymium magnets (purchased from kjmagnetics.com) for this purpose. The strength of the vibration is proportional to the strength of the magnetic field, and although the presence of just one or two magnets usually is enough to get sound going, more magnets seems to encourage a higher acoustic output.
- The orientation of the magnet does matter. Flip the magnet back and forth to determine which side is inducing the sound you like. One side might should sustain the fundamental frequency of the string and the other may damp the fundamental while favoring odd harmonics.
- If you wish to use multiple magnets on a string, you will most likely want all of their poles pointed in the same direction, although interesting magnet placement can yield interesting results...

### Channels
- The 'left' output channel of the board to the optical sensor which creates a closed feedback loop to sustain vibrations on the connected string.
- The 'right' output channel of the board amplifies an auxiliary input signal.

### Additional Notes:
- The optical sensor needs to be placed very precisely over the string to ensure it picks up the vibration. The sensing range of the string is very small and is easy to clip. You can use an oscilloscope on test point 1 (TP1) to make sure the sensor isn't clipping before the gain stage.
- If it is clipping, consider moving the sensor board closer to a termination point of the string where its range of vibration is much smaller.
- The feedback loop is very sensitive to the position of the sensor. You will likely notice timbral changes as you change the point on the string where the sensor is located, the position of the sensor over the string, and the positions of the magnets.
- Bar magnets induce stronger vibrations than cylindrical with the same pull force.
- The resistance of a metal string is a function of its material, gauge, length, and temperature. The higher the resistance, the more apt the string is to detuning. If your strings are detuning too much, try using a thicker wire, more conductive material, or lower current. If you are building a new instrument, try different string lengths.

## Design References
- the 'RelevantDocuments' folder holds data sheets and some blog/forum posts I found helpful while designing the board. Dave Corsie's blog helped me figure out how to design the optical pickup circuit and the DIYAudio blog helped me make a better PCB layout for the PAM8406 Chip.

## Future Improvements
### Support Loads < 2 Ohms
One of the most obvious shortcomings of this system is the wasted power. If a given string is only 0.2 Ohms and we compensate with a 2 Ohm resistor, most (~90%) of the power is being dissipated as heat through the resistor rather that contributing towards generating vibrations. I suspect looking into circuits that are anticipating extremely low loads such as ribbon speakers will be a worthwhile endeavor and likely be a solution for the next version of this circuit.

### Lower Heat Buildup
On the other hand, the large dummy load ensures the majority of the power is dissipated somewhere other than the string, which prevents heat buildup (and thus detuning) in the string. The magnetic field generated by the string is proportional to current only - it does not factor in voltage.

### Controlled Detuning
With more advanced feedback control, it may be possible to control how much or little each string is detuned during actuation. This could allow the performer to change tuning systems on-the-fly with the press of a button - a process that usually takes considerable time and attention.

### Sustainer on every string
A natural progression for the project is to add a sustainer board for every string so that we can sustain any strings rather than just the few we chose for our work-in-progress demonstration. A double-manual harpsichord might work well, where one manual controls the sustainers and the other moves the jacks and plucks the strings as usual.

### Sensing the keys
In the next version of this project, we aim to actively sense the position of each harpsichord key to use as a control signal for the sustainer of each corresponding string. Mechanically separating the harpsichord keys from the strings will allow us to map and re-map the interaction between key presses and actuation however we wish.

## Acknowledgements
- This project was directed and enabled by Professor Joseph Gascho, Associate Professor of Music and Director of the Stearns Collection at the University of Michigan Ann Arbor.
- This project was inspired by the (Electromagnetic Resonator Piano)[http://instrumentslab.org/research/mrp.html], a project by Andrew McPherson at the Augmented Instruments Lab.
- Additional inspiration and insight was provided by (Techniques and Circuits for Electromagnetic Instrument Actuation)[https://www.nime.org/proceedings/2012/nime2012_117.pdf] by Andrew McPherson (see Section 2.4 - Lorentz Force Sustaining).

## Software Versions used
- KiCad (6.0.7-1)-1 for MaxOS Version 13.4.1
- LTSpice 17.0.40 for OS X
