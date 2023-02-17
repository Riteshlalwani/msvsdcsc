# msvsdcsc
## **CRACK SENSING CIRCUIT**

## **The Idea**
---
Any crack in a structure changes the strain profile of the material underneath. In situations like a boiler or a jet engine this can be critical. This strain can be detected using a strain gauge i.e. a device which changes its electrical resistance which change in strain. So if such a variable resistor is placed in a voltage divider/wheatstone bridge we can get different voltage levels for different strains detected. This change in voltage can be sensed by a Mixed signal SoC as discussed in this design.

## **The Implementation**
---
This circuit is implemented using the following 5 stages:-

1. The voltage divider (for the sake of this simulation, the voltage is directly taken)
2. 3-bit Flash type ADC to convert that voltage level into digital output
3. A 8x3 priority encoder to convert the ADC output to binary code
4. A PIPO to send data from the encoder to lcd every clock cycle
5. LCD interfacing circuit to display the data on a LCD

## **FLASH ADC and PRIORITY ENCODER**
---
Also called the parallel A/D converter, this circuit is the simplest to understand. It is formed of a series of comparators, each one comparing the input signal to a unique reference voltage. The comparator outputs connect to the inputs of a priority encoder circuit, which then produces a binary output. The following figure shows a 3-bit flash ADC circuit:



Vref is a stable reference voltage provided by a precision voltage regulator as part of the converter circuit, not shown in the schematic. As the analog input voltage exceeds the reference voltage at each comparator, the comparator outputs will sequentially saturate to a high state. The priority encoder generates a binary number based on the highest-order active input, ignoring all other active inputs.

The calculation for eg. an input of 3.70 V with Vref = 5 V:

The input lies in the range (these voltages can be found by voltage divider calculations) 5/8 * Vref and 6/8 * Vref which would make the output of the 5th comparator high which would give the corresponding input to priority encoder which would give the output 101 (5).

## FLASH ADC Circuit and its parts

# avsdcmp IP in cadence (Schematic)

![vsdcamp](https://user-images.githubusercontent.com/110079763/219593517-cf0d3b06-c150-493f-ac5f-3bfcdd33c1ea.png)

# Symbol of avsdcmp

# FLASH ADC Circuit

![flash ADC](https://user-images.githubusercontent.com/110079763/219593222-1179c9e0-0a24-48d9-beff-e0d2f4020c4e.png)

![flash ADC1](https://user-images.githubusercontent.com/110079763/219593268-b121345c-89ae-40a7-acfd-88bfa46ed7f8.png)

# Output Graphs

# For comparator

![Screenshot from 2023-02-17 02-51-50](https://user-images.githubusercontent.com/110079763/219593405-0426fc63-c571-4b02-99c0-5e59b6a50953.png)

## PIPO (parallel in parallel out)

# D flipflop Schematic

![Screenshot from 2023-02-16 14-11-13](https://user-images.githubusercontent.com/110079763/219593075-f04f1cba-3db7-4a38-a4e7-eefa5ecab4fa.png)

# PIPO Schematic

![Screenshot from 2023-02-16 14-09-33](https://user-images.githubusercontent.com/110079763/219592872-5b1181ea-f77d-40d1-8d96-b5546b561ead.png)

# Output for 1 D Flip FLop

![Screenshot from 2023-02-16 12-01-59](https://user-images.githubusercontent.com/110079763/219592929-9941336b-de69-4585-9475-6434666f687f.png)
