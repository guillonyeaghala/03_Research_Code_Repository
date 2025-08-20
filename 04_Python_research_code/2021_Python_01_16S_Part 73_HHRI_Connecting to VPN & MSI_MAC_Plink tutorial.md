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
Duluth Faculty, Staff, and Students
Follow the instructions on the VPN: Virtual Private Network page.
Twin Cities Health Care Component Users
Follow the instructions on HST/AHC: VPN and Remote Desktop Setup.
Other Faculty, Staff, and Students
Find the client and instructions for your operating system in the tables below. They include information for VPN clients that are supported by the Office of Information Technology as well as clients that are not supported but known to work. 
Windows Operating System
Client
Version
Downloads and/or Instructions
AnyConnect for Windows (Recommended)
This client supports 32-bit and 64-bit processors.
4.9.04043
	•	Download.
	•	Install.
	•	Connect.
	•	See Advanced Instructions.


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
	•	Portable OpenSSH is available at http://www.openssh.com/portable.html at no charge. The portable OpenSSH follows development of the official version, but releases are not synchronized. Portable releases are marked with a 'p' (e.g. 2.3.0p1).
Connecting to MSI with Windows OS

If you are using the most recent version of Windows 10, you can perform basic ssh connections in your Command Prompt application. In this case, you can follow the OSX and Linux instructions below. 
For older versions of Windows, and for running graphical interfaces over SSH, see our PuTTy setup guide. (https://www.msi.umn.edu/support/faq/how-do-i-configure-putty-connect-msi-unix-systems)

	•	Download and install PuTTY.n (https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)  
	•	Double-click on the PuTTY icon and a "PuTTY Configuration" window will pop up (as shown in the image below). In the "Host Name (or IP address)" box, enter: login.msi.umn.edu. 
	


***Note: Login nodes will be retired on February 3, 2021

Dear MSI User,
At the next maintenance day for MSI systems (Wednesday, February 3rd, 2021), the servers named login.msi.umn.edu will be retired from service. The major effect of this retirement will be a quicker login experience for many MSI computational users.

For those MSI users who are used to connecting to login.msi.umn.edu, and then connecting to their desired cluster, you will now be directly connecting to the cluster (mesabi or mangi) from your desktop or laptop by connecting to mesabi.msi.umn.edu, mangi.msi.umn.edu, etc. 

If you forget to connect directly to your desired HPC resource (mesabi, mangi, etc) after the February 3rd Maintenance Day is complete, no worries, your attempt to connect to login.msi.umn.edu will instead be redirected to mangi.msi.umn.edu. You will also see a warning message regarding differing keys on mangi than on the retired login server. You should verify that this key matches the key(s) published on MSI's website at https://www.msi.umn.edu/content/ssh-keys before establishing the connection.

You may try connecting to mesabi.msi.umn.edu or mangi.msi.umn.edu from your own computer right now if you wish. During this early access phase, please let us know if you cannot connect, or notice any odd behavior of the system that impacts your work.
	•	Double-click on the PuTTY icon and a "PuTTY Configuration" window will pop up (as shown in the image below). In the "Host Name (or IP address)" box, enter: mang.msi.umn.edu, or mesabi.msi.umn.edu depending on if you want to log into the interactive node or not 
	


	•	In the left panel, under Window, select "Translation". Ensure the "Remote character set" is UTF-8

 
	•	[This step is optional. It is required only if you wish to use programs with a graphical interface. Under the "SSH" category, select "X11". Check the box next to "Enable X11 forwarding". (Make certain you have a local X client for Windows such as Xming configured, or this step will have no effect and X programs will not be able to open the display.)


	•	Return to "Session" in the left panel and in the "Saved Sessions" box, type in the name you would like to use to refer to this session, then click "Save".

 
	•	The next time you start PuTTY you will be able to recall a saved profile by clicking on the name and then clicking on "Load".   Click on "Open" to start the connection.


	•	The first time you connect to a server you may be asked to cache the server fingerprint. This is a safety feature of ssh. Go ahead and click "Yes" for now. You may be asked twice.

 
	•	You will be asked for your password. You can avoid entering it again when connecting to another server when using ssh keys; please see the ssh keys page for instructions if interested.
	•	PuTTY is now configured to connect to MSI's login node. From here, you can ssh to MSI's HPC clusters.

Connecting to MSI with Linux OS (Changed after January 2021)

Terminal is a preinstalled application which can be used to connect to MSI systems. 

Connecting to Login Host 
Once you open the Terminal App or PuTTy type:
	•	ssh yourMSIusername@login.msi.umn.edu
	•	After January 2021, type ssh yourMSIusername@mangi.msi.umn.edu or ssh yourMSIusername@mesabi.msi.umn.edu
	•	You will be prompted for your MSI password type it in then press enter. The terminal will not display your password as you type. 
	•	The first time you log into MSI systems you will be asked if you would like to cache the server fingerprint, type yes and press return.
	•	
	•	Note: You should be connected to the eduroam network of UMN VPN for this command to be successful.

	•	You are now connected to the MSI Login host (or bastion host). This host will let you view the directories but you can not submit jobs or run interactive computation on this host. 
Connecting to Mangi and Mesabi from Login Host (Changed after January 2021)
	•	Once connected to the Login host, note the word login in your terminal prompt, you can connect to Mangi or Mesabi via ssh.
	•	ssh mangi or ssh mesabi
	•	After January  2021, just use ssh yourMSIusername@mangi.msi.umn.edu or ssh yourMSIusername@mesabi.msi.umn.edu in terminal, instead of ssh yourMSIusername@login.msi.umn.edu as the first step to log in
	•	MSI prohibits moving directly from one system to another. If you are connected to Mangi and would like to connect to Mesabi you will first have to exit back to the Login Host, then ssh to the new system. Commands are highlighted with red boxes in the example below. 

	•	Each time you move between systems you will see the welcome message associated with the system you are connecting to. These messages often contain useful information and should be read. 
	•	NOTE: Like the login node, Mangi and Mesabi have specific nomenclatures in the prompt. When connected to Mangi your prompt will contain cn followed by some number when connected to Mesabi your prompt will contain ln followed by some numbers. This is a quick short cut to remind you which system you are connected to at any time. 
Connecting to MSI with OSX
Terminal is a preinstalled application which can be used to connect to MSI systems. Terminal can be found in the Utilities folder found in the Applications folder. 

Terminal is a preinstalled application which can be used to connect to MSI systems. 

For OSX and Linux OS Terminal Apps
Once you open the Terminal App type:
	•	ssh yourMSIusername@login.msi.umn.edu
	•	After January 2021, type ssh yourMSIusername@mangi.msi.umn.edu or ssh yourMSIusername@mesabi.msi.umn.edu
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

To find out what module versions are available, consult the software page or use the module avail command to search by name. For example to find out what versions of Python are available:

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
How do I transfer data between a Mac or Windows computer and MSI Unix computers?

(https://www.msi.umn.edu/support/faq/how-do-i-transfer-data-between-mac-or-windows-computer-and-msi-unix-computers#:~:text=For%20transferring%20data%20from%20Mac,For%20Windows%2C%20we%20recommend%20WinSCP.)
For transferring data from Mac / Windows computers to MSI Unix computers, any SFTP or SCP software can be used. Connect to login.msi.umn.edu (How is this affected by the login node change after January 2021??) using your MSI username and password and you will be able to transfer files to and from your home directory and group shared directory. For Windows, we recommend WinSCP. For Mac OS X, we recommend using FileZilla.

https://www.msi.umn.edu/support/faq/how-do-i-use-filezilla-transfer-data

File transfer from Mac clients connecting to MSI Unix servers can be done with many different clients. MSI recommends FileZilla as a reliable utility.

Filezilla
	•	Start the FileZilla application to get this screen:
 

 
	•	Enter sftp://login.msi.umn.edu into the Host box, your username into the Username box, your password into the Password box, and 22 into the Port box. Click on Quickconnect. (Tried using sftp://mesabi.msi.umn.edu, username and password since login node changes took place in Jaunary 2021)
 

 
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

If you are on the Unix side and need to pull data from your Windows U: drive, you can use smbclient:

$ smbclient -U 'MSI\username' -D username //falcon2/Users

From the prompt:

smb: \username\ 

You can issue ftp style commands ('ls', 'get', 'cd', etc.). To get an entire directory recursively, use:

smb: \username\ prompt
smb: \username\ recurse
smb: \username\ mget DirectoryName
WinSCP
(https://www.msi.umn.edu/support/faq/how-do-i-transfer-data-between-mac-or-windows-computer-and-msi-unix-computers)
To upload files from your Windows machine to your home directory on a Unix machine, use WinSCP.

	•	These directions explain how to transfer files back and forth between a Windows client and a Unix server. Please see the directions for remote access for an MSI Unix machine from a Windows client if you need to log in interactively.
	•	Download and install WinSCP, a graphical SCP (secure copy protocol) file transfer client for Windows. These directions are based on version 5.7.5; upgrade if needed.
	•	Double-click the WinSCP icon to reach this screen. If you just want to transfer files to or from your group home or lab scratch, you can skip steps 4 and 5 and proceed to step 6.

	•	(Optional: if you want to transfer files elsewhere other than login.msi) Click the "Advanced Options" box in the lower left-hand corner:

	•	(Optional: if you want to transfer files elsewhere other than login.msi) Under the Connection section, click on "Tunnel" to get to this screen and click "Connect through SSH tunnel". Fill in login.msi.umn.edu and your MSI username. Then click back in the left column on Session at the top. In preparation for changes after January 2021,I have tested using mesabi.msi.umn.edu as an alternate login and was able to see my folder)

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
	•	When you are ready to disconnect from the remote system, press F10 or just exit from the WinSCP program.

***Note
I saved the microbiome analysis files under the following paths

C:\Users\onyea005\Documents\mice_tutorial
C:\Users\onyea005\Documents\qiime_tutorial

HHRI important information
To get access to the ASMIC files the path of WinSCP is : 
/panfs/roc/groups/13/prizm001/onyea005
To get access to the HHRI files the path of WinSCP is : 
/panfs/roc/groups/5/isrania

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


MSI Analysis Review
Login in on MSI
First Things First: Open the Terminal
1. 	Download Putty on Windows (http://www.putty.org/) and configure it for MSI access (This step has changed to be mangi.msi.umn.edu or mesabi.msi.umn.edu after January 2021)

	•	Connecting to Mesabi and Itasca from Login Host. Once connected to the Login host, note the word login in your terminal prompt, you can connect to Mesabi or Itasca via ssh (QIIME is currently loaded under Itasca so use ssh Itasca for QIIME analyses)
ssh mesabi or ssh Itasca (no longer needed using a PUTTY, windows, see the step above)
	•	Alternatively, connect to MSI using Terminal on Mac/Linux, located in ApplicationsUtilitiesTerminal.app. 
*NOTE: Like the login node Mesabi and Itasca have specific nomenclatures in the prompt. When connected to Mesabi your prompt will contain ln followed by some number when connected to Itasca your prompt will contain node followed by some numbers. This is a quick short cut to remind you which system you are connected to at any time.
ssh yourusername@login.msi.umn.edu (use ssh yourusername@mangi.msi.umn.edu or ssh yourusername@mesabi.msi.umn.edu respectively)
ssh onyea005@mesabi.msi.umn.edu
	•	Check the folder you are in
pwd
* my home folder is when I first login is “/home/prizm001/onyea005”
*Change the folder to the israni MSI group
cd /panfs/roc/groups/5/isrania/onyea005
	•	Copy the MACQIIME tutorial file to your MSI folder
To copy a folder path, right-click on the file name, then click on option to be able to “copy as pathname”
The qiime tutorial folder is located at “/Volumes/RAVPOWER/ROOK_PC/MAC_Documents/qiime_tutorial”
I copied it to “cd /panfs/roc/groups/5/isrania/onyea005” using the FileZilla interface
Job Walltime (walltime=)
The job walltime is the time from the start to the finish of a job (as you would measure it using a clock on a wall), not including time spent waiting to run. This is in contrast to cputime, which measures the cumulative time all cores spent working on a job. Different job queues have different walltime limits, and it is important to choose a queue with a sufficiently high walltime that enables your job to complete.  Jobs which exceed the requested walltime are killed by the system to make room for other jobs.  Walltime limits are maximums only, and you can always request a shorter walltime, which will reduce the amount of time you wait in the queue for your job to start. If you are unsure how much walltime your job will need start with the queues with shorter walltime limits and only move to others if needed. 

Job Nodes and Cores (nodes=X:ppn=Y)
Many calculations have the ability to use multiple cores (ppn), or (less often) multiple nodes, to improve calculation speed.  Certain job queues have maximum or minimum values for the number  nodes and cores a job may use.  If Node Sharing is enabled for a queue you can request fewer cores (ppn) than exist on an entire node.  If Node Sharing is not enabled then you must request resources equivalent to a multiple of an entire node.  All Itasca queues, and Mesabi’s widest and large queues, do not allow Node Sharing.

Job Memory (mem=)
The memory which a job requires is an important factor when choosing a queue. The largest amount of memory (RAM) that can be requested for a job is limited by the memory on the hardware associated with that queue.  Mesabi has two queues (ram256g and ram1t) with high memory hardware, the largest memory hardware is available through the ram1t queue.  Itasca also has two queues with high memory hardware (sb128 and sb256).

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



Part 1: Testing out the OTU Network map to visualize your microbiome data

http://qiime.org/scripts/make_otu_network.html

Getting MacQIIME to work in El Capitan (OS 10.11)
This section is required only if you are running OS 10.11 (El Capitan) or higher. Apple added a new security feature in El Capitan that makes it impossible to install software into /usr/bin/ folder (that was where I was putting the “macqiime” sourcing script.  I'll fix this in the next release of MacQIIME. In the meantime, we can work around it. The quick solution is to use this command, instead of the “macqiime” script:
source /macqiime/configs/bash_profile.txt
That should give you a *working* terminal with MacQIIME enabled and everything should work.
Since the files are getting to be large, I am moving the qiime2 file to my external hard drive to prevent size issues

https://discussions.apple.com/thread/3871334
cd /Volumes/ROOK_PC/macqiime_test
cd /Volumes/
ls -lh
**You can’t use ROOK_MAC or ROOK_BACKUP directly because you cannot write to the volumes (it may have to do with their formatting)
cd /Volumes/ROOK_PC/MAC_Documents/macqiime_test
make_otu_network.py -i otu_table.biom -m Aspirin_Oral_Map_3.txt -o otu_network
To format the mapping file correctly, check “Part 18_ASMIC GUT_MSI_MACQIIME_GUERRERO_NEGRO_PHYLOSEQ_MICROBIOME_LME” for directions
http://qiime.org/documentation/file_formats.html#metadata-mapping-files
I needed to change the “_SampleID” to “#SampleID”. Amd the last column should be the Description column

Move the updated mapping file back to the test folder

make_otu_network.py -i otu_table.biom -m aspirin_oral_map_macqiime.txt -o otu_network

Visualizing the OTU network in cytoscape

http://qiime.org/tutorials/making_cytoscape_networks.html

https://cytoscape.org/download.html

**There was some wonkiness with cytoscape itself, but the macqiime part seemed to have worked without any issues after I fixed the mapping file.

Part 2: Making sure I can use R from the remote volumes

Now I am going to use a recent ASMIC boxplot R script to test

When running future analyses on MAC, in order to use the external hard drive and save on memory space, here is what you can do

On MAC

Change the root of the path to (/Users/guillaumeonyeaghala/Documents/Dissertation/3_Dissertation Analysis/1_Dissertation Processed Datasets/1_asmic_oral_mapping/)

On external hard drive

Change the root of the path to (/Volumes/ROOK_PC/MAC_Documents/Dissertation/3_Dissertation Analysis/1_Dissertation Processed Datasets/1_asmic_oral_mapping/)

***One last thing to remember, is that the python environments (picrust, macqiime and qiime2) take up space on your mac hard drive (~15GBs) so that is what is eating your memory space even if you can’t find them in documents.


HHRI important information

To get access to the ASMIC files the path of WinSCP is : 
/panfs/roc/groups/13/prizm001/onyea005

To get access to the HHRI files the path of WinSCP is : 
/panfs/roc/groups/5/isrania

The Plink tutorial files are located at 
/home/ isrania/shared/DeKAF-GWAS/plink/

Plink Analysis
https://zzz.bwh.harvard.edu/plink/data.shtml
https://zzz.bwh.harvard.edu/plink/tutorial.shtml
Connecting to MSI to use plink
	•	Mac OS X
	•	Mac OS X comes with OpenSSH pre-installed. Open the Terminal application from inside the Utilities folder, and then type "ssh user@server_name_or_IP_address" to get started.
	•	Portable OpenSSH is available at http://www.openssh.com/portable.html at no charge. The portable OpenSSH follows development of the official version, but releases are not synchronized. Portable releases are marked with a 'p' (e.g. 2.3.0p1).
Connecting to MSI with OSX
Terminal is a preinstalled application which can be used to connect to MSI systems. Terminal can be found in the Utilities folder found in the Applications folder. 

Terminal is a preinstalled application which can be used to connect to MSI systems. 

For OSX and Linux OS Terminal Apps
Once you open the Terminal App type:
	•	ssh yourMSIusername@login.msi.umn.edu
	•	After January 2021, type ssh yourMSIusername@mangi.msi.umn.edu or ssh yourMSIusername@mesabi.msi.umn.edu
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
Login in on MSI
First Things First: Open the Terminal
1. 	Download Putty on Windows (http://www.putty.org/) and configure it for MSI access (This step has changed to be mangi.msi.umn.edu or mesabi.msi.umn.edu after January 2021)

	•	Connecting to Mesabi and Itasca from Login Host. Once connected to the Login host, note the word login in your terminal prompt, you can connect to Mesabi or Itasca via ssh (QIIME is currently loaded under Itasca so use ssh Itasca for QIIME analyses)
ssh mesabi or ssh Itasca (no longer needed using a PUTTY, windows, see the step above)
	•	Alternatively, connect to MSI using Terminal on Mac/Linux, located in ApplicationsUtilitiesTerminal.app. 
*NOTE: Like the login node Mesabi and Itasca have specific nomenclatures in the prompt. When connected to Mesabi your prompt will contain ln followed by some number when connected to Itasca your prompt will contain node followed by some numbers. This is a quick short cut to remind you which system you are connected to at any time.
**You need to be connect. To Anyconnect Va VPN in order for this command to work.
ssh yourusername@login.msi.umn.edu (use ssh yourusername@mangi.msi.umn.edu or ssh yourusername@mesabi.msi.umn.edu respectively)
ssh onyea005@mesabi.msi.umn.edu
	•	Check the folder you are in
pwd
* my home folder is when I first login is “/home/prizm001/onyea005”
*Change the folder to the israni MSI group
cd /panfs/roc/groups/5/isrania/onyea005
	•	Load the plink software
module avail plink
module load plink
plink
A PLINK tutorial
In this tutorial, we will consider using PLINK to analyse example data: randomly selected genotypes (approximately 80,000 autosomal SNPs) from the 89 Asian HapMap individuals. A phenotype has been simulated based on the genotype at one SNP. In this tutorial, we will walk through using PLINK to work with the data, using a range of features: data management, summary statistics, population stratification and basic association analysis.
NOTE These data do not, of course, represent a realistic study design or a realistic disease model. The point of this exercise is simply to get used to running PLINK.
89 HapMap samples and 80K random SNPs
The first step is to obtain a working copy of PLINK and of the example data files.
	•	Make sure you have PLINK installed on your machine (see these instructions).
	•	Download the example data archive file which contains the genotypes, map files and two extra phenotype files, described below (zipped, approximately 2.8M)
	•	Create a new folder/directory on your machine, and unzip the file you downloaded (called hapmap1.zip) into this folder.
***Copy the hapmap1 file to the MSI folder
**To copy a folder path, right-click on the file name, then click on option to be able to “copy as pathname”
I copied it to “cd /panfs/roc/groups/5/isrania/onyea005” using the FileZilla interface
HINT! If you are a Windows user who is unsure how to do this, follow this link

Two phenotypes were generated: a quantitative triat and a disease trait (affection status, coded 1=unaffected, 2=affected), based on a median split of the quantitative trait. The quantitative trait was generated as a function of three simple components:
	•	A random component
	•	Chinese versus Japanese identity
	•	A variant on chromosome 2, rs2222162
Remember, this model is not intended to be realistic. The following contingency table shows the joint distribution of disease and subpopulation:

          Chinese   Japanese
Control      34        11
Case         11        33

which shows a strong relationship between these two variables. The next table shows the association between the variant rs2222162 and disease:

             Genotype
             11      12      22
Control      17      22      6
Case         3       19      22

Again, the strong association is clear. Note that the alleles have been recoded as 1 and 2 (this is not necessary for PLINK to work, however -- it can accept any coding for SNPs).

In summary, we have a single causal variant that is associated with disease. Complicating factors are that this variant is one of 83534 SNPs and also that there might be some degree of confounding of the SNP-disease associations due to the subpopulation-disease association -- i.e. a possibility that population stratification effects will exist. Even though we might expect the two subpopulations to be fairly similar from an overall genetic perspective, and even though the sample size is small, this might still lead to an increase in false positive rates if not controlled for.

We will use the affection status variable as the default variable for analysis (i.e. the sixth column in the PED file). The quantitative trait is in a separate alternate phenotype file, qt.phe. The file pop.phe contains a dummy phenotype that is coded 1 for Chinese individuals and 2 for Japanese individuals. We will use this in investigating between-population differences. You can view these alternate phenotype files in any text editor.

In this tutorial dataset we focus on autosomal SNPs for simplicity, although PLINK does provide support for X and Y chromosome SNPs for a number of analyses. See the main documentation for further information.
Using Plink to analyse these data
This tutorial is intended to introduce some of PLINK's features rather than provide exhaustive coverage of them. Futhermore, it is not intended as an analysis plan for whole genome data, or to represent anything close to 'best practice'.
These hyperlinks show an overview of topics:
	•	Getting started
	•	Making a binary PED file
	•	Working with the binary PED file
	•	Summary statistics: missing rates
	•	Summary statistics: allele frequencies
	•	Basic association analysis
	•	Genotypic association models
	•	Stratification analysis
	•	Association analysis, accounting for clusters
	•	Quantitative trait association analysis
	•	Extracting a SNP of interest
 
Getting started

Just typing plink and specifying a file with no further options is a good way to check that the file is intact, and to get some basic summary statistics about the file.

*move into the plink analysis file
cd /panfs/roc/groups/5/isrania/onyea005/hapmap1
ls
plink --file hapmap1

The --file option takes a single parameter, the root of the input file names, and will look for two files: a PED file and a MAP file with this root name. In otherwords, --file hapmap1 implies hapmap1.ped and hapmap1.map should exist in the current directory.
HINT! It is possible to specify files outside of the current directory, and to have the PED and MAP files have different root names, or not end in .ped and .map, by using the --ped and --map options.
PED and MAP files are plain text files; PED files contain genotype information (one person per row) and MAP files contain information on the name and position of the markers in the PED file. If you are not familiar with the file formats required for PED and MAP files, please consult this page.
The above command should generate something like the following output in the console window. It will also save this information to a file called plink.log.
     @----------------------------------------------------------@
     |         PLINK!       |    v0.99l     |   27/Jul/2006     |
     |----------------------------------------------------------|
     |  (C) 2006 Shaun Purcell, GNU General Public License, v2  |
     |----------------------------------------------------------|
     |       http://pngu.mgh.harvard.edu/purcell/plink/         |
     @----------------------------------------------------------@

     Web-based version check ( --noweb to skip )
     Connecting to web...  OK, v0.99l is current

     *** Pre-Release Testing Version ***

     Writing this text to log file [ plink.log ]
     Analysis started: Mon Jul 31 09:00:11 2006

     Options in effect:
        --file hapmap1

     83534 (of 83534) markers to be included from [ hapmap1.map ]
     89 individuals read from [ hapmap1.ped ]
     89 individuals with nonmissing phenotypes
     Assuming a binary trait (1=unaff, 2=aff, 0=miss)
     Missing phenotype value is also -9
     Before frequency and genotyping pruning, there are 83534 SNPs
     Applying filters (SNP-major mode)
     89 founders and 0 non-founders found
     0 of 89 individuals removed for low genotyping ( MIND > 0.1 )
     859 SNPs failed missingness test ( GENO > 0.1 )
     16994 SNPs failed frequency test ( MAF < 0.01 )
     After frequency and genotyping pruning, there are 65803 SNPs
     Analysis finished: Mon Jul 31 09:00:19 2006

The information contained here can be summarized as follows:
	•	A banner showing copyright information and the version number -- the web-based version check shows that this is an up-to-date version of PLINK and displays a message that v0.99l is a pre-release testing version.
	•	A message indicating that the log file will be saved in plink.log. The name of the output file can be changed with the --out option -- e.g. specifying --out anal1 will generate a log file called anal1.log instead.
	•	A list of the command options specified is given next: in this case it is only a single option, --file hapmap1. By keeping track of log files, and naming each analysis with its own --out name, it makes it easier to keep track of when and how the different output files were generated.
	•	Next is some information on the number of markers and individuals read from the MAP and PED file. In total, just over 80,000 SNPs were read in from the MAP file. It is written "...83534 (of 83534)..." because some SNPs might be excluded (by making the physical position a negative number in the MAP file), in which case the first number would indicate how many SNPs are included. In this case, all SNPs are read in from the PED file. We also see that 89 individuals were read in from the PED file, and that all these individuals had valid phenotype information.
	•	Next, PLINK tells us that the phenotype is an affection status variable, as opposed to a quantitative trait, and lets us know what the missing values are.
	•	The next stage is the filtering stage -- individuals and/or SNPs are removed on the basis of thresholds. Please see this page for more information on setting thresholds. In this case we see that no individuals were removed, but almost 20,000 SNPs were removed, based on missingness (859) and frequency (16994). This particularly high proportion of removed SNPs is based on the fact that these are random HapMap SNPs in the Chinese and Japanese samples, rather than pre-selected markers on a whole-genome association product: there will be many more rare and monomorphic markers here than one would normally expect.
	•	Finally, a line is given that indicates when this analysis finished. You can see that it took 8 seconds (on my machine at least) to read in the file and apply the filters.
If other analyses had been requested, then the other output files generated would have been indicated in the log file. All output files that PLINK generates have the same format: root.extension where root is, by default, "plink" but can be changed with the --out option, and the extension will depend on the type of output file it is (a complete list of extensions is given here).

Making a binary PED file
The first thing we will do is to make a binary PED file. This more compact representation of the data saves space and speeds up subsequent analysis. To make a binary PED file, use the following command.

plink --file hapmap1 --make-bed --out hapmap1

If it runs correctly on your machine, you should see the following in your output:
     above as before
     ...
     Before frequency and genotyping pruning, there are 83534 SNPs
     Applying filters (SNP-major mode)
     89 founders and 0 non-founders found
     0 SNPs failed missingness test ( GENO > 1 )
     0 SNPs failed frequency test ( MAF < 0 )
     After frequency and genotyping pruning, there are 83534 SNPs
     Writing pedigree information to [ hapmap1.fam ]
     Writing map (extended format) information to [ hapmap1.bim ]
     Writing genotype bitfile to [ hapmap1.bed ]
     Using (default) SNP-major mode
     Analysis finished: Mon Jul 31 09:10:05 2006

There are several things to note:
	•	When using the --make-bed option, the threshold filters for missing rates and allele frequency were automatically set to exclude nobody. Although these filters can be specified manually (using --mind, --geno and --maf) to exclude people, this default tends to be wanted when creating a new PED or binary PED file. The commands --extract / --exclude and --keep / --remove can also be applied at this stage.
	•	Three files are created with this command -- the binary file that contains the raw genotype data hapmap1.bed but also a revsied map file hapmap1.bim which contains two extra columns that give the allele names for each SNP, and hapmap1.fam which is just the first six columns of hapmap1.ped. You can view the .bim and .fam files -- but do not try to view the .bed file. None of these three files should be manually editted.
If, for example, you wanted to create a new file that only includes individuals with high genotyping (at least 95% complete), you would run:

plink --file hapmap1 --make-bed --mind 0.05 --out highgeno

which would create files
     highgeno.bed
     highgeno.bim
     highgeno.fam

Working with the binary PED file
To specify that the input data are in binary format, as opposed to the normal text PED/MAP format, just use the --bfile option instead of --file. To repeat the first command we ran (which just loads the data and prints some basic summary statistics):

plink --bfile hapmap1 (*** did not work)
plink --bfile hapmap1 --out hapmap1bin (*** did not work)

     Writing this text to log file [ plink.log ]
     Analysis started: Mon Jul 31 09:12:08 2006

     Options in effect:
             --bfile hapmap1

     Reading map (extended format) from [ hapmap1.bim ]
     83534 markers to be included from [ hapmap1.bim ]
     Reading pedigree information from [ hapmap1.fam ]
     89 individuals read from [ hapmap1.fam ]
     89 individuals with nonmissing phenotypes
     Reading genotype bitfile from [ hapmap1.bed ]
     Detected that binary PED file is v1.00 SNP-major mode
     Before frequency and genotyping pruning, there are 83534 SNPs
     Applying filters (SNP-major mode)
     89 founders and 0 non-founders found
     0 of 89 individuals removed for low genotyping ( MIND > 0.1 )
     859 SNPs failed missingness test ( GENO > 0.1 )
     16994 SNPs failed frequency test ( MAF < 0.01 )
     After frequency and genotyping pruning, there are 65803 SNPs
     Analysis finished: Mon Jul 31 09:12:10 2006

The things to note here:
	•	That three files hapmap1.bim, hapmap1.fam and hapmap1.bed were loaded instead of the usual two files. That is, hapmap1.ped and hapmap1.map are not used in this analysis, and could in fact be deleted now.
	•	The data are loaded in much more quickly -- based on the timestamp at the beginning and end of the log output, this took 2 seconds instead of 10.
Summary statistics: missing rates
Next, we shall generate some simple summary statistics on rates of missing data in the file, using the --missing option:

plink --bfile hapmap1 --missing --out miss_stat

which should generate the following output:
     ...
     0 of 89 individuals removed for low genotyping ( MIND > 0.1 )
     Writing individual missingness information to [ miss_stat.imiss ]
     Writing locus missingness information to [ miss_stat.lmiss ]
     ...
Here we see that no individuals were removed for low genotypes (MIND > 0.1 implies that we accept people with less than 10 percent missingness).
The per individual and per SNP (after excluding individuals on the basis of low genotyping) rates are then output to the files miss_stat.imiss and miss_stat.lmiss respectively. If we had not specified an --out option, the root output filename would have defaulted to "plink".
These output files are standard, plain text files that can be viewed in any text editor, pager, spreadsheet or statistics package (albeit one that can handle large files). Taking a look at the file miss_stat.lmiss, for example using the more command which is present on most systems:

more miss_stat.lmiss

we see
      CHR          SNP   N_MISS     F_MISS
        1    rs6681049        0          0
        1    rs4074137        0          0
        1    rs7540009        0          0
        1    rs1891905        0          0
        1    rs9729550        0          0
        1    rs3813196        0          0
        1    rs6704013        2  0.0224719
        1     rs307347       12   0.134831
        1    rs9439440        2  0.0224719
      ...
That is, for each SNP, we see the number of missing individuals (N_MISS) and the proportion of individuals missing (F_MISS). Similarly:
more miss_stat.imiss

we see
         FID          IID MISS_PHENO     N_MISS     F_MISS
      HCB181            1          N        671 0.00803266
      HCB182            1          N       1156  0.0138387
      HCB183            1          N        498 0.00596164
      HCB184            1          N        412 0.00493212
      HCB185            1          N        329 0.00393852
      HCB186            1          N       1233  0.0147605
      HCB187            1          N        258 0.00308856
      ...

The final column is the actual genotyping rate for that individual -- we see the genotyping rate is very high here.
HINT If you are using a spreadsheet package that can only display a limited number of rows (some popular packages can handle just over 65,000 rows) then it might be desirable to ask PLINK to analyse the data by chromosome, using the --chr option. For example, to perform the above analysis for 
chromosome 1:

plink --bfile hapmap1 --chr 1 --out res1 --missing

then for chromosome 2:

plink --bfile hapmap1 --chr 2 --out res2 --missing

and so on.

Summary statistics: allele frequencies

Next we perform a similar analysis, except requesting allele frequencies instead of genotyping rates. The following command generates a file called freq_stat.frq which contains the minor allele frequency and allele codes for each SNP.

plink --bfile hapmap1 --freq --out freq_stat

It is also possible to perform this frequency analysis (and the missingness analysis) stratified by a categorical, cluster variable. In this case, we shall use the file that indicates whether the individual is from the Chinese or the Japanese sample, pop.phe. This cluster file contains three columns; each row is an individual. The format is described more fully in the main documentation.
To perform a stratified analysis, use the --within option.

plink --bfile hapmap1 --freq --within pop.phe --out freq_stat

The output will now indicate that a file called freq_stat.frq.strat. has been generated instead of freq_stat.frq. If we view this file:

more freq_stat.frq.strat

we see each row is now the allele frequency for each SNP stratifed by subpopulation:
     CHR          SNP     CLST   A1   A2          MAF
       1    rs6681049        1    1    2     0.233333
       1    rs6681049        2    1    2     0.193182
       1    rs4074137        1    1    2          0.1
       1    rs4074137        2    1    2    0.0568182
       1    rs7540009        1    0    2            0
       1    rs7540009        2    0    2            0
       1    rs1891905        1    1    2     0.411111
       1    rs1891905        2    1    2     0.397727
       ...
Here we see that each SNP is represented twice - the CLST column indicates whether the frequency is from the Chinese or Japanese populations, coded as per the pop.phe file.

If you were just interested in a specific SNP, and wanted to know what the frequency was in the two populations, you can use the --snp option to select this SNP:

plink --bfile hapmap1 --snp rs1891905 --freq --within pop.phe --out snp1_frq_stat

would generate a file snp1_frq_stat.frq.strat containing only the population-specific frequencies for this single SNP. You can also specify a range of SNPs by adding the --window kb option or using the options --from and --to, following each with a different SNP (they must be in the correct order and be on the same chromosome). Follow this link for more details.

Basic association analysis
Let's now perform a basic association analysis on the disease trait for all single SNPs. The basic command is

plink --bfile hapmap1 --assoc --out as1

more as1.assoc

which generates an output file as1.assoc which contains the following fields

      CHR          SNP   A1      F_A      F_U   A2        CHISQ            P           OR
        1    rs6681049    1   0.1591   0.2667    2        3.067      0.07991       0.5203
        1    rs4074137    1  0.07955  0.07778    2     0.001919       0.9651        1.025
        1    rs1891905    1   0.4091      0.4    2      0.01527       0.9017        1.038
        1    rs9729550    1   0.1705  0.08889    2        2.631       0.1048        2.106
        1    rs3813196    1  0.03409  0.02222    2       0.2296       0.6318        1.553
        1   rs12044597    1      0.5   0.4889    2      0.02198       0.8822        1.045
        1   rs10907185    1   0.3068   0.2667    2       0.3509       0.5536        1.217
        1   rs11260616    1   0.2326      0.2    2       0.2754       0.5998        1.212
        1     rs745910    1   0.1395   0.1932    2       0.9013       0.3424       0.6773
     ...
where each row is a single SNP association result. The fields are:
	•	Chromosome
	•	SNP identifier
	•	Code for allele 1 (the minor, rare allele based on the entire sample frequencies)
	•	The frequency of this variant in cases
	•	The frequency of this variant in controls
	•	Code for the other allele
	•	The chi-squared statistic for this test (1 df)
	•	The asymptotic significance value for this test
	•	The odds ratio for this test
If a test is not defined (for example, if the variant is monomorphic but was not excluded by the filters) then values of NA for not applicable will be given (as these are read by the package R to indicate missing data, which is convenient if using R to analyse the set of results).

In a Unix/Linux environment, one could simply use the available command line tools to sort the list of association statistics and print out the top ten, for example:

sort --key=7 -nr as1.assoc | head

**Note that Key = 7 indicates which column you are sorting on, column 7 being the Chi-square value, and the head option prints the first 10 observations of a file.

would give

       13    rs9585021    1    0.625   0.2841    2        20.62    5.586e-06          4.2
        2    rs2222162    1   0.2841   0.6222    2        20.51    5.918e-06       0.2409
        9   rs10810856    1   0.2955  0.04444    2        20.01    7.723e-06        9.016
        2    rs4675607    1   0.1628   0.4778    2        19.93     8.05e-06       0.2125
        2    rs4673349    1   0.1818      0.5    2        19.83    8.485e-06       0.2222
        2    rs1375352    1   0.1818      0.5    2        19.83    8.485e-06       0.2222
       21     rs219746    1      0.5   0.1889    2        19.12    1.228e-05        4.294
        1    rs4078404    2      0.5      0.2    1        17.64    2.667e-05            4
       14    rs1152431    2   0.2727   0.5795    1        16.94    3.862e-05       0.2721
       14    rs4899962    2   0.3023   0.6111    1        16.88    3.983e-05       0.2758

Here we see that the simulated disease variant rs2222162 is actually the second most significant SNP in the list, with a large difference in allele frequencies of 0.28 in cases versus 0.62 in controls. However, we also see that, just by chance, a second SNP on chromosome 13 shows a slightly higher test result, with coincidentally similar allele frequencies in cases and controls. (Whether this result is due to chance alone or perhaps represents some confounding due to the population structure in this sample, we will investigate below). This highlights the important point that when performing so many tests, particularly in a small sample, we often expect the distribution of true positive results to be virtually indistinguishable from the best false positive results. That our variant appears in the top ten list is reassuring however.
To get a sorted list of association results, that also includes a range of significance values that are adjusted for multiple testing, use the --adjust flag:

plink --bfile hapmap1 --assoc --adjust --out as2

This generates the file as2.assoc.adjust in addition to the basic as2.assoc output file. Using more, one can easily look at one's most significant associations:

more as2.assoc.adjusted

 CHR          SNP      UNADJ         GC     BONF     HOLM  SIDAK_SS  SIDAK_SD    FDR_BH  FDR_BY
  13    rs9585021  5.586e-06  3.076e-05   0.3676   0.3676    0.3076    0.3076   0.09306       1
   2    rs2222162  5.918e-06  3.231e-05   0.3894   0.3894    0.3226    0.3226   0.09306       1
   9   rs10810856  7.723e-06  4.049e-05   0.5082   0.5082    0.3984    0.3984   0.09306       1
   2    rs4675607   8.05e-06  4.195e-05   0.5297   0.5297    0.4112    0.4112   0.09306       1
   2    rs1375352  8.485e-06  4.386e-05   0.5584   0.5583    0.4279    0.4278   0.09306       1
   2    rs4673349  8.485e-06  4.386e-05   0.5584   0.5583    0.4279    0.4278   0.09306       1
  21     rs219746  1.228e-05  6.003e-05   0.8083   0.8082    0.5544    0.5543    0.1155       1
   1    rs4078404  2.667e-05  0.0001159        1        1    0.8271     0.827    0.2194       1
  14    rs1152431  3.862e-05  0.0001588        1        1    0.9213    0.9212    0.2621       1
  14    rs4899962  3.983e-05   0.000163        1        1    0.9273    0.9272    0.2621       1
   8    rs2470048  4.487e-05  0.0001804        1        1    0.9478    0.9478    0.2684       1
Here we see a pre-sorted list of association results. The fields are as follows:
	•	Chromosome
	•	SNP identifier
	•	Unadjusted, asymptotic significance value
	•	Genomic control adjusted significance value. This is based on a simple estimation of the inflation factor based on median chi-square statistic. These values do not control for multiple testing therefore.
	•	Bonferroni adjusted significance value
	•	Holm step-down adjusted significance value
	•	Sidak single-step adjusted significance value
	•	Sidak step-down adjusted significance value
	•	Benjamini & Hochberg (1995) step-up FDR control
	•	Benjamini & Yekutieli (2001) step-up FDR control
In this particular case, we see that no single variant is significant at the 0.05 level after genome-wide correction. Different correction measures have different properties which are beyond the scope of this tutorial to discuss: it is up to the investigator to decide which to use and how to interpret them.
When the --adjust command is used, the log file records the inflation factor calculated for the genomic control analysis, and the mean chi-squared statistic (that should be 1 under the null):

     Genomic inflation factor (based on median chi-squared) is 1.18739
     Mean chi-squared statistic is 1.14813

These values would actually suggest that although no very strong stratification exists, there is perhaps a hint of an increased false positive rate, as both values are greater than 1.00.
HINT The adjusted significance values that control for multiple testing are, by default, based on the unadjusted significance values. If the flag --gc is specified as well as --adjust then these adjusted values will be based on the genomic-control significance value instead.

In this particular instance, where we already know about the Chinese/Japanese subpopulations, it might be of interest to directly look at the inflation factor that results from having population membership as the phenotype in a case/control analysis, just to provide extra information about the sample. That is, running the command using the alternate phenotype option (i.e. replacing the disease phenotype with the one in pop.phe, which is actually subpopulation membership):

plink --bfile hapmap1 --pheno pop.phe --assoc --adjust --out as3

we see that testing for frequency differences between Chinese and Japanese individuals, we do see some departure from the null distribution:

     Genomic inflation factor (based on median chi-squared) is 1.72519
     Mean chi-squared statistic is 1.58537

That is, the inflation factor of 1.7 represents the maximum possible inflation factor if the disease were perfectly correlated with subpopulation that could arise from the Chinese/Japanese split in the sample (this does not account for any possible within-subpopulation structure, of course, that might also increase SNP-disease false positive rates).

We will return to this issue below, when we consider using the whole genome data to detect stratification more directly.

Genotypic and other association models

We can calculate association statistics based on the 2-by-3 genotype table as well as the standard allelic test; we can also calculate tests that assume dominant or recessive action of the minor allele; finally, we can perform the Cochran-Armitage trend test instead of the basic allelic test. All these tests are performed with the single command --model. Just as the --assoc command, this can be easily applied to all SNPs. In this case, let's just run it for our SNP of interest, rs2222162

plink --bfile hapmap1 --model --snp rs2222162 --out mod1

This generates the file mod1.model which has more than one row per SNP, representing the different tests performed for each SNP. The format of this file is described here. The tests are the basic allelic test, the Cochran-Armitage trend test, dominant and recessive models and a genotypic test. All test statistics are distributed as chi-squared with 1 df under the null, with the exception of the genotypic test which has 2 df.

But there is a problem here: in this particular case, running the basic model command will not produce values for the genotypic tests. This is because, by default, every cell in the 2-by-3 table is required to have at least 5 observations, which does not hold here. This default can be changed with the --cell option. This option is followed by the minimum number of counts in each cell of the 2-by-3 table required before these extended analyses are performed. For example, to force the genotypic tests for this particular SNP just for illustrative purposes, we need to run:

plink --bfile hapmap1 --model --cell 0 --snp rs2222162 --out mod2

more mod2.model

and now the genotypic tests will also be calculated, as we set the minimum number in each cell to 0. We see that the genotype counts in affected and unaffected individuals are

     CHR         SNP     TEST        AFF      UNAFF    CHISQ   DF           P
       2   rs2222162     GENO    3/19/22    17/22/6    19.15    2   6.932e-05
       2   rs2222162    TREND      25/63      56/34    19.15    1   1.207e-05
       2   rs2222162  ALLELIC      25/63      56/34    20.51    1   5.918e-06
       2   rs2222162      DOM      22/22       39/6    13.87    1   0.0001958
       2   rs2222162      REC       3/41      17/28    12.24    1   0.0004679

which, reassuringly, match the values presented in the table above, which were generated when the trait was simulated. Looking at the other test statistics, we see all are highly significant (as would be expected for a strong, common, allelic effect) although the allelic test has the most significant p-value. This makes sense, as the data were essentially simulated under an allelic (dosage) model.

Stratification analysis

The analyses so far have ignored the fact that our sample consists of two similar, but distinct subpopulations, the Chinese and Japanese samples. In this particular case, we already know that the sample consists of these two groups; we also know that the disease is more prevalent in one of the groups. More generally, we might not know anything of potential population substructure in our sample upfront. One way to address such issues is to use whole genome data to cluster individuals into homogeneous groups. There are a number of options and different ways of performing this kind of analysis in PLINK and we will not cover them all here. For illustrative purposes, we shall perform a cluster analysis that pairs up individuals on the basis of genetic identity. The command, which may take a number of minutes to run, is:

plink --bfile hapmap1 --cluster --mc 2 --ppc 0.05 --out str1

which requests IBS clustering (--cluster) but with the constraints that each cluster has no more than two individuals (--mc 2) and that any pair of individuals who have a significance value of less than 0.05 for the test of whether or not the two individuals belong to the same population based on the available SNP data are not merged. These options and tests are described in further detail in the relevant main documentation.

We see the following output in the log file and on the console:
     ...
     Clustering individuals based on genome-wide IBS
     Merge distance p-value constraint = 0.05
     Of these, 3578 are pairable based on constraints
     Writing cluster progress to [ plink.cluster0 ]
     Cannot make clusters that satisfy constraints at step 45
     Writing cluster solution (1) [ str1.cluster1 ]
     Writing cluster solution (2) [ str1.cluster2 ]
     Writing cluster solution (3) [ str1.cluster3 ]
     ...
which indicate that IBS-clustering has been performed. These files are described in the main documentation. The file str1.cluster1 contains the results of clustering in a format that is easy to read:

more str1.cluster1

     SOL-0    HCB181_1 JPT260_1
     SOL-1    HCB182_1 HCB225_1
     SOL-2    HCB183_1 HCB193_1
     SOL-3    HCB184_1 HCB202_1
     SOL-4    HCB185_1 HCB217_1
     SOL-5    HCB186_1 HCB196_1
     SOL-6    HCB187_1 HCB213_1
     SOL-7    HCB188_1 HCB194_1
     SOL-8    HCB189_1 HCB192_1
     SOL-9    HCB190_1 HCB224_1
     SOL-10   HCB191_1 HCB220_1
     SOL-11   HCB195_1 HCB204_1
     SOL-12   HCB197_1 HCB211_1
     SOL-13   HCB198_1 HCB210_1
     SOL-14   HCB199_1 HCB221_1
     SOL-15   HCB200_1 HCB218_1
     SOL-16   HCB201_1 HCB206_1
     SOL-17   HCB203_1 HCB222_1
     SOL-18   HCB205_1 HCB208_1
     SOL-19   HCB207_1 HCB223_1
     SOL-20   HCB209_1 HCB219_1
     SOL-21   HCB212_1 HCB214_1
     SOL-22   HCB215_1 HCB216_1
     SOL-23   JPT226_1 JPT242_1
     SOL-24   JPT227_1 JPT240_1
     SOL-25   JPT228_1 JPT244_1
     SOL-26   JPT229_1 JPT243_1
     SOL-27   JPT230_1 JPT267_1
     SOL-28   JPT231_1 JPT236_1
     SOL-29   JPT232_1 JPT247_1
     SOL-30   JPT233_1 JPT248_1
     SOL-31   JPT234_1 JPT255_1
     SOL-32   JPT235_1 JPT264_1
     SOL-33   JPT237_1 JPT250_1
     SOL-34   JPT238_1 JPT241_1
     SOL-35   JPT239_1 JPT253_1
     SOL-36   JPT245_1 JPT262_1
     SOL-37   JPT246_1 JPT263_1
     SOL-38   JPT249_1 JPT258_1
     SOL-39   JPT251_1 JPT252_1
     SOL-40   JPT254_1 JPT261_1
     SOL-41   JPT256_1 JPT265_1
     SOL-42   JPT257_1
     SOL-43   JPT259_1 JPT269_1
     SOL-44   JPT266_1 JPT268_1

Here we see that all but one pair are concordant for being Chinese or Japanese (the IDs for each individual are in this case coded to represent which HapMap subpopulation they belong to: HCB and JPT. Note, these do not represent any official HapMap coding/ID schemes -- I've used them purely to make it clear which population each individual belongs to). We see that one individual was not paired with anybody else, as there is an odd numbered of subjects overall. This individual would not contribute to any subsequent association testing that conditions on this cluster solution. We also see that the Japanese individual JPT260_1 paired with a Chinese individual HCB181_1 rather than JPT257_1. Clearly, this means that HCB181_1 and JPT260_1 do not differ significantly based on the test we performed: this test will have limited power to distinguish individuals from very similar subpopulations, alternatively, one of these individuals could be of mixed ancestry. In any case, it is interesting that JPT260_1 was not paired with JPT257_1 instead. Further inspection of the data actually reveal that JPT257_1 is somewhat unusual, having very long stretches of homozygous genotypes (use the --homozyg-kb and --homozyg-snp options) are a high inbreeding coefficient, which probably explain why this individual was not considered similar to the other Japanese individuals by this algorithm.

Note By using the --genome option, it is possible to examine the significance tests for all pairs of individuals, as described in the main documentation.

Association analysis, accounting for clusters
After having performed the above matching based on genome-wide IBS, we can now perform the association test conditional on the matching. For this, the relevant file is the str1.cluster2 file, which contains the same information as str1.cluster1 but in the format of a cluster variable file, that can be used in conjunction with the --within option.

For this matched analysis, we shall use the Cochran-Mantel-Haenszel (CMH) association statistic, which tests for SNP-disease association conditional on the clustering supplied by the cluster file; we will also include the --adjust option to get a sorted list of CMH association results:

plink --bfile hapmap1 --mh --within str1.cluster2 --adjust --out aac1

The relevant lines from the log are:
     ...
     Reading clusters from [ str1.cluster2 ]
     89 of 89 individuals assigned to 45 cluster(s)
     ...
     Cochran-Mantel-Haenszel 2x2xK test, K = 45
     Writing results to [ aac1.cmh ]
     Computing corrected significance values (FDR, Sidak, etc)
     Genomic inflation factor (based on median chi-squared) is 1.03878
     Mean chi-squared statistic is 0.988748
     Writing multiple-test corrected significance values to [ aac1.cmh.adjusted ]
     ...
We see that PLINK has correctly assigned the individuals to 45 clusters (i.e. one of these clusters is of size 1, all others are pairs) and then performs the CMH test. The genomic control inflation factors are now reduced to essentially 1.00, which is consistent with the idea that there was some substructure inflating the distribution of test statistics in the previous analysis.

Looking at the adjusted results file:

more aac1.cmh.adjusted

 CHR        SNP      UNADJ         GC    BONF    HOLM  SIDAK_SS  SIDAK_SD   FDR_BH  FDR_BY
   2  rs2222162  1.906e-06  2.963e-06  0.1241  0.1241    0.1167    0.1167   0.1241       1
   2  rs4673349  6.436e-06  9.577e-06  0.4192  0.4191    0.3424    0.3424   0.1261       1
   2  rs1375352  6.436e-06  9.577e-06  0.4192  0.4191    0.3424    0.3424   0.1261       1
  13  rs9585021  7.744e-06  1.145e-05  0.5043  0.5043    0.3961    0.3961   0.1261       1
   2  rs4675607  5.699e-05  7.845e-05       1       1    0.9756    0.9756   0.7423       1
  13  rs9520572  0.0002386  0.0003122       1       1         1         1   0.9017       1
  11   rs992564    0.00026  0.0003392       1       1         1         1   0.9017       1
  12  rs1290910  0.0002738  0.0003566       1       1         1         1   0.9017       1
   4  rs1380899  0.0002747  0.0003577       1       1         1         1   0.9017       1
  14  rs1190968  0.0002747  0.0003577       1       1         1         1   0.9017       1
   8   rs951702  0.0003216  0.0004164       1       1         1         1   0.9017       1
   ...
Here we see that the "disease" variant, rs2222162 has moved from being number 2 in the list to number 1, although it is still not significant after genome-wide correction.

In this last example, we specifically requested that PLINK pair up the most similar individuals. We can also perform the clustering, but with fewer, or different, constraints on the final solution. For example, here we do not impose a maximum cluster size: rather we request that each cluster contains at least 1 case and 1 control (i.e. so that it is informative for association) with the --cc option, and specify a threshold of 0.01 for --ppc:

plink --bfile hapmap1 --cluster --cc --ppc 0.01 --out version2

which generates the following final solution (version2.cluster1):

SOL-0    HCB181_1(1) HCB189_1(1) HCB198_1(1) HCB210_1(2) HCB222_1(1) HCB203_1(1) HCB196_1(1)  
         HCB183_1(2) HCB195_1(1) HCB185_1(1) HCB187_1(1) HCB215_1(2) HCB216_1(1)

SOL-1    HCB182_1(1) HCB186_1(1) HCB207_1(2) HCB223_1(1) HCB194_1(1) HCB188_1(1) HCB199_1(1) 
         HCB221_1(2) HCB225_1(1) HCB217_1(1) HCB190_1(1) HCB202_1(1) HCB224_1(2) HCB201_1(2) 
         HCB206_1(1) HCB208_1(1) HCB209_1(1) HCB213_1(1) HCB212_1(1) HCB214_1(2)

SOL-2    HCB184_1(1) HCB219_1(2) HCB218_1(1) HCB200_1(1) HCB191_1(2) HCB220_1(1) HCB197_1(1)
         HCB211_1(2) HCB192_1(1) HCB204_1(1) JPT255_1(2) HCB193_1(1) JPT245_1(2)

SOL-3    HCB205_1(1) JPT264_1(2) JPT253_1(1) JPT258_1(2) JPT228_1(1) JPT244_1(2) JPT238_1(2) 
         JPT269_1(2) JPT242_1(2) JPT234_1(2) JPT265_1(1) JPT230_1(2) JPT262_1(1) JPT267_1(2) 
         JPT231_1(1) JPT239_1(2) JPT263_1(2) JPT260_1(2)

SOL-4    JPT226_1(1) JPT251_1(2) JPT240_1(2) JPT227_1(2) JPT232_1(2) JPT235_1(2) JPT237_1(2) 
         JPT250_1(1) JPT246_1(2) JPT229_1(2) JPT243_1(1) JPT266_1(2) JPT252_1(2) JPT249_1(2) 
         JPT233_1(1) JPT248_1(2) JPT241_1(2) JPT254_1(1) JPT261_1(2) JPT259_1(2) JPT236_1(2) 
         JPT256_1(1) JPT247_1(2) JPT268_1(2) JPT257_1(2)
The lines have been wrapped for clarity of reading here: normally, an entire cluster file is on a single line. Also note that the phenotype has been added in parentheses after each family/individual ID (as the --cc option was used). Here we see that the resulting clusters have largely separated Chinese and Japanese individuals into different clusters. The clustering results in a five class solution based on the --ppc constraint -- i.e. clearly, to merge any of these five clusters would have involved merging two individuals who are different at the 0.01 level, and this is why the clustering stopped at this point (as opposed to merging everybody, ultimately arriving at a 1-class solution).

Based on this alternate clustering scheme, we can repeat our association analysis.

Note This is not necessarily how actual analysis of real data should be conducted, of course, i.e. by trying different analyses, clusters, etc, until one finds the most significant result... The point of this is just to show what options are available.

plink --bfile hapmap1 --mh --within version2.cluster2 --adjust --out aac2

Now the log file records that five clusters were found, and a low inflation factor:
     ...
     Cochran-Mantel-Haenszel 2x2xK test, K = 5
     Writing results to [ aac2.cmh ]
     Computing corrected significance values (FDR, Sidak, etc)
     ...
     Genomic inflation factor (based on median chi-squared) is 1.01489
     Mean chi-squared statistic is 0.990643
     ...

Looking at aac2.cmh.adjusted, we now see that the disease SNP is genome-wide significant:

 CHR        SNP      UNADJ         GC     BONF     HOLM SIDAK_SS SIDAK_SD   FDR_BH    FDR_BY
   2  rs2222162  8.313e-10  1.104e-09 5.47e-05 5.47e-05 5.47e-05 5.47e-05 5.47e-05 0.0006384
  13  rs9585021  2.432e-06  2.882e-06     0.16     0.16   0.1479   0.1479  0.06931     0.809
   2  rs4675607   3.16e-06  3.731e-06   0.2079   0.2079   0.1877   0.1877  0.06931     0.809
   2  rs4673349   5.63e-06  6.594e-06   0.3705   0.3705   0.3096   0.3096   0.0741    0.8648
   2  rs1375352   5.63e-06  6.594e-06   0.3705   0.3705   0.3096   0.3096   0.0741    0.8648
   ...

That is, rs2222162 now has a significance value of 5.47e-05 even if we use Bonferroni adjustment for multiple comparisons.

A third way to perform the stratification analysis is to specify the number of clusters one wants in the final solution. Here we will specify two clusters, using the --K option, and remove the significance test constraint by setting --ppc to 0 (by omitting that option):

plink --bfile hapmap1 --cluster --K 2 --out version3

This analysis results in the following two-class solution:

SOL-0 HCB181_1 HCB182_1 HCB225_1 HCB189_1 HCB188_1 HCB194_1 HCB205_1
      HCB208_1 HCB199_1 HCB221_1 HCB201_1 HCB206_1 HCB196_1 JPT253_1 
      HCB202_1 HCB203_1 HCB191_1 HCB220_1 HCB197_1 HCB211_1 HCB215_1
      HCB216_1 HCB212_1 HCB213_1 HCB183_1 HCB195_1 HCB193_1 HCB186_1
      HCB207_1 HCB223_1 HCB187_1 HCB209_1 HCB214_1 HCB184_1 HCB219_1
      HCB218_1 HCB200_1 HCB185_1 HCB217_1 HCB198_1 HCB210_1 HCB222_1
      HCB192_1 HCB190_1 HCB224_1

SOL-1 HCB204_1 JPT255_1 JPT257_1 JPT226_1 JPT242_1 JPT228_1 JPT244_1
      JPT238_1 JPT269_1 JPT232_1 JPT247_1 JPT231_1 JPT239_1 JPT229_1
      JPT243_1 JPT236_1 JPT256_1 JPT265_1 JPT227_1 JPT266_1 JPT268_1
      JPT263_1 JPT235_1 JPT237_1 JPT250_1 JPT246_1 JPT240_1 JPT251_1
      JPT259_1 JPT252_1 JPT233_1 JPT248_1 JPT241_1 JPT254_1 JPT261_1
      JPT245_1 JPT264_1 JPT249_1 JPT258_1 JPT230_1 JPT267_1 JPT262_1
      JPT234_1 JPT260_1

Here we see that the solution has assigned all Chinese and all Japanese two separate groups except for two individuals. If we use this cluster solution in the association analysis, we obtain the following results, again obtaining genome-wide significance:

 CHR        SNP      UNADJ        GC     BONF     HOLM SIDAK_SS SIDAK_SD   FDR_BH    FDR_BY
   2  rs2222162  8.951e-10 1.493e-09 5.89e-05 5.89e-05 5.89e-05 5.89e-05 5.89e-05 0.0006875
   2  rs4675607  9.255e-06 1.217e-05    0.609    0.609   0.4561   0.4561   0.2679         1
  13  rs9585021  1.222e-05 1.594e-05   0.8038   0.8038   0.5524   0.5524   0.2679         1
   2  rs1375352  2.753e-05 3.519e-05        1        1   0.8366   0.8365   0.3505         1
   2  rs4673349  2.753e-05 3.519e-05        1        1   0.8366   0.8365   0.3505         1
   9  rs7046471  3.196e-05 4.071e-05        1        1   0.8779   0.8779   0.3505         1
   6  rs9488062  4.481e-05 5.659e-05        1        1   0.9476   0.9476   0.4213         1
   ...

with similarly low inflation factors:
     Genomic inflation factor (based on median chi-squared) is 1.02729
     Mean chi-squared statistic is 0.982804

Finally, given that the actual ancestry of each individual is known in this particular sample, we can always 
use this external clustering in the analysis:

plink --bfile hapmap1 --mh --within pop.phe --adjust --out aac3

Unsurprisingly, this gives very similar results to the two-class solution derived from cluster analysis.
In summary,
	•	We have seen that simple IBS-based clustering approaches seem to work well, at least in terms of differentiating between Chinese and Japanese individuals, with this number of SNPs
	•	We have seen that accounting for this population substructure can lower false positive rates and increase power also - the disease variant is only genome-wide significant after performing a stratified analysis
	•	We have seen a number of different approaches to clustering applied. Which to use in practice is perhaps not a straightforward question. In general, when a small number of discrete subpopulations exist in the sample, then a cluster solution that most closely resembles this structure might be expected to work well. In contrast, if, instead of a small number of discrete, homogeneous clusters, the sample actually contains a complex mixture of individuals from across a range of clines of ancestry, then we might expect the approaches that form a large number of smaller classes (e.g. matching pairs) to perform better.
Finally, it is possible to generate a visualization of the substructure in the sample by creating a matrix of pairwsie IBS distances, then using a statistical package such as R to generate a multidimensional scaling plot, for example: use

plink --bfile hapmap1 --cluster --matrix --out ibd_view

which generates a file ibd_view.mdist. Then, in R, perform the following commands: (note: obviously, you need R installed for to perform these next actions -- it can be freely downloaded here)

m <- as.matrix(read.table("ibd_view.mdist"))
mds <- cmdscale(as.dist(1-m))
k <- c( rep("green",45) , rep("blue",44) )
plot(mds,pch=20,col=k)

which should generate a plot like this: (green represents Chinese individuals, blue represents Japanese individuals).

This plot certainly seems to suggest that at least two quite distinct clusters exist in the sample. Based on viewing this kind of plot, one would be in a better position to determine which approach to stratification to subsequently take.

NEW This plot can now be automatically generated with the --mds-plot option -- see this page.

Quantitative trait association analysis
At the beginning of this tutorial, we mentioned that the disease trait was based on a simple median split of a quantitative trait. Let's now analyse this quantitative trait directly. The basic analytic options are largely unchanged, except that the --mh approach is no longer available (this applies only to case/control samples). The --assoc flag will detect whether or not the phenotype is an affection status code or a quantitative trait and use the appropriate analysis: for quantitative traits, this is ordinary least squares regression. We simply need to tell PLINK to use the quantitative trait (which is in the file qt.phe instead of the default phenotype (i.e. column six of the .ped or .fam file):

plink --bfile hapmap1 --assoc --pheno qt.phe --out quant1

This analysis generates a file quant1.qassoc which has the following fields:

 CHR         SNP  NMISS       BETA      SE         R2        T         P
   1   rs6681049     89    -0.2266  0.3626   0.004469  -0.6249    0.5336
   1   rs4074137     89    -0.2949  0.6005   0.002765  -0.4911    0.6246
   1   rs1891905     89    -0.1053  0.3165   0.001272  -0.3328    0.7401
   1   rs9729550     89     0.5402  0.4616     0.0155     1.17    0.2451
   1   rs3813196     89     0.8053   1.025    0.00705   0.7859     0.434
   1  rs12044597     89    0.01658  0.3776  2.217e-05  0.04392    0.9651
   1  rs10907185     89      0.171   0.373    0.00241   0.4584    0.6478
   1  rs11260616     88    0.03574   0.444  7.533e-05  0.08049     0.936
   1    rs745910     87    -0.3093  0.4458   0.005632  -0.6938    0.4897
   1    rs262688     89      0.411  0.4467   0.009637   0.9201    0.3601
   1   rs2460000     89   -0.03558  0.3821  9.969e-05 -0.09314     0.926
   1    rs260509     89     -0.551   0.438    0.01787   -1.258    0.2118
   ...

The fields in this file represent:
	•	Chromosome
	•	SNP identifier
	•	Number of non-missing individuals for this analysis
	•	Regression coefficient
	•	Standard error of the coefficient
	•	The regression r-squared (multiple correlation coefficient)
	•	t-statistic for regression of phenotype on allele count
	•	Asymptotic significance value for coefficient
If we were to add the --adjust option, then a file quant1.qassoc.adjust would be created:

CHR        SNP      UNADJ         GC      BONF      HOLM  SIDAK_SS  SIDAK_SD    FDR_BH    FDR_BY
  2  rs2222162  9.083e-11  3.198e-09 5.977e-06 5.977e-06 5.977e-06 5.977e-06 5.977e-06 6.976e-05
 21   rs219746  1.581e-07  1.672e-06   0.01041    0.0104   0.01035   0.01035  0.005203   0.06072
  7  rs1922519  4.988e-06  3.038e-05    0.3283    0.3282    0.2798    0.2798    0.1094         1
  2  rs2969348  1.008e-05  5.493e-05    0.6636    0.6636     0.485     0.485    0.1122         1
  3  rs6773558  1.313e-05  6.857e-05    0.8638    0.8638    0.5785    0.5784    0.1122         1
 10  rs3862003  1.374e-05  7.123e-05    0.9038    0.9038     0.595     0.595    0.1122         1
  8   rs660416  1.554e-05  7.905e-05         1         1    0.6405    0.6404    0.1122         1
 14  rs2526935  1.611e-05  8.146e-05         1         1    0.6536    0.6535    0.1122         1
 ...

Here we see that the disease variant is significant after genome-wide correction. However, these tests do not take into account the clustering in the sample in the same way we did before. The genomic control inflation factor estimate is now:

     Genomic inflation factor (based on median chi-squared) is 1.19824
     Mean chi-squared statistic is 1.21478

Instead of performing a stratified analysis or including covariates, one approach is to use permutation: specifically, it is possible to permute (i.e. label-swap phenotypes between individuals) but only within cluster. This controls for any between-cluster association, as this will be constant under all permuted datasets. We request clustered permutation as follows, using the original pairing approach to matching:

plink --bfile hapmap1 --assoc --pheno qt.phe --perm --within str1.cluster2 --out quant2

In this case we are using adaptive permutation. See the section of the main documentation that describes permutation testing for more details. The output will show:
     ...
     89 of 89 individuals assigned to 45 cluster(s)
     ...
     Set to permute within 45 cluster(s)
     Writing QT association results to [ quant2.qassoc ]
     Adaptive permutation: 1000000 of (max) 1000000 : 25 SNPs left

This analysis will take some time depending on how fast your computer is, probably at least 1 hour. The last line shown above will change, counting the number of permutations performed, and the number of SNPs left in the analysis at any given stage. Here it reaches the default maximum of 1 million permutations and 25 SNPs remain still (see the link above for more details on this procedure).
The adaptive permutation procedure results in a file quant2.qassoc.perm. Sorting this file by the emprical p-value (EMP1, the fourth column) we see that the disease variant rs2222162 is top of the list, with an empirical significance value of 1e-6 (essentially indicating that no permuted datasets had a statistic for rs2222162 that exceeded this).

 CHR          SNP      STAT          EMP1         NP
   2    rs2222162     42.01         1e-06    1000000
   6    rs1606447     5.869     5.206e-05      38415
  10    rs1393829     16.34     0.0001896      10549
  21     rs219746     27.49     0.0001896      10549
   2    rs2304287     9.021     0.0001896      10549
   6    rs2326873     6.659     0.0001896      10549
   2    rs1385855      7.59     0.0002227      13468
   2    rs6543704     8.131     0.0002227      13468
   ...

IMPORTANT When using the --within option along with permutaion, the empirical significance values EMP1 will appropriately reflect that we have controlled for the clustering variable. In contrast, the standard chi-squared statistics (STAT in this file) will not reflect the within-cluster analysis. That is, the test used is the same identical test as used in standard analysis -- the only thing that changes is the way we permute the sample. The STAT values will be identical to the standard, non-clustered analysis therefore.
The NP field shows how many permutations were conducted for each SNP. For the SNPs at the bottom of the list, PLINK may well have given up after only 6 permutations (i.e. these were SNPs that were clearly not going to be highly significant if after 6 permutations they were exceeded more than a couple of times). Naturally, this approach speeds up permutation analysis but does not provide a means for controlling for multiple testing (i.e. by comparing each observed test statistic against the maximum of all permuted statistics in each replicate). This can be achieved with the --mperm option:

plink --bfile hapmap1 --assoc --pheno qt.phe --mperm 1000 --within str1.cluster2 --out quant3

With --mperm you must also specify the number of replicates -- this number can be fairly low, as one is primarily interested in the corrected p-values being less than some reasonably high nominal value such as 0.05, rather than accurately estimating the point-wise empirical significance, which might be very small.
Finally, we might want to test whether the association with the continuous phenotype differs between the two populations: for this we can use the --gxe option, along with population membership (which is currently limited to the dichotomous case) being specified as a covariate with the --covar option (same format as cluster files). Let's just perform this analysis for the main SNP of interest rather than all SNPs:

plink --bfile hapmap1 --pheno qt.phe --gxe --covar pop.phe --snp rs2222162 --out quant3

The output will show that a file quant3.qassoc.gxe has been created, which contains the following fields:

 CHR        SNP NMISS1    BETA1      SE1   NMISS2   BETA2     SE2    Z_GXE    P_GXE
   2  rs2222162     45   -2.271   0.2245       44  -1.997  0.1722  -0.9677   0.3332

which show the number of non-missing individuals in each category along with the regression coefficient and standard error, followed by a test of whether these two regression coefficients are significantly different (Z_GXE) and an asymptotic significance value (P_GXE). In this case, we see the similar effect in both populations (regression coefficients around -2) and the test for interaction of SNP x population interaction is not significant.

Extracting a SNP of interest

Finally, given you've identified a SNP, set of SNPs or region of interest, you might want to extract those SNPs as a separate, smaller, more manageable file. In particular, for other applications to analyse the data, you will need to convert from the binary PED file format to a standard PED format. This is done using the --recode options (fully described here). There are a few forms of this option: we will use the --recodeAD that codes the genotypes in a manner that is convenient for subsequent analysis in R or any other non-genetic statistical package. To extract only this single SNP, use:

plink --bfile hapmap1 --snp rs2222162 --recodeAD --out rec_snp1

(to select a region, use the --to and --from options instead, or use --window 100 with --snp to select a 100kb region surrounding that SNP, for example). This particular recode feature codes genotypes as additive (0,1,2) and dominance (0,1,0) components, in a file called rec_snp1.recode.raw. We can then load this file into our statistics package and easily perform other analyses: for example, to repeat the main analysis as a simple logistic regression using the R package (not controlling for clusters):

d <- read.table("rec_snp1.recode.raw" , header=T)
summary(glm(PHENOTYPE-1 ~ rs2222162_A, data=d, family="binomial"))

     Coefficients:
                 Estimate Std. Error z value Pr(>|z|)
     (Intercept)  -1.6795     0.4827  -3.479 0.000503 ***
     rs2222162_A   1.5047     0.3765   3.997 6.42e-05 *** 
which confirms the original analysis. Naturally, things such as survival analysis or other models not implemented in PLINK can now be performed.

Other areas...
That's all for this tutorial. We've seen how to use PLINK to analyse a dummy dataset. Hopefully things went smoothly and you are now more familiar with using PLINK and can start applying it to your own datasets. There are a large number of areas that we have not even touched here:
	•	Using haplotype-based tests, or other multi-locus tests (Hotelling's T(2) test, etc)
	•	Analysing family-based samples
	•	Other summary statistic measures such as Hardy-Weinberg and Mendel errors
	•	Estimating IBD between pairs of individuals
	•	Tests of epistasis
	•	Data-management options such as merging files
	•	etc
but this is enough for now. In time, a second tutorial might appear that covers some of these things...

Extracting SNPS from the  DeKAF GWAS

The DEKAF Plink files are located at 
/home/ isrania/shared/DeKAF-GWAS/plink/

	•	Load the plink software
module avail plink
module load plink
plink
*move into the plink analysis file
cd /panfs/roc/groups/5/isrania/shared/DeKAF-GWAS/plink/
ls

Finally, given you've identified a SNP, set of SNPs or region of interest, you might want to extract those SNPs as a separate, smaller, more manageable file. In particular, for other applications to analyse the data, you will need to convert from the binary PED file format to a standard PED format. This is done using the --recode options (fully described here). There are a few forms of this option: we will use the --recodeAD that codes the genotypes in a manner that is convenient for subsequent analysis in R or any other non-genetic statistical package. To extract only this single SNP, use:

plink --bfile hapmap1 --snp rs2222162 --recodeAD --out rec_snp1(***This command in case sensitive)

plink --bfile Minnesota --snp rs776746 --recode AD --out rs776746_test 
plink --bfile Minnesota --snp rs10264272 --recode AD --out rs10264272_test
plink --bfile Minnesota --snp rs41303343 --recode AD --out rs41303343_test
plink --bfile Minnesota --snp rs35599367 --recode AD --out rs35599367_test
plink --bfile Minnesota --snp rs1057868 --recode AD --out rs1057868_test
plink --bfile Minnesota --snp rs2740574 --recode AD --out rs2740574_test
plink --bfile Minnesota --snp rs12801394 --recode AD --out rs12801394_test
plink --bfile Minnesota --snp rs61733057 --recode AD --out rs61733057_test
plink --bfile Minnesota --snp rs61733056 --recode AD --out rs61733056_test
plink --bfile Minnesota --snp rs737733 --recode AD --out rs737733_test


(to select a region, use the --to and --from options instead, or use --window 100 with --snp to select a 100kb region surrounding that SNP, for example). This particular recode feature codes genotypes as additive (0,1,2) and dominance (0,1,0) components, in a file called rec_snp1.recode.raw. We can then load this file into our statistics package and easily perform other analyses: for example, using the R package 

d <- read.table("rec_snp1.recode.raw" , header=T)
head (d)
summary(glm(PHENOTYPE-1 ~ rs2222162_A, data=d, family="binomial"))

     Coefficients:
                 Estimate Std. Error z value Pr(>|z|)
     (Intercept)  -1.6795     0.4827  -3.479 0.000503 ***
     rs2222162_A   1.5047     0.3765   3.997 6.42e-05 *** 

which confirms the original analysis. Naturally, things such as survival analysis or other models not implemented in PLINK can now be performed.

The GEN03 Plink files are located at 	
cd /panfs/roc/groups/5/isrania/shared/GEN03/Plink
	•	Load the plink software
module avail plink
module load plink
plink
*move into the plink analysis file
cd /panfs/roc/groups/5/isrania/shared/GEN03/Plink
ls

Finally, given you've identified a SNP, set of SNPs or region of interest, you might want to extract those SNPs as a separate, smaller, more manageable file. In particular, for other applications to analyse the data, you will need to convert from the binary PED file format to a standard PED format. This is done using the --recode options (fully described here). There are a few forms of this option: we will use the --recodeAD that codes the genotypes in a manner that is convenient for subsequent analysis in R or any other non-genetic statistical package. To extract only this single SNP, use:

plink --bfile hapmap1 --snp rs2222162 --recodeAD --out rec_snp1(***This command in case sensitive)

plink --bfile UMN1556_AffyID --snp rs776746 --recode AD --out rs776746_GEN03 
plink --bfile UMN1556_AffyID --snp rs10264272 --recode AD --out rs10264272_GEN03
plink --bfile UMN1556_AffyID --snp rs41303343 --recode AD --out rs41303343_GEN03
plink --bfile UMN1556_AffyID --snp rs35599367 --recode AD --out rs35599367_GEN03
plink --bfile UMN1556_AffyID --snp rs1057868 --recode AD --out rs1057868_GEN03
plink --bfile UMN1556_AffyID --snp rs2740574 --recode AD --out rs2740574_GEN03
plink --bfile UMN1556_AffyID --snp rs12801394 --recode AD --out rs12801394_GEN03
plink --bfile UMN1556_AffyID --snp rs61733057 --recode AD --out rs61733057_GEN03
plink --bfile UMN1556_AffyID --snp rs61733056 --recode AD --out rs61733056_GEN03
plink --bfile UMN1556_AffyID --snp rs737733 --recode AD --out rs737733_GEN03


(to select a region, use the --to and --from options instead, or use --window 100 with --snp to select a 100kb region surrounding that SNP, for example). This particular recode feature codes genotypes as additive (0,1,2) and dominance (0,1,0) components, in a file called rec_snp1.recode.raw. We can then load this file into our statistics package and easily perform other analyses: for example, using the R package 

d <- read.table("rec_snp1.recode.raw" , header=T)
head (d)
summary(glm(PHENOTYPE-1 ~ rs2222162_A, data=d, family="binomial"))

     Coefficients:
                 Estimate Std. Error z value Pr(>|z|)
     (Intercept)  -1.6795     0.4827  -3.479 0.000503 ***
     rs2222162_A   1.5047     0.3765   3.997 6.42e-05 *** 

which confirms the original analysis. Naturally, things such as survival analysis or other models not implemented in PLINK can now be performed.

