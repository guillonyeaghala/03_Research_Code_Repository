Command Shell

A command shell, also known as a command-line interpreter, is a text-based interface that allows users to interact with a computer's operating system by typing commands. It reads user input, evaluates it, and executes it as a shell command or passes it to the OS for execution. Essentially, it's a program that sits between the user and the operating system, interpreting commands and managing the system's resources. 

Key aspects of command shells:
	•	Text-based interface: Unlike graphical user interfaces (GUIs), command shells require users to type commands. 
	•	Command execution: The shell interprets commands and executes them, either directly or by passing them to the operating system. 
	•	Scripting: Many shells can also execute commands from script files, allowing for automation and repetitive tasks. 
	•	Programming features: Some shells include programming features like variables, flow control, and functions, making them capable of more complex operations. 
	•	Terminal: A terminal is the application that provides the text-based interface for hosting a command shell.
 
Examples of command shells:
	•	Windows Command Shell (cmd): The original command-line interpreter on Windows, used for basic system management. 
	•	Windows PowerShell: A more powerful shell on Windows, with object-oriented capabilities and extensive cmdlets (commands). 
	•	Bash: A popular shell on Linux and macOS, known for its scripting capabilities and extensive command set. 
	•	zsh: Another popular shell on macOS, known for its features like autocompletion and plugins. 
	•	AI Shell: An interactive shell used to communicate with AI services. 
	•	netsh: A network shell on Windows for configuring and displaying network components. 
Khanipov & Golovko Compute lab setup

Hi Clem,
I hope you had a wonderful weekend. Can we create user accounts for Guillaume (cc'ed) on Farnsworth and Pazuzu please? 
Best,
Kamil. 

Ka-Yiu Wong (Clem)
ka2wong@utmb.edu

Connecting to Pazuzu and Farnsworth using SSH

Communication with Dr. Khanipov

Hi Guillaume,
I apologize, I keep looking to write proper documentation to share. However, I keep running out of time and something gets in the way. Here is a very brief overview to get started and then I will expand on it.

Pazuzu: 129.109.117.7
Farnsworth: 129.109.117.8
VDS-GRE: vds-gre.utmb.edu

VDS-GRE is shared, Pazuzu and Farnsworth are dedicated to our group. They have 512 GB ram, 64 cores, ~300TB HDD space. I use winscp to connect to pazuzu and Farnsworth. Some of the students like to use remote desktop for the visual interface login. 

The logins utilize your UTMB credentials (e.g., for me kakhanip and UTMB password). 

For your data storage that is shared among multiple users I created a folder for you /data2/Israni_Group

Otherwise you can use your home directory as well. The home directory sits on /data1/ which is a RAID6 and /data2/ is a RAID 60. These are two different file servers (data1-MOM, data2-Femputer).

Please, feel free to start data transfers and I will work on getting more information.

Best,
Kamil. 





Step 1: Connect to UTMB VPN



Step 2: I got Putty and WinSCP installed by calling UTMB IT services (ITS Service Desk), but I do not have the right log in (username@msi.umn.edu) for putty. Will try to connect to filezilla or Winscp to move files Data Storage FAQs | Minnesota Supercomputing Institute

Step 3: Connecting via WinSCP or Filezilla

File Transfer FAQs | Minnesota Supercomputing Institute
How do I use FileZilla to transfer data? | Minnesota Supercomputing Institute
How do I transfer data between a Mac or Windows computer and MSI Unix computers? | Minnesota Supercomputing Institute

Downloading WinSCP https://winscp.net/download/WinSCP-5.15.1-Setup.exe/download

I was able to connect to Pazuzu and Farnsworth using WinSCP but my access to VDS-GRE was denied. Will need to follow up on that




Location of the Israni Group Data at UTMB

Kingsley, Nicholas C.
Software Systems Spclst II, ITS-OPS-TechSrvs

Service Request# 117959

“Good afternoon! Try clicking on the following link:
 
\\vlp1researchps01.utmb.edu\IsraniMissionStudy
 
That is the path to the share that has been configured. If it pulls up successfully let me know and I will provide some instructions on how to map it locally for easier access. Thanks!!”

This created a 5TB network drive where all of the microbiome and genomics is located. You can just copy the address provided by Nick into the file explorer tab as shown below



Once you are logged into the UTMB VPN, you can type in /data2/Israni_Group into the directory in FileZilla to locate the folder where the data will be transferred to. Of note, you can also change to the network drive Nick created by typing “\\vlp1researchps01.utmb.edu\IsraniMissionStudy” in the file explorer tab next to the “my documents” in the picture below.





Connecting to the Israni lab drive using WinSCP








Casey’s files from MSI

Casey MSI dataset notes
Casey Dorr’s MSI folder location on Filezilla
	•	/panfs/jay/groups/37/dorr0022
Ajay’s MSI folder location on Filezilla
	•	/panfs/jay/groups/4/isrania
Useful folders for older sequencing files
	•	/panfs/jay/groups/4/isrania/isran001/pre2018_data_release_archive
	•	/panfs/jay/groups/4/isrania/isrania
Casey’s Recent project sequencing files in the isrania MSI group
	•	/common/data_release/isrania/novaseq/200302_A00223_0333_AHHHN5DRXX/Dorr_Project_001
	•	/data_delivery/isrania/umgc/2023-q3/200302_A00223_0333_AHHHN5DRXX/Dorr_Project_001_Relabel
	•	/data_delivery/isrania/umgc/2024-q2/210913_A00223_0647_AHLYJYDSX2/Dorr_Project_008
	•	/data_delivery/isrania/umgc/2024-q2/240301_LH00315_0114_A22HTVTLT3/Dorr_Project_013
	•	/data_delivery/isrania/umgc/2024-q2/240402_LH00315_0148_A22JWJVLT3/Dorr_Project_018
These 5 folders above will be moved to /panfs/jay/groups/37/dorr0022/shared (These are all available in (\\vlp1researchps01.utmb.edu\IsraniMissionStudy)



	
	
Setting up Putty for terminal access?

Connecting to HPC Resources | Minnesota Supercomputing Institute

ITS Service Desk (I can request administrative access for a day to install software)

Connecting to HPC Resources | Minnesota Supercomputing Institute

Option 1: According to (Connecting to HPC Resources | Minnesota Supercomputing Institute) I should be able to ssh directly if I know the IP address












	





Copy data from a command prompt window (https://stackoverflow.com/questions/11543578/copy-text-from-a-windows-cmd-window-to-clipboard)

ssh gconyeag@129.109.117.7
/home/gconyeag
cd /data2/Israni_Group

cd //vlp1researchps01.utmb.edu/IsraniMissionStudy
cd /vlp1researchps01.utmb.edu/IsraniMissionStudy

Option 2: Plug in the correct IP addresses for Pazuzu / Farnsworth into PuTTY






Connecting Pazuzu and Farnsworth to the Israni network drive via command shell

Good afternoon Guillaume,

Please do this command to access the network drive.

cd /mnt/isilon-IsraniMissionStudy

Please don't hesitate to let me know if you have any questions.

Thank you.

Ka-Yiu Wong (Clem)

Accessing the specific files

cd /mnt/isilon-IsraniMissionStudy/Israni_MSI_Backup_data

cd /mnt/isilon-IsraniMissionStudy/Israni_MSI_Backup_data/2024_Q4_Israni_Project_031_PK_Quintiles

How to copy the files to the israni folder

https://stackoverflow.com/questions/14922562/how-do-i-create-a-copy-of-a-directory-in-unix-linux

The option you're looking for is -R.
cp -R path_to_source path_to_destination/

cp -R /mnt/isilon-IsraniMissionStudy/Israni_MSI_Backup_data/2024_Q4_Israni_Project_031_PK_Quintiles /data2/Israni_Group/2024_Q4_Israni_Project_031_PK_Quintiles

Testing out the rsync option to see if it is faster (It is faster!)

https://www.geeksforgeeks.org/rsync-command-in-linux-with-examples/#

Copy/Sync files and directory locally
If neither the source or destination path specifies a remote host, the rsync commands behave as a copy command.
rsync -avh foo/ bar/ 
The above command will copy/sync all the files and directories present in directory foo to directory bar. If the destination directory is not present (here bar), rsync automatically creates one and copies all the data in it

rsync -avh /mnt/isilon-IsraniMissionStudy/Israni_MSI_Backup_data/2024_Q4_Israni_Project_031_PK_Quintiles /data2/Israni_Group/2024_Q4_Israni_Project_031_PK_Quintiles_v2

Option 2(Not Necessary):

Go through the classic PUTTY setup (How do I configure PuTTy to connect to MSI Unix systems? | Minnesota Supercomputing Institute)



Syncing data from UTMB VPN drive to Pazuzu and Farnsworth

Hi Guillaume,
Can you create an account here at https://tacc.utexas.edu/, please? I'll add you to my project allocation, and you can use $SCRATCH as the space to synchronize the transfers between the servers. Here is the guide to usage: https://docs.tacc.utexas.edu/hpc/lonestar6/

In essence, it would be:
1. Rsync to $SCRATCH on ls6.tacc.utexas.edu using your old servers
2. Rsync from $SCRATCH using our servers

Best,
Kamil. 

Getting Started

Username: guillaumeo2025
Password: SpringSemester2025!

Linked it with Duo on my phone
Duo passcode for TACC token is : SpringSemester2025!

























Uploading the data to the shared drive for mounting

I checked the internet speeds, and once I connect to the VPN it does not matter if I am using the wired or wireless connection




Update: Dr. Khanipov recommended that we sync the vlp folder to their server by asking IT to grant them access as follows. I have emailed Nick on the IT team and cc’ed clem and Dr. Khanipov



Analysis of BGUS transcripts

04/16/2025

For combining multi-omics, we can start here in terms of capability and output 
 
https://mixomics.org/mixdiablo/
https://mixomics.org/
 
	
	
Accessing Conda on Pazuzu / Farnsworth





*** Alternative method (We did not use this)


*** Back to the setup

Go to home in pazuzu via WinSCP



Hit Ctrl + alt + H for hidden files and edit the .bashrc hidden file with the two lines of code below

# Conda setup
source /raid/software/miniconda3/etc/profile.d/conda.sh



Now open up a shell and connect to Pazuzu: You should now be able to load up conda using

conda –version



However, you will not yet be able to create a conda environment as you see below


Run the command below to list all your conda environments

conda env list

Run this in conda before setting up environments (the conda config file will create a .condarc file in your home folder)

conda config --set changeps1 false 
conda config --set auto_activate_base false 
conda config --set env_dirs ~/.conda/envsconda 
config --set pkgs_dirs ~/.conda/pkgs

Once you can see the hidden .condarc file, add these two lines in to the text file, after editing the username

pkgs_dirs:
  - /home/justtngu/.conda/pkgs
envs_dirs:
  - /home/justtngu/.conda/envs

pkgs_dirs:
  - /home/gconyeag/.conda/pkgs
envs_dirs:
  - /home/gconyeag/.conda/envs




Then, run the following code to create the conda environment through which you will access jupyter notebooks

conda create -n jupyternb

environment location: /home/gconyeag/.conda/envs/jupyternb

conda activate jupyternb

conda install notebook #this is specific command to install jupyter notebook within your environment of choice


(jupyternb) [gconyeag@pazuzu ~]$ exit

To connect from command prompt, it is better to set up a tunnel first

From the command line

ssh -L 8888:localhost:8888 username@129.109.117.7
 
ssh -L 8888:localhost:8888 gconyeag@129.109.117.7


Then access the conda environment with jupyter notebook installed

conda activate jupyternb

(jupyternb) [gconyeag@pazuzu ~]$ jupyter notebook #this command will then launch the notebook



 Copy the link from the page above and you will be taken to jupyter notebooks in your browser
