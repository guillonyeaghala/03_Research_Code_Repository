HHRI important information

To get access to the ASMIC files the path of WinSCP is : 
/panfs/roc/groups/13/prizm001/onyea005

To get access to the HHRI files the path of WinSCP is : 
/panfs/roc/groups/5/isrania

The Plink tutorial files are located at 
/home/ isrania/shared/DeKAF-GWAS/plink/


*** To copy any line code from the tutorial to the terminal window, select the code and hit CTRL + C, then in the control window right click and it will copy the code 
*** To quit any command that is running in the terminal, hit q

Command Line Shortcuts

	•	Cd = change directory
	•	Ls = list files within a folder
	•	Mkdir = create a new folder
	•	Git + clone + pathfile + filename = copying a folder from another source
	•	Head = look at the first 10 lines of a file
	•	Clear = remove a command
	•	-I = input file
	•	-O = output file
	•	Mote.emperor = 3D plot package for ordination and clustering


*** To copy any line code from the tutorial to the terminal window on windows, select the code and hit CTRL + C, then in the control window right click and it will copy the code 
*** To quit any command that is running in the terminal, hit q

Using MSI for bioinformatics analyses background

A Minnesota Supercomputing Institute (MSI) account was created for
Guillaume Onyeaghala in the group prizm001 with the username onyea005.

In addition, you have been granted access to the prizm001 group's Service Units.
This access will allow you to run jobs on MSI's High-Performance Computing resources.

Passwords
---------
Your password at MSI is the same as the password for your University Internet ID.
If you ever need to change or reset your password, go to https://www.umn.edu/dirtools.

Getting Started
---------------
Quick Start Guides for new MSI users are available on our website at https://www.msi.umn.edu/quick-start-guides

Frequently Asked Questions
--------------------------
You can find answers to a range of frequently asked questions at https://www.msi.umn.edu/support/faq

Getting Help
------------
Our help desk is staffed 8:30 to 5:00 Monday through Friday. Send an email to help@msi.umn.edu, call (612) 626-0802, or visit 587 Walter Library.

Background
Next generation sequencing
Over the last 7 years or so there has been rapid development of methods for next generation sequencing. Here is the process for the Illumina technology (one of the 3 major producers of platforms for next generation sequencing). The biological sample (e.g. a sample of mRNA molecules) is first randomly fragmented into short molecules. Then the ends of the fragments are adenylated and adaptor oligos are attached to the ends.
The fragments are then size selected, purified and put on a flow cell. An Illumina ow cell has 8 lanes and is covered with other oligonucleotides that bind to the adaptors that have been ligated
to the fragmented nucleotide molecules from the sample. The bound fragments are then extended to make copies and these copies bind to the surface of the flow cell. This is continued until there are many copies of the original fragment resulting in a collection of hundreds of millions of clusters.

The reverse strands are then cleaved off and washed away and sequencing primer is hybridized to the bound DNA. The individual clusters are then sequenced in parallel, base by base, by hybridizing fluorescently labeled nucleotides. After each round of extension of the nucleotides a laser excites all of the clusters and a read is made of the base that was just added at each cluster. If a very short sequence is bound to the flow cell it is possible that the machine will sequence the adaptor sequence-this is referred to as adaptor contamination. There is also a measure of the quality of the read that is saved along with the read itself.

These quality measures are on the PHRED scale, so if there is an estimated probability of an error of p, the PHRED based score is -10 log10 p. If we fragment someone's DNA we can then sequence the fragments, and if we can then put the fragments back together we can then get the sequence of that person's genome or transcriptome. This would allow us to determine what alleles this subject has at every locus that displays variation among humans (e.g. SNPs).

Study Design

There are a number of popular algorithms for putting all of the fragments back together: BWA, Maq, SOAP, ELAND and Bowtie. We'll discuss Bowtie in some detail later (BWA uses the same
ideas as Bowtie). ELAND is a proprietary algorithm from Illumina and Maq and SOAP use hash tables and are considerably slower than Bowtie.

We will focus on the Illumina technology as the University of Minnesota has invested in this platform (the HiSeq 2000 and MiSeq platforms is available in the UMN Genomics Center). The MiSeq machine produces longer reads than the HiSeq 2000. We also have some capabilities to do 454 sequencing here. The 454 platform produces longer reads (up to 800 bp now), and
is very popular in the microbiomics literature.

By sequencing larger numbers of fragments one can estimate the sequence more accurately. The goal of using high sequence coverage is that by using more fragments it is more likely that one fragment will cover any random location in the genome. Clearly one wants to cover the entire genome if the goal is to sequence it, hence the depth of coverage is how many overlapping fragments will cover a random location. By deep sequencing, we mean coverage of 30X to 40X, whereas coverage of 4X or so would be considered low coverage.

The rule for determining coverage is: coverage = (length of reads X number of reads)/ (length of
genome).  For a given sequencing budget, there is a tradeoff between the depth of coverage and the number of subjects one can sequence. The objectives of the study should be considered when determining the depth of coverage. If the goal is to accurately sequence a tumor cell then deep coverage is appropriate, however if the goal is to identify rare variants, then lower coverage is acceptable.

For example, the 1000 Genomes project is using about 4X coverage. A reasonable rule of thumb is that one probably needs 20-30X coverage to get false negatives less than 1% in nonrepeat regions of a duploid genome (Li, Ruan and Durbin, 2008). If the organism one is sequencing does not have a sequenced genome, the calculations are considerably more complex and one would want greater coverage in this case. Throughout this treatment we will suppose that the organism under investigation has a sequenced genome.

Another choice when designing your study regards the use of mate-pairs. With the Illumina system one can sequence both ends of a fragment simultaneously. This greatly improves the accuracy of the method and is highly recommended. If the goal is to characterize copy number variations then getting paired end sequences is crucial.

Quality Assessment

We can use the ShortRead package in R to conduct quality assessment and get rid of data that doesn't meet quality criteria. To explore this we will use some data from dairy cows. You can find a description of the data along with the actual data in sra format at this site.
http://trace.ncbi.nlm.nih.gov/Traces/sra/?study=SRP012475

While the NCBI uses the sra format to store data, the standard format for storing data that arises from a sequencing experiment is the fastq format. We can convert these _les to fastq files using a number of tools-here is how to use one supported at NCBI called fastq-dump.

To use this you need to download the compressed file, unpack it and then enter the directory that holds the executable and issue the command (more on these steps later).
sratoolkit.2.1.15-centos linux64/bin/fastq-dump SRR490771.sra

Then this will generate fastq _les which hold the data. If we open the fastq files we see they hold identifiers and information about the reads followed by the actual reads:

Then we define the directory where the data is and read in all of the filenames that are there
 library(ShortRead)
 dataDir <- "/home/cavan/Documents/NGS/bovineRNA-seq"
 fastqDir <- file.path(dataDir, "fastq")
 fls <- list.files(fastqDir, "fastq$", full=TRUE)
 names(fls) <- sub(".fastq", "", basename(fls))

Then we apply the quality assessment function, qa, to each of the files after we read it into R using the readFastq function

 qas <- lapply(seq along(fls), function(i, fls)
+ qa(readFastq(fls[i]), names(fls)[i]), fls)

This can take a long time as it has to read all of these files into R, but note that we do not attempt to read the contents of the files into the R session simultaneously (a couple of objects generated by the call to readFastq stored inside R will really bog it down). Then we rbind the quality assessments together and save to an external location.

 qa <- do.call(rbind, qas)
 save(qa, file=file.path("/home/cavan/Documents/NGS/bovineRNA-seq","qa.rda"))
 report(qa, dest="/home/cavan/Documents/NGS/bovineRNA-seq/reportDir")
[1] "/home/cavan/Documents/NGS/bovineRNA-seq/reportDir/index.html"

This generates a directory called reportDir and this directory will hold images and an html file that you can open with a browser to inspect the quality report. So we now use the browser to do this-we see the data from run 12 is of considerably higher quality than the others but nothing is too bad. One can implement soft trimming of the sequences to get rid of basecalls that are of poor quality. A function to do this is as follows (thanks to Jeremy Leipzig who
posted this on his blog). Note that this can require a lot of computational resources for
each fastq file.

# softTrim
# trim first position lower than minQuality and all subsequent positions
# omit sequences that after trimming are shorter than minLength
# left trim to firstBase, (1 implies no left trim)
# input: ShortReadQ reads
# integer minQuality
# integer firstBase
# integer minLength
# output: ShortReadQ trimmed reads
library("ShortRead")
softTrim <- function(reads, minQuality, firstBase=1, minLength=5)f
qualMat <- as(FastqQuality(quality(quality(reads))),'matrix')
qualList <-split(qualMat, row(qualMat))
ends <- as.integer(lapply(qualList, function(x)fwhich(x <
minQuality)[1]-1g))
# length=end-start+1, so set start to no more than length+1 to avoid
# negative-length
starts <-as.integer(lapply(ends,function(x)fmin(x+1,firstBase)g))
# use whatever QualityScore subclass is sent
newQ <-ShortReadQ(sread=subseq(sread(reads),start=starts,end=ends),
quality=new(Class=class(quality(reads)),
quality=subseq(quality(quality(reads)),
start=starts,end=ends)),id=id(reads))

# apply minLength using srFilter
lengthCutoff <- srFilter(function(x) f width(x)=minLengthg,
name="length cutoff")
newQ[lengthCutoff(newQ)]
g

To use:
 library("ShortRead")
 source("softTrimFunction.R")
 reads <- readFastq("myreads.fq")
 trimmedReads <- softTrim(reads=reads,minQuality=5,firstBase=4,minLength=3)
 writeFastq(trimmedReads,file="trimmed.fq")

Bowtie Background

The keys to understanding how the algorithm implemented by Bowtie are the Burrows Wheeler transform (BWT) and the FM index.

1. the FM index: see Ferragina, P. and Manzini, G. (2000), \Opportunistic data structures with applications"

2. the Burrows Wheeler transform (BWT): see Burrows, M. and Wheeler, D.J. (1994), \A Block-sorting lossless data compression algorithm".

The Burrows Wheeler transform is an invertible transformation of a string (i.e. a sequence of letters). To compute this transform, one 1) determines all rotations of the string 2) sorts the rotations lexicographically to generate an array M 3) saves the last letter of each sorted string in addition to the row of M that corresponds to the original string.
Intro to Linux, WinSCP and PuTTY
In order to use the latest tools in bioinformatics you need access to a linux based operating system (or Mac OS X). So we will give some background on how to use a linux based
system. The linux operating system is based on the UNIX operating system (by a guy named Linus Torvalds), but is freely available (UNIX was developed by a private company). There are a number of implementations of linux that go by different names: e.g. Ubuntu, Debian, Fedora, and OPENsuse.

Most beginners choose Ubuntu (or one of its o_shoots, like Mint). I use Ubuntu or Fedora on my computers and centos is used on the server we will use for this course. You can install it for free on any PC. Such systems are based on a hierarchical structure for organizing files into directories. We will use a remote server, so we need a way to communicate between the computer you are using and this remote machine.

To connect to a linux server from a Windows machine you need a Windows program that allows you to log on to a terminal and the ability to transfer files to and from the remote server and your
Windows machine.

WinSCP is a free program that allows one to do this. You can download the data from the WinSCP website and it installs like a regular Windows program: just allow your computer
to install it and go with the default configuration.

To get a terminal window you can use a program called PuTTY. So download the program PuTTY (which is just the file called PuTTY.exe) and create a subdirectory in your Program Files
directory called PuTTY. Then save PuTTY.exe in the directory you just created.

Once installed start WinSCP and use boris.biostat.umn.edu as the hostname along with your user name and password. You can then get a terminal window using putty: just click on the
box that shows 2 computers that are connected right below the session tab. Then disconnect when you are done (using the option in the session tab).

Much of the time we will use plain text files: these are files that have no formatting, unlike the files you use in a typical word processing program. To edit text files you need to use a text editor. I like vi and vim (which has capabilities to interact with large files). Sometimes we will encounter files we can't read with a text editor, for example binary files and compressed files.
The editor pico is simple to use and installed on the server we will use for this class.

To use this editor you type pico followed by the file name-if the file doesn't exist it will create the file otherwise it will open up the file so that you can edit it. For example: [user0001@boris ~]$ pico temp1 will open the file temp1 for editing. Commands at the bottom of the window tell you how to conduct various operations.

You can create subdirectories in your home directory by typing

[user0001@boris ~]$ mkdir NGS

This would create a NGS subdirectory where you can put files related to your NGS project. Then to keep your microarray data separate you could do

[user0001@boris ~]$ mkdir microarray

To move around hierarchical directory structure you use the command cd. We enter the NGS directory with

[user0001@boris ~]$ cd NGS

To go back to our home location (i.e. the directory you start from when you log on) you just type cd and if you want to go up one level in the hierarchy you type cd followed by the name of the file. To list all of the files and directories in your current location you type ls. To copy a file you use cp, to move a file from one location to another you use mv and to remove a file you use rm.
One can modify the behavior of these commands by using options.

For example, if I want to see the size of the files (and some additional information) rather than just list them I could issue the command

[user0001@boris ~]$ ls -l

Other useful commands include more and less for viewing the contents of _les and man for information about a command (from the manual). For instance

[user0001@boris ~]$ man ls

Will open a help file with information about the options that are available for the ls command.

Wild cards are characters that operating systems use to hold a variable value. These are extremely useful as it lets the user specify many tasks without having to do a bunch of repetitive typing. For example, suppose you had a bunch of files that ended with .txt and you wanted to delete them. You could issue commands like

[user0001@boris ~]$ rm file1.txt
[user0001@boris ~]$ rm file2.txt
[user0001@boris ~]$ rm file3.txt ...

Or you could just use

[user0001@boris ~]$ rm file*txt

or even

[user0001@boris ~]$ rm *txt

If you issue a command to run a program but wish you hadn't and want it to stop you push the Ctrl and C keys simultaneously.

UMN Virtual private network for research

https://it.umn.edu/services-technologies/virtual-private-network-vpn
For Mac, check https://it.umn.edu/services-technologies/how-tos/hstahc-vpn-remote-desktop-setup
Overview
A Virtual Private Network (VPN) is a service that allows you to connect to the University's network when you are not on campus. There are certain applications managed by the University that are limited to the University's network. To access these applications when you are off campus you will need to use VPN.
The University's VPN uses encryption technologies that enable data to be transmitted securely. Use VPN any time you are working on a computer using a shared, public internet connection (e.g. a coffee shop, hotel room) and are handling information you wish to be secure.
Most users should connect to “UofM Split” VPN network. This will allow access to University of Minnesota resources (such as files.umn.edu, PeopleSoft, and more) from off-campus. View more details regarding VPN Requirements for UMN Applications. 
Please note the following VPN technical details:
	•	Maximum connection time for a single session is 12 hours
	•	Idle timeout is 15 minutes
Idle timeout occurs when network traffic between your computer and the VPN system stops. This situation typically occurs when a computer goes to sleep or is powered down.
VPN should be used only when resources cannot be accessed by other means. A VPN is not needed for web-based UMN resources and services, such as umn.edu web pages, University Gmail, Canvas, and G Suite and Office 365, and others.
Highlights
	•	Provides increased privacy and security
	•	Enables access to some restricted U of M applications
Getting Started
Mac: VPN Setup 
In order to use remote desktop on a Mac, a VPN connection must be created. A VPN connection can be created by using the Cisco AnyConnect Secure Mobility Client
Note: You must select "Install as Administrator" when installing the Cisco AnyConnect Client 
To connect to VPN: 
	•	Open the Cisco AnyConnect Secure Mobility Client 
	•	Tip: If you cannot locate AnyConnect, try searching for it in Spotlight Search (Cmd + Space) in the upper right of the desktop
	•	Under the Ready to Connect Box select UMN - Department Pools
	•	Note: If the drop down box is blank and nothing appears, type tc-vpn-1.umn.edu into the blank box.
	•	In the Group field select :
	•	For Remote Desktop connections
	•	Select Departmental Pools
	•	AnyConnect-AHC01 (both personal and HST supported devices)
	•	For Shared Drive connections from HST-supported devices
	•	Select Departmental Pools
	•	Select AnyConnect-UofMvpnFull
	•	Note: HST supported devices can use either AHC01 or UofMvpn to get the servers
	•	AnyConnect-AHC01 - You won't be able to use the internet when connected to this VPN.
	•	AnyConnect-UofMvpnFull - You will be able to access the internet access when connected to this VPN.
	•	Note: Personal devices cannot connect directly to the shared drive
	•	If you are on a personal device you must first remote into an AHC computer and then connect to the shared drive from that computer
	•	Enter your Internet ID and password
	•	Select OK
	•	Note: If connected successfully you will see an icon on the dock appear displaying a padlock 
https://it.umn.edu/services-technologies/how-tos/downloads-guides-connect-anyconnect-vpn-1
Downloads and Guides: Connect to AnyConnect VPN for Windows 10
This article provides step-by-step instructions to connect to the Virtual Private Network (VPN) client Cisco AnyConnect using a Windows 10 computer. If you need to download the client, visit our Downloads & Guides page to download the client and find installation instructions. Both Full and Split Tunnel VPN Pools and certain Departmental Pools require Duo Two Factor Authentication to connect, using Duo and VPN is outlined in VPN: Using Duo Append Mode with Cisco AnyConnect. For assistance in connecting, including Login Failed messages, contact Technology Help for assistance.
Connecting to VPN with AnyConnect
	•	Navigate to Cisco AnyConnect Secure Mobility Client from the Start menu. > All apps > Cisco > Cisco AnyConnect Secure Mobility Client
	•	Select your connection preference from the drop-down menu. 


	•	UMN - Split Tunnel - General Access VPN Pool: For most UofM VPN needs, including shared drive access.
	•	AnyConnect - UofMvpnFull - Uses a UofM IP address for entirety of connection.
	•	Generally not needed, specific Application and resource requirements are found at VPN Requirements for UMN Applications.
**Important note: From January 2021, you are required to use your DUO account to sign into VPN. This should automatically occur and when I tested it, no additional installation was needed from me (01/26/2021)
	•	Departmental Pools - Your department may require use of their VPN Pool. 
	•	Select your specific departmental pool after clicking 'Connect'.
	•	If you can't see ANY options here, uninstall the client completely and download and install the newest version from our Downloads & Guides page, making sure to extract the ENTIRE AnyConnect folder, not just the installer file.
	•	Click Connect.
	•	Enter your Internet ID and password in the sign in window.
	•	Click OK.
Using AnyConnect
Where did the client go?
	•	The AnyConnect Client window may close once the connection is established
	•	Select the up-arrow in the lower-right hand corner of the taskbar () to find AnyConnect
	•	Hover your mouse over the Cisco AnyConnect logo to see a "tooltip" showing the status of the VPN connection
	•	If you can't find the icon, hovering over different icons will tell you the name of the program each icon opens

View general connectivity information
	•	Open the Cisco AnyConnect Client by clicking the logo in the taskbar on the bottom right.
	•	Click the Settings Icon.
	•	Select the Statistics tab.
Disconnect from VPN
	•	Open the Cisco AnyConnect Client by clicking the logo in the task bar on the bottom right.
	•	Click Disconnect.

How to connect to your work PC from your home (or MAC) running Windows:

1. Download and install recommended Cisco AnyConnect VPN client if you haven't already.
Located here:  https://it.umn.edu/services-technologies/virtual-private-network-vpn

2. Use search in the bottom left to locate "Remote Desktop Connection" application, Launch it.

3. For Computer:  enter your IP address 128.101.67.12
Leave Username as is (none specified), Click Connect.

4. If prompted to enter Username: ad.umn.edu\onyea005    and your UMN password
If not prompted, Select "More Choices", and "Use a different account" and then enter
ad.umn.edu\onyea005 and your password.  Hit OK.

5. Click Yes when yellow certificate verification message appears, and you should be logged in on your WBOB PC! Be sure to Sign Out when finished, NOT Shut Down.  Your PC needs to remain on and running for future remote connections.
How to connect to your work PC from your home MAC computer (Running MAC OS directly):

Before you start, you need the Microsoft Remote Desktop app (https://it.umn.edu/services-technologies/how-tos/hstahc-vpn-remote-desktop-setup)
Go to the App Store, search for Microsoft Remote Desktop, download it from iCloud
(click on the cloud, enter your iCloud account, download/install).

Follow these steps:

1. If the software is downloaded and installed, launch the Microsoft Remote Desktop app, then where
you see a plus(+) sign near the upper left, click that and choose Desktop.

2. In the window that launches, fill in the IP address (128.101.67.12) for "PC Name", and then (optionally) give it an easier to remember "Friendly Name"  (WBOB PC or something) then click Add

2a. Before you can connect, make sure that you have launched your Cisco AnyConnect VPN client and logged in with your UMN information.  If for some reason you need to download and install that still find it here: https://it.umn.edu/vpn-downloads-guides

3. Double click the icon that is created after you clicked ADD, then enter your UMN username and password (as if you were logging onto the PC from right in front of it).  Hit Continue, and when it prompts again (about certificate) hit Continue again.

It should connect and launch your profile on that PC.  From there, treat it as if you are sitting in front of it, as you normally would, and when finished be sure to LOG OUT (not shut down) in the bottom left corner so that that machine remains on and can be connected to later.
for cisco VPN, enter
tc-vpn-1.umn.edu
then pick










Connecting to and using MSI resources
https://www.msi.umn.edu/content/connecting-hpc-resources
https://www.msi.umn.edu/support/faq
https://www.msi.umn.edu/getting-started
https://www.msi.umn.edu/quick-start-guides
https://www.msi.umn.edu/content/connecting-hpc-resources
Summary
MSI provides access to a variety of High-Performance Computing (HPC) systems designed to tackle a wide range of computational needs. This document will show you how you can access all of MSI computational resources from your personal computer.


Skills/Software Needed
	•	If you are connecting from a computer running Windows OS you may need to download and install PuTTY.  OSX and Linux does not require additional software to connect.

	•	SSH: Your computer will communicate with MSI resources via SSH. More information about SSH. 
Connecting to MSI
SSH (Use Secure Shell)
SSH, also known as Secure Socket Shell, is a network protocol that provides administrators with a secure way to access a remote computer. SSH also refers to the suite of utilities that implement the protocol. Secure Shell provides strong authentication and secure encrypted data communications between two computers connecting over an insecure network such as the Internet. SSH is widely used by network administrators for managing systems and applications remotely, allowing them to log in to another computer over a network, execute commands and move files from one computer to another.
The main features of SSH include secure remote logins, terminal emulation, fully integrated secure file transfers, and secure tunneling of X11 traffic. Many University of Minnesota systems require secure telnet and FTP connections.
Install a SSH Client
	•	Unix/Linux
	•	OpenSSH is freely usable and re-usable by everyone under a BSD license. OpenSSH is available from http://www.openssh.org/portable.html.
	•	Windows
	•	Cygwin provides a Windows port of the most current OpenSSH tools.
	•	PuTTY is a full-featured, SSH compliant client for Windows. You can get Putty at   http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html, as well as PSCP, a tool for encrypted remote file transfer
	•	WinSCP is a freeware Secure Copy (SCP) protocol client for Windows, which can be used to connect to an SSH server, mainly to UNIX machines, for file transfer using the SCP service. http://winscp.net/eng/index.php
	•	SecureCRT software requires you to purchase a license. http://www.vandyke.com/products/securecrt/
	•	Mac OS X
	•	Mac OS X comes with OpenSSH pre-installed. Open the Terminal application from inside the Utilities folder, and then type "ssh user@server_name_or_IP_address" to get started.
	•	Portable OpenSSH is available at http://www.openssh.com/portable.html at no charge. The portable OpenSSH follows development of the official version, but releases are not synchronized. Portable releases are marked with a 'p' (e.g. 2.3.0p1) 
	
How do I transfer data between a Mac and MSI Unix computers?

(https://www.msi.umn.edu/support/faq/how-do-i-transfer-data-between-mac-or-windows-computer-and-msi-unix-computers#:~:text=For%20transferring%20data%20from%20Mac,For%20Windows%2C%20we%20recommend%20WinSCP.)

For transferring data from Mac / Windows computers to MSI Unix computers, any SFTP or SCP software can be used. Connect to login.msi.umn.edu (How is this affected by the login node change after January 2021??) using your MSI username and password and you will be able to transfer files to and from your home directory and group shared directory. For Windows, we recommend WinSCP. For Mac OS X, we recommend using FileZilla.

https://www.msi.umn.edu/support/faq/how-do-i-use-filezilla-transfer-data

Enter sftp://mangi.msi.umn.edu (or sftp://mesabi.msi.umn.edu) into the Host box, your username into the Username box, your password into the Password box, and 22 into the Port box. Click on Quickconnect.

File transfer from Mac clients connecting to MSI Unix servers can be done with many different clients. MSI recommends FileZilla as a reliable utility.

Filezilla

*** The new instructions make you enter the password once, then FileZilla will make enter an option to use DUO and log in. It will fail otherwise

	•	Select New Site to create a new connection
	•	Enter the following information in the General tab:
	•	Protocol: SFTP
	•	Host: enter one of our server names
	•	mesabi.msi.umn.edu
	•	mangi.msi.umn.edu
	•	Logon Type: Interactive
	•	User: your username


Select the Transfer Settings tab
	•	Select 'Limit number of simultaneous connections'
	•	Set simultaneous connections to: 1


	•	Click Connect
	•	In the first password box, enter your UMN account password
	•	In the second password box, type the corresponding number for the Duo prompt of your choice (example: 1 will typically Duo push)
	•	You can now use FileZilla to transfer files

 
	•	Enter sftp://login.msi.umn.edu into the Host box, your username into the Username box, your password into the Password box, and 22 into the Port box. Click on Quickconnect. (Tried using sftp://mesabi.msi.umn.edu, username and password since login node changes took place in Jaunary 2021) Fall 2022 Update. Both filezilla and winscp will request duo login as well.
	•	Enter sftp://mangi.msi.umn.edu (or sftp://mesabi.msi.umn.edu) into the Host box, your username into the Username box, your password into the Password box, and 22 into the Port box. Click on Quickconnect.

 
	•	The first time you connect you may get a window asking you to confirm the identity of the server. Click on the "Always trust this host" checkbox and click on OK to continue.
 

 
	•	You are now connected to the server, you can copy files to and from the remote machine by dragging them them back and forth.
 

To Shutdown FileZilla:
	•	While connected click on the "Disconnect" button in the FileZilla window.
 

 
To fix "Authentication Failed. Critical error: Could not connect to server" error when transferring files:
Filezilla on Macs cache passwords, which are not automatically updated.  This can cause an issue when a user changes their UMNid password and updates it within Filezilla when trying to connect to MSI login servers.  A connection with the new password will be established, but when the user attempt to move files, Filezilla will attempt to use the cached password, which causes an "Authentication failed.  Critical error: Could not connect to server" error.  

The fix is:
	•	Go to Filezilla preferences -> Select Interface in the left pane
	•	check the box Do Not Save Passwords. 

 To fix "Filezilla can't be opened because it is from an unidentified developer" error:
Newer Macs allow only certain approved programs to be installed by default. Filezilla is not recognized as an approved program, so if you receive an error such as 'Filezilla can't be opened because it is from an unidentified developer', you'll need to follow the below steps:
	•	Open 'System Preferences > Security & Privary > General
	•	Change 'Allow applications downloaded from' to 'Anywhere'








How do I transfer data between Windows and MSI Unix computers?

(https://www.msi.umn.edu/support/faq/how-do-i-transfer-data-between-mac-or-windows-computer-and-msi-unix-computers#:~:text=For%20transferring%20data%20from%20Mac,For%20Windows%2C%20we%20recommend%20WinSCP.)

For transferring data from Mac / Windows computers to MSI Unix computers, any SFTP or SCP software can be used. Connect to login.msi.umn.edu (How is this affected by the login node change after January 2021??) using your MSI username and password and you will be able to transfer files to and from your home directory and group shared directory. For Windows, we recommend WinSCP. For Mac OS X, we recommend using FileZilla.
WinSCP
(https://www.msi.umn.edu/support/faq/how-do-i-transfer-data-between-mac-or-windows-computer-and-msi-unix-computers)
If you are using the most recent version of Windows 10, you can perform basic ssh connections in your Command Prompt application. In this case, you can follow the OSX and Linux instructions below. 
For older versions of Windows, and for running graphical interfaces over SSH, see our PuTTy setup guide. (https://www.msi.umn.edu/support/faq/how-do-i-configure-putty-connect-msi-unix-systems)

***Note: Login nodes will be retired on February 3, 2021

Dear MSI User,
At the next maintenance day for MSI systems (Wednesday, February 3rd, 2021), the servers named login.msi.umn.edu will be retired from service. The major effect of this retirement will be a quicker login experience for many MSI computational users.

For those MSI users who are used to connecting to login.msi.umn.edu, and then connecting to their desired cluster, you will now be directly connecting to the cluster (mesabi or mangi) from your desktop or laptop by connecting to mesabi.msi.umn.edu, mangi.msi.umn.edu, etc. 

If you forget to connect directly to your desired HPC resource (mesabi, mangi, etc) after the February 3rd Maintenance Day is complete, no worries, your attempt to connect to login.msi.umn.edu will instead be redirected to mangi.msi.umn.edu. You will also see a warning message regarding differing keys on mangi than on the retired login server. You should verify that this key matches the key(s) published on MSI's website at https://www.msi.umn.edu/content/ssh-keys before establishing the connection.

You may try connecting to mesabi.msi.umn.edu or mangi.msi.umn.edu from your own computer right now if you wish. During this early access phase, please let us know if you cannot connect, or notice any odd behavior of the system that impacts your work.

To upload files from your Windows machine to your home directory on a Unix machine, use WinSCP.

	•	These directions explain how to transfer files back and forth between a Windows client and a Unix server. Please see the directions for remote access for an MSI Unix machine from a Windows client if you need to log in interactively.
	•	Download and install WinSCP, a graphical SCP (secure copy protocol) file transfer client for Windows. These directions are based on version 5.7.5; upgrade if needed.
	•	Double-click the WinSCP icon to reach this screen. If you just want to transfer files to or from your group home or lab scratch, you can skip steps 4 and 5 and proceed to step 6.

	•	(Optional: if you want to transfer files elsewhere other than login.msi) Click the "Advanced Options" box in the lower left-hand corner:

	•	(Optional: if you want to transfer files elsewhere other than login.msi) Under the Connection section, click on "Tunnel" to get to this screen and click "Connect through SSH tunnel". Fill in login.msi.umn.edu and your MSI username. Then click back in the left column on Session at the top. In preparation for changes after January 2021,I have tested using mesabi.msi.umn.edu as an alternate login and was able to see my folder)
Enter sftp://login.msi.umn.edu into the Host box, your username into the Username box, your password into the Password box, and 22 into the Port box. Click on Quickconnect. (Tried using sftp://mesabi.msi.umn.edu, username and password since login node changes took place in Jaunary 2021) Fall 2022 Update. Both filezilla and winscp will request duo login as well.



	•	(Mandatory for all cases) Enter the name of the MSI server you wish to use, such as itasca, cascade, hosting, etc, in this format: server.msi.umn.edu. (Do not use the actual name server.msi.umn.edu, which does not exist.) If you skipped steps 4 and 5, the only option here is login.msi.umn.edu. Enter your MSI username. Click "Save".

	•	Make sure the session is called something you want it to be called, then click "OK" on this screen:

	•	The first time you connect you will get a "Warning" window concerning the server's key fingerprint. This is normal. Click "Yes". You may get this window twice.

	•	Login to the session

	•	You will now get a window asking for your password. After entering your password click "OK". Do not save your password! For alternatives to saving your password using ssh keys contact help@msi.umn.edu.

	•	You may get the "Warning" message again at this point.
	•	You may be prompted for the password a second time.
	•	Once connected to the remote system, a window will appear showing the local and remote systems.

	•	To move to the desired directories, use the navigation bars at the top. The local system is on the left and the remote system on the right. The toolbar at the bottom of the window contains function keys for performing copies, deletes, etc.
	•	To perform an action, highlight the desired files or directories and press the appropriate function key (F5 for a copy). It will then prompt you for confirmation. Click "OK". Depending on the size of the file(s) and the speed of your network connection, an upload or download could take anywhere from a few seconds to several minutes.
	•	When you are ready to disconnect from the remote system, press F10 or just exit from the WinSCP program. (If you are on the Unix side and need to pull data from your Windows U: drive, you can use smbclient)

Coding on MSI with Windows OS

	•	Download and install PuTTY.n (https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)  
	•	Double-click on the PuTTY icon and a "PuTTY Configuration" window will pop up (as shown in the image below). In the "Host Name (or IP address)" box, enter: login.msi.umn.edu. 
	


	•	Double-click on the PuTTY icon and a "PuTTY Configuration" window will pop up (as shown in the image below). In the "Host Name (or IP address)" box, enter: mang.msi.umn.edu, or mesabi.msi.umn.edu depending on if you want to log into the interactive node or not 
	


	•	In the left panel, under Window, select "Translation". Ensure the "Remote character set" is UTF-8

 
	•	[This step is optional. It is required only if you wish to use programs with a graphical interface. Under the "SSH" category, select "X11". Check the box next to "Enable X11 forwarding". (Make certain you have a local X client for Windows such as Xming configured, or this step will have no effect and X programs will not be able to open the display.)


	•	Return to "Session" in the left panel and in the "Saved Sessions" box, type in the name you would like to use to refer to this session, then click "Save".

 
	•	The next time you start PuTTY you will be able to recall a saved profile by clicking on the name and then clicking on "Load".   Click on "Open" to start the connection.


	•	The first time you connect to a server you may be asked to cache the server fingerprint. This is a safety feature of ssh. Go ahead and click "Yes" for now. You may be asked twice.

 
	•	You will be asked for your password. You can avoid entering it again when connecting to another server when using ssh keys; please see the ssh keys page for instructions if interested.

*** Starting June 7th, 2023 we will need to update the SSH keys (https://www.msi.umn.edu/content/ssh-keys-renew-june-2023)

	•	PuTTY is now configured to connect to MSI's login node. From here, you can ssh to MSI's HPC clusters.

Coding on MSI with Linux OS/MAC (Changed after January 2021)

Terminal is a preinstalled application which can be used to connect to MSI systems. 

Connecting to Login Host 
Once you open the Terminal App or PuTTy type:
	•	ssh yourMSIusername@login.msi.umn.edu
	•	After January 2021, type ssh yourMSIusername@mangi.msi.umn.edu or ssh yourMSIusername@mesabi.msi.umn.edu
	•	ssh onyea005@mesabi.msi.umn.edu
	•	You will be prompted for your MSI password type it in then press enter. The terminal will not display your password as you type. 
	•	The first time you log into MSI systems you will be asked if you would like to cache the server fingerprint, type yes and press return.
**When you attempt to log in after June 2023, we will need to update the keys based on the guide found at (https://www.msi.umn.edu/content/ssh-keys-renew-june-2023)
Note:
Before proceeding with the instructions, ensure that you have the correct SSH key. The new key can be found at the top of the page.
The appropriate hostnames are:
agate.msi.umn.edu
mesabi.msi.umn.edu
mangi.msi.umn.edu
login-test.devops.stratus.msi.umn.edu - this host is only for demonstration purposes, and should be replaced with one of the appropriate hostnames
Windows Operating System:
Command Prompt:

	•	Option 1: Using Command Prompt
	•	Open Command Prompt.
	•	Type the following command, replacing "hostname" with the appropriate hostname in the note above : ssh-keygen -R hostname.
	•	Press Enter to execute the command.

 ssh-keygen -R agate.msi.umn.edu

 ssh-keygen -R mesabi.msi.umn.edu

 ssh-keygen -R mangi.msi.umn.edu

	•	Note: You should be connected to the eduroam network of UMN VPN for this command to be successful.

	•	You are now connected to the MSI Login host (or bastion host). This host will let you view the directories but you can not submit jobs or run interactive computation on this host. 
	•	Once connected to the Login host, note the word login in your terminal prompt, you can connect to Mangi or Mesabi via ssh.
	•	ssh mangi or ssh mesabi
	•	After January  2021, just use ssh yourMSIusername@mangi.msi.umn.edu or ssh yourMSIusername@mesabi.msi.umn.edu in terminal, instead of ssh yourMSIusername@login.msi.umn.edu as the first step to log in
	•	MSI prohibits moving directly from one system to another. If you are connected to Mangi and would like to connect to Mesabi you will first have to exit back to the Login Host, then ssh to the new system. Commands are highlighted with red boxes in the example below. 

	•	Each time you move between systems you will see the welcome message associated with the system you are connecting to. These messages often contain useful information and should be read. 
	•	NOTE: Like the login node, Mangi and Mesabi have specific nomenclatures in the prompt. When connected to Mangi your prompt will contain cn followed by some number when connected to Mesabi your prompt will contain ln followed by some numbers. This is a quick short cut to remind you which system you are connected to at any time. 
Coding on MSI with OSX V2
	•	Note: You should be connected to the eduroam network of UMN VPN for this command to be successful.
Terminal is a preinstalled application which can be used to connect to MSI systems. Terminal can be found in the Utilities folder found in the Applications folder. 

Terminal is a preinstalled application which can be used to connect to MSI systems. 

For OSX and Linux OS Terminal Apps
Once you open the Terminal App type:
	•	ssh yourMSIusername@login.msi.umn.edu
	•	After January 2021, type ssh yourMSIusername@mangi.msi.umn.edu or ssh yourMSIusername@mesabi.msi.umn.edu
	•	ssh onyea005@mesabi.msi.umn.edu
	•	You will be prompted for your MSI password type it in then press enter. The terminal will not display your password as you type. 
	•	The first time you log into MSI systems you will be asked if you would like to cache the server fingerprint, type yes and press return.


	•	You are now connected to the MSI Login host (or bastion host). This host will let you view the directories but you can not submit jobs or run interactive computation on this host. 
Connecting to Mangi and Mesabi (Itasca was one of the old queues) from Login Host (Changed after January 2021)
	•	Once connected to the Login host, note the word login in your terminal prompt, you can connect to Mangi or Mesabi via ssh.
	•	ssh mangi or ssh mesabi
	•	After January  2021, just use ssh yourMSIusername@mangi.msi.umn.edu or ssh yourMSIusername@mesabi.msi.umn.edu in terminal, instead of ssh yourMSIusername@login.msi.umn.edu as the first step to log in
	•	MSI prohibits moving directly from one system to another. If you are connected to Mangi and would like to connect to Mesabi you will first have to exit back to the Login Host, then ssh to the new system. Commands are highlighted with red boxes in the example below. 

	•	Each time you move between systems you will see the welcome message associated with the system you are connecting to. These messages often contain useful information and should be read. 
	•	NOTE: Like the login node, Mangi and Mesabi have specific nomenclatures in the prompt. When connected to Mangi your prompt will contain cn followed by some number when connected to Mesabi your prompt will contain ln followed by some numbers. This is a quick short cut to remind you which system you are connected to at any time. 


Brief Summary for connecting to MSI for analyses

	•	Connect to VPN for MAC using Cisco AnyConnect Secure Mobility Client
	•	Connect to FileZilla to be able to see the files being moved and created in a dynamic fashion
	•	Enter sftp://login.msi.umn.edu into the Host box, your username into the Username box, your password into the Password box, and 22 into the Port box. Click on Quickconnect. (Tried using sftp://mesabi.msi.umn.edu, username and password since login node changes took place in Jaunary 2021)

	•	Connect to Mesabi (recommended default for MSI to be connected to the supercomputers)
Alternatively, connect to MSI using Terminal on Mac/Linux, located in ApplicationsUtilitiesTerminal.app. 
*NOTE: Like the login node Mesabi and Itasca have specific nomenclatures in the prompt. When connected to Mesabi your prompt will contain ln followed by some number when connected to Itasca your prompt will contain node followed by some numbers. This is a quick short cut to remind you which system you are connected to at any time.
**You need to be connected to Cisco Anyconnect via VPN in order for this command to work and it will ask you to login via DUO

ssh yourusername@login.msi.umn.edu (use ssh yourusername@mangi.msi.umn.edu or ssh yourusername@mesabi.msi.umn.edu respectively)
ssh onyea005@mesabi.msi.umn.edu
	•	Check the folder you are in
pwd
* my home folder is when I first login is “/home/prizm001/onyea005”
* /home/isrania/onyea005
*Change the folder to the israni MSI group
cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
cd /panfs/roc/groups/5/isrania/onyea005
cd /panfs/roc/groups/5/isrania/onyea005/Israni4_Project_016




Choosing a Job Queue
Summary
Most MSI systems use job queues to efficiently and fairly manage when computations are executed.  A job queue is an automated waiting list for use of a particular set of computational hardware.  When computational jobs are submitted to a job queue they wait in the queue in line until the appropriate resources become available.  Different job queues have different resources and limitations.  When submitting a job, it is very important to choose a job queue which has resources and limitations suitable to the particular calculation.
This document outlines factors to consider when choosing a job queue.   These factors are important when choosing where to place a job. This document is best used on all MSI systems and in conjunction with the Queues page that outlines the resource limitations for each queue.

Please note that Mesabi's "widest" queue requires special permission to use. Please submit your code for review at: help@msi.umn.edu.

Guidelines
There are several important factors to consider when choosing a job queue for a specific program or custom script.  In most cases, jobs are submitted via PBS scripts as described in Job Submission and Scheduling. 

Overall System
Each MSI system contains job queues managing sets of hardware with different resource and policy limitations. MSI currently has three primary systems: the newest supercomputer Mesabi, the supercomputer Itasca, and the Lab compute cluster. Mesabi is MSI's newest supercomputer, with the highest performance hardware, and a wide variety of queues suitable for many different job types. Mesabi should be your first choice when doing any computation at MSI. Itasca is a supercomputer with queues most suitable for multi-node jobs which will complete within 1-2 days.  The Lab cluster is primarily for interactive software that is graphical in nature, and testing. Which system to choose depends highly on which system has queues appropriate for your software/script. The variety of queues on Mesabi will be suitable for most users, but the Queue page should be examined.

Job Walltime (walltime=)
The job walltime is the time from the start to the finish of a job (as you would measure it using a clock on a wall), not including time spent waiting to run. This is in contrast to cputime, which measures the cumulative time all cores spent working on a job. Different job queues have different walltime limits, and it is important to choose a queue with a sufficiently high walltime that enables your job to complete.  Jobs which exceed the requested walltime are killed by the system to make room for other jobs.  Walltime limits are maximums only, and you can always request a shorter walltime, which will reduce the amount of time you wait in the queue for your job to start. If you are unsure how much walltime your job will need start with the queues with shorter walltime limits and only move to others if needed. 

Job Nodes and Cores (nodes=X:ppn=Y)

Many calculations have the ability to use multiple cores (ppn), or (less often) multiple nodes, to improve calculation speed.  Certain job queues have maximum or minimum values for the number  nodes and cores a job may use.  If Node Sharing is enabled for a queue you can request fewer cores (ppn) than exist on an entire node.  If Node Sharing is not enabled then you must request resources equivalent to a multiple of an entire node.  All Itasca queues, and Mesabi’s widest and large queues, do not allow Node Sharing.

Job Memory (mem=)

The memory which a job requires is an important factor when choosing a queue. The largest amount of memory (RAM) that can be requested for a job is limited by the memory on the hardware associated with that queue.  Mesabi has two queues (ram256g and ram1t) with high memory hardware, the largest memory hardware is available through the ram1t queue.  Itasca also has two queues with high memory hardware (sb128 and sb256).

User and Group Limitations

To efficiently share resources, many queues have limits on the number of jobs or cores a particular user or group may simultaneously use.  If a workflow requires many jobs to complete, it can be helpful to choose queues which will allow many jobs to run simultaneously.  Mesabi allows more simultaneous jobs to run than Itasca.

Special Hardware

Some queues contain nodes with special hardware, GPU accelerators and solid-state scratch drives being the most common. If a calculation needs to use special hardware, then it is important to choose a queue with the correct hardware available. Furthermore, those queues may require additional resources to be specified (e.g., GPU nodes require ":gpus=X").

Queue Congestion

At certain times particular queues may become overloaded with submitted jobs.  In such a case, it can be helpful to send jobs to queues with lower utilization (node status). Sending jobs to lower utilization queues can decrease wait time and improve throughput. Care must be taken to make sure calculations will fit within queue limitations.

Accessing Software Resources
MSI provides access to approximately 400 software packages. To find out if a particular software package is installed, you can browse or search a list of the software packages that MSI provides under the Help and Documentation section of this website. The MSI webpage includes descriptions of software packages including example usage to help you get started.

Support Tiers

Software packages will be marked with a support tier -- either Primary, Secondary, or Minimal. Primary support is given to mature, popular scientific software important to a large fraction of the user base generally receive the highest level of support. These software are compiled, installed, benchmarked, and documented as a service to you. They are tested after major system upgrades, new versions are installed in a timely manner, and we expect to be able to offer advice to assist with the majority of inquiries. Conversely, Secondary or Minimally supported software are only useful to a very small number of users. These software may not be actively maintained, regularly updated, or tested after system updates. They may be removed without notice if usage is found to be too low to continue support.

How To Access Software

HPC and Interactive HPC Systems
To access software on Linux systems, use the module command to load the software into your environment. You can find the module name to load on the MSI software page for a package. For example, if you want to use the 2014b version of MATLAB, you would type in your job script or at the command line:

% module load matlab/R2014b
% matlab

To find out what module versions are available, consult the software page or use the vail command to search by name. For example to find out what versions of Python are available:

% module avail python

Windows Software
Windows-only software can be accessed and run via the Windows Virtual Environment (Citrix). Once a windows session is started you access software applications in the same manner as if you were using a physical Windows computer. 

Need a Specific Package?

First try and install it yourself in your MSI home directory following the guidelines below:

Software Installation Guide
Installing software on any Unix platform can be challenging for inexperienced and veteran users alike.
While it is impossible to cover every issue you may experience, the following steps will allow you to compile and install in your own home directory many of the libraries and scientific applications that you will come across.
Introductory Compiling
Building a Python interpreter from its source code is straightforward, requiring only a C compiler, and will serve as an instructive example.
Installation materials are most commonly distributed as compressed tar files with the extension .tar.gz or .tar.bz.
These files can be untarred and unzipped with the tar -xzf and tar -xjf commands, respectively.
We have downloaded the file Python-2.7.2.tar.gz from www.python.org to our home directory, so the appropriate command is:
tar -xzf Python-2.7.2.tar.gz
cd Python-2.7.2

Your next step should always be to review the distributed files for a README file, which can be viewed in a text editor, or some other file that is named to indicate it contains installation instructions.
In many cases, and as is the case with Python, a configure script is included. In the simplest case, all that is necessary to compile the code is to run the configurescript, then make.
By default many codes will try to install to system directories you cannot access. The recommended installation method is to create a directory in your home directory for software installation.

Pwd (It will display a result such as /home/xe2/nlabello/Python-2.7.2)

mkdir /home/xe2/nlabello/software/python
./configure --prefix=/home/xe2/nlabello/software/python/
make install

When the make install step completes you will have access to binary in /home/xe2/nlabello/software/python/bin




Log in to an interactive node, making sure you have enough time and memory (https://www.msi.umn.edu/content/interactive-queue-use-isub)
isub -n nodes=1:ppn=8 -m 16GB -w 8:00:00	
#I don’t think I need to log into mesabi or Itasca to run this job if I am logged into the interactive node since it logs me into the lab queue (So use mesabi instead)
MSI's LabQi Cluster will be shut down and decommissioned during February Maintenance (February 5, 2020.) Several months ago, MSI released the Mesabi Interactive Queue as a replacement for the LabQi Cluster (ssh lab.)  The Mesabi Interactive Queue can be used in real-time for tasks such as interactive data exploration, creating plots or images, visualizations, or testing sections of code that will be used on other MSI queues.

Here are several examples of how you can request an interactive HPC job on the new Mesabi Interactive Queue.

The basic syntax for submitting an interactive job on Mesabi (I assume that in combination with the new login on mesabi and mangi the syntax occurs after login into mesabi.msi.umn.edu):

qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I

The basic syntax for testing MPI code between 2 nodes:
qsub -l nodes=2:ppn=2,mem=8gb,walltime=12:00:00 -q interactive -W x=nmatchpolicy:exactnode -I

The basic syntax for creating a job with X-forwarding for GUI's and visulization:
qsub -X -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I

The full specifications of the Mesabi Interactive Queue can be found on the MSI Queues webpage. Also, more examples on how to use qsub -I for interactive use can be found on the Interactive queue use with qsub webpage.
https://www.msi.umn.edu/content/interactive-queue-use-srun
** Will need to learn how to use Slurm to schedule jobs (https://www.msi.umn.edu/slurm)
Back to the interactive login
qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I
Now you can look at the contents of the directory using the ls command:

ls -lh
You can always check the location of your current directory (aka folder) using the pwd command.  If you type:

pwd

*** To copy any line code from the tutorial to the terminal window on windows, select the code and hit CTRL + C, then in the control window right click and it will copy the code 
*** To quit any command that is running in the terminal, hit q
Copy the MACQIIME tutorial file to your MSI folder
To copy a folder path, right-click on the file name, then click on option to be able to “copy as pathname”
The qiime tutorial folder is located at “/Volumes/RAVPOWER/ROOK_PC/MAC_Documents/qiime_tutorial”
I copied it to “cd /panfs/roc/groups/5/isrania/onyea005” using the FileZilla interface

Using QIIME2 on MSI
The tutorial I will be using was initially located at “/Volumes/RAVPOWER/ROOK_PC/MSI2021/Dissertation/3_Dissertation Analysis/2_Dissertation Programs & MSI Scripts/2_Fall 2019 asmic oral gut programs/Part 31_QIIME2_Moving Pictures Tutorial.docx”
	•	Copy the QIIME2 tutorial file to your MSI folder
To copy a folder path, right-click on the file name, then click on option to be able to “copy as pathname”
The qiime tutorial folder is located at “/Volumes/RAVPOWER/ROOK_PC/MAC_Documents/qiime2”
I copied it to “cd /panfs/roc/groups/5/isrania/onyea005” using the FileZilla interface
	•	Log in to an interactive node, making sure you have enough time and memory (https://www.msi.umn.edu/content/interactive-queue-use-isub)
isub -n nodes=1:ppn=8 -m 16GB -w 8:00:00	
#I don’t think I need to log into mesabi or Itasca to run this job if I am logged into the interactive node since it logs me into the lab queue (So use mesabi instead)
MSI's LabQi Cluster will be shut down and decommissioned during February Maintenance (February 5, 2020.) Several months ago, MSI released the Mesabi Interactive Queue as a replacement for the LabQi Cluster (ssh lab.)  The Mesabi Interactive Queue can be used in real-time for tasks such as interactive data exploration, creating plots or images, visualizations, or testing sections of code that will be used on other MSI queues.

Here are several examples of how you can request an interactive HPC job on the new Mesabi Interactive Queue.

The basic syntax for submitting an interactive job on Mesabi (I assume that in combination with the new login on mesabi and mangi the syntax occurs after login into mesabi.msi.umn.edu):

qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I

The basic syntax for testing MPI code between 2 nodes:
qsub -l nodes=2:ppn=2,mem=8gb,walltime=12:00:00 -q interactive -W x=nmatchpolicy:exactnode -I

The basic syntax for creating a job with X-forwarding for GUI's and visulization:
qsub -X -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I

The full specifications of the Mesabi Interactive Queue can be found on the MSI Queues webpage. Also, more examples on how to use qsub -I for interactive use can be found on the Interactive queue use with qsub webpage.
https://www.msi.umn.edu/content/interactive-queue-use-srun
** Will need to learn how to use Slurm to schedule jobs (https://www.msi.umn.edu/slurm)
Back to the interactive login
qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I
	•	Load the QIIME module
module avail qiime2
**Under mesabi
module load qiime2/2019.1
	•	Change to the qiime2 tutorial folder
Pwd	
cd /panfs/roc/groups/5/isrania/onyea005/qiime2/

Notes for future me

*** I have moved all of the documents that used to be on my MAC to my external hard drive (MAC_Documents/ in the ROOK PC partition, which is all backed up in the ROOK_MAC and ROOK_BACKUP partitions as well. However, I cannot use ROOK_MAC or ROOK_BACKUP for the analyses since they are on MAC extended format with no writing permissions, but the terminal can write to the ROOK_PC partition as it is in DOS format

** If for some reason the PC volumes doesn’t show up on the MAC, follow these directions to use the command line (terminal) so that you can list the drive and manually mount them

https://discussions.apple.com/thread/7571770

http://osxdaily.com/2013/05/13/mount-unmount-drives-from-the-command-line-in-mac-os-x/

I also have the music folder in there, which can be found by going in the MACINTOSH HD root folder

*** I was not able to find the data folder in the songbird folder (went to http://osxdaily.com/2013/01/17/access-root-directory-mac-os-x/ to find out how to find the root file for QIIME2, then looked up somgbird in mackintosh HD. So it looks like I will have to test directly on the asmic oral qiime2 folder)

Macintosh HD/Users/guillonyeaghala/music


MISSION 16S data analysis

Step 1: Access the files on MSI

Sequencing has finished, and we have released the data to the Israni MSI group under the following directory:

/home/isrania/data_delivery/umgc/2022-q2/220513_M04803_0493_000000000-DGJJH/Israni4_Project_018

When you are under FileZilla, use the right-click and hold the “option” button to copy the ravpower external drive as a pathname (Should be /Volumes/RAVPOWER2/)

I have attached a sequencing summary.

**It looks like the Filezilla update may ask you to reenter the password when transferring files. I unchecked the following options

Select the Transfer Settings tab
	•	Select 'Limit number of simultaneous connections'
	•	Set simultaneous connections to: 1

Step 2: Move the files to the external hard drive, and back to the MSI folder 

/panfs/roc/groups/5/isrania/onyea005

Making sure I can use R from the remote volumes

When running future analyses on MAC, in order to use the external hard drive and save on memory space, here is what you can do

On MAC

Change the root of the path to (/Users/guillaumeonyeaghala/Documents/Dissertation/3_Dissertation Analysis/1_Dissertation Processed Datasets/1_asmic_oral_mapping/)

On external hard drive

Change the root of the path to (/Volumes/ROOK_PC/MAC_Documents/Dissertation/3_Dissertation Analysis/1_Dissertation Processed Datasets/1_asmic_oral_mapping/)

***One last thing to remember, is that the python environments (picrust, macqiime and qiime2) take up space on your mac hard drive (~15GBs) so that is what is eating your memory space even if you can’t find them in documents.

HUMANN 3.5 data analysis of BMT dataset

Step 1: Access the files on MSI

Sequencing has finished, and we have released the data to the Israni MSI group under the following directory:

/home/isrania/data_delivery/umgc/2022-q2/220513_M04803_0493_000000000-DGJJH/Israni4_Project_018

The data provided by Dr. Staley is found at: 

Hi Guillaume,

I just added you to my group and you can find data in 

/panfs/roc/data_release/0/umgc/cmstaley/nextseq/210712_NB551164_0296_AH2C37BGXH/Staley_Project_069/. The sample key is attached.

*This was moved to /Volumes/RAVPOWER2/ROOK_04_PC/Staley_Project_069
When you are under FileZilla, use the right-click and hold the “option” button to copy the ravpower external drive as a pathname (Should be /Volumes/RAVPOWER2/)

I have attached a sequencing summary.

It was moved to /Volumes/RAVPOWER2/ROOK_04_PC/Staley_Project_069/MMF PK Sample Log.xlsx

**It looks like the Filezilla update may ask you to reenter the password when transferring files. I unchecked the following options

Select the Transfer Settings tab
	•	Select 'Limit number of simultaneous connections'
	•	Set simultaneous connections to: 1

Step 2: Move the files to the external hard drive, and back to the MSI folder 

/panfs/roc/groups/5/isrania/onyea005

Making sure I can use R from the remote volumes

When running future analyses on MAC, in order to use the external hard drive and save on memory space, here is what you can do

On MAC

Change the root of the path to (/Users/guillaumeonyeaghala/Documents/Dissertation/3_Dissertation Analysis/1_Dissertation Processed Datasets/1_asmic_oral_mapping/)

On external hard drive

Change the root of the path to (/Volumes/ROOK_PC/MAC_Documents/Dissertation/3_Dissertation Analysis/1_Dissertation Processed Datasets/1_asmic_oral_mapping/)

***One last thing to remember, is that the python environments (picrust, macqiime and qiime2) take up space on your mac hard drive (~15GBs) so that is what is eating your memory space even if you can’t find them in documents.
HUMANN Installation 2022 (OLD)


	•	Connect to VPN for MAC using Cisco AnyConnect Secure Mobility Client
	•	Connect to FileZilla to be able to see the files being moved and created in a dynamic fashion
	•	Enter sftp://login.msi.umn.edu into the Host box, your username into the Username box, your password into the Password box, and 22 into the Port box. Click on Quickconnect. (Tried using sftp://mesabi.msi.umn.edu, username and password since login node changes took place in Jaunary 2021)

	•	Connect to Mesabi (recommended default for MSI to be connected to the supercomputers)
Alternatively, connect to MSI using Terminal on Mac/Linux, located in ApplicationsUtilitiesTerminal.app. 
*NOTE: Like the login node Mesabi and Itasca have specific nomenclatures in the prompt. When connected to Mesabi your prompt will contain ln followed by some number when connected to Itasca your prompt will contain node followed by some numbers. This is a quick short cut to remind you which system you are connected to at any time.
**You need to be connected to Cisco Anyconnect via VPN in order for this command to work and it will ask you to login via DUO

ssh yourusername@login.msi.umn.edu (use ssh yourusername@mangi.msi.umn.edu or ssh yourusername@mesabi.msi.umn.edu respectively)
ssh onyea005@mesabi.msi.umn.edu
	•	Check the folder you are in
pwd
* my home folder is when I first login is “/home/prizm001/onyea005”
* /home/isrania/onyea005
*Change the folder to the israni MSI group
cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
cd /panfs/roc/groups/5/isrania/onyea005
cd /panfs/roc/groups/5/isrania/onyea005/Israni4_Project_016




HUMANN metagenomics & metatranscriptomics pipeline

https://huttenhower.sph.harvard.edu/humann/

Installing HUMANN

Requirements
Software
MetaPhlAn 3.0
Bowtie2 (version >= 2.2) (automatically installed)
Diamond (version >= 0.9.24) (automatically installed)
Python (version >= 3.7)
MinPath (automatically installed)
Xipe (optional / included)
Rapsearch2 (version >= 2.21) (only required if using rapsearch2 for translated search)
Usearch (version >= 7.0) (only required if using usearch for translated search)
SAMtools (only required if bam input files are provided)
Biom-format (only required if input or output files are in biom format)
Please install the required software in a location in your $PATH or provide the location with an optional argument to HUMAnN 3.0. For example, the location of the Bowtie2 install ($BOWTIE2_DIR) can be provided with "--bowtie2 $BOWTIE2_DIR".

Some of the required dependencies are installed automatically when installing HUMAnN 3.0 with pip. If these dependencies do not appear to be installed after installing HUMAnN 3.0 with pip, it might be that your environment is setup to use wheels instead of installing from source. HUMAnN 3.0 must be installed from source for it to also be able to install dependencies. To force pip to install HUMAnN 3.0 from source add one of the following options to your install command, "--no-use-wheel" or "--no-binary :all:".

If you always run with input files of type 2, 3, and 4 (for information on input file types, see section Workflow by input file type), MetaPhlAn2, Bowtie2, and Diamond are not required. Also if you always run with one or more bypass options (for information on bypass options, see section Workflow by bypass mode), the software required for the steps you bypass does not need to be installed.

Other
Memory (>= 16 Gb)
Disk space (>= 15 Gb [to accommodate comprehensive sequence databases])
Operating system (Linux or Mac)
If always running with files of type 2, 3, and 4 (for information on file types, see section Workflow by input file type), less disk space is required.


Working on the installation (The first part of the tutorial did not work, so I had to install metaphlan within the mpa conda environment, and install humann within mpa)

Before beginning this tutorial, create a new directory and change to that directory.
pwd
Move to the folder where I will install and use HUMANN
cd /home/isrania/onyea005
ls -lh
mkdir HUMANN
cd /home/isrania/onyea005/HUMANN
How To Access Software

HPC and Interactive HPC Systems
To access software on Linux systems, use the module command to load the software into your environment. You can find the module name to load on the MSI software page for a package. For example, if you want to use the 2014b version of MATLAB, you would type in your job script or at the command line:

% module load matlab/R2014b
% matlab

To find out what module versions are available, consult the software page or use the vail command to search by name. For example to find out what versions of Python are available:

% module avail python
	•	Load the conda module
module avail conda
**Under mesabi
module load conda/python3

I will opt for the conda installation since it also includes METAPLHAN

https://github.com/biobakery/biobakery/wiki/humann3

There were conflicts with the already installed conda environment I am using, so I will create a new environment.
https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html

conda activate and conda deactivate only work on conda 4.6 and later versions. For conda versions prior to 4.6, run:
	•	Windows: activate or deactivate
	•	Linux and macOS: source activate or source deactivate
1. Install conda
Bioconda requires the conda package manager to be installed. If you have an Anaconda Python installation, you already have it. Otherwise, the best way to install it is with the Miniconda package. The Python 3 version is recommended.
On MacOS, run:
*You can skip this step on MSI since you are not operating on a MaC interface

curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.sh
sh Miniconda3-latest-MacOSX-x86_64.sh
2. Set up channels
After installing conda you will need to add the bioconda channel as well as the other channels bioconda depends on. It is important to add them in this order so that the priority is set correctly (that is, conda-forge is highest priority).
The conda-forge channel contains many general-purpose packages not already found in the defaults channel.
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge

https://github.com/biobakery/MetaPhlAn

Installation
The best way to install MetaPhlAn is through conda via the Bioconda channel. If you have not configured you Anaconda installation in order to fetch packages from Bioconda, please follow these steps in order to setup the channels.

https://github.com/biobakery/biobakery/wiki/humann3
http://huttenhower.sph.harvard.edu/metaphlan
https://github.com/biobakery/MetaPhlAn
https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-3.0

Installation

The best way to install MetaPhlAn is through conda via the Bioconda channel. If you have not configured your Anaconda installation to fetch packages from Bioconda, please follow these steps to set up the channels.

You can install MetaPhlAn by running the command below:

It is recommended to create an isolated conda environment and install MetaPhlAn into it.

***Make sure to set uo the proper conda channels (as indicated above) before running the conda create command or the mpa environment will not be installed
cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
conda create --name mpa -c bioconda python=3.7 metaphlan
This allows having the correct version of all the dependencies isolated from the system's python installation.

Before using MetaPhlAn, you should activate the mpa environment:

conda activate mpa
CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'.
To initialize your shell, run

    $ conda init <SHELL_NAME>

*I had to run conda init bash, then close the terminal window
conda init bash 

* 
After the update, I had to use the full name to active conda environments (https://stackoverflow.com/questions/58369030/could-not-find-conda-environment)
conda info --envs
source activate /Users/guillaumeonyeaghala/miniconda2/envs/mpa

/home/isrania/onyea005/.conda/envs/mpa
conda activate /home/isrania/onyea005/.conda/envs/mpa

1.2 HUMANN Installation within the metaphlan conda environment
 
NOTE: If you are using a bioBakery machine image (e.g. in Google Cloud) you do not need to install HUMAnN because the software and its dependencies are already available.

1.2.1 Install HUMAnN
The easiest way to install HUMAnN is with pip, Conda, or Docker. For other options, please refer to the HUMAnN User Manual.

***To install with pip (*This step did not work, will use conda to install humann within the mpa environment with metaphlan below)

Pwd *Skip
pip install human *Skip

Back to the HUMANN installation

You can also install via Conda, which will additionally install MetaPhlAn and utility dependencies. Before doing so, make sure to configure your channels so that biobakery is at the top of the list (see https://bioconda.github.io/user/install.html#set-up-channels):

conda config --add channels biobakery 
biobakery should already be at the top since you set it up to create the MPA environment
conda install -c biobakery humann

By default, a pip (or other) HUMAnN install does not include the full-scale nucleotide and protein sequence databases needed for real-world metagenome or metatranscriptome profiling, as these are quite large (several GBs). The humann_databases script can be used to fetch these from the web and connect them to your HUMAnN installation. For tutorial purposes, we'll be using (small) demo databases, which are also fetchable with humann_databases (if not already available).

pwd
humann_databases --download chocophlan DEMO humann_dbs
humann_databases --download uniref DEMO_diamond humann_dbs

The commands above download the demo-scale databases to a folder named humann_dbs in the current working directory. Please note that you can download these (and full-scale) databases to any location on your machine: HUMAnN will track where you have placed the databases and update its configuration file.

After installation from pip or another source, you may optionally test your local HUMAnN environment (this takes ~1 minute):

humann_test
Activate the conda environment
Now that you have an environment for HUMANN, activate it using the environment’s name:
After the update, I had to use the full name to active conda environments (https://stackoverflow.com/questions/58369030/could-not-find-conda-environment)
conda info --envs
source activate /Users/guillaumeonyeaghala/miniconda2/envs/mpa

/home/isrania/onyea005/.conda/envs/mpa
conda activate /home/isrania/onyea005/.conda/envs/mpa
To deactivate an environment, run source deactivate.
The step below to download the databases  did not work, so I checked the specific error online “subprocess.CalledProcessError: Command '['bowtie2-build', '--usage']' returned non-zero exit status 250.” As seen below, I had to first load the MPA conda environment, uninstall bowtie, reinstall bowtie and finally install metaphlan for things to play nice.

https://forum.biobakery.org/t/cant-get-metaphlan-to-complete-within-humann-3/1834
https://forum.biobakery.org/t/cant-get-metaphlan-to-complete-within-humann-3/1834/4
https://forum.biobakery.org/t/humann-3-cannot-call-bowtie2-version/576/17

If you installed bowtie2 with conda you could try:
conda remove bowtie2
conda install bowtie2
bowtie2-build --usage
removing bowtie also removed metaphlan. You can install MetaPhlAn by running

conda install -c bioconda metaphlan
conda install -c biobakery humann
MetaPhlAn needs the clade markers and the database to be downloaded locally. To obtain them:

pwd
cd /home/isrania/onyea005/HUMANN
metaphlan --install 
If you have installed MetaPhlAn using Anaconda, it is advised to install the database in a folder outside the Conda environment. To do this, run

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
metaphlan --install --bowtie2db </Volumes/RAVPOWER/ROOK_PC/HUMANN/metaphlan_db>
metaphlan --install --bowtie2db /Volumes/RAVPOWER/ROOK_PC/HUMANN/metaphlan_db

cd /home/isrania/onyea005/HUMANN
metaphlan --install --bowtie2db /home/isrania/onyea005/HUMANN/metaphlan_db
If you install the database in a different location, remember to run MetaPhlAn using --bowtie2db <database folder>!

By default, the latest MetaPhlAn database is downloaded and built. You can download a specific version with the --index parameter

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
metaphlan --install --index mpa_v30_CHOCOPhlAn_201901 --bowtie2db </Volumes/RAVPOWER/ROOK_PC/HUMANN/metaphlan_db>

metaphlan --install --index mpa_v30_CHOCOPhlAn_201901 --bowtie2db /Volumes/RAVPOWER/ROOK_PC/HUMANN/metaphlan_db2

cd /home/isrania/onyea005/HUMANN
metaphlan --install --index mpa_v30_CHOCOPhlAn_201901 --bowtie2db /home/isrania/onyea005/HUMANN/metaphlan_db2
When --index is specified, MetaPhlAn skips the check for the latest database version and run the analysis using the database version provided by --index located in --bowtie2db. [the link was found at https://drive.google.com/drive/folders/1_HaY16mT7mZ_Z8JtesH8zCfG9ikWcLXG]

https://github.com/biobakery/humann

5. Download the databases

Downloading the databases is a required step if your input is a filtered shotgun sequencing metagenome file (fastq, fastq.gz, fasta, or fasta.gz format). If your input files will always be mapping results files (sam, bam or blastm8 format) or gene tables (tsv or biom format), you do not need to download the ChocoPhlAn and translated search databases.

Download the ChocoPhlAn database

Download the ChocoPhlAn database providing $INSTALL_LOCATION as the location to install the database (approximate size = 16.4 GB).

humann_databases --download chocophlan full $INSTALL_LOCATION

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_databases --download chocophlan full $cd /Volumes/RAVPOWER/ROOK_PC/HUMANN/Databases

cd /Volumes/RAVPOWER2/ROOK_PC/Israni4_Project_016
humann_databases --download chocophlan full $cd /Volumes/RAVPOWER2/ROOK_PC/HUMANN/Databases

cd /panfs/roc/groups/5/isrania/onyea005/HUMANN/Databases
humann_databases --download chocophlan full $cd /panfs/roc/groups/5/isrania/onyea005/HUMANN/Databases

NOTE: The humann config file will be updated to point to this location for the default chocophlan database. If you move this database, please use the "humann_config" command to update the default location of this database. Alternatively you can always provide the location of the chocophlan database you would like to use with the "--nucleotide-database " option to humann.

Download a translated search database

While there is only one ChocoPhlAn database, users have a choice of translated search databases which offer trade-offs between resolution, coverage, and performance. For help selecting a translated search database, see the following guides:

Selecting a level of gene family resolution
Selecting a scope for translated search

Download a translated search database providing $INSTALL_LOCATION as the location to install the database:

To download the full UniRef90 database (20.7GB, recommended):

humann_databases --download uniref uniref90_diamond $INSTALL_LOCATION

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_databases --download uniref uniref90_diamond $cd /Volumes/RAVPOWER/ROOK_PC/HUMANN/Databases

cd /Volumes/RAVPOWER2/ROOK_PC/Israni4_Project_016
humann_databases --download uniref uniref90_diamond $cd /Volumes/RAVPOWER2/ROOK_PC/HUMANN/Databases
To download the EC-filtered UniRef90 database (0.9GB):

humann_databases --download uniref uniref90_ec_filtered_diamond $INSTALL_LOCATION

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_databases --download uniref uniref90_ec_filtered_diamond $cd /Volumes/RAVPOWER/ROOK_PC/HUMANN/Databases

cd /Volumes/RAVPOWER2/ROOK_PC/Israni4_Project_016
humann_databases --download uniref uniref90_ec_filtered_diamond $cd /Volumes/RAVPOWER2/ROOK_PC/HUMANN/Databases

cd /panfs/roc/groups/5/isrania/onyea005/HUMANN/Databases
humann_databases --download uniref uniref90_ec_filtered_diamond $cd /panfs/roc/groups/5/isrania/onyea005/HUMANN/Databases
To download the full UniRef50 database (6.9GB):

humann_databases --download uniref uniref50_diamond $INSTALL_LOCATION

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_databases --download uniref uniref50_diamond $cd /Volumes/RAVPOWER/ROOK_PC/HUMANN/Databases
To download the EC-filtered UniRef50 database (0.3GB):

humann_databases --download uniref uniref50_ec_filtered_diamond $INSTALL_LOCATION

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_databases --download uniref uniref50_ec_filtered_diamond $cd /Volumes/RAVPOWER/ROOK_PC/HUMANN/Databases

NOTE: The humann config file will be updated to point to this location for the default uniref database. If you move this database, please use the humann_config command to update the default location of this database. Alternatively you can always provide the location of the uniref database you would like to use with the --protein-database <uniref> option to humann.

NOTE: By default HUMAnN 3.0 runs DIAMOND for translated alignment. If you would like to use RAPSearch2 for translated alignment, first download the RAPSearch2 formatted database by running this command with the rapsearch2 formatted database selected. It is suggested that you install both databases in the same folder so this folder can be the default uniref database location. This will allow you to switch between alignment software without having to specify a different location for the database.
Installation Update
If you have already installed HUMAnN 3.0, using the Initial Installation steps, and would like to upgrade your installed version to the latest version, please follow these steps.

Download HUMAnN 3.0
Install HUMAnN 3.0

Since you have already downloaded the databases in the initial installation, you do not need to download the databases again unless there are new versions available. However, you will want to update your latest HUMAnN 3.0 install to point to the databases you have downloaded as by default the new install configuration will point to the demo databases.

To update your HUMAnN 3.0 configuration file to include the locations of your downloaded databases, please use the following steps.

Update the location of the ChocoPhlAn database (replacing $DIR with the full path to the directory containing the ChocoPhlAn database)


cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_config --update database_folders nucleotide $DIR
humann_config --update database_folders nucleotide /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases
humann_config --update database_folders nucleotide /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases/chocophlan

Update the location of the UniRef database (replacing $DIR with the full path to the directory containing the UniRef database)


cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_config --update database_folders protein $DIR

humann_config --update database_folders protein /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases/uniref

Please note, after a new installation, all of the settings in the configuration file, like the database folders, will be reset to the defaults. If you have any additional settings that differ from the defaults, please update them at this time.

For more information on the HUMAnN 3.0 configuration file, please see the Configuration section.

HUMANN Installation (AUGUST 2023, FAILED)


	•	Connect to VPN for MAC using Cisco AnyConnect Secure Mobility Client
	•	Connect to FileZilla to be able to see the files being moved and created in a dynamic fashion
	•	Enter sftp://login.msi.umn.edu into the Host box, your username into the Username box, your password into the Password box, and 22 into the Port box. Click on Quickconnect. (Tried using sftp://mesabi.msi.umn.edu, username and password since login node changes took place in Jaunary 2021)

	•	Connect to Mesabi (recommended default for MSI to be connected to the supercomputers)
Alternatively, connect to MSI using Terminal on Mac/Linux, located in ApplicationsUtilitiesTerminal.app. 
*NOTE: Like the login node Mesabi and Itasca have specific nomenclatures in the prompt. When connected to Mesabi your prompt will contain ln followed by some number when connected to Itasca your prompt will contain node followed by some numbers. This is a quick short cut to remind you which system you are connected to at any time.
**You need to be connected to Cisco Anyconnect via VPN in order for this command to work and it will ask you to login via DUO




	
	
Step 1: Check the installation instructions 

https://forum.biobakery.org/t/announcing-humann-3-5/3995

Step 2: Install Metaphlan 4.0

https://forum.biobakery.org/t/metaphlan4-and-the-new-version-humann/3985/4

https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4

Step 3: Install a new environment of Humann 3.0 (Which upgrades it to 3.5, 3.6 or 3.7)

https://forum.biobakery.org/t/announcing-humann-3-6-critical-update/4155

https://huttenhower.sph.harvard.edu/humann

Step 4: Test HUMANN

https://github.com/biobakery/biobakery/wiki/humann3











Step 1: Check the installation instructions 

https://forum.biobakery.org/t/announcing-humann-3-5/3995

Step 2: Install Metaphlan 4.0

https://forum.biobakery.org/t/metaphlan4-and-the-new-version-humann/3985/4

https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4

MetaPhlAn 4

Home

MetaPhlAn documentation
•	MetaPhlAn 4
•	MetaPhlAn 3.1
•	MetaPhlAn 3.0
•	MetaPhlAn2

StrainPhlAn documentation
•	StrainPhlAn 4
•	StrainPhlAn 3
•	StrainPhlAn
•	Strain Sharing Inference

Workshop on Genomics 2023
•	MetaPhlAn workshop
•	HUMAnN workshop


MetaPhlAn is a computational tool for profiling the composition of microbial communities (Bacteria, Archaea and Eukaryotes) from metagenomic shotgun sequencing data (i.e. not 16S) with species-level. With StrainPhlAn, it is possible to perform accurate strain-level microbial profiling.

MetaPhlAn 4 relies on ~5.1M unique clade-specific marker genes (the latest marker information file can be found here) identified from ~1M microbial genomes (~236,600 references and 771,500 metagenomic assembled genomes) spanning 26,970 species-level genome bins (SGBs, http://segatalab.cibio.unitn.it/data/Pasolli_et_al.html), 4,992 of them taxonomically unidentified at the species level (the full list of the species included in the latest database can be found here), allowing:

•	unambiguous taxonomic assignments;
•	an accurate estimation of organismal relative abundance;
•	SGB-level resolution for bacteria, archaea and eukaryotes;
•	strain identification and tracking
•	orders of magnitude speedups compared to existing methods.
•	metagenomic strain-level population genomics



HUMANN Analysis 082023 demo run (FAILED)

	•	Connect to VPN for MAC using Cisco AnyConnect Secure Mobility Client
	•	Connect to FileZilla to be able to see the files being moved and created in a dynamic fashion
	•	Enter sftp://login.msi.umn.edu into the Host box, your username into the Username box, your password into the Password box, and 22 into the Port box. Click on Quickconnect. (Tried using sftp://mesabi.msi.umn.edu, username and password since login node changes took place in Jaunary 2021)

	•	Connect to Mesabi (recommended default for MSI to be connected to the supercomputers)
Alternatively, connect to MSI using Terminal on Mac/Linux, located in ApplicationsUtilitiesTerminal.app. 
*NOTE: Like the login node Mesabi and Itasca have specific nomenclatures in the prompt. When connected to Mesabi your prompt will contain ln followed by some number when connected to Itasca your prompt will contain node followed by some numbers. This is a quick short cut to remind you which system you are connected to at any time.
**You need to be connected to Cisco Anyconnect via VPN in order for this command to work and it will ask you to login via DUO




ssh yourusername@login.msi.umn.edu (use ssh yourusername@mangi.msi.umn.edu or ssh yourusername@mesabi.msi.umn.edu respectively)
ssh onyea005@mesabi.msi.umn.edu
	•	Check the folder you are in
pwd
* my home folder is when I first login is “/home/prizm001/onyea005”
* /home/isrania/onyea005
*Change the folder to the israni MSI group
cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
cd /panfs/roc/groups/5/isrania/onyea005
cd /panfs/roc/groups/5/isrania/onyea005/Israni4_Project_016

Log in to an interactive node, making sure you have enough time and memory (https://www.msi.umn.edu/content/interactive-queue-use-isub)

isub -n nodes=1:ppn=8 -m 16GB -w 8:00:00	
#I don’t think I need to log into mesabi or Itasca to run this job if I am logged into the interactive node since it logs me into the lab queue (So use mesabi instead)
MSI's LabQi Cluster will be shut down and decommissioned during February Maintenance (February 5, 2020.) Several months ago, MSI released the Mesabi Interactive Queue as a replacement for the LabQi Cluster (ssh lab.)  The Mesabi Interactive Queue can be used in real-time for tasks such as interactive data exploration, creating plots or images, visualizations, or testing sections of code that will be used on other MSI queues.

https://www.msi.umn.edu/content/interactive-hpc

What is Interactive HPC?

MSI’s interactive HPC resources are composed of some of the same systems used to support our batch scheduled HPC activities. The main difference between the two services is the way in which users interact with the MSI resource. Interactive HPC systems allow real-time user inputs in order to facilitate code development, real-time data exploration, and visualizations. Interactive HPC systems are used when data are too large to download to a desktop or laptop, software is difficult or impossible to install on a personal machine, or specialized hardware resources (e.g., GPUs) are needed to visualize large datasets. User input might come through a command line interface (i.e., system shell) while debugging a piece of software or through mouse clicks while using a program with a graphical interface.

What can I do with Interactive HPC?

Unlike MSI’s other computing options, the Interactive HPC resources are designed only to be used in real time for tasks such as interactive data exploration, creating plots or images, visualizations, or testing sections of code that will be used on other MSI systems.

Common use cases include:
	•	Testing a compiled program at the command line before submitting it as a job
	•	Developing code or exploring data in an interactive environment like MATLAB, IPython, or Rstudio
	•	Visualizing data or building simulations in applications like Schrodinger, COMSOL, or Avizo

What types of Interactive HPC are available?
	•	srun:  Submit an interactive job on the current HPC system to provide a command line or graphical interface to interactively run software. 
	•	NICE: Connects a user to a graphical environment, which may have specialized GPUs for visualizing large datasets.
	•	Total Slots: 32 slots for accelerated remote desktops
	•	Total Memory:  Up to 16 GB/session
	•	Graphical Accelerators: Nvidia GK107GL
	•	CITRIX: Citrix for Windows Environments: Connects a user to a graphical Windows environment, where they can access specialized Windows software for modeling and data analysis.
	•	NX NoMachine: Connects a user to a graphical environment, which may be used as a lightweight remote desktop for connecting to other MSI resources.
	•	Jupyter Notebooks: A web portal for notebook-based computing in the browser, which can be used for reproducible and shareable data analysis, visualization, and scripted control of larger tasks. Currently supports Python 2, Python 3, and R. 
Here are several examples of how you can request an interactive HPC job on the new Mesabi Interactive Queue.

The basic syntax for submitting an interactive job on Mesabi (I assume that in combination with the new login on mesabi and mangi the syntax occurs after login into mesabi.msi.umn.edu):

qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I

The basic syntax for testing MPI code between 2 nodes:
qsub -l nodes=2:ppn=2,mem=8gb,walltime=12:00:00 -q interactive -W x=nmatchpolicy:exactnode -I

The basic syntax for creating a job with X-forwarding for GUI's and visulization:
qsub -X -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I

Back to the interactive login
qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I

The full specifications of the Mesabi Interactive Queue can be found on the MSI Queues webpage. Also, more examples on how to use qsub -I for interactive use can be found on the Interactive queue use with qsub webpage.
https://www.msi.umn.edu/content/interactive-queue-use-srun
** Will need to learn how to use Slurm to schedule jobs (https://www.msi.umn.edu/slurm)
Interactive queue use with srun
It is possible to start an interactive job on any of MSI’s clusters that support job submission. Interactive jobs can be useful for tasks including data exploration, development, or (with X11 forwarding) visualization activities.
The basic syntax for submitting an interactive job is illustrated using the following example for Mesabi:
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=1gb -t 1:00:00 -p interactive --pty bash
If you are submitting an interactive job from a session with X11 forwarding and would like to continue using X11 forwarding inside your job, you would add the additional flag ‘-x11’, as follows:
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=1gb -t 1:00:00 -p interactive --x11 --pty bash
The basic syntax for requesting two nodes, for instance to test MPI code between two nodes:
srun -N 2 --ntasks-per-node=1  --mem-per-cpu=1gb -t 1:00:00 -p interactive --pty bash
The syntax for requesting an interactive gpu node with a k40 GPU is:
srun -n 12 -t 1:00:00 -p interactive-gpu --gres=gpu:k40:1 --pty bash
You may also submit an interactive job using an interactive submission script. For instance, to submit an interactive job to test an MPI code (in our case named ‘interactive.sh’) with contents:
#SBATCH -N 2
#SBATCH --ntasks-per-node=1
#SBATCH --mem-per-cpu=1gb
#SBATCH -t 1:00:00
#SBATCH -p interactive
./my_application
You would then submit this job using:
sbatch interactive.sh
Interactive jobs are subject to the same rules and limitations as batch jobs. Interactive jobs have access to only those partitions that are available on the cluster they are submitted from. Jobs that request more resources may wait in the queue longer, so be sure that you are requesting only those resources that you need.
 An interactive job supports all of the usual Slurm commands. These commands are included as flags when requesting an interactive job. 
 Example interactive srun Session
 username@mydesktop$ ssh -Y msiusername@mesabi.msi.umn.edu
Password:
Last login: Tue May 10 15:16:06 2018 from mydesktop.mydept.umn.edu
-------------------------------------------------------------------------------
            University of Minnesota Supercomputing Institute
                                Mesabi
                        HP Haswell Linux Cluster
-------------------------------------------------------------------------------
[... output truncated ...]
---------------
username@ln0006 [~] % srun -N 2 --ntasks-per-node=1  --mem-per-cpu=1gb --x11 -t 1:00:00 --pty bash
username@cn0001 [~] % 
username@cn0001 [~] % echo $SLURM_JOB_NODELIST
cn[0001-0002]
username@cn0001 [~] %
***Note that this interactive job is using two nodes (-N 2). At this point you are currently in your home directory, and can load modules and launch software, including GUI software.

Back to the interactive login using srun (might take a few mins to get the access)

*Old syntax
qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I
*New syntax
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=16gb -t 4:00:00 -p interactive --pty bash
Now you can look at the contents of the directory using the ls command:

ls -lh
You can always check the location of your current directory (aka folder) using the pwd command.  If you type:

pwd
Login into an interactive node to process the data
*I will try to add a CPU per task option to the interactive run and to the sbatch file
https://www.msi.umn.edu/support/faq/how-can-i-use-gnu-parallel-run-lot-commands-parallel
https://www.msi.umn.edu/content/interactive-queue-use-srun
Interactive queue use with srun
It is possible to start an interactive job on any of MSI’s clusters that support job submission. Interactive jobs can be useful for tasks including data exploration, development, or (with X11 forwarding) visualization activities.
 The basic syntax for submitting an interactive job is illustrated using the following example for Mesabi:
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=1gb -t 1:00:00 -p interactive --pty bash
If you are submitting an interactive job from a session with X11 forwarding and would like to continue using X11 forwarding inside your job, you would add the additional flag ‘-x11’, as follows:
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=1gb -t 1:00:00 -p interactive --x11 --pty bash
The basic syntax for requesting two nodes, for instance to test MPI code between two nodes:
srun -N 2 --ntasks-per-node=1  --mem-per-cpu=1gb -t 1:00:00 -p interactive --pty bash
The syntax for requesting an interactive gpu node with a k40 GPU is:
srun -n 12 -t 1:00:00 -p interactive-gpu --gres=gpu:k40:1 --pty bash
https://stackoverflow.com/questions/51139711/hpc-cluster-select-the-number-of-cpus-and-threads-in-slurm-sbatch
The terminology used in the sbatch man page might be a bit confusing. Thus, I want to be sure I am getting the options set right. Suppose I have a task to run on a single node with N threads. Am I correct to assume that I would use --nodes=1 and --ntasks=N?
I am used to thinking about using, for example, pthreads to create N threads within a single process. Is the result of that what they refer to as "cores" or "cpus per task"? CPUs and threads are not the same things in my mind.

Depending on the parallelism you are using: distributed or shared memory
--ntasks=# : Number of "tasks" (use with distributed parallelism).
--ntasks-per-node=# : Number of "tasks" per node (use with distributed parallelism).
--cpus-per-task=# : Number of CPUs allocated to each task (use with shared memory parallelism).

From this question: if every node has 24 cores, is there any difference between these commands?
sbatch --ntasks 24 [...]
sbatch --ntasks 1 --cpus-per-task 24 [...]
Answer: (by Matthew Mjelde)
Yes there is a difference between those two submissions. You are correct that usually ntasks is for mpi and cpus-per-task is for multithreading, but let’s look at your commands:
For your first example, the sbatch --ntasks 24 […] will allocate a job with 24 tasks. These tasks in this case are only 1 CPUs, but may be split across multiple nodes. So you get a total of 24 CPUs across multiple nodes.
For your second example, the sbatch --ntasks 1 --cpus-per-task 24 [...] will allocate a job with 1 task and 24 CPUs for that task. Thus you will get a total of 24 CPUs on a single node.
In other words, a task cannot be split across multiple nodes. Therefore, using --cpus-per-task will ensure it gets allocated to the same node, while using --ntasks can and may allocate it to multiple nodes.

Another good Q&A from CÉCI's support website: Suppose you need 16 cores. Here are some use cases:
	•	you use mpi and do not care about where those cores are distributed: --ntasks=16
	•	you want to launch 16 independent processes (no communication): --ntasks=16
	•	you want those cores to spread across distinct nodes: --ntasks=16 and --ntasks-per-node=1 or --ntasks=16 and --nodes=16
	•	you want those cores to spread across distinct nodes and no interference from other jobs: --ntasks=16 --nodes=16 --exclusive
	•	you want 16 processes to spread across 8 nodes to have two processes per node: --ntasks=16 --ntasks-per-node=2
	•	you want 16 processes to stay on the same node: --ntasks=16 --ntasks-per-node=16
	•	you want one process that can use 16 cores for multithreading: --ntasks=1 --cpus-per-task=16
	•	you want 4 processes that can use 4 cores each for multithreading: --ntasks=4 --cpus-per-task=4

https://stackoverflow.com/questions/49466020/specify-number-of-cpus-for-a-job-on-slurm
https://researchcomputing.princeton.edu/support/knowledge-base/scaling-analysis
https://docs-research-it.berkeley.edu/services/high-performance-computing/user-guide/running-your-jobs/scheduler-examples/
Back to the interactive login using srun (might take a few mins to get the access)

https://www.msi.umn.edu/content/interactive-queue-use-srun
*Old syntax
qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I
*New syntax
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=16gb -t 4:00:00 -p interactive --pty bash

*Testing out adding multiple CPUs to run the code faster (https://www.msi.umn.edu/support/faq)
It seems like nodes and CPU is different so the n-task per node is what determine thee number of CPUs we can use?
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=32gb -t 4:00:00 -p interactive --pty bash
*I will use this code to ask for multiple CPUs for the analysis)https://stackoverflow.com/questions/51139711/hpc-cluster-select-the-number-of-cpus-and-threads-in-slurm-sbatch
--ntasks=4 --cpus-per-task=4

srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=32gb -t 4:00:00 -p interactive --pty bash

*Per the SLURM PDF guide (See slide 10) the interactive queue has a maximum of 4 cores/CPU that can be requested through srun. I will request 8 when submitting a job via SLURM
srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=32gb -t 4:00:00 -p interactive --pty bash

srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=8gb -t 4:00:00 -p interactive --pty bash

*Setting the srun to 16GB since metaphlan 4 mentioned that it needs 15GB to run
srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=16gb -t 4:00:00 -p interactive --pty bash
Change the folder to the location with the metatranscriptomics data
**I created a new folder called HUMANN082023 for this new analysis via Filezilla

/home/isrania/onyea005/Israni4_Project_016
cd /home/isrania/onyea005/Israni4_Project_016
cd /home/isrania/onyea005/Israni4_016_test

pwd
cd /home/isrania/onyea005/HUMANN082023

Back to https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4

Pre-requisites

MetaPhlAn requires python 3 or newer with numpy, and Biopython libraries installed. Python libraries are automatically installed by pip. MetaPhlAn relies on BowTie2 (version 2.3 or higher) to map reads against marker genes. Check that bowtie2 is present in the system path with execute and read permissions.

If MetaPhlAn is installed using conda, no pre-requisites are needed.

MetaPhlAn is integrated with advanced heatmap plotting with hclust2 and cladogram visualization with GraPhlAn. If you use such visualization tools please refer to their prerequisites.

The best way to install MetaPhlAn is through conda via the Bioconda channel. If you have not configured your Anaconda installation to fetch packages from Bioconda, please follow these steps to set up the channels.

You can install MetaPhlAn by running
$ conda install -c bioconda metaphlan

It is recommended to create an isolated conda environment and install MetaPhlAn into it.
$ conda create --name mpa -c bioconda python=3.7 metaphlan

If during the installation you encounter an incompatibility error with the glibc package, we suggest you to add the conda-forge channel to conda or run one of the following commands.
$ conda install -c conda-forge -c bioconda metaphlan
$ conda create --name mpa -c conda-forge -c bioconda python=3.7 metaphlan

This allows having the correct version of all the dependencies isolated from the system's python installation.

Before using MetaPhlAn, you should activate the mpa environment:
$ conda activate mpa
	•	Load the conda module
module avail conda
**Under mesabi
module load conda/python3
Activate the conda environment
Now that you have an environment for HUMANN, activate it using the environment’s name:
After the update, I had to use the full name to active conda environments (https://stackoverflow.com/questions/58369030/could-not-find-conda-environment)
conda info --envs
	•	Create a new conda environment for metaphlan 4
conda create --name mpa -c bioconda python=3.7 metaphlan
pwd
conda create --name mpa082023 -c conda-forge -c bioconda python=3.7 metaphlan

**Skip the part below for now:

MetaPhlAn is also available in PyPi
$ pip install metaphlan

Alternatively, you can manually download from GitHub or clone the repository using the following command
$ git clone https://github.com/biobakery/MetaPhlAn.git

and install MetaPhlAn by running
$ pip install .

If you choose this way, you'll need to install manually some dependencies!
MetaPhlAn needs the clade markers and the database to be downloaded locally. To obtain them:
$ metaphlan --install 

Important! The MetaPhlAn 4 database has been substantially increased in comparison with the previous 3.1 version. Thus, for running MetaPhlAn 4, a minimum of 15GB or memory is needed.

If you have installed MetaPhlAn using Anaconda, it is advised to install the database in a folder outside the Conda environment. To do this, run
$ metaphlan --install --bowtie2db <database folder>

If you install the database in a different location, remember to run MetaPhlAn using --bowtie2db <database folder>!

By default, the latest MetaPhlAn database is downloaded and built. You can download a specific version with the --index parameter

$ metaphlan --install --index mpa_vJan21_CHOCOPhlAnSGB_202103 --bowtie2db <database folder>

When --index is specified, MetaPhlAn skips the check for the latest database version and run the analysis using the database version provided by --index located in --bowtie2db.

This option is recommended when MetaPhlAn is run on HPC clusters or containerized
If you have issues in downloading the database, you can get it from:
	•	Segatalab FTP

Just download the .tar, .md5, and the mpa_latest files and place them in the metaphlan_databases folder.

Basic Usage

Important! The MetaPhlAn 4 database has been substantially increased in comparison with the previous 3.1 version. Thus, for running MetaPhlAn 4, a minimum of 15GB or memory is needed.
$ metaphlan metagenome.fastq --input_type fastq -o profiled_metagenome.txt

It is highly recommended to save the intermediate BowTie2 output for re-running MetaPhlAn extremely quickly (--bowtie2out), and use multiple CPUs (--nproc) if available:
$ metaphlan metagenome.fastq --bowtie2out metagenome.bowtie2.bz2 --nproc 5 --input_type fastq -o profiled_metagenome.txt

If you already mapped your metagenome against the marker DB (using a previous MetaPhlAn run), you can obtain the results in few seconds by using the previously saved --bowtie2out file and specifying the input (--input_type bowtie2out):
$ metaphlan metagenome.bowtie2.bz2 --nproc 5 --input_type bowtie2out -o profiled_metagenome.txt

bowtie2out files generated with MetaPhlAn versions below 3.0 are not compatible. Starting from MetaPhlAn 3, the BowTie2 output now includes the size of the profiled metagenome.
You can also provide an externally BowTie2-mapped SAM if you specify this format with --input_type. Two steps here: first map your metagenome with BowTie2 and then feed MetaPhlAn with the obtained SAM:
$ bowtie2 --sam-no-hd --sam-no-sq --no-unal --very-sensitive -S metagenome.sam -x metaphlan_databases/mpa_vJan21_CHOCOPhlAnSGB_202103  -U metagenome.fastq
$ metaphlan metagenome.sam --input_type sam -o profiled_metagenome.txt

MetaPhlAn can also natively handle paired-end metagenomes (but does not use the paired-end information), and, more generally, metagenomes stored in multiple files (but you need to specify the --bowtie2out parameter):
$ metaphlan metagenome_1.fastq,metagenome_2.fastq --bowtie2out metagenome.bowtie2.bz2 --nproc 5 --input_type fastq -o profiled_metagenome.txt

Starting from version 3, MetaPhlAn can estimate the unclassified fraction of the metagenome. The relative abundance profile is scaled according to the percentage of reads mapping to a clade in the database.
$ metaphlan metagenome.fastq --bowtie2out metagenome.bowtie2.bz2 --nproc 5 --input_type fastq --unclassified_estimation -o profiled_metagenome.txt

If you want to estimate the unknown fraction of a metagenome and your input file is a SAM file, remember to specify the metagenome size using --nreads. You can get easily get the metagenome size from a SAM file if you have run bowtie2 without the --no-unal parameter by running
$ wc -l metagenome.sam

Otherwise, read_fastx.py is your choice: this will print on the standard error the metagenome size.
$ read_fastx.py metagenome.fastq > /dev/null

You can provide the specific database version with --index.

By default MetaPhlAn is run with --index latest: the latest version of the database is used; if it is not available, MetaPhlAn will try to download it.

When --index is specified, MetaPhlAn skips the check for the latest database version and run the analysis using the database version provided by --index located in --bowtie2db.

For advanced options and other analysis types (such as strain tracking) please refer to the full command-line options metaphlan --help.

Utility Scripts

MetaPhlAn's repository features a few utility scripts to aid in the manipulation of sample output and its visualization. These scripts can be found under the utils folder in the MetaPhlAn directory.

Merging Tables
The script merge_metaphlan_tables.py allows to combine MetaPhlAn output from several samples to be merged into one table Bugs (rows) vs Samples (columns) with the table enlisting the relative normalized abundances per sample per bug.
To merge multiple output files, run the script as below
$ merge_metaphlan_tables.py metaphlan_output1.txt metaphlan_output2.txt > metaphlan_output3.txt output/merged_abundance_table.txt
Wildcards can be used as needed:
$ merge_metaphlan_tables.py metaphlan_output*.txt > output/merged_abundance_table.txt
Output files can be merged only if the profiling was performed with the same version of the MetaPhlAn database.

There is no limit to how many files you can merge.

Converting SGB profiles to the GTDB taxonomy
The script sgb_to_gtdb_profile.py allows to convert a SGB-based MetaPhlAn 4 output into a GTDB-taxonomy-based profile.

To do so, run the script as below
$ sgb_to_gtdb_profile.py -i metaphlan_output.txt -o metaphlan_output_gtdb.txt

Alpha and beta diversity calculation

The script calculate_diversity.R allows to compute alpha and/or beta diversity, with different metrics of choice, starting from a merged MetaPhlAn table. Available alpha-diversity metrics are richness, shannon, simpson, and gini. Available beta-diversity distance functions are bray-curtis, jaccard, weighted-unifrac, unweighted-unifrac, centered log-ratio, and aitchison. For example, to generate a beta diversity distance matrix with bray-curtis, you need to run the script as below:
Rscript calculate_diversity.R -f merged_mpa4_profiles.tsv -d beta -m bray-curtis

To compute UniFrac distances, the SGB tree in the Newick format (available here) must be provided.

For the full list of options, please run:
Rscript calculate_diversity.R

Heatmap Visualization

The hclust2 script generates a hierarchically-clustered heatmap from MetaPhlAn abundance profiles. To generate the heatmap for a merged MetaPhlAn output table (as described above), you need to run the script as below:

hclust2.py \
  -i HMP.species.txt \
  -o HMP.sqrt_scale.png \
  --skip_rows 1 \
  --ftop 50 \
  --f_dist_f correlation \
  --s_dist_f braycurtis \
  --cell_aspect_ratio 9 \
  -s --fperc 99 \
  --flabel_size 4 \
  --metadata_rows 2,3,4 \
  --legend_file HMP.sqrt_scale.legend.png \
  --max_flabel_len 100 \
  --metadata_height 0.075 \
  --minv 0.01 \
  --no_slabels \
  --dpi 300 \
  --slinkage complete

GraPhlAn Visualization

The tutorial of using GraPhlAn can be found from the GraPhlAn wiki.

Customizing the database

In order to add a marker to the database, the user needs the following steps:
	•	Reconstruct the marker sequences (in fasta format) from the MetaPhlAn BowTie2 database by:
bowtie2-inspect metaphlan_databases/mpa_vJan21_CHOCOPhlAnSGB_202103 > metaphlan_databases/mpa_vJan21_CHOCOPhlAnSGB_202103_markers.fasta
	•	Add the marker sequence stored in a file new_marker.fasta to the marker set:
cat new_marker.fasta >> metaphlan_databases/mpa_vJan21_CHOCOPhlAnSGB_202103_markers.fasta
	•	Rebuild the bowtie2 database:
bowtie2-build metaphlan_databases/mpa_vJan21_CHOCOPhlAnSGB_202103_markers.fasta metaphlan_databases/mpa_vJan21_CHOCOPhlAnSGB_NEW
	•	Assume that the new marker was extracted from genome1, genome2. Update the taxonomy file from the Python console as follows:
import pickle
import bz2

db = pickle.load(bz2.open('metaphlan_databases/mpa_vJan21_CHOCOPhlAnSGB_202103.pkl', 'r'))

# Add the taxonomy of the new genomes
db['taxonomy']['7-levels taxonomy with clade names of genome1'] = ('7-levels NCBI taxonomy id of genome1', length of genome1)
db['taxonomy']['7-levels taxonomy with clade names of genome2'] = ('7-levels NCBI taxonomy id of genome1', length of genome2)

# Add the information of the new marker as the other markers
db['markers'][new_marker_name] = {
                                   'clade': the clade that the marker belongs to,
                                   'ext': {the GCA of the first external genome where the marker appears,
                                           the GCA of the second external genome where the marker appears,
                                          },
                                   'len': length of the marker,
                                   'taxon': the taxon of the marker
                                }
                                   
To see an example, try to print the first marker information:
 print list(db['markers'].items())[0]

# Save the new mpa_pkl file
with bz2.BZ2File('metaphlan_databases/mpa_vJan21_CHOCOPhlAnSGB_NEW.pkl', 'w') as ofile:
    pickle.dump(db, ofile, pickle.HIGHEST_PROTOCOL)

	•	To use the new database, remember to run metaphlan.py with the "--index mpa_vJan21_CHOCOPhlAnSGB_NEW" parameter.

Step 3: Install a new environment of Humann 3.0

https://huttenhower.sph.harvard.edu/humann


Step 4: Upgrade Humann to version 3.5/3.6
https://forum.biobakery.org/t/announcing-humann-3-5/3995

https://forum.biobakery.org/t/announcing-humann-3-6-critical-update/4155

**It looks like a new installation of HUMANN 3.0 will update it to 3.6. I will install it within the same environment as metaphlan 4.0

Getting HUMAnN 3.0 (and MetaPhlAn 3.0)
Install via conda
	•	(Optionally) Create a new conda environment for the installation
	•	conda create --name biobakery3 python=3.7
	•	conda activate biobakery3
	•	(If you haven't already) Set conda channel priority:
	•	conda config --add channels defaults
	•	conda config --add channels bioconda
	•	conda config --add channels conda-forge
	•	conda config --add channels biobakery
	•	Install HUMAnN 3.0 software with demo databases:
	•	conda install humann -c biobakery
	•	Conda-installing HUMAnN 3.0 will automatically install MetaPhlAn 3.0.
	•	To install only MetaPhlAn 3.0 execute:
conda install metaphlan -c bioconda.

After the update, I had to use the full name to active conda environments (https://stackoverflow.com/questions/58369030/could-not-find-conda-environment)
conda info --envs
source activate /Users/guillaumeonyeaghala/miniconda2/envs/mpa

/home/isrania/onyea005/.conda/envs/mpa
conda activate /home/isrania/onyea005/.conda/envs/mpa
conda activate /home/isrania/onyea005/.conda/envs/mpa082023

conda activate /home/isrania/onyea005/.conda/envs/mpa082023

conda init --help
conda init bash

conda activate /home/isrania/onyea005/.conda/envs/mpa082023

*Set up the channel priority

conda config --add channels defaults
conda config --add channels bioconda	
conda config --add channels conda-forge
conda config --add channels biobakery

*Install HUMANN 3.0 (Technically 3.6?) within the same environment which should have metaphlan 4.0
conda install humann -c biobakery

Test your installation
	•	
	•	Run HUMAnN unit tests:
	•	humann_test (~1 minute)
	•	Run the HUMAnN demo:
	•	humann -i demo.fastq -o sample_results
	•	Note: MetaPhlAn will fetch and build its marker database during this process, which takes a few minutes (but only has to be done once).
*Testing the installation
humann_test

Upgrading your databases
*Getting restarted with the analysis
	•	Load the conda module
module avail conda
**Under mesabi
module load conda/python3
Activate the conda environment
Now that you have an environment for HUMANN, activate it using the environment’s name:
After the update, I had to use the full name to active conda environments (https://stackoverflow.com/questions/58369030/could-not-find-conda-environment)
conda info --envs

Change the folder to the location with the metatranscriptomics data
**I created a new folder called HUMANN082023 for this new analysis via Filezilla

/home/isrania/onyea005/Israni4_Project_016
cd /home/isrania/onyea005/Israni4_Project_016
cd /home/isrania/onyea005/Israni4_016_test

pwd
cd /home/isrania/onyea005/HUMANN082023

After the update, I had to use the full name to active conda environments (https://stackoverflow.com/questions/58369030/could-not-find-conda-environment)
conda info --envs
source activate /Users/guillaumeonyeaghala/miniconda2/envs/mpa

/home/isrania/onyea005/.conda/envs/mpa
conda activate /home/isrania/onyea005/.conda/envs/mpa
conda activate /home/isrania/onyea005/.conda/envs/mpa082023

conda activate /home/isrania/onyea005/.conda/envs/mpa082023

conda init --help
conda init bash

conda activate /home/isrania/onyea005/.conda/envs/mpa082023
humann_test
https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4
Important! The MetaPhlAn 4 database has been substantially increased in comparison with the previous 3.1 version. Thus, for running MetaPhlAn 4, a minimum of 15GB or memory is needed.

If you have installed MetaPhlAn using Anaconda, it is advised to install the database in a folder outside the Conda environment. To do this, run
$ metaphlan --install --bowtie2db <database folder>

If you install the database in a different location, remember to run MetaPhlAn using --bowtie2db <database folder>!

By default, the latest MetaPhlAn database is downloaded and built. You can download a specific version with the --index parameter

$ metaphlan --install --index mpa_vJan21_CHOCOPhlAnSGB_202103 --bowtie2db <database folder>

When --index is specified, MetaPhlAn skips the check for the latest database version and run the analysis using the database version provided by --index located in --bowtie2db.

This option is recommended when MetaPhlAn is run on HPC clusters or containerized
If you have issues in downloading the database, you can get it from:
	•	Segatalab FTP

Just download the .tar, .md5, and the mpa_latest files and place them in the metaphlan_databases folder.










*Getting restarted with the analysis

ssh yourusername@login.msi.umn.edu (use ssh yourusername@mangi.msi.umn.edu or ssh yourusername@mesabi.msi.umn.edu respectively)
ssh onyea005@mesabi.msi.umn.edu
	•	Check the folder you are in
pwd
* my home folder is when I first login is “/home/prizm001/onyea005”
* /home/isrania/onyea005
*Change the folder to the israni MSI group
cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
cd /panfs/roc/groups/5/isrania/onyea005
cd /panfs/roc/groups/5/isrania/onyea005/Israni4_Project_016

Log in to an interactive node, making sure you have enough time and memory (https://www.msi.umn.edu/content/interactive-queue-use-isub)

isub -n nodes=1:ppn=8 -m 16GB -w 8:00:00	
#I don’t think I need to log into mesabi or Itasca to run this job if I am logged into the interactive node since it logs me into the lab queue (So use mesabi instead)
MSI's LabQi Cluster will be shut down and decommissioned during February Maintenance (February 5, 2020.) Several months ago, MSI released the Mesabi Interactive Queue as a replacement for the LabQi Cluster (ssh lab.)  The Mesabi Interactive Queue can be used in real-time for tasks such as interactive data exploration, creating plots or images, visualizations, or testing sections of code that will be used on other MSI queues.

https://www.msi.umn.edu/content/interactive-hpc

What is Interactive HPC?

MSI’s interactive HPC resources are composed of some of the same systems used to support our batch scheduled HPC activities. The main difference between the two services is the way in which users interact with the MSI resource. Interactive HPC systems allow real-time user inputs in order to facilitate code development, real-time data exploration, and visualizations. Interactive HPC systems are used when data are too large to download to a desktop or laptop, software is difficult or impossible to install on a personal machine, or specialized hardware resources (e.g., GPUs) are needed to visualize large datasets. User input might come through a command line interface (i.e., system shell) while debugging a piece of software or through mouse clicks while using a program with a graphical interface.

What can I do with Interactive HPC?

Unlike MSI’s other computing options, the Interactive HPC resources are designed only to be used in real time for tasks such as interactive data exploration, creating plots or images, visualizations, or testing sections of code that will be used on other MSI systems.

Common use cases include:
	•	Testing a compiled program at the command line before submitting it as a job
	•	Developing code or exploring data in an interactive environment like MATLAB, IPython, or Rstudio
	•	Visualizing data or building simulations in applications like Schrodinger, COMSOL, or Avizo

What types of Interactive HPC are available?
	•	srun:  Submit an interactive job on the current HPC system to provide a command line or graphical interface to interactively run software. 
	•	NICE: Connects a user to a graphical environment, which may have specialized GPUs for visualizing large datasets.
	•	Total Slots: 32 slots for accelerated remote desktops
	•	Total Memory:  Up to 16 GB/session
	•	Graphical Accelerators: Nvidia GK107GL
	•	CITRIX: Citrix for Windows Environments: Connects a user to a graphical Windows environment, where they can access specialized Windows software for modeling and data analysis.
	•	NX NoMachine: Connects a user to a graphical environment, which may be used as a lightweight remote desktop for connecting to other MSI resources.
	•	Jupyter Notebooks: A web portal for notebook-based computing in the browser, which can be used for reproducible and shareable data analysis, visualization, and scripted control of larger tasks. Currently supports Python 2, Python 3, and R. 
Here are several examples of how you can request an interactive HPC job on the new Mesabi Interactive Queue.

The basic syntax for submitting an interactive job on Mesabi (I assume that in combination with the new login on mesabi and mangi the syntax occurs after login into mesabi.msi.umn.edu):

qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I

The basic syntax for testing MPI code between 2 nodes:
qsub -l nodes=2:ppn=2,mem=8gb,walltime=12:00:00 -q interactive -W x=nmatchpolicy:exactnode -I

The basic syntax for creating a job with X-forwarding for GUI's and visulization:
qsub -X -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I

Back to the interactive login
qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I

The full specifications of the Mesabi Interactive Queue can be found on the MSI Queues webpage. Also, more examples on how to use qsub -I for interactive use can be found on the Interactive queue use with qsub webpage.
https://www.msi.umn.edu/content/interactive-queue-use-srun
** Will need to learn how to use Slurm to schedule jobs (https://www.msi.umn.edu/slurm)
Interactive queue use with srun
It is possible to start an interactive job on any of MSI’s clusters that support job submission. Interactive jobs can be useful for tasks including data exploration, development, or (with X11 forwarding) visualization activities.
The basic syntax for submitting an interactive job is illustrated using the following example for Mesabi:
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=1gb -t 1:00:00 -p interactive --pty bash
If you are submitting an interactive job from a session with X11 forwarding and would like to continue using X11 forwarding inside your job, you would add the additional flag ‘-x11’, as follows:
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=1gb -t 1:00:00 -p interactive --x11 --pty bash
The basic syntax for requesting two nodes, for instance to test MPI code between two nodes:
srun -N 2 --ntasks-per-node=1  --mem-per-cpu=1gb -t 1:00:00 -p interactive --pty bash
The syntax for requesting an interactive gpu node with a k40 GPU is:
srun -n 12 -t 1:00:00 -p interactive-gpu --gres=gpu:k40:1 --pty bash
You may also submit an interactive job using an interactive submission script. For instance, to submit an interactive job to test an MPI code (in our case named ‘interactive.sh’) with contents:
#SBATCH -N 2
#SBATCH --ntasks-per-node=1
#SBATCH --mem-per-cpu=1gb
#SBATCH -t 1:00:00
#SBATCH -p interactive
./my_application
You would then submit this job using:
sbatch interactive.sh
Interactive jobs are subject to the same rules and limitations as batch jobs. Interactive jobs have access to only those partitions that are available on the cluster they are submitted from. Jobs that request more resources may wait in the queue longer, so be sure that you are requesting only those resources that you need.
 An interactive job supports all of the usual Slurm commands. These commands are included as flags when requesting an interactive job. 
 Example interactive srun Session
 username@mydesktop$ ssh -Y msiusername@mesabi.msi.umn.edu
Password:
Last login: Tue May 10 15:16:06 2018 from mydesktop.mydept.umn.edu
-------------------------------------------------------------------------------
            University of Minnesota Supercomputing Institute
                                Mesabi
                        HP Haswell Linux Cluster
-------------------------------------------------------------------------------
[... output truncated ...]
---------------
username@ln0006 [~] % srun -N 2 --ntasks-per-node=1  --mem-per-cpu=1gb --x11 -t 1:00:00 --pty bash
username@cn0001 [~] % 
username@cn0001 [~] % echo $SLURM_JOB_NODELIST
cn[0001-0002]
username@cn0001 [~] %
***Note that this interactive job is using two nodes (-N 2). At this point you are currently in your home directory, and can load modules and launch software, including GUI software.

Back to the interactive login using srun (might take a few mins to get the access)

*Old syntax
qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I
*New syntax
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=16gb -t 4:00:00 -p interactive --pty bash
Now you can look at the contents of the directory using the ls command:

ls -lh
You can always check the location of your current directory (aka folder) using the pwd command.  If you type:

pwd
Login into an interactive node to process the data
*I will try to add a CPU per task option to the interactive run and to the sbatch file
https://www.msi.umn.edu/support/faq/how-can-i-use-gnu-parallel-run-lot-commands-parallel
https://www.msi.umn.edu/content/interactive-queue-use-srun
Interactive queue use with srun
It is possible to start an interactive job on any of MSI’s clusters that support job submission. Interactive jobs can be useful for tasks including data exploration, development, or (with X11 forwarding) visualization activities.
 The basic syntax for submitting an interactive job is illustrated using the following example for Mesabi:
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=1gb -t 1:00:00 -p interactive --pty bash
If you are submitting an interactive job from a session with X11 forwarding and would like to continue using X11 forwarding inside your job, you would add the additional flag ‘-x11’, as follows:
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=1gb -t 1:00:00 -p interactive --x11 --pty bash
The basic syntax for requesting two nodes, for instance to test MPI code between two nodes:
srun -N 2 --ntasks-per-node=1  --mem-per-cpu=1gb -t 1:00:00 -p interactive --pty bash
The syntax for requesting an interactive gpu node with a k40 GPU is:
srun -n 12 -t 1:00:00 -p interactive-gpu --gres=gpu:k40:1 --pty bash
https://stackoverflow.com/questions/51139711/hpc-cluster-select-the-number-of-cpus-and-threads-in-slurm-sbatch
The terminology used in the sbatch man page might be a bit confusing. Thus, I want to be sure I am getting the options set right. Suppose I have a task to run on a single node with N threads. Am I correct to assume that I would use --nodes=1 and --ntasks=N?
I am used to thinking about using, for example, pthreads to create N threads within a single process. Is the result of that what they refer to as "cores" or "cpus per task"? CPUs and threads are not the same things in my mind.

Depending on the parallelism you are using: distributed or shared memory
--ntasks=# : Number of "tasks" (use with distributed parallelism).
--ntasks-per-node=# : Number of "tasks" per node (use with distributed parallelism).
--cpus-per-task=# : Number of CPUs allocated to each task (use with shared memory parallelism).

From this question: if every node has 24 cores, is there any difference between these commands?
sbatch --ntasks 24 [...]
sbatch --ntasks 1 --cpus-per-task 24 [...]
Answer: (by Matthew Mjelde)
Yes there is a difference between those two submissions. You are correct that usually ntasks is for mpi and cpus-per-task is for multithreading, but let’s look at your commands:
For your first example, the sbatch --ntasks 24 […] will allocate a job with 24 tasks. These tasks in this case are only 1 CPUs, but may be split across multiple nodes. So you get a total of 24 CPUs across multiple nodes.
For your second example, the sbatch --ntasks 1 --cpus-per-task 24 [...] will allocate a job with 1 task and 24 CPUs for that task. Thus you will get a total of 24 CPUs on a single node.
In other words, a task cannot be split across multiple nodes. Therefore, using --cpus-per-task will ensure it gets allocated to the same node, while using --ntasks can and may allocate it to multiple nodes.

Another good Q&A from CÉCI's support website: Suppose you need 16 cores. Here are some use cases:
	•	you use mpi and do not care about where those cores are distributed: --ntasks=16
	•	you want to launch 16 independent processes (no communication): --ntasks=16
	•	you want those cores to spread across distinct nodes: --ntasks=16 and --ntasks-per-node=1 or --ntasks=16 and --nodes=16
	•	you want those cores to spread across distinct nodes and no interference from other jobs: --ntasks=16 --nodes=16 --exclusive
	•	you want 16 processes to spread across 8 nodes to have two processes per node: --ntasks=16 --ntasks-per-node=2
	•	you want 16 processes to stay on the same node: --ntasks=16 --ntasks-per-node=16
	•	you want one process that can use 16 cores for multithreading: --ntasks=1 --cpus-per-task=16
	•	you want 4 processes that can use 4 cores each for multithreading: --ntasks=4 --cpus-per-task=4

https://stackoverflow.com/questions/49466020/specify-number-of-cpus-for-a-job-on-slurm
https://researchcomputing.princeton.edu/support/knowledge-base/scaling-analysis
https://docs-research-it.berkeley.edu/services/high-performance-computing/user-guide/running-your-jobs/scheduler-examples/
Back to the interactive login using srun (might take a few mins to get the access)

https://www.msi.umn.edu/content/interactive-queue-use-srun
*Old syntax
qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I
*New syntax
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=16gb -t 4:00:00 -p interactive --pty bash

*Testing out adding multiple CPUs to run the code faster (https://www.msi.umn.edu/support/faq)
It seems like nodes and CPU is different so the n-task per node is what determine thee number of CPUs we can use?
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=32gb -t 4:00:00 -p interactive --pty bash
*I will use this code to ask for multiple CPUs for the analysis)https://stackoverflow.com/questions/51139711/hpc-cluster-select-the-number-of-cpus-and-threads-in-slurm-sbatch
--ntasks=4 --cpus-per-task=4

srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=32gb -t 4:00:00 -p interactive --pty bash

*Per the SLURM PDF guide (See slide 10) the interactive queue has a maximum of 4 cores/CPU that can be requested through srun. I will request 8 when submitting a job via SLURM
srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=32gb -t 4:00:00 -p interactive --pty bash

srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=8gb -t 4:00:00 -p interactive --pty bash

*Setting the srun to 16GB since metaphlan 4 mentioned that it needs 15GB to run
srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=16gb -t 4:00:00 -p interactive --pty bash

*Upping the requirements due to Methaphan4
srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=32gb -t 4:00:00 -p interactive --pty bash
Change the folder to the location with the metatranscriptomics data
**I created a new folder called HUMANN082023 for this new analysis via Filezilla

/home/isrania/onyea005/Israni4_Project_016
cd /home/isrania/onyea005/Israni4_Project_016
cd /home/isrania/onyea005/Israni4_016_test

pwd
cd /home/isrania/onyea005/HUMANN082023
*I created a new folder for the database via filezilla
/panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases
	•	Load the conda module
module avail conda
**Under mesabi
module load conda/python3
Activate the conda environment
Now that you have an environment for HUMANN, activate it using the environment’s name:
After the update, I had to use the full name to active conda environments (https://stackoverflow.com/questions/58369030/could-not-find-conda-environment)
conda info --envs
Change the folder to the location with the metatranscriptomics data

**I created a new folder called HUMANN082023 for this new analysis via Filezilla

/home/isrania/onyea005/Israni4_Project_016
cd /home/isrania/onyea005/Israni4_Project_016
cd /home/isrania/onyea005/Israni4_016_test

pwd
cd /home/isrania/onyea005/HUMANN082023

After the update, I had to use the full name to active conda environments (https://stackoverflow.com/questions/58369030/could-not-find-conda-environment)
conda info --envs
source activate /Users/guillaumeonyeaghala/miniconda2/envs/mpa

/home/isrania/onyea005/.conda/envs/mpa
conda activate /home/isrania/onyea005/.conda/envs/mpa
conda activate /home/isrania/onyea005/.conda/envs/mpa082023

conda activate /home/isrania/onyea005/.conda/envs/mpa082023

conda init --help
conda init bash

conda activate /home/isrania/onyea005/.conda/envs/mpa082023
humann_test





https://github.com/biobakery/biobakery/wiki/humann3

I will now pick up on the HUMANN tutorial

Welcome to the HUMAnN 3.0 tutorial

Tutorials for previous versions of HUMAnN:
	•	HUMAnN 1.0 tutorial
	•	HUMAnN 2.0 tutorial
HUMAnN (the HMP Unified Metabolic Analysis Network) is a method for efficiently and accurately profiling the abundance of microbial metabolic pathways and other molecular functions from metagenomic or metatranscriptomic sequencing data. It is appropriate for any type of microbial community, not just the human microbiome (the name "HUMAnN" is a historical product of the method's origins in the Human Microbiome Project).

HUMAnN 3.0 can be installed 1) from PyPI, 2) as source code from the HUMAnN Github repository, or 3) using our quickstart documentation.

Support for HUMAnN is available via the HUMAnN channel of the bioBakery Support Forum.

Table of Contents
	•	1. Setup
	•	1.1 Requirements
	•	1.2 Installation
	•	1.2.1 Install HUMAnN
	•	1.2.2 Install MetaPhlAn
	•	2. Metagenome functional profiling
	•	2.1 HUMAnN input formats
	•	2.2 Running HUMAnN: the basics
	•	2.3 HUMAnN default outputs
	•	demo_genefamilies.tsv
	•	demo_pathabundance.tsv
	•	demo_pathcoverage.tsv
	•	2.4 Running HUMAnN: pre-mapped SAM/BAM input
	•	2.5 Running HUMAnN: pre-computed protein blastx M8 input
	•	2.6 Running HUMAnN: skipping translated search
	•	3. Manipulating HUMAnN output tables
	•	3.1 Normalizing RPKs to relative abundance
	•	3.2 Regrouping genes to other functional categories
	•	3.3 Attaching names to features
	•	4. Advanced HUMAnN topics
	•	4.1 HUMAnN intermediate files
	•	4.2 HUMAnN for multiple samples
	•	5. Plotting stratified functions












Overview


1. Setup
1.1 Requirements
	•	Python (>= 3.7)
	•	MetaPhlAn (version >= 3.0)
	•	Bowtie2 (version >= 2.2)
	•	DIAMOND (=0.9.36)
	•	MinPath
NOTE: Bowtie2, DIAMOND, and MinPath are automatically installed when installing HUMAnN. If these dependencies do not appear to be installed after installing HUMAnN with pip, it might be that your environment is setup to use wheels instead of installing from source. HUMAnN must be installed from source for it to also be able to install dependencies. To force pip to install HUMAnN from source, add one of the following options to your install command, --no-use-wheel or --no-binary :all:.

1.2 Installation
NOTE: If you are using a bioBakery machine image (e.g. in Google Cloud) you do not need to install HUMAnN because the software and its dependencies are already available.

1.2.1 Install HUMAnN
The easiest way to install HUMAnN is with pip, Conda, or Docker. For other options, please refer to the HUMAnN User Manual.

To install with pip:
pip install humann

You can also install via Conda, which will additionally install MetaPhlAn and utility dependencies. Before doing so, make sure to configure your channels so that biobakery is at the top of the list:

conda install -c biobakery humann

By default, a pip (or other) HUMAnN install does not include the full-scale nucleotide and protein sequence databases needed for real-world metagenome or metatranscriptome profiling, as these are quite large (several GBs). The humann_databases script can be used to fetch these from the web and connect them to your HUMAnN installation. For tutorial purposes, we'll be using (small) demo databases, which are also fetchable with humann_databases (if not already available).

humann_databases --download chocophlan DEMO humann_dbs
humann_databases --download uniref DEMO_diamond humann_dbs

The commands above download the demo-scale databases to a folder named humann_dbs in the current working directory. Please note that you can download these (and full-scale) databases to any location on your machine: HUMAnN will track where you have placed the databases and update its configuration file.

After installation from pip or another source, you may optionally test your local HUMAnN environment (this takes ~1 minute):

humann_test

Which yields (highly abbreviated):

test_calculate_percent_identity (basic_tests_nucleotide_search.TestBasicHumann2NucleotideSearchFunctions) ... ok
test_calculate_percent_identity_insert_delete (basic_tests_nucleotide_search.TestBasicHumann2NucleotideSearchFunctions) ... ok
test_calculate_percent_identity_missing_cigar_identifier (basic_tests_nucleotide_search.TestBasicHumann2NucleotideSearchFunctions) ... ok
test_calculate_percent_identity_missing_md_field (basic_tests_nucleotide_search.TestBasicHumann2NucleotideSearchFunctions) ... ok
test_calculate_percent_identity_multiple_M_cigar_fields (basic_tests_nucleotide_search.TestBasicHumann2NucleotideSearchFunctions) ... ok
...

1.2.2 Install MetaPhlAn

HUMAnN provides organism-specific community functional profiles, and to do so it first detects the organisms present in a community using MetaPhlAn. As MetaPhlAn is thus a prerequisite for running the HUMAnN software, please ensure that you've installed MetaPhlAn before running HUMAnN. You can test if MetaPhlAn is available in your system path by executing 

metaphlan --version.

pwd
cd /home/isrania/onyea005/HUMANN082023	
humann_test
metaphlan --version
I have tested the HUMANN installation and checked that metaphlan is installed

2. Metagenome functional profiling

A note on inspecting files from the command line: During this tutorial you'll inspect many HUMAnN input and output files, many of which are tab-delimitted tables (TSV files). For simplicity the tutorial uses less -S file.tsv for this task. To match the more spreadsheet-like columns seen in the tutorial document, you would first use the column command, as in:

column -t -s $'\t' file.tsv | less -S

2.1 HUMAnN input formats

HUMAnN can start from a few different types of input data each in a few different types of formats:

Quality-controlled shotgun sequencing reads
This is the most common starting point
A metagenome (DNA reads) or metatranscriptome (RNA reads)
Formats: fastq, fastq.gz, fasta, or fasta.gz
Pre-computed mappings of reads to database sequences
Formats: sam, bam, or blastm8
Pre-computed (typically gene) abundance tables
Formats: tsv or biom

This tutorial will use demo input files distributed with the HUMAnN installation under the examples directory. If you installed with pip or conda and don't have these handy, you can obtain copies by right-clicking these links and selecting "save link as":

demo.fastq.gz
demo.sam
demo.m8

*Skip the attempt to copy the files from MSI below, I will use filezilla but download things overnight due to the long duration
cd /panfs/roc/data_release/0/umgc/cmstaley/nextseq/210712_NB551164_0296_AH2C37BGXH/Staley_Project_069
cp /panfs/roc/data_release/0/umgc/cmstaley/nextseq/210712_NB551164_0296_AH2C37BGXH/Staley_Project_069 /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Staley_Project_069_v2

pwd
cd /home/isrania/onyea005/HUMANN082023
After right clicking and downloading the 3 files on my local computer, I created a folder call demo at /home/isrania/onyea005/HUMANN082023/demo and moved the three files there


2.2 Running HUMAnN: the basics
If you are running this tutorial as part of a short course, all input files have been pre-downloaded into the cloud instances. To run this tutorial from the correct location, enter:

cd ~/Tutorials/humann3
cd /home/isrania/onyea005/HUMANN082023/demo
Check if all input files are available in the input directory with ls. We will copy files from this folder to the current working directory over the course of the tutorial. Let us start with the aforementioned 3 demo input files.


cp input/demo* 

A canonical HUMAnN run uses nucleotide mapping and translated search to provide organism-specific gene and pathway abundance profiles from a single metagenome. By default, gene families are annotated using UniRef90 definitions and pathways using MetaCyc definitions. From the examples folder (or the location where you saved demo.fastq.gz above) execute the following:

humann --input demo.fastq.gz --output demo_fastq

humann --input demo.fastq.gz --output demo_fastq
*With Metaphlan4 you probably want to run the updated version. (https://forum.biobakery.org/t/metaphlan4-run-time/5017)
humann --input demo.fastq.gz --output demo_fastq --threads 4

Since you have already downloaded the databases in the initial installation, you do not need to download the databases again unless there are new versions available. However, you will want to update your latest HUMAnN 3.0 install to point to the databases you have downloaded as by default the new install configuration will point to the demo databases.

To update your HUMAnN 3.0 configuration file to include the locations of your downloaded databases, please use the following steps.

Update the location of the ChocoPhlAn database (replacing $DIR with the full path to the directory containing the ChocoPhlAn database)


cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_config --update database_folders nucleotide $DIR
humann_config --update database_folders nucleotide /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases
humann_config --update database_folders nucleotide /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases/chocophlan

Update the location of the UniRef database (replacing $DIR with the full path to the directory containing the UniRef database)


cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_config --update database_folders protein $DIR

humann_config --update database_folders protein /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases/uniref

A canonical HUMAnN run uses nucleotide mapping and translated search to provide organism-specific gene and pathway abundance profiles from a single metagenome. By default, gene families are annotated using UniRef90 definitions and pathways using MetaCyc definitions. From the examples folder (or the location where you saved demo.fastq.gz above) execute the following:

humann --input demo.fastq.gz --output demo_fastq

humann --input demo.fastq.gz --output demo_fastq
*With Metaphlan4 you probably want to run the updated version. (https://forum.biobakery.org/t/metaphlan4-run-time/5017)
humann --input demo.fastq.gz --output demo_fastq --threads 4

(Note1: If you have more than 1 CPU available for computation, you can use N of them by adding --threads N to the above command.)

** I am getting the following error

Downloading MetaPhlAn database
Please note due to the size this might take a few minutes

\Downloading and uncompressing indexes

File /home/isrania/onyea005/.conda/envs/mpa082023/lib/python3.7/site-packages/metaphlan/metaphlan_databases/mpa_vOct22_CHOCOPhlAnSGB_202212_bt2.tar already present!

File /home/isrania/onyea005/.conda/envs/mpa082023/lib/python3.7/site-packages/metaphlan/metaphlan_databases/mpa_vOct22_CHOCOPhlAnSGB_202212_bt2.md5 already present!
MD5 checksums do not correspond! If this happens again, you should remove the database files and rerun MetaPhlAn so they are re-downloaded
https://forum.biobakery.org/t/metaphlan-3-md5-error/484
https://forum.biobakery.org/t/humann3-metaphlan-3-0-error-md5-checksums-do-not-match/619/4
https://github.com/biobakery/MetaPhlAn/issues/100
https://groups.google.com/g/metaphlan-users/c/5ltD8X8Xitc?pli=1

*First I will try to reinstall metaphlan within the environment

https://forum.biobakery.org/t/announcing-humann-3-5/3995
	•	Upgrading MetaPhlAn:
conda install -c bioconda metaphlan=4.0.0

*Then I tried rerunning the Humann command

A canonical HUMAnN run uses nucleotide mapping and translated search to provide organism-specific gene and pathway abundance profiles from a single metagenome. By default, gene families are annotated using UniRef90 definitions and pathways using MetaCyc definitions. From the examples folder (or the location where you saved demo.fastq.gz above) execute the following:

humann --input demo.fastq.gz --output demo_fastq

humann --input demo.fastq.gz --output demo_fastq
*With Metaphlan4 you probably want to run the updated version. (https://forum.biobakery.org/t/metaphlan4-run-time/5017)
humann --input demo.fastq.gz --output demo_fastq --threads 4
** This is very time consuming, so I will try to submit a script to run the test code:



Learning how to use SLURM to schedule a job since running the humann pipeline on all the files in the folder would be veery time consuming
https://www.msi.umn.edu/slurm
What is Slurm?
Slurm is a best-in-class, highly-scalable scheduler for HPC clusters. It allocates resources, provides a framework for executing tasks, and arbitrates contention for resources by managing queues of pending work.
Why is MSI transitioning to the Slurm scheduler?
Slurm has become an industry standard for scheduling among HPC centers. It’s an open-source scheduler with a plugin framework that allows us to leverage tools developed at other centers. It is capable of stable management of a larger number of jobs than our current scheduler. Finally, it’s architecture opens opportunities to leverage technologies that will be useful for many areas of scientific computation.
How does the transition to Slurm impact my work on MSI systems?
The most obvious adjustment everyone will need to make is to learn a new set of commands for submitting jobs and checking on job status. If you have written scripts that depend on the job scheduler, they will need to be modified to match the syntax used in Slurm. This is also true of some software that MSI maintains.
https://www.msi.umn.edu/content/job-submission-and-scheduling-slurm
Job Submission and Scheduling (Slurm)
MSI systems use job queues to efficiently and fairly manage when computations are executed. When computational jobs are submitted to a job queue they wait in the queue until the appropriate computational resources are available.
 
The queuing system which MSI uses is called Slurm. To submit a job to a Slurm queue, users create Slurm job scripts. Slurm job scripts contain information on the resources requested for the calculation, as well as the commands for executing the calculation.
Slurm Script Format
A Slurm job script is a small text file containing information about what resources a job requires, including time, number of nodes, and memory.   The Slurm script also contains the commands needed to begin executing the desired computation. A sample Slurm job script is shown below.
#!/bin/bash -l        
#SBATCH --time=8:00:00
#SBATCH --ntasks=8
#SBATCH --mem=10g
#SBATCH --tmp=10g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=sample_email@umn.edu 
cd ~/program_directory
module load intel 
module load ompi/intel 
mpirun -np 8 program_name < inputfile > outputfile
The first line in the Slurm script defines which type of shell the script will be read with (how the system will read the file).  It is recommended to make the first line #!/bin/bash -l
Commands for the Slurm queuing system begin with #SBATCH.  Lines 2–4 in the above sample script contain the Slurm resource request.   The sample job will require 8 hours, 8 processor cores,  and 10 gigabytes of memory.  The resource request must contain appropriate values; if the requested time, processors, or memory are not suitable for the hardware the job will not be able to run. 
The two lines containing #SBATCH --mail-type, and #SBATCH --mail-user=sample_email@umn.edu , are both commands having to do with sending message emails to the user. The first of these lines instruct the Slurm system to send a message email when the job aborts, begins, or ends.  Other options here include NONE, BEGIN, END, and FAIL. The second command specifies the email address to be used. Using the message emails is recommended because the reason for a job failure can often be determined using information in the emails.
The rest of the sample Slurm script contains the commands which will be executed to begin the calculation. A Slurm script should contain the appropriate change directory commands to get to the job execution location. A Slurm script also needs to contain module load commands for any software modules that the calculation might need. The last lines of a Slurm script contain commands used to execute the calculation. In the above example the final line contains an execution command to start a program which uses MPI communication to run on 8 processor cores.
Submitting Job Scripts
Once a job script is made it is submitted using the sbatch command:
sbatch -p partitionname scriptname   
Here partitionname is the name of the partition (queue) being submitted to, and scriptname is the name of the job script.  The -p partitionname portion of the command may be omitted, in which case the job would be submitted to whichever partition is set as the default.  Alternatively, the partition specification can be placed inside the job script (see below).
To view all of the jobs submitted by a particular user use the command:
squeue -u username
This command will display the status of the specified jobs, and the associated job ID numbers. The command squeue by itself will show all jobs on the system.
To cancel a submitted job use the command:
scancel jobIDnumber
Here jobIDnumber should be replaced with the appropriate job ID number determined by using the squeue command.
Slurm Script Commands
Below is a table summarizing some commands that can be used inside Slurm job scripts. The first four commands (interpreter specification, and resource request) are required, while the other commands are optional. Each of the below Slurm commands is meant to go on a single line within a Slurm script.
 
Slurm command
Effect
#!/bin/bash -l
Specifies how the Slurm file should be read (by the bash interpreter). A statement like this is required to be the first line of a Slurm script.
#SBATCH --time=8:00:00
Specifies the maximum limit for how long the job will be allowed to run. (8 hours)
#SBATCH --ntasks=8
Specifies the number of processors (cores) that will be reserved for this job.  (8)
#SBATCH --mem=10g
Specifies the maximum limit for memory usage. This job will die if the application tries to use more than 10GB of memory.
#SBATCH --tmp=10g
Specifies 10 GB of temporary disk will be available for this job in /tmp.
#SBATCH --mail-type=ALL  
Specifies which events will trigger an email message. Other options here include NONE, BEGIN, END, and FAIL. 

#SBATCH --mail-user=me@umn.edu
Specifies the email address that should be used when the Slurm system sends message emails.
#SBATCH -p small,mygroup
Specifies the partition to be the “small” partition, or a partition named “mygroup”. The job will start at the earliest time one of these partitions can accommodate the job.
#SBATCH --gres=gpu:v100:2
#SBATCH -p v100
Request two v100 GPUs for a job submitted to the V100 queue.

https://www.msi.umn.edu/partitions#slurm
Slurm Partitions
Under MSI's new scheduler, Slurm, queues are known as partitions. The job partitions on our systems manage different sets of hardware, and have different limits for quantities such as walltime, available processors, and available memory. When submitting a calculation it is important to choose a partition where the job is suited to the hardware and resource limitations.
Selecting a Partition
Each MSI system contains job partitions managing sets of hardware with different resource and policy limitations. MSI currently has two primary systems: the supercomputer Mesabi and the Mesabi expansion Mangi. Mesabi has high-performance hardware and a wide variety of partitions suitable for many different job types. Mangi expands Mesabi and should be your first choice for submitting jobs. Which system to choose depends highly on which system has partitions appropriate for your software/script. More information about selecting a partitions and the different partition parameters can be found on the Choosing A Partition (Slurm) page.
Below is a summary of the available partitions organized by system, and the associated limitations. The quantities listed are totals or upper limits.
Partition name
Node sharing?
Cores per node
Walltime limit
Total node memory
Advised memory per core
Local scratch per node
Maximum Nodes per Job
amdsmall (1)
Yes
128
96:00:00
248 GB
1900 MB
415 GB
1
amdlarge
No
128
24:00:00
248 GB
1900 MB
415 GB
32
amd512
Yes
128
96:00:00
499 GB
4000 MB
415 GB
1
amd2tb
Yes
128
96:00:00
1995 GB
15 GB
415 GB
1
v100 (1)
Yes
24
24:00:00
374 GB
15 GB
859 GB
1
small
Yes
24
96:00:00
60 GB
2500 MB
429 GB
10
large
No
24
24:00:00
60 GB
2500 MB
429 GB
48
max
Yes
24
696:00:00
60 GB
2500 MB
429 GB
1
ram256g
Yes
24
96:00:00
248 GB
10 GB
429 GB
2
ram1t (2)
Yes
32
96:00:00
1002 GB
31 GB
380 GB
2
k40 (1)
Yes
24
24:00:00
123 GB
5 GB
429 GB
40
interactive (3)
Yes
24
24:00:00
60 GB
2 GB
228 GB
2
interactive-gpu (3)
Yes
24
24:00:00
60 GB
2 GB
228 GB
2
preempt (4)
Yes
24
24:00:00
60 GB
2 GB
228 GB
2
preempt-gpu (4)
Yes
24
24:00:00
60 GB
2 GB
228 GB
2

(1) Note: In addition to selecting a GPU partition, GPUs need to be requested for all GPU jobs.  A  k40 GPU can be requested by including the following two lines in your submission script: 
#SBATCH -p k40                                             
#SBATCH --gres=gpu:k40:1
A  V100 GPU can be requested by including the following two lines in your submission script: 
#SBATCH -p v100                                            
#SBATCH --gres=gpu:v100:1
(2) Note: The ram1t nodes contain Intel Ivy Bridge processors, which do not support all of the optimized instructions of the Haswell processors. Programs compiled using the Haswell instructions will only run on the Haswell processors.
(3) Note: Users are limited to 2 jobs in the interactive and interactive-gpu partitions. 
(4) Note: Jobs in the preempt and preempt-gpu partitions may be killed at any time to make room for jobs in the interactive or interactive-gpu partitions.

Submitting Job Scripts
Once a job script is made it is submitted using the sbatch command:
sbatch -p partitionname scriptname   
Here partitionname is the name of the partition (queue) being submitted to, and scriptname is the name of the job script.  The -p partitionname portion of the command may be omitted, in which case the job would be submitted to whichever partition is set as the default.  Alternatively, the partition specification can be placed inside the job script (see
sbatch -p partitionname scriptname   
cd /home/isrania/onyea005
sbatch -p amdsmall HUMANN_MISSION_MSI_SLURM.rtf   
sbatch -p amdsmall HUMANN_MISSION_MSI_SLURM.txt   
**I wasn’t able to figure out how I could save a txt file into a shell script to submit to MSI, so I will use the online script comverter I found

https://support.ceci-hpc.be/doc/_contents/QuickStart/SubmittingJobs/SlurmTutorial.html
http://www.ceci-hpc.be/scriptgen.html

https://ubccr.freshdesk.com/support/solutions/articles/5000688140-submitting-a-slurm-job-script

Create a SLURM script using an editor such as vi or emacs using steps 1 through 3.  The script (or file) can be called anything you want but should end in .sh (i.e. myscript.sh).  If you are unfamiliar with the UNIX commands to edit files, please read this article

http://www.compciv.org/recipes/cli/basic-shell-scripts/

http://www.compciv.org/bash-guide/
https://slurm.schedmd.com/quickstart.html

The gameplan is to save the file as a txt file from word on my mac, move over to windows to forcefully modify the txt extension to SH 

https://support.apple.com/guide/terminal/use-command-line-text-editors-apdb02f1133-25af-4c65-8976-159609f99817/mac

nvm 
https://www.msi.umn.edu/content/job-submission-and-scheduling-slurm
Job Submission and Scheduling (Slurm)
MSI systems use job queues to efficiently and fairly manage when computations are executed. When computational jobs are submitted to a job queue they wait in the queue until the appropriate computational resources are available.
 
The queuing system which MSI uses is called Slurm. To submit a job to a Slurm queue, users create Slurm job scripts. Slurm job scripts contain information on the resources requested for the calculation, as well as the commands for executing the calculation.
Slurm Script Format
A Slurm job script is a small text file containing information about what resources a job requires, including time, number of nodes, and memory.   The Slurm script also contains the commands needed to begin executing the desired computation. A sample Slurm job script is shown below.
#!/bin/bash -l        
#SBATCH --time=8:00:00
#SBATCH --ntasks=8
#SBATCH --mem=10g
#SBATCH --tmp=10g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=sample_email@umn.edu 
cd ~/program_directory
module load intel 
module load ompi/intel 
mpirun -np 8 program_name < inputfile > outputfile
The first line in the Slurm script defines which type of shell the script will be read with (how the system will read the file).  It is recommended to make the first line #!/bin/bash -l
Commands for the Slurm queuing system begin with #SBATCH.  Lines 2–4 in the above sample script contain the Slurm resource request.   The sample job will require 8 hours, 8 processor cores,  and 10 gigabytes of memory.  The resource request must contain appropriate values; if the requested time, processors, or memory are not suitable for the hardware the job will not be able to run. 
The two lines containing #SBATCH --mail-type, and #SBATCH --mail-user=sample_email@umn.edu , are both commands having to do with sending message emails to the user. The first of these lines instruct the Slurm system to send a message email when the job aborts, begins, or ends.  Other options here include NONE, BEGIN, END, and FAIL. The second command specifies the email address to be used. Using the message emails is recommended because the reason for a job failure can often be determined using information in the emails.
The rest of the sample Slurm script contains the commands which will be executed to begin the calculation. A Slurm script should contain the appropriate change directory commands to get to the job execution location. A Slurm script also needs to contain module load commands for any software modules that the calculation might need. The last lines of a Slurm script contain commands used to execute the calculation. In the above example the final line contains an execution command to start a program which uses MPI communication to run on 8 processor cores.
http://www.compciv.org/recipes/cli/basic-shell-scripts/

Let's use the nano text editor to create a shell script named hello.sh. Follow these steps:
	•	Run nano hello.sh
	•	nano should open up and present an empty file for you to work in. Type in the shell command the commands you want to run via SLURM:
Here is the code for the test MISSION metatranscriptomics dataset
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --ntasks=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /home/isrania/onyea005/Israni4_Project_016 
module load conda/python3 
conda activate /home/isrania/onyea005/.conda/envs/mpa 
for f in *.fastq.gz; do humann -i $f -o mission_job; done

Here is the code for the full MISSION metatranscriptomics dataset
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /panfs/roc/groups/5/isrania/onyea005/Israni4_Project_016
module load conda/python3 
conda activate /home/isrania/onyea005/.conda/envs/mpa 
for f in *.fastq.gz; do humann -i $f -o mission_subset --threads 8 --metaphlan-options "--read_min_len 49"; done
Here is the code I will be submitting for the demo dataset using Metaphlan4 (save it in NANO)
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/demo
module load conda/python3 
conda activate /home/isrania/onyea005/.conda/envs/mpa082023
humann --input demo.fastq.gz --output humann_demo --threads 4
	•	Then press Ctrl-X on your keyboard to Exit nano (type in pwd to determine the location of the sh file)
	•	nano will ask you if you want to save the modified file. Hit the y key (for "yes"). > Save the file as MISSION_TEST.sh (or whatever you chose o name)
	•	nano will then confirm if you want to save to the file name. Hit Enter to confirm this.
	•	Here partitionname is the name of the partition (queue) being submitted to, and scriptname is the name of the job script.  The -p partitionname portion of the command may be omitted, in which case the job would be submitted to whichever partition is set as the default.  Alternatively, the partition specification can be placed inside the job script
Going back to the HUMANN tutuorial, here is where we left offAlternatively, if you're comfortable with shell scripting, you can use the following command to process all six at once:

*Now this can be submitted (home and the /panfs/jay/groups/4/ strings seem to be interchangeable
sbatch -p partitionname scriptname   
cd /home/isrania/onyea005
sbatch -p amdsmall HUMANN_MISSION_MSI_SLURM.rtf   
sbatch -p amdsmall MISSION_TEST.sh
sbatch -p amdsmall MISSION_FULL.sh   
sbatch -p amdsmall humann_demo.sh
sbatch -p amdsmall humann_demo_2.sh   

Running HUMANN pulled the following error

CRITICAL ERROR: Error executing: /home/isrania/onyea005/.conda/envs/mpa082023/bin/metaphlan /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/demo/humann_demo/demo_humann_temp/tmpxoe3r59q/tmpbar25yd4 -t rel_ab -o /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/demo/humann_demo/demo_humann_temp/demo_metaphlan_bugs_list.tsv --input_type fastq --bowtie2out /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/demo/humann_demo/demo_humann_temp/demo_metaphlan_bowtie2.txt --nproc 4

Error message returned from metaphlan :
No MetaPhlAn BowTie2 database found (--index option)!
Expecting location bowtie2db
Exiting...





To view all of the jobs submitted by a particular user use the command:
squeue -u username
squeue -u onyea005
This command will display the status of the specified jobs, and the associated job ID numbers. The command squeue by itself will show all jobs on the system.

 To cancel a submitted job use the command:
scancel jobIDnumber
Here jobIDnumber should be replaced with the appropriate job ID number determined by using the squeue command.
https://ubccr.freshdesk.com/support/solutions/articles/5000686861-how-do-i-check-the-status-of-my-job-s-
squeue - Job States
R - Job is running on compute nodes
PD - Job is waiting on compute nodes
CG - Job is completing
squeue - Job Reasons
(Resources) - Job is waiting for compute nodes to become available
(Priority) - Jobs with higher priority are waiting for compute nodes.  Check this knowledge base article for info about job priority
(ReqNodeNotAvail) - The compute nodes requested by the job are not available for a variety of reasons, including:
cluster downtime
nodes offline
temporary scheduling backlog

HUMANN Analysis 082023 demo run V2 (fixing bowtie “No MetaPhlAn BowTie2 database found (--index option)!”

	•	Connect to VPN for MAC using Cisco AnyConnect Secure Mobility Client
	•	Connect to FileZilla to be able to see the files being moved and created in a dynamic fashion
	•	Enter sftp://login.msi.umn.edu into the Host box, your username into the Username box, your password into the Password box, and 22 into the Port box. Click on Quickconnect. (Tried using sftp://mesabi.msi.umn.edu, username and password since login node changes took place in Jaunary 2021)

	•	Connect to Mesabi (recommended default for MSI to be connected to the supercomputers)
Alternatively, connect to MSI using Terminal on Mac/Linux, located in ApplicationsUtilitiesTerminal.app. 
*NOTE: Like the login node Mesabi and Itasca have specific nomenclatures in the prompt. When connected to Mesabi your prompt will contain ln followed by some number when connected to Itasca your prompt will contain node followed by some numbers. This is a quick short cut to remind you which system you are connected to at any time.
**You need to be connected to Cisco Anyconnect via VPN in order for this command to work and it will ask you to login via DUO

Note to self: 
Running HUMANN pulled the following error
CRITICAL ERROR: Error executing: /home/isrania/onyea005/.conda/envs/mpa082023/bin/metaphlan /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/demo/humann_demo/demo_humann_temp/tmpxoe3r59q/tmpbar25yd4 -t rel_ab -o /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/demo/humann_demo/demo_humann_temp/demo_metaphlan_bugs_list.tsv --input_type fastq --bowtie2out /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/demo/humann_demo/demo_humann_temp/demo_metaphlan_bowtie2.txt --nproc 4
Error message returned from metaphlan :
No MetaPhlAn BowTie2 database found (--index option)!
Expecting location bowtie2db
Exiting...
Potential fixes were discussed here: 
https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4
Important! The MetaPhlAn 4 database has been substantially increased in comparison with the previous 3.1 version. Thus, for running MetaPhlAn 4, a minimum of 15GB or memory is needed.

If you have installed MetaPhlAn using Anaconda, it is advised to install the database in a folder outside the Conda environment. To do this, run
$ metaphlan --install --bowtie2db <database folder>

If you install the database in a different location, remember to run MetaPhlAn using --bowtie2db <database folder>!

By default, the latest MetaPhlAn database is downloaded and built. You can download a specific version with the --index parameter

$ metaphlan --install --index mpa_vJan21_CHOCOPhlAnSGB_202103 --bowtie2db <database folder>

When --index is specified, MetaPhlAn skips the check for the latest database version and run the analysis using the database version provided by --index located in --bowtie2db.

This option is recommended when MetaPhlAn is run on HPC clusters or containerized
If you have issues in downloading the database, you can get it from:
	•	Segatalab FTP

Just download the .tar, .md5, and the mpa_latest files and place them in the metaphlan_databases folder.





















Step 1: Check the installation instructions 

https://forum.biobakery.org/t/announcing-humann-3-5/3995

Step 2: Install Metaphlan 4.0

https://forum.biobakery.org/t/metaphlan4-and-the-new-version-humann/3985/4

https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4

Step 3: Install a new environment of Humann 3.0 (Which upgrades it to 3.5, 3.6 or 3.7)

https://forum.biobakery.org/t/announcing-humann-3-6-critical-update/4155

https://huttenhower.sph.harvard.edu/humann

Step 4: Test HUMANN

https://github.com/biobakery/biobakery/wiki/humann3











Step 1: Check the installation instructions 

https://forum.biobakery.org/t/announcing-humann-3-5/3995

Step 2: Install Metaphlan 4.0

https://forum.biobakery.org/t/metaphlan4-and-the-new-version-humann/3985/4

https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4

MetaPhlAn 4

Home

MetaPhlAn documentation
•	MetaPhlAn 4
•	MetaPhlAn 3.1
•	MetaPhlAn 3.0
•	MetaPhlAn2

StrainPhlAn documentation
•	StrainPhlAn 4
•	StrainPhlAn 3
•	StrainPhlAn
•	Strain Sharing Inference

Workshop on Genomics 2023
•	MetaPhlAn workshop
•	HUMAnN workshop


MetaPhlAn is a computational tool for profiling the composition of microbial communities (Bacteria, Archaea and Eukaryotes) from metagenomic shotgun sequencing data (i.e. not 16S) with species-level. With StrainPhlAn, it is possible to perform accurate strain-level microbial profiling.

MetaPhlAn 4 relies on ~5.1M unique clade-specific marker genes (the latest marker information file can be found here) identified from ~1M microbial genomes (~236,600 references and 771,500 metagenomic assembled genomes) spanning 26,970 species-level genome bins (SGBs, http://segatalab.cibio.unitn.it/data/Pasolli_et_al.html), 4,992 of them taxonomically unidentified at the species level (the full list of the species included in the latest database can be found here), allowing:

•	unambiguous taxonomic assignments;
•	an accurate estimation of organismal relative abundance;
•	SGB-level resolution for bacteria, archaea and eukaryotes;
•	strain identification and tracking
•	orders of magnitude speedups compared to existing methods.
•	metagenomic strain-level population genomics

ssh yourusername@login.msi.umn.edu (use ssh yourusername@mangi.msi.umn.edu or ssh yourusername@mesabi.msi.umn.edu respectively)
ssh onyea005@mesabi.msi.umn.edu
	•	Check the folder you are in
pwd
* my home folder is when I first login is “/home/prizm001/onyea005”
* /home/isrania/onyea005
*Change the folder to the israni MSI group
cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
cd /panfs/roc/groups/5/isrania/onyea005
cd /panfs/roc/groups/5/isrania/onyea005/Israni4_Project_016

Log in to an interactive node, making sure you have enough time and memory (https://www.msi.umn.edu/content/interactive-queue-use-isub)

isub -n nodes=1:ppn=8 -m 16GB -w 8:00:00	
#I don’t think I need to log into mesabi or Itasca to run this job if I am logged into the interactive node since it logs me into the lab queue (So use mesabi instead)
MSI's LabQi Cluster will be shut down and decommissioned during February Maintenance (February 5, 2020.) Several months ago, MSI released the Mesabi Interactive Queue as a replacement for the LabQi Cluster (ssh lab.)  The Mesabi Interactive Queue can be used in real-time for tasks such as interactive data exploration, creating plots or images, visualizations, or testing sections of code that will be used on other MSI queues.

https://www.msi.umn.edu/content/interactive-hpc

What is Interactive HPC?

MSI’s interactive HPC resources are composed of some of the same systems used to support our batch scheduled HPC activities. The main difference between the two services is the way in which users interact with the MSI resource. Interactive HPC systems allow real-time user inputs in order to facilitate code development, real-time data exploration, and visualizations. Interactive HPC systems are used when data are too large to download to a desktop or laptop, software is difficult or impossible to install on a personal machine, or specialized hardware resources (e.g., GPUs) are needed to visualize large datasets. User input might come through a command line interface (i.e., system shell) while debugging a piece of software or through mouse clicks while using a program with a graphical interface.

What can I do with Interactive HPC?

Unlike MSI’s other computing options, the Interactive HPC resources are designed only to be used in real time for tasks such as interactive data exploration, creating plots or images, visualizations, or testing sections of code that will be used on other MSI systems.

Common use cases include:
	•	Testing a compiled program at the command line before submitting it as a job
	•	Developing code or exploring data in an interactive environment like MATLAB, IPython, or Rstudio
	•	Visualizing data or building simulations in applications like Schrodinger, COMSOL, or Avizo

What types of Interactive HPC are available?
	•	srun:  Submit an interactive job on the current HPC system to provide a command line or graphical interface to interactively run software. 
	•	NICE: Connects a user to a graphical environment, which may have specialized GPUs for visualizing large datasets.
	•	Total Slots: 32 slots for accelerated remote desktops
	•	Total Memory:  Up to 16 GB/session
	•	Graphical Accelerators: Nvidia GK107GL
	•	CITRIX: Citrix for Windows Environments: Connects a user to a graphical Windows environment, where they can access specialized Windows software for modeling and data analysis.
	•	NX NoMachine: Connects a user to a graphical environment, which may be used as a lightweight remote desktop for connecting to other MSI resources.
	•	Jupyter Notebooks: A web portal for notebook-based computing in the browser, which can be used for reproducible and shareable data analysis, visualization, and scripted control of larger tasks. Currently supports Python 2, Python 3, and R. 
Here are several examples of how you can request an interactive HPC job on the new Mesabi Interactive Queue.

The basic syntax for submitting an interactive job on Mesabi (I assume that in combination with the new login on mesabi and mangi the syntax occurs after login into mesabi.msi.umn.edu):

qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I

The basic syntax for testing MPI code between 2 nodes:
qsub -l nodes=2:ppn=2,mem=8gb,walltime=12:00:00 -q interactive -W x=nmatchpolicy:exactnode -I

The basic syntax for creating a job with X-forwarding for GUI's and visulization:
qsub -X -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I

Back to the interactive login
qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I

The full specifications of the Mesabi Interactive Queue can be found on the MSI Queues webpage. Also, more examples on how to use qsub -I for interactive use can be found on the Interactive queue use with qsub webpage.
https://www.msi.umn.edu/content/interactive-queue-use-srun
** Will need to learn how to use Slurm to schedule jobs (https://www.msi.umn.edu/slurm)
Interactive queue use with srun
It is possible to start an interactive job on any of MSI’s clusters that support job submission. Interactive jobs can be useful for tasks including data exploration, development, or (with X11 forwarding) visualization activities.
The basic syntax for submitting an interactive job is illustrated using the following example for Mesabi:
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=1gb -t 1:00:00 -p interactive --pty bash
If you are submitting an interactive job from a session with X11 forwarding and would like to continue using X11 forwarding inside your job, you would add the additional flag ‘-x11’, as follows:
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=1gb -t 1:00:00 -p interactive --x11 --pty bash
The basic syntax for requesting two nodes, for instance to test MPI code between two nodes:
srun -N 2 --ntasks-per-node=1  --mem-per-cpu=1gb -t 1:00:00 -p interactive --pty bash
The syntax for requesting an interactive gpu node with a k40 GPU is:
srun -n 12 -t 1:00:00 -p interactive-gpu --gres=gpu:k40:1 --pty bash
You may also submit an interactive job using an interactive submission script. For instance, to submit an interactive job to test an MPI code (in our case named ‘interactive.sh’) with contents:
#SBATCH -N 2
#SBATCH --ntasks-per-node=1
#SBATCH --mem-per-cpu=1gb
#SBATCH -t 1:00:00
#SBATCH -p interactive
./my_application
You would then submit this job using:
sbatch interactive.sh
Interactive jobs are subject to the same rules and limitations as batch jobs. Interactive jobs have access to only those partitions that are available on the cluster they are submitted from. Jobs that request more resources may wait in the queue longer, so be sure that you are requesting only those resources that you need.
 An interactive job supports all of the usual Slurm commands. These commands are included as flags when requesting an interactive job. 
 Example interactive srun Session
 username@mydesktop$ ssh -Y msiusername@mesabi.msi.umn.edu
Password:
Last login: Tue May 10 15:16:06 2018 from mydesktop.mydept.umn.edu
-------------------------------------------------------------------------------
            University of Minnesota Supercomputing Institute
                                Mesabi
                        HP Haswell Linux Cluster
-------------------------------------------------------------------------------
[... output truncated ...]
---------------
username@ln0006 [~] % srun -N 2 --ntasks-per-node=1  --mem-per-cpu=1gb --x11 -t 1:00:00 --pty bash
username@cn0001 [~] % 
username@cn0001 [~] % echo $SLURM_JOB_NODELIST
cn[0001-0002]
username@cn0001 [~] %
***Note that this interactive job is using two nodes (-N 2). At this point you are currently in your home directory, and can load modules and launch software, including GUI software.

Back to the interactive login using srun (might take a few mins to get the access)

*Old syntax
qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I
*New syntax
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=16gb -t 4:00:00 -p interactive --pty bash
Now you can look at the contents of the directory using the ls command:

ls -lh
You can always check the location of your current directory (aka folder) using the pwd command.  If you type:

pwd
Login into an interactive node to process the data
*I will try to add a CPU per task option to the interactive run and to the sbatch file
https://www.msi.umn.edu/support/faq/how-can-i-use-gnu-parallel-run-lot-commands-parallel
https://www.msi.umn.edu/content/interactive-queue-use-srun
Interactive queue use with srun
It is possible to start an interactive job on any of MSI’s clusters that support job submission. Interactive jobs can be useful for tasks including data exploration, development, or (with X11 forwarding) visualization activities.
 The basic syntax for submitting an interactive job is illustrated using the following example for Mesabi:
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=1gb -t 1:00:00 -p interactive --pty bash
If you are submitting an interactive job from a session with X11 forwarding and would like to continue using X11 forwarding inside your job, you would add the additional flag ‘-x11’, as follows:
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=1gb -t 1:00:00 -p interactive --x11 --pty bash
The basic syntax for requesting two nodes, for instance to test MPI code between two nodes:
srun -N 2 --ntasks-per-node=1  --mem-per-cpu=1gb -t 1:00:00 -p interactive --pty bash
The syntax for requesting an interactive gpu node with a k40 GPU is:
srun -n 12 -t 1:00:00 -p interactive-gpu --gres=gpu:k40:1 --pty bash
https://stackoverflow.com/questions/51139711/hpc-cluster-select-the-number-of-cpus-and-threads-in-slurm-sbatch
The terminology used in the sbatch man page might be a bit confusing. Thus, I want to be sure I am getting the options set right. Suppose I have a task to run on a single node with N threads. Am I correct to assume that I would use --nodes=1 and --ntasks=N?
I am used to thinking about using, for example, pthreads to create N threads within a single process. Is the result of that what they refer to as "cores" or "cpus per task"? CPUs and threads are not the same things in my mind.

Depending on the parallelism you are using: distributed or shared memory
--ntasks=# : Number of "tasks" (use with distributed parallelism).
--ntasks-per-node=# : Number of "tasks" per node (use with distributed parallelism).
--cpus-per-task=# : Number of CPUs allocated to each task (use with shared memory parallelism).

From this question: if every node has 24 cores, is there any difference between these commands?
sbatch --ntasks 24 [...]
sbatch --ntasks 1 --cpus-per-task 24 [...]
Answer: (by Matthew Mjelde)
Yes there is a difference between those two submissions. You are correct that usually ntasks is for mpi and cpus-per-task is for multithreading, but let’s look at your commands:
For your first example, the sbatch --ntasks 24 […] will allocate a job with 24 tasks. These tasks in this case are only 1 CPUs, but may be split across multiple nodes. So you get a total of 24 CPUs across multiple nodes.
For your second example, the sbatch --ntasks 1 --cpus-per-task 24 [...] will allocate a job with 1 task and 24 CPUs for that task. Thus you will get a total of 24 CPUs on a single node.
In other words, a task cannot be split across multiple nodes. Therefore, using --cpus-per-task will ensure it gets allocated to the same node, while using --ntasks can and may allocate it to multiple nodes.

Another good Q&A from CÉCI's support website: Suppose you need 16 cores. Here are some use cases:
	•	you use mpi and do not care about where those cores are distributed: --ntasks=16
	•	you want to launch 16 independent processes (no communication): --ntasks=16
	•	you want those cores to spread across distinct nodes: --ntasks=16 and --ntasks-per-node=1 or --ntasks=16 and --nodes=16
	•	you want those cores to spread across distinct nodes and no interference from other jobs: --ntasks=16 --nodes=16 --exclusive
	•	you want 16 processes to spread across 8 nodes to have two processes per node: --ntasks=16 --ntasks-per-node=2
	•	you want 16 processes to stay on the same node: --ntasks=16 --ntasks-per-node=16
	•	you want one process that can use 16 cores for multithreading: --ntasks=1 --cpus-per-task=16
	•	you want 4 processes that can use 4 cores each for multithreading: --ntasks=4 --cpus-per-task=4

https://stackoverflow.com/questions/49466020/specify-number-of-cpus-for-a-job-on-slurm
https://researchcomputing.princeton.edu/support/knowledge-base/scaling-analysis
https://docs-research-it.berkeley.edu/services/high-performance-computing/user-guide/running-your-jobs/scheduler-examples/
Back to the interactive login using srun (might take a few mins to get the access)

https://www.msi.umn.edu/content/interactive-queue-use-srun
*Old syntax
qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I
*New syntax
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=16gb -t 4:00:00 -p interactive --pty bash

*Testing out adding multiple CPUs to run the code faster (https://www.msi.umn.edu/support/faq)
It seems like nodes and CPU is different so the n-task per node is what determine thee number of CPUs we can use?
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=32gb -t 4:00:00 -p interactive --pty bash
*I will use this code to ask for multiple CPUs for the analysis)https://stackoverflow.com/questions/51139711/hpc-cluster-select-the-number-of-cpus-and-threads-in-slurm-sbatch
--ntasks=4 --cpus-per-task=4

srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=32gb -t 4:00:00 -p interactive --pty bash

*Per the SLURM PDF guide (See slide 10) the interactive queue has a maximum of 4 cores/CPU that can be requested through srun. I will request 8 when submitting a job via SLURM
srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=32gb -t 4:00:00 -p interactive --pty bash

srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=8gb -t 4:00:00 -p interactive --pty bash

*Setting the srun to 16GB since metaphlan 4 mentioned that it needs 15GB to run
srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=16gb -t 4:00:00 -p interactive --pty bash
Change the folder to the location with the metatranscriptomics data
**I created a new folder called HUMANN082023 for this new analysis via Filezilla

/home/isrania/onyea005/Israni4_Project_016
cd /home/isrania/onyea005/Israni4_Project_016
cd /home/isrania/onyea005/Israni4_016_test

pwd
cd /home/isrania/onyea005/HUMANN082023

Back to https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4

Pre-requisites

MetaPhlAn requires python 3 or newer with numpy, and Biopython libraries installed. Python libraries are automatically installed by pip. MetaPhlAn relies on BowTie2 (version 2.3 or higher) to map reads against marker genes. Check that bowtie2 is present in the system path with execute and read permissions.

If MetaPhlAn is installed using conda, no pre-requisites are needed.

MetaPhlAn is integrated with advanced heatmap plotting with hclust2 and cladogram visualization with GraPhlAn. If you use such visualization tools please refer to their prerequisites.

The best way to install MetaPhlAn is through conda via the Bioconda channel. If you have not configured your Anaconda installation to fetch packages from Bioconda, please follow these steps to set up the channels.

You can install MetaPhlAn by running
$ conda install -c bioconda metaphlan

It is recommended to create an isolated conda environment and install MetaPhlAn into it.
$ conda create --name mpa -c bioconda python=3.7 metaphlan

If during the installation you encounter an incompatibility error with the glibc package, we suggest you to add the conda-forge channel to conda or run one of the following commands.
$ conda install -c conda-forge -c bioconda metaphlan
$ conda create --name mpa -c conda-forge -c bioconda python=3.7 metaphlan

This allows having the correct version of all the dependencies isolated from the system's python installation.

Before using MetaPhlAn, you should activate the mpa environment:
$ conda activate mpa
	•	Load the conda module
module avail conda
**Under mesabi
module load conda/python3
Activate the conda environment
Now that you have an environment for HUMANN, activate it using the environment’s name:
After the update, I had to use the full name to active conda environments (https://stackoverflow.com/questions/58369030/could-not-find-conda-environment)
conda info --envs
	•	Create a new conda environment for metaphlan 4
conda create --name mpa -c bioconda python=3.7 metaphlan
pwd
conda create --name mpa082023 -c conda-forge -c bioconda python=3.7 metaphlan
conda create --name mpa082023V2 -c conda-forge -c bioconda python=3.7 metaphlan




** I was getting the following error using the original analysis 082023 guidelines on MSI, likely because metaphlan 4 was not pointing to the right database

Downloading MetaPhlAn database
Please note due to the size this might take a few minutes

\Downloading and uncompressing indexes

File /home/isrania/onyea005/.conda/envs/mpa082023/lib/python3.7/site-packages/metaphlan/metaphlan_databases/mpa_vOct22_CHOCOPhlAnSGB_202212_bt2.tar already present!

File /home/isrania/onyea005/.conda/envs/mpa082023/lib/python3.7/site-packages/metaphlan/metaphlan_databases/mpa_vOct22_CHOCOPhlAnSGB_202212_bt2.md5 already present!
MD5 checksums do not correspond! If this happens again, you should remove the database files and rerun MetaPhlAn so they are re-downloaded
https://forum.biobakery.org/t/metaphlan-3-md5-error/484
https://forum.biobakery.org/t/humann3-metaphlan-3-0-error-md5-checksums-do-not-match/619/4
https://github.com/biobakery/MetaPhlAn/issues/100
https://groups.google.com/g/metaphlan-users/c/5ltD8X8Xitc?pli=1

*First I will try to reinstall metaphlan within the environment

https://forum.biobakery.org/t/announcing-humann-3-5/3995
	•	Upgrading MetaPhlAn:
conda install -c bioconda metaphlan=4.0.0

**Skip the part below for now:

MetaPhlAn is also available in PyPi
$ pip install metaphlan

Alternatively, you can manually download from GitHub or clone the repository using the following command
$ git clone https://github.com/biobakery/MetaPhlAn.git

and install MetaPhlAn by running
$ pip install .

If you choose this way, you'll need to install manually some dependencies!
MetaPhlAn needs the clade markers and the database to be downloaded locally. To obtain them:
$ metaphlan --install 

Important! The MetaPhlAn 4 database has been substantially increased in comparison with the previous 3.1 version. Thus, for running MetaPhlAn 4, a minimum of 15GB or memory is needed.

If you have installed MetaPhlAn using Anaconda, it is advised to install the database in a folder outside the Conda environment. To do this, run
$ metaphlan --install --bowtie2db <database folder>

If you install the database in a different location, remember to run MetaPhlAn using --bowtie2db <database folder>!

By default, the latest MetaPhlAn database is downloaded and built. You can download a specific version with the --index parameter

$ metaphlan --install --index mpa_vJan21_CHOCOPhlAnSGB_202103 --bowtie2db <database folder>

When --index is specified, MetaPhlAn skips the check for the latest database version and run the analysis using the database version provided by --index located in --bowtie2db.

This option is recommended when MetaPhlAn is run on HPC clusters or containerized
If you have issues in downloading the database, you can get it from:
	•	Segatalab FTP

Just download the .tar, .md5, and the mpa_latest files and place them in the metaphlan_databases folder.

Basic Usage

Important! The MetaPhlAn 4 database has been substantially increased in comparison with the previous 3.1 version. Thus, for running MetaPhlAn 4, a minimum of 15GB or memory is needed.
$ metaphlan metagenome.fastq --input_type fastq -o profiled_metagenome.txt

It is highly recommended to save the intermediate BowTie2 output for re-running MetaPhlAn extremely quickly (--bowtie2out), and use multiple CPUs (--nproc) if available:
$ metaphlan metagenome.fastq --bowtie2out metagenome.bowtie2.bz2 --nproc 5 --input_type fastq -o profiled_metagenome.txt

If you already mapped your metagenome against the marker DB (using a previous MetaPhlAn run), you can obtain the results in few seconds by using the previously saved --bowtie2out file and specifying the input (--input_type bowtie2out):
$ metaphlan metagenome.bowtie2.bz2 --nproc 5 --input_type bowtie2out -o profiled_metagenome.txt

bowtie2out files generated with MetaPhlAn versions below 3.0 are not compatible. Starting from MetaPhlAn 3, the BowTie2 output now includes the size of the profiled metagenome.
You can also provide an externally BowTie2-mapped SAM if you specify this format with --input_type. Two steps here: first map your metagenome with BowTie2 and then feed MetaPhlAn with the obtained SAM:
$ bowtie2 --sam-no-hd --sam-no-sq --no-unal --very-sensitive -S metagenome.sam -x metaphlan_databases/mpa_vJan21_CHOCOPhlAnSGB_202103  -U metagenome.fastq
$ metaphlan metagenome.sam --input_type sam -o profiled_metagenome.txt

MetaPhlAn can also natively handle paired-end metagenomes (but does not use the paired-end information), and, more generally, metagenomes stored in multiple files (but you need to specify the --bowtie2out parameter):
$ metaphlan metagenome_1.fastq,metagenome_2.fastq --bowtie2out metagenome.bowtie2.bz2 --nproc 5 --input_type fastq -o profiled_metagenome.txt

Starting from version 3, MetaPhlAn can estimate the unclassified fraction of the metagenome. The relative abundance profile is scaled according to the percentage of reads mapping to a clade in the database.
$ metaphlan metagenome.fastq --bowtie2out metagenome.bowtie2.bz2 --nproc 5 --input_type fastq --unclassified_estimation -o profiled_metagenome.txt

If you want to estimate the unknown fraction of a metagenome and your input file is a SAM file, remember to specify the metagenome size using --nreads. You can get easily get the metagenome size from a SAM file if you have run bowtie2 without the --no-unal parameter by running
$ wc -l metagenome.sam

Otherwise, read_fastx.py is your choice: this will print on the standard error the metagenome size.
$ read_fastx.py metagenome.fastq > /dev/null

You can provide the specific database version with --index.

By default MetaPhlAn is run with --index latest: the latest version of the database is used; if it is not available, MetaPhlAn will try to download it.

When --index is specified, MetaPhlAn skips the check for the latest database version and run the analysis using the database version provided by --index located in --bowtie2db.

For advanced options and other analysis types (such as strain tracking) please refer to the full command-line options metaphlan --help.

Utility Scripts

MetaPhlAn's repository features a few utility scripts to aid in the manipulation of sample output and its visualization. These scripts can be found under the utils folder in the MetaPhlAn directory.

Merging Tables
The script merge_metaphlan_tables.py allows to combine MetaPhlAn output from several samples to be merged into one table Bugs (rows) vs Samples (columns) with the table enlisting the relative normalized abundances per sample per bug.
To merge multiple output files, run the script as below
$ merge_metaphlan_tables.py metaphlan_output1.txt metaphlan_output2.txt > metaphlan_output3.txt output/merged_abundance_table.txt
Wildcards can be used as needed:
$ merge_metaphlan_tables.py metaphlan_output*.txt > output/merged_abundance_table.txt
Output files can be merged only if the profiling was performed with the same version of the MetaPhlAn database.

There is no limit to how many files you can merge.

Converting SGB profiles to the GTDB taxonomy
The script sgb_to_gtdb_profile.py allows to convert a SGB-based MetaPhlAn 4 output into a GTDB-taxonomy-based profile.

To do so, run the script as below
$ sgb_to_gtdb_profile.py -i metaphlan_output.txt -o metaphlan_output_gtdb.txt

Alpha and beta diversity calculation

The script calculate_diversity.R allows to compute alpha and/or beta diversity, with different metrics of choice, starting from a merged MetaPhlAn table. Available alpha-diversity metrics are richness, shannon, simpson, and gini. Available beta-diversity distance functions are bray-curtis, jaccard, weighted-unifrac, unweighted-unifrac, centered log-ratio, and aitchison. For example, to generate a beta diversity distance matrix with bray-curtis, you need to run the script as below:
Rscript calculate_diversity.R -f merged_mpa4_profiles.tsv -d beta -m bray-curtis

To compute UniFrac distances, the SGB tree in the Newick format (available here) must be provided.

For the full list of options, please run:
Rscript calculate_diversity.R

Heatmap Visualization

The hclust2 script generates a hierarchically-clustered heatmap from MetaPhlAn abundance profiles. To generate the heatmap for a merged MetaPhlAn output table (as described above), you need to run the script as below:

hclust2.py \
  -i HMP.species.txt \
  -o HMP.sqrt_scale.png \
  --skip_rows 1 \
  --ftop 50 \
  --f_dist_f correlation \
  --s_dist_f braycurtis \
  --cell_aspect_ratio 9 \
  -s --fperc 99 \
  --flabel_size 4 \
  --metadata_rows 2,3,4 \
  --legend_file HMP.sqrt_scale.legend.png \
  --max_flabel_len 100 \
  --metadata_height 0.075 \
  --minv 0.01 \
  --no_slabels \
  --dpi 300 \
  --slinkage complete

GraPhlAn Visualization

The tutorial of using GraPhlAn can be found from the GraPhlAn wiki.

Customizing the database

In order to add a marker to the database, the user needs the following steps:
	•	Reconstruct the marker sequences (in fasta format) from the MetaPhlAn BowTie2 database by:
bowtie2-inspect metaphlan_databases/mpa_vJan21_CHOCOPhlAnSGB_202103 > metaphlan_databases/mpa_vJan21_CHOCOPhlAnSGB_202103_markers.fasta
	•	Add the marker sequence stored in a file new_marker.fasta to the marker set:
cat new_marker.fasta >> metaphlan_databases/mpa_vJan21_CHOCOPhlAnSGB_202103_markers.fasta
	•	Rebuild the bowtie2 database:
bowtie2-build metaphlan_databases/mpa_vJan21_CHOCOPhlAnSGB_202103_markers.fasta metaphlan_databases/mpa_vJan21_CHOCOPhlAnSGB_NEW
	•	Assume that the new marker was extracted from genome1, genome2. Update the taxonomy file from the Python console as follows:
import pickle
import bz2

db = pickle.load(bz2.open('metaphlan_databases/mpa_vJan21_CHOCOPhlAnSGB_202103.pkl', 'r'))

# Add the taxonomy of the new genomes
db['taxonomy']['7-levels taxonomy with clade names of genome1'] = ('7-levels NCBI taxonomy id of genome1', length of genome1)
db['taxonomy']['7-levels taxonomy with clade names of genome2'] = ('7-levels NCBI taxonomy id of genome1', length of genome2)

# Add the information of the new marker as the other markers
db['markers'][new_marker_name] = {
                                   'clade': the clade that the marker belongs to,
                                   'ext': {the GCA of the first external genome where the marker appears,
                                           the GCA of the second external genome where the marker appears,
                                          },
                                   'len': length of the marker,
                                   'taxon': the taxon of the marker
                                }
                                   
To see an example, try to print the first marker information:
 print list(db['markers'].items())[0]

# Save the new mpa_pkl file
with bz2.BZ2File('metaphlan_databases/mpa_vJan21_CHOCOPhlAnSGB_NEW.pkl', 'w') as ofile:
    pickle.dump(db, ofile, pickle.HIGHEST_PROTOCOL)

	•	To use the new database, remember to run metaphlan.py with the "--index mpa_vJan21_CHOCOPhlAnSGB_NEW" parameter.

Step 3: Install a new environment of Humann 3.0

https://huttenhower.sph.harvard.edu/humann


Step 4: Upgrade Humann to version 3.5/3.6
https://forum.biobakery.org/t/announcing-humann-3-5/3995

https://forum.biobakery.org/t/announcing-humann-3-6-critical-update/4155

**It looks like a new installation of HUMANN 3.0 will update it to 3.6. I will install it within the same environment as metaphlan 4.0

Getting HUMAnN 3.0 (and MetaPhlAn 3.0)
Install via conda
	•	(Optionally) Create a new conda environment for the installation
	•	conda create --name biobakery3 python=3.7
	•	conda activate biobakery3
	•	(If you haven't already) Set conda channel priority:
	•	conda config --add channels defaults
	•	conda config --add channels bioconda
	•	conda config --add channels conda-forge
	•	conda config --add channels biobakery
	•	Install HUMAnN 3.0 software with demo databases:
	•	conda install humann -c biobakery
	•	Conda-installing HUMAnN 3.0 will automatically install MetaPhlAn 3.0.
	•	To install only MetaPhlAn 3.0 execute:
conda install metaphlan -c bioconda.

After the update, I had to use the full name to active conda environments (https://stackoverflow.com/questions/58369030/could-not-find-conda-environment)
conda info --envs
source activate /Users/guillaumeonyeaghala/miniconda2/envs/mpa

/home/isrania/onyea005/.conda/envs/mpa
conda activate /home/isrania/onyea005/.conda/envs/mpa
conda activate /home/isrania/onyea005/.conda/envs/mpa082023

conda activate /home/isrania/onyea005/.conda/envs/mpa082023
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V2

conda init --help
conda init bash

conda activate /home/isrania/onyea005/.conda/envs/mpa082023

*Set up the channel priority

conda config --add channels defaults
conda config --add channels bioconda	
conda config --add channels conda-forge
conda config --add channels biobakery

*Install HUMANN 3.0 (Technically 3.6?) within the same environment which should have metaphlan 4.0
conda install humann -c biobakery

The step below to download the databases  did not work, so I checked the specific error online “subprocess.CalledProcessError: Command '['bowtie2-build', '--usage']' returned non-zero exit status 250.” As seen below, I had to first load the MPA conda environment, uninstall bowtie, reinstall bowtie and finally install metaphlan for things to play nice.

https://forum.biobakery.org/t/cant-get-metaphlan-to-complete-within-humann-3/1834
https://forum.biobakery.org/t/cant-get-metaphlan-to-complete-within-humann-3/1834/4
https://forum.biobakery.org/t/humann-3-cannot-call-bowtie2-version/576/17

If you installed bowtie2 with conda you could try:
conda remove bowtie2
conda install bowtie2
bowtie2-build --usage
removing bowtie also removed metaphlan. 

You can install MetaPhlAn by running

conda install -c bioconda metaphlan
conda install -c biobakery humann
MetaPhlAn needs the clade markers and the database to be downloaded locally. To obtain them:

metaphlan --install 

pwd
cd /home/isrania/onyea005/HUMANN082023
conda install -c bioconda metaphlan=4.0.0

*First I will try to reinstall metaphlan within the environment

https://forum.biobakery.org/t/announcing-humann-3-5/3995
	•	Upgrading MetaPhlAn:
conda install -c bioconda metaphlan=4.0.0

If you have installed MetaPhlAn using Anaconda, it is advised to install the database in a folder outside the Conda environment. To do this, run

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
metaphlan --install --bowtie2db </Volumes/RAVPOWER/ROOK_PC/HUMANN/metaphlan_db>
metaphlan --install --bowtie2db /Volumes/RAVPOWER/ROOK_PC/HUMANN/metaphlan_db

cd /home/isrania/onyea005/HUMANN
metaphlan --install --bowtie2db /home/isrania/onyea005/HUMANN/metaphlan_db
If you install the database in a different location, remember to run MetaPhlAn using --bowtie2db <database folder>!

By default, the latest MetaPhlAn database is downloaded and built. You can download a specific version with the --index parameter

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
metaphlan --install --index mpa_v30_CHOCOPhlAn_201901 --bowtie2db </Volumes/RAVPOWER/ROOK_PC/HUMANN/metaphlan_db>

metaphlan --install --index mpa_v30_CHOCOPhlAn_201901 --bowtie2db /Volumes/RAVPOWER/ROOK_PC/HUMANN/metaphlan_db2

cd /home/isrania/onyea005/HUMANN
metaphlan --install --index mpa_v30_CHOCOPhlAn_201901 --bowtie2db /home/isrania/onyea005/HUMANN/metaphlan_db2
When --index is specified, MetaPhlAn skips the check for the latest database version and run the analysis using the database version provided by --index located in --bowtie2db. [the link was found at https://drive.google.com/drive/folders/1_HaY16mT7mZ_Z8JtesH8zCfG9ikWcLXG]

https://github.com/biobakery/humann

5. Download the databases

Downloading the databases is a required step if your input is a filtered shotgun sequencing metagenome file (fastq, fastq.gz, fasta, or fasta.gz format). If your input files will always be mapping results files (sam, bam or blastm8 format) or gene tables (tsv or biom format), you do not need to download the ChocoPhlAn and translated search databases.

Download the ChocoPhlAn database

Download the ChocoPhlAn database providing $INSTALL_LOCATION as the location to install the database (approximate size = 16.4 GB).

humann_databases --download chocophlan full $INSTALL_LOCATION

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_databases --download chocophlan full $cd /Volumes/RAVPOWER/ROOK_PC/HUMANN/Databases

cd /Volumes/RAVPOWER2/ROOK_PC/Israni4_Project_016
humann_databases --download chocophlan full $cd /Volumes/RAVPOWER2/ROOK_PC/HUMANN/Databases

cd /panfs/roc/groups/5/isrania/onyea005/HUMANN/Databases
humann_databases --download chocophlan full $cd /panfs/roc/groups/5/isrania/onyea005/HUMANN/Databases

NOTE: The humann config file will be updated to point to this location for the default chocophlan database. If you move this database, please use the "humann_config" command to update the default location of this database. Alternatively you can always provide the location of the chocophlan database you would like to use with the "--nucleotide-database " option to humann.

Download a translated search database

While there is only one ChocoPhlAn database, users have a choice of translated search databases which offer trade-offs between resolution, coverage, and performance. For help selecting a translated search database, see the following guides:

Selecting a level of gene family resolution
Selecting a scope for translated search

Download a translated search database providing $INSTALL_LOCATION as the location to install the database:

To download the full UniRef90 database (20.7GB, recommended):

humann_databases --download uniref uniref90_diamond $INSTALL_LOCATION

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_databases --download uniref uniref90_diamond $cd /Volumes/RAVPOWER/ROOK_PC/HUMANN/Databases

cd /Volumes/RAVPOWER2/ROOK_PC/Israni4_Project_016
humann_databases --download uniref uniref90_diamond $cd /Volumes/RAVPOWER2/ROOK_PC/HUMANN/Databases
To download the EC-filtered UniRef90 database (0.9GB):

humann_databases --download uniref uniref90_ec_filtered_diamond $INSTALL_LOCATION

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_databases --download uniref uniref90_ec_filtered_diamond $cd /Volumes/RAVPOWER/ROOK_PC/HUMANN/Databases

cd /Volumes/RAVPOWER2/ROOK_PC/Israni4_Project_016
humann_databases --download uniref uniref90_ec_filtered_diamond $cd /Volumes/RAVPOWER2/ROOK_PC/HUMANN/Databases

cd /panfs/roc/groups/5/isrania/onyea005/HUMANN/Databases
humann_databases --download uniref uniref90_ec_filtered_diamond $cd /panfs/roc/groups/5/isrania/onyea005/HUMANN/Databases
To download the full UniRef50 database (6.9GB):

humann_databases --download uniref uniref50_diamond $INSTALL_LOCATION

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_databases --download uniref uniref50_diamond $cd /Volumes/RAVPOWER/ROOK_PC/HUMANN/Databases
To download the EC-filtered UniRef50 database (0.3GB):

humann_databases --download uniref uniref50_ec_filtered_diamond $INSTALL_LOCATION

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_databases --download uniref uniref50_ec_filtered_diamond $cd /Volumes/RAVPOWER/ROOK_PC/HUMANN/Databases

NOTE: The humann config file will be updated to point to this location for the default uniref database. If you move this database, please use the humann_config command to update the default location of this database. Alternatively you can always provide the location of the uniref database you would like to use with the --protein-database <uniref> option to humann.

NOTE: By default HUMAnN 3.0 runs DIAMOND for translated alignment. If you would like to use RAPSearch2 for translated alignment, first download the RAPSearch2 formatted database by running this command with the rapsearch2 formatted database selected. It is suggested that you install both databases in the same folder so this folder can be the default uniref database location. This will allow you to switch between alignment software without having to specify a different location for the database.
Installation Update
If you have already installed HUMAnN 3.0, using the Initial Installation steps, and would like to upgrade your installed version to the latest version, please follow these steps.

Download HUMAnN 3.0
Install HUMAnN 3.0

Since you have already downloaded the databases in the initial installation, you do not need to download the databases again unless there are new versions available. However, you will want to update your latest HUMAnN 3.0 install to point to the databases you have downloaded as by default the new install configuration will point to the demo databases.

To update your HUMAnN 3.0 configuration file to include the locations of your downloaded databases, please use the following steps.

Update the location of the ChocoPhlAn database (replacing $DIR with the full path to the directory containing the ChocoPhlAn database)


cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_config --update database_folders nucleotide $DIR
humann_config --update database_folders nucleotide /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases
humann_config --update database_folders nucleotide /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases/chocophlan

Update the location of the UniRef database (replacing $DIR with the full path to the directory containing the UniRef database)


cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_config --update database_folders protein $DIR

humann_config --update database_folders protein /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases/uniref

Please note, after a new installation, all of the settings in the configuration file, like the database folders, will be reset to the defaults. If you have any additional settings that differ from the defaults, please update them at this time.

For more information on the HUMAnN 3.0 configuration file, please see the Configuration section.

Test your installation
	•	
	•	Run HUMAnN unit tests:
	•	humann_test (~1 minute)
	•	Run the HUMAnN demo:
	•	humann -i demo.fastq -o sample_results
	•	Note: MetaPhlAn will fetch and build its marker database during this process, which takes a few minutes (but only has to be done once).
*Testing the installation
humann_test
metaphlan --version
Upgrading your databases
*I created a new folder for the database via filezilla
/panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases

https://huttenhower.sph.harvard.edu/humann
https://forum.biobakery.org/t/announcing-humann-3-5/3995
https://forum.biobakery.org/t/announcing-humann-3-6-critical-update/4155

The HUMAnN installation comes with small sequence and annotation databases for testing/tutorial purposes.
*I created a new folder for the database via filezilla (The very last step was aborted, will need to check if I need to redownload)
/panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases
metaphlan --install --bowtie2db <database folder>
metaphlan --install --bowtie2db /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases
	•	To upgrade your pangenome database:
	•	humann_databases --download chocophlan full /path/to/databases --update-config yes
	•	To upgrade your protein database:
	•	humann_databases --download uniref uniref90_diamond /path/to/databases --update-config yes
	•	To upgrade your annotations database:
	•	humann_databases --download utility_mapping full /path/to/databases --update-config yes
	•	To profile a sample using updated databases:
	•	humann -i sample_reads.fastq -o sample_results

*I created a new folder for the database via filezilla
/panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases2
To upgrade your pangenome database
humann_databases --download chocophlan full /path/to/databases --update-config yes
humann_databases --download chocophlan full /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases --update-config yes

To upgrade your protein database
humann_databases --download uniref uniref90_diamond /path/to/databases --update-config yes
humann_databases --download uniref uniref90_diamond /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases --update-config yes

To upgrade your annotations database
humann_databases --download utility_mapping full /path/to/databases --update-config yes
humann_databases --download utility_mapping full /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases --update-config yes
https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4
Important! The MetaPhlAn 4 database has been substantially increased in comparison with the previous 3.1 version. Thus, for running MetaPhlAn 4, a minimum of 15GB or memory is needed.

If you have installed MetaPhlAn using Anaconda, it is advised to install the database in a folder outside the Conda environment. To do this, run
$ metaphlan --install --bowtie2db <database folder>

If you install the database in a different location, remember to run MetaPhlAn using --bowtie2db <database folder>!

By default, the latest MetaPhlAn database is downloaded and built. You can download a specific version with the --index parameter

$ metaphlan --install --index mpa_vJan21_CHOCOPhlAnSGB_202103 --bowtie2db <database folder>

When --index is specified, MetaPhlAn skips the check for the latest database version and run the analysis using the database version provided by --index located in --bowtie2db.

This option is recommended when MetaPhlAn is run on HPC clusters or containerized
If you have issues in downloading the database, you can get it from:
	•	Segatalab FTP

Just download the .tar, .md5, and the mpa_latest files and place them in the metaphlan_databases folder. (I think you need to use metaphlan --help , then look at the bowtie2DB path location

https://forum.biobakery.org/t/announcing-humann-3-5/3995

How to upgrade from earlier versions of MetaPhlAn and HUMAnN 3:
	•	Upgrading MetaPhlAn:
	•	$ conda install -c bioconda metaphlan=4.0.0
	•	Upgrading HUMAnN software:
	•	$ pip install humann --upgrade
	•	You DO NOT need to re-download the latest (v3.1) pangenome database, the DIAMOND-formatted UniRef90/50 databases, or the accessory mapping files to use HUMAnN 3.5. You can point your HUMAnN 3.5 installation to the locations of these existing files using the humann_config script.

If you have installed MetaPhlAn using Anaconda, it is advised to install the database in a folder outside the Conda environment. To do this, run

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
metaphlan --install --bowtie2db </Volumes/RAVPOWER/ROOK_PC/HUMANN/metaphlan_db>
metaphlan --install --bowtie2db /Volumes/RAVPOWER/ROOK_PC/HUMANN/metaphlan_db

cd /home/isrania/onyea005/HUMANN
metaphlan --install --bowtie2db /home/isrania/onyea005/HUMANN/metaphlan_db

pwd
cd /home/isrania/onyea005/HUMANN082023/Databases2
metaphlan --install --bowtie2db /home/isrania/onyea005/HUMANN082023/Databases2/metaphlan_db

If you install the database in a different location, remember to run MetaPhlAn using --bowtie2db <database folder>!

*I created a new folder for the database via filezilla. Updating the metaphlan and Humann databases in a single command overnight.
/panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases2
cd /home/isrania/onyea005/HUMANN082023/Databases2
To upgrade your pangenome database
humann_databases --download chocophlan full /path/to/databases --update-config yes
humann_databases --download chocophlan full /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases --update-config yes
humann_databases --download chocophlan full /home/isrania/onyea005/HUMANN082023/Databases2 --update-config yes
To upgrade your protein database
humann_databases --download uniref uniref90_diamond /path/to/databases --update-config yes
humann_databases --download uniref uniref90_diamond /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases --update-config yes
humann_databases --download uniref uniref90_diamond /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases --update-config yes

To upgrade your annotations database
humann_databases --download utility_mapping full /path/to/databases --update-config yes
humann_databases --download utility_mapping full /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases --update-config yes
humann_databases --download utility_mapping full /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases --update-config yes

pwd
cd /home/isrania/onyea005/HUMANN082023/Databases2
metaphlan --install --bowtie2db /home/isrania/onyea005/HUMANN082023/Databases2/metaphlan_db
humann_databases --download chocophlan full /home/isrania/onyea005/HUMANN082023/Databases2 --update-config yes
humann_databases --download uniref uniref90_diamond /home/isrania/onyea005/HUMANN082023/Databases2 --update-config yes
humann_databases --download utility_mapping full /home/isrania/onyea005/HUMANN082023/Databases2 --update-config yes

*This is the code report in case I need to look at t again

(base) [onyea005@ln0004 ~]$ module load conda/python3
(base) [onyea005@ln0004 ~]$ conda activate /home/isrania/onyea005/.conda/envs/mpa082023v2
Not a conda environment: /home/isrania/onyea005/.conda/envs/mpa082023v2
(base) [onyea005@ln0004 ~]$ conda activate /home/isrania/onyea005/.conda/envs/mpa082023V2
(mpa082023V2) [onyea005@ln0004 ~]$ metaphlan --install --bowtie2db /home/isrania/onyea005/HUMANN082023/Databases2/metaphlan_db

Downloading http://cmprod1.cibio.unitn.it/biobakery4/metaphlan_databases/mpa_latest
Downloading file of size: 0.00 MB
0.01 MB 25600.00 %  21.64 MB/sec  0 min -0 sec         
Downloading MetaPhlAn database
Please note due to the size this might take a few minutes

\Downloading and uncompressing indexes

Downloading http://cmprod1.cibio.unitn.it/biobakery4/metaphlan_databases/bowtie2_indexes/mpa_vOct22_CHOCOPhlAnSGB_202212_bt2.tar
Downloading file of size: 20348.27 MB
20348.27 MB 100.00 %   1.23 MB/sec  0 min -0 sec         
Downloading http://cmprod1.cibio.unitn.it/biobakery4/metaphlan_databases/bowtie2_indexes/mpa_vOct22_CHOCOPhlAnSGB_202212_bt2.md5
Downloading file of size: 0.00 MB
0.01 MB 11070.27 %  60.91 MB/sec  0 min -0 sec         
Downloading and uncompressing additional files

Downloading http://cmprod1.cibio.unitn.it/biobakery4/metaphlan_databases/mpa_vOct22_CHOCOPhlAnSGB_202212.tar
Downloading file of size: 2884.91 MB
2884.91 MB 100.00 %   1.58 MB/sec  0 min -0 sec         
Downloading http://cmprod1.cibio.unitn.it/biobakery4/metaphlan_databases/mpa_vOct22_CHOCOPhlAnSGB_202212.md5
Downloading file of size: 0.00 MB
0.01 MB 11702.86 %  66.60 MB/sec  0 min -0 sec         

Decompressing /home/isrania/onyea005/HUMANN082023/Databases2/metaphlan_db/mpa_vOct22_CHOCOPhlAnSGB_202212_SGB.fna.bz2 into /home/isrania/onyea005/HUMANN082023/Databases2/metaphlan_db/mpa_vOct22_CHOCOPhlAnSGB_202212_SGB.fna
Terminated
(mpa082023V2) [onyea005@ln0004 ~]$ humann_databases --download chocophlan full /home/isrania/onyea005/HUMANN082023/Databases2 --update-config yes


Process killed for exceeding 15 minutes of user CPU time on a login node. (PID: 3438682 Name: python)

Creating subdirectory to install database: /home/isrania/onyea005/HUMANN082023/Databases2/chocophlan
Download URL: http://huttenhower.sph.harvard.edu/humann_data/chocophlan/full_chocophlan.v201901_v31.tar.gz
Downloading file of size: 15.37 GB

15.37 GB 100.00 %  12.48 MB/sec  0 min -0 sec         
Extracting: /home/isrania/onyea005/HUMANN082023/Databases2/full_chocophlan.v201901_v31.tar.gz

Database installed: /home/isrania/onyea005/HUMANN082023/Databases2/chocophlan

HUMAnN configuration file updated: database_folders : nucleotide = /home/isrania/onyea005/HUMANN082023/Databases2/chocophlan
(mpa082023V2) [onyea005@ln0004 ~]$ humann_databases --download uniref uniref90_diamond /home/isrania/onyea005/HUMANN082023/Databases2 --update-config yes
Creating subdirectory to install database: /home/isrania/onyea005/HUMANN082023/Databases2/uniref
Download URL: http://huttenhower.sph.harvard.edu/humann_data/uniprot/uniref_annotated/uniref90_annotated_v201901b_full.tar.gz
Downloading file of size: 19.17 GB

19.17 GB 100.00 %  12.54 MB/sec  0 min -0 sec         
Extracting: /home/isrania/onyea005/HUMANN082023/Databases2/uniref90_annotated_v201901b_full.tar.gz

Database installed: /home/isrania/onyea005/HUMANN082023/Databases2/uniref

HUMAnN configuration file updated: database_folders : protein = /home/isrania/onyea005/HUMANN082023/Databases2/uniref
(mpa082023V2) [onyea005@ln0004 ~]$ humann_databases --download utility_mapping full /home/isrania/onyea005/HUMANN082023/Databases2 --update-config yes
Creating subdirectory to install database: /home/isrania/onyea005/HUMANN082023/Databases2/utility_mapping
Download URL: http://huttenhower.sph.harvard.edu/humann_data/full_mapping_v201901b.tar.gz
Downloading file of size: 2.55 GB

2.55 GB 100.00 %  12.37 MB/sec  0 min -0 sec         
Extracting: /home/isrania/onyea005/HUMANN082023/Databases2/full_mapping_v201901b.tar.gz

Database installed: /home/isrania/onyea005/HUMANN082023/Databases2/utility_mapping

HUMAnN configuration file updated: database_folders : utility_mapping = /home/isrania/onyea005/HUMANN082023/Databases2/utility_mapping

My goal here is to make sure that metaphlan point to the right files 

https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4

https://forum.biobakery.org/t/sgb-to-gtdb-profile-py-utility-does-not-work-out-of-the-box-in-metaphlan-4-0-5/4895/7

pwd
cd /home/isrania/onyea005/HUMANN082023/Databases2
metaphlan –help
humann --config	
If this does not work, I will need to try other guidelines (either including a bowtiedb command in the main command, or the guidelines for mkdir for the metaphlandb)

https://forum.biobakery.org/t/metaphlan4-and-the-new-version-humann/3985/5

The --index parameter is used in metaphlan to specify a specific version of the database you want to run. If not specified, MetaPhlAn will look for the latest version available.
So, for running MetaPhlAn 4 with the v30 database, you will have to use the following additional parameters:
--mpa3 --index mpa_v30_CHOCOPhlAn_201901 --bowtie2db v30_db_dir
Being v30_db_dir the directory containing the v30 database.

Hi, how to specify a metaphlan4 database when run humann3? cause the latest version (vOct22) of metaphlan database not compatible with humann3. The problem is that even though i download the vJan21 database in the metaphlan4 install dir( in my cluster it’s /anaconda3/envs/bioinfo/lib/python3.9/site-packages/metaphlan/metaphlan_databases) , when i run human3, Metaphlan automatically downloaded the latest version （vOct22） of the database again in the same directory, then i got the error:
ERROR: The MetaPhlAn taxonomic profile provided was not generated with the database version v3 or vJan21 . Please update your version of MetaPhlAn to at least v3.0 or if you are using MetaPhlAn v4 please use the database vJan21

We are currently working in making humann3 compatible with version vOct22. In the meanwhile you can try to run humann with the option: --metaphlan-options "-t -index mpa_vJan21_CHOCOPhlAnSGB_202103"

https://forum.biobakery.org/t/specifying-directory-for-db-to-be-downloaded-to/344/5
https://github.com/biobakery/MetaPhlAn/issues/91
https://github.com/biobakery/MetaPhlAn/issues/145
https://omicx.cc/posts/2021-12-17-install-and-setup-metaphlan-3/

2. Download and setup database
MetaPhlAn needs the clade markers and the database to be downloaded locally. The default MetaPhlAn 3.0 database folder path is $HOME/miniconda3/envs/mpa/lib/python3.7/site-packages/metaphlan/metaphlan_databases/. But it’s recommended to install the database in a folder outside the Conda environment.
# Database dir: $HOME/db/metaphlan_databases
$ cd
$ mkdir -p db/metaphlan_databases
Then download and install latest databases:
# Enter `mpa` environment first
$ conda activate mpa
# Download & install latest database
(mpa) $ metaphlan --install --bowtie2db ~/db/metaphlan_databases
You could also manual download related files from:
	•	Dropbox
	•	Google Drive
	•	Zenodo
Only download the .tar, .md5, and the mpa_latest files and place them in the metaphlan_databases folder.
3. Test the installation
Create a new folder metaphlan_analysis and enter:
(mpa) $ mkdir metaphlan_analysis
(mpa) $ cd metaphlan_analysis
Then download a sample file:
(mpa) $ curl -LO https://github.com/biobakery/biobakery/raw/master/demos/biobakery_demos/data/metaphlan3/input/SRS014476-Supragingival_plaque.fasta.gz

# or

(mpa) $ wget https://github.com/biobakery/biobakery/raw/master/demos/biobakery_demos/data/metaphlan3/input/SRS014476-Supragingival_plaque.fasta.gz
Run a single sample:
(mpa) $ metaphlan SRS014476-Supragingival_plaque.fasta.gz --input_type fasta > SRS014476-Supragingival_plaque_profile.txt
It will output two files:
	•	SRS014476-Supragingival_plaque.fasta.gz.bowtie2out.txt: contains the intermediate mapping results to unique sequence markers.
	•	SRS014476-Supragingival_plaque_profile.txt: contains the final computed organism abundances.

pwd
cd /home/isrania/onyea005/HUMANN082023/Databases2
metaphlan –help	
humann --config	
$ curl -LO https://github.com/biobakery/biobakery/raw/master/demos/biobakery_demos/data/metaphlan3/input/SRS014476-Supragingival_plaque.fasta.gz
metaphlan SRS014476-Supragingival_plaque.fasta.gz --input_type fasta > SRS014476-Supragingival_plaque_profile.txt
Downloading http://cmprod1.cibio.unitn.it/biobakery4/metaphlan_databases/bowtie2_indexes/mpa_vOct22_CHOCOPhlAnSGB_202212_bt2.tar
Downloading file of size: 20348.27 MB
20348.27 MB 100.00 %   2.12 MB/sec  0 min -0 sec         
Downloading http://cmprod1.cibio.unitn.it/biobakery4/metaphlan_databases/bowtie2_indexes/mpa_vOct22_CHOCOPhlAnSGB_202212_bt2.md5
Downloading file of size: 0.00 MB
0.01 MB 11070.27 %  43.98 MB/sec  0 min -0 sec         
Downloading and uncompressing additional files

Downloading http://cmprod1.cibio.unitn.it/biobakery4/metaphlan_databases/mpa_vOct22_CHOCOPhlAnSGB_202212.tar
Downloading file of size: 2884.91 MB
2884.91 MB 100.00 %   1.71 MB/sec  0 min -0 sec         
Downloading http://cmprod1.cibio.unitn.it/biobakery4/metaphlan_databases/mpa_vOct22_CHOCOPhlAnSGB_202212.md5
Downloading file of size: 0.00 MB
0.01 MB 11702.86 %  67.42 MB/sec  0 min -0 sec         

Decompressing /home/isrania/onyea005/.conda/envs/mpa082023V2/lib/python3.7/site-packages/metaphlan/metaphlan_databases/mpa_vOct22_CHOCOPhlAnSGB_202212_SGB.fna.bz2 into /home/isrania/onyea005/.conda/envs/mpa082023V2/lib/python3.7/site-packages/metaphlan/metaphlan_databases/mpa_vOct22_CHOCOPhlAnSGB_202212_SGB.fna
Terminated
(mpa082023V2) [onyea005@ln0004 HUMANN082023]$ 

Process killed for exceeding 15 minutes of user CPU time on a login node. (PID: 3585861 Name: python)
Doing the step above tried to download the databases directly for metaphlan, so I will have to try the –index option followed by using the index when running metaphlan.
https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4
Important! The MetaPhlAn 4 database has been substantially increased in comparison with the previous 3.1 version. Thus, for running MetaPhlAn 4, a minimum of 15GB or memory is needed.

If you have installed MetaPhlAn using Anaconda, it is advised to install the database in a folder outside the Conda environment. To do this, run
$ metaphlan --install --bowtie2db <database folder>

If you install the database in a different location, remember to run MetaPhlAn using --bowtie2db <database folder>!

By default, the latest MetaPhlAn database is downloaded and built. You can download a specific version with the --index parameter

$ metaphlan --install --index mpa_vJan21_CHOCOPhlAnSGB_202103 --bowtie2db <database folder>

When --index is specified, MetaPhlAn skips the check for the latest database version and run the analysis using the database version provided by --index located in --bowtie2db.

This option is recommended when MetaPhlAn is run on HPC clusters or containerized
If you have issues in downloading the database, you can get it from:
	•	Segatalab FTP

Just download the .tar, .md5, and the mpa_latest files and place them in the metaphlan_databases folder. (I think you need to use metaphlan --help , then look at the bowtie2DB path location

https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4

By default, the latest MetaPhlAn database is downloaded and built. You can download a specific version with the --index parameter

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
metaphlan --install --index mpa_v30_CHOCOPhlAn_201901 --bowtie2db </Volumes/RAVPOWER/ROOK_PC/HUMANN/metaphlan_db>

metaphlan --install --index mpa_v30_CHOCOPhlAn_201901 --bowtie2db /Volumes/RAVPOWER/ROOK_PC/HUMANN/metaphlan_db2

cd /home/isrania/onyea005/HUMANN
metaphlan --install --index mpa_v30_CHOCOPhlAn_201901 --bowtie2db /home/isrania/onyea005/HUMANN/metaphlan_db2
When --index is specified, MetaPhlAn skips the check for the latest database version and run the analysis using the database version provided by --index located in --bowtie2db. [the link was found at https://drive.google.com/drive/folders/1_HaY16mT7mZ_Z8JtesH8zCfG9ikWcLXG]

From https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4, I also went and manually downloaded the files from http://cmprod1.cibio.unitn.it/biobakery4/metaphlan_databases/
 On my computer And I placed them into a folder to save time in the future. I will also use the index option to force the download of each specific .tar .md5 and the mpa_latest files to place them in the metaphlan_db folder. 

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
metaphlan --install --index mpa_v30_CHOCOPhlAn_201901 --bowtie2db </Volumes/RAVPOWER/ROOK_PC/HUMANN/metaphlan_db>

metaphlan --install --index mpa_v30_CHOCOPhlAn_201901 --bowtie2db /Volumes/RAVPOWER/ROOK_PC/HUMANN/metaphlan_db2

cd /home/isrania/onyea005/HUMANN
metaphlan --install --index mpa_v30_CHOCOPhlAn_201901 --bowtie2db /home/isrania/onyea005/HUMANN/metaphlan_db2
pwd
cd /home/isrania/onyea005/HUMANN082023/Databases2
metaphlan –help
humann --config	
https://omicx.cc/posts/2021-12-17-install-and-setup-metaphlan-3/
2. Download and setup database
MetaPhlAn needs the clade markers and the database to be downloaded locally. The default MetaPhlAn 3.0 database folder path is $HOME/miniconda3/envs/mpa/lib/python3.7/site-packages/metaphlan/metaphlan_databases/. But it’s recommended to install the database in a folder outside the Conda environment.
# Database dir: $HOME/db/metaphlan_databases
$ cd
$ mkdir -p db/metaphlan_databases
Then download and install latest databases:
# Enter `mpa` environment first
$ conda activate mpa
# Download & install latest database
(mpa) $ metaphlan --install --bowtie2db ~/db/metaphlan_databases
You could also manual download related files from:
	•	Dropbox
	•	Google Drive
	•	Zenodo
Only download the .tar, .md5, and the mpa_latest files and place them in the metaphlan_databases folder.
# Database dir: $HOME/db/metaphlan_databases
cd
mkdir -p home/isrania/onyea005/HUMANN082023/Databases2/metaphlan_databases
cd home/isrania/onyea005/HUMANN082023/Databases2/metaphlan_databases
metaphlan --install --index mpa_latest --bowtie2db /home/isrania/onyea005/HUMANN082023/Databases2/metaphlan_databases
metaphlan --install --index mpa_vJan21_CHOCOPhlAnSGB_202103.md5 --bowtie2db /home/isrania/onyea005/HUMANN082023/Databases2/metaphlan_databases
metaphlan --install --index mpa_vJan21_CHOCOPhlAnSGB_202103.tar --bowtie2db /home/isrania/onyea005/HUMANN082023/Databases2/metaphlan_databases

metaphlan --install --index mpa_vOct22_CHOCOPhlAnSGB_202212.md5 --bowtie2db /home/isrania/onyea005/HUMANN082023/Databases2/metaphlan_databases
metaphlan --install --index mpa_vOct22_CHOCOPhlAnSGB_202212.tar --bowtie2db /home/isrania/onyea005/HUMANN082023/Databases2/metaphlan_databases

*The code above did not work, I will try this version at the end of the day
pwd
cd /home/isrania/onyea005/HUMANN082023/Databases2
metaphlan --help
humann --config	
metaphlan --install --bowtie2db /home/isrania/onyea005/HUMANN082023/Databases2/metaphlan_databases
https://forum.biobakery.org/t/permanently-set-custom-database-path/959
https://forum.biobakery.org/t/run-humann-3-for-producing-metaphlan-4s-outputs/5385/6

The link above seems to have the best of both worlds when it comes to analysis. From what I understand, HUMANN also downloads the same databases as metaphlan so the humann config file knows which databases to use. The metaphlan database only become an issue when trying to run metaphlan as a standalone option?

https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4

*Using the option where I have downloaded the metaphlan databases to a specific folder and I point metaphlan to it 
pwd
cd /home/isrania/onyea005/HUMANN082023/Databases2
metaphlan --help	
humann --config	
curl -LO https://github.com/biobakery/biobakery/raw/master/demos/biobakery_demos/data/metaphlan3/input/SRS014476-Supragingival_plaque.fasta.gz
metaphlan SRS014476-Supragingival_plaque.fasta.gz --input_type fasta > SRS014476-Supragingival_plaque_profile.txt --bowtie2db /home/isrania/onyea005/HUMANN082023/Databases2/metaphlan_db

*The step above did not wor, trying with another folder which holds the databases I manually downloaded
metaphlan SRS014476-Supragingival_plaque.fasta.gz --input_type fasta > SRS014476-Supragingival_plaque_profile.txt --bowtie2db /home/isrania/onyea005/HUMANN082023/Databases2/metaphlan_databases

*Trying out the HUMANN command while I wait
Pwd
cd /home/isrania/onyea005/HUMANN082023/demo
humann -i SRS014476-Supragingival_plaque.fasta.gz -o sample_results
humann -i demo.fastq.gz -o sample_results

I got the error message below:
Creating output directory: /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/demo/sample_results
Output files will be written to: /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/demo/sample_results
Decompressing gzipped file ...

Running metaphlan ........

CRITICAL ERROR: Error executing: /home/isrania/onyea005/.conda/envs/mpa082023V2/bin/metaphlan /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/demo/sample_results/demo_humann_temp/tmpfd4827zj/tmpmm1j2v9g -t rel_ab -o /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/demo/sample_results/demo_humann_temp/demo_metaphlan_bugs_list.tsv --input_type fastq --bowtie2out /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/demo/sample_results/demo_humann_temp/demo_metaphlan_bowtie2.txt

Error message returned from metaphlan :
Only read 2147479552 bytes (out of 2655803348) from reference index file /home/isrania/onyea005/.conda/envs/mpa082023V2/lib/python3.7/site-packages/metaphlan/metaphlan_databases/mpa_vOct22_CHOCOPhlAnSGB_202212.4.bt2l
Error: Encountered internal Bowtie 2 exception (#1)
Command: /home/isrania/onyea005/.conda/envs/mpa082023V2/bin/bowtie2-align-l --wrapper basic-0 --seed 1992 --quiet --very-sensitive -x /home/isrania/onyea005/.conda/envs/mpa082023V2/lib/python3.7/site-packages/metaphlan/metaphlan_databases/mpa_vOct22_CHOCOPhlAnSGB_202212 -p 4 --passthrough -U - 
(ERR): bowtie2-align exited with value 1
Traceback (most recent call last):
  File "/home/isrania/onyea005/.conda/envs/mpa082023V2/bin/read_fastx.py", line 10, in <module>
    sys.exit(main())
  File "/home/isrania/onyea005/.conda/envs/mpa082023V2/lib/python3.7/site-packages/metaphlan/utils/read_fastx.py", line 168, in main
    f_nreads, f_avg_read_length = read_and_write_raw(f, opened=False, min_len=min_len, prefix_id=prefix_id)
  File "/home/isrania/onyea005/.conda/envs/mpa082023V2/lib/python3.7/site-packages/metaphlan/utils/read_fastx.py", line 130, in read_and_write_raw
    nreads, avg_read_length = read_and_write_raw_int(inf, min_len=min_len, prefix_id=prefix_id)
  File "/home/isrania/onyea005/.conda/envs/mpa082023V2/lib/python3.7/site-packages/metaphlan/utils/read_fastx.py", line 109, in read_and_write_raw_int
    print_record(description + "__{}{}{}".format(prefix_id, '.' if prefix_id else '', idx), sequence, qual, fmt))
BrokenPipeError: [Errno 32] Broken pipe

*Trying to run HUMANN with a –metaphlan option 
https://forum.biobakery.org/t/run-humann-3-for-producing-metaphlan-4s-outputs/5385
*Trying out the HUMANN command while I wait
Pwd
cd /home/isrania/onyea005/HUMANN082023/demo
metaphlan --help	
humann --config	
humann -i SRS014476-Supragingival_plaque.fasta.gz -o sample_results
#The nomenclature was taken from (https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4 and https://forum.biobakery.org/t/run-humann-3-for-producing-metaphlan-4s-outputs/5385)
pwd
metaphlan --help	
humann --config	
humann -i demo.fastq.gz -o sample_results --metaphlan-options "demo.fastq.gz --input_type fastq -o sample_results/demo.txt --bowtie2db /home/isrania/onyea005/HUMANN082023/Databases2/chocophlan"

HUMANN Analysis 082023 demo run V2 (fixing bowtie “No MetaPhlAn BowTie2 database found (--index option)!”, STILL NOT WORKING.

	•	Connect to VPN for MAC using Cisco AnyConnect Secure Mobility Client
	•	Connect to FileZilla to be able to see the files being moved and created in a dynamic fashion
	•	Enter sftp://login.msi.umn.edu into the Host box, your username into the Username box, your password into the Password box, and 22 into the Port box. Click on Quickconnect. (Tried using sftp://mesabi.msi.umn.edu, username and password since login node changes took place in Jaunary 2021)

	•	Connect to Mesabi (recommended default for MSI to be connected to the supercomputers)
Alternatively, connect to MSI using Terminal on Mac/Linux, located in ApplicationsUtilitiesTerminal.app. 
*NOTE: Like the login node Mesabi and Itasca have specific nomenclatures in the prompt. When connected to Mesabi your prompt will contain ln followed by some number when connected to Itasca your prompt will contain node followed by some numbers. This is a quick short cut to remind you which system you are connected to at any time.
**You need to be connected to Cisco Anyconnect via VPN in order for this command to work and it will ask you to login via DUO




	
	
Step 1: Check the installation instructions 

https://forum.biobakery.org/t/announcing-humann-3-5/3995

Step 2: Install Metaphlan 4.0

https://forum.biobakery.org/t/metaphlan4-and-the-new-version-humann/3985/4

https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4

Step 3: Install a new environment of Humann 3.0 (Which upgrades it to 3.5, 3.6 or 3.7)

https://forum.biobakery.org/t/announcing-humann-3-6-critical-update/4155

https://huttenhower.sph.harvard.edu/humann

Step 4: Test HUMANN

https://github.com/biobakery/biobakery/wiki/humann3











Step 1: Check the installation instructions 

https://forum.biobakery.org/t/announcing-humann-3-5/3995

Step 2: Install Metaphlan 4.0

https://forum.biobakery.org/t/metaphlan4-and-the-new-version-humann/3985/4

https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4

MetaPhlAn 4

Home

MetaPhlAn documentation
•	MetaPhlAn 4
•	MetaPhlAn 3.1
•	MetaPhlAn 3.0
•	MetaPhlAn2

StrainPhlAn documentation
•	StrainPhlAn 4
•	StrainPhlAn 3
•	StrainPhlAn
•	Strain Sharing Inference

Workshop on Genomics 2023
•	MetaPhlAn workshop
•	HUMAnN workshop


MetaPhlAn is a computational tool for profiling the composition of microbial communities (Bacteria, Archaea and Eukaryotes) from metagenomic shotgun sequencing data (i.e. not 16S) with species-level. With StrainPhlAn, it is possible to perform accurate strain-level microbial profiling.

MetaPhlAn 4 relies on ~5.1M unique clade-specific marker genes (the latest marker information file can be found here) identified from ~1M microbial genomes (~236,600 references and 771,500 metagenomic assembled genomes) spanning 26,970 species-level genome bins (SGBs, http://segatalab.cibio.unitn.it/data/Pasolli_et_al.html), 4,992 of them taxonomically unidentified at the species level (the full list of the species included in the latest database can be found here), allowing:

•	unambiguous taxonomic assignments;
•	an accurate estimation of organismal relative abundance;
•	SGB-level resolution for bacteria, archaea and eukaryotes;
•	strain identification and tracking
•	orders of magnitude speedups compared to existing methods.
•	metagenomic strain-level population genomics




ssh yourusername@login.msi.umn.edu (use ssh yourusername@mangi.msi.umn.edu or ssh yourusername@mesabi.msi.umn.edu respectively)
ssh onyea005@mesabi.msi.umn.edu
	•	Check the folder you are in
pwd
* my home folder is when I first login is “/home/prizm001/onyea005”
* /home/isrania/onyea005
*Change the folder to the israni MSI group
cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
cd /panfs/roc/groups/5/isrania/onyea005
cd /panfs/roc/groups/5/isrania/onyea005/Israni4_Project_016

Creating and MSI script to install Metaphlan and HUMANN 

Learning how to use SLURM to schedule a job since running the humann pipeline on all the files in the folder would be veery time consuming
https://www.msi.umn.edu/slurm
What is Slurm?
Slurm is a best-in-class, highly-scalable scheduler for HPC clusters. It allocates resources, provides a framework for executing tasks, and arbitrates contention for resources by managing queues of pending work.
Why is MSI transitioning to the Slurm scheduler?
Slurm has become an industry standard for scheduling among HPC centers. It’s an open-source scheduler with a plugin framework that allows us to leverage tools developed at other centers. It is capable of stable management of a larger number of jobs than our current scheduler. Finally, it’s architecture opens opportunities to leverage technologies that will be useful for many areas of scientific computation.
How does the transition to Slurm impact my work on MSI systems?
The most obvious adjustment everyone will need to make is to learn a new set of commands for submitting jobs and checking on job status. If you have written scripts that depend on the job scheduler, they will need to be modified to match the syntax used in Slurm. This is also true of some software that MSI maintains.
https://www.msi.umn.edu/content/job-submission-and-scheduling-slurm
Job Submission and Scheduling (Slurm)
MSI systems use job queues to efficiently and fairly manage when computations are executed. When computational jobs are submitted to a job queue they wait in the queue until the appropriate computational resources are available.
 
The queuing system which MSI uses is called Slurm. To submit a job to a Slurm queue, users create Slurm job scripts. Slurm job scripts contain information on the resources requested for the calculation, as well as the commands for executing the calculation.
Slurm Script Format
A Slurm job script is a small text file containing information about what resources a job requires, including time, number of nodes, and memory.   The Slurm script also contains the commands needed to begin executing the desired computation. A sample Slurm job script is shown below.
#!/bin/bash -l        
#SBATCH --time=8:00:00
#SBATCH --ntasks=8
#SBATCH --mem=10g
#SBATCH --tmp=10g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=sample_email@umn.edu 
cd ~/program_directory
module load intel 
module load ompi/intel 
mpirun -np 8 program_name < inputfile > outputfile
The first line in the Slurm script defines which type of shell the script will be read with (how the system will read the file).  It is recommended to make the first line #!/bin/bash -l
Commands for the Slurm queuing system begin with #SBATCH.  Lines 2–4 in the above sample script contain the Slurm resource request.   The sample job will require 8 hours, 8 processor cores,  and 10 gigabytes of memory.  The resource request must contain appropriate values; if the requested time, processors, or memory are not suitable for the hardware the job will not be able to run. 
The two lines containing #SBATCH --mail-type, and #SBATCH --mail-user=sample_email@umn.edu , are both commands having to do with sending message emails to the user. The first of these lines instruct the Slurm system to send a message email when the job aborts, begins, or ends.  Other options here include NONE, BEGIN, END, and FAIL. The second command specifies the email address to be used. Using the message emails is recommended because the reason for a job failure can often be determined using information in the emails.
The rest of the sample Slurm script contains the commands which will be executed to begin the calculation. A Slurm script should contain the appropriate change directory commands to get to the job execution location. A Slurm script also needs to contain module load commands for any software modules that the calculation might need. The last lines of a Slurm script contain commands used to execute the calculation. In the above example the final line contains an execution command to start a program which uses MPI communication to run on 8 processor cores.
Submitting Job Scripts
Once a job script is made it is submitted using the sbatch command:
sbatch -p partitionname scriptname   
Here partitionname is the name of the partition (queue) being submitted to, and scriptname is the name of the job script.  The -p partitionname portion of the command may be omitted, in which case the job would be submitted to whichever partition is set as the default.  Alternatively, the partition specification can be placed inside the job script (see below).
To view all of the jobs submitted by a particular user use the command:
squeue -u username
This command will display the status of the specified jobs, and the associated job ID numbers. The command squeue by itself will show all jobs on the system.
To cancel a submitted job use the command:
scancel jobIDnumber
Here jobIDnumber should be replaced with the appropriate job ID number determined by using the squeue command.
Slurm Script Commands
Below is a table summarizing some commands that can be used inside Slurm job scripts. The first four commands (interpreter specification, and resource request) are required, while the other commands are optional. Each of the below Slurm commands is meant to go on a single line within a Slurm script.
 
Slurm command
Effect
#!/bin/bash -l
Specifies how the Slurm file should be read (by the bash interpreter). A statement like this is required to be the first line of a Slurm script.
#SBATCH --time=8:00:00
Specifies the maximum limit for how long the job will be allowed to run. (8 hours)
#SBATCH --ntasks=8
Specifies the number of processors (cores) that will be reserved for this job.  (8)
#SBATCH --mem=10g
Specifies the maximum limit for memory usage. This job will die if the application tries to use more than 10GB of memory.
#SBATCH --tmp=10g
Specifies 10 GB of temporary disk will be available for this job in /tmp.
#SBATCH --mail-type=ALL  
Specifies which events will trigger an email message. Other options here include NONE, BEGIN, END, and FAIL. 

#SBATCH --mail-user=me@umn.edu
Specifies the email address that should be used when the Slurm system sends message emails.
#SBATCH -p small,mygroup
Specifies the partition to be the “small” partition, or a partition named “mygroup”. The job will start at the earliest time one of these partitions can accommodate the job.
#SBATCH --gres=gpu:v100:2
#SBATCH -p v100
Request two v100 GPUs for a job submitted to the V100 queue.

https://www.msi.umn.edu/partitions#slurm
Slurm Partitions
Under MSI's new scheduler, Slurm, queues are known as partitions. The job partitions on our systems manage different sets of hardware, and have different limits for quantities such as walltime, available processors, and available memory. When submitting a calculation it is important to choose a partition where the job is suited to the hardware and resource limitations.
Selecting a Partition
Each MSI system contains job partitions managing sets of hardware with different resource and policy limitations. MSI currently has two primary systems: the supercomputer Mesabi and the Mesabi expansion Mangi. Mesabi has high-performance hardware and a wide variety of partitions suitable for many different job types. Mangi expands Mesabi and should be your first choice for submitting jobs. Which system to choose depends highly on which system has partitions appropriate for your software/script. More information about selecting a partitions and the different partition parameters can be found on the Choosing A Partition (Slurm) page.
Below is a summary of the available partitions organized by system, and the associated limitations. The quantities listed are totals or upper limits.
Partition name
Node sharing?
Cores per node
Walltime limit
Total node memory
Advised memory per core
Local scratch per node
Maximum Nodes per Job
amdsmall (1)
Yes
128
96:00:00
248 GB
1900 MB
415 GB
1
amdlarge
No
128
24:00:00
248 GB
1900 MB
415 GB
32
amd512
Yes
128
96:00:00
499 GB
4000 MB
415 GB
1
amd2tb
Yes
128
96:00:00
1995 GB
15 GB
415 GB
1
v100 (1)
Yes
24
24:00:00
374 GB
15 GB
859 GB
1
small
Yes
24
96:00:00
60 GB
2500 MB
429 GB
10
large
No
24
24:00:00
60 GB
2500 MB
429 GB
48
max
Yes
24
696:00:00
60 GB
2500 MB
429 GB
1
ram256g
Yes
24
96:00:00
248 GB
10 GB
429 GB
2
ram1t (2)
Yes
32
96:00:00
1002 GB
31 GB
380 GB
2
k40 (1)
Yes
24
24:00:00
123 GB
5 GB
429 GB
40
interactive (3)
Yes
24
24:00:00
60 GB
2 GB
228 GB
2
interactive-gpu (3)
Yes
24
24:00:00
60 GB
2 GB
228 GB
2
preempt (4)
Yes
24
24:00:00
60 GB
2 GB
228 GB
2
preempt-gpu (4)
Yes
24
24:00:00
60 GB
2 GB
228 GB
2

(1) Note: In addition to selecting a GPU partition, GPUs need to be requested for all GPU jobs.  A  k40 GPU can be requested by including the following two lines in your submission script: 
#SBATCH -p k40                                             
#SBATCH --gres=gpu:k40:1
A  V100 GPU can be requested by including the following two lines in your submission script: 
#SBATCH -p v100                                            
#SBATCH --gres=gpu:v100:1
(2) Note: The ram1t nodes contain Intel Ivy Bridge processors, which do not support all of the optimized instructions of the Haswell processors. Programs compiled using the Haswell instructions will only run on the Haswell processors.
(3) Note: Users are limited to 2 jobs in the interactive and interactive-gpu partitions. 
(4) Note: Jobs in the preempt and preempt-gpu partitions may be killed at any time to make room for jobs in the interactive or interactive-gpu partitions.

Submitting Job Scripts
Once a job script is made it is submitted using the sbatch command:
sbatch -p partitionname scriptname   
Here partitionname is the name of the partition (queue) being submitted to, and scriptname is the name of the job script.  The -p partitionname portion of the command may be omitted, in which case the job would be submitted to whichever partition is set as the default.  Alternatively, the partition specification can be placed inside the job script (see
sbatch -p partitionname scriptname   
cd /home/isrania/onyea005
sbatch -p amdsmall HUMANN_MISSION_MSI_SLURM.rtf   
sbatch -p amdsmall HUMANN_MISSION_MSI_SLURM.txt   
**I wasn’t able to figure out how I could save a txt file into a shell script to submit to MSI, so I will use the online script comverter I found

https://support.ceci-hpc.be/doc/_contents/QuickStart/SubmittingJobs/SlurmTutorial.html
http://www.ceci-hpc.be/scriptgen.html

https://ubccr.freshdesk.com/support/solutions/articles/5000688140-submitting-a-slurm-job-script

Create a SLURM script using an editor such as vi or emacs using steps 1 through 3.  The script (or file) can be called anything you want but should end in .sh (i.e. myscript.sh).  If you are unfamiliar with the UNIX commands to edit files, please read this article

http://www.compciv.org/recipes/cli/basic-shell-scripts/

http://www.compciv.org/bash-guide/
https://slurm.schedmd.com/quickstart.html

The gameplan is to save the file as a txt file from word on my mac, move over to windows to forcefully modify the txt extension to SH 

https://support.apple.com/guide/terminal/use-command-line-text-editors-apdb02f1133-25af-4c65-8976-159609f99817/mac

nvm 
https://www.msi.umn.edu/content/job-submission-and-scheduling-slurm
Job Submission and Scheduling (Slurm)
MSI systems use job queues to efficiently and fairly manage when computations are executed. When computational jobs are submitted to a job queue they wait in the queue until the appropriate computational resources are available.
 
The queuing system which MSI uses is called Slurm. To submit a job to a Slurm queue, users create Slurm job scripts. Slurm job scripts contain information on the resources requested for the calculation, as well as the commands for executing the calculation.
Slurm Script Format
A Slurm job script is a small text file containing information about what resources a job requires, including time, number of nodes, and memory.   The Slurm script also contains the commands needed to begin executing the desired computation. A sample Slurm job script is shown below.
#!/bin/bash -l        
#SBATCH --time=8:00:00
#SBATCH --ntasks=8
#SBATCH --mem=10g
#SBATCH --tmp=10g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=sample_email@umn.edu 
cd ~/program_directory
module load intel 
module load ompi/intel 
mpirun -np 8 program_name < inputfile > outputfile
The first line in the Slurm script defines which type of shell the script will be read with (how the system will read the file).  It is recommended to make the first line #!/bin/bash -l
Commands for the Slurm queuing system begin with #SBATCH.  Lines 2–4 in the above sample script contain the Slurm resource request.   The sample job will require 8 hours, 8 processor cores,  and 10 gigabytes of memory.  The resource request must contain appropriate values; if the requested time, processors, or memory are not suitable for the hardware the job will not be able to run. 
The two lines containing #SBATCH --mail-type, and #SBATCH --mail-user=sample_email@umn.edu , are both commands having to do with sending message emails to the user. The first of these lines instruct the Slurm system to send a message email when the job aborts, begins, or ends.  Other options here include NONE, BEGIN, END, and FAIL. The second command specifies the email address to be used. Using the message emails is recommended because the reason for a job failure can often be determined using information in the emails.
The rest of the sample Slurm script contains the commands which will be executed to begin the calculation. A Slurm script should contain the appropriate change directory commands to get to the job execution location. A Slurm script also needs to contain module load commands for any software modules that the calculation might need. The last lines of a Slurm script contain commands used to execute the calculation. In the above example the final line contains an execution command to start a program which uses MPI communication to run on 8 processor cores.
http://www.compciv.org/recipes/cli/basic-shell-scripts/

Let's use the nano text editor to create a shell script named hello.sh. Follow these steps:
	•	Run nano hello.sh
	•	nano should open up and present an empty file for you to work in. Type in the shell command the commands you want to run via SLURM:
Here is the code for the test MISSION metatranscriptomics dataset
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --ntasks=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /home/isrania/onyea005/Israni4_Project_016 
module load conda/python3 
conda activate /home/isrania/onyea005/.conda/envs/mpa 
for f in *.fastq.gz; do humann -i $f -o mission_job; done

Here is the code for the full MISSION metatranscriptomics dataset
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /panfs/roc/groups/5/isrania/onyea005/Israni4_Project_016
module load conda/python3 
conda activate /home/isrania/onyea005/.conda/envs/mpa 
for f in *.fastq.gz; do humann -i $f -o mission_subset --threads 8 --metaphlan-options "--read_min_len 49"; done
Here is the code I will be submitting for the demo dataset using Metaphlan4 (save it in NANO)
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/demo
module load conda/python3 
conda activate /home/isrania/onyea005/.conda/envs/mpa082023
humann --input demo.fastq.gz --output humann_demo --threads 4

Here is how I will create the script to install Metaphlan and human on MSI
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /home/isrania/onyea005/HUMANN082023
module load conda/python3 
conda create --name mpa082023V3 -c conda-forge -c bioconda python=3.7 metaphlan
conda info --envs
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
mkdir -p /home/isrania/onyea005/HUMANN082023/databases3
mkdir -p /home/isrania/onyea005/HUMANN082023/databases3/metaphlan_databases
metaphlan --install --bowtie2db /home/isrania/onyea005/HUMANN082023/databases3/metaphlan_databases
conda config --add channels defaults
conda config --add channels bioconda	
conda config --add channels conda-forge
conda config --add channels biobakery
conda install humann -c biobakery
humann_databases --download chocophlan full /home/isrania/onyea005/HUMANN082023/databases3 --update-config yes
humann_databases --download uniref uniref90_diamond /home/isrania/onyea005/HUMANN082023/databases3 --update-config yes
humann_databases --download utility_mapping full /home/isrania/onyea005/HUMANN082023/databases3 --update-config yes
My 08/11/2023 slurm output was a bit suspicious so I will try a different slurm submission where I will have manually created the databases 4 folders so I don’ t mess around with 
mkdir -p /home/isrania/onyea005/HUMANN082023/databases4
cd /home/isrania/onyea005/HUMANN082023/databases4
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /home/isrania/onyea005/HUMANN082023
module load conda/python3 
conda create --name mpa082023V4 -c conda-forge -c bioconda python=3.7 metaphlan
conda info --envs
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V4
metaphlan --install --bowtie2db /home/isrania/onyea005/HUMANN082023/databases4
conda config --add channels defaults
conda config --add channels bioconda	
conda config --add channels conda-forge
conda config --add channels biobakery
conda install humann -c biobakery
humann_databases --download chocophlan full /home/isrania/onyea005/HUMANN082023/databases4 --update-config yes
humann_databases --download uniref uniref90_diamond /home/isrania/onyea005/HUMANN082023/databases4 --update-config yes
humann_databases --download utility_mapping full /home/isrania/onyea005/HUMANN082023/databases4 --update-config yes
submitting a different slurm script based on my test run of mpa082023V2 to test the metaphlan folded in database
mkdir -p /home/isrania/onyea005/HUMANN082023/databases4
cd /home/isrania/onyea005/HUMANN082023/databases4
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /home/isrania/onyea005/HUMANN082023/demo
module load conda/python3 
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V2
pwd
metaphlan --help	
humann --config	
humann -i demo.fastq.gz -o sample_results --metaphlan-options "demo.fastq.gz --input_type fastq -o sample_results/demo.txt --bowtie2db /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases2/chocophlan"
*Metaphlan does not seem to recognize the databases linked  in the path, so I will try running HUMANN without the metaphlan option (*This gave me the following error “CRITICAL ERROR: The directory provided for ChocoPhlAn contains files ( mpa_latest ) that are not of the expected version. Please install the latest version of the database: v201901_v31” so I have 2 options. 1) create a metaphlan4 environment and run the test file within that one environment (https://omicx.cc/posts/2021-12-17-install-and-setup-metaphlan-3/) 2) create a metaphlan + humann environment and 3 environment, then ask for help on how to point to the correct database in that environment (https://github.com/biobakery/biobakery/wiki/humann3 and https://github.com/biobakery/humann )
mkdir -p /home/isrania/onyea005/HUMANN082023/databases4
cd /home/isrania/onyea005/HUMANN082023/databases4
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /home/isrania/onyea005/HUMANN082023/demo
module load conda/python3 
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V2
pwd
metaphlan --help	
humann --config	
humann -i demo.fastq.gz -o sample_results 
*Trying the metaphlan options suggested in “https://forum.biobakery.org/t/metaphlan4-and-the-new-version-humann/3985/5”
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /home/isrania/onyea005/HUMANN082023/demo
module load conda/python3 
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V2
pwd
metaphlan --help	
humann --config	
humann -i demo.fastq.gz -o sample_results --metaphlan-options "-t  --mpa3 --index mpa_vJan21_CHOCOPhlAnSGB_202103 --bowtie2db /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases3"
















*Trying the human demo coding before trying the metaphlan options suggested in “https://forum.biobakery.org/t/metaphlan4-and-the-new-version-humann/3985/5”
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /home/isrania/onyea005/HUMANN082023/demo
module load conda/python3 
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
pwd
metaphlan --help	
humann --config	
humann -i demo.fastq.gz -o test_results 
#humann -i demo.fastq.gz -o test_results --metaphlan-options "-t  --mpa3 --index mpa_vJan21_CHOCOPhlAnSGB_202103 --bowtie2db /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases3"
*Trying the same option as above but commiting more CPUs to the task
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /home/isrania/onyea005/HUMANN082023/demo
module load conda/python3 
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
pwd
metaphlan --help	
humann --config	
humann -i demo.fastq.gz -o test_results3 --metaphlan-options "--mpa3 --index mpa_vJan21_CHOCOPhlAnSGB_202103 --bowtie2db /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases3" --threads 8
#humann -i demo.fastq.gz -o test_results2 --metaphlan-options "-t  --mpa3 --index mpa_vJan21_CHOCOPhlAnSGB_202103 --bowtie2db /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases3" --threads 8
#humann -i demo.fastq.gz -o test_results --threads 8
*As I mentioned above Metaphlan does not seem to recognize the databases linked  in the path, so I will try running HUMANN without the metaphlan option (*This gave me the following error “CRITICAL ERROR: The directory provided for ChocoPhlAn contains files ( mpa_latest ) that are not of the expected version. Please install the latest version of the database: v201901_v31” so I have 2 options. 1) create a metaphlan4 environment and run the test file within that one environment (https://omicx.cc/posts/2021-12-17-install-and-setup-metaphlan-3/) 2) create a metaphlan + humann environment and 3 environment, then ask for help on how to point to the correct database in that environment (https://github.com/biobakery/biobakery/wiki/humann3 and https://github.com/biobakery/humann )
Install humann and metaphlan without touching the metaphlan databases outside of chocophlan (This will be used to replicate the error so I can ask for help specifying the bowtie database in the humann command)
*I am also transferring the downloaded files from (/Volumes/RAVPOWER2/ROOK_04_PC/metaphlan_databases_082023) to a folder called /home/isrania/onyea005/HUMANN082023/databases3 via Fillezilla
cd /home/isrania/onyea005/HUMANN082023
module load conda/python3 
#conda config --add channels defaults
#conda config --add channels bioconda
#conda config --add channels conda-forge
#conda create --name mpa082023V5 -c bioconda python=3.7 metaphlan
conda create --name mpa082023V5 -c conda-forge -c bioconda python=3.7 metaphlan
conda info --envs
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V5
conda config --add channels defaults
conda config --add channels bioconda	
conda config --add channels conda-forge
conda config --add channels biobakery
conda install humann -c biobakery
humann_databases --download chocophlan full /home/isrania/onyea005/HUMANN082023/databases5 --update-config yes
humann_databases --download uniref uniref90_diamond /home/isrania/onyea005/HUMANN082023/databases5 --update-config yes
humann_databases --download utility_mapping full /home/isrania/onyea005/HUMANN082023/databases5 --update-config yes
	•	Then press Ctrl-X on your keyboard to Exit nano (type in pwd to determine the location of the sh file)
	•	nano will ask you if you want to save the modified file. Hit the y key (for "yes"). > Save the file as MISSION_TEST.sh (or whatever you chose o name)
	•	nano will then confirm if you want to save to the file name. Hit Enter to confirm this.
	•	Here partitionname is the name of the partition (queue) being submitted to, and scriptname is the name of the job script.  The -p partitionname portion of the command may be omitted, in which case the job would be submitted to whichever partition is set as the default.  Alternatively, the partition specification can be placed inside the job script
Going back to the HUMANN tutuorial, here is where we left offAlternatively, if you're comfortable with shell scripting, you can use the following command to process all six at once:


ssh yourusername@login.msi.umn.edu (use ssh yourusername@mangi.msi.umn.edu or ssh yourusername@mesabi.msi.umn.edu respectively)
ssh onyea005@mesabi.msi.umn.edu
	•	Check the folder you are in
pwd
* my home folder is when I first login is “/home/prizm001/onyea005”
* /home/isrania/onyea005
*Change the folder to the israni MSI group
cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
cd /panfs/roc/groups/5/isrania/onyea005
cd /panfs/roc/groups/5/isrania/onyea005/Israni4_Project_016

*Now this can be submitted (home and the /panfs/jay/groups/4/ strings seem to be interchangeable
sbatch -p partitionname scriptname   
cd /home/isrania/onyea005
sbatch -p amdsmall HUMANN_MISSION_MSI_SLURM.rtf   
sbatch -p amdsmall MISSION_TEST.sh
sbatch -p amdsmall MISSION_FULL.sh   
sbatch -p amdsmall humann_demo.sh
sbatch -p amdsmall humann_demo_2.sh   
sbatch -p amdsmall metaphlan_humann_install.sh   
sbatch -p amdsmall metaphlan_humann_install_v2.sh   
sbatch -p amdsmall metaphlan_bowtie2db_chocophlan.sh   
sbatch -p amdsmall metaphlan_bowtie2db_chocophlanv2.sh   
sbatch -p amdsmall humann082023_demo.sh
sbatch -p amdsmall humann_demo_metaphlan_options.sh
sbatch -p amdsmall metaphlan_only_install.sh
sbatch -p amdsmall mpa082023_humann_demo_test.sh
sbatch -p amdsmall mpa082023_humann_demo_test_8CPU.sh
sbatch -p amdsmall mpa082023_humann_demo_test_v3.sh
sbatch -p amdsmall mpa082023_humann_demo_test_v4.sh
 To view all of the jobs submitted by a particular user use the command:
squeue -u username
squeue -u onyea005
This command will display the status of the specified jobs, and the associated job ID numbers. The command squeue by itself will show all jobs on the system.

 To cancel a submitted job use the command:
scancel jobIDnumber
Here jobIDnumber should be replaced with the appropriate job ID number determined by using the squeue command.
https://ubccr.freshdesk.com/support/solutions/articles/5000686861-how-do-i-check-the-status-of-my-job-s-
squeue - Job States
R - Job is running on compute nodes
PD - Job is waiting on compute nodes
CG - Job is completing
squeue - Job Reasons
(Resources) - Job is waiting for compute nodes to become available
(Priority) - Jobs with higher priority are waiting for compute nodes.  Check this knowledge base article for info about job priority
(ReqNodeNotAvail) - The compute nodes requested by the job are not available for a variety of reasons, including:
cluster downtime
nodes offline
temporary scheduling backlog

HUMANN Analysis 082023 demo run V2 (fixing bowtie “No MetaPhlAn BowTie2 database found (--index option)!”, STILL NOT WORKING.

	•	Connect to VPN for MAC using Cisco AnyConnect Secure Mobility Client
	•	Connect to FileZilla to be able to see the files being moved and created in a dynamic fashion
	•	Enter sftp://login.msi.umn.edu into the Host box, your username into the Username box, your password into the Password box, and 22 into the Port box. Click on Quickconnect. (Tried using sftp://mesabi.msi.umn.edu, username and password since login node changes took place in Jaunary 2021)

	•	Connect to Mesabi (recommended default for MSI to be connected to the supercomputers)
Alternatively, connect to MSI using Terminal on Mac/Linux, located in ApplicationsUtilitiesTerminal.app. 
*NOTE: Like the login node Mesabi and Itasca have specific nomenclatures in the prompt. When connected to Mesabi your prompt will contain ln followed by some number when connected to Itasca your prompt will contain node followed by some numbers. This is a quick short cut to remind you which system you are connected to at any time.
**You need to be connected to Cisco Anyconnect via VPN in order for this command to work and it will ask you to login via DUO




	
	
Step 1: Check the installation instructions 

https://forum.biobakery.org/t/announcing-humann-3-5/3995

Step 2: Install Metaphlan 4.0

https://forum.biobakery.org/t/metaphlan4-and-the-new-version-humann/3985/4

https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4

Step 3: Install a new environment of Humann 3.0 (Which upgrades it to 3.5, 3.6 or 3.7)

https://forum.biobakery.org/t/announcing-humann-3-6-critical-update/4155

https://huttenhower.sph.harvard.edu/humann

Step 4: Test HUMANN

https://github.com/biobakery/biobakery/wiki/humann3











Step 1: Check the installation instructions 

https://forum.biobakery.org/t/announcing-humann-3-5/3995

Step 2: Install Metaphlan 4.0

https://forum.biobakery.org/t/metaphlan4-and-the-new-version-humann/3985/4

https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4

MetaPhlAn 4

Home

MetaPhlAn documentation
•	MetaPhlAn 4
•	MetaPhlAn 3.1
•	MetaPhlAn 3.0
•	MetaPhlAn2

StrainPhlAn documentation
•	StrainPhlAn 4
•	StrainPhlAn 3
•	StrainPhlAn
•	Strain Sharing Inference

Workshop on Genomics 2023
•	MetaPhlAn workshop
•	HUMAnN workshop


MetaPhlAn is a computational tool for profiling the composition of microbial communities (Bacteria, Archaea and Eukaryotes) from metagenomic shotgun sequencing data (i.e. not 16S) with species-level. With StrainPhlAn, it is possible to perform accurate strain-level microbial profiling.

MetaPhlAn 4 relies on ~5.1M unique clade-specific marker genes (the latest marker information file can be found here) identified from ~1M microbial genomes (~236,600 references and 771,500 metagenomic assembled genomes) spanning 26,970 species-level genome bins (SGBs, http://segatalab.cibio.unitn.it/data/Pasolli_et_al.html), 4,992 of them taxonomically unidentified at the species level (the full list of the species included in the latest database can be found here), allowing:

•	unambiguous taxonomic assignments;
•	an accurate estimation of organismal relative abundance;
•	SGB-level resolution for bacteria, archaea and eukaryotes;
•	strain identification and tracking
•	orders of magnitude speedups compared to existing methods.
•	metagenomic strain-level population genomics




ssh yourusername@login.msi.umn.edu (use ssh yourusername@mangi.msi.umn.edu or ssh yourusername@mesabi.msi.umn.edu respectively)
ssh onyea005@mesabi.msi.umn.edu
	•	Check the folder you are in
pwd
* my home folder is when I first login is “/home/prizm001/onyea005”
* /home/isrania/onyea005
*Change the folder to the israni MSI group
cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
cd /panfs/roc/groups/5/isrania/onyea005
cd /panfs/roc/groups/5/isrania/onyea005/Israni4_Project_016

Creating and MSI script to install Metaphlan and HUMANN 

Learning how to use SLURM to schedule a job since running the humann pipeline on all the files in the folder would be veery time consuming
https://www.msi.umn.edu/slurm
What is Slurm?
Slurm is a best-in-class, highly-scalable scheduler for HPC clusters. It allocates resources, provides a framework for executing tasks, and arbitrates contention for resources by managing queues of pending work.
Why is MSI transitioning to the Slurm scheduler?
Slurm has become an industry standard for scheduling among HPC centers. It’s an open-source scheduler with a plugin framework that allows us to leverage tools developed at other centers. It is capable of stable management of a larger number of jobs than our current scheduler. Finally, it’s architecture opens opportunities to leverage technologies that will be useful for many areas of scientific computation.
How does the transition to Slurm impact my work on MSI systems?
The most obvious adjustment everyone will need to make is to learn a new set of commands for submitting jobs and checking on job status. If you have written scripts that depend on the job scheduler, they will need to be modified to match the syntax used in Slurm. This is also true of some software that MSI maintains.
https://www.msi.umn.edu/content/job-submission-and-scheduling-slurm
Job Submission and Scheduling (Slurm)
MSI systems use job queues to efficiently and fairly manage when computations are executed. When computational jobs are submitted to a job queue they wait in the queue until the appropriate computational resources are available.
 
The queuing system which MSI uses is called Slurm. To submit a job to a Slurm queue, users create Slurm job scripts. Slurm job scripts contain information on the resources requested for the calculation, as well as the commands for executing the calculation.
Slurm Script Format
A Slurm job script is a small text file containing information about what resources a job requires, including time, number of nodes, and memory.   The Slurm script also contains the commands needed to begin executing the desired computation. A sample Slurm job script is shown below.
#!/bin/bash -l        
#SBATCH --time=8:00:00
#SBATCH --ntasks=8
#SBATCH --mem=10g
#SBATCH --tmp=10g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=sample_email@umn.edu 
cd ~/program_directory
module load intel 
module load ompi/intel 
mpirun -np 8 program_name < inputfile > outputfile
The first line in the Slurm script defines which type of shell the script will be read with (how the system will read the file).  It is recommended to make the first line #!/bin/bash -l
Commands for the Slurm queuing system begin with #SBATCH.  Lines 2–4 in the above sample script contain the Slurm resource request.   The sample job will require 8 hours, 8 processor cores,  and 10 gigabytes of memory.  The resource request must contain appropriate values; if the requested time, processors, or memory are not suitable for the hardware the job will not be able to run. 
The two lines containing #SBATCH --mail-type, and #SBATCH --mail-user=sample_email@umn.edu , are both commands having to do with sending message emails to the user. The first of these lines instruct the Slurm system to send a message email when the job aborts, begins, or ends.  Other options here include NONE, BEGIN, END, and FAIL. The second command specifies the email address to be used. Using the message emails is recommended because the reason for a job failure can often be determined using information in the emails.
The rest of the sample Slurm script contains the commands which will be executed to begin the calculation. A Slurm script should contain the appropriate change directory commands to get to the job execution location. A Slurm script also needs to contain module load commands for any software modules that the calculation might need. The last lines of a Slurm script contain commands used to execute the calculation. In the above example the final line contains an execution command to start a program which uses MPI communication to run on 8 processor cores.
Submitting Job Scripts
Once a job script is made it is submitted using the sbatch command:
sbatch -p partitionname scriptname   
Here partitionname is the name of the partition (queue) being submitted to, and scriptname is the name of the job script.  The -p partitionname portion of the command may be omitted, in which case the job would be submitted to whichever partition is set as the default.  Alternatively, the partition specification can be placed inside the job script (see below).
To view all of the jobs submitted by a particular user use the command:
squeue -u username
This command will display the status of the specified jobs, and the associated job ID numbers. The command squeue by itself will show all jobs on the system.
To cancel a submitted job use the command:
scancel jobIDnumber
Here jobIDnumber should be replaced with the appropriate job ID number determined by using the squeue command.
Slurm Script Commands
Below is a table summarizing some commands that can be used inside Slurm job scripts. The first four commands (interpreter specification, and resource request) are required, while the other commands are optional. Each of the below Slurm commands is meant to go on a single line within a Slurm script.
 
Slurm command
Effect
#!/bin/bash -l
Specifies how the Slurm file should be read (by the bash interpreter). A statement like this is required to be the first line of a Slurm script.
#SBATCH --time=8:00:00
Specifies the maximum limit for how long the job will be allowed to run. (8 hours)
#SBATCH --ntasks=8
Specifies the number of processors (cores) that will be reserved for this job.  (8)
#SBATCH --mem=10g
Specifies the maximum limit for memory usage. This job will die if the application tries to use more than 10GB of memory.
#SBATCH --tmp=10g
Specifies 10 GB of temporary disk will be available for this job in /tmp.
#SBATCH --mail-type=ALL  
Specifies which events will trigger an email message. Other options here include NONE, BEGIN, END, and FAIL. 

#SBATCH --mail-user=me@umn.edu
Specifies the email address that should be used when the Slurm system sends message emails.
#SBATCH -p small,mygroup
Specifies the partition to be the “small” partition, or a partition named “mygroup”. The job will start at the earliest time one of these partitions can accommodate the job.
#SBATCH --gres=gpu:v100:2
#SBATCH -p v100
Request two v100 GPUs for a job submitted to the V100 queue.

https://www.msi.umn.edu/partitions#slurm
Slurm Partitions
Under MSI's new scheduler, Slurm, queues are known as partitions. The job partitions on our systems manage different sets of hardware, and have different limits for quantities such as walltime, available processors, and available memory. When submitting a calculation it is important to choose a partition where the job is suited to the hardware and resource limitations.
Selecting a Partition
Each MSI system contains job partitions managing sets of hardware with different resource and policy limitations. MSI currently has two primary systems: the supercomputer Mesabi and the Mesabi expansion Mangi. Mesabi has high-performance hardware and a wide variety of partitions suitable for many different job types. Mangi expands Mesabi and should be your first choice for submitting jobs. Which system to choose depends highly on which system has partitions appropriate for your software/script. More information about selecting a partitions and the different partition parameters can be found on the Choosing A Partition (Slurm) page.
Below is a summary of the available partitions organized by system, and the associated limitations. The quantities listed are totals or upper limits.
Partition name
Node sharing?
Cores per node
Walltime limit
Total node memory
Advised memory per core
Local scratch per node
Maximum Nodes per Job
amdsmall (1)
Yes
128
96:00:00
248 GB
1900 MB
415 GB
1
amdlarge
No
128
24:00:00
248 GB
1900 MB
415 GB
32
amd512
Yes
128
96:00:00
499 GB
4000 MB
415 GB
1
amd2tb
Yes
128
96:00:00
1995 GB
15 GB
415 GB
1
v100 (1)
Yes
24
24:00:00
374 GB
15 GB
859 GB
1
small
Yes
24
96:00:00
60 GB
2500 MB
429 GB
10
large
No
24
24:00:00
60 GB
2500 MB
429 GB
48
max
Yes
24
696:00:00
60 GB
2500 MB
429 GB
1
ram256g
Yes
24
96:00:00
248 GB
10 GB
429 GB
2
ram1t (2)
Yes
32
96:00:00
1002 GB
31 GB
380 GB
2
k40 (1)
Yes
24
24:00:00
123 GB
5 GB
429 GB
40
interactive (3)
Yes
24
24:00:00
60 GB
2 GB
228 GB
2
interactive-gpu (3)
Yes
24
24:00:00
60 GB
2 GB
228 GB
2
preempt (4)
Yes
24
24:00:00
60 GB
2 GB
228 GB
2
preempt-gpu (4)
Yes
24
24:00:00
60 GB
2 GB
228 GB
2

(1) Note: In addition to selecting a GPU partition, GPUs need to be requested for all GPU jobs.  A  k40 GPU can be requested by including the following two lines in your submission script: 
#SBATCH -p k40                                             
#SBATCH --gres=gpu:k40:1
A  V100 GPU can be requested by including the following two lines in your submission script: 
#SBATCH -p v100                                            
#SBATCH --gres=gpu:v100:1
(2) Note: The ram1t nodes contain Intel Ivy Bridge processors, which do not support all of the optimized instructions of the Haswell processors. Programs compiled using the Haswell instructions will only run on the Haswell processors.
(3) Note: Users are limited to 2 jobs in the interactive and interactive-gpu partitions. 
(4) Note: Jobs in the preempt and preempt-gpu partitions may be killed at any time to make room for jobs in the interactive or interactive-gpu partitions.

Submitting Job Scripts
Once a job script is made it is submitted using the sbatch command:
sbatch -p partitionname scriptname   
Here partitionname is the name of the partition (queue) being submitted to, and scriptname is the name of the job script.  The -p partitionname portion of the command may be omitted, in which case the job would be submitted to whichever partition is set as the default.  Alternatively, the partition specification can be placed inside the job script (see
sbatch -p partitionname scriptname   
cd /home/isrania/onyea005
sbatch -p amdsmall HUMANN_MISSION_MSI_SLURM.rtf   
sbatch -p amdsmall HUMANN_MISSION_MSI_SLURM.txt   
**I wasn’t able to figure out how I could save a txt file into a shell script to submit to MSI, so I will use the online script comverter I found

https://support.ceci-hpc.be/doc/_contents/QuickStart/SubmittingJobs/SlurmTutorial.html
http://www.ceci-hpc.be/scriptgen.html

https://ubccr.freshdesk.com/support/solutions/articles/5000688140-submitting-a-slurm-job-script

Create a SLURM script using an editor such as vi or emacs using steps 1 through 3.  The script (or file) can be called anything you want but should end in .sh (i.e. myscript.sh).  If you are unfamiliar with the UNIX commands to edit files, please read this article

http://www.compciv.org/recipes/cli/basic-shell-scripts/

http://www.compciv.org/bash-guide/
https://slurm.schedmd.com/quickstart.html

The gameplan is to save the file as a txt file from word on my mac, move over to windows to forcefully modify the txt extension to SH 

https://support.apple.com/guide/terminal/use-command-line-text-editors-apdb02f1133-25af-4c65-8976-159609f99817/mac

nvm 
https://www.msi.umn.edu/content/job-submission-and-scheduling-slurm
Job Submission and Scheduling (Slurm)
MSI systems use job queues to efficiently and fairly manage when computations are executed. When computational jobs are submitted to a job queue they wait in the queue until the appropriate computational resources are available.
 
The queuing system which MSI uses is called Slurm. To submit a job to a Slurm queue, users create Slurm job scripts. Slurm job scripts contain information on the resources requested for the calculation, as well as the commands for executing the calculation.
Slurm Script Format
A Slurm job script is a small text file containing information about what resources a job requires, including time, number of nodes, and memory.   The Slurm script also contains the commands needed to begin executing the desired computation. A sample Slurm job script is shown below.
#!/bin/bash -l        
#SBATCH --time=8:00:00
#SBATCH --ntasks=8
#SBATCH --mem=10g
#SBATCH --tmp=10g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=sample_email@umn.edu 
cd ~/program_directory
module load intel 
module load ompi/intel 
mpirun -np 8 program_name < inputfile > outputfile
The first line in the Slurm script defines which type of shell the script will be read with (how the system will read the file).  It is recommended to make the first line #!/bin/bash -l
Commands for the Slurm queuing system begin with #SBATCH.  Lines 2–4 in the above sample script contain the Slurm resource request.   The sample job will require 8 hours, 8 processor cores,  and 10 gigabytes of memory.  The resource request must contain appropriate values; if the requested time, processors, or memory are not suitable for the hardware the job will not be able to run. 
The two lines containing #SBATCH --mail-type, and #SBATCH --mail-user=sample_email@umn.edu , are both commands having to do with sending message emails to the user. The first of these lines instruct the Slurm system to send a message email when the job aborts, begins, or ends.  Other options here include NONE, BEGIN, END, and FAIL. The second command specifies the email address to be used. Using the message emails is recommended because the reason for a job failure can often be determined using information in the emails.
The rest of the sample Slurm script contains the commands which will be executed to begin the calculation. A Slurm script should contain the appropriate change directory commands to get to the job execution location. A Slurm script also needs to contain module load commands for any software modules that the calculation might need. The last lines of a Slurm script contain commands used to execute the calculation. In the above example the final line contains an execution command to start a program which uses MPI communication to run on 8 processor cores.
http://www.compciv.org/recipes/cli/basic-shell-scripts/

Let's use the nano text editor to create a shell script named hello.sh. Follow these steps:
	•	Run nano hello.sh
	•	nano should open up and present an empty file for you to work in. Type in the shell command the commands you want to run via SLURM:
Here is the code for the test MISSION metatranscriptomics dataset
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --ntasks=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /home/isrania/onyea005/Israni4_Project_016 
module load conda/python3 
conda activate /home/isrania/onyea005/.conda/envs/mpa 
for f in *.fastq.gz; do humann -i $f -o mission_job; done

Here is the code for the full MISSION metatranscriptomics dataset
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /panfs/roc/groups/5/isrania/onyea005/Israni4_Project_016
module load conda/python3 
conda activate /home/isrania/onyea005/.conda/envs/mpa 
for f in *.fastq.gz; do humann -i $f -o mission_subset --threads 8 --metaphlan-options "--read_min_len 49"; done
Here is the code I will be submitting for the demo dataset using Metaphlan4 (save it in NANO)
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/demo
module load conda/python3 
conda activate /home/isrania/onyea005/.conda/envs/mpa082023
humann --input demo.fastq.gz --output humann_demo --threads 4

Here is how I will create the script to install Metaphlan and human on MSI
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /home/isrania/onyea005/HUMANN082023
module load conda/python3 
conda create --name mpa082023V3 -c conda-forge -c bioconda python=3.7 metaphlan
conda info --envs
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
mkdir -p /home/isrania/onyea005/HUMANN082023/databases3
mkdir -p /home/isrania/onyea005/HUMANN082023/databases3/metaphlan_databases
metaphlan --install --bowtie2db /home/isrania/onyea005/HUMANN082023/databases3/metaphlan_databases
conda config --add channels defaults
conda config --add channels bioconda	
conda config --add channels conda-forge
conda config --add channels biobakery
conda install humann -c biobakery
humann_databases --download chocophlan full /home/isrania/onyea005/HUMANN082023/databases3 --update-config yes
humann_databases --download uniref uniref90_diamond /home/isrania/onyea005/HUMANN082023/databases3 --update-config yes
humann_databases --download utility_mapping full /home/isrania/onyea005/HUMANN082023/databases3 --update-config yes
My 08/11/2023 slurm output was a bit suspicious so I will try a different slurm submission where I will have manually created the databases 4 folders so I don’ t mess around with 
mkdir -p /home/isrania/onyea005/HUMANN082023/databases4
cd /home/isrania/onyea005/HUMANN082023/databases4
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /home/isrania/onyea005/HUMANN082023
module load conda/python3 
conda create --name mpa082023V4 -c conda-forge -c bioconda python=3.7 metaphlan
conda info --envs
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V4
metaphlan --install --bowtie2db /home/isrania/onyea005/HUMANN082023/databases4
conda config --add channels defaults
conda config --add channels bioconda	
conda config --add channels conda-forge
conda config --add channels biobakery
conda install humann -c biobakery
humann_databases --download chocophlan full /home/isrania/onyea005/HUMANN082023/databases4 --update-config yes
humann_databases --download uniref uniref90_diamond /home/isrania/onyea005/HUMANN082023/databases4 --update-config yes
humann_databases --download utility_mapping full /home/isrania/onyea005/HUMANN082023/databases4 --update-config yes
submitting a different slurm script based on my test run of mpa082023V2 to test the metaphlan folded in database
mkdir -p /home/isrania/onyea005/HUMANN082023/databases4
cd /home/isrania/onyea005/HUMANN082023/databases4
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /home/isrania/onyea005/HUMANN082023/demo
module load conda/python3 
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V2
pwd
metaphlan --help	
humann --config	
humann -i demo.fastq.gz -o sample_results --metaphlan-options "demo.fastq.gz --input_type fastq -o sample_results/demo.txt --bowtie2db /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases2/chocophlan"
*Metaphlan does not seem to recognize the databases linked  in the path, so I will try running HUMANN without the metaphlan option (*This gave me the following error “CRITICAL ERROR: The directory provided for ChocoPhlAn contains files ( mpa_latest ) that are not of the expected version. Please install the latest version of the database: v201901_v31” so I have 2 options. 1) create a metaphlan4 environment and run the test file within that one environment (https://omicx.cc/posts/2021-12-17-install-and-setup-metaphlan-3/) 2) create a metaphlan + humann environment and 3 environment, then ask for help on how to point to the correct database in that environment (https://github.com/biobakery/biobakery/wiki/humann3 and https://github.com/biobakery/humann )
mkdir -p /home/isrania/onyea005/HUMANN082023/databases4
cd /home/isrania/onyea005/HUMANN082023/databases4
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /home/isrania/onyea005/HUMANN082023/demo
module load conda/python3 
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V2
pwd
metaphlan --help	
humann --config	
humann -i demo.fastq.gz -o sample_results 
*Trying the metaphlan options suggested in “https://forum.biobakery.org/t/metaphlan4-and-the-new-version-humann/3985/5”
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /home/isrania/onyea005/HUMANN082023/demo
module load conda/python3 
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V2
pwd
metaphlan --help	
humann --config	
humann -i demo.fastq.gz -o sample_results --metaphlan-options "-t  --mpa3 --index mpa_vJan21_CHOCOPhlAnSGB_202103 --bowtie2db /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases3"
















*Trying the human demo coding before trying the metaphlan options suggested in “https://forum.biobakery.org/t/metaphlan4-and-the-new-version-humann/3985/5”
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /home/isrania/onyea005/HUMANN082023/demo
module load conda/python3 
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
pwd
metaphlan --help	
humann --config	
humann -i demo.fastq.gz -o test_results 
#humann -i demo.fastq.gz -o test_results --metaphlan-options "-t  --mpa3 --index mpa_vJan21_CHOCOPhlAnSGB_202103 --bowtie2db /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases3"
*Trying the same option as above but commiting more CPUs to the task
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /home/isrania/onyea005/HUMANN082023/demo
module load conda/python3 
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
pwd
metaphlan --help	
humann --config	
humann -i demo.fastq.gz -o test_results3 --metaphlan-options "--mpa3 --index mpa_vJan21_CHOCOPhlAnSGB_202103 --bowtie2db /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases3" --threads 8
#humann -i demo.fastq.gz -o test_results2 --metaphlan-options "-t  --mpa3 --index mpa_vJan21_CHOCOPhlAnSGB_202103 --bowtie2db /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases3" --threads 8
#humann -i demo.fastq.gz -o test_results --threads 8
*As I mentioned above Metaphlan does not seem to recognize the databases linked  in the path, so I will try running HUMANN without the metaphlan option (*This gave me the following error “CRITICAL ERROR: The directory provided for ChocoPhlAn contains files ( mpa_latest ) that are not of the expected version. Please install the latest version of the database: v201901_v31” so I have 2 options. 1) create a metaphlan4 environment and run the test file within that one environment (https://omicx.cc/posts/2021-12-17-install-and-setup-metaphlan-3/) 2) create a metaphlan + humann environment and 3 environment, then ask for help on how to point to the correct database in that environment (https://github.com/biobakery/biobakery/wiki/humann3 and https://github.com/biobakery/humann )
Install humann and metaphlan without touching the metaphlan databases outside of chocophlan (This will be used to replicate the error so I can ask for help specifying the bowtie database in the humann command)
*I am also transferring the downloaded files from (/Volumes/RAVPOWER2/ROOK_04_PC/metaphlan_databases_082023) to a folder called /home/isrania/onyea005/HUMANN082023/databases3 via Fillezilla
cd /home/isrania/onyea005/HUMANN082023
module load conda/python3 
#conda config --add channels defaults
#conda config --add channels bioconda
#conda config --add channels conda-forge
#conda create --name mpa082023V5 -c bioconda python=3.7 metaphlan
conda create --name mpa082023V5 -c conda-forge -c bioconda python=3.7 metaphlan
conda info --envs
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V5
conda config --add channels defaults
conda config --add channels bioconda	
conda config --add channels conda-forge
conda config --add channels biobakery
conda install humann -c biobakery
humann_databases --download chocophlan full /home/isrania/onyea005/HUMANN082023/databases5 --update-config yes
humann_databases --download uniref uniref90_diamond /home/isrania/onyea005/HUMANN082023/databases5 --update-config yes
humann_databases --download utility_mapping full /home/isrania/onyea005/HUMANN082023/databases5 --update-config yes
	•	Then press Ctrl-X on your keyboard to Exit nano (type in pwd to determine the location of the sh file)
	•	nano will ask you if you want to save the modified file. Hit the y key (for "yes"). > Save the file as MISSION_TEST.sh (or whatever you chose o name)
	•	nano will then confirm if you want to save to the file name. Hit Enter to confirm this.
	•	Here partitionname is the name of the partition (queue) being submitted to, and scriptname is the name of the job script.  The -p partitionname portion of the command may be omitted, in which case the job would be submitted to whichever partition is set as the default.  Alternatively, the partition specification can be placed inside the job script
Going back to the HUMANN tutuorial, here is where we left offAlternatively, if you're comfortable with shell scripting, you can use the following command to process all six at once:


ssh yourusername@login.msi.umn.edu (use ssh yourusername@mangi.msi.umn.edu or ssh yourusername@mesabi.msi.umn.edu respectively)
ssh onyea005@mesabi.msi.umn.edu
	•	Check the folder you are in
pwd
* my home folder is when I first login is “/home/prizm001/onyea005”
* /home/isrania/onyea005
*Change the folder to the israni MSI group
cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
cd /panfs/roc/groups/5/isrania/onyea005
cd /panfs/roc/groups/5/isrania/onyea005/Israni4_Project_016

*Now this can be submitted (home and the /panfs/jay/groups/4/ strings seem to be interchangeable
sbatch -p partitionname scriptname   
cd /home/isrania/onyea005
sbatch -p amdsmall HUMANN_MISSION_MSI_SLURM.rtf   
sbatch -p amdsmall MISSION_TEST.sh
sbatch -p amdsmall MISSION_FULL.sh   
sbatch -p amdsmall humann_demo.sh
sbatch -p amdsmall humann_demo_2.sh   
sbatch -p amdsmall metaphlan_humann_install.sh   
sbatch -p amdsmall metaphlan_humann_install_v2.sh   
sbatch -p amdsmall metaphlan_bowtie2db_chocophlan.sh   
sbatch -p amdsmall metaphlan_bowtie2db_chocophlanv2.sh   
sbatch -p amdsmall humann082023_demo.sh
sbatch -p amdsmall humann_demo_metaphlan_options.sh
sbatch -p amdsmall metaphlan_only_install.sh
sbatch -p amdsmall mpa082023_humann_demo_test.sh
sbatch -p amdsmall mpa082023_humann_demo_test_8CPU.sh
sbatch -p amdsmall mpa082023_humann_demo_test_v3.sh
sbatch -p amdsmall mpa082023_humann_demo_test_v4.sh
 To view all of the jobs submitted by a particular user use the command:
squeue -u username
squeue -u onyea005
This command will display the status of the specified jobs, and the associated job ID numbers. The command squeue by itself will show all jobs on the system.

 To cancel a submitted job use the command:
scancel jobIDnumber
Here jobIDnumber should be replaced with the appropriate job ID number determined by using the squeue command.
https://ubccr.freshdesk.com/support/solutions/articles/5000686861-how-do-i-check-the-status-of-my-job-s-
squeue - Job States
R - Job is running on compute nodes
PD - Job is waiting on compute nodes
CG - Job is completing
squeue - Job Reasons
(Resources) - Job is waiting for compute nodes to become available
(Priority) - Jobs with higher priority are waiting for compute nodes.  Check this knowledge base article for info about job priority
(ReqNodeNotAvail) - The compute nodes requested by the job are not available for a variety of reasons, including:
cluster downtime
nodes offline
temporary scheduling backlog

HUMANN Installation (AUGUST 2023, WORKING)

	•	Connect to VPN for MAC using Cisco AnyConnect Secure Mobility Client
	•	Connect to FileZilla to be able to see the files being moved and created in a dynamic fashion
	•	Enter sftp://login.msi.umn.edu into the Host box, your username into the Username box, your password into the Password box, and 22 into the Port box. Click on Quickconnect. (Tried using sftp://mesabi.msi.umn.edu, username and password since login node changes took place in Jaunary 2021)

	•	Connect to Mesabi (recommended default for MSI to be connected to the supercomputers)
Alternatively, connect to MSI using Terminal on Mac/Linux, located in ApplicationsUtilitiesTerminal.app. 
*NOTE: Like the login node Mesabi and Itasca have specific nomenclatures in the prompt. When connected to Mesabi your prompt will contain ln followed by some number when connected to Itasca your prompt will contain node followed by some numbers. This is a quick short cut to remind you which system you are connected to at any time.
**You need to be connected to Cisco Anyconnect via VPN in order for this command to work and it will ask you to login via DUO

Note to self: 
Running HUMANN pulled the following error
CRITICAL ERROR: Error executing: /home/isrania/onyea005/.conda/envs/mpa082023/bin/metaphlan /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/demo/humann_demo/demo_humann_temp/tmpxoe3r59q/tmpbar25yd4 -t rel_ab -o /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/demo/humann_demo/demo_humann_temp/demo_metaphlan_bugs_list.tsv --input_type fastq --bowtie2out /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/demo/humann_demo/demo_humann_temp/demo_metaphlan_bowtie2.txt --nproc 4
Error message returned from metaphlan :
No MetaPhlAn BowTie2 database found (--index option)!
Expecting location bowtie2db
Exiting...
Potential fixes were discussed here: 
https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4
Important! The MetaPhlAn 4 database has been substantially increased in comparison with the previous 3.1 version. Thus, for running MetaPhlAn 4, a minimum of 15GB or memory is needed.

If you have installed MetaPhlAn using Anaconda, it is advised to install the database in a folder outside the Conda environment. To do this, run
$ metaphlan --install --bowtie2db <database folder>

If you install the database in a different location, remember to run MetaPhlAn using --bowtie2db <database folder>!

By default, the latest MetaPhlAn database is downloaded and built. You can download a specific version with the --index parameter

$ metaphlan --install --index mpa_vJan21_CHOCOPhlAnSGB_202103 --bowtie2db <database folder>

When --index is specified, MetaPhlAn skips the check for the latest database version and run the analysis using the database version provided by --index located in --bowtie2db.

This option is recommended when MetaPhlAn is run on HPC clusters or containerized
If you have issues in downloading the database, you can get it from:
	•	Segatalab FTP

Just download the .tar, .md5, and the mpa_latest files and place them in the metaphlan_databases folder.





















Step 1: Check the installation instructions 

https://forum.biobakery.org/t/announcing-humann-3-5/3995

Step 2: Install Metaphlan 4.0

https://forum.biobakery.org/t/metaphlan4-and-the-new-version-humann/3985/4

https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4

Step 3: Install a new environment of Humann 3.0 (Which upgrades it to 3.5, 3.6 or 3.7)

https://forum.biobakery.org/t/announcing-humann-3-6-critical-update/4155

https://huttenhower.sph.harvard.edu/humann

Step 4: Test HUMANN

https://github.com/biobakery/biobakery/wiki/humann3











Step 1: Check the installation instructions 

https://forum.biobakery.org/t/announcing-humann-3-5/3995

Step 2: Install Metaphlan 4.0

https://forum.biobakery.org/t/metaphlan4-and-the-new-version-humann/3985/4

https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4

MetaPhlAn 4

Home

MetaPhlAn documentation
•	MetaPhlAn 4
•	MetaPhlAn 3.1
•	MetaPhlAn 3.0
•	MetaPhlAn2

StrainPhlAn documentation
•	StrainPhlAn 4
•	StrainPhlAn 3
•	StrainPhlAn
•	Strain Sharing Inference

Workshop on Genomics 2023
•	MetaPhlAn workshop
•	HUMAnN workshop


MetaPhlAn is a computational tool for profiling the composition of microbial communities (Bacteria, Archaea and Eukaryotes) from metagenomic shotgun sequencing data (i.e. not 16S) with species-level. With StrainPhlAn, it is possible to perform accurate strain-level microbial profiling.

MetaPhlAn 4 relies on ~5.1M unique clade-specific marker genes (the latest marker information file can be found here) identified from ~1M microbial genomes (~236,600 references and 771,500 metagenomic assembled genomes) spanning 26,970 species-level genome bins (SGBs, http://segatalab.cibio.unitn.it/data/Pasolli_et_al.html), 4,992 of them taxonomically unidentified at the species level (the full list of the species included in the latest database can be found here), allowing:

•	unambiguous taxonomic assignments;
•	an accurate estimation of organismal relative abundance;
•	SGB-level resolution for bacteria, archaea and eukaryotes;
•	strain identification and tracking
•	orders of magnitude speedups compared to existing methods.
•	metagenomic strain-level population genomics

ssh yourusername@login.msi.umn.edu (use ssh yourusername@mangi.msi.umn.edu or ssh yourusername@mesabi.msi.umn.edu respectively)
ssh onyea005@mesabi.msi.umn.edu
	•	Check the folder you are in
pwd
* my home folder is when I first login is “/home/prizm001/onyea005”
* /home/isrania/onyea005
*Change the folder to the israni MSI group
cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
cd /panfs/roc/groups/5/isrania/onyea005
cd /panfs/roc/groups/5/isrania/onyea005/Israni4_Project_016

Creating and MSI script to install Metaphlan and HUMANN 

Learning how to use SLURM to schedule a job since running the humann pipeline on all the files in the folder would be veery time consuming
https://www.msi.umn.edu/slurm
What is Slurm?
Slurm is a best-in-class, highly-scalable scheduler for HPC clusters. It allocates resources, provides a framework for executing tasks, and arbitrates contention for resources by managing queues of pending work.
Why is MSI transitioning to the Slurm scheduler?
Slurm has become an industry standard for scheduling among HPC centers. It’s an open-source scheduler with a plugin framework that allows us to leverage tools developed at other centers. It is capable of stable management of a larger number of jobs than our current scheduler. Finally, it’s architecture opens opportunities to leverage technologies that will be useful for many areas of scientific computation.
How does the transition to Slurm impact my work on MSI systems?
The most obvious adjustment everyone will need to make is to learn a new set of commands for submitting jobs and checking on job status. If you have written scripts that depend on the job scheduler, they will need to be modified to match the syntax used in Slurm. This is also true of some software that MSI maintains.
https://www.msi.umn.edu/content/job-submission-and-scheduling-slurm
Job Submission and Scheduling (Slurm)
MSI systems use job queues to efficiently and fairly manage when computations are executed. When computational jobs are submitted to a job queue they wait in the queue until the appropriate computational resources are available.
 
The queuing system which MSI uses is called Slurm. To submit a job to a Slurm queue, users create Slurm job scripts. Slurm job scripts contain information on the resources requested for the calculation, as well as the commands for executing the calculation.
Slurm Script Format
A Slurm job script is a small text file containing information about what resources a job requires, including time, number of nodes, and memory.   The Slurm script also contains the commands needed to begin executing the desired computation. A sample Slurm job script is shown below.
#!/bin/bash -l        
#SBATCH --time=8:00:00
#SBATCH --ntasks=8
#SBATCH --mem=10g
#SBATCH --tmp=10g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=sample_email@umn.edu 
cd ~/program_directory
module load intel 
module load ompi/intel 
mpirun -np 8 program_name < inputfile > outputfile
The first line in the Slurm script defines which type of shell the script will be read with (how the system will read the file).  It is recommended to make the first line #!/bin/bash -l
Commands for the Slurm queuing system begin with #SBATCH.  Lines 2–4 in the above sample script contain the Slurm resource request.   The sample job will require 8 hours, 8 processor cores,  and 10 gigabytes of memory.  The resource request must contain appropriate values; if the requested time, processors, or memory are not suitable for the hardware the job will not be able to run. 
The two lines containing #SBATCH --mail-type, and #SBATCH --mail-user=sample_email@umn.edu , are both commands having to do with sending message emails to the user. The first of these lines instruct the Slurm system to send a message email when the job aborts, begins, or ends.  Other options here include NONE, BEGIN, END, and FAIL. The second command specifies the email address to be used. Using the message emails is recommended because the reason for a job failure can often be determined using information in the emails.
The rest of the sample Slurm script contains the commands which will be executed to begin the calculation. A Slurm script should contain the appropriate change directory commands to get to the job execution location. A Slurm script also needs to contain module load commands for any software modules that the calculation might need. The last lines of a Slurm script contain commands used to execute the calculation. In the above example the final line contains an execution command to start a program which uses MPI communication to run on 8 processor cores.
Submitting Job Scripts
Once a job script is made it is submitted using the sbatch command:
sbatch -p partitionname scriptname   
Here partitionname is the name of the partition (queue) being submitted to, and scriptname is the name of the job script.  The -p partitionname portion of the command may be omitted, in which case the job would be submitted to whichever partition is set as the default.  Alternatively, the partition specification can be placed inside the job script (see below).
To view all of the jobs submitted by a particular user use the command:
squeue -u username
This command will display the status of the specified jobs, and the associated job ID numbers. The command squeue by itself will show all jobs on the system.
To cancel a submitted job use the command:
scancel jobIDnumber
Here jobIDnumber should be replaced with the appropriate job ID number determined by using the squeue command.
Slurm Script Commands
Below is a table summarizing some commands that can be used inside Slurm job scripts. The first four commands (interpreter specification, and resource request) are required, while the other commands are optional. Each of the below Slurm commands is meant to go on a single line within a Slurm script.
 
Slurm command
Effect
#!/bin/bash -l
Specifies how the Slurm file should be read (by the bash interpreter). A statement like this is required to be the first line of a Slurm script.
#SBATCH --time=8:00:00
Specifies the maximum limit for how long the job will be allowed to run. (8 hours)
#SBATCH --ntasks=8
Specifies the number of processors (cores) that will be reserved for this job.  (8)
#SBATCH --mem=10g
Specifies the maximum limit for memory usage. This job will die if the application tries to use more than 10GB of memory.
#SBATCH --tmp=10g
Specifies 10 GB of temporary disk will be available for this job in /tmp.
#SBATCH --mail-type=ALL  
Specifies which events will trigger an email message. Other options here include NONE, BEGIN, END, and FAIL. 

#SBATCH --mail-user=me@umn.edu
Specifies the email address that should be used when the Slurm system sends message emails.
#SBATCH -p small,mygroup
Specifies the partition to be the “small” partition, or a partition named “mygroup”. The job will start at the earliest time one of these partitions can accommodate the job.
#SBATCH --gres=gpu:v100:2
#SBATCH -p v100
Request two v100 GPUs for a job submitted to the V100 queue.

https://www.msi.umn.edu/partitions#slurm
Slurm Partitions
Under MSI's new scheduler, Slurm, queues are known as partitions. The job partitions on our systems manage different sets of hardware, and have different limits for quantities such as walltime, available processors, and available memory. When submitting a calculation it is important to choose a partition where the job is suited to the hardware and resource limitations.
Selecting a Partition
Each MSI system contains job partitions managing sets of hardware with different resource and policy limitations. MSI currently has two primary systems: the supercomputer Mesabi and the Mesabi expansion Mangi. Mesabi has high-performance hardware and a wide variety of partitions suitable for many different job types. Mangi expands Mesabi and should be your first choice for submitting jobs. Which system to choose depends highly on which system has partitions appropriate for your software/script. More information about selecting a partitions and the different partition parameters can be found on the Choosing A Partition (Slurm) page.
Below is a summary of the available partitions organized by system, and the associated limitations. The quantities listed are totals or upper limits.
Partition name
Node sharing?
Cores per node
Walltime limit
Total node memory
Advised memory per core
Local scratch per node
Maximum Nodes per Job
amdsmall (1)
Yes
128
96:00:00
248 GB
1900 MB
415 GB
1
amdlarge
No
128
24:00:00
248 GB
1900 MB
415 GB
32
amd512
Yes
128
96:00:00
499 GB
4000 MB
415 GB
1
amd2tb
Yes
128
96:00:00
1995 GB
15 GB
415 GB
1
v100 (1)
Yes
24
24:00:00
374 GB
15 GB
859 GB
1
small
Yes
24
96:00:00
60 GB
2500 MB
429 GB
10
large
No
24
24:00:00
60 GB
2500 MB
429 GB
48
max
Yes
24
696:00:00
60 GB
2500 MB
429 GB
1
ram256g
Yes
24
96:00:00
248 GB
10 GB
429 GB
2
ram1t (2)
Yes
32
96:00:00
1002 GB
31 GB
380 GB
2
k40 (1)
Yes
24
24:00:00
123 GB
5 GB
429 GB
40
interactive (3)
Yes
24
24:00:00
60 GB
2 GB
228 GB
2
interactive-gpu (3)
Yes
24
24:00:00
60 GB
2 GB
228 GB
2
preempt (4)
Yes
24
24:00:00
60 GB
2 GB
228 GB
2
preempt-gpu (4)
Yes
24
24:00:00
60 GB
2 GB
228 GB
2

(1) Note: In addition to selecting a GPU partition, GPUs need to be requested for all GPU jobs.  A  k40 GPU can be requested by including the following two lines in your submission script: 
#SBATCH -p k40                                             
#SBATCH --gres=gpu:k40:1
A  V100 GPU can be requested by including the following two lines in your submission script: 
#SBATCH -p v100                                            
#SBATCH --gres=gpu:v100:1
(2) Note: The ram1t nodes contain Intel Ivy Bridge processors, which do not support all of the optimized instructions of the Haswell processors. Programs compiled using the Haswell instructions will only run on the Haswell processors.
(3) Note: Users are limited to 2 jobs in the interactive and interactive-gpu partitions. 
(4) Note: Jobs in the preempt and preempt-gpu partitions may be killed at any time to make room for jobs in the interactive or interactive-gpu partitions.

Submitting Job Scripts
Once a job script is made it is submitted using the sbatch command:
sbatch -p partitionname scriptname   
Here partitionname is the name of the partition (queue) being submitted to, and scriptname is the name of the job script.  The -p partitionname portion of the command may be omitted, in which case the job would be submitted to whichever partition is set as the default.  Alternatively, the partition specification can be placed inside the job script (see
sbatch -p partitionname scriptname   
cd /home/isrania/onyea005
sbatch -p amdsmall HUMANN_MISSION_MSI_SLURM.rtf   
sbatch -p amdsmall HUMANN_MISSION_MSI_SLURM.txt   
**I wasn’t able to figure out how I could save a txt file into a shell script to submit to MSI, so I will use the online script comverter I found

https://support.ceci-hpc.be/doc/_contents/QuickStart/SubmittingJobs/SlurmTutorial.html
http://www.ceci-hpc.be/scriptgen.html

https://ubccr.freshdesk.com/support/solutions/articles/5000688140-submitting-a-slurm-job-script

Create a SLURM script using an editor such as vi or emacs using steps 1 through 3.  The script (or file) can be called anything you want but should end in .sh (i.e. myscript.sh).  If you are unfamiliar with the UNIX commands to edit files, please read this article

http://www.compciv.org/recipes/cli/basic-shell-scripts/

http://www.compciv.org/bash-guide/
https://slurm.schedmd.com/quickstart.html

The gameplan is to save the file as a txt file from word on my mac, move over to windows to forcefully modify the txt extension to SH 

https://support.apple.com/guide/terminal/use-command-line-text-editors-apdb02f1133-25af-4c65-8976-159609f99817/mac

nvm 
https://www.msi.umn.edu/content/job-submission-and-scheduling-slurm
Job Submission and Scheduling (Slurm)
MSI systems use job queues to efficiently and fairly manage when computations are executed. When computational jobs are submitted to a job queue they wait in the queue until the appropriate computational resources are available.
 
The queuing system which MSI uses is called Slurm. To submit a job to a Slurm queue, users create Slurm job scripts. Slurm job scripts contain information on the resources requested for the calculation, as well as the commands for executing the calculation.
Slurm Script Format
A Slurm job script is a small text file containing information about what resources a job requires, including time, number of nodes, and memory.   The Slurm script also contains the commands needed to begin executing the desired computation. A sample Slurm job script is shown below.
#!/bin/bash -l        
#SBATCH --time=8:00:00
#SBATCH --ntasks=8
#SBATCH --mem=10g
#SBATCH --tmp=10g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=sample_email@umn.edu 
cd ~/program_directory
module load intel 
module load ompi/intel 
mpirun -np 8 program_name < inputfile > outputfile
The first line in the Slurm script defines which type of shell the script will be read with (how the system will read the file).  It is recommended to make the first line #!/bin/bash -l
Commands for the Slurm queuing system begin with #SBATCH.  Lines 2–4 in the above sample script contain the Slurm resource request.   The sample job will require 8 hours, 8 processor cores,  and 10 gigabytes of memory.  The resource request must contain appropriate values; if the requested time, processors, or memory are not suitable for the hardware the job will not be able to run. 
The two lines containing #SBATCH --mail-type, and #SBATCH --mail-user=sample_email@umn.edu , are both commands having to do with sending message emails to the user. The first of these lines instruct the Slurm system to send a message email when the job aborts, begins, or ends.  Other options here include NONE, BEGIN, END, and FAIL. The second command specifies the email address to be used. Using the message emails is recommended because the reason for a job failure can often be determined using information in the emails.
The rest of the sample Slurm script contains the commands which will be executed to begin the calculation. A Slurm script should contain the appropriate change directory commands to get to the job execution location. A Slurm script also needs to contain module load commands for any software modules that the calculation might need. The last lines of a Slurm script contain commands used to execute the calculation. In the above example the final line contains an execution command to start a program which uses MPI communication to run on 8 processor cores.
http://www.compciv.org/recipes/cli/basic-shell-scripts/

Let's use the nano text editor to create a shell script named hello.sh. Follow these steps:
	•	Run nano hello.sh
	•	nano should open up and present an empty file for you to work in. Type in the shell command the commands you want to run via SLURM:
Here is the code for the test MISSION metatranscriptomics dataset
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --ntasks=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /home/isrania/onyea005/Israni4_Project_016 
module load conda/python3 
conda activate /home/isrania/onyea005/.conda/envs/mpa 
for f in *.fastq.gz; do humann -i $f -o mission_job; done

Here is the code for the full MISSION metatranscriptomics dataset
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /panfs/roc/groups/5/isrania/onyea005/Israni4_Project_016
module load conda/python3 
conda activate /home/isrania/onyea005/.conda/envs/mpa 
for f in *.fastq.gz; do humann -i $f -o mission_subset --threads 8 --metaphlan-options "--read_min_len 49"; done

*In my failed installations, Metaphlan did not seem to recognize the databases linked  in the path, so I will try running HUMANN without the metaphlan option (*This gave me the following error “CRITICAL ERROR: The directory provided for ChocoPhlAn contains files ( mpa_latest ) that are not of the expected version. Please install the latest version of the database: v201901_v31” so I have 2 options. 1) create a metaphlan4 environment and run the test file within that one environment (https://omicx.cc/posts/2021-12-17-install-and-setup-metaphlan-3/) 2) create a metaphlan + humann environment and 3 environment, then ask for help on how to point to the correct database in that environment (https://github.com/biobakery/biobakery/wiki/humann3 and https://github.com/biobakery/humann )
**Trying to install metaphlan remotely
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /home/isrania/onyea005/HUMANN082023
module load conda/python3 
conda create --name mpa082023V3 -c conda-forge -c bioconda python=3.7 metaphlan
conda info –envs
	•	Then press Ctrl-X on your keyboard to Exit nano (type in pwd to determine the location of the sh file)
	•	nano will ask you if you want to save the modified file. Hit the y key (for "yes"). > Save the file as MISSION_TEST.sh (or whatever you chose o name)
	•	nano will then confirm if you want to save to the file name. Hit Enter to confirm this.
	•	Here partitionname is the name of the partition (queue) being submitted to, and scriptname is the name of the job script.  The -p partitionname portion of the command may be omitted, in which case the job would be submitted to whichever partition is set as the default.  Alternatively, the partition specification can be placed inside the job script

ssh yourusername@login.msi.umn.edu (use ssh yourusername@mangi.msi.umn.edu or ssh yourusername@mesabi.msi.umn.edu respectively)
ssh onyea005@mesabi.msi.umn.edu
	•	Check the folder you are in
pwd
* my home folder is when I first login is “/home/prizm001/onyea005”
* /home/isrania/onyea005
*Change the folder to the israni MSI group
cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
cd /panfs/roc/groups/5/isrania/onyea005
cd /panfs/roc/groups/5/isrania/onyea005/Israni4_Project_016

*Now this can be submitted (home and the /panfs/jay/groups/4/ strings seem to be interchangeable
sbatch -p partitionname scriptname   
cd /home/isrania/onyea005
sbatch -p amdsmall HUMANN_MISSION_MSI_SLURM.rtf   
sbatch -p amdsmall MISSION_TEST.sh
sbatch -p amdsmall MISSION_FULL.sh   
sbatch -p amdsmall humann_demo.sh
sbatch -p amdsmall humann_demo_2.sh   
sbatch -p amdsmall metaphlan_humann_install.sh   
sbatch -p amdsmall metaphlan_humann_install_v2.sh   
sbatch -p amdsmall metaphlan_bowtie2db_chocophlan.sh   
sbatch -p amdsmall metaphlan_bowtie2db_chocophlanv2.sh   
sbatch -p amdsmall humann082023_demo.sh
sbatch -p amdsmall humann_demo_metaphlan_options.sh
sbatch -p amdsmall metaphlan_only_install.sh
sbatch -p amdsmall mpa082023_humann_demo_test.sh
sbatch -p amdsmall mpa082023_humann_demo_test_8CPU.sh
sbatch -p amdsmall mpa082023_humann_demo_test_v3.sh
sbatch -p amdsmall mpa082023_humann_demo_test_v4.sh
 To view all of the jobs submitted by a particular user use the command:
squeue -u username
squeue -u onyea005
This command will display the status of the specified jobs, and the associated job ID numbers. The command squeue by itself will show all jobs on the system.

 To cancel a submitted job use the command:
scancel jobIDnumber
Here jobIDnumber should be replaced with the appropriate job ID number determined by using the squeue command.
https://ubccr.freshdesk.com/support/solutions/articles/5000686861-how-do-i-check-the-status-of-my-job-s-
squeue - Job States
R - Job is running on compute nodes
PD - Job is waiting on compute nodes
CG - Job is completing
squeue - Job Reasons
(Resources) - Job is waiting for compute nodes to become available
(Priority) - Jobs with higher priority are waiting for compute nodes.  Check this knowledge base article for info about job priority
(ReqNodeNotAvail) - The compute nodes requested by the job are not available for a variety of reasons, including:
cluster downtime
nodes offline
temporary scheduling backlog
*It looks like metaphlan was successfully installed via slurm, so I can try the following options bellow

Log in to an interactive node, making sure you have enough time and memory (https://www.msi.umn.edu/content/interactive-queue-use-isub)

isub -n nodes=1:ppn=8 -m 16GB -w 8:00:00	
#I don’t think I need to log into mesabi or Itasca to run this job if I am logged into the interactive node since it logs me into the lab queue (So use mesabi instead)
MSI's LabQi Cluster will be shut down and decommissioned during February Maintenance (February 5, 2020.) Several months ago, MSI released the Mesabi Interactive Queue as a replacement for the LabQi Cluster (ssh lab.)  The Mesabi Interactive Queue can be used in real-time for tasks such as interactive data exploration, creating plots or images, visualizations, or testing sections of code that will be used on other MSI queues.

https://www.msi.umn.edu/content/interactive-hpc

What is Interactive HPC?

MSI’s interactive HPC resources are composed of some of the same systems used to support our batch scheduled HPC activities. The main difference between the two services is the way in which users interact with the MSI resource. Interactive HPC systems allow real-time user inputs in order to facilitate code development, real-time data exploration, and visualizations. Interactive HPC systems are used when data are too large to download to a desktop or laptop, software is difficult or impossible to install on a personal machine, or specialized hardware resources (e.g., GPUs) are needed to visualize large datasets. User input might come through a command line interface (i.e., system shell) while debugging a piece of software or through mouse clicks while using a program with a graphical interface.

What can I do with Interactive HPC?

Unlike MSI’s other computing options, the Interactive HPC resources are designed only to be used in real time for tasks such as interactive data exploration, creating plots or images, visualizations, or testing sections of code that will be used on other MSI systems.

Common use cases include:
	•	Testing a compiled program at the command line before submitting it as a job
	•	Developing code or exploring data in an interactive environment like MATLAB, IPython, or Rstudio
	•	Visualizing data or building simulations in applications like Schrodinger, COMSOL, or Avizo

What types of Interactive HPC are available?
	•	srun:  Submit an interactive job on the current HPC system to provide a command line or graphical interface to interactively run software. 
	•	NICE: Connects a user to a graphical environment, which may have specialized GPUs for visualizing large datasets.
	•	Total Slots: 32 slots for accelerated remote desktops
	•	Total Memory:  Up to 16 GB/session
	•	Graphical Accelerators: Nvidia GK107GL
	•	CITRIX: Citrix for Windows Environments: Connects a user to a graphical Windows environment, where they can access specialized Windows software for modeling and data analysis.
	•	NX NoMachine: Connects a user to a graphical environment, which may be used as a lightweight remote desktop for connecting to other MSI resources.
	•	Jupyter Notebooks: A web portal for notebook-based computing in the browser, which can be used for reproducible and shareable data analysis, visualization, and scripted control of larger tasks. Currently supports Python 2, Python 3, and R. 
Here are several examples of how you can request an interactive HPC job on the new Mesabi Interactive Queue.

The basic syntax for submitting an interactive job on Mesabi (I assume that in combination with the new login on mesabi and mangi the syntax occurs after login into mesabi.msi.umn.edu):

qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I

The basic syntax for testing MPI code between 2 nodes:
qsub -l nodes=2:ppn=2,mem=8gb,walltime=12:00:00 -q interactive -W x=nmatchpolicy:exactnode -I

The basic syntax for creating a job with X-forwarding for GUI's and visulization:
qsub -X -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I

Back to the interactive login
qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I

The full specifications of the Mesabi Interactive Queue can be found on the MSI Queues webpage. Also, more examples on how to use qsub -I for interactive use can be found on the Interactive queue use with qsub webpage.
https://www.msi.umn.edu/content/interactive-queue-use-srun
** Will need to learn how to use Slurm to schedule jobs (https://www.msi.umn.edu/slurm)
Interactive queue use with srun
It is possible to start an interactive job on any of MSI’s clusters that support job submission. Interactive jobs can be useful for tasks including data exploration, development, or (with X11 forwarding) visualization activities.
The basic syntax for submitting an interactive job is illustrated using the following example for Mesabi:
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=1gb -t 1:00:00 -p interactive --pty bash
If you are submitting an interactive job from a session with X11 forwarding and would like to continue using X11 forwarding inside your job, you would add the additional flag ‘-x11’, as follows:
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=1gb -t 1:00:00 -p interactive --x11 --pty bash
The basic syntax for requesting two nodes, for instance to test MPI code between two nodes:
srun -N 2 --ntasks-per-node=1  --mem-per-cpu=1gb -t 1:00:00 -p interactive --pty bash
The syntax for requesting an interactive gpu node with a k40 GPU is:
srun -n 12 -t 1:00:00 -p interactive-gpu --gres=gpu:k40:1 --pty bash
You may also submit an interactive job using an interactive submission script. For instance, to submit an interactive job to test an MPI code (in our case named ‘interactive.sh’) with contents:
#SBATCH -N 2
#SBATCH --ntasks-per-node=1
#SBATCH --mem-per-cpu=1gb
#SBATCH -t 1:00:00
#SBATCH -p interactive
./my_application
You would then submit this job using:
sbatch interactive.sh
Interactive jobs are subject to the same rules and limitations as batch jobs. Interactive jobs have access to only those partitions that are available on the cluster they are submitted from. Jobs that request more resources may wait in the queue longer, so be sure that you are requesting only those resources that you need.
 An interactive job supports all of the usual Slurm commands. These commands are included as flags when requesting an interactive job. 
 Example interactive srun Session
 username@mydesktop$ ssh -Y msiusername@mesabi.msi.umn.edu
Password:
Last login: Tue May 10 15:16:06 2018 from mydesktop.mydept.umn.edu
-------------------------------------------------------------------------------
            University of Minnesota Supercomputing Institute
                                Mesabi
                        HP Haswell Linux Cluster
-------------------------------------------------------------------------------
[... output truncated ...]
---------------
username@ln0006 [~] % srun -N 2 --ntasks-per-node=1  --mem-per-cpu=1gb --x11 -t 1:00:00 --pty bash
username@cn0001 [~] % 
username@cn0001 [~] % echo $SLURM_JOB_NODELIST
cn[0001-0002]
username@cn0001 [~] %
***Note that this interactive job is using two nodes (-N 2). At this point you are currently in your home directory, and can load modules and launch software, including GUI software.

Back to the interactive login using srun (might take a few mins to get the access)

*Old syntax
qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I
*New syntax
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=16gb -t 4:00:00 -p interactive --pty bash
Now you can look at the contents of the directory using the ls command:

ls -lh
You can always check the location of your current directory (aka folder) using the pwd command.  If you type:

pwd
Login into an interactive node to process the data
*I will try to add a CPU per task option to the interactive run and to the sbatch file
https://www.msi.umn.edu/support/faq/how-can-i-use-gnu-parallel-run-lot-commands-parallel
https://www.msi.umn.edu/content/interactive-queue-use-srun
Interactive queue use with srun
It is possible to start an interactive job on any of MSI’s clusters that support job submission. Interactive jobs can be useful for tasks including data exploration, development, or (with X11 forwarding) visualization activities.
 The basic syntax for submitting an interactive job is illustrated using the following example for Mesabi:
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=1gb -t 1:00:00 -p interactive --pty bash
If you are submitting an interactive job from a session with X11 forwarding and would like to continue using X11 forwarding inside your job, you would add the additional flag ‘-x11’, as follows:
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=1gb -t 1:00:00 -p interactive --x11 --pty bash
The basic syntax for requesting two nodes, for instance to test MPI code between two nodes:
srun -N 2 --ntasks-per-node=1  --mem-per-cpu=1gb -t 1:00:00 -p interactive --pty bash
The syntax for requesting an interactive gpu node with a k40 GPU is:
srun -n 12 -t 1:00:00 -p interactive-gpu --gres=gpu:k40:1 --pty bash
https://stackoverflow.com/questions/51139711/hpc-cluster-select-the-number-of-cpus-and-threads-in-slurm-sbatch
The terminology used in the sbatch man page might be a bit confusing. Thus, I want to be sure I am getting the options set right. Suppose I have a task to run on a single node with N threads. Am I correct to assume that I would use --nodes=1 and --ntasks=N?
I am used to thinking about using, for example, pthreads to create N threads within a single process. Is the result of that what they refer to as "cores" or "cpus per task"? CPUs and threads are not the same things in my mind.

Depending on the parallelism you are using: distributed or shared memory
--ntasks=# : Number of "tasks" (use with distributed parallelism).
--ntasks-per-node=# : Number of "tasks" per node (use with distributed parallelism).
--cpus-per-task=# : Number of CPUs allocated to each task (use with shared memory parallelism).

From this question: if every node has 24 cores, is there any difference between these commands?
sbatch --ntasks 24 [...]
sbatch --ntasks 1 --cpus-per-task 24 [...]
Answer: (by Matthew Mjelde)
Yes there is a difference between those two submissions. You are correct that usually ntasks is for mpi and cpus-per-task is for multithreading, but let’s look at your commands:
For your first example, the sbatch --ntasks 24 […] will allocate a job with 24 tasks. These tasks in this case are only 1 CPUs, but may be split across multiple nodes. So you get a total of 24 CPUs across multiple nodes.
For your second example, the sbatch --ntasks 1 --cpus-per-task 24 [...] will allocate a job with 1 task and 24 CPUs for that task. Thus you will get a total of 24 CPUs on a single node.
In other words, a task cannot be split across multiple nodes. Therefore, using --cpus-per-task will ensure it gets allocated to the same node, while using --ntasks can and may allocate it to multiple nodes.

Another good Q&A from CÉCI's support website: Suppose you need 16 cores. Here are some use cases:
	•	you use mpi and do not care about where those cores are distributed: --ntasks=16
	•	you want to launch 16 independent processes (no communication): --ntasks=16
	•	you want those cores to spread across distinct nodes: --ntasks=16 and --ntasks-per-node=1 or --ntasks=16 and --nodes=16
	•	you want those cores to spread across distinct nodes and no interference from other jobs: --ntasks=16 --nodes=16 --exclusive
	•	you want 16 processes to spread across 8 nodes to have two processes per node: --ntasks=16 --ntasks-per-node=2
	•	you want 16 processes to stay on the same node: --ntasks=16 --ntasks-per-node=16
	•	you want one process that can use 16 cores for multithreading: --ntasks=1 --cpus-per-task=16
	•	you want 4 processes that can use 4 cores each for multithreading: --ntasks=4 --cpus-per-task=4

https://stackoverflow.com/questions/49466020/specify-number-of-cpus-for-a-job-on-slurm
https://researchcomputing.princeton.edu/support/knowledge-base/scaling-analysis
https://docs-research-it.berkeley.edu/services/high-performance-computing/user-guide/running-your-jobs/scheduler-examples/
Back to the interactive login using srun (might take a few mins to get the access)

https://www.msi.umn.edu/content/interactive-queue-use-srun
*Old syntax
qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I
*New syntax
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=16gb -t 4:00:00 -p interactive --pty bash

*Testing out adding multiple CPUs to run the code faster (https://www.msi.umn.edu/support/faq)
It seems like nodes and CPU is different so the n-task per node is what determine thee number of CPUs we can use?
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=32gb -t 4:00:00 -p interactive --pty bash
*I will use this code to ask for multiple CPUs for the analysis)https://stackoverflow.com/questions/51139711/hpc-cluster-select-the-number-of-cpus-and-threads-in-slurm-sbatch
--ntasks=4 --cpus-per-task=4

srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=32gb -t 4:00:00 -p interactive --pty bash

*Per the SLURM PDF guide (See slide 10) the interactive queue has a maximum of 4 cores/CPU that can be requested through srun. I will request 8 when submitting a job via SLURM
srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=32gb -t 4:00:00 -p interactive --pty bash

srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=8gb -t 4:00:00 -p interactive --pty bash

*Setting the srun to 16GB since metaphlan 4 mentioned that it needs 15GB to run
srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=16gb -t 12:00:00 -p interactive --pty bash
srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=32gb -t 12:00:00 -p interactive --pty bash

cd /home/isrania/onyea005/HUMANN082023
module load conda/python3 
conda info --envs
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
metaphlan --version
conda config --add channels defaults
conda config --add channels bioconda	
conda config --add channels conda-forge
conda config --add channels biobakery
conda install humann -c biobakery
	•	I got an error message, so I will try reinstalling humann (everything looked ok)
conda install humann -c biobakery
humann_test
humann_databases --download chocophlan full /home/isrania/onyea005/HUMANN082023/databases3 --update-config yes
humann_databases --download uniref uniref90_diamond /home/isrania/onyea005/HUMANN082023/databases5 --update-config yes
humann_databases --download utility_mapping full /home/isrania/onyea005/HUMANN082023/databases5 --update-config yes
HUMANN Analysis 082023 demo run (WORKING)

	•	Connect to VPN for MAC using Cisco AnyConnect Secure Mobility Client
	•	Connect to FileZilla to be able to see the files being moved and created in a dynamic fashion
	•	Enter sftp://login.msi.umn.edu into the Host box, your username into the Username box, your password into the Password box, and 22 into the Port box. Click on Quickconnect. (Tried using sftp://mesabi.msi.umn.edu, username and password since login node changes took place in Jaunary 2021)

	•	Connect to Mesabi (recommended default for MSI to be connected to the supercomputers)
Alternatively, connect to MSI using Terminal on Mac/Linux, located in ApplicationsUtilitiesTerminal.app. 
*NOTE: Like the login node Mesabi and Itasca have specific nomenclatures in the prompt. When connected to Mesabi your prompt will contain ln followed by some number when connected to Itasca your prompt will contain node followed by some numbers. This is a quick short cut to remind you which system you are connected to at any time.
**You need to be connected to Cisco Anyconnect via VPN in order for this command to work and it will ask you to login via DUO




ssh yourusername@login.msi.umn.edu (use ssh yourusername@mangi.msi.umn.edu or ssh yourusername@mesabi.msi.umn.edu respectively)
ssh onyea005@mesabi.msi.umn.edu
	•	Check the folder you are in
pwd
* my home folder is when I first login is “/home/prizm001/onyea005”
* /home/isrania/onyea005
*Change the folder to the israni MSI group
cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
cd /panfs/roc/groups/5/isrania/onyea005
cd /panfs/roc/groups/5/isrania/onyea005/Israni4_Project_016




https://github.com/biobakery/biobakery/wiki/humann3

I will now pick up on the HUMANN tutorial

Welcome to the HUMAnN 3.0 tutorial

Tutorials for previous versions of HUMAnN:
	•	HUMAnN 1.0 tutorial
	•	HUMAnN 2.0 tutorial
HUMAnN (the HMP Unified Metabolic Analysis Network) is a method for efficiently and accurately profiling the abundance of microbial metabolic pathways and other molecular functions from metagenomic or metatranscriptomic sequencing data. It is appropriate for any type of microbial community, not just the human microbiome (the name "HUMAnN" is a historical product of the method's origins in the Human Microbiome Project).

HUMAnN 3.0 can be installed 1) from PyPI, 2) as source code from the HUMAnN Github repository, or 3) using our quickstart documentation.

Support for HUMAnN is available via the HUMAnN channel of the bioBakery Support Forum.

Table of Contents
	•	1. Setup
	•	1.1 Requirements
	•	1.2 Installation
	•	1.2.1 Install HUMAnN
	•	1.2.2 Install MetaPhlAn
	•	2. Metagenome functional profiling
	•	2.1 HUMAnN input formats
	•	2.2 Running HUMAnN: the basics
	•	2.3 HUMAnN default outputs
	•	demo_genefamilies.tsv
	•	demo_pathabundance.tsv
	•	demo_pathcoverage.tsv
	•	2.4 Running HUMAnN: pre-mapped SAM/BAM input
	•	2.5 Running HUMAnN: pre-computed protein blastx M8 input
	•	2.6 Running HUMAnN: skipping translated search
	•	3. Manipulating HUMAnN output tables
	•	3.1 Normalizing RPKs to relative abundance
	•	3.2 Regrouping genes to other functional categories
	•	3.3 Attaching names to features
	•	4. Advanced HUMAnN topics
	•	4.1 HUMAnN intermediate files
	•	4.2 HUMAnN for multiple samples
	•	5. Plotting stratified functions


Overview


1. Setup
1.1 Requirements
	•	Python (>= 3.7)
	•	MetaPhlAn (version >= 3.0)
	•	Bowtie2 (version >= 2.2)
	•	DIAMOND (=0.9.36)
	•	MinPath
NOTE: Bowtie2, DIAMOND, and MinPath are automatically installed when installing HUMAnN. If these dependencies do not appear to be installed after installing HUMAnN with pip, it might be that your environment is setup to use wheels instead of installing from source. HUMAnN must be installed from source for it to also be able to install dependencies. To force pip to install HUMAnN from source, add one of the following options to your install command, --no-use-wheel or --no-binary :all:.

1.2 Installation
NOTE: If you are using a bioBakery machine image (e.g. in Google Cloud) you do not need to install HUMAnN because the software and its dependencies are already available.

1.2.1 Install HUMAnN
The easiest way to install HUMAnN is with pip, Conda, or Docker. For other options, please refer to the HUMAnN User Manual.

To install with pip:
pip install humann

You can also install via Conda, which will additionally install MetaPhlAn and utility dependencies. Before doing so, make sure to configure your channels so that biobakery is at the top of the list:

conda install -c biobakery humann

By default, a pip (or other) HUMAnN install does not include the full-scale nucleotide and protein sequence databases needed for real-world metagenome or metatranscriptome profiling, as these are quite large (several GBs). The humann_databases script can be used to fetch these from the web and connect them to your HUMAnN installation. For tutorial purposes, we'll be using (small) demo databases, which are also fetchable with humann_databases (if not already available).

humann_databases --download chocophlan DEMO humann_dbs
humann_databases --download uniref DEMO_diamond humann_dbs

The commands above download the demo-scale databases to a folder named humann_dbs in the current working directory. Please note that you can download these (and full-scale) databases to any location on your machine: HUMAnN will track where you have placed the databases and update its configuration file.

After installation from pip or another source, you may optionally test your local HUMAnN environment (this takes ~1 minute):

humann_test

Which yields (highly abbreviated):

test_calculate_percent_identity (basic_tests_nucleotide_search.TestBasicHumann2NucleotideSearchFunctions) ... ok
test_calculate_percent_identity_insert_delete (basic_tests_nucleotide_search.TestBasicHumann2NucleotideSearchFunctions) ... ok
test_calculate_percent_identity_missing_cigar_identifier (basic_tests_nucleotide_search.TestBasicHumann2NucleotideSearchFunctions) ... ok
test_calculate_percent_identity_missing_md_field (basic_tests_nucleotide_search.TestBasicHumann2NucleotideSearchFunctions) ... ok
test_calculate_percent_identity_multiple_M_cigar_fields (basic_tests_nucleotide_search.TestBasicHumann2NucleotideSearchFunctions) ... ok
...

1.2.2 Install MetaPhlAn

HUMAnN provides organism-specific community functional profiles, and to do so it first detects the organisms present in a community using MetaPhlAn. As MetaPhlAn is thus a prerequisite for running the HUMAnN software, please ensure that you've installed MetaPhlAn before running HUMAnN. You can test if MetaPhlAn is available in your system path by executing 

metaphlan --version.

pwd
cd /home/isrania/onyea005/HUMANN082023	
humann_test
metaphlan --version
I have tested the HUMANN installation and checked that metaphlan is installed

2. Metagenome functional profiling

A note on inspecting files from the command line: During this tutorial you'll inspect many HUMAnN input and output files, many of which are tab-delimitted tables (TSV files). For simplicity the tutorial uses less -S file.tsv for this task. To match the more spreadsheet-like columns seen in the tutorial document, you would first use the column command, as in:

column -t -s $'\t' file.tsv | less -S

2.1 HUMAnN input formats

HUMAnN can start from a few different types of input data each in a few different types of formats:

Quality-controlled shotgun sequencing reads
This is the most common starting point
A metagenome (DNA reads) or metatranscriptome (RNA reads)
Formats: fastq, fastq.gz, fasta, or fasta.gz
Pre-computed mappings of reads to database sequences
Formats: sam, bam, or blastm8
Pre-computed (typically gene) abundance tables
Formats: tsv or biom

This tutorial will use demo input files distributed with the HUMAnN installation under the examples directory. If you installed with pip or conda and don't have these handy, you can obtain copies by right-clicking these links and selecting "save link as":

demo.fastq.gz
demo.sam
demo.m8

*Skip the attempt to copy the files from MSI below, I will use filezilla but download things overnight due to the long duration
cd /panfs/roc/data_release/0/umgc/cmstaley/nextseq/210712_NB551164_0296_AH2C37BGXH/Staley_Project_069
cp /panfs/roc/data_release/0/umgc/cmstaley/nextseq/210712_NB551164_0296_AH2C37BGXH/Staley_Project_069 /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Staley_Project_069_v2

pwd
cd /home/isrania/onyea005/HUMANN082023
After right clicking and downloading the 3 files on my local computer, I created a folder call demo at /home/isrania/onyea005/HUMANN082023/demo and moved the three files there

2.2 Running HUMAnN: the basics
If you are running this tutorial as part of a short course, all input files have been pre-downloaded into the cloud instances. To run this tutorial from the correct location, enter:

cd ~/Tutorials/humann3
cd /home/isrania/onyea005/HUMANN082023/demo
Check if all input files are available in the input directory with ls. We will copy files from this folder to the current working directory over the course of the tutorial. Let us start with the aforementioned 3 demo input files.

cp input/demo* 

A canonical HUMAnN run uses nucleotide mapping and translated search to provide organism-specific gene and pathway abundance profiles from a single metagenome. By default, gene families are annotated using UniRef90 definitions and pathways using MetaCyc definitions. From the examples folder (or the location where you saved demo.fastq.gz above) execute the following:

humann --input demo.fastq.gz --output demo_fastq

humann --input demo.fastq.gz --output demo_fastq
*With Metaphlan4 you probably want to run the updated version. (https://forum.biobakery.org/t/metaphlan4-run-time/5017)
humann --input demo.fastq.gz --output demo_fastq --threads 4

Since you have already downloaded the databases in the initial installation, you do not need to download the databases again unless there are new versions available. However, you will want to update your latest HUMAnN 3.0 install to point to the databases you have downloaded as by default the new install configuration will point to the demo databases.

To update your HUMAnN 3.0 configuration file to include the locations of your downloaded databases, please use the following steps.

Update the location of the ChocoPhlAn database (replacing $DIR with the full path to the directory containing the ChocoPhlAn database)


cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_config --update database_folders nucleotide $DIR
humann_config --update database_folders nucleotide /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases
humann_config --update database_folders nucleotide /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases/chocophlan

Update the location of the UniRef database (replacing $DIR with the full path to the directory containing the UniRef database)


cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_config --update database_folders protein $DIR

humann_config --update database_folders protein /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases/uniref

A canonical HUMAnN run uses nucleotide mapping and translated search to provide organism-specific gene and pathway abundance profiles from a single metagenome. By default, gene families are annotated using UniRef90 definitions and pathways using MetaCyc definitions. From the examples folder (or the location where you saved demo.fastq.gz above) execute the following:

humann --input demo.fastq.gz --output demo_fastq

humann --input demo.fastq.gz --output demo_fastq
*With Metaphlan4 you probably want to run the updated version. (https://forum.biobakery.org/t/metaphlan4-run-time/5017)
humann --input demo.fastq.gz --output demo_fastq --threads 4

(Note1: If you have more than 1 CPU available for computation, you can use N of them by adding --threads N to the above command.)

** I am getting the following error

Downloading MetaPhlAn database
Please note due to the size this might take a few minutes

\Downloading and uncompressing indexes

File /home/isrania/onyea005/.conda/envs/mpa082023/lib/python3.7/site-packages/metaphlan/metaphlan_databases/mpa_vOct22_CHOCOPhlAnSGB_202212_bt2.tar already present!

File /home/isrania/onyea005/.conda/envs/mpa082023/lib/python3.7/site-packages/metaphlan/metaphlan_databases/mpa_vOct22_CHOCOPhlAnSGB_202212_bt2.md5 already present!
MD5 checksums do not correspond! If this happens again, you should remove the database files and rerun MetaPhlAn so they are re-downloaded
https://forum.biobakery.org/t/metaphlan-3-md5-error/484
https://forum.biobakery.org/t/humann3-metaphlan-3-0-error-md5-checksums-do-not-match/619/4
https://github.com/biobakery/MetaPhlAn/issues/100
https://groups.google.com/g/metaphlan-users/c/5ltD8X8Xitc?pli=1

*First I will try to reinstall metaphlan within the environment

https://forum.biobakery.org/t/announcing-humann-3-5/3995
	•	Upgrading MetaPhlAn:
conda install -c bioconda metaphlan=4.0.0

*Then I tried rerunning the Humann command

A canonical HUMAnN run uses nucleotide mapping and translated search to provide organism-specific gene and pathway abundance profiles from a single metagenome. By default, gene families are annotated using UniRef90 definitions and pathways using MetaCyc definitions. From the examples folder (or the location where you saved demo.fastq.gz above) execute the following:

humann --input demo.fastq.gz --output demo_fastq

humann --input demo.fastq.gz --output demo_fastq
*With Metaphlan4 you probably want to run the updated version. (https://forum.biobakery.org/t/metaphlan4-run-time/5017)
humann --input demo.fastq.gz --output demo_fastq --threads 4




*Now that HUMANN has installed the databases via srun, I can go back to SLURM and schedule jobs to run the  demo dataset
Creating and MSI script to install Metaphlan and HUMANN 

Learning how to use SLURM to schedule a job since running the humann pipeline on all the files in the folder would be veery time consuming
https://www.msi.umn.edu/slurm
What is Slurm?
Slurm is a best-in-class, highly-scalable scheduler for HPC clusters. It allocates resources, provides a framework for executing tasks, and arbitrates contention for resources by managing queues of pending work.
Why is MSI transitioning to the Slurm scheduler?
Slurm has become an industry standard for scheduling among HPC centers. It’s an open-source scheduler with a plugin framework that allows us to leverage tools developed at other centers. It is capable of stable management of a larger number of jobs than our current scheduler. Finally, it’s architecture opens opportunities to leverage technologies that will be useful for many areas of scientific computation.
How does the transition to Slurm impact my work on MSI systems?
The most obvious adjustment everyone will need to make is to learn a new set of commands for submitting jobs and checking on job status. If you have written scripts that depend on the job scheduler, they will need to be modified to match the syntax used in Slurm. This is also true of some software that MSI maintains.
https://www.msi.umn.edu/content/job-submission-and-scheduling-slurm
Job Submission and Scheduling (Slurm)
MSI systems use job queues to efficiently and fairly manage when computations are executed. When computational jobs are submitted to a job queue they wait in the queue until the appropriate computational resources are available.
 
The queuing system which MSI uses is called Slurm. To submit a job to a Slurm queue, users create Slurm job scripts. Slurm job scripts contain information on the resources requested for the calculation, as well as the commands for executing the calculation.
Slurm Script Format
A Slurm job script is a small text file containing information about what resources a job requires, including time, number of nodes, and memory.   The Slurm script also contains the commands needed to begin executing the desired computation. A sample Slurm job script is shown below.
#!/bin/bash -l        
#SBATCH --time=8:00:00
#SBATCH --ntasks=8
#SBATCH --mem=10g
#SBATCH --tmp=10g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=sample_email@umn.edu 
cd ~/program_directory
module load intel 
module load ompi/intel 
mpirun -np 8 program_name < inputfile > outputfile
The first line in the Slurm script defines which type of shell the script will be read with (how the system will read the file).  It is recommended to make the first line #!/bin/bash -l
Commands for the Slurm queuing system begin with #SBATCH.  Lines 2–4 in the above sample script contain the Slurm resource request.   The sample job will require 8 hours, 8 processor cores,  and 10 gigabytes of memory.  The resource request must contain appropriate values; if the requested time, processors, or memory are not suitable for the hardware the job will not be able to run. 
The two lines containing #SBATCH --mail-type, and #SBATCH --mail-user=sample_email@umn.edu , are both commands having to do with sending message emails to the user. The first of these lines instruct the Slurm system to send a message email when the job aborts, begins, or ends.  Other options here include NONE, BEGIN, END, and FAIL. The second command specifies the email address to be used. Using the message emails is recommended because the reason for a job failure can often be determined using information in the emails.
The rest of the sample Slurm script contains the commands which will be executed to begin the calculation. A Slurm script should contain the appropriate change directory commands to get to the job execution location. A Slurm script also needs to contain module load commands for any software modules that the calculation might need. The last lines of a Slurm script contain commands used to execute the calculation. In the above example the final line contains an execution command to start a program which uses MPI communication to run on 8 processor cores.
Submitting Job Scripts
Once a job script is made it is submitted using the sbatch command:
sbatch -p partitionname scriptname   
Here partitionname is the name of the partition (queue) being submitted to, and scriptname is the name of the job script.  The -p partitionname portion of the command may be omitted, in which case the job would be submitted to whichever partition is set as the default.  Alternatively, the partition specification can be placed inside the job script (see below).
To view all of the jobs submitted by a particular user use the command:
squeue -u username
This command will display the status of the specified jobs, and the associated job ID numbers. The command squeue by itself will show all jobs on the system.
To cancel a submitted job use the command:
scancel jobIDnumber
Here jobIDnumber should be replaced with the appropriate job ID number determined by using the squeue command.
Slurm Script Commands
Below is a table summarizing some commands that can be used inside Slurm job scripts. The first four commands (interpreter specification, and resource request) are required, while the other commands are optional. Each of the below Slurm commands is meant to go on a single line within a Slurm script.
 
Slurm command
Effect
#!/bin/bash -l
Specifies how the Slurm file should be read (by the bash interpreter). A statement like this is required to be the first line of a Slurm script.
#SBATCH --time=8:00:00
Specifies the maximum limit for how long the job will be allowed to run. (8 hours)
#SBATCH --ntasks=8
Specifies the number of processors (cores) that will be reserved for this job.  (8)
#SBATCH --mem=10g
Specifies the maximum limit for memory usage. This job will die if the application tries to use more than 10GB of memory.
#SBATCH --tmp=10g
Specifies 10 GB of temporary disk will be available for this job in /tmp.
#SBATCH --mail-type=ALL  
Specifies which events will trigger an email message. Other options here include NONE, BEGIN, END, and FAIL. 

#SBATCH --mail-user=me@umn.edu
Specifies the email address that should be used when the Slurm system sends message emails.
#SBATCH -p small,mygroup
Specifies the partition to be the “small” partition, or a partition named “mygroup”. The job will start at the earliest time one of these partitions can accommodate the job.
#SBATCH --gres=gpu:v100:2
#SBATCH -p v100
Request two v100 GPUs for a job submitted to the V100 queue.

https://www.msi.umn.edu/partitions#slurm
Slurm Partitions
Under MSI's new scheduler, Slurm, queues are known as partitions. The job partitions on our systems manage different sets of hardware, and have different limits for quantities such as walltime, available processors, and available memory. When submitting a calculation it is important to choose a partition where the job is suited to the hardware and resource limitations.
Selecting a Partition
Each MSI system contains job partitions managing sets of hardware with different resource and policy limitations. MSI currently has two primary systems: the supercomputer Mesabi and the Mesabi expansion Mangi. Mesabi has high-performance hardware and a wide variety of partitions suitable for many different job types. Mangi expands Mesabi and should be your first choice for submitting jobs. Which system to choose depends highly on which system has partitions appropriate for your software/script. More information about selecting a partitions and the different partition parameters can be found on the Choosing A Partition (Slurm) page.
Below is a summary of the available partitions organized by system, and the associated limitations. The quantities listed are totals or upper limits.
Partition name
Node sharing?
Cores per node
Walltime limit
Total node memory
Advised memory per core
Local scratch per node
Maximum Nodes per Job
amdsmall (1)
Yes
128
96:00:00
248 GB
1900 MB
415 GB
1
amdlarge
No
128
24:00:00
248 GB
1900 MB
415 GB
32
amd512
Yes
128
96:00:00
499 GB
4000 MB
415 GB
1
amd2tb
Yes
128
96:00:00
1995 GB
15 GB
415 GB
1
v100 (1)
Yes
24
24:00:00
374 GB
15 GB
859 GB
1
small
Yes
24
96:00:00
60 GB
2500 MB
429 GB
10
large
No
24
24:00:00
60 GB
2500 MB
429 GB
48
max
Yes
24
696:00:00
60 GB
2500 MB
429 GB
1
ram256g
Yes
24
96:00:00
248 GB
10 GB
429 GB
2
ram1t (2)
Yes
32
96:00:00
1002 GB
31 GB
380 GB
2
k40 (1)
Yes
24
24:00:00
123 GB
5 GB
429 GB
40
interactive (3)
Yes
24
24:00:00
60 GB
2 GB
228 GB
2
interactive-gpu (3)
Yes
24
24:00:00
60 GB
2 GB
228 GB
2
preempt (4)
Yes
24
24:00:00
60 GB
2 GB
228 GB
2
preempt-gpu (4)
Yes
24
24:00:00
60 GB
2 GB
228 GB
2

(1) Note: In addition to selecting a GPU partition, GPUs need to be requested for all GPU jobs.  A  k40 GPU can be requested by including the following two lines in your submission script: 
#SBATCH -p k40                                             
#SBATCH --gres=gpu:k40:1
A  V100 GPU can be requested by including the following two lines in your submission script: 
#SBATCH -p v100                                            
#SBATCH --gres=gpu:v100:1
(2) Note: The ram1t nodes contain Intel Ivy Bridge processors, which do not support all of the optimized instructions of the Haswell processors. Programs compiled using the Haswell instructions will only run on the Haswell processors.
(3) Note: Users are limited to 2 jobs in the interactive and interactive-gpu partitions. 
(4) Note: Jobs in the preempt and preempt-gpu partitions may be killed at any time to make room for jobs in the interactive or interactive-gpu partitions.

Submitting Job Scripts
Once a job script is made it is submitted using the sbatch command:
sbatch -p partitionname scriptname   
Here partitionname is the name of the partition (queue) being submitted to, and scriptname is the name of the job script.  The -p partitionname portion of the command may be omitted, in which case the job would be submitted to whichever partition is set as the default.  Alternatively, the partition specification can be placed inside the job script (see
sbatch -p partitionname scriptname   
cd /home/isrania/onyea005
sbatch -p amdsmall HUMANN_MISSION_MSI_SLURM.rtf   
sbatch -p amdsmall HUMANN_MISSION_MSI_SLURM.txt   
**I wasn’t able to figure out how I could save a txt file into a shell script to submit to MSI, so I will use the online script comverter I found

https://support.ceci-hpc.be/doc/_contents/QuickStart/SubmittingJobs/SlurmTutorial.html
http://www.ceci-hpc.be/scriptgen.html

https://ubccr.freshdesk.com/support/solutions/articles/5000688140-submitting-a-slurm-job-script

Create a SLURM script using an editor such as vi or emacs using steps 1 through 3.  The script (or file) can be called anything you want but should end in .sh (i.e. myscript.sh).  If you are unfamiliar with the UNIX commands to edit files, please read this article

http://www.compciv.org/recipes/cli/basic-shell-scripts/

http://www.compciv.org/bash-guide/
https://slurm.schedmd.com/quickstart.html

The gameplan is to save the file as a txt file from word on my mac, move over to windows to forcefully modify the txt extension to SH 

https://support.apple.com/guide/terminal/use-command-line-text-editors-apdb02f1133-25af-4c65-8976-159609f99817/mac

nvm 
https://www.msi.umn.edu/content/job-submission-and-scheduling-slurm
Job Submission and Scheduling (Slurm)
MSI systems use job queues to efficiently and fairly manage when computations are executed. When computational jobs are submitted to a job queue they wait in the queue until the appropriate computational resources are available.
 
The queuing system which MSI uses is called Slurm. To submit a job to a Slurm queue, users create Slurm job scripts. Slurm job scripts contain information on the resources requested for the calculation, as well as the commands for executing the calculation.
Slurm Script Format
A Slurm job script is a small text file containing information about what resources a job requires, including time, number of nodes, and memory.   The Slurm script also contains the commands needed to begin executing the desired computation. A sample Slurm job script is shown below.
#!/bin/bash -l        
#SBATCH --time=8:00:00
#SBATCH --ntasks=8
#SBATCH --mem=10g
#SBATCH --tmp=10g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=sample_email@umn.edu 
cd ~/program_directory
module load intel 
module load ompi/intel 
mpirun -np 8 program_name < inputfile > outputfile
The first line in the Slurm script defines which type of shell the script will be read with (how the system will read the file).  It is recommended to make the first line #!/bin/bash -l
Commands for the Slurm queuing system begin with #SBATCH.  Lines 2–4 in the above sample script contain the Slurm resource request.   The sample job will require 8 hours, 8 processor cores,  and 10 gigabytes of memory.  The resource request must contain appropriate values; if the requested time, processors, or memory are not suitable for the hardware the job will not be able to run. 
The two lines containing #SBATCH --mail-type, and #SBATCH --mail-user=sample_email@umn.edu , are both commands having to do with sending message emails to the user. The first of these lines instruct the Slurm system to send a message email when the job aborts, begins, or ends.  Other options here include NONE, BEGIN, END, and FAIL. The second command specifies the email address to be used. Using the message emails is recommended because the reason for a job failure can often be determined using information in the emails.
The rest of the sample Slurm script contains the commands which will be executed to begin the calculation. A Slurm script should contain the appropriate change directory commands to get to the job execution location. A Slurm script also needs to contain module load commands for any software modules that the calculation might need. The last lines of a Slurm script contain commands used to execute the calculation. In the above example the final line contains an execution command to start a program which uses MPI communication to run on 8 processor cores.
http://www.compciv.org/recipes/cli/basic-shell-scripts/

Let's use the nano text editor to create a shell script named hello.sh. Follow these steps:
	•	Run nano hello.sh
	•	nano should open up and present an empty file for you to work in. Type in the shell command the commands you want to run via SLURM:

*Trying the human demo coding before trying the metaphlan options suggested in “https://forum.biobakery.org/t/metaphlan4-and-the-new-version-humann/3985/5”
#!/bin/bash -l        
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL  
#SBATCH --mail-user=onyea005@umn.edu 
cd /home/isrania/onyea005/HUMANN082023/demo
module load conda/python3 
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
pwd
metaphlan --help	
humann --config	
humann -i demo.fastq.gz -o test_results 
#humann -i demo.fastq.gz -o test_results --metaphlan-options "-t  --mpa3 --index mpa_vJan21_CHOCOPhlAnSGB_202103 --bowtie2db /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Databases3"
	•	Then press Ctrl-X on your keyboard to Exit nano (type in pwd to determine the location of the sh file)
	•	nano will ask you if you want to save the modified file. Hit the y key (for "yes"). > Save the file as MISSION_TEST.sh (or whatever you chose o name)
	•	nano will then confirm if you want to save to the file name. Hit Enter to confirm this.
	•	Here partitionname is the name of the partition (queue) being submitted to, and scriptname is the name of the job script.  The -p partitionname portion of the command may be omitted, in which case the job would be submitted to whichever partition is set as the default.  Alternatively, the partition specification can be placed inside the job script
Going back to the HUMANN tutorial, here is where we left offAlternatively, if you're comfortable with shell scripting, you can use the following command to process all six at once:


ssh yourusername@login.msi.umn.edu (use ssh yourusername@mangi.msi.umn.edu or ssh yourusername@mesabi.msi.umn.edu respectively)
ssh onyea005@mesabi.msi.umn.edu
	•	Check the folder you are in
pwd
* my home folder is when I first login is “/home/prizm001/onyea005”
* /home/isrania/onyea005
*Change the folder to the israni MSI group
cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
cd /panfs/roc/groups/5/isrania/onyea005
cd /panfs/roc/groups/5/isrania/onyea005/Israni4_Project_016

*Now this can be submitted (home and the /panfs/jay/groups/4/ strings seem to be interchangeable
sbatch -p partitionname scriptname   
cd /home/isrania/onyea005
sbatch -p amdsmall HUMANN_MISSION_MSI_SLURM.rtf   
sbatch -p amdsmall MISSION_TEST.sh
sbatch -p amdsmall MISSION_FULL.sh   
sbatch -p amdsmall humann_demo.sh
sbatch -p amdsmall humann_demo_2.sh   
sbatch -p amdsmall metaphlan_humann_install.sh   
sbatch -p amdsmall metaphlan_humann_install_v2.sh   
sbatch -p amdsmall metaphlan_bowtie2db_chocophlan.sh   
sbatch -p amdsmall metaphlan_bowtie2db_chocophlanv2.sh   
sbatch -p amdsmall humann082023_demo.sh
sbatch -p amdsmall humann_demo_metaphlan_options.sh
sbatch -p amdsmall metaphlan_only_install.sh
sbatch -p amdsmall mpa082023_humann_demo_test.sh
sbatch -p amdsmall mpa082023_humann_demo_test_8CPU.sh
sbatch -p amdsmall mpa082023_humann_demo_test_v3.sh
sbatch -p amdsmall mpa082023_humann_demo_test_v4.sh
 To view all of the jobs submitted by a particular user use the command:
squeue -u username
squeue -u onyea005
This command will display the status of the specified jobs, and the associated job ID numbers. The command squeue by itself will show all jobs on the system.

 To cancel a submitted job use the command:
scancel jobIDnumber
Here jobIDnumber should be replaced with the appropriate job ID number determined by using the squeue command.
https://ubccr.freshdesk.com/support/solutions/articles/5000686861-how-do-i-check-the-status-of-my-job-s-
squeue - Job States
R - Job is running on compute nodes
PD - Job is waiting on compute nodes
CG - Job is completing
squeue - Job Reasons
(Resources) - Job is waiting for compute nodes to become available
(Priority) - Jobs with higher priority are waiting for compute nodes.  Check this knowledge base article for info about job priority
(ReqNodeNotAvail) - The compute nodes requested by the job are not available for a variety of reasons, including:
cluster downtime
nodes offline
temporary scheduling backlog

OMG I had one humann batch work!! Checking to see what was in the SH file so I can replicate it. Now adding a test run for the shotgun data (DEC2023)
Step 1: Check the installation instructions 

https://forum.biobakery.org/t/announcing-humann-3-5/3995

Step 2: Install Metaphlan 4.0

https://forum.biobakery.org/t/metaphlan4-and-the-new-version-humann/3985/4

https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4

Step 3: Install a new environment of Humann 3.0 (Which upgrades it to 3.5, 3.6 or 3.7)

https://forum.biobakery.org/t/announcing-humann-3-6-critical-update/4155

https://huttenhower.sph.harvard.edu/humann

Step 4: Test HUMANN

https://github.com/biobakery/biobakery/wiki/humann3









Step 1: Check the installation instructions 

https://forum.biobakery.org/t/announcing-humann-3-5/3995

Step 2: Install Metaphlan 4.0

https://forum.biobakery.org/t/metaphlan4-and-the-new-version-humann/3985/4

https://github.com/biobakery/MetaPhlAn/wiki/MetaPhlAn-4

MetaPhlAn 4

Home

MetaPhlAn documentation
•	MetaPhlAn 4
•	MetaPhlAn 3.1
•	MetaPhlAn 3.0
•	MetaPhlAn2

StrainPhlAn documentation
•	StrainPhlAn 4
•	StrainPhlAn 3
•	StrainPhlAn
•	Strain Sharing Inference

Workshop on Genomics 2023
•	MetaPhlAn workshop
•	HUMAnN workshop


MetaPhlAn is a computational tool for profiling the composition of microbial communities (Bacteria, Archaea and Eukaryotes) from metagenomic shotgun sequencing data (i.e. not 16S) with species-level. With StrainPhlAn, it is possible to perform accurate strain-level microbial profiling.

MetaPhlAn 4 relies on ~5.1M unique clade-specific marker genes (the latest marker information file can be found here) identified from ~1M microbial genomes (~236,600 references and 771,500 metagenomic assembled genomes) spanning 26,970 species-level genome bins (SGBs, http://segatalab.cibio.unitn.it/data/Pasolli_et_al.html), 4,992 of them taxonomically unidentified at the species level (the full list of the species included in the latest database can be found here), allowing:

•	unambiguous taxonomic assignments;
•	an accurate estimation of organismal relative abundance;
•	SGB-level resolution for bacteria, archaea and eukaryotes;
•	strain identification and tracking
•	orders of magnitude speedups compared to existing methods.
•	metagenomic strain-level population genomics

ssh yourusername@login.msi.umn.edu (use ssh yourusername@mangi.msi.umn.edu or ssh yourusername@mesabi.msi.umn.edu respectively)
ssh onyea005@mesabi.msi.umn.edu

*DEC2023: Since MSI is transitioning away from mesabi, will need to update my logins for Filezilla and ssh to mangi (https://www.msi.umn.edu/content/connecting-hpc-resources)
ssh yourMSIusername@resourcename.msi.umn.edu
ssh onyea005@mangi.msi.umn.edu

	•	Check the folder you are in
pwd
* my home folder is when I first login is “/home/prizm001/onyea005”
* /home/isrania/onyea005
*Change the folder to the israni MSI group
cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
cd /panfs/roc/groups/5/isrania/onyea005
cd /panfs/roc/groups/5/isrania/onyea005/Israni4_Project_016
**created a new folder called sftp://onyea005@mangi.msi.umn.edu/panfs/jay/groups/4/isrania/onyea005/dec2023shotguntest. The full dataset is located at /panfs/jay/groups/4/isrania/shared/UMGC_Data_Backup/2023-q4/Israni4_Project_023

pwd
ls
nano mpa082023_humann_demo_test.sh
**Here is the code that worked
#!/bin/bash -l
#SBATCH --time=48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=onyea005@umn.edu
cd /home/isrania/onyea005/HUMANN082023/demo
module load conda/python3
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
pwd
metaphlan --help
humann --config
humann -i demo.fastq.gz -o test_results
*Now that I have a code that worked, I could test the samples from Dr. Staley. (See 03_NextGen_Part 10_11_Metatranscriptomics_HUMANN_remake_MSI)
** This is very time consuming, so I will try to submit a script to run the test code:

Alternatively, if you're comfortable with shell scripting, you can use the following command to process all six at once:

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN/demo_comb
for f in *.fasta; do humann -i $f -o hmp_subset; done

cd /home/isrania/onyea005/Israni4_Project_016
cd /home/isrania/onyea005/Israni4_016_test
for f in *.fastq.gz; do humann -i $f -o mission_subset; done
for f in *.fastq.gz; do humann -i $f -o mission_subset --threads 4 --metaphlan-options "--read_min_len 49"; done
Based on the code above, here is my new slurm code
Let's use the nano text editor to create a shell script named hello.sh. Follow these steps:
	•	Run nano hello.sh
	•	nano should open up and present an empty file for you to work in. Type in the shell command the commands you want to run via SLURM:
#!/bin/bash -l
#SBATCH --time=96:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=onyea005@umn.edu
cd /home/isrania/onyea005/HUMANN082023/Staley_Project_069
module load conda/python3
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
pwd
metaphlan --help
humann --config
for f in *.fastq.gz; do humann -i $f -o staley_subset --threads 8; done
humann -i demo.fastq.gz -o test_results
*Now I can submit the updated code to MSI (Hopefully the duration for the job is long enough. It took 6 hours for 1 file so I only included 10 files for about 60 hours run time)

*DEC2023 version
#!/bin/bash -l
#SBATCH --time=96:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=8
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=onyea005@umn.edu
cd /home/isrania/onyea005/dec2023shotguntest
module load conda/python3
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
pwd
metaphlan --help
humann --config
for f in *.fastq.gz; do humann -i $f -o dec203shotgun_subset --threads 8; done
After reviewing the following resources
https://www.msi.umn.edu/partitions
https://www.msi.umn.edu/content/choosing-job-partition#slurm
https://www.msi.umn.edu/content/job-submission-and-scheduling-slurm
I will try resubmitting the job to a larger partition to see if I can get the data processed faster (If this doesn’t work, my backup plan is to process batches of 50 to 100 sequences and then move them all to a single folder for the subsequent steps):
*DEC2023 version for amd2TB
#!/bin/bash -l
#SBATCH --time=96:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=64
#SBATCH --cpus-per-task=64
#SBATCH --mem=256g
#SBATCH --tmp=256g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=onyea005@umn.edu
cd /home/isrania/onyea005/dec2023shotguntest
module load conda/python3
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
pwd
metaphlan --help
humann --config
for f in *.fastq.gz; do humann -i $f -o dec203shotgun_subset2 --threads 64; done



*DEC2023 version for amd512
#!/bin/bash -l
#SBATCH --time=96:00:00
#SBATCH --ntasks=32
#SBATCH --mem=32g
#SBATCH --tmp=32g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=onyea005@umn.edu
cd /home/isrania/onyea005/dec2023shotguntest
module load conda/python3
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
pwd
metaphlan --help
humann --config
for f in *.fastq.gz; do humann -i $f -o dec203shotgun_subset3 --threads 32; done
*DEC2023 version for amd512 (trying to up the cpus to 64)
#!/bin/bash -l
#SBATCH --time=96:00:00
#SBATCH --ntasks=64
#SBATCH --mem=64g
#SBATCH --tmp=64g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=onyea005@umn.edu
cd /home/isrania/onyea005/dec2023shotguntest
module load conda/python3
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
pwd
metaphlan --help
humann --config
for f in *.fastq.gz; do humann -i $f -o dec203shotgun_subset4 --threads 64; done
I will make subfolders containing 30 to 40 files and submit these batches to AMD512. The data is located as */panfs/jay/groups/4/isrania/shared/UMGC_Data_Backup/2023-q4/Israni4_Project_023)
*BATCH 1 version for amd512 (trying to up the cpus to 64)
#!/bin/bash -l
#SBATCH --time=96:00:00
#SBATCH --ntasks=64
#SBATCH --mem=64g
#SBATCH --tmp=64g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=onyea005@umn.edu
cd /panfs/jay/groups/4/isrania/shared/UMGC_Data_Backup/2023-q4/Israni4_Project_023/Batch1
module load conda/python3
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
pwd
metaphlan --help
humann --config
for f in *.fastq.gz; do humann -i $f -o batch1_subset --threads 64; done
*BATCH 2 version for amd512 (trying to up the cpus to 64)
#!/bin/bash -l
#SBATCH --time=96:00:00
#SBATCH --ntasks=64
#SBATCH --mem=64g
#SBATCH --tmp=64g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=onyea005@umn.edu
cd /panfs/jay/groups/4/isrania/shared/UMGC_Data_Backup/2023-q4/Israni4_Project_023/Batch2
module load conda/python3
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
pwd
metaphlan --help
humann --config
for f in *.fastq.gz; do humann -i $f -o batch2_subset --threads 64; done

Jobs 3 through 11 failed because I requested too much memory on the isrania partition on MSI. I will learn how to move as much of the data I do not need to second tier storage before rerunning these jobs requesting less memory.
https://www.msi.umn.edu/group/isrania/storage
https://www.msi.umn.edu/support/faq/how-can-i-check-my-storage-quota
https://www.msi.umn.edu/content/second-tier-storage
https://www.msi.umn.edu/support/faq/what-are-some-user-friendly-ways-use-second-tier-storage-s3
*** I will use cyberduck since I can use that on both windows and MAC. First I need to fetch my S3 credentials by being logged into MSI
https://www.msi.umn.edu/content/s3-credentials
This page allows you to view or reset your S3 credentials for MSI Second Tier Storage.
Bucket access lists are associated with your user account. Resetting your credentials is like changing a password. You will need to update your S3 clients to use the new credentials to access your buckets.
If you are currently logged into the website, you should see your current S3 access and secret keys below:
Your S3 access key is: Z51SYD3F8VFDJ5CLHATZ
Your S3 secret key is: EarKqpiDhJNSD0QwOkZtHMzabcyk3gr1yrliNjJw
In cyberduck, go to file, new browser, and then enter the info above for s3.msi.umn.edu as the 
 
https://www.msi.umn.edu/support/faq/how-can-my-programs-interface-second-tier-storage

Nevermind, the cyberduck bucket step is annoying so I will just have to create a new bucket manually.
https://www.msi.umn.edu/support/faq/how-do-i-use-second-tier-storage-command-line
Check my quota usage based on ncdu with https://www.msi.umn.edu/support/faq/how-can-i-check-my-storage-quota
Based on my ncdu usage, it looks like humann uses a chunk of the data (~500gb) so I should check were my db folders are located
module load conda/python3
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
humann_config
**After meeting with the MSI helpdesk, I was advised to use ls -l to list my files and determine which files are taking up space. In addition, I can use the following options to track the files more accurately
https://alvinalexander.com/photos/linux-ls-command-how-sort-by-filesize-reversed/#:~:text=The%20%2DS%20option%20is%20the,the%20end%20of%20the%20output.
ls -Slhr
The -S option is the key, telling the ls command to sort the file listing by size. The -h option tells ls to make the output human readable, and -r tells it to reverse the output, so in this case the largest files are shown at the end of the output.
**First, I need to make sure that all my files were backed up on the second tier storage
Now that I know which folders are taking up space within the HUMANN folder, I know which ones I will delete after syncing all the files to second tier storage
https://www.msi.umn.edu/support/faq/how-do-i-use-second-tier-storage-command-line
https://www.msi.umn.edu/support/faq/how-can-my-programs-interface-second-tier-storage
In order to check what your second tier storage folder names are, use 
s3cmd ls
2023-12-31 13:31  s3://onyea005data
2021-09-20 20:46  s3://prizmentlabgroup_umgc
s3cmd ls s3://onyea005data/onyea005/ -l
In order to sync your data from the israniagroup, run: 

s3cmd sync ~/mydirectory s3://mynewbucket
s3cmd sync /home/isrania/onyea005 s3://onyea005data

**The step above is time consuming, so I will submit a batch job to sync my data to second tier storage

#!/bin/bash -l
#SBATCH --time=24:00:00
#SBATCH --ntasks=8
#SBATCH --tmp=64g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=onyea005@umn.edu 
s3cmd sync /home/isrania/onyea005 s3://onyea005data

#!/bin/bash -l
#SBATCH --time=24:00:00
#SBATCH --ntasks=8
#SBATCH --mail-type=ALL
#SBATCH --mail-user=onyea005@umn.edu 
s3cmd sync /home/isrania/onyea005 s3://onyea005data

*** Backing up all the data_release data to second tier storage on S3.
*First, making a folder for data release

s3cmd mb s3://israniadatarelease

#!/bin/bash -l
#SBATCH --time=24:00:00
#SBATCH --ntasks=8
#SBATCH --tmp=64g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=onyea005@umn.edu 

s3cmd sync /common/data_release/isrania s3://israniadatarelease

*Now running the job

sbatch -p amdsmall s3syncisraniadatarelease.sh

**Now I can check the files which are taking up space in my user folder
https://www.msi.umn.edu/support/faq/how-can-i-check-my-storage-quota

You can see your individual utilization in addition to the group's usage with:
groupquota -u
Usage breakdown
The ncdu utility is useful to see how the space in your home directory is being used. To use ncdu in your home directory, issue the following commands:

cd
module load ncdu
ncdu

* * I will need to check the size using an alternative option

https://alvinalexander.com/photos/linux-ls-command-how-sort-by-filesize-reversed/#:~:text=The%20%2DS%20option%20is%20the,the%20end%20of%20the%20output.
ls -Slhr
ls -Slh
The -S option is the key, telling the ls command to sort the file listing by size. The -h option tells ls to make the output human readable, and -r tells it to reverse the output, so in this case the largest files are shown at the end of the output.
**Since the data in my folder has been backed up to S3 (s3cmd ls s3://onyea005data/onyea005/ -l), I can start deleting some older files

*Checking why the dec2023shotguntest folder is so large (Because each of the subfolders created by HUMANN is 6G per sequence file)

cd /home/isrania/onyea005/dec2023shotguntest
module load ncdu
ncdu

*Checking why the Human2023 folder is so large (Will have to remove the staley test files)

cd /home/isrania/onyea005/HUMANN082023
module load ncdu
ncdu

**I will remove the Staley dataset. In addition, based on the databases used by humann (see below) I will removes the Databases2 folder as well

module load conda/python3
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
humann_config

I will also remove the conda environments that are not being used as well

https://docs.kanaries.net/topics/Python/conda-remove-environment
What is the Command Used to Remove a Conda Environment?
The command used to remove a Conda environment is conda env remove --name env_name, where env_name is the name of the environment you want to remove. This command will delete the specified environment, along with all its associated packages and dependencies.
Where are Conda Environments Stored?
Conda environments are stored in the envs directory within your Conda installation. The exact path will depend on your operating system and Conda configuration. You can view the location of all your Conda environments using the conda env list command.
2: Removing a Conda Environment
Now that we understand what a Conda environment is and where it's stored, let's discuss how to remove it. As mentioned earlier, the command to remove a Conda environment is conda env remove --name env_name. However, before you run this command, it's a good idea to deactivate the environment if it's currently active.

conda env list 
conda env remove --name env_name.
conda env remove --name blasttest	
conda env remove --name mpa
conda env remove --name mpa082023V2

Now that I have removed the folders from old HUMANN, QIIME and Metatranscriptomics, I can check how much memory I am using 

cd
module load ncdu
ncdu

cd /panfs/jay/groups/4/isrania/shared/UMGC_Data_Backup/2023-q4/Israni4_Project_023
module load ncdu
ncdu

**Next, I need to figure out how to use scratch storage to run the jobs.


https://www.msi.umn.edu/content/scratch-storage


*BATCH 3 version for amd512 (trying to up the cpus to 64)
#!/bin/bash -l
#SBATCH --time=96:00:00
#SBATCH --ntasks=64
#SBATCH --mem=64g
#SBATCH --tmp=64g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=onyea005@umn.edu
cd /panfs/jay/groups/4/isrania/shared/UMGC_Data_Backup/2023-q4/Israni4_Project_023/Batch3
module load conda/python3
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
pwd
metaphlan --help
humann --config
for f in *.fastq.gz; do humann -i $f -o batch3_subset --threads 64; done
*BATCH 4 version for amd512 (trying to up the cpus to 64)
#!/bin/bash -l
#SBATCH --time=96:00:00
#SBATCH --ntasks=64
#SBATCH --mem=64g
#SBATCH --tmp=64g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=onyea005@umn.edu
cd /panfs/jay/groups/4/isrania/shared/UMGC_Data_Backup/2023-q4/Israni4_Project_023/Batch4
module load conda/python3
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
pwd
metaphlan --help
humann --config
for f in *.fastq.gz; do humann -i $f -o batch4_subset --threads 64; done
*BATCH 5 version for amd512 (trying to up the cpus to 64)
#!/bin/bash -l
#SBATCH --time=96:00:00
#SBATCH --ntasks=64
#SBATCH --mem=64g
#SBATCH --tmp=64g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=onyea005@umn.edu
cd /panfs/jay/groups/4/isrania/shared/UMGC_Data_Backup/2023-q4/Israni4_Project_023/Batch5
module load conda/python3
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
pwd
metaphlan --help
humann --config
for f in *.fastq.gz; do humann -i $f -o batch5_subset --threads 64; done
*BATCH 6 version for amd512 (trying to up the cpus to 64)
#!/bin/bash -l
#SBATCH --time=96:00:00
#SBATCH --ntasks=64
#SBATCH --mem=64g
#SBATCH --tmp=64g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=onyea005@umn.edu
cd /panfs/jay/groups/4/isrania/shared/UMGC_Data_Backup/2023-q4/Israni4_Project_023/Batch6
module load conda/python3
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
pwd
metaphlan --help
humann --config
for f in *.fastq.gz; do humann -i $f -o batch6_subset --threads 64; done
*BATCH 7 version for amd512 (trying to up the cpus to 64)
#!/bin/bash -l
#SBATCH --time=96:00:00
#SBATCH --ntasks=64
#SBATCH --mem=64g
#SBATCH --tmp=64g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=onyea005@umn.edu
cd /panfs/jay/groups/4/isrania/shared/UMGC_Data_Backup/2023-q4/Israni4_Project_023/Batch7
module load conda/python3
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
pwd
metaphlan --help
humann --config
for f in *.fastq.gz; do humann -i $f -o batch7_subset --threads 64; done
*BATCH 8 version for amd512 (trying to up the cpus to 64)
#!/bin/bash -l
#SBATCH --time=96:00:00
#SBATCH --ntasks=64
#SBATCH --mem=64g
#SBATCH --tmp=64g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=onyea005@umn.edu
cd /panfs/jay/groups/4/isrania/shared/UMGC_Data_Backup/2023-q4/Israni4_Project_023/Batch8
module load conda/python3
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
pwd
metaphlan --help
humann --config
for f in *.fastq.gz; do humann -i $f -o batch8_subset --threads 64; done
*BATCH 9 version for amd512 (trying to up the cpus to 64)
#!/bin/bash -l
#SBATCH --time=96:00:00
#SBATCH --ntasks=64
#SBATCH --mem=64g
#SBATCH --tmp=64g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=onyea005@umn.edu
cd /panfs/jay/groups/4/isrania/shared/UMGC_Data_Backup/2023-q4/Israni4_Project_023/Batch9
module load conda/python3
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
pwd
metaphlan --help
humann --config
for f in *.fastq.gz; do humann -i $f -o batch9_subset --threads 64; done
*BATCH 10 version for amd512 (trying to up the cpus to 64)
#!/bin/bash -l
#SBATCH --time=96:00:00
#SBATCH --ntasks=64
#SBATCH --mem=64g
#SBATCH --tmp=64g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=onyea005@umn.edu
cd /panfs/jay/groups/4/isrania/shared/UMGC_Data_Backup/2023-q4/Israni4_Project_023/Batch10
module load conda/python3
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
pwd
metaphlan --help
humann --config
for f in *.fastq.gz; do humann -i $f -o batch10_subset --threads 64; done
*BATCH 11 version for amd512 (trying to up the cpus to 64)
#!/bin/bash -l
#SBATCH --time=96:00:00
#SBATCH --ntasks=64
#SBATCH --mem=64g
#SBATCH --tmp=64g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=onyea005@umn.edu
cd /panfs/jay/groups/4/isrania/shared/UMGC_Data_Backup/2023-q4/Israni4_Project_023/Batch11
module load conda/python3
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
pwd
metaphlan --help
humann --config
for f in *.fastq.gz; do humann -i $f -o batch11_subset --threads 64; done




**Instead of doing large batches via amd512, trying to run an array based on trevor’s feedback below :

See also https://www.youtube.com/watch?v=FrTGAMC7w1I&ab_channel=MinnesotaSupercomputingInstitute%7CUMN
https://www.warp.dev/terminus/chmod-x#:~:text=On%20Linux%20and%20Unix%2Dlike,be%20run%20as%20a%20program.&text=Note%20that%20the%20%2B%20sign%20here%20translates%20to%20%E2%80%9Cadd%20permission%E2%80%9D.
https://www.computerhope.com/unix/uchmod.htm
I don't have a good gauge for what humann uses in terms of memory/time. I'm running 171 samples currently. I think most of the time is used by bowtie2 which is just a slow piece of software. 
most of the ram is used by loading databases so that is a little dependent on what one you are loading. 

something to consider is running it as a job array. 
https://www.msi.umn.edu/support/faq/how-do-i-use-job-array
they run each sample separately so I create a file with a list of commands: 

for i in *removedcombined.fastq.gz; do echo "humann --input $i --search-mode uniref50 --output Humann3Output/${i//.fastq/}Humann_uniref50 --threads 8" >> humann3er.cmd; done
chmod +x humann3er.cmd

then a shell script:
########################################
#!/bin/bash -l
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --mem=60g
#SBATCH -t 12:00:00
#

module load conda
module load bowtie2

source /common/software/install/migrated/anaconda/python3-2020.07-mamba/etc/profile.d/conda.sh
conda activate humann3.6

cd /scratch.global/goul0109/zinc/combined_paired/

value=$(sed -n "${SLURM_ARRAY_TASK_ID}p" humann3er.cmd)
${value}
########################################
then you submit is like this:

sbatch --array=1-171 humann.sh

so 1-171 is saying that's how many commands you are going to run. 
then that number 1,2,170,171 etc. is used by the shell script in the value line to pull the line from the file with one command per line and run it. 
the SBATCH commands are what you need for each single command to run 
and each command takes the environment from the shell script (module load conda etc.) 

you can test it with:
sbatch --array=1-2 humann.sh
to see if a couple work before starting 500 jobs. 

***Starting to prep the command file needed for the array. From what I understand, this first step will automatically create a cmd file: they run each sample separately so I create a file with a list of commands: 

for i in *removedcombined.fastq.gz; do echo "humann --input $i --search-mode uniref50 --output Humann3Output/${i//.fastq/}Humann_uniref50 --threads 8" >> humann3er.cmd; done
chmod +x humann3er.cmd
***I will stick to uniref90 based on this discussion (https://groups.google.com/g/humann-users/c/YHv8gIzhiqg?pli=1)
for f in *.fastq.gz; do echo "humann -i $f -o /panfs/jay/groups/4/isrania/onyea005/DEC2023array/$f_subset --threads 8" >> humann3arraytest.cmd; done
*The version above didn’t work so I will try this version below, based on trevor’s additional feedback (I did not realize that the .cmd file did not overwrite previous version but added more lines of code so I needed to open the file via text edit and make manual changes):
for i in *.fastq.gz; do echo "humann -i $i -o /panfs/jay/groups/4/isrania/onyea005/DEC2023array/subset --threads 8" >> humann3arraytest.cmd; done
chmod +x humann3arraytest.cmd
Now work on the batch job file
#!/bin/bash -l
#SBATCH --time=6:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --mem=64g
#SBATCH --tmp=64g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=onyea005@umn.edu
cd /home/isrania/onyea005/dec2023shotguntest
module load conda/python3
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3
value=$(sed -n "${SLURM_ARRAY_TASK_ID}p" /panfs/jay/groups/4/isrania/onyea005/humann3arraytest.cmd)
${value}
**The code above was not working and I reached out to Trevor for some additional feedback. Maybe I can circumvent the use of a command file and combine the array call with the command call here? (Dumbed down version of the code based on the example at https://www.msi.umn.edu/support/faq/how-do-i-use-job-array)
#!/bin/bash -l
#SBATCH --time=6:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --mem=64g
#SBATCH --tmp=64g
#SBATCH --mail-type=ALL
#SBATCH --mail-user=onyea005@umn.edu

module load conda/python3
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3

cd /home/isrania/onyea005/dec2023shotguntest
humann --input input${SLURM_ARRAY_TASK_ID}.fastq.gz --output subset${SLURM_ARRAY_TASK_ID} --threads 8
	•	Then press Ctrl-X on your keyboard to Exit nano (type in pwd to determine the location of the sh file)
	•	nano will ask you if you want to save the modified file. Hit the y key (for "yes"). > Save the file as MISSION_TEST.sh (or whatever you chose o name)
	•	nano will then confirm if you want to save to the file name. Hit Enter to confirm this.
	•	Here partitionname is the name of the partition (queue) being submitted to, and scriptname is the name of the job script.  The -p partitionname portion of the command may be omitted, in which case the job would be submitted to whichever partition is set as the default.  Alternatively, the partition specification can be placed inside the job script
Going back to the HUMANN tutorial, here is where we left offAlternatively, if you're comfortable with shell scripting, you can use the following command to process all six at once:


ssh yourusername@login.msi.umn.edu (use ssh yourusername@mangi.msi.umn.edu or ssh yourusername@mesabi.msi.umn.edu respectively)
ssh onyea005@mesabi.msi.umn.edu

*DEC2023: Since MSI is transitioning away from mesabi, will need to update my logins for Filezilla and ssh to mangi (https://www.msi.umn.edu/content/connecting-hpc-resources)
ssh yourMSIusername@resourcename.msi.umn.edu
ssh onyea005@mangi.msi.umn.edu

	•	Check the folder you are in
pwd
* my home folder is when I first login is “/home/prizm001/onyea005”
* /home/isrania/onyea005
*Change the folder to the israni MSI group
cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
cd /panfs/roc/groups/5/isrania/onyea005
cd /panfs/roc/groups/5/isrania/onyea005/Israni4_Project_016

*Now this can be submitted (home and the /panfs/jay/groups/4/ strings seem to be interchangeable
sbatch -p partitionname scriptname   
cd /home/isrania/onyea005
sbatch -p amdsmall HUMANN_MISSION_MSI_SLURM.rtf   
sbatch -p amdsmall MISSION_TEST.sh
sbatch -p amdsmall MISSION_FULL.sh   
sbatch -p amdsmall humann_demo.sh
sbatch -p amdsmall humann_demo_2.sh   
sbatch -p amdsmall metaphlan_humann_install.sh   
sbatch -p amdsmall metaphlan_humann_install_v2.sh   
sbatch -p amdsmall metaphlan_bowtie2db_chocophlan.sh   
sbatch -p amdsmall metaphlan_bowtie2db_chocophlanv2.sh   
sbatch -p amdsmall humann082023_demo.sh
sbatch -p amdsmall humann_demo_metaphlan_options.sh
sbatch -p amdsmall metaphlan_only_install.sh
sbatch -p amdsmall mpa082023_humann_demo_test.sh
sbatch -p amdsmall mpa082023_humann_demo_test_8CPU.sh
sbatch -p amdsmall mpa082023_humann_demo_test_v3.sh
sbatch -p amdsmall mpa082023_humann_demo_test_v4.sh
sbatch -p amdsmall humann_staley_test.sh
sbatch -p amdsmall dec2023shotguntest.sh
sbatch -p amd2tb dec2023shotguntestamd2tb.sh
sbatch -p amd512 dec2023shotguntestamd512.sh
sbatch -p amd512 dec2023shotguntestamd512v2.sh
sbatch -p amd512 dec2023batch1.sh
sbatch -p amd512 dec2023batch2.sh

sbatch -p amdsmall s3syncdec2023.sh

sbatch -p amd512 dec2023batch3.sh
sbatch -p amd512 dec2023batch4.sh
sbatch -p amd512 dec2023batch5.sh
sbatch -p amd512 dec2023batch6.sh
sbatch -p amd512 dec2023batch7.sh
sbatch -p amd512 dec2023batch8.sh
sbatch -p amd512 dec2023batch9.sh
sbatch -p amd512 dec2023batch10.sh
sbatch -p amd512 dec2023batch11.sh
sbatch --array=1-6 dec2023arraytest.sh
sbatch --array=1-6 dec2023arrayrenamedtest.sh

 To view all of the jobs submitted by a particular user use the command:
squeue -u username
squeue -u onyea005
This command will display the status of the specified jobs, and the associated job ID numbers. The command squeue by itself will show all jobs on the system.

 To cancel a submitted job use the command:
scancel jobIDnumber
Here jobIDnumber should be replaced with the appropriate job ID number determined by using the squeue command.
https://ubccr.freshdesk.com/support/solutions/articles/5000686861-how-do-i-check-the-status-of-my-job-s-
squeue - Job States
R - Job is running on compute nodes
PD - Job is waiting on compute nodes
CG - Job is completing
squeue - Job Reasons
(Resources) - Job is waiting for compute nodes to become available
(Priority) - Jobs with higher priority are waiting for compute nodes.  Check this knowledge base article for info about job priority
(ReqNodeNotAvail) - The compute nodes requested by the job are not available for a variety of reasons, including:
cluster downtime
nodes offline
temporary scheduling backlog
Now that the files were processed in “/panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Staley_Project_069/staley_subset” I can continue the tutorial. First I will need to log in via srun for more computing power
Log in to an interactive node, making sure you have enough time and memory (https://www.msi.umn.edu/content/interactive-queue-use-isub)

isub -n nodes=1:ppn=8 -m 16GB -w 8:00:00	
#I don’t think I need to log into mesabi or Itasca to run this job if I am logged into the interactive node since it logs me into the lab queue (So use mesabi instead)
MSI's LabQi Cluster will be shut down and decommissioned during February Maintenance (February 5, 2020.) Several months ago, MSI released the Mesabi Interactive Queue as a replacement for the LabQi Cluster (ssh lab.)  The Mesabi Interactive Queue can be used in real-time for tasks such as interactive data exploration, creating plots or images, visualizations, or testing sections of code that will be used on other MSI queues.

https://www.msi.umn.edu/content/interactive-hpc

What is Interactive HPC?

MSI’s interactive HPC resources are composed of some of the same systems used to support our batch scheduled HPC activities. The main difference between the two services is the way in which users interact with the MSI resource. Interactive HPC systems allow real-time user inputs in order to facilitate code development, real-time data exploration, and visualizations. Interactive HPC systems are used when data are too large to download to a desktop or laptop, software is difficult or impossible to install on a personal machine, or specialized hardware resources (e.g., GPUs) are needed to visualize large datasets. User input might come through a command line interface (i.e., system shell) while debugging a piece of software or through mouse clicks while using a program with a graphical interface.

What can I do with Interactive HPC?

Unlike MSI’s other computing options, the Interactive HPC resources are designed only to be used in real time for tasks such as interactive data exploration, creating plots or images, visualizations, or testing sections of code that will be used on other MSI systems.

Common use cases include:
	•	Testing a compiled program at the command line before submitting it as a job
	•	Developing code or exploring data in an interactive environment like MATLAB, IPython, or Rstudio
	•	Visualizing data or building simulations in applications like Schrodinger, COMSOL, or Avizo

What types of Interactive HPC are available?
	•	srun:  Submit an interactive job on the current HPC system to provide a command line or graphical interface to interactively run software. 
	•	NICE: Connects a user to a graphical environment, which may have specialized GPUs for visualizing large datasets.
	•	Total Slots: 32 slots for accelerated remote desktops
	•	Total Memory:  Up to 16 GB/session
	•	Graphical Accelerators: Nvidia GK107GL
	•	CITRIX: Citrix for Windows Environments: Connects a user to a graphical Windows environment, where they can access specialized Windows software for modeling and data analysis.
	•	NX NoMachine: Connects a user to a graphical environment, which may be used as a lightweight remote desktop for connecting to other MSI resources.
	•	Jupyter Notebooks: A web portal for notebook-based computing in the browser, which can be used for reproducible and shareable data analysis, visualization, and scripted control of larger tasks. Currently supports Python 2, Python 3, and R. 
Here are several examples of how you can request an interactive HPC job on the new Mesabi Interactive Queue.

The basic syntax for submitting an interactive job on Mesabi (I assume that in combination with the new login on mesabi and mangi the syntax occurs after login into mesabi.msi.umn.edu):

qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I

The basic syntax for testing MPI code between 2 nodes:
qsub -l nodes=2:ppn=2,mem=8gb,walltime=12:00:00 -q interactive -W x=nmatchpolicy:exactnode -I

The basic syntax for creating a job with X-forwarding for GUI's and visulization:
qsub -X -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I

Back to the interactive login
qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I

The full specifications of the Mesabi Interactive Queue can be found on the MSI Queues webpage. Also, more examples on how to use qsub -I for interactive use can be found on the Interactive queue use with qsub webpage.
https://www.msi.umn.edu/content/interactive-queue-use-srun
** Will need to learn how to use Slurm to schedule jobs (https://www.msi.umn.edu/slurm)
Interactive queue use with srun
It is possible to start an interactive job on any of MSI’s clusters that support job submission. Interactive jobs can be useful for tasks including data exploration, development, or (with X11 forwarding) visualization activities.
The basic syntax for submitting an interactive job is illustrated using the following example for Mesabi:
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=1gb -t 1:00:00 -p interactive --pty bash
If you are submitting an interactive job from a session with X11 forwarding and would like to continue using X11 forwarding inside your job, you would add the additional flag ‘-x11’, as follows:
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=1gb -t 1:00:00 -p interactive --x11 --pty bash
The basic syntax for requesting two nodes, for instance to test MPI code between two nodes:
srun -N 2 --ntasks-per-node=1  --mem-per-cpu=1gb -t 1:00:00 -p interactive --pty bash
The syntax for requesting an interactive gpu node with a k40 GPU is:
srun -n 12 -t 1:00:00 -p interactive-gpu --gres=gpu:k40:1 --pty bash
You may also submit an interactive job using an interactive submission script. For instance, to submit an interactive job to test an MPI code (in our case named ‘interactive.sh’) with contents:
#SBATCH -N 2
#SBATCH --ntasks-per-node=1
#SBATCH --mem-per-cpu=1gb
#SBATCH -t 1:00:00
#SBATCH -p interactive
./my_application
You would then submit this job using:
sbatch interactive.sh
Interactive jobs are subject to the same rules and limitations as batch jobs. Interactive jobs have access to only those partitions that are available on the cluster they are submitted from. Jobs that request more resources may wait in the queue longer, so be sure that you are requesting only those resources that you need.
 An interactive job supports all of the usual Slurm commands. These commands are included as flags when requesting an interactive job. 
 Example interactive srun Session
 username@mydesktop$ ssh -Y msiusername@mesabi.msi.umn.edu
Password:
Last login: Tue May 10 15:16:06 2018 from mydesktop.mydept.umn.edu
-------------------------------------------------------------------------------
            University of Minnesota Supercomputing Institute
                                Mesabi
                        HP Haswell Linux Cluster
-------------------------------------------------------------------------------
[... output truncated ...]
---------------
username@ln0006 [~] % srun -N 2 --ntasks-per-node=1  --mem-per-cpu=1gb --x11 -t 1:00:00 --pty bash
username@cn0001 [~] % 
username@cn0001 [~] % echo $SLURM_JOB_NODELIST
cn[0001-0002]
username@cn0001 [~] %
***Note that this interactive job is using two nodes (-N 2). At this point you are currently in your home directory, and can load modules and launch software, including GUI software.

Back to the interactive login using srun (might take a few mins to get the access)

*Old syntax
qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I
*New syntax
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=16gb -t 4:00:00 -p interactive --pty bash
Now you can look at the contents of the directory using the ls command:

ls -lh
You can always check the location of your current directory (aka folder) using the pwd command.  If you type:

pwd
Login into an interactive node to process the data
*I will try to add a CPU per task option to the interactive run and to the sbatch file
https://www.msi.umn.edu/support/faq/how-can-i-use-gnu-parallel-run-lot-commands-parallel
https://www.msi.umn.edu/content/interactive-queue-use-srun
Interactive queue use with srun
It is possible to start an interactive job on any of MSI’s clusters that support job submission. Interactive jobs can be useful for tasks including data exploration, development, or (with X11 forwarding) visualization activities.
 The basic syntax for submitting an interactive job is illustrated using the following example for Mesabi:
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=1gb -t 1:00:00 -p interactive --pty bash
If you are submitting an interactive job from a session with X11 forwarding and would like to continue using X11 forwarding inside your job, you would add the additional flag ‘-x11’, as follows:
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=1gb -t 1:00:00 -p interactive --x11 --pty bash
The basic syntax for requesting two nodes, for instance to test MPI code between two nodes:
srun -N 2 --ntasks-per-node=1  --mem-per-cpu=1gb -t 1:00:00 -p interactive --pty bash
The syntax for requesting an interactive gpu node with a k40 GPU is:
srun -n 12 -t 1:00:00 -p interactive-gpu --gres=gpu:k40:1 --pty bash
https://stackoverflow.com/questions/51139711/hpc-cluster-select-the-number-of-cpus-and-threads-in-slurm-sbatch
The terminology used in the sbatch man page might be a bit confusing. Thus, I want to be sure I am getting the options set right. Suppose I have a task to run on a single node with N threads. Am I correct to assume that I would use --nodes=1 and --ntasks=N?
I am used to thinking about using, for example, pthreads to create N threads within a single process. Is the result of that what they refer to as "cores" or "cpus per task"? CPUs and threads are not the same things in my mind.

Depending on the parallelism you are using: distributed or shared memory
--ntasks=# : Number of "tasks" (use with distributed parallelism).
--ntasks-per-node=# : Number of "tasks" per node (use with distributed parallelism).
--cpus-per-task=# : Number of CPUs allocated to each task (use with shared memory parallelism).

From this question: if every node has 24 cores, is there any difference between these commands?
sbatch --ntasks 24 [...]
sbatch --ntasks 1 --cpus-per-task 24 [...]
Answer: (by Matthew Mjelde)
Yes there is a difference between those two submissions. You are correct that usually ntasks is for mpi and cpus-per-task is for multithreading, but let’s look at your commands:
For your first example, the sbatch --ntasks 24 […] will allocate a job with 24 tasks. These tasks in this case are only 1 CPUs, but may be split across multiple nodes. So you get a total of 24 CPUs across multiple nodes.
For your second example, the sbatch --ntasks 1 --cpus-per-task 24 [...] will allocate a job with 1 task and 24 CPUs for that task. Thus you will get a total of 24 CPUs on a single node.
In other words, a task cannot be split across multiple nodes. Therefore, using --cpus-per-task will ensure it gets allocated to the same node, while using --ntasks can and may allocate it to multiple nodes.

Another good Q&A from CÉCI's support website: Suppose you need 16 cores. Here are some use cases:
	•	you use mpi and do not care about where those cores are distributed: --ntasks=16
	•	you want to launch 16 independent processes (no communication): --ntasks=16
	•	you want those cores to spread across distinct nodes: --ntasks=16 and --ntasks-per-node=1 or --ntasks=16 and --nodes=16
	•	you want those cores to spread across distinct nodes and no interference from other jobs: --ntasks=16 --nodes=16 --exclusive
	•	you want 16 processes to spread across 8 nodes to have two processes per node: --ntasks=16 --ntasks-per-node=2
	•	you want 16 processes to stay on the same node: --ntasks=16 --ntasks-per-node=16
	•	you want one process that can use 16 cores for multithreading: --ntasks=1 --cpus-per-task=16
	•	you want 4 processes that can use 4 cores each for multithreading: --ntasks=4 --cpus-per-task=4

https://stackoverflow.com/questions/49466020/specify-number-of-cpus-for-a-job-on-slurm
https://researchcomputing.princeton.edu/support/knowledge-base/scaling-analysis
https://docs-research-it.berkeley.edu/services/high-performance-computing/user-guide/running-your-jobs/scheduler-examples/
Back to the interactive login using srun (might take a few mins to get the access)

https://www.msi.umn.edu/content/interactive-queue-use-srun
*Old syntax
qsub -l nodes=1:ppn=4,mem=8gb,walltime=12:00:00 -q interactive -I
*New syntax
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=16gb -t 4:00:00 -p interactive --pty bash

*Testing out adding multiple CPUs to run the code faster (https://www.msi.umn.edu/support/faq)
It seems like nodes and CPU is different so the n-task per node is what determine thee number of CPUs we can use?
srun -N 1 --ntasks-per-node=4  --mem-per-cpu=32gb -t 4:00:00 -p interactive --pty bash
*I will use this code to ask for multiple CPUs for the analysis)https://stackoverflow.com/questions/51139711/hpc-cluster-select-the-number-of-cpus-and-threads-in-slurm-sbatch
--ntasks=4 --cpus-per-task=4

srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=32gb -t 4:00:00 -p interactive --pty bash

*Per the SLURM PDF guide (See slide 10) the interactive queue has a maximum of 4 cores/CPU that can be requested through srun. I will request 8 when submitting a job via SLURM
srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=32gb -t 4:00:00 -p interactive --pty bash

srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=8gb -t 4:00:00 -p interactive --pty bash

*Setting the srun to 16GB since metaphlan 4 mentioned that it needs 15GB to run
srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=16gb -t 12:00:00 -p interactive --pty bash
srun -N 1 --ntasks-per-node=4 --cpus-per-task=4 --mem-per-cpu=32gb -t 12:00:00 -p interactive --pty bash
https://github.com/biobakery/biobakery/wiki/humann3
2.2 Running HUMAnN: the basics
In order to run this analysis and the demo at the same time, I created a new folder “/panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Staley_Project_069/demo” where I moved all the relevant files from the tutorials (after 
If you are running this tutorial as part of a short course, all input files have been pre-downloaded into the cloud instances. To run this tutorial from the correct location, enter:
cd ~/Tutorials/humann3
module load conda/python3 
conda info --envs
conda activate /home/isrania/onyea005/.conda/envs/mpa082023V3

cd ~/Tutorials/humann3
pwd
cd /home/isrania/onyea005/HUMANN082023/Staley_Project_069/demo
cd /home/isrania/onyea005/HUMANN082023/Staley_Project_069/staley_subset
** Now running the same demo code on the subset data from Dr. Staley
See 4.2 HUMAnN for multiple samples (https://github.com/biobakery/biobakery/wiki/humann3)
It is typical to execute HUMAnN on multiple individual samples, then merge the resulting gene families or pathways into a single tab-delimited multi-sample table. When combining or comparing any HUMAnN output, it is critical to normalize the default RPK abundance units (which will be sensitive to the sequencing depth of each sample) into CPM units (which will not be).
Regardless of how you run HUMAnN, this will result in six output directories each containing the three main analysis types (gene families, pathway abundances, and pathway coverages) along with intermediate files. Let's merge the 6 per-sample gene family abundance outputs into a single table for this dataset using the script humann_join_tables:


Join the output files (gene families, coverage, and abundance) from the HUMAnN 3.0 runs from all samples into three files

	•	$ humann_join_tables --input $OUTPUT_DIR --output humann_genefamilies.tsv --file_name genefamilies_relab
	•	$ humann_join_tables --input $OUTPUT_DIR --output humann_pathcoverage.tsv --file_name pathcoverage
	•	$ humann_join_tables --input $OUTPUT_DIR --output humann_pathabundance.tsv --file_name pathabundance_relab

For each command, replace $OUTPUT_DIR with the full path to the folder containing the HUMAnN 3.0 output files.

The resulting files from these commands are named humann_genefamilies.tsv, humann_pathabundance.tsv, and humann_pathcoverage.tsv.
cd /home/isrania/onyea005/HUMANN082023/Staley_Project_069/staley_subset

cd /home/isrania/onyea005/HUMANN082023/Staley_Project_069
ls
humann_join_tables -i mission_subset_full -o mission_subset_full_genefamilies.tsv --file_name genefamilies
humann_join_tables -i mission_subset_full -o mission_subset_full_pathcoverage.tsv --file_name pathcoverage
humann_join_tables -i mission_subset_full -o mission_subset_full_pathabundance.tsv --file_name pathabundance

humann_join_tables -i staley_subset -o staley_subset_genefamilies.tsv --file_name genefamilies
humann_join_tables -i staley_subset -o staley_subset_pathcoverage.tsv --file_name pathcoverage
humann_join_tables -i staley_subset -o staley_subset_pathabundance.tsv --file_name pathabundance
Check if all input files are available in the input directory with ls. We will copy files from this folder to the current working directory over the course of the tutorial. Let us start with the aforementioned 3 demo input files.
cp input/demo* .
No need to run this command since I manually copied the processed demo files via filezilla
A canonical HUMAnN run uses nucleotide mapping and translated search to provide organism-specific gene and pathway abundance profiles from a single metagenome. By default, gene families are annotated using UniRef90 definitions and pathways using MetaCyc definitions. From the examples folder (or the location where you saved demo.fastq.gz above) execute the following:
humann --input demo.fastq.gz --output demo_fastq
No need to rerun this step since I already processed the tutorial files and test staley sequences via HUMANN 
(Note1: If you have more than 1 CPU available for computation, you can use N of them by adding --threads N to the above command.)
This command yields (blank lines removed):

Creating output directory: demo_fastq
Output files will be written to: demo_fastq
Running metaphlan ........
Found g__Bacteroides.s__Bacteroides_dorei : 57.96% of mapped reads
Found g__Bacteroides.s__Bacteroides_vulgatus : 42.04% of mapped reads
Total species selected from prescreen: 2
Selected species explain 100.00% of predicted community composition
Creating custom ChocoPhlAn database ........
Running bowtie2-build ........
Running bowtie2 ........
Total bugs from nucleotide alignment: 2
g__Bacteroides.s__Bacteroides_vulgatus: 1195 hits
g__Bacteroides.s__Bacteroides_dorei: 1260 hits
Total gene families from nucleotide alignment: 545
Unaligned reads after nucleotide alignment: 88.3095238095 %
Running diamond ........
Aligning to reference database: uniref90_demo_prots_v201901b.dmnd
Total bugs after translated alignment: 3
g__Bacteroides.s__Bacteroides_vulgatus: 1195 hits
g__Bacteroides.s__Bacteroides_dorei: 1260 hits
unclassified: 252 hits
Total gene families after translated alignment: 559
Unaligned reads after translated alignment: 87.2000000000 %
Computing gene families ...
Computing pathways abundance and coverage ...
Output files created: 
demo_fastq/demo_genefamilies.tsv
demo_fastq/demo_pathabundance.tsv
demo_fastq/demo_pathcoverage.tsv
Note the three phases of HUMAnN's tiered search in the above log: 1) identifying community species (with MetaPhlAn), 2) mapping reads to community pangenomes (with bowtie2), and 3) aligning unmapped reads to a protein database (with DIAMOND).
Why is nucleotide-level search generally faster than translated search?
What makes nucleotide-level search extra fast in HUMAnN?
conda install -c bioconda metaphlan=4.0.0

conda install -c bioconda metaphlan=4.0.0

conda install -c bioconda metaphlan=4.0.0



	
	
2.3 HUMAnN default outputs
(Note: The outputs in this tutorial were generated using HUMAnN 3.0.0 with its corresponding demo databases. If you are running a different version of HUMAnN or are using a different set of databases, the outputs might differ.)
When HUMAnN is run from any input type, three main output files will be created:
$OUTPUT_DIR/$SAMPLE_genefamilies.tsv
$OUTPUT_DIR/$SAMPLE_pathabundance.tsv
$OUTPUT_DIR/$SAMPLE_pathcoverage.tsv
For the demo.fastq.gz file analyzed above, these look like:
demo_fastq/demo_genefamilies.tsv
demo_fastq/demo_pathabundance.tsv
demo_fastq/demo_pathcoverage.tsv
Let's inspect the contents of the individual files:
demo_genefamilies.tsv
This file contains the abundances of each gene family in the community in reads per kilobase (RPK) units. Each gene family's total abundance is broken down into the contributions from individual organisms when possible (delineated by the pipe character). For example:
UniRef90_G1UL42|g__Bacteroides.s__Bacteroides_dorei
Read mappings generated during (organism-agnostic) translated search are assigned to an unclassified bin. We refer to this format as "stratified output" and the individual per-organism and unclassified contributions are referred to as "stratifications". By default, gene families are identified by their unique UniRef90 identifiers (e.g. UniRef90_G1UL42 in the example above). While these are convenient for computing, they are not very easy to interpret by eye. We'll see how to attach human-readable names to UniRef90 (and other) identifiers in a subsequent section.
You can also learn more about a given UniRef family by looking up its representative protein on UniProt thusly (note the removal of the UniRef90_ prefix):
https://www.uniprot.org/uniprot/G1UL42
Reads that map to gene families that lack UniRef90 identifiers are represented as UniRef90_unknown while unmapped reads are collected as UNMAPPED. The total coverage depth (RPK) of the sample is thus represented by summing either 1) UNMAPPED, UniRef90_unknown, and all UniRef IDs prior to organism-specific decomposition, or 2) UNMAPPED, UniRef90_unknown|<organism>, and all other UniRef_*|<organism> IDs. The sum of either set of values can then be normalized to CPMs (Copies Per Million) or relative abundances.
To inspect the demo_genefamilies.tsv file, use:
column -t -s $'\t' demo_fastq/demo_genefamilies.tsv | less -S
column -t -s $'\t' demo_fastq/demo_genefamilies.tsv | less -S
*Remember that you can enter the q key to exit the file explorer
column -t -s $'\t' /home/isrania/onyea005/HUMANN082023/Staley_Project_069/demo/demo_genefamilies.tsv | less -S

column -t -s $'\t' /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Staley_Project_069/staley_subset_genefamilies.tsv | less -S
Which yields:
# Gene Family                                               demo_Abundance-RPKs
UNMAPPED                                                    18312.0000000000
UniRef90_G1UL42                                             333.3333333333
UniRef90_G1UL42|g__Bacteroides.s__Bacteroides_dorei         333.3333333333
UniRef90_I9QXW8                                             333.3333333333
UniRef90_I9QXW8|g__Bacteroides.s__Bacteroides_dorei         333.3333333333
UniRef90_A0A174QBF2                                         200.0000000000
UniRef90_A0A174QBF2|g__Bacteroides.s__Bacteroides_vulgatus  200.0000000000
UniRef90_A0A078RDY6                                         166.6666666667
UniRef90_A0A078RDY6|g__Bacteroides.s__Bacteroides_vulgatus  166.6666666667
UniRef90_G1ULL9                                             166.6666666667
UniRef90_G1ULL9|g__Bacteroides.s__Bacteroides_vulgatus      166.6666666667
UniRef90_A0A015QIN1                                         66.6666666667
UniRef90_A0A015QIN1|g__Bacteroides.s__Bacteroides_vulgatus  66.6666666667
Let's find some of the "unclassified" stratifications in this file using the grep command:
grep unclassified demo_fastq/demo_genefamilies.tsv | less -S
grep unclassified demo_fastq/demo_genefamilies.tsv | less -S

grep unclassified /home/isrania/onyea005/HUMANN082023/Staley_Project_069/demo/demo_genefamilies.tsv | less -S

grep unclassified /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Staley_Project_069/staley_subset_genefamilies.tsv | less -S

Which yields:
UniRef90_Q9ZUH4|unclassified  18.3746362785
UniRef90_W8YTG4|unclassified  13.9024544125
UniRef90_Q6XMI3|unclassified  13.5681161633
UniRef90_U5FT06|unclassified  13.4157878980
UniRef90_Z9K4C5|unclassified  12.7418650404
UniRef90_U3KI10|unclassified  12.3842423992
UniRef90_B8RBV1|unclassified  12.1442525829
UniRef90_U3KP22|unclassified  11.6963164253
UniRef90_Z9JLB1|unclassified  11.5045104368
UniRef90_Q8GT21|unclassified  11.2241858840

	•	HUMAnN can quantify protein families at 90 or 50% identity (via mapping to UniRef90 and UniRef50 representatives, respectively). What are the advantages of quantifying UniRef90 gene families?
	•	UniRef50 families?

demo_pathabundance.tsv
This file lists the abundances of each pathway in the community, also in RPK units as described for gene families. For details on the calculation of pathway abundances from constituent gene family abundances, see the HUMAnN User Manual. Like gene families, pathways are broken down into per-organism contributions when possible (again delineated by the | character), with the total read abundance consisting of pathways assigned to organisms, UNMAPPED reads, and UNINTEGRATED reads that are attributable to specific gene families (typically also within specific organisms), but for gene families that are not annotated to any pathway in the corresponding database (e.g. MetaCyc).
As with gene families, pathway RPKs can be normalized to copies per million or relative abundances by summing either 1) UNMAPPED reads, UNINTEGRATED organism-specific features, and organism-specific (i.e. pipe-containing) pathways, or 2) UNMAPPED reads, total UNINTEGRATED features, and pathways that are not broken down per organism.
To inspect the demo_pathabundance.tsv file, use:
column -t -s $'\t' demo_fastq/demo_pathabundance.tsv | less -S
column -t -s $'\t' demo_fastq/demo_pathabundance.tsv | less -S

column -t -s $'\t' /home/isrania/onyea005/HUMANN082023/Staley_Project_069/demo/demo_ pathabundance.tsv | less -S

column -t -s $'\t' /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Staley_Project_069/staley_subset_pathabundance.tsv | less -S

Which yields:
# Pathway                                                                   demo_Abundance
UNMAPPED                                                                    9520.1365299060
UNINTEGRATED                                                                4236.5362970855
UNINTEGRATED|unclassified                                                   54.1292645042
PWY-5423: oleoresin monoterpene volatiles biosynthesis                      18.3746362785
PWY-5423: oleoresin monoterpene volatiles biosynthesis|unclassified         18.3746362785
PWY-4203: volatile benzenoid biosynthesis I (ester formation)               13.3772872602
PWY-4203: volatile benzenoid biosynthesis I (ester formation)|unclassified  13.3772872602
demo_pathcoverage.tsv
(Note to instructors: Pathway coverage output is being revised; you may skip reviewing it during tutorials pending these updates.)

	•	Unlike gene family abundance, stratified pathway abundance does not necessarily sum to yield the community total pathway abundance. Why not? (Hint: HUMAnN estimates the number of complete pathways in the community or a given stratification.)
2.4 Running HUMAnN: pre-mapped SAM/BAM input
The raw results of mapping metagenome/metatranscriptome reads against a gene family nucleotide database can be saved as a SAM/BAM file and used to run HUMAnN after the mapping (e.g. bowtie2) step. The only requirement is that gene family names provided in such a file be annotated using either 1) identifiers compatible with HUMAnN's default mapping to UniRef, or 2) UniRef identifiers directly. Unmapped reads in such an input file are searched against a non-organism-specific protein database as are unmapped reads in a "full" HUMAnN run. Inspect the demo sam file:
less -S demo.sam
We can transpose and view a single row (per-read alignment) as followed:
head -1 demo.sam | tr "\t" "\n"
The head -1 command extracts the first line of the file, while the tr command replaces tabs with newlines:
s__Bacteroides_dorei_read000001
16
357276|g__Bacteroides.s__Bacteroides_dorei|UniRef90_R7NX76|UniRef50_R5GDB8|927
423
11
100M
*
0
0
TTTCGTATTAGTGGAGTATGGCATTGAAAGTACGGATGATGACACGTTGCGCAGGATAAACCGGGGACATACTTTTGCCGTTTCTGCAGAGGCTGTTCGG
IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII
AS:i:-12
XS:i:-30
XN:i:0
XM:i:2
XO:i:0
XG:i:0
NM:i:2
MD:Z:3T38A57
YT:Z:UU
We can provide this sam file as input to HUMAnN using a command similar to the one used above for the fastq file:
humann --input demo.sam --output demo_sam
which yields (blank lines removed):
Creating output directory: demo_sam
Output files will be written to: demo_sam
Process the sam mapping results ...
Computing gene families ...
Computing pathways abundance and coverage ...
Output files created: 
demo_sam/demo_genefamilies.tsv
demo_sam/demo_pathabundance.tsv
demo_sam/demo_pathcoverage.tsv
Note the creation of the three expected output files.

	•	Gap filling allows us to quantify a pathway even when a small number of its reactions are conspicuously absent. What are some potential pros and cons of this approach in real data?
2.5 Running HUMAnN: pre-computed protein blastx M8 input
HUMAnN typically searches metagenome/metatranscriptome reads against two databases: first a nucleotide database using rapid mapping, then a broader protein database using more computationally expensive translated search. One can execute HUMAnN using either search alone, however; using nucleotides only is more computationally efficient but less sensitive, and using translated search alone can emulate HUMAnN 1.0 output or be carried out even for very novel or unusual microbial communities (i.e. those with few reference genomes available).
If BLAST tab-delimited M8 format search results have already been generated for HUMAnN-compatible protein IDs (UniRef90 by default), they can be run by providing the appropriate input format. Let's examine such a file:
less -S demo.m8
Which yields:
s__Bacteroides_vulgatus_read000635|100  UniRef90_X8JRH1|978     57.9    19      8       0       95      39      189     207     2.0e-01 21.6
unclassified_read000001|100  UniRef90_W8YTG4|1614  90.6  32  3   0  98   3   382  413  9.0e-15  65.9
unclassified_read000002|100  UniRef90_U5FT06|1224  90.9  33  3   0  100  2   127  159  2.1e-14  64.7
unclassified_read000003|100  UniRef90_V5V2L3|2169  90.6  32  3   0  3    98  388  419  4.5e-14  63.5
unclassified_read000004|100  UniRef90_U5FT06|1224  90.6  32  3   0  99   4   148  179  4.6e-14  63.5
unclassified_read000005|100  UniRef90_X8FHU5|1080  90.6  32  3   0  98   3   240  271  3.1e-15  67.4
unclassified_read000006|100  UniRef90_Z9JXD8|1428  90.6  32  3   0  98   3   22   53   2.6e-14  64.3
unclassified_read000007|100  UniRef90_Q6XMI3|1137  90.6  32  3   0  3    98  155  186  2.0e-14  64.7
unclassified_read000007|100  UniRef90_Q9SPV4|1077  65.6  32  11  0  3    98  129  160  1.5e-09  48.5
unclassified_read000008|100  UniRef90_Z9JUT2|1308  90.6  32  3   0  98   3   222  253  2.4e-15  67.8
unclassified_read000009|100  UniRef90_X5M7Z0|1134  93.9  33  2   0  1    99  202  234  2.9e-16  70.9
To run HUMAnN using this file as input, use:
humann --input demo.m8 --output demo_m8
This again produces the three expected outputs (blank lines removed):
Creating output directory: /demo_m8
Output files will be written to: /demo_m8
Process the blastm8 mapping results ...
Computing gene families ...
Computing pathways abundance and coverage ...
Output files created: 
/demo_m8/demo_genefamilies.tsv
/demo_m8/demo_pathabundance.tsv
/demo_m8/demo_pathcoverage.tsv

	•	The BLAST m8 output fields allow us to compute the coverage of the database sequence by a single sequencing read (subject coverage) and the coverage of the read by the database sequence (query coverage). Why is subject coverage almost always smaller than query coverage?

2.6 Running HUMAnN: skipping translated search
Conversely, to run HUMAnN without any translated protein search (i.e. using rapid nucleotide mapping alone), one can provide the --bypass-translated-search flag. This applies to FASTA, FASTQ, SAM/BAM, or other raw nucleotide inputs:
humann --input demo.fastq.gz --output demo_fastq_fast --bypass-translated-search
Note the absence of a diamond (translated search) step in this run compared to the demo_fastq analysis above:
Creating output directory: demo_fastq_fast
Output files will be written to: demo_fastq_fast
Running metaphlan ........
Found g__Bacteroides.s__Bacteroides_dorei : 58.10% of mapped reads
Found g__Bacteroides.s__Bacteroides_vulgatus : 41.90% of mapped reads
Total species selected from prescreen: 2
Selected species explain 100.00% of predicted community composition
Creating custom ChocoPhlAn database ........
Running bowtie2-build ........
Running bowtie2 ........
Total bugs from nucleotide alignment: 2
g__Bacteroides.s__Bacteroides_vulgatus: 1195 hits
g__Bacteroides.s__Bacteroides_dorei: 1260 hits
Total gene families from nucleotide alignment: 545
Unaligned reads after nucleotide alignment: 88.3095238095 %
Bypass translated search
Computing gene families ...
Computing pathways abundance and coverage ...
Output files created:
demo_fastq_fast/demo_genefamilies.tsv
demo_fastq_fast/demo_pathabundance.tsv
demo_fastq_fast/demo_pathcoverage.tsv
It is informative to compare the results of this demo file with and without translated search; even in a small, subsampled input, including amino acid search is more computationally costly but recovers additional gene families. To count unique gene families in the original file, execute:
cut -f1 demo_fastq/demo_genefamilies.tsv | tail -n +3 | grep -v "|" | sort -u | wc -l 
What is this command doing?
	•	cut -f1 extracts the first column of the file, i.e. gene identifiers;
	•	tail -n +3 removes the first two lines of the file, i.e. the header and UNMAPPED;
	•	grep -v "|" removes the stratified headers;
	•	sort -u isolates the unique headers;
	•	wc -l counts those headers.
The resulting answer is 559 (unique gene families). The same command executed on the file demo_fastq_fast/demo_genefamilies.tsv yields 545 unique gene families. We infer that 559 - 545 = 14 additional gene families were uncovered using translated search. (On a non-demo metagenome, the relative number of additional gene families uncovered by translated search would be far larger!)
In general, any HUMAnN intermediate file can be saved and used to resume processing from an intermediate point in the analysis pipeline, and several steps including taxonomic pre-screening, nucleotide search, or amino acid search can be independently skipped. For more information, see the relevant sections of the HUMAnN User Manual.

	•	Under what circumstances would skipping translated search be reasonable?
	•	Under what circumstances would skipping translated search be a bad idea?
3. Manipulating HUMAnN output tables
3.1 Normalizing RPKs to relative abundance
To facilitate comparisons between samples with different sequencing depths, it is important to normalize HUMAnN's default RPK values prior to performing statistical analyses. Sum-normalization to relative abundance or "copies per million" (CPM) units are common approaches for this; these methods are implemented in the humann_renorm_table script (with special considerations for HUMAnN's output formats, e.g. feature stratification). Let's normalize our gene abundances to CPM units:
humann_renorm_table --input demo_fastq/demo_genefamilies.tsv \
    --output demo_fastq/demo_genefamilies-cpm.tsv --units cpm --update-snames

humann_renorm_table --input demo_fastq/demo_genefamilies.tsv \
 --output demo_fastq/demo_genefamilies-cpm.tsv --units cpm --update-snames

humann_renorm_table --input /home/isrania/onyea005/HUMANN082023/Staley_Project_069/demo/demo_genefamilies.tsv \
 --output /home/isrania/onyea005/HUMANN082023/Staley_Project_069/demo/demo_genefamilies-cpm.tsv --units cpm --update-snames

humann_renorm_table --input /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Staley_Project_069/staley_subset_genefamilies.tsv \
 --output /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Staley_Project_069/staley_subset_genefamilies_cpm.tsv --units cpm --update-snames
Inspect the output file demo_fastq/demo_genefamilies-cpm.tsv using the less command as we did above:
# Gene Family                                               demo_Abundance-CPM
UNMAPPED                                                    690444
UniRef90_G1UL42                                             12568.2
UniRef90_G1UL42|g__Bacteroides.s__Bacteroides_dorei         12568.2
UniRef90_I9QXW8                                             12568.2
UniRef90_I9QXW8|g__Bacteroides.s__Bacteroides_dorei         12568.2
UniRef90_A0A174QBF2                                         7540.89
UniRef90_A0A174QBF2|g__Bacteroides.s__Bacteroides_vulgatus  7540.89
UniRef90_A0A078RDY6                                         6284.08
UniRef90_A0A078RDY6|g__Bacteroides.s__Bacteroides_vulgatus  6284.08
UniRef90_G1ULL9                                             6284.08
UniRef90_G1ULL9|g__Bacteroides.s__Bacteroides_vulgatus      6284.08
UniRef90_A0A015QIN1                                         2513.63
UniRef90_A0A015QIN1|g__Bacteroides.s__Bacteroides_vulgatus  2513.63
Note the difference in scale: units tend to be larger than in the original file as a result of being scaled to sum to 1 million. Also note that the sample header has been updated from demo_Abundance-RPK to demo_Abundance-CPM courtesy of the --update-snames flag.
By default, humann_renorm_table 1) forces the community total abundances to sum to 1 million and then 2) adjusts the stratifications so that they continue to behave as fractions of community total abundances (see humann_renorm_table -h for additional options). Let's verify that the unstratified abundance values sum up to 1 million:
grep -v "#" demo_fastq/demo_genefamilies-cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc
grep -v "#" demo_fastq/demo_genefamilies-cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc

grep -v "#" /home/isrania/onyea005/HUMANN082023/Staley_Project_069/demo/demo_genefamilies-cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc

grep -v "#" /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Staley_Project_069/staley_subset_genefamilies_cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc
What is this command doing?
	•	grep -v "#": excludes lines containing "#", in this case the file's header;
	•	grep -v "|": excludes lines containing "|", which mark stratified abundances;
	•	cut -f2: extracts the second column of the file containing the abundances;
	•	paste -s -d+ -: converts the column of numbers to a summation (1 + 2 + 3 + ...);
	•	bc: sums the numbers.
The final answer should be very close to 1 million (it may be slightly off due to rounding error). A small modification of the above command can be used to find the sum of the stratified values (this sum will be < 1,000,000 unless you also account for the UNMAPPED value).

	•	HUMAnN natively normalizes gene family abundance by gene length. Why is this step important? (In other words, why are RPK units more interpretable than raw read counts?)
3.2 Regrouping genes to other functional categories
HUMAnN's default "units" of microbial function are UniRef gene families (which we use internally to compute reaction abundances, and from there pathway abundances). However, starting from gene family abundance, it is possible to reconstruct the abundance of other functional categories in a microbiome using the humann_regroup_table script. Inspect the regrouping options using the humann_regroup_table -h command.
Let's regroup our CPM-normalized gene family abundance values to MetaCyc reaction (RXN) abundances, which are bundled with the default HUMAnN installation:
humann_regroup_table --input demo_fastq/demo_genefamilies-cpm.tsv \
    --output demo_fastq/rxn-cpm.tsv --groups uniref90_rxn
humann_regroup_table --input demo_fastq/demo_genefamilies-cpm.tsv \
 --output demo_fastq/rxn-cpm.tsv --groups uniref90_rxn

humann_regroup_table --input /home/isrania/onyea005/HUMANN082023/Staley_Project_069/demo/demo_genefamilies-cpm.tsv \
 --output /home/isrania/onyea005/HUMANN082023/Staley_Project_069/demo/demo_rxn-cpm.tsv --groups uniref90_rxn

humann_regroup_table --input /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Staley_Project_069/staley_subset_genefamilies_cpm.tsv \
 --output /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Staley_Project_069/staley_subset_genefamilies_rxn_cpm.tsv --groups uniref90_rxn
We are given the output message:
Original Feature Count: 560; Grouped 1+ times: 60 (10.7%); Grouped 2+ times: 13 (2.3%)
Indicating that the original file contained 560 features (UniRef90 gene families and UNMAPPED). Of these, 60 were annotated to a RXN, and 13 were annotated to more than one RXN. Inspect the output file demo_fastq/rxn-cpm.tsv using the less command:
less demo_fastq/rxn-cpm.tsv 

less /home/isrania/onyea005/HUMANN082023/Staley_Project_069/demo/demo_rxn-cpm.tsv

less /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Staley_Project_069/staley_subset_genefamilies_rxn_cpm.tsv 

# Gene Family                                        demo_Abundance-CPM
UNMAPPED                                             690444.0
UNGROUPED                                            290292.34000000026
UNGROUPED|g__Bacteroides.s__Bacteroides_dorei        139621.93409999993
UNGROUPED|g__Bacteroides.s__Bacteroides_vulgatus     148318.30399999986
UNGROUPED|unclassified                               2352.097
2.5.1.19-RXN                                         233.716
2.5.1.19-RXN|g__Bacteroides.s__Bacteroides_dorei     133.704
2.5.1.19-RXN|g__Bacteroides.s__Bacteroides_vulgatus  100.012
2.7.13.3-RXN                                         236.729
2.7.13.3-RXN|g__Bacteroides.s__Bacteroides_dorei     172.166
2.7.13.3-RXN|g__Bacteroides.s__Bacteroides_vulgatus  64.5624
2.7.7.13-RXN                                         398.989
2.7.7.13-RXN|g__Bacteroides.s__Bacteroides_dorei     119.697
2.7.7.13-RXN|g__Bacteroides.s__Bacteroides_vulgatus  279.292
We see that UniRef90 features have been regrouped as RXN features, such as 2.5.1.19-RXN. A new stratified feature also appears in the output: UNGROUPED. This feature reports the total abundance of features that were not otherwise included in a group (similar to the UNINTEGRATED feature of the pathways output). If each UniRef90 gene family were regrouped into at most one RXN, the total community abundance of features in the regrouped file (including UNGROUPED and UNMAPPED) would still be 1 million.
The actual sum here (1,024,360) is slightly higher than 1 million due to the 13 UniRef90 features whose abundances were counted in two (or more) different RXN features. You can verify this by modifying the summation command we used above:
grep -v "#" demo_fastq/rxn-cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc
grep -v "#" demo_fastq/rxn-cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc

grep -v "#" /home/isrania/onyea005/HUMANN082023/Staley_Project_069/demo/demo_rxn-cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc

grep -v "#" /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Staley_Project_069/staley_subset_genefamilies_rxn_cpm.tsv | grep -v "|" | cut -f2 | paste -s -d+ - | bc
You could force the RXN abundances themselves to sum to 1 million by running humann_renorm_table on the above table, or by regrouping UniRef90s to RXNs before renormalizing their abundances (and then normalizing at the RXN stage). The results will be the same! (That said, in practice, we typically normalize only once at the gene family level to adjust for initial differences in sample sequencing depth.)

	•	When regrouping genes to RXNs (or ECs or KOs), we sum over the gene sequences belonging to a particular group. Under what circumstance would it make sense to average over the members of a group rather than sum? (Hint: the answer is related to the difference between the "is_a" and "part_of" relationships used in the Gene Ontology.)

3.3 Attaching names to features
So far we've been looking at a bunch of UniRef90 codes and RXN codes. While HUMAnN is happy to work with these all day, it's typically helpful to attach some human-readable descriptions of these IDs to facilitate biological interpretation. This can be accomplished with the humann_rename_table script. Let's attach names to the RXN table we generated above:
humann_rename_table --input demo_fastq/rxn-cpm.tsv \
    --output demo_fastq/rxn-cpm-named.tsv --names metacyc-rxn
humann_rename_table --input demo_fastq/rxn-cpm.tsv \
--output demo_fastq/rxn-cpm-named.tsv --names metacyc-rxn

humann_rename_table --input /home/isrania/onyea005/HUMANN082023/Staley_Project_069/demo/demo_rxn-cpm.tsv \
--output /home/isrania/onyea005/HUMANN082023/Staley_Project_069/demo/demo_rxn-cpm-named.tsv --names metacyc-rxn

humann_rename_table --input /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Staley_Project_069/staley_subset_genefamilies_rxn_cpm.tsv \
--output /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Staley_Project_069/staley_subset_genefamilies_rxn_cpm_named.tsv --names metacyc-rxn

We are told that the script Renamed 116 of 118 entries (98.31%), suggesting that most features were successfully renamed (indeed, the two features that did not receive new names were the special UNMAPPED and UNGROUPED features). Let's confirm this by inspecting the new output table with less:
less /home/isrania/onyea005/HUMANN082023/Staley_Project_069/demo/demo_rxn-cpm-named.tsv

less /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Staley_Project_069/staley_subset_genefamilies_rxn_cpm_named.tsv

# Gene Family                                                                                                          demo_Abundance-CPM
UNMAPPED                                                                                                               690444.0
UNGROUPED                                                                                                              290292.34000000026
UNGROUPED|g__Bacteroides.s__Bacteroides_dorei                                                                          139621.93409999993
UNGROUPED|g__Bacteroides.s__Bacteroides_vulgatus                                                                       148318.30399999986
UNGROUPED|unclassified                                                                                                 2352.097
2.5.1.19-RXN: (expasy) 3-phosphoshikimate 1-carboxyvinyltransferase [2.5.1.19]                                         233.716
2.5.1.19-RXN: (expasy) 3-phosphoshikimate 1-carboxyvinyltransferase [2.5.1.19]|g__Bacteroides.s__Bacteroides_dorei     133.704
2.5.1.19-RXN: (expasy) 3-phosphoshikimate 1-carboxyvinyltransferase [2.5.1.19]|g__Bacteroides.s__Bacteroides_vulgatus  100.012
2.7.13.3-RXN: (expasy) Histidine kinase [2.7.13.3]                                                                     236.729
2.7.13.3-RXN: (expasy) Histidine kinase [2.7.13.3]|g__Bacteroides.s__Bacteroides_dorei                                 172.166
2.7.13.3-RXN: (expasy) Histidine kinase [2.7.13.3]|g__Bacteroides.s__Bacteroides_vulgatus                              64.5624
2.7.7.13-RXN: (expasy) Mannose-1-phosphate guanylyltransferase [2.7.7.13]                                              398.989
2.7.7.13-RXN: (expasy) Mannose-1-phosphate guanylyltransferase [2.7.7.13]|g__Bacteroides.s__Bacteroides_dorei          119.697
2.7.7.13-RXN: (expasy) Mannose-1-phosphate guanylyltransferase [2.7.7.13]|g__Bacteroides.s__Bacteroides_vulgatus       279.292
It turns out that our friend 2.5.1.19-RXN is actually 3-phosphoshikimate 1-carboxyvinyltransferase, though you probably already knew that... In the event that a name can't be found for a non-special feature (e.g. UNMAPPED), the script will assign it the name NO_NAME for consistency.
The regrouping and renaming options used above are limited for demo purposes. Just as can install full-scale nucleotide and protein databases for real-world HUMAnN use, you can install a full-scale "utility mapping" database to help out with your downstream analyses. See the humann_databases script for acquiring these databases.

	•	From a statistical testing perspective, what could be an advantage of regrouping raw gene abundance to a smaller list of gene groups?
	•	What is a more practical advantage of regrouping?


	
**I also exported the TSV genefamilies_cpm_rxn_named file to excel to manually search for the beta glucuronidase  pathways  using the search function in  Excel. The pathways are mapped using MetaCYC  (see the note above). Can also google pathways (https://biocyc.org/gene?orgid=META&id=BETA-GLUCURONID-MONOMER)

5. Plotting stratified functions (figure out how to get a pathabund.pcl in a regular dataset)

For this section, we'll operate on a table of pathway abundances from ~400 samples from the Human Microbiome Project (as originally profiled by HUMAnN 2.0):
To download the files rightclick on the hyperlink on the webpage and select “save link as”

*See https://forum.biobakery.org/t/tool-to-generate-pcl-type-file/1204
*Also see https://groups.google.com/g/humann-users/c/9WZT7mLAa54?pli=1
It seems that the pcl fine is the following “This script expects the file to be a tab-delimited table (PCL just implies features on the rows and samples on the columns). My best guess at the moment is that your file picked up some additional formatting (from being edited in Excel, for example). Can you run "dos2unix $FILE" on your file (where $FILE is the file name)? This may help to restore the file to normal text.”

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
hmp_pathabund.pcl
Download or copy (if in VM: cp input/hmp_pathabund.pcl .) this file to your working directory and examine it using less:

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
hmp_pathabund.pcl
less -S hmp_pathabund.pcl
Which yields:

FEATURE \ SAMPLE                                                                                      SRS011084    SRS011086      SRS011090
STSite                                                                                                Stool        Tongue_dorsum  Buccal_mucosa
1CMET2-PWY: N10-formyl-tetrahydrofolate biosynthesis                                                  0.000498359  0.000628096    0.000304951
1CMET2-PWY: N10-formyl-tetrahydrofolate biosynthesis|g__Acidovorax.s__Acidovorax_ebreus               0            0              0
1CMET2-PWY: N10-formyl-tetrahydrofolate biosynthesis|g__Bacteroides.s__Bacteroides_finegoldii         0            0              0
1CMET2-PWY: N10-formyl-tetrahydrofolate biosynthesis|g__Haemophilus.s__Haemophilus_influenzae         0            0              0
1CMET2-PWY: N10-formyl-tetrahydrofolate biosynthesis|g__Klebsiella.s__Klebsiella_pneumoniae           0            0              0
1CMET2-PWY: N10-formyl-tetrahydrofolate biosynthesis|g__Staphylococcus.s__Staphylococcus_epidermidis  0            0              0
1CMET2-PWY: N10-formyl-tetrahydrofolate biosynthesis|g__Veillonella.s__Veillonella_sp_oral_taxon_158  0            0              0
1CMET2-PWY: N10-formyl-tetrahydrofolate biosynthesis|unclassified                                     7.89502e-06  1.60101e-05    0

We can see three samples from three different body sites (via the STSite metadatum). A single pathway, N10-formyl-tetrahydrofolate biosynthesis (1CMET2-PWY), and its stratifications are visible. In these three samples, the pathway can only be assigned to the unclassified stratum, and with very low abundance. In a third sample (SRS011090) the pathway is only observed at the community level (how?).

Using MaAsLin 2.0 (or another method for making statistical associations with microbiome data), we can compare community total abundances from HUMAnN tables with sample metadata. In this case, a model relating community-total pathway abundance to sample body site reveals a strong enrichment for L-homoserine and L-methionine biosynthesis (METSYN-PWY) at the three oral sites (buccal mucosa, tongue, and plaque).

Now that we have identified this enrichment, use the grep command to explore the stratifications of this pathway from the hmp_pathabund.pcl file.

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
grep 'METSYN-PWY' hmp_pathabund.pcl | less -S
Do the major contributing species seem reasonable for an orally-enriched pathway?
HUMAnN includes a utility humann_barplot to assist with visualizing stratified outputs. Let's use this tool to make a plot of the differentially abundant pathway we identified above:

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_barplot --input hmp_pathabund.pcl --focal-metadata STSite --last-metadata STSite \
  --output plot1.png --focal-feature METSYN-PWY 

Trying to generate the distribution of betaglucuronidase in our dataset using the focal_feature option (I was able to find the code “BETA-GLUCURONID-RXN” in the excel file from the genefamiles.tsv file processed in step 3)
cd /panfs/roc/groups/5/isrania/onyea005/Israni4_Project_016
column -t -s $'\t' /panfs/roc/groups/5/isrania/onyea005/Israni4_Project_016/mission_subset_genefamilies-cpm-snames-rxn-named.tsv | less -S
humann_barplot --input /panfs/roc/groups/5/isrania/onyea005/Israni4_Project_016/mission_subset_genefamilies-cpm-snames-rxn-named.tsv \
  --output plot_betaglucuronidase_test.png --focal-feature BETA-GLUCURONID-RXN
Use see plot1.png to open the resulting figure if working in a graphical environment.)



Not terribly informative... Let's try adding a "sort" on the summed stratified abundance:

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_barplot --input hmp_pathabund.pcl --focal-metadata STSite --last-metadata STSite \
    --output plot2.png --focal-feature METSYN-PWY --sort sum


A pattern has started to emerge: we can clearly see that the oral body sites are enriched on the left (high) end of the plot. We can continue this line of analysis by adding an additional sort on body site (metadata) and "logstack" scaling (i.e. scaling the tops of the stacked bars in proportion to the log of community abundance, while scaling species contributions linearly within total bar height):

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_barplot --input hmp_pathabund.pcl --focal-metadata STSite --last-metadata STSite \
    --output plot3.png --focal-feature METSYN-PWY --sort sum metadata --scaling logstack



We see now that the pathway is most strongly enriched in the buccal mucosa, where it is contributed almost exclusively by Streptococcus species. The pathway's relative abundance in the other two oral sites is generally lower (by about an order of magnitude, as emphasized on the log scale), and is more often "unclassified" at the tongue body site.

How could you find other pathways with a similar abundance distribution in this functional profile?
What might be causing the "unclassified" contributors of homoserine/methionine synthesis in tongue dorsum communities?
Let's examine one additional pathway, coenzyme A biosynthesis (COA-PWY), which is more broadly conserved across body sites:



cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_barplot --input hmp_pathabund.pcl --focal-metadata STSite --last-metadata STSite \
    --output plot4.png --focal-feature COA-PWY --sort sum


Now sorting by total abundance is less informative. To remedy that, let's try some additional plotting options: 1) regrouping species to genera (--as-genera), 2) removing samples that lack the coenzyme A biosynthesis pathway (--remove-zeros), 3) and sorting on ecological similarity (--sort braycurtis):

cd /Volumes/RAVPOWER/ROOK_PC/HUMANN
humann_barplot --input hmp_pathabund.pcl --focal-metadata STSite --last-metadata STSite \
    --output plot5.png --focal-feature COA-PWY --sort braycurtis --scaling logstack --as-genera --remove-zeros


Notably, sorting by the similarity of pathway contributions has tended to group samples from the same body site even without the explicit metadata sort. This is because, for broadly distributed pathways, organismal contributions tend to follow overall sample ecology (which tends to be quite different from one body site to the next). This plot also includes an appearance of the dark gray "other" bin, which in this case represents the sum of contributions from named genera that were outside of the top N we selected for coloring (default N=18).

Is it surprising to see coenzyme A biosynthesis present in diverse human body site microbiomes?

Testing the various grouping methods to assess Beta-glucuronidase transcripts in the MISSION HUMAnN 3 processed dataset.

Understanding UNMAPPED vs UNGROUPED

https://forum.biobakery.org/t/loss-of-information-when-doing-renorm-then-regroup/80/3
https://forum.biobakery.org/t/which-database-should-i-use-to-run-humann-regroup-table-and-humann-rename-table-command/1141/5
https://forum.biobakery.org/t/extremely-high-unmapped-rates-in-human-metatranscriptomics-samples/801/2
https://forum.biobakery.org/t/one-to-many-problem-with-humann2-regroup-table/511
https://groups.google.com/g/humann-users/c/tlkjsp22iPg
https://training.galaxyproject.org/archive/2021-10-01//topics/metagenomics/tutorials/metatranscriptomics-short/tutorial.html

EC4 category for beta glucuronidase (https://www.sciencedirect.com/topics/biochemistry-genetics-and-molecular-biology/enzyme-commission-number)
Step1: Convert the dataset to metacyc, Ec4 and pfam based on the advice in the biobakery forums
Activate the conda environment
Now that you have an environment for HUMANN, activate it using the environment’s name:
After the update, I had to use the full name to active conda environments (https://stackoverflow.com/questions/58369030/could-not-find-conda-environment)
conda info --envs
source activate /Users/guillaumeonyeaghala/miniconda3/envs/mpa

Mappings are available for both UniRef90 and UniRef50 gene families to the following systems:

MetaCyc Reactions
KEGG Orthogroups (KOs)
Pfam domains
Level-4 enzyme commission (EC) categories
EggNOG (including COGs)
Gene Ontology (GO)
Informative GO

The files had been downloaded at “/Volumes/RAVPOWER2/ROOK_PC/HUMANN/Databases/utility_mapping” when I did the installation

AUG2023: I checked and the needed files were indeed present at /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/databases5/utility_mapping




Part 1: Uniref90ec filtered dataset
Let's regroup our CPM-normalized gene family abundance values to MetaCyc reaction (RXN) abundances, which are bundled with the default HUMAnN installation:
humann_regroup_table --input demo_fastq/demo_genefamilies-cpm.tsv \
    --output demo_fastq/rxn-cpm.tsv --groups uniref90_rxn

cd /Users/guillaumeonyeaghala/Desktop
humann_regroup_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_genefamilies.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_genefamilies_rxn.tsv --groups uniref90_rxn

*Normalizing the values to cpm
humann_renorm_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_genefamilies_rxn.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_genefamilies_rxn_cpm.tsv --units cpm --update-snames

*Renaming the features
humann_rename_table -h

humann_rename_table --input demo_fastq/rxn-cpm.tsv \
    --output demo_fastq/rxn-cpm-named.tsv --names metacyc-rxn

humann_rename_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_genefamilies_rxn_cpm.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_genefamilies_rxn_cpm_named.tsv --names metacyc-rxn

*Let’s regroup based on EC4 numbers
cd /Users/guillaumeonyeaghala/Desktop
humann_regroup_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_genefamilies.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_genefamilies_ec4.tsv --custom /Volumes/RAVPOWER2/ROOK_PC/HUMANN/Databases/utility_mapping/map_level4ec_uniref90.txt.gz

humann_regroup_table --input /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Staley_Project_069/staley_subset_genefamilies_cpm.tsv \
--output /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Staley_Project_069/staley_subset_genefamilies_rxn_cpm_ec4.tsv --custom /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/databases5/utility_mapping/map_level4ec_uniref90.txt.gz


*Normalizing the values to cpm
humann_renorm_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_genefamilies_ec4.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_genefamilies_ec4_cpm.tsv --units cpm --update-snames

*Renaming the features
humann_rename_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_genefamilies_ec4_cpm.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_genefamilies_ec4_cpm_named.tsv --names ec

humann_rename_table --input /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Staley_Project_069/staley_subset_genefamilies_rxn_cpm_ec4.tsv \
--output /panfs/jay/groups/4/isrania/onyea005/HUMANN082023/Staley_Project_069/staley_subset_genefamilies_rxn_cpm_ec4_named.tsv --names ec

*Let’s regroup based on pfam numbers
cd /Users/guillaumeonyeaghala/Desktop
humann_regroup_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_genefamilies.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_genefamilies_pfam.tsv --custom /Volumes/RAVPOWER2/ROOK_PC/HUMANN/Databases/utility_mapping/map_pfam_uniref90.txt.gz

*Normalizing the values to cpm
humann_renorm_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_genefamilies_pfam.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_genefamilies_pfam_cpm.tsv --units cpm --update-snames

*Renaming the features
humann_rename_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_genefamilies_pfam_cpm.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_genefamilies_pfam_cpm_named.tsv --names pfam

You can then go to http://pfam.xfam.org/family/pf00703 to identify the proper pfamcode for  beta_glucuronidase

Part 2: Uniref90 full dataset
Let's regroup our CPM-normalized gene family abundance values to MetaCyc reaction (RXN) abundances, which are bundled with the default HUMAnN installation:
humann_regroup_table --input demo_fastq/demo_genefamilies-cpm.tsv \
    --output demo_fastq/rxn-cpm.tsv --groups uniref90_rxn

cd /Users/guillaumeonyeaghala/Desktop
humann_regroup_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_full_genefamilies.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_full_genefamilies_rxn.tsv --groups uniref90_rxn

*Normalizing the values to cpm
humann_renorm_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_full_genefamilies_rxn.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_full_genefamilies_rxn_cpm.tsv --units cpm --update-snames

*Renaming the features
humann_rename_table -h

humann_rename_table --input demo_fastq/rxn-cpm.tsv \
    --output demo_fastq/rxn-cpm-named.tsv --names metacyc-rxn

humann_rename_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_full_genefamilies_rxn_cpm.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_full_genefamilies_rxn_cpm_named.tsv --names metacyc-rxn

*Let’s regroup based on EC4 numbers
cd /Users/guillaumeonyeaghala/Desktop
humann_regroup_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_full_genefamilies.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_full_genefamilies_ec4.tsv --custom /Volumes/RAVPOWER2/ROOK_PC/HUMANN/Databases/utility_mapping/map_level4ec_uniref90.txt.gz

*Normalizing the values to cpm
humann_renorm_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_full_genefamilies_ec4.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_full_genefamilies_ec4_cpm.tsv --units cpm --update-snames

*Renaming the features
humann_rename_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_full_genefamilies_ec4_cpm.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_full_genefamilies_ec4_cpm_named.tsv --names ec

*Let’s regroup based on pfam numbers
cd /Users/guillaumeonyeaghala/Desktop
humann_regroup_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_full_genefamilies.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_full_genefamilies_pfam.tsv --custom /Volumes/RAVPOWER2/ROOK_PC/HUMANN/Databases/utility_mapping/map_pfam_uniref90.txt.gz

*Normalizing the values to cpm
humann_renorm_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_full_genefamilies_pfam.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_full_genefamilies_pfam_cpm.tsv --units cpm --update-snames

*Renaming the features
humann_rename_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_full_genefamilies_pfam_cpm.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_full_genefamilies_pfam_cpm_named.tsv --names pfam



You can then go to http://pfam.xfam.org/family/pf00703 to identify the proper pfamcode for  beta_glucuronidase


Part 3: Uniref50 full dataset
Let's regroup our CPM-normalized gene family abundance values to MetaCyc reaction (RXN) abundances, which are bundled with the default HUMAnN installation:
humann_regroup_table --input demo_fastq/demo_genefamilies-cpm.tsv \
    --output demo_fastq/rxn-cpm.tsv --groups uniref90_rxn

cd /Users/guillaumeonyeaghala/Desktop
humann_regroup_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_50_full_genefamilies.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_50_full_genefamilies_rxn.tsv --groups uniref50_rxn

*Normalizing the values to cpm
humann_renorm_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_50_full_genefamilies_rxn.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_50_full_genefamilies_rxn_cpm.tsv --units cpm --update-snames

*Renaming the features
humann_rename_table -h

humann_rename_table --input demo_fastq/rxn-cpm.tsv \
    --output demo_fastq/rxn-cpm-named.tsv --names metacyc-rxn

humann_rename_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_50_full_genefamilies_rxn_cpm.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_50_full_genefamilies_rxn_cpm_named.tsv --names metacyc-rxn

*Let’s regroup based on EC4 numbers
cd /Users/guillaumeonyeaghala/Desktop
humann_regroup_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_50_full_genefamilies.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_50_full_genefamilies_ec4.tsv --custom /Volumes/RAVPOWER2/ROOK_PC/HUMANN/Databases/utility_mapping/map_level4ec_uniref50.txt.gz

*Normalizing the values to cpm
humann_renorm_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_50_full_genefamilies_ec4.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_50_full_genefamilies_ec4_cpm.tsv --units cpm --update-snames

*Renaming the features
humann_rename_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_50_full_genefamilies_ec4_cpm.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_50_full_genefamilies_ec4_cpm_named.tsv --names ec

*Let’s regroup based on pfam numbers
cd /Users/guillaumeonyeaghala/Desktop
humann_regroup_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_50_full_genefamilies.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_50_full_genefamilies_pfam.tsv --custom /Volumes/RAVPOWER2/ROOK_PC/HUMANN/Databases/utility_mapping/map_pfam_uniref50.txt.gz

*Normalizing the values to cpm
humann_renorm_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_50_full_genefamilies_pfam.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_50_full_genefamilies_pfam_cpm.tsv --units cpm --update-snames

*Renaming the features
humann_rename_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_50_full_genefamilies_pfam_cpm.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_50_full_genefamilies_pfam_cpm_named.tsv --names pfam

You can then go to http://pfam.xfam.org/family/pf00703 to identify the proper pfamcode for  beta_glucuronidase


Part 4: Uniref50ecfiltered dataset
Let's regroup our CPM-normalized gene family abundance values to MetaCyc reaction (RXN) abundances, which are bundled with the default HUMAnN installation:
humann_regroup_table --input demo_fastq/demo_genefamilies-cpm.tsv \
    --output demo_fastq/rxn-cpm.tsv --groups uniref90_rxn

cd /Users/guillaumeonyeaghala/Desktop
humann_regroup_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_50ecfiltered_genefamilies.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_50ecfiltered_genefamilies_rxn.tsv --groups uniref50_rxn

*Normalizing the values to cpm
humann_renorm_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_50ecfiltered_genefamilies_rxn.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_50ecfiltered_genefamilies_rxn_cpm.tsv --units cpm --update-snames

*Renaming the features
humann_rename_table -h

humann_rename_table --input demo_fastq/rxn-cpm.tsv \
    --output demo_fastq/rxn-cpm-named.tsv --names metacyc-rxn

humann_rename_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_50ecfiltered_genefamilies_rxn_cpm.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_50ecfiltered_genefamilies_rxn_cpm_named.tsv --names metacyc-rxn

*Let’s regroup based on EC4 numbers
cd /Users/guillaumeonyeaghala/Desktop
humann_regroup_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_50ecfiltered_genefamilies.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_50ecfiltered_genefamilies_ec4.tsv --custom /Volumes/RAVPOWER2/ROOK_PC/HUMANN/Databases/utility_mapping/map_level4ec_uniref50.txt.gz

*Normalizing the values to cpm
humann_renorm_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_50ecfiltered_genefamilies_ec4.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_50ecfiltered_genefamilies_ec4_cpm.tsv --units cpm --update-snames

*Renaming the features
humann_rename_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_50ecfiltered_genefamilies_ec4_cpm.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_50ecfiltered_genefamilies_ec4_cpm_named.tsv --names ec

*Let’s regroup based on pfam numbers
cd /Users/guillaumeonyeaghala/Desktop
humann_regroup_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_50ecfiltered_genefamilies.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_50ecfiltered_genefamilies_pfam.tsv --custom /Volumes/RAVPOWER2/ROOK_PC/HUMANN/Databases/utility_mapping/map_pfam_uniref50.txt.gz

*Normalizing the values to cpm
humann_renorm_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_50ecfiltered_genefamilies_pfam.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_50ecfiltered_genefamilies_pfam_cpm.tsv --units cpm --update-snames

*Renaming the features
humann_rename_table --input /Users/guillaumeonyeaghala/Desktop/mission_subset_50ecfiltered_genefamilies_pfam_cpm.tsv \
    --output /Users/guillaumeonyeaghala/Desktop/mission_subset_50ecfiltered_genefamilies_pfam_cpm_named.tsv --names pfam

You can then go to http://pfam.xfam.org/family/pf00703 to identify the proper pfamcode for  beta_glucuronidase


Explanation time. How to choose between Uniref90 and Uniref 50?

https://www.uniprot.org/help/uniref

The UniProt Reference Clusters (UniRef) provide clustered sets of sequences from the UniProt Knowledgebase (including isoforms) and selected UniParc records in order to obtain complete coverage of the sequence space at several resolutions while hiding redundant sequences (but not their descriptions) from view. Unlike in UniParc, sequence fragments are merged in UniRef: The UniRef100 database combines identical sequences and sub-fragments with 11 or more residues from any organism into a single UniRef entry, displaying the sequence of a representative protein, the accession numbers of all the merged entries and links to the corresponding UniProtKB and UniParc records. UniRef90 is built by clustering UniRef100 sequences with 11 or more residues using the MMseqs2 algorithm (Steinegger M. and Soeding J., Nat. Commun. 9 (2018)) such that each cluster is composed of sequences that have at least 90% sequence identity to and 80% overlap with the longest sequence (a.k.a. seed sequence) of the cluster. Similarly, UniRef50 is built by clustering UniRef90 seed sequences that have at least 50% sequence identity to and 80% overlap with the longest sequence in the cluster. Prior to 2013 there was no overlap threshold, so clusters were more heterogeneous in length. UniRef90 and UniRef50 yield a database size reduction of approximately 58% and 79%, respectively, providing for significantly faster sequence similarity searches. The seed sequence is the longest member of a cluster. However, the longest sequence is not always the most informative. There is often more biologically relevant information (name, function, cross-references) available on other cluster members. All the proteins in a cluster are therefore ranked with the following priority to facilitate the selection of a biologically relevant representative for the cluster:
	•	quality of the entry: manually reviewed entries (from the UniProtKB/Swiss-Prot section) are preferred
	•	annotation score: entries that have higher UniProtKB annotation scores are preferred. This also means that UniProtKB entries will always take precedence over entries that are in UniParc but not in UniProtKB (annotation score is undefined in UniParc, which does not contain any annotations).
	•	organism: entries from reference proteomes and model organisms are preferred
	•	length of the sequence: longest sequence is preferred
How does this link to unmapped vs ungrouped in our outputs?
What is important to remember is that it is not  unusual to have a  50 /50 split between your unmapped vs mapped features in your  WGS or  metatranscriptomics dataset. As described here (https://groups.google.com/g/humann-users/c/YHv8gIzhiqg), “UniRef90 is a good default choice since it is comprehensive, non-redundant, and more likely to contain isofunctional clusters. UniRef50 clusters can be very broad, so there's a risk that the cluster representative might not reflect the function of the homologous sequence(s) found in your dataset. One situation where UniRef50 might be preferable is when dealing with very poorly characterized microbiomes. In that case, requiring reads to map at 90% identity to UniRef90 might be too stringent, and so mapping at 50% identity to UniRef50 could explain a larger portion of sample reads (albeit at reduced resolution)”. This means that you may have a slightly lower amount of unmapped reads when you use uniref50 instead of uniref90, but it does not affect the proportion of ungrouped features. (for example, remappung using uniref50 meant we went from 55% unmapped on average to 49% unmapped on average)

Understanding UNMAPPED vs UNGROUPED

https://forum.biobakery.org/t/loss-of-information-when-doing-renorm-then-regroup/80/3
https://forum.biobakery.org/t/which-database-should-i-use-to-run-humann-regroup-table-and-humann-rename-table-command/1141/5
https://forum.biobakery.org/t/extremely-high-unmapped-rates-in-human-metatranscriptomics-samples/801/2
https://forum.biobakery.org/t/one-to-many-problem-with-humann2-regroup-table/511
https://groups.google.com/g/humann-users/c/tlkjsp22iPg
https://training.galaxyproject.org/archive/2021-10-01//topics/metagenomics/tutorials/metatranscriptomics-short/tutorial.html

When we are specifically discussing the issue of ungrouped features, we can find the following information here (https://forum.biobakery.org/t/loss-of-information-when-doing-renorm-then-regroup/80/3) “Having about a 50/50 mix of known/unknown reads is very typical. The ungrouped fraction depends on how rare the group annotation is that you are using. For example most proteins have a pfam domain, so grouping by pfam leaves a small ungrouped fraction. Conversely only ~10% of proteins have an EC annotation, so that leaves a larger ungrouped fraction”. 












*Next steps: Figuring out how to get the shotgun taxonomy from the shotgun  sequencing output (not broken down by  gene function byt by phylogeny) [https://github.com/biobakery/humann]


	


