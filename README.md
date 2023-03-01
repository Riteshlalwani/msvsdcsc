# msvsdcsc
## **CRACK SENSING CIRCUIT**


## **_The Idea_**
---
Any crack in a structure changes the strain profile of the material underneath. In situations like a boiler or a jet engine this can be critical. This strain can be detected using a strain gauge i.e. a device which changes its electrical resistance which change in strain. So if such a variable resistor is placed in a voltage divider/wheatstone bridge we can get different voltage levels for different strains detected. This change in voltage can be sensed by a Mixed signal SoC as discussed in this design.

![Screenshot from 2023-02-27 11-52-27](https://user-images.githubusercontent.com/110079763/221767338-847a0b31-3456-4fc6-a6b5-7ea279395fcd.png)


## **_The Implementation_**
---
This circuit is implemented using the following 5 stages:-

1. The voltage divider (for the sake of this simulation, the voltage is directly taken)
2. 3-bit Flash type ADC to convert that voltage level into digital output
3. A 8x3 priority encoder to convert the ADC output to binary code
4. A PIPO to send data from the encoder to lcd every clock cycle
5. LCD interfacing circuit to display the data on a LCD

## **_FLASH ADC and PRIORITY ENCODER_**
---
Also called the parallel A/D converter, this circuit is the simplest to understand. It is formed of a series of comparators, each one comparing the input signal to a unique reference voltage. The comparator outputs connect to the inputs of a priority encoder circuit, which then produces a binary output. The following figure shows a 3-bit flash ADC circuit:



Vref is a stable reference voltage provided by a precision voltage regulator as part of the converter circuit, not shown in the schematic. As the analog input voltage exceeds the reference voltage at each comparator, the comparator outputs will sequentially saturate to a high state. The priority encoder generates a binary number based on the highest-order active input, ignoring all other active inputs.

The calculation for eg. an input of 0.8 V with Vref = 1.1 V:

The input lies in the range (these voltages can be found by voltage divider calculations) 5/8 * Vref and 6/8 * Vref which would make the output of the 5th comparator high which would give the corresponding input to priority encoder which would give the output 101 (5).

## **_FLASH ADC Circuit and its parts_**

# avsdcmp IP in cadence (Schematic)

![vsdcamp](https://user-images.githubusercontent.com/110079763/219593517-cf0d3b06-c150-493f-ac5f-3bfcdd33c1ea.png)

# Symbol of avsdcmp

# FLASH ADC Circuit

![flash ADC](https://user-images.githubusercontent.com/110079763/219593222-1179c9e0-0a24-48d9-beff-e0d2f4020c4e.png)

![flash ADC1](https://user-images.githubusercontent.com/110079763/219593268-b121345c-89ae-40a7-acfd-88bfa46ed7f8.png)

# **_Output Graphs_**

![Screenshot from 2023-02-20 14-36-41](https://user-images.githubusercontent.com/110079763/220255798-33b516a1-d923-437e-a4d1-78e04f78d2ae.png)

# For comparator

![Screenshot from 2023-02-17 02-51-50](https://user-images.githubusercontent.com/110079763/219593405-0426fc63-c571-4b02-99c0-5e59b6a50953.png)

## **_PIPO (parallel in parallel out)_**

Parallel In Parallel Out (PIPO) shift registers are the type of storage devices in which both data loading as well as data retrieval processes occur in parallel mode.

![pipo](https://user-images.githubusercontent.com/110079763/220256234-422ee408-9109-4678-9ef4-4e260fc84fe8.png)
The LCD interfacing circuit is a circuit designed in verilog to transfer the input on its terminal to a LCD (16x2 as shown in the figure) on every register select operation.
# D flipflop Schematic

![Screenshot from 2023-02-16 14-11-13](https://user-images.githubusercontent.com/110079763/219593075-f04f1cba-3db7-4a38-a4e7-eefa5ecab4fa.png)

# PIPO Schematic

![Screenshot from 2023-02-16 14-09-33](https://user-images.githubusercontent.com/110079763/219592872-5b1181ea-f77d-40d1-8d96-b5546b561ead.png)

# Output for 1 D Flip FLop

![Screenshot from 2023-02-16 12-01-59](https://user-images.githubusercontent.com/110079763/219592929-9941336b-de69-4585-9475-6434666f687f.png)

## **_Priority Encoder_**

![priority Encoder](https://user-images.githubusercontent.com/110079763/221664970-0a67aade-86ca-4358-bd37-8c34b8b4d9c5.png)

![logic](https://user-images.githubusercontent.com/110079763/221665277-37dc56bf-17af-4768-9cd1-661bf6d748c9.png)

# Q0 output bit Schematic for 8:3 Priority Encoder

![Screenshot from 2023-02-22 14-37-02](https://user-images.githubusercontent.com/110079763/221664273-e6d4036e-c1a9-45ab-8106-addaa1218cdc.png)

# Complete Circuit Schematic for priority Encoder

![Screenshot from 2023-02-22 15-21-34](https://user-images.githubusercontent.com/110079763/221664349-f0d41a8c-6aa3-4f46-8858-a07181b069d3.png)

# Output(for input I5,I4,I3,I2,I1 = 1 )

![Screenshot from 2023-02-22 15-21-11](https://user-images.githubusercontent.com/110079763/221664644-82764a95-d2c1-48ff-bb44-26f7f0687cfd.png)

# Priority Encoder simulation using verilog code and testbench

![priority encoder output](https://user-images.githubusercontent.com/110079763/220256036-72e8dfcd-876f-4d8e-b60c-962bce23d689.png)
## **_Verilog codes for designing digital blocks_**
1)Priority Encoder

```
module krunal_priority(i,d);
  // declare
input [7:0] i;
  // store and declare output values
  output [2:0] d;
  reg [2:0] y;
  always @(i)
  begin
   
        // priority encoder
        // if condition to chose 
        // output based on priority. 
        if(i[7]==1) y=3'b111;
        else if(i[6]==1) y=3'b110;
        else if(i[5]==1) y=3'b101;
        else if(i[4]==1) y=3'b100;
        else if(i[3]==1) y=3'b011;
        else if(i[2]==1) y=3'b010;
        else if(i[1]==1) y=3'b001;
        else
        y=3'b000;
     
   
  end
assign d = y;
endmodule
```
2) PIPO register
```
module krunal_pipo(clk,a,q);
input clk;
input[2:0]a;
output[2:0]q;
reg[2:0]q;
always@(posedge clk)
begin
q<=a;
end
endmodule
```
3) LCD interfacing circuit
```
module lcd_2(
    clk,
  din,din1,din2,
  output reg rs, rw,
  output reg dout
    );

input reg clk;
input din;
input din1;
input din2; 

integer  i = 0;
 
reg [2:0] data = 0 ;



always@(posedge clk)
begin
data[0]  <= din; 
data[1] <= din1; 
data[2] <= din2; 
   
   if(i <= 2)
   begin
    rs   <= 1'b1;
    rw   <= 1'b0; 
    dout <= data[i];
    i <= i + 1; 
   end
 	else
   begin
   i <= 0;
   rs    <= 1'b0;
   rw    <= 1'b0;
   dout  <= 1'b0;
   end
	
end
 

endmodule
```

## **_Complete crack sensing Circuit and Simulation results_**

# Black Box for CSC (7-I/O pins except Power rails )

![Screenshot from 2023-02-27 11-52-27](https://user-images.githubusercontent.com/110079763/221665930-b21d56f2-1087-4dc3-9550-5e20551d5c54.png)

# Integrated CSC Schematic 

![Screenshot from 2023-02-27 11-30-39](https://user-images.githubusercontent.com/110079763/221666080-05d5643d-636d-4d57-8376-2f9d1afd28af.png)

# Output Graphs (Giving the desired 101 output)

![Screenshot from 2023-02-27 11-51-36](https://user-images.githubusercontent.com/110079763/221666319-4499c481-d760-4ab8-bb9d-18367f28d641.png)

# **_Graph of all I/O pins_** 


![Screenshot from 2023-02-28 00-34-15](https://user-images.githubusercontent.com/110079763/221763747-0bb58bed-4313-4863-a19a-5424c02276fb.png)


## Acknowledgments


- Kunal Ghosh, Director, VSD Corp. Pvt. Ltd.
- Madhav Rao, Associate Professor, IIIT Bangalore
- Kavya Agarwal, Mtech ECE student, International Institute of Information Technology, Bangalore

```
# Contact Information

- Ritesh Lalwani, Mtech ECE student, International Institute of Information Technology, Bangalore  ritesh7328@gmail.com
- Kunal Ghosh, Director, VSD Corp. Pvt. Ltd. kunalghosh@gmail.com

```
# *References*

- https://github.com/krunalbadlani/crack_sensing_circuit
