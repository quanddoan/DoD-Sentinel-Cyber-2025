# Listening post
## Difficulty: Easy
## Tools: [Audacity](https://www.audacityteam.org/download/), [CyberChef](https://cyberchef.org/)
## Description:
We've intercepted a radio broadcast being bounced off a satellite likely intended for the North Torbian cells located around the world. Do you think you can unravel what they are transmitting?
## Solve:
- Listening to the mp3, we can recognize the dialing sounds of 2 distinct pitches making up the audio file. We assign the higher pitch as 1 and lower pitch as 0.
- Since I was not familiar with working with audio files, I had to transcribe manually. Audacity was helpful in slowing down the tempo (```Effect -> Pitch and Tempo -> Change Tempo```). The result was: ```1011110011001110100001001000110111001011100110111100111010010000101000001001010011001110100100111001001111001100100110111010000010001011100101111100110010100000100010111100111110001101100111011100111010011110101000001000110010001011110010111000110110000010```
- We know that every flag begins with ```C1```, which is ```01000011 00110001``` in binary. The first 16 bit of the transcript is ```10111100 11001110```, which is the exact bitwise invert.
- Inverting the transcript, we have: ```0100001100110001011110110111001000110100011001000011000101101111010111110110101100110001011011000110110000110011011001000101111101110100011010000011001101011111011101000011000001110010011000100011000101100001010111110111001101110100001101000111001001111101```, then use CyberChef to decode and get the flag.