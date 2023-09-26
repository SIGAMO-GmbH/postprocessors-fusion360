# The Postprocessors we modified for our machines. 


## ESTLCAM

We have a small Gantry CNC. 

Our Modifications: 
- Output 1 (M8 / M9) for our vacuum table.
- Output 2 (M10 / M11) for the mist coolant.



## Fanuc Robodrill

Our Robodrill has a 3+2 Axis Setup. 

Our Modifications: 
- Our 4. and 5. Axis are called A and B in the machine.
- I programmed a delay, when drill with internal cooling is used. The smaller the drill, the longer the delay. This formula works for our pump, so that we always have the coolant flowing, before we  start the drilling.
- with longer tools in the machine we want the tool change to be in a save position. For shorter tools we do not need to move the table and just change the under the workpiece.
- Modified the tool list, to display the data show on our tool clips, so that we can double check that we have the right tool in the right position. 

### Wait for internal coolant


```
  // set coolant after we have positioned at Z
  setCoolant(tool.coolant);
  

  // if option activated, waits based on tool diameter to flush the tool. 
  if (getProperty("waitForInternalCooling")) {
    // tool.coolant == 3: Internal Cooling
    // tool.type == 1: Drill
    if (tool.coolant == 3 && tool.type == 1){
      onDwell(5 * (1/tool.diameter));
    }
  }

```

2,5  mm --> 2 sec  
5,0  mm --> 1 sec 
10,0 mm --> 0,5 sec 
works for us.. 
