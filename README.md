## BHG-2000 Nuclear Event Detector

<br><br>
<div align="center">
  <img width="500" img src="https://github.com/user-attachments/assets/13fd84dd-e7de-46cd-ae96-a41285c8501c" alt="BHG-2000">
</div>

<br><br>

The BHG-2000 uses BPW34S PIN diodes to detect the prompt flash of a nuclear detonation. 
The diodes are shielded from ambient visible light by a cover treated with Musou black ultra-absorbing paint, suppressing the visible light response. 
The dominant detectable signal is therefore the prompt gamma flux, which passes through the cover and generates photocurrent in the silicon bulk via Compton scattering. 
This current is amplified by a transimpedance amplifier and used to trigger a nuclear event detection pulse.

As part of the Bhangmeter project this was done with a HSN-1000L module, this has now been made obsolete so the BHG-2000 is provided as a replacement

<div align="center">
  <img width="500"alt="image" src="https://github.com/user-attachments/assets/5f2e4348-94f9-4423-b923-28978f2a8a4e" />
</div>
<br><br>
# How does it work

<div align="center">
  <img width="1000" alt="image" src="https://github.com/user-attachments/assets/75c0207a-de1a-45d7-acfc-76adcc3d55d6" />
</div>


The circuit has three main sections



- **Transimpedance Amplifier**

The transimpedance amplifier is based on the LTC6244 op-amp, this was chosen as it has low input bias current, low input capacitance and adequate  GBW.

There are four BPW34S diodes which are used to detect the gamma via Compton scattering in the silicon bulk. The BPW34S photodiodes are reverse biased to -5V to speed up response time. 
The BPW has a high active area (~7mm²) to increase likelihood  of Compton interactions

The TIA has a second amplification stage which is nominally set to ~38X gain, this is to help increase the small signal from the TIA stage into the pulse generator


- **Active Low Pulse Generator**

The second stage is a LT1797 op amp configured as a comparator, the RC time constant of R4 and C3 determines the width of the pulse. The LT1797 has a high output current capability to drive the active low pulse indicating an event has occurred


- **Power Stage**

The BHG-2000 is designed to work from a single 5V supply, the MAX868 generates a negative 7V5 rail which is used to generate the -5V V_PD rail via an ADP7182A. A second ADP7182A is used to generate the op-amp -2V5 There is an ADP151 generating the op-amp 2V5 rail

<br><br>
# BIST Functionality


To enable the BHG-2000 to self-check, a Built-In Self-Test (BIST) function is incorporated. 
When BIST_EN is asserted, an LED illuminates the BPW34S photodiodes directly, injecting a known optical stimulus into the signal chain. 
A valid NED output pulse confirms the TIA, amplification stage, and pulse generator are all functioning correctly. 
The BIST LED is driven via a MOSFET switch (Q2) with current set by R5, and is gated by Q1 to prevent false triggering during normal operation.

<br><br>
# Design Files

Available in the repo are:

1) The gerbers, this is a simple 4 layer board with through vias so should be inexpensive to manufacture
2) The schematic PDF which documents the functionality of the board
3) The assembly drawing to aid populating the board
4) The 3D print file for the cover
<br><br>
# BhangmeterV2 Compatibility 

<div align="center">
  <img width="500" alt="image" src="https://github.com/user-attachments/assets/92e5c953-5712-4069-8f73-6f1d1be3a651" />
</div>

The BHG-2000 is designed as a pin-for-pin replacement for the HSN-1000L module used in the [BhangmeterV2](https://github.com/bigcrimping/bhangmeterV2). The HSN-1000L has been discontinued and is no longer available, the BHG-2000 is intended to slot directly into the existing socket without modification to the host board.


The interface signals are compatible:
- **5V** — single supply input
- **NED** — active low nuclear event detection output
- **BIST_EN** — active high BIST trigger input
- **GND**

Note that the BHG-2000 exposes additional pins (EXT_CAP1, EXT_CAP2, EXT_PD), it is safest not to fit R15, R16, R17, R12 and R2.
<br><br>
# BOM Configuration / Testing Requirements 


Unfortunately I have not been able to find a gamma source I can calibrate the TIA with, as such the value of R6 (gain) and C4 (stability) still need to be formalised. 
Likewise the extra capacitance of the four diodes is limiting the bandwidth of the TIA, once the BPW34 sensitivity is known the number of BPW34 devices needed and gain required can be set.  


If you have access to a Cs-137 or Co-60 source and are able to assist with calibration in the UK or Europe, please get in contact at info@bigcrimping.com
<br><br>

# Next steps

Next steps are to:

1) Calibrate the gamma to current curve and modify the gain setup to detect the prompt gamma
2) Make an optical only version with a single BPW34S diode

In the module the signal at the output of the TIA and the output of the second gain stage are put onto dedicated pins, the plan is to update the Bhangmeter project so that the optical/UV "double pulse" is detected and from that the yield can be inferred. There will be a new housing with appropriate filter for this purpose





