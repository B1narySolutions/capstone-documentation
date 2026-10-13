# Carrier board rev 2: change list

Based on the sponsor spec, the capstone student manual, Rocco's own list (2026-10-13)
and the current board (seed3-carrier-board, commit 61c2f36).

## What rev 1 has today
Guitar input (1/4", buffered, input-level pot), Daisy Seed sockets, headphone amp (TPA6111A2) on a 1/8" jack,
1/4" line out that the headphone jack's switch mutes in hardware, volume and tone pots, a 4-pin display UART
header, a 2-pin 5 V header with P-FET reverse-polarity protection, and two test points (VREF, AMP_5V).
Board is 95.8 x 66.2 mm, 2 layers.

## Rocco's items

1. **Digital jack detect and mute (instead of the hardware switch).** So you can record from the 1/4" out
   while monitoring on headphones.
   - Use jacks with a detect switch on both outputs and wire each switch to a spare GPIO (pull-up, plug in = low).
   - Feed the 1/4" line out from the Seed's second output (AUDIO_OUT_2, unused today) through its own buffer,
     instead of from the headphone amp's left channel. Then firmware sets each output's level independently.
   - Drive the TPA6111A2 SHUTDOWN pin from a GPIO so headphones can be fully muted, and give the
     speaker amp a mute line too.
   - Default to the laptop rule in firmware (headphones in = speaker off), with a setting to keep the line out on.

2. **Better 5 V input connector from the battery/power board.** Keep the P-FET reverse-polarity protection and
   swap the 2-pin header for a keyed, locking connector (JST-XH or JST-PH 2-pin are common; a barrel jack works
   if the supply is external). Agree on the connector with your teammate first, and use the same family for
   the display and speaker-amp cables. Also add a PTC fuse and a 5 V TVS diode, which the old TODO flagged.

3. **4 layers.** Makes routing easier and gives a solid ground plane, which helps audio noise.
   - Trade-off: the course only pays for 2-layer boards under 100 x 100 mm (manual sec 11.1). A 4-layer order
     comes out of the team budget and must go through the WUFOO order process (sec 9.8).
   - JLCPCB's 4-layer prototype price is usually low (check their quote; shipping is often most of the cost).
   - Either way, stay under 100 x 100 mm so a 2-layer fallback is still free.

## Other items (spec and manual)

4. **Add the AUX input (1/8").** Required by the spec, and the board has none. AUDIO_IN_2 is unused, so route
   a 1/8" stereo jack there through an L+R summing and level network. Firmware mixes AUX after the amp model.
   A detect switch on this jack is a cheap extra.

5. **Add the team name to the silkscreen.** The free Tuesday order requires the team name and designer name on the
   board (manual sec 11.1). Rev 1 has the designers but no team name. The "please dont plug it in backwards"
   text can go once the connector is keyed.

6. **Design for test (manual sec 16.8, 20.2).** The manual says to assume the board won't work and add indicator
   lights, test points and isolation switches.
   - Test points with a nearby GND: AUDIO_IN (needed to set the input trim), 5 V in, 3V3, VREF,
     line out, HP_L/HP_R, AUX.
   - A power LED on 5 V.
   - A 0 ohm resistor or jumper to cut power to the headphone amp and each output stage separately.

7. **Speaker amp connection.** Spec needs 5-10 W. If the power amp stays on the power board, add a keyed connector
   carrying line-level audio and the mute line. 5 V alone gives about 3 W into 4 ohms, so the amp needs a
   higher rail (for example a 2-cell battery).

8. **Battery level to the Seed.** A divider from the battery into a spare ADC pin so the touchscreen can show charge.
   Needs one wire on the power board connector.

9. **Confirm the Seed3 pinout (open item).** The board uses the Daisy Seed Rev4 footprint. Check VIN (pin 39)
   and the audio pins (16-19) before ordering.

10. **Add mounting holes.** Rev 1 has none. Add 3-4 M3 holes at the corners for the enclosure.

11. **Keyed display connector.** The 4-pin display header carries 5 V and is unkeyed.

12. **Mono plugs in the headphone jack (open item).** A mono plug shorts the right channel through 4.7 ohms.
    Raise that resistor or bench-check how hot U2 gets.

## Nice to have (stretch goals)

13. **Footswitch jack.** A 1/4" TRS jack on spare GPIOs for a wired footswitch like the Fender MS4.
14. **USB-C audio interface.** USB_D+/D- only reach the breakout header today. Bring them to a panel USB-C port.
