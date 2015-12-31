DIY Micro LC Filter
===

This "dead bug" LC Filter is designed for running a 5V vTX system off a 1S battery with enough RF rejection to be suitible for the latest crop of brushed micro-quads.

The complete <10g vTX system this filter was specfically designed for is outlined in this document.

With these instructions you can construct a fully contained noise tolerant video transmission system, including LC Power Filter, 700TVL Camera, 200mW vTX and CP or Linear Antenna totalling under 10g.

The negligible 2g additional load from the LC filter vs comparable DIY micro vTX systems is offset by the elimination of a separate vTX battery while maintaining tranmission integrity.

Original inspiration in german at [FPV-Team.de blog][].

###Ingredients:

 - TX5823 Video Transmitter
 - Sony CMOS 700TVL 170DEG FOV Camera
 - DIY Micro 1S LC Filter (Construction details below)
 - 3-Position DIP Switch (Channel Selection)
 - DIY IBCrazy CP TX Antenna
 - 3.3V LDO Regulator for TX5823
 
##Mini LC Filter

The only difference between my version of the LC Filter Circuit and what was presented by FPV-Team.de is the final layout of the SMD components.

The resulting footprint is minimized in my improved implementation, and the tighter arrangement is more physically robust and allows for easier DIY construction.
 
###Mini LC Filter Components:

 - L1:	FIXED IND 330UH 320MA 1.86 OHM	(Digikey: 308-1511-1-ND	)
 - D1:	DIODE SCHOTTKY 20V 1A DO213AB	(Digikey: BYM13-20-E3/96GICT-ND)
 - C1:	CAP ALUM 330UF 20% 6.3V SMD 	(Digikey: PCE3860CT-ND)
 - C2:	CAP CER 0.1UF 16V X7R 0603		(Digikey: 490-1532-1-ND)

###Mini LC Filter Construction Steps:

 1. Tin both ends of D1
 2. Tin both pads of L1
 3. Arrange D1 and L1 as pictured in the diagrams below
 4. Verify diode polarity!
 5. Bridge the tinned pads of L1 and D1, joining the first two components
 4. Tin the leads of C1 and brace yourself for the most difficult step
 5. Use fine tip tweezers to CAREFULLY place C2 between the leads of C1
 6. Hold C2 with tweezers while soldering it in parallel with the leads of C1
 7. Place the L1/D1 assembly alongside the C1/S2 assembly per diagram
 8. Solder L1 and C1 via the C1 anode (positive) lead
 9. Optionally re-heat the L1-D1 junction flowing solder against the can of C1 to reinforce the L1-C1 connections
 10. Sit back and observe your mastery of soldering dead-bug style
 
###Side View:

			________
	Vin -> |   D1  #|\
		   |_______#| \____________
			  |     | |           #|
			  |     |/|           #|
			  | L1  | |     C1    #|
			  |     | |           #|
			  |     | |___________#|
			  |_____| |_|__________|
		 Vout -> \-----\---[C2]---/<---- GND

###Top View:

			   _____     ___________ 
			  |     |   /        \ |
			__|_____|  /         #\|
		   |   D1  #|\|          ##|
		   |_______#|/|     C1   ##|
			  | L1  | |          ##|
			  |     |  \         #/|
			  |_____|   \________/_|

###Bottom View:

			   _____     __________ 
			  |     |   /         #|
			__|     |  /  [C1]    #|
		   |D1|     |\|           #|
		   |__| L1  |=====[C2]=======
			  |     | |           #|
			  |     |  \          #|
			  |_____|   \_________#|
		  
##Circuit Diagram:
	 
	 Vin-[D1]->|---[L1]---o---o-Vout
						  |   |
						 [C1][C2]
						  -   |
						  |   |
	 GND------------------o---o-GND
 
 
##TX5823:

###TX5823 Module Connections:
	   _______________________________
	  |                               |
	   ) GND                          |
	  |                          VID (
	   ) ANT                          |
	  |            TX5823        AUD (
	   ) GND                          |
	  |                          GND (
	   ) CH3                          |
	  |                          VCC ( 
	   ) CH2                          |
	  |                          GND ( 
	   ) CH1                          |
	  |_______________________________|  
	 
####TX5823 Information:

EXPLICIT WARNING: DO NOT APPLY POWER TO THE TX5823 WITHOUT ANTENNA, YOU WILL DAMAGE OR DESTROY THE MODULE

Many TX5823 modules are sold as 5V compatible but they lack the 3.3V Regulator required for in-spec operation of the RTC6705 transmitter chip. Many vendors consider this an acceptable overclock, I regard it a potential "fast and hard" life and certain "premature death" for your transmitter.

In addition to adding the 3.3V LDO Regulator, you should populate the nearest empty pad to the Regulator with a 0.01uF 0603 Capacitor if not already present; this acts as a reference bypass voltage to lower output noise and increase the Power Supply (Noise) Rejection Ratio, or PSRR.

####TX5823 Frequency Band Selection Table:

Pins 3 and 4 on the RTC6705 are used to select the frequency band. On the TX5823-120228 module I received from FoxTechFPV the pins are set to: [FIXME].

The band selection pins of the RTC6705 are on internal pull-ups and the pads are connected to ground; a resistor/bridge/shunt/jumper present draws the connected input to ground. 

Translation: No jumper is a 1, jumper present is 0, X means it doesn't matter.

The following table was copied from page 7 of the [RTC6705 datasheet][RTC6705 Datasheet].

	Band	Pin 1	Pin 2	000		001		010		011		100		101		110		111
	------------------------------------------------------------------------------------
	A		0		0		5865	5845	5825	5805	5785	5765	5745	5725
	B		0		1		5733	5752	5771	5790	5809	5828	5847	5866
	E		1		X		5705	5685	5665	5645	5885	5905	5925	5945

Example: An unmodified board with no band selection resistors populated and no DIP switch installed to select channels will default to 5.945GHz.
	
[FPV-Team.de blog]: https://fpv-team.de/blog-aktuelles-news/entry/videouebertragung/mini-lc-filter-fuer-nanocopter "Mini LC-Filter Fur Nanocopter"
[Digikey-L1]: https://www.digikey.com/product-detail/en/CDRH74NP-331MC-B/308-1511-1-ND/956739
[Digikey-D1]: https://www.digikey.com/product-detail/en/BYM13-20-E3%2F96/BYM13-20-E3%2F96GICT-ND/3847605
[Digikey-C1]: https://www.digikey.com/product-detail/en/EEE-0JA331P/PCE3860CT-ND/766236
[Digikey-C1-3S]: https://www.digikey.com/product-detail/en/UCL1C331MCL6GS/493-3925-2-ND/2300274
[Digikey-C2]: https://www.digikey.com/product-detail/en/GRM188R71C104KA01D/490-1532-1-ND/587771
[Digikey-BYP]: http://www.digikey.com/product-search/en?keywords=490-1525-1-ND
[Digikey-DIP]: https://www.digikey.com/product-detail/en/SDA03H1BD/CKN6061-ND/949915
[Digikey-REG]: https://www.digikey.com/product-detail/en/CAT6219-330TDGT3/CAT6219-330TDGT3CT-ND/1631223
[IBCrazy-CPL]: http://www.rcgroups.com/forums/showthread.php?t=1388264 "The ultimate circularly polarized aerial antenna!"
[RTC6705 Datasheet]: http://sky.geocities.jp/oumeastro/RTC6705-DST-001.pdf
