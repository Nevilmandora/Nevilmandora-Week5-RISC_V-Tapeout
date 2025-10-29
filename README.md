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




  
