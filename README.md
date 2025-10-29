# OpenROAD Installation :

* OpenROAD is an open-source, foundational application for semiconductor digital design that provides a complete RTL-to-GDSII physical design flow.

  ## Steps to install OpenROAD :

  ### Clone the OpenROAD Repository:
  
  * Clone the OpenROAD Repository by follwing command.
 
         git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts


    <img width="1920" height="990" alt="Screenshot (134)" src="https://github.com/user-attachments/assets/51d2ff42-7f5a-4e14-95a2-58cb9e8625bb" />

 * When i run directly above command then it gives me the error so that i try with below commands and after that it successfully clone the repository.

          git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
          cd OpenROAD-flow-scripts/
          git pull --recurse-submodules
          
          rm -rf OpenROAD-flow-scripts
          
          git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts

* After that run the setup script.

          sudo ./setup.sh

  <img width="1920" height="1014" alt="Screenshot (135)" src="https://github.com/user-attachments/assets/f749bb23-ff22-4351-8008-dc060a00c930" />

  <img width="1920" height="900" alt="Screenshot (138)" src="https://github.com/user-attachments/assets/f2bd5bdc-f78a-42fb-9ae3-b542f43a4034" />

* After that run following commands line by line.

          source Build.sh

          cd OpenROAD-flow-scripts/

          sudo ./setup.sh

          ./build_openroad.sh --local

          source ./env.sh

* Note : This all things will not done easily , it will take some debugging skills, but don't worry whenever error comes simply take screenshot of that error and give it to chatgpt and try to resolve it. that's it.

  <img width="1920" height="989" alt="Screenshot (141)" src="https://github.com/user-attachments/assets/c5db7b22-5176-4779-b8d5-cb43938d1980" />

  ### Run the OpenROAD Flow :

          cd flow/

          make

  <img width="1920" height="1002" alt="Screenshot (143)" src="https://github.com/user-attachments/assets/14c4ef63-1006-4523-99a4-14b55b9a81db" />

  ### Launch the GUI for final layout :

          make gui_final

  <img width="1920" height="993" alt="Screenshot (146)" src="https://github.com/user-attachments/assets/c008b17d-e493-44de-895f-a91f57c570a5" />

  <img width="1920" height="1014" alt="Screenshot (147)" src="https://github.com/user-attachments/assets/3d0d52f6-6c24-43df-b4f6-8a5f89b4e67b" />

  * So this way, the installation is complete of OpenROAD, and now we are ready for the perform the RTL to GDS Flow.


# Floorplanning and Placement of the VSDBabySoC using the OpenROAD :



## 1.   Make the directories :

* Inside OpenROAD-flow-scripts/flow/designs/sky130hd/, create a folder named vsdbabysoc.

* Create other folder named vsdbabysoc in OpenROAD-flow-scripts/flow/designs/src/ and place all Verilog files here.

  <img width="1920" height="824" alt="Screenshot (149)" src="https://github.com/user-attachments/assets/784da46c-45fb-41f8-9c43-66e0aede5f8f" />



## 2. Copy Folders :

* From your VSDBabySoC folder, copy the following folders into sky130hd/vsdbabysoc:
  
   gds: Contains avsddac.gds, avsdpll.gds. 
  
* include: Contains sandpiper.vh, sandpiper_gen.vh, sp_default.vh, sp_verilog.vh.
  
* lef: Contains avsddac.lef, avsdpll.lef.
  
* lib: Contains avsddac.lib, avsdpll.lib.

  <img width="1920" height="1024" alt="Screenshot (152)" src="https://github.com/user-attachments/assets/4dcd626f-df97-4c56-8438-8dea0885cbcb" />

 * As shown in above image copy the folders from the vsdbabysoc to OpenROAD directory.
 
<img width="1920" height="1014" alt="Screenshot (153)" src="https://github.com/user-attachments/assets/351ebf44-2a8a-443f-8429-3e9b131c3ca9" />

* In Above image we see that folders are copied successfully.



 ## 3. Copy Constraint and Configuration Files:

*  Copy vsdbabysoc_synthesis.sdc into sky130hd/vsdbabysoc.
  
*  Copy macro.cfg and pin_order.cfg into sky130hd/vsdbabysoc.



## 4. Create Config File:

*  Create a config.mk file in sky130hd/vsdbabysoc with the required configuration details.

* The content for the config.mk file should be as represented below.


         # Design and Platform Configuration
         export DESIGN_NICKNAME = vsdbabysoc
         export DESIGN_NAME = vsdbabysoc
         export PLATFORM    = sky130hd

         # Design Paths
         export vsdbabysoc_DIR = /home/nevil/week5/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc

         # Explicitly list Verilog files for synthesis
         export VERILOG_FILES = ~/week5/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc/vsdbabysoc.v \
                         ~/week5/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc/rvmyth.v \
                         ~/week5/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc/clk_gate.v
   

         # Include Directory for Verilog Header Files
         export VERILOG_INCLUDE_DIRS = $(vsdbabysoc_DIR)/include

         export YOSYS_OPTIONS += -I$(vsdbabysoc_DIR)/include

         # Constraints File
         export SDC_FILE = $(vsdbabysoc_DIR)/vsdbabysoc_synthesis.sdc

         # Additional GDS Files
         export ADDITIONAL_GDS = $(vsdbabysoc_DIR)/gds/avsddac.gds \
                            $(vsdbabysoc_DIR)/gds/avsdpll.gds

         # Additional LEF Files
         export ADDITIONAL_LEFS = $(vsdbabysoc_DIR)/lef/avsddac.lef \
                            $(vsdbabysoc_DIR)/lef/avsdpll.lef

         # Additional LIB Files
         export ADDITIONAL_LIBS = $(vsdbabysoc_DIR)/lib/avsddac.lib \
                            $(vsdbabysoc_DIR)/lib/avsdpll.lib

         # Pin Order and Macro Placement Configurations
         export FP_PIN_ORDER_CFG = $(vsdbabysoc_DIR)/pin_order.cfg
         export MACRO_PLACEMENT_CFG = $(vsdbabysoc_DIR)/macro.cfg

         # Clock Configuration
         export CLOCK_PORT = CLK
         export CLOCK_NET  = $(CLOCK_PORT)
         export CLOCK_PERIOD = 11

         # Floorplanning Configuration
         export DIE_AREA   = 0 0 1600 1600
         export CORE_AREA  = 20 20 1590 1590

         # Placement Configuration
         export PLACE_PINS_ARGS = -exclude left:0-600 -exclude left:1000-1600 -exclude right:* -exclude top:* -exclude bottom:*

         # Tuning for Timing and Buffers
         export TNS_END_PERCENT     = 100
         export REMOVE_ABC_BUFFERS  = 1
         export CTS_BUF_DISTANCE    = 600
         export SKIP_GATE_CLONING   = 1

         # Magic Tool Configuration
         export MAGIC_ZEROIZE_ORIGIN = 0
         export MAGIC_EXT_USE_GDS    = 1

### Note : Add your directory location paths as your device repository location.

### This setup is very important for the floorplan and placement od the vsdbabysoc, So create proper setup and add all the required files for it.

* The structure of the files should be as shown in below image after the setup completion.

<img width="1920" height="879" alt="Screenshot (155)" src="https://github.com/user-attachments/assets/45ce8351-03f1-42dc-bd11-dc1649e20618" />


* Now go to the terminal and run the following commands.

         cd OpenROAD-flow-scripts

         source env.sh

         cd flow

  ## Run the Synthesis :

         make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk synth

  <img width="1920" height="1014" alt="Screenshot (161)" src="https://github.com/user-attachments/assets/f7405d6c-ae69-475d-9dda-c80cda8f6622" />

  <img width="1920" height="1011" alt="Screenshot (160)" src="https://github.com/user-attachments/assets/fdf76a71-b07a-41fb-9c3c-481851ea556d" />



## Synthesis Netlist :

         gvim results/sky130hd/vsdbabysoc/base/1_2_yosys.v
         

  <img width="1920" height="1024" alt="Screenshot (167)" src="https://github.com/user-attachments/assets/fddc7912-82ec-4bc0-8ae6-ef271dba271e" />

  <img width="1920" height="1011" alt="Screenshot (163)" src="https://github.com/user-attachments/assets/c0a08970-317b-4886-8ef6-43a43ec52492" />

  <img width="1920" height="1004" alt="Screenshot (164)" src="https://github.com/user-attachments/assets/b9e7870f-e03f-42d8-995b-a0ebceb5ddb9" />

  <img width="1920" height="1007" alt="Screenshot (165)" src="https://github.com/user-attachments/assets/95e90ef2-3ce8-4408-b20d-8d526842e1fd" />



## Synthesis log :

         gvim logs/sky130hd/vsdbabysoc/base/1_2_yosys.log

<img width="1920" height="1014" alt="Screenshot (162)" src="https://github.com/user-attachments/assets/d4c43e35-78bf-442f-bb91-1c552210c226" />


## Synthesis Check :

         gvim reports/sky130hd/vsdbabysoc/base/synth_check.txt

<img width="1920" height="1004" alt="Screenshot (166)" src="https://github.com/user-attachments/assets/a6d092f3-88c0-48be-9047-8c2f076710ab" />


## Sythesis Stats : 

         gvim reports/sky130hd/vsdbabysoc/base/synth_stat.txt

<img width="1920" height="1017" alt="Screenshot (168)" src="https://github.com/user-attachments/assets/5cb552ba-82e9-46d8-8124-a2e0f4c9273b" />

<img width="1920" height="1017" alt="Screenshot (169)" src="https://github.com/user-attachments/assets/abef5509-dbc4-42cd-b9cb-eddebcd3aa88" />



## Floorplan :

        make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk floorplan


* i am getting the syntax error at line no. 54 in that basically comment is represented by //. So, remove that line numbers from 54 to 58 . and also in last portion of the file two blocks are represented by // So that instead of // , use /* and */ for comment, means remove the // and add /* and */ in that last two blocks of the file. After this change command will successfully run.


<img width="1920" height="1024" alt="Screenshot (171)" src="https://github.com/user-attachments/assets/2d08acae-aab4-4281-ab8d-ddf76b99d535" />
 
<img width="1920" height="1017" alt="Screenshot (170)" src="https://github.com/user-attachments/assets/5121412f-ff5c-4a71-bb71-d902ba6376bf" />

<img width="1920" height="1017" alt="Screenshot (172)" src="https://github.com/user-attachments/assets/8a4c7ea3-8dea-48d0-8d08-cb845e7ad63d" />


## Floorplan GUI :

        make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_floorplan

<img width="1920" height="1004" alt="Screenshot (173)" src="https://github.com/user-attachments/assets/fe020dc3-cb39-43c7-a129-9957c47410aa" />


<img width="1920" height="1014" alt="Screenshot (174)" src="https://github.com/user-attachments/assets/2c4c8b49-1e93-4b43-918b-fb03d3021cfd" />

<img width="1920" height="1004" alt="Screenshot (184)" src="https://github.com/user-attachments/assets/95307c71-2603-4869-928d-c9b1ec53d132" />
  
<img width="1920" height="1011" alt="Screenshot (186)" src="https://github.com/user-attachments/assets/e8416e1a-0a3c-416e-b8bc-97f69c961524" />



## Placement : 

        make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk place
        
<img width="1920" height="1011" alt="Screenshot (179)" src="https://github.com/user-attachments/assets/d2b6f7bf-8016-4002-8713-12cc01ff3302" />

<img width="1920" height="1011" alt="Screenshot (178)" src="https://github.com/user-attachments/assets/470bf3c6-4d62-4643-93ad-478b7dbdcd20" />

<img width="1920" height="1008" alt="Screenshot (180)" src="https://github.com/user-attachments/assets/cda96c3a-394e-4457-9aeb-25d869659bf6" />

<img width="1920" height="1007" alt="Screenshot (175)" src="https://github.com/user-attachments/assets/8df0fc3b-71bf-445f-8e2f-fbe247d4c9d6" />



## Placement GUI :

         make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_place
         

<img width="1920" height="1014" alt="Screenshot (181)" src="https://github.com/user-attachments/assets/4333a0c9-0e7a-4839-874b-fd47b59fccb7" />

<img width="1920" height="1007" alt="Screenshot (182)" src="https://github.com/user-attachments/assets/aef708d9-2407-4c6f-b763-0e70cf64a8cb" />

<img width="1920" height="1021" alt="Screenshot (183)" src="https://github.com/user-attachments/assets/cb6485cd-2603-44d5-a8e9-4e32862f4668" />


### Try to Analyze and understand all reports. 


