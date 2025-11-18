# Whole-Brain Thalamocortical Model

## Overview

This repository contains the **public release of the whole-brain thalamocortical model** developed by the Bazhlab at UCSD. The model aims to simulate large-scale brain dynamics by integrating thalamocortical circuits using high-performance C++ code. It is designed for neuroscientists, computational modelers, and anyone interested in the study of brain network dynamics.

## Getting Started

### Prerequisites

- GNU Make
- C++11-compatible compiler (e.g., g++, clang++)

### Cloning the repository

   ```bash
   git clone https://github.com/bazhlab-ucsd/whole-brain-public.git
   cd whole-brain-public
   ```
### Generating Connectivity

If a connection file (e.g. connection_info) is not already available, it can be generated using generate_network.cpp through Makefile, running network options like:
   ```bash
   make network
   ```
or
   ```bash
   make network_mri_LH_multilayer
   ```
The required input files are listed in Makefile for each case.

### Running Simulations

To run a simulation, use
   
   ```bash
   make run
   ```
Output data will be generated in the output folder specified in Makefile (third agrument in "run").

### Simulation parameters

See params.txt to setup and modify simulation parameters. (e.g. "tmax" sets simulation time in miliseconds)

## Stimulation

1. Stimulation is implemented as injected current to target populations of cells. General stimulation parameters can be set up in params.txt
   ```cpp
    # Turn stimulation On (1: on; 0: off)
    stim_on 1 

    # Stim Start & Stop Times (duration of pulses hard coded in CellSyn.cpp)
    stim_start 10000
    stim_stop 10500
    
    # Stimulation strength
    stim_stren 0.5
   ```
2. Additional stimulation configuration can be modified in CellSyn.cpp inside CellSyn::calc()
  ```cpp
   // STIMULATION
  int period=5000; // <------------- set period in ms for periodic stimulation pulses
  double stime=fmod(x,period);
  
  if (stim_on == 1){
    if(x>=stim_start&&x<stim_stop){
      // 500 ms pulses @ start of each second
      if ((stime>=0)&&(stime<500)){ // <------------- set duration of pulse in ms 
        // Stim all of clust 1
        if (this->type == E_CX && stim_ids[this->m]==1){ // && (rand01() <= .8)){  //E_L2 <------ set target layer here (e.g. this->type == E_CX4 for L4, etc)
          current=current + stim_stren;
        }
  ```
3. Specific identities of target cells to be stimulated within a layer can be provided in an external file (e.g. c1half_rand.csv). The file should be a single column of 1 or 0 zero values, one for each cell in the target layer. Only cell indices with a 1 will receive input current. The file is provided as an input in Makefile "run".


## Documentation

- For details about input files, model parameters, and simulation options, please see comments in source and sample configuration files.

## Acknowledgements

- Developed and maintained by [Bazhlab at UCSD](https://bazhlab.ucsd.edu).

---
