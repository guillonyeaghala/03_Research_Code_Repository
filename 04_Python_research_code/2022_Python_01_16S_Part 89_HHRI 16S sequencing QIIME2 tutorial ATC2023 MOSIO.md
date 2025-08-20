Notes for future me

*** I have moved all of the documents that used to be on my MAC to my external hard drive (MAC_Documents/ in the ROOK PC partition, which is all backed up in the ROOK_MAC and ROOK_BACKUP partitions as well. However, I cannot use ROOK_MAC or ROOK_BACKUP for the analyses since they are on MAC extended format with no writing permissions, but the terminal can write to the ROOK_PC partition as it is in DOS format

** If for some reason the PC volumes doesn’t show up on the MAC, follow these directions to use the command line (terminal) so that you can list the drive and manually mount them

https://discussions.apple.com/thread/7571770

http://osxdaily.com/2013/05/13/mount-unmount-drives-from-the-command-line-in-mac-os-x/

I also have the music folder in there, which can be found by going in the MACINTOSH HD root folder

*** I was not able to find the data folder in the songbird folder (went to http://osxdaily.com/2013/01/17/access-root-directory-mac-os-x/ to find out how to find the root file for QIIME2, then looked up somgbird in mackintosh HD. So it looks like I will have to test directly on the asmic oral qiime2 folder)

Macintosh HD/Users/guillonyeaghala/music

*To copy a pathname into the terminal window on mac, go to thee folder using the file explorer, right click on the folder and press the “option key” while right clicking to see the option to “copy as a pathname”

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

*Issues activating the conda environment
Activate the conda environment
Now that you have a QIIME 2 environment, activate it using the environment’s name:
source activate qiime2-2019.10
conda activate qiime2-2019.10

conda info --envs
*After updating my miniconda installations when I installed HUMANN3, I think I need to use the full path name for qiime 2
source activate /Users/guillaumeonyeaghala/miniconda2/envs/qiime2-2019.10

*** ASMIC Oral and Gut Microbiome comparisons

	•	First, copy the 100 gut sample sequences from V1 and V3 (forward and reverse, so a total of 200) into a separate folder from the WBOB_PC backup files. You can find the sample IDs from the asmic gut mapping file, if you sort the sample ID and the file names in descending order.
	•	Second, copy the oral manifest file to the desktop, and add the 100 gut sample forward and reverse sequence file pathways to that document. You can find the sample IDs from the asmic gut mapping file, if you sort the sample ID and the file names in descending order.
	•	Third, copy the 100 gut sequence files and the 100 oral sequence file in the same folder, and place it in the location for the analysis (V1_V3_Gut)
	•	Fourth, Create a new mapping file (combined from the asmic oral map and the asmic gut map, with a total of 200 samples i.e. row). Sort by Subject ID then sort by collection when copying the files together to append them, and make sure that you have create a site variable (oral vs. gut)



	•	
“Moving Pictures” tutorial

https://docs.qiime2.org/2019.10/tutorials/moving-pictures/

Note
This guide assumes you have installed QIIME 2 using one of the procedures in the install documents.
Note
This guide uses QIIME 2-specific terminology, please see the glossary for more details.
In this tutorial you’ll use QIIME 2 to perform an analysis of human microbiome samples from two individuals at four body sites at five timepoints, the first of which immediately followed antibiotic usage. A study based on these samples was originally published in Caporaso et al. (2011). The data used in this tutorial were sequenced on an Illumina HiSeq using the Earth Microbiome Project hypervariable region 4 (V4) 16S rRNA sequencing protocol.
Activate the conda environment
Now that you have a QIIME 2 environment, activate it using the environment’s name:
source activate qiime2-2019.10
conda activate qiime2-2019.10

conda info --envs
*After updating my miniconda installations when I installed HUMANN3, I think I need to use the full path name for qiime 2
source activate /Users/guillaumeonyeaghala/miniconda2/envs/qiime2-2019.10
To deactivate an environment, run source deactivate.
Before beginning this tutorial, create a new directory and change to that directory.
pwd
Move to the documents folder
cd /Users/guillaumeonyeaghala/Documents/qiime2
mkdir qiime2-moving-pictures-tutorial
cd /Users/guillaumeonyeaghala/Documents/qiime2/qiime2-moving-pictures-tutorial
Since the files are getting to be large, I am moving the qiime2 file to my external hard drive to prevent size issues

https://discussions.apple.com/thread/3871334
cd /Volumes/ROOK_PC/MAC_Documents/qiime2/asmic_oral_test
mkdir qiime2-moving-pictures-tutorial-2020
cd /Volumes/ROOK_PC/MAC_Documents/qiime2/qiime2-moving-pictures-tutorial-2020
cd /Volumes/RAVPOWER/ROOK_PC/MAC_Documents/qiime2/qiime2-moving-pictures-tutorial-2020
Sample metadata
Before starting the analysis, explore the sample metadata to familiarize yourself with the samples used in this study. The sample metadata is available as a Google Sheet. 	
*** Use the curl option in the mac terminal and the wget on windows (through the command line in linux)
curl -sL \
  "https://data.qiime2.org/2019.10/tutorials/moving-pictures/sample_metadata.tsv" > \
  "sample-metadata.tsv"
ls
Obtaining and importing data
Download the sequence reads that we’ll use in this analysis. In this tutorial we’ll work with a small subset of the complete sequence data so that the commands will run quickly.
pwd
mkdir emp-single-end-sequences
ls
curl -sL \
  "https://data.qiime2.org/2019.10/tutorials/moving-pictures/emp-single-end-sequences/barcodes.fastq.gz" > \
  "emp-single-end-sequences/barcodes.fastq.gz"
curl -sL \
  "https://data.qiime2.org/2019.10/tutorials/moving-pictures/emp-single-end-sequences/sequences.fastq.gz" > \
  "emp-single-end-sequences/sequences.fastq.gz"
** you should import the files via curl first before changing the file since the curl link contains the folder name for the download
cd /Users/guillaumeonyeaghala/Documents/qiime2-moving-pictures-tutorial/emp-single-end-sequences
cd /Volumes/ROOK_PC/MAC_Documents/qiime2/qiime2-moving-pictures-tutorial-2020/emp-single-end-sequences

ls
*** To continue the tutorial you should switch back to the moving picture tutorial folder
cd /Users/guillaumeonyeaghala/Documents/qiime2-moving-pictures-tutorial
cd /Volumes/ROOK_PC/MAC_Documents/qiime2/qiime2-moving-pictures-tutorial-2020
All data that is used as input to QIIME 2 is in form of QIIME 2 artifacts, which contain information about the type of data and the source of the data. So, the first thing we need to do is import these sequence data files into a QIIME 2 artifact.
The semantic type of this QIIME 2 artifact is EMPSingleEndSequences. EMPSingleEndSequences QIIME 2 artifacts contain sequences that are multiplexed, meaning that the sequences have not yet been assigned to samples (hence the inclusion of both sequences.fastq.gz and barcodes.fastq.gz files, where the barcodes.fastq.gz contains the barcode read associated with each sequence in sequences.fastq.gz.) To learn about how to import sequence data in other formats, see the importing data tutorial.
qiime tools import \
  --type EMPSingleEndSequences \
  --input-path emp-single-end-sequences \
  --output-path emp-single-end-sequences.qza
** This will create the qiime zipped artifact in the folder you selected when you ran the command
Tip
Links are included to view and download precomputed QIIME 2 artifacts and visualizations created by commands in the documentation. For example, the command above created a single emp-single-end-sequences.qza file, and a corresponding precomputed file is linked above. You can view precomputed QIIME 2 artifacts and visualizations without needing to install additional software (e.g. QIIME 2).
QIIME 1 Users
In QIIME 1, we generally suggested performing demultiplexing through QIIME (e.g., with split_libraries.py or split_libraries_fastq.py) as this step also performed quality control of sequences. We now separate the demultiplexing and quality control steps, so you can begin QIIME 2 with either multiplexed sequences (as we’re doing here) or demultiplexed sequences.
Demultiplexing sequences
To demultiplex sequences we need to know which barcode sequence is associated with each sample. This information is contained in the sample metadata file. You can run the following commands to demultiplex the sequences (the demux emp-single command refers to the fact that these sequences are barcoded according to the Earth Microbiome Project protocol, and are single-end reads). The demux.qza QIIME 2 artifact will contain the demultiplexed sequences. The second output (demux-details.qza) presents Golay error correction details, and will not be explored in this tutorial (you can visualize these data using qiime metadata tabulate).
qiime demux emp-single \
  --i-seqs emp-single-end-sequences.qza \
  --m-barcodes-file sample-metadata.tsv \
  --m-barcodes-column barcode-sequence \
  --o-per-sample-sequences demux.qza \
  --o-error-correction-details demux-details.qza
** I did not need to move to the file with the sequence and barcode (in the emp-single-end sequences folder) because the qza file created used the sequence file and the barcode file to have the barcode as the merging variable for the demultiplexing step)
After demultiplexing, it’s useful to generate a summary of the demultiplexing results. This allows you to determine how many sequences were obtained per sample, and also to get a summary of the distribution of sequence qualities at each position in your sequence data.
qiime demux summarize \
  --i-data demux.qza \
  --o-visualization demux.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view demux.qzv
Alternatively, you can view QIIME 2 artifacts and visualizations at view.qiime2.org by uploading files or providing URLs. There are also precomputed results that can be viewed or downloaded after each step in the tutorial. These can be used if you’re reading the tutorial, but not running the commands yourself.
Sequence quality control and feature table construction
QIIME 2 plugins are available for several quality control methods, including DADA2, Deblur, and basic quality-score-based filtering. In this tutorial we present this step using DADA2 and Deblur. These steps are interchangeable, so you can use whichever of these you prefer. The result of both of these methods will be a FeatureTable[Frequency] QIIME 2 artifact, which contains counts (frequencies) of each unique sequence in each sample in the dataset, and a FeatureData[Sequence] QIIME 2 artifact, which maps feature identifiers in the FeatureTable to the sequences they represent.
Note
As you work through one or both of the options in this section, you’ll create artifacts with filenames that are specific to the method that you’re running (e.g., the feature table that you generate with dada2 denoise-single will be called table-dada2.qza). After creating these artifacts you’ll rename the artifacts from one of the two options to more generic filenames (e.g., table.qza). This process of creating a specific name for an artifact and then renaming it is only done to allow you to choose which of the two options you’d like to use for this step, and then complete the tutorial without paying attention to that choice again. It’s important to note that in this step, or any step in QIIME 2, the filenames that you’re giving to artifacts or visualizations are not important.
QIIME 1 Users
The FeatureTable[Frequency] QIIME 2 artifact is the equivalent of the QIIME 1 OTU or BIOM table, and the FeatureData[Sequence] QIIME 2 artifact is the equivalent of the QIIME 1 representative sequences file. Because the “OTUs” resulting from DADA2 and Deblur are created by grouping unique sequences, these are the equivalent of 100% OTUs from QIIME 1, and are generally referred to as sequence variants. In QIIME 2, these OTUs are higher resolution than the QIIME 1 default of 97% OTUs, and they’re higher quality since these quality control steps are better than those implemented in QIIME 1. This should therefore result in more accurate estimates of diversity and taxonomic composition of samples than was achieved with QIIME 1.
Option 1: DADA2
DADA2 is a pipeline for detecting and correcting (where possible) Illumina amplicon sequence data. As implemented in the q2-dada2 plugin, this quality control process will additionally filter any phiX reads (commonly present in marker gene Illumina sequence data) that are identified in the sequencing data, and will filter chimeric sequences.
The dada2 denoise-single method requires two parameters that are used in quality filtering: --p-trim-left m, which trims off the first m bases of each sequence, and --p-trunc-len n which truncates each sequence at position n. This allows the user to remove low quality regions of the sequences. To determine what values to pass for these two parameters, you should review the Interactive Quality Plot tab in the demux.qzv file that was generated by qiime demux summarize above.
Question
Based on the plots you see in demux.qzv, what values would you choose for --p-trunc-len and --p-trim-left in this case?
In the demux.qzv quality plots, we see that the quality of the initial bases seems to be high, so we won’t trim any bases from the beginning of the sequences. The quality seems to drop off around position 120, so we’ll truncate our sequences at 120 bases. This next command may take up to 10 minutes to run, and is the slowest step in this tutorial.
Output artifacts:
qiime dada2 denoise-single \
  --i-demultiplexed-seqs demux.qza \
  --p-trim-left 0 \
  --p-trunc-len 120 \
  --o-representative-sequences rep-seqs-dada2.qza \
  --o-table table-dada2.qza \
  --o-denoising-stats stats-dada2.qza
Output visualizations:
qiime metadata tabulate \
  --m-input-file stats-dada2.qza \
  --o-visualization stats-dada2.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view stats-dada2.qzv
If you’d like to continue the tutorial using this FeatureTable (opposed to the Deblur feature table generated in Option 2), run the following commands.
mv rep-seqs-dada2.qza rep-seqs.qza
mv table-dada2.qza table.qza
*** This command will rename the rep-seqs-dada2.qza and the tabe-dada2.qza files to the new names
FeatureTable and FeatureData summaries
After the quality filtering step completes, you’ll want to explore the resulting data. You can do this using the following two commands, which will create visual summaries of the data. The feature-table summarize command will give you information on how many sequences are associated with each sample and with each feature, histograms of those distributions, and some related summary statistics. The feature-table tabulate-seqs command will provide a mapping of feature IDs to sequences, and provide links to easily BLAST each sequence against the NCBI nt database. The latter visualization will be very useful later in the tutorial, when you want to learn more about specific features that are important in the data set.
qiime feature-table summarize \
  --i-table table.qza \
  --o-visualization table.qzv \
  --m-sample-metadata-file sample-metadata.tsv
qiime feature-table tabulate-seqs \
  --i-data rep-seqs.qza \
  --o-visualization rep-seqs.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view table.qzv
qiime tools view rep-seqs.qzv
*** In order to tell which file type you have, you can use the qiime tools command
qiime tools --help
qiime tools peek table.qza
Generate a tree for phylogenetic diversity analyses
QIIME supports several phylogenetic diversity metrics, including Faith’s Phylogenetic Diversity and weighted and unweighted UniFrac. In addition to counts of features per sample (i.e., the data in the FeatureTable[Frequency] QIIME 2 artifact), these metrics require a rooted phylogenetic tree relating the features to one another. This information will be stored in a Phylogeny[Rooted] QIIME 2 artifact. To generate a phylogenetic tree we will use align-to-tree-mafft-fasttree pipeline from the q2-phylogeny plugin.
First, the pipeline uses the mafft program to perform a multiple sequence alignment of the sequences in our FeatureData[Sequence] to create a FeatureData[AlignedSequence] QIIME 2 artifact. Next, the pipeline masks (or filters) the alignment to remove positions that are highly variable. These positions are generally considered to add noise to a resulting phylogenetic tree. Following that, the pipeline applies FastTree to generate a phylogenetic tree from the masked alignment. The FastTree program creates an unrooted tree, so in the final step in this section midpoint rooting is applied to place the root of the tree at the midpoint of the longest tip-to-tip distance in the unrooted tree.
qiime phylogeny align-to-tree-mafft-fasttree \
  --i-sequences rep-seqs.qza \
  --o-alignment aligned-rep-seqs.qza \
  --o-masked-alignment masked-aligned-rep-seqs.qza \
  --o-tree unrooted-tree.qza \
  --o-rooted-tree rooted-tree.qza
ls

Alpha and beta diversity analysis
QIIME 2’s diversity analyses are available through the q2-diversity plugin, which supports computing alpha and beta diversity metrics, applying related statistical tests, and generating interactive visualizations. We’ll first apply the core-metrics-phylogenetic method, which rarefies a FeatureTable[Frequency] to a user-specified depth, computes several alpha and beta diversity metrics, and generates principle coordinates analysis (PCoA) plots using Emperor for each of the beta diversity metrics. The metrics computed by default are:
	•	Alpha diversity
	•	Shannon’s diversity index (a quantitative measure of community richness)
	•	Observed OTUs (a qualitative measure of community richness)
	•	Faith’s Phylogenetic Diversity (a qualitiative measure of community richness that incorporates phylogenetic relationships between the features)
	•	Evenness (or Pielou’s Evenness; a measure of community evenness)
	•	Beta diversity
	•	Jaccard distance (a qualitative measure of community dissimilarity)
	•	Bray-Curtis distance (a quantitative measure of community dissimilarity)
	•	unweighted UniFrac distance (a qualitative measure of community dissimilarity that incorporates phylogenetic relationships between the features)
	•	weighted UniFrac distance (a quantitative measure of community dissimilarity that incorporates phylogenetic relationships between the features)
An important parameter that needs to be provided to this script is --p-sampling-depth, which is the even sampling (i.e. rarefaction) depth. Because most diversity metrics are sensitive to different sampling depths across different samples, this script will randomly subsample the counts from each sample to the value provided for this parameter. For example, if you provide --p-sampling-depth 500, this step will subsample the counts in each sample without replacement so that each sample in the resulting table has a total count of 500. If the total count for any sample(s) are smaller than this value, those samples will be dropped from the diversity analysis. Choosing this value is tricky. We recommend making your choice by reviewing the information presented in the table.qzv file that was created above. Choose a value that is as high as possible (so you retain more sequences per sample) while excluding as few samples as possible.
Question
View the table.qzv QIIME 2 artifact, and in particular the Interactive Sample Detail tab in that visualization. What value would you choose to pass for --p-sampling-depth? How many samples will be excluded from your analysis based on this choice? How many total sequences will you be analyzing in the core-metrics-phylogenetic command?
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny rooted-tree.qza \
  --i-table table.qza \
  --p-sampling-depth 1103 \
  --m-metadata-file sample-metadata.tsv \
  --output-dir core-metrics-results
ls
**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls core-metrics-results/
Here we set the --p-sampling-depth parameter to 1103. This value was chosen based on the number of sequences in the L3S313 sample because it’s close to the number of sequences in the next few samples that have higher sequence counts, and because it is considerably higher (relatively) than the number of sequences in the samples that have fewer sequences. This will allow us to retain most of our samples. The three samples that have fewer sequences will be dropped from the core-metrics-phylogenetic analyses and anything that uses these results. It is worth noting that all three of these samples are “right palm” samples. Losing a disproportionate number of samples from one metadata category is not ideal. However, we are dropping a small enough number of samples here that this felt like the best compromise between total sequences analyzed and number of samples retained.
Note
The sampling depth of 1103 was chosen based on the DADA2 feature table summary. If you are using a Deblur feature table rather than a DADA2 feature table, you might want to choose a different even sampling depth. Apply the logic from the previous paragraph to help you choose an even sampling depth.
Note
In many Illumina runs you’ll observe a few samples that have very low sequence counts. You will typically want to exclude those from the analysis by choosing a larger value for the sampling depth at this stage.
After computing diversity metrics, we can begin to explore the microbial composition of the samples in the context of the sample metadata. This information is present in the sample metadata file that was downloaded earlier.
We’ll first test for associations between categorical metadata columns and alpha diversity data. We’ll do that here for the Faith Phylogenetic Diversity (a measure of community richness) and evenness metrics.
qiime diversity alpha-group-significance \
  --i-alpha-diversity core-metrics-results/faith_pd_vector.qza \
  --m-metadata-file sample-metadata.tsv \
  --o-visualization core-metrics-results/faith-pd-group-significance.qzv

qiime diversity alpha-group-significance \
  --i-alpha-diversity core-metrics-results/evenness_vector.qza \
  --m-metadata-file sample-metadata.tsv \
  --o-visualization core-metrics-results/evenness-group-significance.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view core-metrics-results/faith-pd-group-significance.qzv
qiime tools view core-metrics-results/evenness-group-significance.qzv
Question
Which categorical sample metadata columns are most strongly associated with the differences in microbial community richness? Are these differences statistically significant?
Question
Which categorical sample metadata columns are most strongly associated with the differences in microbial community evenness? Are these differences statistically significant?
In this data set, no continuous sample metadata columns (e.g., days-since-experiment-start) are correlated with alpha diversity, so we won’t test for those associations here. If you’re interested in performing those tests (for this data set, or for others), you can use the qiime diversity alpha-correlation command.
Alpha rarefaction plotting
In this section we’ll explore alpha diversity as a function of sampling depth using the qiime diversity alpha-rarefaction visualizer. This visualizer computes one or more alpha diversity metrics at multiple sampling depths, in steps between 1 (optionally controlled with --p-min-depth) and the value provided as --p-max-depth. At each sampling depth step, 10 rarefied tables will be generated, and the diversity metrics will be computed for all samples in the tables. The number of iterations (rarefied tables computed at each sampling depth) can be controlled with --p-iterations. Average diversity values will be plotted for each sample at each even sampling depth, and samples can be grouped based on metadata in the resulting visualization if sample metadata is provided with the --m-metadata-file parameter.
qiime diversity alpha-rarefaction \
  --i-table table.qza \
  --i-phylogeny rooted-tree.qza \
  --p-max-depth 4000 \
  --m-metadata-file sample-metadata.tsv \
  --o-visualization alpha-rarefaction.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view alpha-rarefaction.qzv
The visualization will have two plots. The top plot is an alpha rarefaction plot, and is primarily used to determine if the richness of the samples has been fully observed or sequenced. If the lines in the plot appear to “level out” (i.e., approach a slope of zero) at some sampling depth along the x-axis, that suggests that collecting additional sequences beyond that sampling depth would not be likely to result in the observation of additional features. If the lines in a plot don’t level out, this may be because the richness of the samples hasn’t been fully observed yet (because too few sequences were collected), or it could be an indicator that a lot of sequencing error remains in the data (which is being mistaken for novel diversity).
The bottom plot in this visualization is important when grouping samples by metadata. It illustrates the number of samples that remain in each group when the feature table is rarefied to each sampling depth. If a given sampling depth d is larger than the total frequency of a sample s (i.e., the number of sequences that were obtained for sample s), it is not possible to compute the diversity metric for sample s at sampling depth d. If many of the samples in a group have lower total frequencies than d, the average diversity presented for that group at d in the top plot will be unreliable because it will have been computed on relatively few samples. When grouping samples by metadata, it is therefore essential to look at the bottom plot to ensure that the data presented in the top plot is reliable.
Note
The value that you provide for --p-max-depth should be determined by reviewing the “Frequency per sample” information presented in the table.qzv file that was created above. In general, choosing a value that is somewhere around the median frequency seems to work well, but you may want to increase that value if the lines in the resulting rarefaction plot don’t appear to be leveling out, or decrease that value if you seem to be losing many of your samples due to low total frequencies closer to the minimum sampling depth than the maximum sampling depth.
Question
When grouping samples by “body-site” and viewing the alpha rarefaction plot for the “observed_otus” metric, which body sites (if any) appear to exhibit sufficient diversity coverage (i.e., their rarefaction curves level off)? How many sequence variants appear to be present in those body sites?
Question
When grouping samples by “body-site” and viewing the alpha rarefaction plot for the “observed_otus” metric, the line for the “right palm” samples appears to level out at about 40, but then jumps to about 140. What do you think is happening here? (Hint: be sure to look at both the top and bottom plots.)

Alpha and beta diversity analysis
Next we’ll analyze sample composition in the context of categorical metadata using PERMANOVA (first described in Anderson (2001)) using the beta-group-significance command. The following commands will test whether distances between samples within a group, such as samples from the same body site (e.g., gut), are more similar to each other then they are to samples from the other groups (e.g., tongue, left palm, and right palm). If you call this command with the --p-pairwise parameter, as we’ll do here, it will also perform pairwise tests that will allow you to determine which specific pairs of groups (e.g., tongue and gut) differ from one another, if any. This command can be slow to run, especially when passing --p-pairwise, since it is based on permutation tests. So, unlike the previous commands, we’ll run beta-group-significance on specific columns of metadata that we’re interested in exploring, rather than all metadata columns to which it is applicable. Here we’ll apply this to our unweighted UniFrac distances, using two sample metadata columns, as follows.
Beta diversity for the body site variable
qiime diversity beta-group-significance \
  --i-distance-matrix core-metrics-results/unweighted_unifrac_distance_matrix.qza \
  --m-metadata-file sample-metadata.tsv \
  --m-metadata-column body-site \
  --o-visualization core-metrics-results/unweighted-unifrac-body-site-significance.qzv \
  --p-pairwise
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view core-metrics-results/unweighted-unifrac-body-site-significance.qzv
Beta diversity for the subject variable
qiime diversity beta-group-significance \
  --i-distance-matrix core-metrics-results/unweighted_unifrac_distance_matrix.qza \
  --m-metadata-file sample-metadata.tsv \
  --m-metadata-column subject \
  --o-visualization core-metrics-results/unweighted-unifrac-subject-group-significance.qzv \
  --p-pairwise
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view core-metrics-results/unweighted-unifrac-subject-group-significance.qzv
Question
Are the associations between subjects and differences in microbial composition statistically significant? How about body sites? What specific pairs of body sites are significantly different from each other?
Again, none of the continuous sample metadata that we have for this data set are correlated with sample composition, so we won’t test for those associations here. If you’re interested in performing those tests, you can use the qiime metadata distance-matrix in combination with qiime diversity mantel and qiime diversity bioenv commands.
Finally, ordination is a popular approach for exploring microbial community composition in the context of sample metadata. We can use the Emperor tool to explore principal coordinates (PCoA) plots in the context of sample metadata. While our core-metrics-phylogenetic command did already generate some Emperor plots, we want to pass an optional parameter, --p-custom-axes, which is very useful for exploring time series data. The PCoA results that were used in core-metrics-phylogeny are also available, making it easy to generate new visualizations with Emperor. We will generate Emperor plots for unweighted UniFrac and Bray-Curtis so that the resulting plot will contain axes for principal coordinate 1, principal coordinate 2, and days since the experiment start. We will use that last axis to explore how these samples changed over time.
Emperor plot for unweighted unifracs
qiime emperor plot \
  --i-pcoa core-metrics-results/unweighted_unifrac_pcoa_results.qza \
  --m-metadata-file sample-metadata.tsv \
  --p-custom-axes days-since-experiment-start \
  --o-visualization core-metrics-results/unweighted-unifrac-emperor-days-since-experiment-start.qzv
 Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view core-metrics-results/unweighted-unifrac-emperor-days-since-experiment-start.qzv
Emperor plot for Bray Curtis 
qiime emperor plot \
  --i-pcoa core-metrics-results/bray_curtis_pcoa_results.qza \
  --m-metadata-file sample-metadata.tsv \
  --p-custom-axes days-since-experiment-start \
  --o-visualization core-metrics-results/bray-curtis-emperor-days-since-experiment-start.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view core-metrics-results/bray-curtis-emperor-days-since-experiment-start.qzv
Question
Do the Emperor plots support the other beta diversity analyses we’ve performed here? (Hint: Experiment with coloring points by different metadata.)
Question
What differences do you observe between the unweighted UniFrac and Bray-Curtis PCoA plots?

Taxonomic analysis
In the next sections we’ll begin to explore the taxonomic composition of the samples, and again relate that to sample metadata. The first step in this process is to assign taxonomy to the sequences in our FeatureData[Sequence] QIIME 2 artifact. We’ll do that using a pre-trained Naive Bayes classifier and the q2-feature-classifier plugin. This classifier was trained on the Greengenes 13_8 99% OTUs, where the sequences have been trimmed to only include 250 bases from the region of the 16S that was sequenced in this analysis (the V4 region, bound by the 515F/806R primer pair). We’ll apply this classifier to our sequences, and we can generate a visualization of the resulting mapping from sequence to taxonomy.
Note
Taxonomic classifiers perform best when they are trained based on your specific sample preparation and sequencing parameters, including the primers that were used for amplification and the length of your sequence reads. Therefore in general you should follow the instructions in Training feature classifiers with q2-feature-classifier to train your own taxonomic classifiers. We provide some common classifiers on our data resources page, including Silva-based 16S classifiers, though in the future we may stop providing these in favor of having users train their own classifiers which will be most relevant to their sequence data.
curl -sL \
  "https://data.qiime2.org/2019.10/common/gg-13-8-99-515-806-nb-classifier.qza" > \
  "gg-13-8-99-515-806-nb-classifier.qza"
ls
Running the taxanomic analysis code
qiime feature-classifier classify-sklearn \
  --i-classifier gg-13-8-99-515-806-nb-classifier.qza \
  --i-reads rep-seqs.qza \
  --o-classification taxonomy.qza

qiime metadata tabulate \
  --m-input-file taxonomy.qza \
  --o-visualization taxonomy.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view taxonomy.qzv
Question
Recall that our rep-seqs.qzv visualization allows you to easily BLAST the sequence associated with each feature against the NCBI nt database. Using that visualization and the taxonomy.qzv visualization created here, compare the taxonomic assignments with the taxonomy of the best BLAST hit for a few features. How similar are the assignments? If they’re dissimilar, at what taxonomic level do they begin to differ (e.g., species, genus, family, …)?
Next, we can view the taxonomic composition of our samples with interactive bar plots. Generate those plots with the following command and then open the visualization.
qiime taxa barplot \
  --i-table table.qza \
  --i-taxonomy taxonomy.qza \
  --m-metadata-file sample-metadata.tsv \
  --o-visualization taxa-bar-plots.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view taxa-bar-plots.qzv
Question
Visualize the samples at Level 2 (which corresponds to the phylum level in this analysis), and then sort the samples by body-site, then by subject, and then by days-since-experiment-start. What are the dominant phyla in each in body-site? Do you observe any consistent change across the two subjects between days-since-experiment-start 0 and the later timepoints?
Differential abundance testing with ANCOM
ANCOM can be applied to identify features that are differentially abundant (i.e. present in different abundances) across sample groups. As with any bioinformatics method, you should be aware of the assumptions and limitations of ANCOM before using it. We recommend reviewing the ANCOM paper before using this method.
Note
Differential abundance testing in microbiome analysis is an active area of research. There are two QIIME 2 plugins that can be used for this: q2-gneiss and q2-composition. This section uses q2-composition, but there is another tutorial which uses gneiss on a different dataset if you are interested in learning more.
ANCOM is implemented in the q2-composition plugin. ANCOM assumes that few (less than about 25%) of the features are changing between groups. If you expect that more features are changing between your groups, you should not use ANCOM as it will be more error-prone (an increase in both Type I and Type II errors is possible). Because we expect a lot of features to change in abundance across body sites, in this tutorial we’ll filter our full feature table to only contain gut samples. We’ll then apply ANCOM to determine which, if any, sequence variants and genera are differentially abundant across the gut samples of our two subjects.
We’ll start by creating a feature table that contains only the gut samples. (To learn more about filtering, see the Filtering Data tutorial.)
qiime feature-table filter-samples \
  --i-table table.qza \
  --m-metadata-file sample-metadata.tsv \
  --p-where "[body-site]='gut'" \
  --o-filtered-table gut-table.qza
ANCOM operates on a FeatureTable[Composition] QIIME 2 artifact, which is based on frequencies of features on a per-sample basis, but cannot tolerate frequencies of zero. To build the composition artifact, a FeatureTable[Frequency] artifact must be provided to add-pseudocount (an imputation method), which will produce the FeatureTable[Composition] artifact.
qiime composition add-pseudocount \
  --i-table gut-table.qza \
  --o-composition-table comp-gut-table.qza
We can then run ANCOM on the subject column to determine what features differ in abundance across the gut samples of the two subjects.
qiime composition ancom \
  --i-table comp-gut-table.qza \
  --m-metadata-file sample-metadata.tsv \
  --m-metadata-column subject \
  --o-visualization ancom-subject.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view ancom-subject.qzv
Question
Which sequence variants differ in abundance across Subject? In which subject is each sequence variant more abundant? What are the taxonomies of some of these sequence variants? (To answer the last question you’ll need to refer to another visualization that was generated in this tutorial.)
We’re also often interested in performing a differential abundance test at a specific taxonomic level. To do this, we can collapse the features in our FeatureTable[Frequency] at the taxonomic level of interest, and then re-run the above steps. In this tutorial, we collapse our feature table at the genus level (i.e. level 6 of the Greengenes taxonomy).
qiime taxa collapse \
  --i-table gut-table.qza \
  --i-taxonomy taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table gut-table-l6.qza

qiime composition add-pseudocount \
  --i-table gut-table-l6.qza \
  --o-composition-table comp-gut-table-l6.qza

qiime composition ancom \
  --i-table comp-gut-table-l6.qza \
  --m-metadata-file sample-metadata.tsv \
  --m-metadata-column subject \
  --o-visualization l6-ancom-subject.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view l6-ancom-subject.qzv
***Final notes for the qiime 2 tutorial
 
There are a few file formats that you should be aware of when you are working with qiime2 formats:

In order to check the format use the following commands

*** In order to tell which file type you have, you can use the qiime tools command
qiime tools --help
qiime tools peek table.qza
The format analogs are as follows:

OTU table (count features / sample) = FeatureTable[Frequency]
Representative features (rep set by OTU)= FeatureTable[Sequence]
Pseudocount Table for ANCOM (Absolute abundance derived from OTU) = FeatureTable[Comparison] 
Dental Microbiome analysis notes

W:\Microbiome and Transplant\microbiome Female urinary suppl\Israniproject10\plaque-55-samples-analysis\51+PLA

W:\Microbiome and Transplant\microbiome Female urinary suppl\Israniproject10

Check for instruction for how to use LEFse for the analysis!

*Creating the manifest for importing the sequence data:

*For the Dental Microbiome Analysis, take the example from “/Volumes/RAVPOWER/ROOK_PC/MAC_Documents/qiime2/asmic_oral_test/oral_manifest_full.xlsx” and apply it to the metadata sample name structure found at “/Volumes/RAVPOWER/ROOK_PC/MSI2021/Dental_HHRI/plaque_55_samples_analysis/plaque-55-samples-analysis/biom-taxa-otutable-PLA37-metadata.txt” for the files located at “/Volumes/RAVPOWER/ROOK_PC/MSI2021/Dental_HHRI/Israni_Project_010”

*The original mapping files are located at 

/Volumes/RAVPOWER/ROOK_PC/MSI2021/Dental_HHRI/plaque_55_samples_analysis/plaque-55-samples-analysis/51+PLA/map-file-51+PLA.txt

/Volumes/RAVPOWER/ROOK_PC/MSI2021/Dental_HHRI/plaque_55_samples_analysis/plaque-55-samples-analysis/51+PLA/map-file-78.txt	

My issue is that the “mapping file” in the dataset only uses case & control to indicate DKF vs SKF status (In the paper they had 11 DKF and 9 SKF based on plaque samples alone, after excluding the 3 Acute Rejection cases and thus matched the number of cases and control for the cohort variable), and none of the mapping files have all the covariates I need, so I need to merge and consolidate the files to have a master mapping file (all the covariate across the different oral collection sites, so there will be the same data across more than 1 row for patients who had multiple site collections) then I can work from that master mapping file and subset as needed to replicate the analysis. 

For the analysis in the manuscript, we used 20 participants with plaque samples (likely the MapfileCohort_PD and MapfileCohort_PD subclass, although these files are at 23 observations because we had 3 samples from AR patients in the dataset, but these were removed from the subsequent analysis). 

Based on the paper, I also needed to find the mapping file which defined declining kidney function vs stable kidney function. It was located at “W:\Microbiome and Transplant\microbiome Female urinary suppl\David-redcap\dental-plaqque-Scr-data	
It seems like the analysis the latest draft of the paper was based on is located in “
W:\Microbiome and Transplant\microbiome Female urinary suppl\Israniproject10\combinedSKFvsDKF-noAR-Female-only‘’

(Start from the H:\ROOK_PC\MSI2021\Dental_HHRI\Dental_HHRI_og_mapping_files)

**In order to save an excel file as a TSV file, first you have to save it as a TXT (tab delimited file), then open the TXT file with notepad > “Save as” > Select all files > Manually add the .TSV extention at the end


**My manifest file is located at  “/Volumes/RAVPOWER/ROOK_PC/MSI2021/Dental_HHRI/dental_manifest_test.xlsx”

After reaching  out to Amutha, we found the proper PLA files and moved them to  “/Volumes/RAVPOWER/ROOK_PC/MSI2021/Amutha MSI/plaque-HMP” and I need to add these files to  “/Volumes/RAVPOWER/ROOK_PC/MSI2021/Dental_HHRI/Israni_Project_010” with the other files needed for the analysis

**My sequencing files are located at “/Volumes/RAVPOWER/ROOK_PC/MSI2021/Dental_HHRI/Israni_Project_010”

**My mapping file is located at “/Volumes/RAVPOWER/ROOK_PC/MSI2021/Dental_HHRI/Dental_HHRI_og_mapping_files_v4.xlsx” 


**MISSION  16S  QIIME2 data  analysis

Copy the following files to  “/Volumes/RAVPOWER2/Professional Folder/HHRI/23_MISSION 16S Microbiome/Datasets/”

**My manifest file is located at  “/Volumes/RAVPOWER/ROOK_PC/MSI2021/Dental_HHRI/dental_manifest_test.xlsx”

**My mapping file is located at “/Volumes/RAVPOWER/ROOK_PC/MSI2021/Dental_HHRI/Dental_HHRI_og_mapping_files_v4.xlsx” 

The files I am using to create the new manifest and mapping files are located at “/Volumes/RAVPOWER2/ROOK_PC/MSI2022/Israni4_Project_018”

The finalized manifest  is located at “ /Volumes/RAVPOWER2/Professional Folder/HHRI/23_MISSION 16S Microbiome/Datasets/Israni4_Project_018_Manifest.xlsx”

I  used the file  “/Volumes/RAVPOWER2/Professional Folder/HHRI/23_MISSION 16S Microbiome/Datasets/MISSION_16S_sample pull_V3miRNA.xlsx” to create the mapping file, and used the data from  “/Volumes/RAVPOWER2/Professional Folder/HHRI/23_MISSION 16S Microbiome/Datasets/Results_P3_1_MISSION_PK_samples list.xlsx” to differentiate PK samples 

** Be sure to not only rely on  /Volumes/RAVPOWER2/Professional Folder/HHRI/23_MISSION 16S Microbiome/Datasets/MISSION_16S_sample pull_V3miRNA.xlsx because Ajay asked to add some PK samples at the last minute to the sequencing data. Use “/Volumes/RAVPOWER2/Professional Folder/HHRI/23_MISSION 16S Microbiome/Datasets/Israni4_Project_018_MiSeq_Summary.xlsx” to finagle a forward and reverse sequence primer and to crosscheck your manifest!!!

Use the demograhic information  from “/Volumes/RAVPOWER2/Professional Folder/HHRI/19_MISSION_Nsolver_miRNA/Datasets/Beta_glucoronidase_gene_families_renamed_test_mosio_M1M2_nomissing_v4.csv” to complete the mapping file

Also use “/Volumes/RAVPOWER2/Professional Folder/HHRI/19_MISSION_Nsolver_miRNA/Datasets/2_MicrobiomeAndImmunos_DATA_2021-11-15_1357_GOV1.xlsx” for the other samples  demographic information.

Also use the  “/Volumes/RAVPOWER2/Professional Folder/HHRI/23_MISSION 16S Microbiome/Datasets/MISSION study B-gluc.xlsx” to include the latest Cytagen beta glucuronidase activity data into the dataset

Use the  data from   “/Volumes/RAVPOWER2/InVitro_PK correlation/4_MPA_invitro_corr_hilo.csv” to include the PK and HER data  to the mapping file. Actually, use “/Volumes/RAVPOWER2/InVitro_PK correlation/3_MPA_invitro_corr_betaglucfluorometric_doseadjustedpk.csv” 

***Finally use the template from “/Volumes/RAVPOWER2/ROOK_PC/MSI2021/Dental_HHRI/Dental_HHRI_og_mapping_files_v4.xlsx” to format your mapping file properly (remember that your column names shouldn’t have spaces in them and the last column should be called “description”.

You can download this file as tab-separated text by selecting File > Download as > Tab-separated values. Alternatively, the following command will download the sample metadata as tab-separated text and save it in the file sample-metadata.tsv. This sample-metadata.tsv file is used throughout the rest of the tutorial.
Save your final manifest and metadata file as tsv files (From excel, save the file as a tab separated txt file. Then open the file on the notepad, open the file, and then select save as all files. Manually add a “.tsv” to the name of the file.
MAR2023 16S analysis notes

*Creating the manifest for importing the sequence data:

*For the Dental Microbiome Analysis, take the example from “/Volumes/RAVPOWER/ROOK_PC/MAC_Documents/qiime2/asmic_oral_test/oral_manifest_full.xlsx” and apply it to the metadata sample name structure found at “/Volumes/RAVPOWER/ROOK_PC/MSI2021/Dental_HHRI/plaque_55_samples_analysis/plaque-55-samples-analysis/biom-taxa-otutable-PLA37-metadata.txt” for the files located at “/Volumes/RAVPOWER/ROOK_PC/MSI2021/Dental_HHRI/Israni_Project_010”

*For the MAR2023 16 analysis, take example from the manifest file located at “D:\ROOK_02_Professional\HHRI\23_MISSION 16S Microbiome\Datasets\Israni4_Project_018_Manifest”
 
First, copy the files from “D:\ROOK_02_Professional\HHRI\23_MISSION 16S Microbiome\Datasets” to “D:\TEMP_UMGC Stool DNA submission DEC2022\Datasets”. Then, use the “Israni4_Project_018_Manifest” to create a new manifest file for your current data.

The data generated by the samples sent in DEC2022 for this analysis are located in “D:\ROOK_03_PC\Israni4_Project_020” and backed up in “D:\ROOK_03_PC\UMGC Data\2023-q1”

When you open “Israni4_Project_018_Manifest” excel file, you can copy the file name in excel using the steps here (https://www.wps.com/academy/how-to-copy-file-names-from-a-folder-in-excel-quick-tutorials-1863841/). For some reason copying the files in Windows 10 does nos allow me to copy it into excel, so I had do follow the steps  in the link using excel for mac. 

Once you copy the filesnames to excel, reorganize the data into two columns (one for the forward read or R1, and another for the reverse read or R2). One trick to speed up the pathname editing is to replace the root name “FD” from each aliquot with “$PWD/Israni_Project_020/FD” using Ctrl + H and replace all.

Next, to extract the proper sample IDs to match the pathnames in the manifest, copy the file from “D:\TEMP_UMGC Stool DNA submission DEC2022\Documents\20_MAP_Mission Sample Collection Tracking_Master_REDCAP_PKCHECKPREP_Share” and sort by sample ID. This is the list of all the samples that should be included in the final analysis, so use this as the limiting factor for the manifest file. Sort those files from A  to Z and then copy those files to the “Israni4_Project_020_Manifest” Excel File.

Notes: Excluded 3510 FD32021-1 since  the sample ID label was also used for 3504 at M1, and there is a 3510 M2 collection at M2 (FD32029-1)

The sample FD32062 was sent to UMGC but I do not have it in the spreadsheet and will have to look it up in redcap.
$PWD/Israni_Project_020/FD32062-1_S56_L001_R1_001.fastq.gz
$PWD/Israni_Project_020/FD32062-1_S56_L001_R2_001.fastq.gz”

Looks like two samples were sent for “
$PWD/Israni_Project_020/FD450030-3_S144_L001_R1_001.fastq.gz
$PWD/Israni_Project_020/FD450030-3_S144_L001_R2_001.fastq.gz”

Looks like two samples were sent for “
$PWD/Israni_Project_020/FD45067-1_S247_L001_R1_001.fastq.gz
$PWD/Israni_Project_020/FD45067-1_S247_L001_R2_001.fastq.gz”

Looks like two samples were sent for “
$PWD/Israni_Project_020/FD45079-1_S200_L001_R1_001.fastq.gz
$PWD/Israni_Project_020/FD45079-1_S200_L001_R2_001.fastq.gz”

It looks like I messed up the copying of the files names as path names so I had a lot of duplicate names in the excel spreadsheet post sorting. I cross checked with the file names from the UMGC data release folder and I was able to confirm that there were only one sample pair where appropriate for the list above. 

In the excel MAPPING file, in order to include the forward and reverse primer sequences for each sample, you need to use the “Israni4_Project_018_MiSeq_Summary” that you will receive from the UMGC team in the data release email. In that excel file, look at the “ Barcode sequence” column. Sort the sample IDs to match those of the mapping file and merge the 2 files. Then, use an excel formula to separate the forward and reverse primer sequences (i.e. LEFT,5 and RIGHT 5). This will generate 2 different columns for your forward and reverse primer sequences.

**In order to save an excel file as a TSV file, first you have to save it as a TXT (tab delimited file), then open the TXT file with notepad > “Save as” > Select all files > Manually add the .TSV extension at the end















*Additional QIIME2 notes

*** In order to tell which file type you have, you can use the qiime tools command
qiime tools --help
qiime tools peek table.qza
qiime tools peek pkonly-table-l6.qza
**For this test, we need to use a FeatureTable[RelativeeFrequency] (https://docs.qiime2.org/2018.6/plugins/available/feature-table/relative-frequency/)

qiime feature-table relative-frequency \
  --i-table oral-table.qza \
  --o-relative-frequency-table oral-rel-table.qza
qiime longitudinal nmit \
  --i-table oral-rel-table.qza \
  --m-metadata-file Aspirin_Oral_Map.tsv \
  --p-individual-id-column Subject_ID \
  --p-corr-method pearson \
  --o-distance-matrix oral-nmit-dm.qza

***Final notes for the qiime 2 tutorial
 
There are a few file formats that you should be aware of when you are working with qiime2 formats. In order to check the format use the following commands

*** In order to tell which file type you have, you can use the qiime tools command
qiime tools --help
qiime tools peek table.qza
The format analogs are as follows:

OTU table (count features / sample) = FeatureTable[Frequency]
Representative features (rep set by OTU)= FeatureTable[Sequence]
Pseudocount Table for ANCOM (Absolute abundance derived from OTU) = FeatureTable[Comparison] 

Check out this material at https://rpubs.com/aschrecengost/794628

For importing QIIME2 data into R and working with those artifacts

Request from Moataz (04/14/2023)

Thank you Guillaume for sharing this with me.

I am wondering if you have the lineage matrix file.
I want to try converting the data to a phyloseq object and do some analysis using some pipelines in R. I took a microbiome course and I need it to train myself on some real world data.

Thank you so much!
Wish you a wonderful weekend.

Here are the sources I checked

https://rpubs.com/aschrecengost/794628
https://otagoedna.github.io/edna_workshop_june2021/notebook_htmls/06_importing_into_R.html
https://taylorreiter.github.io/2022-08-29-From-raw-metagenome-reads-to-taxonomic-differential-abundance-with-sourmash-and-corncob/
https://github.com/joey711/phyloseq/pull/1253/files
https://github.com/joey711/phyloseq

I shared the “hhri16s-rooted-tree”, “hhri-16s-unrooted-tree” “hhri-16s-rep-seqs” and “hhri-taxonomy” QZA files. 

Based on the lineage files and the document at https://rpubs.com/aschrecengost/794628  I can figure out how to export a file from QIIME2 and using https://otagoedna.github.io/edna_workshop_june2021/notebook_htmls/06_importing_into_R.html to import the file into R/phyloseq. Here is the email reply bellow:

Good afternoon Moataz, 

Here are the requested lineage matrices and the other files you would need to run phyloseq analyses. I have had good success using guides such as "https://rpubs.com/aschrecengost/794628" and  "https://otagoedna.github.io/edna_workshop_june2021/notebook_htmls/06_importing_into_R.html" to modify files for phyloseq.
However, I would recommend starting from scratch from the fasq files as there are always slight changes in formatting from one pipeline to the next (especially since these packages get revised roughly every 6 months). 
The fastq_files are located under Israni_4_project020 in the shared folder of Dr. israni MSI file (which you should have access to, "isran001").

Hope this helps!
Best regards, 

Guillaume

Request from Ajay (04/20/23)

Compare the microbiome at M2 vs M4 for alpha diversity, beta diversity and differential abundance. 

I updated the mapping file at “/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/18_HHRI_UMGC_Projects/04_12192022_Israni4_Project_019and020_16sSequencing/Documents/42_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_nodups.xlsx” to only include the prospective samples, and then created a new variable “time_point” in order to make sure that both the PK and M2 datapoints are coded as “2”. 

I will need to check if the mapping files in QIIME2 allow for any missingness (i.e.  will it only include samples with both M2 and M4, or do we have to properly specify a longitudinal model so that the error due to having the samples from the same individual at different timepoints is properly checked.

I will also include more timepoints where possible in the longitudinal analyses (as Pam will likely ask for that). I set up one dataset for M1M2, another for M2M4 and a last one for M2M4M6. To do so I chose custom sort in excel by Patient_id and then. By Time_point. From there I can exclude any participant that did not fit the mapping file for each longitudinal analysis.

Then I moved the txt files to “/Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets

*05/03/2023: Ajay requested the same M2&M4 Analysis for M4&M6, as well as M2&M6

Note
**You do not need to use a continuous variable in order to vew the PCOA chart, you can just visualize the one that is automatically created when you generate the matrices
qiime tools view /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/pk-sequencing-M2M4M6-only-core-metrics-results/bray_curtis_emperor.qzv
 
	•	I do not want to present just the alpha diversity & beta diversity results with time as a predictor because the observations are longitudinal (i.e. they are paired and correlated) so I checked the following post for a solution
	•	https://forum.qiime2.org/t/p-metric-for-beta-diversity/4415/4


Future steps: Check out the training material at https://chmi-sops.github.io/mydoc_qiime2.html 

for random forest and hierarchical clustering for machine learning modeling techniques for microbiome data



Testing the QIIME 2 tutorial on the asmic data, HHRI dental, HHRI 16S MAR2023 dataset, HHRI 16S NOV2023 dataset and Shotgun import.

Step 1: Importing the asmic oral data (https://docs.qiime2.org/2019.10/tutorials/importing/)

Based on my data description, I have demultiplexed (aka 1 file per sample) paired end sequences (aka each sample has a forward and reverse read )
Casava 1.8 paired-end demultiplexed fastq
Format description
In Casava 1.8 demultiplexed (paired-end) format, there are two fastq.gz files for each sample in the study, each containing the forward or reverse reads for that sample. The file name includes the sample identifier. The forward and reverse read file names for a single sample might look like L2S357_15_L001_R1_001.fastq.gz and L2S357_15_L001_R2_001.fastq.gz, respectively. The underscore-separated fields in this file name are:
	•	the sample identifier,
	•	the barcode sequence or a barcode identifier,
	•	the lane number,
	•	the direction of the read (i.e. R1 or R2), and
	•	the set number.
Activate the conda environment
Now that you have a QIIME 2 environment, activate it using the environment’s name:
source activate qiime2-2019.10
conda info --envs
*After updating my miniconda installations when I installed HUMANN3, I think I need to use the full path name for qiime 2
source activate /Users/guillaumeonyeaghala/miniconda2/envs/qiime2-2019.10
FEB2024
Testing the shotgun import tool for Q2 found at “https://github.com/gregcaporaso/q2-sapienns”
I will use qiime2-2019.10_snic as my test environment for the shotgun plugin
source activate /Users/guillaumeonyeaghala/miniconda3/envs/qiime2-2019.10_snic
*It looks like it might be a better move to install Qiime 2024.2 instead as it includes the shotgun plugin

https://docs.qiime2.org/2024.2/install/
https://docs.qiime2.org/2024.2/install/native/

Natively installing QIIME 2
This guide describes how to natively install the available QIIME 2 2024.2 distributions.
Miniconda
Installing Miniconda
Miniconda provides the conda environment and package manager, and is the recommended way to install QIIME 2. Follow the Miniconda instructions for downloading and installing Miniconda. You may choose either Miniconda2 or Miniconda3 (i.e. Miniconda Python 2 or 3). QIIME 2 will work with either version of Miniconda. It is important to follow all of the directions provided in the Miniconda instructions, particularly ensuring that you run conda init at the end of the installation process, to ensure that your Miniconda installation is fully installed and available for the following commands.
Updating Miniconda
After installing Miniconda and opening a new terminal, make sure you’re running the latest version of conda:
conda update conda
Installing wget
conda install wget
Install QIIME 2 within a conda environment
Once you have Miniconda installed, create a conda environment and install the QIIME 2 2024.2 distribution of your choice within the environment. We highly recommend creating a new environment specifically for the QIIME 2 distribution and release being installed, as there are many required dependencies that you may not want added to an existing environment. You can choose whatever name you’d like for the environment. In this example, we’ll name the environments qiime2-<distro>-2024.2 to indicate what QIIME 2 release is installed (i.e. 2024.2).
QIIME 2 Amplicon Distribution
	•	Instructions
	•	macOS (Intel) and OS X
	•	macOS (Apple Silicon)
	•	Linux
	•	Windows (via WSL)
wget https://data.qiime2.org/distro/amplicon/qiime2-amplicon-2024.2-py38-osx-conda.yml
conda env create -n qiime2-amplicon-2024.2 --file qiime2-amplicon-2024.2-py38-osx-conda.yml
OPTIONAL CLEANUP
rm qiime2-amplicon-2024.2-py38-osx-conda.yml
QIIME 2 Shotgun Distribution
	•	Instructions
	•	macOS (Intel) and OS X
	•	macOS (Apple Silicon)
	•	Linux
	•	Windows (via WSL)
wget https://data.qiime2.org/distro/shotgun/qiime2-shotgun-2024.2-py38-osx-conda.yml
conda env create -n qiime2-shotgun-2024.2 --file qiime2-shotgun-2024.2-py38-osx-conda.yml
**Need to use curl instead of wget (weird, my shell got mad at me using curl so back to wget we go)
https://stackoverflow.com/questions/4572153/os-x-equivalent-of-linuxs-wget
curl https://data.qiime2.org/distro/shotgun/qiime2-shotgun-2024.2-py38-osx-conda.yml
wget https://data.qiime2.org/distro/shotgun/qiime2-shotgun-2024.2-py38-osx-conda.yml
conda env create -n qiime2-shotgun-2024.2 --file qiime2-shotgun-2024.2-py38-osx-conda.yml

To deactivate an environment, run source deactivate.
Before beginning this tutorial, create a new directory called asmic_oral_test and change to that directory.
pwd
Move to the documents folder
cd /Volumes/RAVPOWER/ROOK_PC/MAC_Documents/V1_V3_Gut
cd /Volumes/RAVPOWER/ROOK_PC/MSI2021/Dental_HHRI
cd /Volumes/RAVPOWER2/ROOK_PC/MSI2021/Dental_HHRI



*16S dataset location (right click on the folder location, and then hold the option key to be able to select “copy as a pathname”)
$PWD/Israni_Project_018/F32002_1_S7_R1_001.fastq.gz
cd /Volumes/RAVPOWER2/ROOK_PC/MSI2022/Israni4_Project_018
cd /Volumes/RAVPOWER2/ROOK_PC/MSI2022/Israni4_Project_018_Datasets
cd /Volumes/RAVPOWER2/ROOK_03_PC/Israni4_Project_020_Datasets
/Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets	
cd /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets

Testing the QIIME 2 tutorial on the asmic data, HHRI dental, HHRI 16S MAR2023 dataset

Step 1: Importing the asmic oral data (https://docs.qiime2.org/2019.10/tutorials/importing/)

Based on my data description, I have demultiplexed (aka 1 file per sample) paired end sequences (aka each sample has a forward and reverse read )
Importing data(use the name of the folder with your sequences)
** The format option are prespecified as seen (https://forum.qiime2.org/t/importing-data-and-input-format/6270)

This updated manifest version has the $PWD/oral-test/filename for absolute path, instead of /Users/guillaumeonyeaghala/Documents/qiime2/asmic_oral_test /oral_test/filename as the path in the manifest and was successful in importing the data.Luckily, it does seem to accept both .fastq and .fasq.gz files (it doesn’t care about compression, similar to QIIME1 and DADA2)

qiime tools import \
  --type 'SampleData[PairedEndSequencesWithQuality]' \
  --input-path oral_gut_manifest_full.tsv \
  --input-format PairedEndFastqManifestPhred33V2 \
  --output-path oral-demux-paired-end.qza

qiime tools import \
  --type 'SampleData[PairedEndSequencesWithQuality]' \
  --input-path dental_manifest_test_pla_norec.txt \
  --input-format PairedEndFastqManifestPhred33V2 \
  --output-path hhridental-demux-paired-end.qza

*I was not able to run the manifest using the Israni4_project18 filepath, so I retried copying the fastq files to the location of the dental microbiome folder
qiime tools import \
  --type 'SampleData[PairedEndSequencesWithQuality]' \
  --input-path Israni4_Project_018_Manifest.txt \
  --input-format PairedEndFastqManifestPhred33V2 \
  --output-path hhri16s-demux-paired-end.qza

*I Moved the folder with the sequence data within the cd path. It worked
qiime tools import \
  --type 'SampleData[PairedEndSequencesWithQuality]' \
  --input-path Israni4_Project_018_Manifest.txt \
  --input-format PairedEndFastqManifestPhred33V2 \
  --output-path hhri16s-demux-paired-end.qza

*16S MAR2023
*This time around, I moved the manifest txt file to a folder called Israni4_Project_020_Datasets, and moved the folder where the sequence files are located (/Volumes/RAVPOWER2/ROOK_03_PC/Israni4_Project_020) to that folder as well

qiime tools import \
  --type 'SampleData[PairedEndSequencesWithQuality]' \
  --input-path Israni4_Project_020_ManifestV3.txt \
  --input-format PairedEndFastqManifestPhred33V2 \
  --output-path hhri16s-demux-paired-end.qza

*16S NOV2023
*This time around, I moved the manifest txt file to a folder called Israni4_Project_020_Datasets, and moved the folder where the sequence files are located (/Volumes/RAVPOWER2/ROOK_03_PC/Israni4_Project_020) to that folder as well

qiime tools import \
  --type 'SampleData[PairedEndSequencesWithQuality]' \
  --input-path Israni4_Project_020_ManifestV3.txt \
  --input-format PairedEndFastqManifestPhred33V2 \
  --output-path hhri16s-demux-paired-end.qza


After demultiplexing, it’s useful to generate a summary of the demultiplexing results. This allows you to determine how many sequences were obtained per sample, and also to get a summary of the distribution of sequence qualities at each position in your sequence data.
qiime demux summarize \
  --i-data oral-demux-paired-end.qza \
  --o-visualization oral-gut-demux.qzv

qiime demux summarize \
  --i-data hhridental-demux-paired-end.qza \
  --o-visualization hhridental-demux.qzv

qiime demux summarize \
  --i-data hhri16s-demux-paired-end.qza \
  --o-visualization hhri16s-demux.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view oral-gut-demux.qzv
qiime tools view hhridental-demux.qzv
qiime tools view hhri16s-demux.qzv
Alternatively, you can view QIIME 2 artifacts and visualizations at view.qiime2.org by uploading files or providing URLs. There are also precomputed results that can be viewed or downloaded after each step in the tutorial. These can be used if you’re reading the tutorial, but not running the commands yourself.
Sequence quality control and feature table construction
QIIME 2 plugins are available for several quality control methods, including DADA2, Deblur, and basic quality-score-based filtering. In this tutorial we present this step using DADA2 and Deblur. These steps are interchangeable, so you can use whichever of these you prefer. The result of both of these methods will be a FeatureTable[Frequency] QIIME 2 artifact, which contains counts (frequencies) of each unique sequence in each sample in the dataset, and a FeatureData[Sequence] QIIME 2 artifact, which maps feature identifiers in the FeatureTable to the sequences they represent.
Note
As you work through one or both of the options in this section, you’ll create artifacts with filenames that are specific to the method that you’re running (e.g., the feature table that you generate with dada2 denoise-single will be called table-dada2.qza). After creating these artifacts you’ll rename the artifacts from one of the two options to more generic filenames (e.g., table.qza). This process of creating a specific name for an artifact and then renaming it is only done to allow you to choose which of the two options you’d like to use for this step, and then complete the tutorial without paying attention to that choice again. It’s important to note that in this step, or any step in QIIME 2, the filenames that you’re giving to artifacts or visualizations are not important.
QIIME 1 Users
The FeatureTable[Frequency] QIIME 2 artifact is the equivalent of the QIIME 1 OTU or BIOM table, and the FeatureData[Sequence] QIIME 2 artifact is the equivalent of the QIIME 1 representative sequences file. Because the “OTUs” resulting from DADA2 and Deblur are created by grouping unique sequences, these are the equivalent of 100% OTUs from QIIME 1, and are generally referred to as sequence variants. In QIIME 2, these OTUs are higher resolution than the QIIME 1 default of 97% OTUs, and they’re higher quality since these quality control steps are better than those implemented in QIIME 1. This should therefore result in more accurate estimates of diversity and taxonomic composition of samples than was achieved with QIIME 1.
Option 1: DADA2
DADA2 is a pipeline for detecting and correcting (where possible) Illumina amplicon sequence data. As implemented in the q2-dada2 plugin, this quality control process will additionally filter any phiX reads (commonly present in marker gene Illumina sequence data) that are identified in the sequencing data, and will filter chimeric sequences.
The dada2 denoise-single method requires two parameters that are used in quality filtering: --p-trim-left m, which trims off the first m bases of each sequence, and --p-trunc-len n which truncates each sequence at position n. This allows the user to remove low quality regions of the sequences. To determine what values to pass for these two parameters, you should review the Interactive Quality Plot tab in the demux.qzv file that was generated by qiime demux summarize above.
Question
Based on the plots you see in demux.qzv, what values would you choose for --p-trunc-len and --p-trim-left in this case?
In the demux.qzv quality plots, we see that the quality of the initial bases seems to be high, so we won’t trim any bases from the beginning of the sequences. The quality seems to drop off around position 120, so we’ll truncate our sequences at 120 bases. This next command may take up to 10 minutes to run, and is the slowest step in this tutorial.
Output artifacts:
qiime dada2 denoise-single \
  --i-demultiplexed-seqs oral-demux-paired-end.qza \
  --p-trim-left 0 \
  --p-trunc-len 250 \
  --o-representative-sequences oral-gut-rep-seqs-dada2.qza \
  --o-table oral-gut-table-dada2.qza \
  --o-denoising-stats oral-gut-stats-dada2.qza

qiime dada2 denoise-single \
  --i-demultiplexed-seqs hhridental-demux-paired-end.qza \
  --p-trim-left 0 \
  --p-trunc-len 240 \
  --o-representative-sequences hhridental-rep-seqs-dada2.qza \
  --o-table hhridental-table-dada2.qza \
  --o-denoising-stats hhridental-stats-dada2.qza

*16S MAR2023
qiime dada2 denoise-single \
  --i-demultiplexed-seqs hhri16s-demux-paired-end.qza \
  --p-trim-left 10 \
  --p-trunc-len 250 \
  --o-representative-sequences hhri16s-rep-seqs-dada2.qza \
  --o-table hhri16s-table-dada2.qza \
  --o-denoising-stats hhri16s-stats-dada2.qza

*16S NOV2023
qiime dada2 denoise-single \
  --i-demultiplexed-seqs hhri16s-demux-paired-end.qza \
  --p-trim-left 10 \
  --p-trunc-len 250 \
  --o-representative-sequences hhri16s-rep-seqs-dada2.qza \
  --o-table hhri16s-table-dada2.qza \
  --o-denoising-stats hhri16s-stats-dada2.qza
If you’d like to continue the tutorial using this FeatureTable (opposed to the Deblur feature table generated in Option 2), run the following commands.
mv oral-gut-rep-seqs-dada2.qza oral-gut-rep-seqs.qza
mv oral-gut-table-dada2.qza oral-gut-table.qza

mv hhridental-rep-seqs-dada2.qza hhridental-rep-seqs.qza
mv hhridental-table-dada2.qza hhridental-table.qza

mv hhri16s-rep-seqs-dada2.qza hhri16s-rep-seqs.qza
mv hhri16s-table-dada2.qza hhri16s-table.qza

*** This command will rename the rep-seqs-dada2.qza and the tabe-dada2.qza files to the new names
mv hhri16s-rep-seqs-dada2.qza hhri16s-rep-seqs.qza
mv hhri16s-table-dada2.qza hhri16s-table.qza
FeatureTable and FeatureData summaries
After the quality filtering step completes, you’ll want to explore the resulting data. You can do this using the following two commands, which will create visual summaries of the data. The feature-table summarize command will give you information on how many sequences are associated with each sample and with each feature, histograms of those distributions, and some related summary statistics. The feature-table tabulate-seqs command will provide a mapping of feature IDs to sequences, and provide links to easily BLAST each sequence against the NCBI nt database. The latter visualization will be very useful later in the tutorial, when you want to learn more about specific features that are important in the data set.
qiime feature-table summarize \
  --i-table oral-gut-table.qza \
  --o-visualization oral-gut-table.qzv \
  --m-sample-metadata-file Aspirin_Oral_Gut_Map.tsv
qiime feature-table tabulate-seqs \
  --i-data oral-gut-rep-seqs.qza \
  --o-visualization oral-gut-rep-seqs.qzv

qiime feature-table summarize \
  --i-table hhridental-table.qza \
  --o-visualization hhridental	-table.qzv \
  --m-sample-metadata-file Dental_HHRI_og_mapping_files_v3.txt
qiime feature-table tabulate-seqs \
  --i-data hhridental-rep-seqs.qza \
  --o-visualization hhridental-rep-seqs.qzv

*I had to make sure the file name had no spaces, and that variable names do not repeat
qiime feature-table summarize \
  --i-table hhri16s-table.qza \
  --o-visualization hhri16s-table.qzv \
  --m-sample-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt

qiime feature-table tabulate-seqs \
  --i-data hhri16s-rep-seqs.qza \
  --o-visualization hhri16s-rep-seqs.qzv

*16S MAR2023
*I had to make sure the file name had no spaces, and that variable names do not repeat
qiime feature-table summarize \
  --i-table hhri16s-table.qza \
  --o-visualization hhri16s-table.qzv \
  --m-sample-metadata-file 28_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023.txt

qiime feature-table tabulate-seqs \
  --i-data hhri16s-rep-seqs.qza \
  --o-visualization hhri16s-rep-seqs.qzv

*16S NOV2023
*I had to make sure the file name had no spaces, and that variable names do not repeat
qiime feature-table summarize \
  --i-table hhri16s-table.qza \
  --o-visualization hhri16s-table.qzv \
  --m-sample-metadata-file 48_MAP_MISSION_16S_sample_pull_mappingtemplate_NOV2023_PKMAP_nodups_V2.txt

qiime feature-table tabulate-seqs \
  --i-data hhri16s-rep-seqs.qza \
  --o-visualization hhri16s-rep-seqs.qzv

I had to update the following sample-ids in the metadata file above, so I had to add them as dummy sample IDs for the analysis
The following IDs are not present in the metadata: 'FD32005-1-2', 'FD32007-1-2', 'FD32016-1-2', 'FD32018-1-2', 'FD32127-1-2', 'FD45004-1-2', 'FD45005-1-2', 'FD45006-1-2', 'FD45012-2', 'FD45017-1-2', 'FD45019-1-2', 'FD45020-1-2', 'FD45039-1-2', 'FD45041-1', 'FD45044-1-1', 'FD45044-1-2', 'FD45046-1-2'

*I also went back and made edits to the mapping file to include all the additional processed PK data generated between MAR2023 and NOV2023. The file was moved to /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/48_MAP_MISSION_16S_MISSION PK and Demographics n =84 1026203 with revisions no ht and wt and crcl_MPAT0.csv and used to create /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/48_MAP_MISSION_16S_sample_pull_mappingtemplate_NOV2023_PKMAP_nodups_V4.txt
In short, I deleted the previous MOATAZPK columns, and after removing all the “.” From the variable names in the dataset that mataz generated, I then matched the data from the N=84 dataset to the mapping file by PID at each timepoint.
*16S NOV2023
*I had to make sure the file name had no spaces, and that variable names do not repeat
qiime feature-table summarize \
  --i-table hhri16s-table.qza \
  --o-visualization hhri16s-table.qzv \
  --m-sample-metadata-file 48_MAP_MISSION_16S_sample_pull_mappingtemplate_NOV2023_PKMAP_nodups_V3.txt

qiime feature-table summarize \
  --i-table hhri16s-table.qza \
  --o-visualization hhri16s-table.qzv \
  --m-sample-metadata-file 48_MAP_MISSION_16S_sample_pull_mappingtemplate_NOV2023_PKMAP_nodups_V4.txt

qiime feature-table tabulate-seqs \
  --i-data hhri16s-rep-seqs.qza \
  --o-visualization hhri16s-rep-seqs.qzv

Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view oral-gut-table.qzv
qiime tools view oral-gut-rep-seqs.qzv

qiime tools view hhridental--table.qzv
qiime tools view hhridental-rep-seqs.qzv

qiime tools view hhri16s-table.qzv
qiime tools view hhri16s-rep-seqs.qzv
Generate a tree for phylogenetic diversity analyses
QIIME supports several phylogenetic diversity metrics, including Faith’s Phylogenetic Diversity and weighted and unweighted UniFrac. In addition to counts of features per sample (i.e., the data in the FeatureTable[Frequency] QIIME 2 artifact), these metrics require a rooted phylogenetic tree relating the features to one another. This information will be stored in a Phylogeny[Rooted] QIIME 2 artifact. To generate a phylogenetic tree we will use align-to-tree-mafft-fasttree pipeline from the q2-phylogeny plugin.
First, the pipeline uses the mafft program to perform a multiple sequence alignment of the sequences in our FeatureData[Sequence] to create a FeatureData[AlignedSequence] QIIME 2 artifact. Next, the pipeline masks (or filters) the alignment to remove positions that are highly variable. These positions are generally considered to add noise to a resulting phylogenetic tree. Following that, the pipeline applies FastTree to generate a phylogenetic tree from the masked alignment. The FastTree program creates an unrooted tree, so in the final step in this section midpoint rooting is applied to place the root of the tree at the midpoint of the longest tip-to-tip distance in the unrooted tree.
qiime phylogeny align-to-tree-mafft-fasttree \
  --i-sequences oral-gut-rep-seqs.qza \
  --o-alignment aligned-oral-gut-rep-seqs.qza \
  --o-masked-alignment masked-aligned-oral-gut-rep-seqs.qza \
  --o-tree oral-gut-unrooted-tree.qza \
  --o-rooted-tree oral-gut-rooted-tree.qza
ls

qiime phylogeny align-to-tree-mafft-fasttree \
  --i-sequences hhridental-rep-seqs.qza \
  --o-alignment aligned-hhridental-rep-seqs.qza \
  --o-masked-alignment masked-aligned-hhridental-rep-seqs.qza \
  --o-tree hhridental-unrooted-tree.qza \
  --o-rooted-tree hhridental-rooted-tree.qza
ls

qiime phylogeny align-to-tree-mafft-fasttree \
  --i-sequences hhri16s-rep-seqs.qza \
  --o-alignment aligned-hhri16s-rep-seqs.qza \
  --o-masked-alignment masked-aligned-hhri16s-rep-seqs.qza \
  --o-tree hhri16s-unrooted-tree.qza \
  --o-rooted-tree hhri16s-rooted-tree.qza
ls

*16S MAR2023 
qiime phylogeny align-to-tree-mafft-fasttree \
  --i-sequences hhri16s-rep-seqs.qza \
  --o-alignment aligned-hhri16s-rep-seqs.qza \
  --o-masked-alignment masked-aligned-hhri16s-rep-seqs.qza \
  --o-tree hhri16s-unrooted-tree.qza \
  --o-rooted-tree hhri16s-rooted-tree.qza
ls

*16S NOV2023 
qiime phylogeny align-to-tree-mafft-fasttree \
  --i-sequences hhri16s-rep-seqs.qza \
  --o-alignment aligned-hhri16s-rep-seqs.qza \
  --o-masked-alignment masked-aligned-hhri16s-rep-seqs.qza \
  --o-tree hhri16s-unrooted-tree.qza \
  --o-rooted-tree hhri16s-rooted-tree.qza
ls



Alpha and beta diversity analysis
QIIME 2’s diversity analyses are available through the q2-diversity plugin, which supports computing alpha and beta diversity metrics, applying related statistical tests, and generating interactive visualizations. We’ll first apply the core-metrics-phylogenetic method, which rarefies a FeatureTable[Frequency] to a user-specified depth, computes several alpha and beta diversity metrics, and generates principle coordinates analysis (PCoA) plots using Emperor for each of the beta diversity metrics. The metrics computed by default are:
	•	Alpha diversity
	•	Shannon’s diversity index (a quantitative measure of community richness)
	•	Observed OTUs (a qualitative measure of community richness)
	•	Faith’s Phylogenetic Diversity (a qualitiative measure of community richness that incorporates phylogenetic relationships between the features)
	•	Evenness (or Pielou’s Evenness; a measure of community evenness)
	•	Beta diversity
	•	Jaccard distance (a qualitative measure of community dissimilarity)
	•	Bray-Curtis distance (a quantitative measure of community dissimilarity)
	•	unweighted UniFrac distance (a qualitative measure of community dissimilarity that incorporates phylogenetic relationships between the features)
	•	weighted UniFrac distance (a quantitative measure of community dissimilarity that incorporates phylogenetic relationships between the features)
An important parameter that needs to be provided to this script is --p-sampling-depth, which is the even sampling (i.e. rarefaction) depth. Because most diversity metrics are sensitive to different sampling depths across different samples, this script will randomly subsample the counts from each sample to the value provided for this parameter. For example, if you provide --p-sampling-depth 500, this step will subsample the counts in each sample without replacement so that each sample in the resulting table has a total count of 500. If the total count for any sample(s) are smaller than this value, those samples will be dropped from the diversity analysis. Choosing this value is tricky. We recommend making your choice by reviewing the information presented in the table.qzv file that was created above. Choose a value that is as high as possible (so you retain more sequences per sample) while excluding as few samples as possible.
Alpha Diversity
Question
View the table.qzv QIIME 2 artifact, and in particular the Interactive Sample Detail tab in that visualization. What value would you choose to pass for --p-sampling-depth? How many samples will be excluded from your analysis based on this choice? How many total sequences will you be analyzing in the core-metrics-phylogenetic command?
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny oral-gut-rooted-tree.qza \
  --i-table oral-gut-table.qza \
  --p-sampling-depth 6000 \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --output-dir oral-gut-core-metrics-results
ls
**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls oral-gut-core-metrics-results/
**Getting the Shannon value for all the sample, then creating a new mapping file so you can use that as a covariate
qiime diversity alpha-group-significance \
  --i-alpha-diversity oral-gut-core-metrics-results/shannon_vector.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --o-visualization oral-gut-core-metrics-results/shannon-group-significance.qzv
**You need to visualize the file then export the raw values as TSV
qiime tools view oral-gut-core-metrics-results/shannon-group-significance.qzv

qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhridental-rooted-tree.qza \
  --i-table hhridental-table.qza \
  --p-sampling-depth 25000 \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --output-dir hhridental-core-metrics-results

*16s sequencing dataset
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table hhri16s-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --output-dir hhri16s-core-metrics-results

**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls hhri16s-core-metrics-results/

**Getting the Shannon value for all the sample, then creating a new mapping file so you can use that as a covariate
qiime diversity alpha-group-significance \
  --i-alpha-diversity hhri16s-core-metrics-results/shannon_vector.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --o-visualization hhri16s-core-metrics-results/shannon-group-significance.qzv

**You need to visualize the file then export the raw values as TSV
qiime tools view hhri16s-core-metrics-results/shannon-group-significance.qzv
Here we set the --p-sampling-depth parameter to 1103. This value was chosen based on the number of sequences in the L3S313 sample because it’s close to the number of sequences in the next few samples that have higher sequence counts, and because it is considerably higher (relatively) than the number of sequences in the samples that have fewer sequences. This will allow us to retain most of our samples. The three samples that have fewer sequences will be dropped from the core-metrics-phylogenetic analyses and anything that uses these results. It is worth noting that all three of these samples are “right palm” samples. Losing a disproportionate number of samples from one metadata category is not ideal. However, we are dropping a small enough number of samples here that this felt like the best compromise between total sequences analyzed and number of samples retained.
Note
The sampling depth of 1103 was chosen based on the DADA2 feature table summary. If you are using a Deblur feature table rather than a DADA2 feature table, you might want to choose a different even sampling depth. Apply the logic from the previous paragraph to help you choose an even sampling depth.
Note
In many Illumina runs you’ll observe a few samples that have very low sequence counts. You will typically want to exclude those from the analysis by choosing a larger value for the sampling depth at this stage.
After computing diversity metrics, we can begin to explore the microbial composition of the samples in the context of the sample metadata. This information is present in the sample metadata file that was downloaded earlier.
We’ll first test for associations between categorical metadata columns and alpha diversity data. We’ll do that here for the Faith Phylogenetic Diversity (a measure of community richness) and evenness metrics.
Try Alpha diversity Measures at baseline and post intervention (also in aspirin arm vs placebo arms for pairwise test comparisons) 

qiime feature-table filter-samples \
  --i-table oral-gut-table.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --p-where "[Collection] = '1' " \
  --o-filtered-table oral-gut-pre-table.qza

qiime feature-table filter-samples \
  --i-table oral-gut-pre-table.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --p-where "[Site] = 'Oral' " \
  --o-filtered-table oral-only-pre-table.qza

qiime feature-table filter-samples \
  --i-table oral-gut-pre-table.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --p-where "[Site] = 'Gut' " \
  --o-filtered-table gut-only-pre-table.qza

qiime feature-table filter-samples \
  --i-table oral-gut-table.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --p-where "[Collection] = '2' " \
  --o-filtered-table oral-gut-post-table.qza

qiime feature-table filter-samples \
  --i-table oral-gut-post-table.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --p-where "[Site] = 'Oral' " \
  --o-filtered-table oral-only-post-table.qza

qiime feature-table filter-samples \
  --i-table oral-gut-pre-table.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --p-where "[Site] = 'Gut' " \
  --o-filtered-table gut-only-post-table.qza

qiime feature-table filter-samples \
  --i-table oral-gut-table.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --p-where "[Contents] = 'Aspirin' " \
  --o-filtered-table oral-gut-asa-table.qza

qiime feature-table filter-samples \
  --i-table oral-gut-table.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --p-where "[Contents] = 'Placebo' " \
  --o-filtered-table oral-gut-pcb-table.qza

qiime feature-table filter-samples \
  --i-table oral-gut-table.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --p-where "[Site] = 'Gut' " \
  --o-filtered-table gut-only-table.qza

*16S dataset, subsetting by PK samples to make barplot for high vs low invitro

qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --p-where "[Group] = 'PK' " \
  --o-filtered-table pk-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny oral-gut-rooted-tree.qza \
  --i-table oral-only-pre-table.qza \
  --p-sampling-depth 6000 \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --output-dir oral-only-pre-core-metrics-results

qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --output-dir pk-only-core-metrics-results

**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls pk-only-core-metrics-results/

**Getting the Shannon value for all the sample, then creating a new mapping file so you can use that as a covariate
qiime diversity alpha-group-significance \
  --i-alpha-diversity pk-only-core-metrics-results/shannon_vector.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --o-visualization pk-only-core-metrics-results/shannon-group-significance.qzv

**You need to visualize the file then export the raw values as TSV
qiime tools view pk-only-core-metrics-results/shannon-group-significance.qzv
Question
View the table.qzv QIIME 2 artifact, and in particular the Interactive Sample Detail tab in that visualization. What value would you choose to pass for --p-sampling-depth? How many samples will be excluded from your analysis based on this choice? How many total sequences will you be analyzing in the core-metrics-phylogenetic command?
***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny oral-gut-rooted-tree.qza \
  --i-table oral-gut-pre-table.qza \
  --p-sampling-depth 6000 \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --output-dir oral-gut-pre-core-metrics-results
**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls oral-gut-pre-core-metrics-results/

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny oral-gut-rooted-tree.qza \
  --i-table oral-only-pre-table.qza \
  --p-sampling-depth 6000 \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --output-dir oral-only-pre-core-metrics-results
**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls oral-only-pre-core-metrics-results/

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny oral-gut-rooted-tree.qza \
  --i-table gut-only-pre-table.qza \
  --p-sampling-depth 6000 \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --output-dir gut-only-pre-core-metrics-results
**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls gut-only-pre-core-metrics-results/

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny oral-gut-rooted-tree.qza \
  --i-table oral-gut-post-table.qza \
  --p-sampling-depth 6000 \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --output-dir oral-gut-post-core-metrics-results
**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls oral-gut-post-core-metrics-results/

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny oral-gut-rooted-tree.qza \
  --i-table oral-only-post-table.qza \
  --p-sampling-depth 6000 \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --output-dir oral-only-post-core-metrics-results
**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls oral-only-post-core-metrics-results/

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny oral-gut-rooted-tree.qza \
  --i-table gut-only-post-table.qza \
  --p-sampling-depth 6000 \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --output-dir gut-only-post-core-metrics-results
**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls gut-only-post-core-metrics-results/

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny oral-gut-rooted-tree.qza \
  --i-table oral-gut-asa-table.qza \
  --p-sampling-depth 6000 \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --output-dir oral-gut-asa-core-metrics-results
**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls oral-gut-asa-core-metrics-results/

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny oral-gut-rooted-tree.qza \
  --i-table oral-gut-pcb-table.qza \
  --p-sampling-depth 6000 \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --output-dir oral-gut-pcb-core-metrics-results
**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls oral-gut-pcb-core-metrics-results/

qiime diversity core-metrics-phylogenetic \
  --i-phylogeny oral-gut-rooted-tree.qza \
  --i-table gut-only-table.qza \
  --p-sampling-depth 6000 \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --output-dir gut-only-core-metrics-results
**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls gut-only-core-metrics-results/

*Making a filtered HHRI dental dataset without AR for the overall microbiome composition by site (note the exclude ids option)

qiime feature-table filter-samples \
  --i-table hhridental-table.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --p-where "[Cohort1] = 'AR1' " \
  --p-exclude-ids \
  --o-filtered-table hhridental-noAR1-table.qza

qiime feature-table summarize \
  --i-table hhridental-noAR1-table.qza \
  --o-visualization hhridental-noAR1-table.qzv \
  --m-sample-metadata-file Dental_HHRI_og_mapping_files_v3.txt

qiime tools view hhridental-noAR1-table.qzv

qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhridental-rooted-tree.qza \
  --i-table hhridental-noAR1-table.qza \
  --p-sampling-depth 25000 \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --output-dir hhridental-noAR1-core-metrics-results2

*Making a filtered HHRI dental dataset without AR for the overall microbiome composition by site

qiime feature-table filter-samples \
  --i-table hhridental-noAR1-table.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --p-where "[bodysite] = 'PLA' " \
  --o-filtered-table hhridental-PLA-table.qza

qiime tools peek hhridental-PLA-table.qza

qiime feature-table summarize \
  --i-table hhridental-PLA-table.qza \
  --o-visualization hhridental-PLA-table.qzv \
  --m-sample-metadata-file Dental_HHRI_og_mapping_files_v3.txt

qiime tools view hhridental-PLA-table.qzv
*Since the filtering by body site was not working, I converted that variable to a numeric one for testing purposes; After some testing, it turns out that none of the sequence files have the “P” code for plaque, so the isrania10 folder did not have any plaque samples.	

qiime feature-table filter-samples \
  --i-table hhridental-table.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v4.txt \
  --p-where "[Site] = '4' " \
  --o-filtered-table hhridental-PLA-table.qza

qiime tools peek hhridental-PLA-table.qza

qiime feature-table summarize \
  --i-table hhridental-PLA-table.qza \
  --o-visualization hhridental-PLA-table.qzv \
  --m-sample-metadata-file Dental_HHRI_og_mapping_files_v4.txt

qiime tools view hhridental-PLA-table.qzv

qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhridental-rooted-tree.qza \
  --i-table hhridental-PLA-table.qza \
  --p-sampling-depth 25000 \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --output-dir hhridental-PLA-core-metrics-results

qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhridental-rooted-tree.qza \
  --i-table hhridental-PLA-table.qza \
  --p-sampling-depth 25000 \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --output-dir hhridental-PLA-core-metrics-results
We’ll first test for associations between categorical metadata columns and alpha diversity data. We’ll do that here for the Faith Phylogenetic Diversity (a measure of community richness) and evenness metrics.
qiime diversity alpha-group-significance \
  --i-alpha-diversity oral-gut-pre-core-metrics-results/shannon_vector.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --o-visualization oral-gut-pre-core-metrics-results/shannon-group-significance.qzv

qiime diversity alpha-group-significance \
  --i-alpha-diversity oral-only-pre-core-metrics-results/shannon_vector.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --o-visualization oral-only-pre-core-metrics-results/shannon-group-significance.qzv

qiime diversity alpha-group-significance \
  --i-alpha-diversity gut-only-pre-core-metrics-results/shannon_vector.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --o-visualization gut-only-pre-core-metrics-results/shannon-group-significance.qzv

qiime diversity alpha-group-significance \
  --i-alpha-diversity oral-gut-post-core-metrics-results/shannon_vector.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --o-visualization oral-gut-post-core-metrics-results/shannon-group-significance.qzv

qiime diversity alpha-group-significance \
  --i-alpha-diversity oral-only-post-core-metrics-results/shannon_vector.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --o-visualization oral-only-post-core-metrics-results/shannon-group-significance.qzv

qiime diversity alpha-group-significance \
  --i-alpha-diversity gut-only-post-core-metrics-results/shannon_vector.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --o-visualization gut-only-post-core-metrics-results/shannon-group-significance.qzv

qiime diversity alpha-group-significance \
  --i-alpha-diversity oral-gut-asa-core-metrics-results/shannon_vector.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --o-visualization oral-gut-asa-core-metrics-results/shannon-group-significance.qzv

qiime diversity alpha-group-significance \
  --i-alpha-diversity oral-gut-pcb-core-metrics-results/shannon_vector.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --o-visualization oral-gut-pcb-core-metrics-results/shannon-group-significance.qzv

qiime diversity alpha-group-significance \
  --i-alpha-diversity gut-only-core-metrics-results/shannon_vector.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --o-visualization gut-only-core-metrics-results/shannon-group-significance.qzv

*Repeating the alpha group significance analysis for the dental microbiome paper

qiime diversity alpha-group-significance \
  --i-alpha-diversity hhridental-noAR1-core-metrics-results2/shannon_vector.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --o-visualization hhridental-noAR1-core-metrics-results2/shannon-group-significance.qzv
 
qiime diversity alpha-group-significance \
  --i-alpha-diversity hhridental-PLA-core-metrics-results/shannon_vector.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --o-visualization hhridental-PLA-core-metrics-results/shannon-group-significance.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view oral-gut-pre-core-metrics-results/shannon-group-significance.qzv
qiime tools view oral-only-pre-core-metrics-results/shannon-group-significance.qzv
qiime tools view gut-only-pre-core-metrics-results/shannon-group-significance.qzv
qiime tools view oral-gut-post-core-metrics-results/shannon-group-significance.qzv
qiime tools view oral-only-post-core-metrics-results/shannon-group-significance.qzv
qiime tools view gut-only-post-core-metrics-results/shannon-group-significance.qzv
qiime tools view oral-gut-asa-core-metrics-results/shannon-group-significance.qzv
qiime tools view oral-gut-pcb-core-metrics-results/shannon-group-significance.qzv
qiime tools view gut-only-core-metrics-results/shannon-group-significance.qzv

qiime tools view hhridental-noAR1-core-metrics-results2/shannon-group-significance.qzv
qiime tools view hhridental-PLA-core-metrics-results/shannon-group-significance.qzv

Reproducing the results from amutha for the Group meeting presentation

*Making a filtered HHRI dental dataset without AR for the overall microbiome composition by site

qiime feature-table filter-samples \
  --i-table hhridental-noAR1-table.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --p-where "[Gender] = 'Female' " \
  --o-filtered-table hhridental-female-table.qza

qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhridental-rooted-tree.qza \
  --i-table hhridental-female-table.qza \
  --p-sampling-depth 25000 \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --output-dir hhridental-female-core-metrics-results

qiime diversity alpha-group-significance \
  --i-alpha-diversity hhridental-female-core-metrics-results/shannon_vector.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --o-visualization hhridental-female-core-metrics-results/shannon-group-significance.qzv

Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated. (*type Shift + Command + 5) to get a snipping tool on mac
qiime tools view hhridental-female-core-metrics-results/shannon-group-significance.qzv

*16S dataset, subsetting by PK samples to make barplot for high vs low invitro

qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --p-where "[Group] = 'PK' " \
  --o-filtered-table pk-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny oral-gut-rooted-tree.qza \
  --i-table oral-only-pre-table.qza \
  --p-sampling-depth 6000 \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --output-dir oral-only-pre-core-metrics-results

qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --output-dir pk-only-core-metrics-results

**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls pk-only-core-metrics-results/

**Getting the Shannon value for all the sample, then creating a new mapping file so you can use that as a covariate
qiime diversity alpha-group-significance \
  --i-alpha-diversity pk-only-core-metrics-results/shannon_vector.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --o-visualization pk-only-core-metrics-results/shannon-group-significance.qzv

**You need to visualize the file then export the raw values as TSV
qiime tools view pk-only-core-metrics-results/shannon-group-significance.qzv

Testing longitudinal analyses for alpha diversity on the asmic oral full dataset
Performing longitudinal and paired sample comparisons with q2-longitudinal
https://docs.qiime2.org/2019.10/tutorials/longitudinal/
Pairwise difference comparisons
Pairwise difference tests determine whether the value of a specific metric changed significantly between pairs of paired samples (e.g., pre- and post-treatment).
This visualizer currently supports comparison of feature abundance (e.g., microbial sequence variants or taxa) in a feature table, or of metadata values in a sample metadata file. Alpha diversity values (e.g., observed sequence variants) and beta diversity values (e.g., principal coordinates) are useful metrics for comparison with these tests, and should be contained in one of the metadata files given as input. In the example below, we will test whether alpha diversity (Shannon diversity index) changed significantly between two different time points in the ECAM data according to delivery mode. (Find the Shannon diversity artifact from your processing)
*Here, I use the metadata I downloaded with Shannon values for all samples as the mapping file
qiime longitudinal pairwise-differences \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-column Site \
  --p-state-column Collection \
  --p-state-1 1 \
  --p-state-2 2 \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-gut-pairwise-differences.qzv

qiime longitudinal pairwise-differences \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-asa-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-column Site \
  --p-state-column Collection \
  --p-state-1 1 \
  --p-state-2 2 \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-gut-asa-pairwise-differences.qzv

qiime longitudinal pairwise-differences \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-pcb-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-column Site \
  --p-state-column Collection \
  --p-state-1 1 \
  --p-state-2 2 \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-gut-pcb-pairwise-differences.qzv

**Testing a way to show paired differences at baseline and post intervention

qiime longitudinal pairwise-differences \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-column Collection \
  --p-state-column Site \
  --p-state-1  'Oral'  \
  --p-state-2 'Gut' \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-gut-pairwise-differences_test.qzv

qiime longitudinal pairwise-differences \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-pre-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-column Contents \
  --p-state-column Site \
  --p-state-1  'Oral'  \
  --p-state-2 'Gut' \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-gut-pre-pairwise-differences_test.qzv

qiime longitudinal pairwise-differences \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-post-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-column Contents \
  --p-state-column Site \
  --p-state-1  'Oral'  \
  --p-state-2 'Gut' \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-gut-post-pairwise-differences_test.qzv

*16S dataset (Not needed for our datastructures unless you want to compare M1 to M2 in everyone)

qiime longitudinal pairwise-differences \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --m-metadata-file pk-only-core-metrics-results/shannon_vector.qza \
  --p-metric shannon \
  --p-group-column Contents \
  --p-state-column Site \
  --p-state-1  'Oral'  \
  --p-state-2 'Gut' \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-gut-post-pairwise-differences_test.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view oral-gut-pairwise-differences.qzv
qiime tools view oral-gut-asa-pairwise-differences.qzv
qiime tools view oral-gut-pcb-pairwise-differences.qzv
qiime tools view oral-gut-pairwise-differences_test.qzv
qiime tools view oral-gut-pre-pairwise-differences_test.qzv
qiime tools view oral-gut-post-pairwise-differences_test.qzv
Linear mixed effect models
Linear mixed effects (LME) models test the relationship between a single response variable and one or more independent variables, where observations are made across dependent samples, e.g., in repeated-measures sampling experiments. This implementation takes at least one numeric state-column (e.g., Time) and one or more comma-separated group-columns (which may be categorical or numeric metadata columns; these are the fixed effects) as independent variables in a LME model, and plots regression plots of the response variable (“metric”) as a function of the state column and each group column. Additionally, the individual-id-column parameter should be a metadata column that indicates the individual subject/site that was sampled repeatedly. The response variable may either be a sample metadata mapping file column or a feature ID in the feature table. A comma-separated list of random effects can also be input to this action; a random intercept for each individual is included by default, but another common random effect that users may wish to use is a random slope for each individual, which can be set by using the state-column value as input to the random-effects parameter. Here we use LME to test whether alpha diversity (Shannon diversity index) changed over time and in response to delivery mode, diet, and sex in the ECAM data set.
Note
Deciding whether a factor is a fixed effect or a random effect can be complicated. In general, a factor should be a fixed effect if the different factor levels (metadata column values) represent (more or less) all possible discrete values. For example, delivery mode, sex, and diet (dominantly breast-fed or formula-fed) are designated as fixed effects in the example below. Conversely, a factor should be a random effect if its values represent random samples from a population. For example, we could imagine having metadata variables like body-weight, daily-kcal-from-breastmilk, number-of-peanuts-eaten-per-day, or mg-of-penicillin-administered-daily; such values would represent random samples from within a population, and are unlikely to capture all possible values representative of the whole population. Not sure about the factors in your experiment? 🤔 Consult a statistician or reputable statistical tome for guidance. 📚
*I am running a Linear Mixed effect model for the Site variable as a predictor of change in the overall study
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Site \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization oral-gut-shannon-linear-mixed-effects.qzv
*For this model, I am using a specific mapping file where I copied the matched oral shanon diversity values (from DADA2 in R) to the gut samples, and I used sorting by Subject ID and by Collection to make sure I had the matching samples. (I actually exported the values from the oral-gut-core-metrics file to a text file, and used it in R to do the same analysis)
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Map_Shannon_Dada.tsv \
  --m-metadata-file gut-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Shannon_Dada_Oral \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization gut-only-oral-shannon-linear-mixed-effects.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view oral-gut-shannon-linear-mixed-effects.qzv
qiime tools view gut-only-oral-shannon-linear-mixed-effects.qzv
I can also run a linear mixed effect at a single time point (But it doesn’t work because Site is not a numerical variable… I am not so sure of the model here either ways so I’ll come back to it). Technically the second model is a post test only?? And can tell us if the correlation in Shannon value between the 2 sites (oral and gut) differs in the aspirin and the placebo group after 6 weeks.. but that model doesn’t make sense to run at baseline. The model above where you specifically match Oral to Gut values is more interpretable though.
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --m-metadata-file oral-gut-pre-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Contents \
  --p-state-column Site \
  --p-individual-id-column Subject_ID \
  --o-visualization oral-gut-pre-shannon-linear-mixed-effects.qzv

qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --m-metadata-file oral-gut-post-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Contents \
  --p-state-column Site \
  --p-individual-id-column Subject_ID \
  --o-visualization oral-gut-post-shannon-linear-mixed-effects.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view oral-gut-pre-shannon-linear-mixed-effects.qzv
qiime tools view oral-gut-post-shannon-linear-mixed-effects.qzv
The visualizer produced by this command contains several results. First, the input parameters are shown at the top of the visualization for convenience (e.g., when flipping through multiple visualizations it is useful to have a summary). Next, the “model summary” shows some descriptive information about the LME model that was trained. This just shows descriptive information about the “groups”; in this case, groups will be individuals (as set by the --p-individual-id-column). The main results to examine will be the “model results” below the “model summary”. These results summarize the effects of each fixed effect (and their interactions) on the dependent variable (shannon diversity). This table shows parameter estimates, estimate standard errors, z scores, P values (P>|z|), and 95% confidence interval upper and lower bounds for each parameter. We see in this table that shannon diversity is significantly impacted by month of life and by diet, as well as several interacting factors. More information about LME models and the interpretation of these data can be found on the statsmodels LME description page, which provides a number of useful technical references for further reading.
Finally, scatter plots categorized by each “group column” are shown at the bottom of the visualization, with linear regression lines (plus 95% confidence interval in grey) for each group. If --p-lowess is enabled, instead locally weighted averages are shown for each group. Two different groups of scatter plots are shown. First, regression scatterplots show the relationship between state_column (x-axis) and metric (y-axis) for each sample. These plots are just used as a quick summary for reference; users are recommended to use the volatility visualizer for interactive plotting of their longitudinal data. Volatility plots can be used to qualitatively identify outliers that disproportionately drive the variance within individuals and groups, including by inspecting residuals in relation to control limits (see note below and the section on “Volatility analysis” for more details).
The second set of scatterplots are fit vs. residual plots, which show the relationship between metric predictions for each sample (on the x-axis), and the residual or observation error (prediction - actual value) for each sample (on the y-axis). Residuals should be roughly zero-centered and normal across the range of measured values. Uncentered, systematically high or low, and autocorrelated values could indicate a poor model. If your residual plots look like an ugly mess without any apparent relationship between values, you are doing a good job. If you see a U-shaped curve or other non-random distribution, either your predictor variables (group_columns and/or random_effects) are failing to capture all explanatory information, causing information to leak into your residuals, or else you are not using an appropriate model for your data 🙁. Check your predictor variables and available metadata columns to make sure you aren’t missing anything.
Note
If you want to dot your i’s and cross your t’s, residual and predicted values for each sample can be obtained in the “Download raw data as tsv” link below the regression scatterplots. This file can be input as metadata to the volatility visualizer to check whether residuals are correlated with other metadata columns. If they are, those columns should probably be used as prediction variables in your model! Control limits (± 2 and 3 standard deviations) can be toggled on/off to easily identify outliers, which can be particularly useful for re-examining fit vs. residual plots with this visualizer. 🍝
Volatility analysis (from longitudinal analysis tutorial)
The volatility visualizer generates interactive line plots that allow us to assess how volatile a dependent variable is over a continuous, independent variable (e.g., time) in one or more groups. Multiple metadata files (including alpha and beta diversity artifacts) and FeatureTable[RelativeFrequency] tables can be used as input, and in the interactive visualization we can select different dependent variables to plot on the y-axis.
Here we examine how variance in Shannon diversity and other metadata changes across time (set with the state-column parameter) in the ECAM cohort, both in groups of samples (interactively selected as described below) and in individual subjects (set with the individual-id-column parameter).
qiime longitudinal volatility \
  --m-metadata-file ecam-sample-metadata.tsv \
  --m-metadata-file shannon.qza \
  --p-default-metric shannon \
  --p-default-group-column delivery \
  --p-state-column month \
  --p-individual-id-column studyid \
  --o-visualization volatility.qzv

qiime longitudinal pairwise-differences \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-pcb-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-column Site \
  --p-state-column Collection \
  --p-state-1 1 \
  --p-state-2 2 \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-gut-pcb-pairwise-differences.qzv

qiime longitudinal volatility \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --m-metadata-file oral-gut-asa-core-metrics-results/Shannon_vector.qza \
  --p-default-metric shannon \
  --p-default-group-column Site \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization oral_gut_asa_volatility.qzv

qiime longitudinal volatility \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --m-metadata-file oral-gut-pcb-core-metrics-results/Shannon_vector.qza \
  --p-default-metric shannon \
  --p-default-group-column Site \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization oral_gut_pcb_volatility.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view oral_gut_asa_volatility.qzv
qiime tools view oral_gut_pcb_volatility.qzv
In the resulting visualization, a line plot appears on the left-hand side of the plot and a panel of “Plot Controls” appears to the right. These “Plot Controls” interactively adjust several variables and parameters. This allows us to determine how groups’ and individuals’ values change across a single independent variable, state-column. Interective features in this visualization include:
	•	The “Metric column” tab lets us select which continuous metadata values to plot on the y-axis. All continuous numeric columns found in metadata/artifacts input to this action will appear as options in this drop-down tab. In this example, the initial variable plotted in the visualization is shannon diversity because this column was designated by the optional default-metric parameter.
	•	The “Group column” tab lets us select which categorical metadata values to use for calculating mean values. All categorical metadata columns found in metadata/artifacts input to this action will appear as options in this drop-down tab. These mean values are plotted on the line plot, and the thickness and opacity of these mean lines can be modified using the slider bars in the “Plot Controls” on the right-hand side of the visualization. Error bars (standard deviation) can be toggled on and off with a button in the “Plot Controls”.
	•	Longitudinal values for each individual subject are plotted as “spaghetti” lines (so-called because this tangled mass of individual vectors looks like a ball of spaghetti). The thickness and opacity of spaghetti can be modified using the slider bars in the “Plot Controls” on the right-hand side of the visualization.
	•	Color scheme can be adjusted using the “Color scheme” tab.
	•	Global mean and warning/control limits (2X and 3X standard deviations from global mean) can be toggled on/off with the buttons in the “Plot Controls”. The goal of plotting these values is to show how a variable is changing over time (or a gradient) in relation to the mean. Large departures from the mean values can cross the warning/control limits, indicating a major disruption at that state; for example, antibiotic use or other disturbances impacting diversity could be tracked with these plots.
	•	Group mean lines and spaghetti can also be modified with the “scatter size” and “scatter opacity” slider bars in the “Plot Controls”. These adjust the size and opacity of individual points. Maximize scatter opacity and minimize line opacity to transform these into longitudinal scatter plots!
	•	Relevant sample metadata at individual points can be viewed by hovering the mouse over a point of interest.
If the interactive features of this visualization don’t quite scratch your itch, click on the “Open in Vega Editor” button at the top of the “Plot Controls” and customize to your heart’s content. This opens a window for manually editing plot characteristics in Vega Editor, a visualization tool external to QIIME2.
Try Alpha diversity Measures at comparing the prospective vs cross-sectional cohorts at PK time

*16S dataset, subsetting by PK samples to make barplot for high vs low invitro

qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --p-where "[Group] = 'PK' " \
  --o-filtered-table pk-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny oral-gut-rooted-tree.qza \
  --i-table oral-only-pre-table.qza \
  --p-sampling-depth 6000 \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --output-dir oral-only-pre-core-metrics-results

qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --output-dir pk-only-core-metrics-results

**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls pk-only-core-metrics-results/

**Getting the Shannon value for all the sample, then creating a new mapping file so you can use that as a covariate
qiime diversity alpha-group-significance \
  --i-alpha-diversity pk-only-core-metrics-results/shannon_vector.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --o-visualization pk-only-core-metrics-results/shannon-group-significance.qzv

**You need to visualize the file then export the raw values as TSV
qiime tools view pk-only-core-metrics-results/shannon-group-significance.qzv

*MAR2023 16S
qiime phylogeny align-to-tree-mafft-fasttree \
  --i-sequences hhri16s-rep-seqs.qza \
  --o-alignment aligned-hhri16s-rep-seqs.qza \
  --o-masked-alignment masked-aligned-hhri16s-rep-seqs.qza \
  --o-tree hhri16s-unrooted-tree.qza \
  --o-rooted-tree hhri16s-rooted-tree.qza
ls

*Making a table for the samples for which we have both sequencing and PK data from Moataz
*Found at “/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/18_HHRI_UMGC_Projects/04_12192022_Israni4_Project_019and020_16sSequencing/Documents/41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt” using the alt command after rightclicking to copy as a pathname.
qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --p-where "[AUC_sequencing] = 'Both' " \
  --o-filtered-table pk-sequencing-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-sequencing-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --output-dir pk-sequencing-only-core-metrics-results

**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls pk-sequencing-only-core-metrics-results/

**Getting the Shannon value for all the sample, then creating a new mapping file so you can use that as a covariate
qiime diversity alpha-group-significance \
  --i-alpha-diversity pk-sequencing-only-core-metrics-results/shannon_vector.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-only-core-metrics-results/shannon-group-significance.qzv
**For the NOV2023 analysis, I will need to create a variable that indicates which participants have both the diarrhea data and the 16S data, as well as the subset for those who have the moataz pk data as well. I created the variable “mosio_sequencing” in column UN, and “mosio_sequencing_pk” in column VI. I saved the mapping file as /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/48_MAP_MISSION_16S_sample_pull_mappingtemplate_NOV2023_PKMAP_nodups_V5.txt

In addition, I created some categorical variables to facilitate the analysis in QIIME2: “everdiarrheacateg” “everdiarrheatimeframe” “everCTCAE2categ” “everCTCAE2timeframe” and “maxCTCAEcateg”

*NOV2023 16S
qiime phylogeny align-to-tree-mafft-fasttree \
  --i-sequences hhri16s-rep-seqs.qza \
  --o-alignment aligned-hhri16s-rep-seqs.qza \
  --o-masked-alignment masked-aligned-hhri16s-rep-seqs.qza \
  --o-tree hhri16s-unrooted-tree.qza \
  --o-rooted-tree hhri16s-rooted-tree.qza
ls

*Making a table for the samples for which we have both sequencing and PK data from Moataz
*Found at “/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/18_HHRI_UMGC_Projects/04_12192022_Israni4_Project_019and020_16sSequencing/Documents/41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt” using the alt command after rightclicking to copy as a pathname.
qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 48_MAP_MISSION_16S_sample_pull_mappingtemplate_NOV2023_PKMAP_nodups_V5.txt\
  --p-where "[mosio_sequencing] = 1 " \
  --o-filtered-table mosio-sequencing-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table! Set the sampling depth to 10000 based on the qzv description after generating the table
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table mosio-sequencing-only-table.qza \
  --p-sampling-depth 10000 \
  --m-metadata-file 48_MAP_MISSION_16S_sample_pull_mappingtemplate_NOV2023_PKMAP_nodups_V5.txt\
  --output-dir mosio-sequencing-only-only-core-metrics-results

**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls mosio-sequencing-only-only-core-metrics-results/

**Getting the Shannon value for all the sample, then creating a new mapping file so you can use that as a covariate
qiime diversity alpha-group-significance \
  --i-alpha-diversity mosio-sequencing-only-only-core-metrics-results/shannon_vector.qza \
  --m-metadata-file 48_MAP_MISSION_16S_sample_pull_mappingtemplate_NOV2023_PKMAP_nodups_V5.txt\
  --o-visualization mosio-sequencing-only-only-core-metrics-results/shannon-group-significance.qzv

MOSIO alpha diversity M1
*Making a table for the samples for which we have both sequencing and PK data from Moataz
qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --p-where "[mosio_sequencing] = 1 " \
  --o-filtered-table m1-mosio-sequencing-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table! Set the sampling depth to 10000 based on the qzv description after generating the table
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table m1-mosio-sequencing-only-table.qza \
  --p-sampling-depth 25000 \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --output-dir m1-25k-mosio-sequencing-only-only-core-metrics-results

**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls m1-25k-mosio-sequencing-only-only-core-metrics-results/

**Getting the Shannon value for all the sample, then creating a new mapping file so you can use that as a covariate
qiime diversity alpha-group-significance \
  --i-alpha-diversity m1-25k-mosio-sequencing-only-only-core-metrics-results/shannon_vector.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --o-visualization m1-25k-mosio-sequencing-only-only-core-metrics-results/shannon-group-significance.qzv

**Getting the Faith index value for all the samples, then creating a new mapping file so you can use that as a covariate
qiime diversity alpha-group-significance \
  --i-alpha-diversity m1-25k-mosio-sequencing-only-only-core-metrics-results/faith_pd_vector.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --o-visualization m1-25k-mosio-sequencing-only-only-core-metrics-results/faith-group-significance.qzv

MOSIO alpha diversity M2
*Making a table for the samples for which we have both sequencing and PK data from Moataz
qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --p-where "[mosio_sequencing] = 1 " \
  --o-filtered-table m2-mosio-sequencing-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table! Set the sampling depth to 10000 based on the qzv description after generating the table
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table m2-mosio-sequencing-only-table.qza \
  --p-sampling-depth 10000 \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --output-dir m2-mosio-sequencing-only-only-core-metrics-results

**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls m2-mosio-sequencing-only-only-core-metrics-results/

**Getting the Shannon value for all the sample, then creating a new mapping file so you can use that as a covariate
qiime diversity alpha-group-significance \
  --i-alpha-diversity m2-mosio-sequencing-only-only-core-metrics-results/shannon_vector.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --o-visualization m2-mosio-sequencing-only-only-core-metrics-results/shannon-group-significance.qzv

**Getting the Faith index value for all the samples, then creating a new mapping file so you can use that as a covariate
qiime diversity alpha-group-significance \
  --i-alpha-diversity m2-mosio-sequencing-only-only-core-metrics-results/faith_pd_vector.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --o-visualization m2-mosio-sequencing-only-only-core-metrics-results/faith-group-significance.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
**You need to visualize the file then export the raw values as TSV
qiime tools view pk-sequencing-only-core-metrics-results/shannon-group-significance.qzv
qiime tools view m1-25k-mosio-sequencing-only-only-core-metrics-results/shannon-group-significance.qzv
qiime tools view m1-25k-mosio-sequencing-only-only-core-metrics-results/faith-group-significance.qzv

qiime tools view m2-mosio-sequencing-only-only-core-metrics-results/shannon-group-significance.qzv
qiime tools view m2-mosio-sequencing-only-only-core-metrics-results/faith-group-significance.qzv

**after exporting the Shannon indices from  the raw data as a TSV file, I just saved the ID and Shannon columns, sorted the sample-id column in the mapping file, and then matched it to the id column for the Shannon indices raw tsv file.

Shannon Alpha diversity for prospective and cross-sectional cohorts separately

*16S MAR2023
qiime feature-table filter-samples \
  --i-table pk-sequencing-only-table.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --p-where "[Study_arm_2] = 'Cross-sectional' " \
  --o-filtered-table pk-sequencing-cs-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-sequencing-cs-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --output-dir pk-sequencing-cs-only-core-metrics-results

**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls pk-sequencing-cs-only-core-metrics-results/

**Getting the Shannon value for all the sample, then creating a new mapping file so you can use that as a covariate
qiime diversity alpha-group-significance \
  --i-alpha-diversity pk-sequencing-cs-only-core-metrics-results/shannon_vector.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-cs-only-core-metrics-results/shannon-group-significance.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
**You need to visualize the file then export the raw values as TSV
qiime tools view pk-sequencing-cs-only-core-metrics-results/shannon-group-significance.qzv
Shannon Alpha diversity for prospective and cross-sectional cohorts separately

qiime feature-table filter-samples \
  --i-table pk-sequencing-only-table.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --p-where "[Study_arm_2] = 'Prospective' " \
  --o-filtered-table pk-sequencing-ps-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-sequencing-ps-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --output-dir pk-sequencing-ps-only-core-metrics-results

**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls pk-sequencing-ps-only-core-metrics-results/

**Getting the Shannon value for all the sample, then creating a new mapping file so you can use that as a covariate
qiime diversity alpha-group-significance \
  --i-alpha-diversity pk-sequencing-ps-only-core-metrics-results/shannon_vector.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-ps-only-core-metrics-results/shannon-group-significance.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
**You need to visualize the file then export the raw values as TSV
qiime tools view pk-sequencing-ps-only-core-metrics-results/shannon-group-significance.qzv
Longitudinal Alpha diversity for M2 vs M4 vs M6. The first step is to generate the alpha & beta diversity matrices needed for the longitudinal analyses.

qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 43_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4M6.txt \
  --p-where "[Study_arm_2] = 'Prospective' " \
  --o-filtered-table pk-sequencing-M2M4M6-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-sequencing-M2M4M6-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file 43_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4M6.txt \
  --output-dir pk-sequencing-M2M4M6-only-core-metrics-results
Testing longitudinal analyses for alpha diversity on the asmic oral full dataset
Performing longitudinal and paired sample comparisons with q2-longitudinal
https://docs.qiime2.org/2019.10/tutorials/longitudinal/
Pairwise difference comparisons
Pairwise difference tests determine whether the value of a specific metric changed significantly between pairs of paired samples (e.g., pre- and post-treatment).
This visualizer currently supports comparison of feature abundance (e.g., microbial sequence variants or taxa) in a feature table, or of metadata values in a sample metadata file. Alpha diversity values (e.g., observed sequence variants) and beta diversity values (e.g., principal coordinates) are useful metrics for comparison with these tests, and should be contained in one of the metadata files given as input. In the example below, we will test whether alpha diversity (Shannon diversity index) changed significantly between two different time points in the ECAM data according to delivery mode. (Find the Shannon diversity artifact from your processing)
*Here, I use the metadata I downloaded with Shannon values for all samples as the mapping file
qiime longitudinal pairwise-differences \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-column Site \
  --p-state-column Collection \
  --p-state-1 1 \
  --p-state-2 2 \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-gut-pairwise-differences.qzv

*16SMAR2023
*Had to remove the ‘p-group-column” categorical variable as I am only interested in alpha diversity change over time itself. This changed the hypothesis to (is the change in alpha diversity itself between the timepoints different from 0?)
qiime longitudinal pairwise-differences \
  --m-metadata-file 43_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4M6.txt \
  --m-metadata-file pk-sequencing-M2M4M6-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-state-column Time_point \
  --p-state-1 2 \
  --p-state-2 6 \
  --p-individual-id-column Patient_ID \
  --p-replicate-handling random \
  --o-visualization pk-sequencing-M2M4M6-pairwise-differences.qzv

*NOV2023
MOSIO alpha diversity at M1 & M2
*Making a table for the samples for which we have both sequencing and PK data from Moataz
qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal.txt\
  --p-where "[mosio_sequencing] = 1 " \
  --o-filtered-table m1m2-mosio-sequencing-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table! Set the sampling depth to 10000 based on the qzv description after generating the table
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table m1m2-mosio-sequencing-only-table.qza \
  --p-sampling-depth 25000 \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal.txt\
  --output-dir m1m2-25k-mosio-sequencing-only-only-core-metrics-results

**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls m1m2-25k-mosio-sequencing-only-only-core-metrics-results/

*Testing if the Shannon diversity trajectory is different by diarrhea outcome
qiime longitudinal pairwise-differences \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-column everdiarrhea1categ \
  --p-state-column Collection_timepoint\
  --p-state-1 'M1' \
  --p-state-2 'M2_PK' \
  --p-individual-id-column patient_id \
  --p-replicate-handling random \
  --o-visualization m1m2everdiarrhea1categ.qzv

qiime longitudinal pairwise-differences \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-column everdiarrhea1timeframe \
  --p-state-column Collection_timepoint\
  --p-state-1 'M1' \
  --p-state-2 'M2_PK' \
  --p-individual-id-column patient_id \
  --p-replicate-handling random \
  --o-visualization m1m2everdiarrhea1timeframe.qzv

qiime longitudinal pairwise-differences \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-column everCTCAE21categ \
  --p-state-column Collection_timepoint\
  --p-state-1 'M1' \
  --p-state-2 'M2_PK' \
  --p-individual-id-column patient_id \
  --p-replicate-handling random \
  --o-visualization m1m2everCTCAE21categ.qzv

qiime longitudinal pairwise-differences \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-column everCTCAE21timeframe \
  --p-state-column Collection_timepoint\
  --p-state-1 'M1' \
  --p-state-2 'M2_PK' \
  --p-individual-id-column patient_id \
  --p-replicate-handling random \
  --o-visualization m1m2everCTCAE21timeframe.qzv

qiime longitudinal pairwise-differences \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-column maxCTCAEcateg \
  --p-state-column Collection_timepoint\
  --p-state-1 'M1' \
  --p-state-2 'M2_PK' \
  --p-individual-id-column patient_id \
  --p-replicate-handling random \
  --o-visualization m1m2maxCTCAEcateg.qzv

*Testing if the faith diversity trajectory is different by diarrhea outcome
qiime longitudinal pairwise-differences \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/faith_pd_vector.qza \
  --p-metric faith_pd \
  --p-group-column everdiarrhea1categ \
  --p-state-column Collection_timepoint\
  --p-state-1 'M1' \
  --p-state-2 'M2_PK' \
  --p-individual-id-column patient_id \
  --p-replicate-handling random \
  --o-visualization faithm1m2everdiarrhea1categ.qzv

qiime longitudinal pairwise-differences \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/faith_pd_vector.qza \
  --p-metric faith_pd \
  --p-group-column everdiarrhea1timeframe \
  --p-state-column Collection_timepoint\
  --p-state-1 'M1' \
  --p-state-2 'M2_PK' \
  --p-individual-id-column patient_id \
  --p-replicate-handling random \
  --o-visualization faithm1m2everdiarrhea1timeframe.qzv

qiime longitudinal pairwise-differences \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/faith_pd_vector.qza \
  --p-metric faith_pd \
  --p-group-column everCTCAE21categ \
  --p-state-column Collection_timepoint\
  --p-state-1 'M1' \
  --p-state-2 'M2_PK' \
  --p-individual-id-column patient_id \
  --p-replicate-handling random \
  --o-visualization faithm1m2everCTCAE21categ.qzv

qiime longitudinal pairwise-differences \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/faith_pd_vector.qza \
  --p-metric faith_pd \
  --p-group-column everCTCAE21timeframe \
  --p-state-column Collection_timepoint\
  --p-state-1 'M1' \
  --p-state-2 'M2_PK' \
  --p-individual-id-column patient_id \
  --p-replicate-handling random \
  --o-visualization faithm1m2everCTCAE21timeframe.qzv

qiime longitudinal pairwise-differences \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/faith_pd_vector.qza \
  --p-metric faith_pd \
  --p-group-column maxCTCAEcateg \
  --p-state-column Collection_timepoint\
  --p-state-1 'M1' \
  --p-state-2 'M2_PK' \
  --p-individual-id-column patient_id \
  --p-replicate-handling random \
  --o-visualization faithm1m2maxCTCAEcateg.qzv

Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
**You need to visualize the file then export the raw values as TSV
qiime tools view pk-sequencing-M2M4M6-pairwise-differences.qzv
qiime tools view m1m2everdiarrhea1categ.qzv
qiime tools view m1m2everdiarrhea1timeframe.qzv
qiime tools view m1m2everCTCAE21categ.qzv
qiime tools view m1m2everCTCAE21timeframe.qzv
qiime tools view m1m2maxCTCAEcateg.qzv


qiime tools view faithm1m2everdiarrhea1categ.qzv
qiime tools view faithm1m2everdiarrhea1timeframe.qzv
qiime tools view faithm1m2everCTCAE21categ.qzv
qiime tools view faithm1m2everCTCAE21timeframe.qzv
qiime tools view faithm1m2maxCTCAEcateg.qzv
Linear mixed effect models
Linear mixed effects (LME) models test the relationship between a single response variable and one or more independent variables, where observations are made across dependent samples, e.g., in repeated-measures sampling experiments. This implementation takes at least one numeric state-column (e.g., Time) and one or more comma-separated group-columns (which may be categorical or numeric metadata columns; these are the fixed effects) as independent variables in a LME model, and plots regression plots of the response variable (“metric”) as a function of the state column and each group column. Additionally, the individual-id-column parameter should be a metadata column that indicates the individual subject/site that was sampled repeatedly. The response variable may either be a sample metadata mapping file column or a feature ID in the feature table. A comma-separated list of random effects can also be input to this action; a random intercept for each individual is included by default, but another common random effect that users may wish to use is a random slope for each individual, which can be set by using the state-column value as input to the random-effects parameter. Here we use LME to test whether alpha diversity (Shannon diversity index) changed over time and in response to delivery mode, diet, and sex in the ECAM data set.
Note
Deciding whether a factor is a fixed effect or a random effect can be complicated. In general, a factor should be a fixed effect if the different factor levels (metadata column values) represent (more or less) all possible discrete values. For example, delivery mode, sex, and diet (dominantly breast-fed or formula-fed) are designated as fixed effects in the example below. Conversely, a factor should be a random effect if its values represent random samples from a population. For example, we could imagine having metadata variables like body-weight, daily-kcal-from-breastmilk, number-of-peanuts-eaten-per-day, or mg-of-penicillin-administered-daily; such values would represent random samples from within a population, and are unlikely to capture all possible values representative of the whole population. Not sure about the factors in your experiment? 🤔 Consult a statistician or reputable statistical tome for guidance. 📚
*I am running a Linear Mixed effect model for the Site variable as a predictor of change in the overall study
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Site \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization oral-gut-shannon-linear-mixed-effects.qzv
*For this model, I am using a specific mapping file where I copied the matched oral shanon diversity values (from DADA2 in R) to the gut samples, and I used sorting by Subject ID and by Collection to make sure I had the matching samples. (I actually exported the values from the oral-gut-core-metrics file to a text file, and used it in R to do the same analysis)
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Map_Shannon_Dada.tsv \
  --m-metadata-file gut-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Shannon_Dada_Oral \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization gut-only-oral-shannon-linear-mixed-effects.qzv

*16SMAR2023 
*Trying a longitudinal model without the categorical variable “  --p-group-columns Site \” as a fixed effect. Alternatively, I can try using time as a fixed effect.
qiime longitudinal linear-mixed-effects \
  --m-metadata-file 43_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4M6.txt \
   --m-metadata-file pk-sequencing-M2M4M6-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M2M4M6-shannon-linear-mixed-effects.qzv

*NOV2023
*Testing the Shannon alpha diversity trajectory by LME
qiime longitudinal linear-mixed-effects \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns everdiarrhea1categ \
  --p-state-column Collection_timepoint_num\
  --p-individual-id-column patient_id \
  --o-visualization lme_m1m2everdiarrhea1categ.qzv

qiime longitudinal linear-mixed-effects \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns everdiarrhea1timeframe \
  --p-state-column Collection_timepoint_num\
  --p-individual-id-column patient_id \
  --o-visualization lme_m1m2everdiarrhea1timeframe.qzv

qiime longitudinal linear-mixed-effects \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns everCTCAE21categ \
  --p-state-column Collection_timepoint_num\
  --p-individual-id-column patient_id \
  --o-visualization lme_m1m2everCTCAE21categ.qzv

qiime longitudinal linear-mixed-effects \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns everCTCAE21timeframe \
  --p-state-column Collection_timepoint_num\
  --p-individual-id-column patient_id \
  --o-visualization lme_m1m2everCTCAE21timeframe.qzv

qiime longitudinal linear-mixed-effects \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns maxCTCAEcateg \
  --p-state-column Collection_timepoint_num\
  --p-individual-id-column patient_id \
  --o-visualization lme_m1m2maxCTCAEcateg.qzv

*Testing the Faith PD alpha diversity trajectory by LME
qiime longitudinal linear-mixed-effects \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/faith_pd_vector.qza \
  --p-metric faith_pd \
  --p-group-columns everdiarrhea1categ \
  --p-state-column Collection_timepoint_num\
  --p-individual-id-column patient_id \
  --o-visualization lme_faithm1m2everdiarrhea1categ.qzv

qiime longitudinal linear-mixed-effects \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/faith_pd_vector.qza \
  --p-metric faith_pd \
  --p-group-columns everdiarrhea1timeframe \
  --p-state-column Collection_timepoint_num\
  --p-individual-id-column patient_id \
  --o-visualization lme_faithm1m2everdiarrhea1timeframe.qzv

qiime longitudinal linear-mixed-effects \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/faith_pd_vector.qza \
  --p-metric faith_pd \
  --p-group-columns everCTCAE21categ \
  --p-state-column Collection_timepoint_num\
  --p-individual-id-column patient_id \
  --o-visualization lme_faithm1m2everCTCAE21categ.qzv

qiime longitudinal linear-mixed-effects \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/faith_pd_vector.qza \
  --p-metric faith_pd \
  --p-group-columns everCTCAE21timeframe \
  --p-state-column Collection_timepoint_num\
  --p-individual-id-column patient_id \
  --o-visualization lme_faithm1m2everCTCAE21timeframe.qzv

qiime longitudinal linear-mixed-effects \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/faith_pd_vector.qza \
  --p-metric faith_pd \
  --p-group-columns maxCTCAEcateg \
  --p-state-column Collection_timepoint_num\
  --p-individual-id-column patient_id \
  --o-visualization lme_faithm1m2maxCTCAEcateg.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M2M4M6-shannon-linear-mixed-effects.qzv
qiime tools view lme_m1m2everdiarrhea1categ.qzv
qiime tools view lme_m1m2everdiarrhea1timeframe.qzv
qiime tools view lme_m1m2everCTCAE21categ.qzv
qiime tools view lme_m1m2everCTCAE21timeframe.qzv
qiime tools view lme_m1m2maxCTCAEcateg.qzv

qiime tools view lme_faithm1m2everdiarrhea1categ.qzv
qiime tools view lme_faithm1m2everdiarrhea1timeframe.qzv
qiime tools view lme_faithm1m2everCTCAE21categ.qzv
qiime tools view lme_faithm1m2everCTCAE21timeframe.qzv
qiime tools view lme_faithm1m2maxCTCAEcateg.qzv

The visualizer produced by this command contains several results. First, the input parameters are shown at the top of the visualization for convenience (e.g., when flipping through multiple visualizations it is useful to have a summary). Next, the “model summary” shows some descriptive information about the LME model that was trained. This just shows descriptive information about the “groups”; in this case, groups will be individuals (as set by the --p-individual-id-column). The main results to examine will be the “model results” below the “model summary”. These results summarize the effects of each fixed effect (and their interactions) on the dependent variable (shannon diversity). This table shows parameter estimates, estimate standard errors, z scores, P values (P>|z|), and 95% confidence interval upper and lower bounds for each parameter. We see in this table that shannon diversity is significantly impacted by month of life and by diet, as well as several interacting factors. More information about LME models and the interpretation of these data can be found on the statsmodels LME description page, which provides a number of useful technical references for further reading.
Finally, scatter plots categorized by each “group column” are shown at the bottom of the visualization, with linear regression lines (plus 95% confidence interval in grey) for each group. If --p-lowess is enabled, instead locally weighted averages are shown for each group. Two different groups of scatter plots are shown. First, regression scatterplots show the relationship between state_column (x-axis) and metric (y-axis) for each sample. These plots are just used as a quick summary for reference; users are recommended to use the volatility visualizer for interactive plotting of their longitudinal data. Volatility plots can be used to qualitatively identify outliers that disproportionately drive the variance within individuals and groups, including by inspecting residuals in relation to control limits (see note below and the section on “Volatility analysis” for more details).
The second set of scatterplots are fit vs. residual plots, which show the relationship between metric predictions for each sample (on the x-axis), and the residual or observation error (prediction - actual value) for each sample (on the y-axis). Residuals should be roughly zero-centered and normal across the range of measured values. Uncentered, systematically high or low, and autocorrelated values could indicate a poor model. If your residual plots look like an ugly mess without any apparent relationship between values, you are doing a good job. If you see a U-shaped curve or other non-random distribution, either your predictor variables (group_columns and/or random_effects) are failing to capture all explanatory information, causing information to leak into your residuals, or else you are not using an appropriate model for your data 🙁. Check your predictor variables and available metadata columns to make sure you aren’t missing anything.
Note
If you want to dot your i’s and cross your t’s, residual and predicted values for each sample can be obtained in the “Download raw data as tsv” link below the regression scatterplots. This file can be input as metadata to the volatility visualizer to check whether residuals are correlated with other metadata columns. If they are, those columns should probably be used as prediction variables in your model! Control limits (± 2 and 3 standard deviations) can be toggled on/off to easily identify outliers, which can be particularly useful for re-examining fit vs. residual plots with this visualizer. 🍝
Volatility analysis (from longitudinal analysis tutorial)
The volatility visualizer generates interactive line plots that allow us to assess how volatile a dependent variable is over a continuous, independent variable (e.g., time) in one or more groups. Multiple metadata files (including alpha and beta diversity artifacts) and FeatureTable[RelativeFrequency] tables can be used as input, and in the interactive visualization we can select different dependent variables to plot on the y-axis.
Here we examine how variance in Shannon diversity and other metadata changes across time (set with the state-column parameter) in the ECAM cohort, both in groups of samples (interactively selected as described below) and in individual subjects (set with the individual-id-column parameter).
qiime longitudinal volatility \
  --m-metadata-file ecam-sample-metadata.tsv \
  --m-metadata-file shannon.qza \
  --p-default-metric shannon \
  --p-default-group-column delivery \
  --p-state-column month \
  --p-individual-id-column studyid \
  --o-visualization volatility.qzv

*16SMAR2023
*Again, excluding site as a categorical variable here as we are mainly interested at the change over time itself
qiime longitudinal volatility \
  --m-metadata-file 43_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4M6.txt \
  --m-metadata-file pk-sequencing-M2M4M6-only-core-metrics-results/Shannon_vector.qza \
  --p-default-metric shannon \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M2M4M6-only-volatility.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M2M4M6-only-volatility.qzv
In the resulting visualization, a line plot appears on the left-hand side of the plot and a panel of “Plot Controls” appears to the right. These “Plot Controls” interactively adjust several variables and parameters. This allows us to determine how groups’ and individuals’ values change across a single independent variable, state-column. Interective features in this visualization include:
	•	The “Metric column” tab lets us select which continuous metadata values to plot on the y-axis. All continuous numeric columns found in metadata/artifacts input to this action will appear as options in this drop-down tab. In this example, the initial variable plotted in the visualization is shannon diversity because this column was designated by the optional default-metric parameter.
	•	The “Group column” tab lets us select which categorical metadata values to use for calculating mean values. All categorical metadata columns found in metadata/artifacts input to this action will appear as options in this drop-down tab. These mean values are plotted on the line plot, and the thickness and opacity of these mean lines can be modified using the slider bars in the “Plot Controls” on the right-hand side of the visualization. Error bars (standard deviation) can be toggled on and off with a button in the “Plot Controls”.
	•	Longitudinal values for each individual subject are plotted as “spaghetti” lines (so-called because this tangled mass of individual vectors looks like a ball of spaghetti). The thickness and opacity of spaghetti can be modified using the slider bars in the “Plot Controls” on the right-hand side of the visualization.
	•	Color scheme can be adjusted using the “Color scheme” tab.
	•	Global mean and warning/control limits (2X and 3X standard deviations from global mean) can be toggled on/off with the buttons in the “Plot Controls”. The goal of plotting these values is to show how a variable is changing over time (or a gradient) in relation to the mean. Large departures from the mean values can cross the warning/control limits, indicating a major disruption at that state; for example, antibiotic use or other disturbances impacting diversity could be tracked with these plots.
	•	Group mean lines and spaghetti can also be modified with the “scatter size” and “scatter opacity” slider bars in the “Plot Controls”. These adjust the size and opacity of individual points. Maximize scatter opacity and minimize line opacity to transform these into longitudinal scatter plots!
	•	Relevant sample metadata at individual points can be viewed by hovering the mouse over a point of interest.
If the interactive features of this visualization don’t quite scratch your itch, click on the “Open in Vega Editor” button at the top of the “Plot Controls” and customize to your heart’s content. This opens a window for manually editing plot characteristics in Vega Editor, a visualization tool external to QIIME2.

Longitudinal Alpha diversity for M2 vs M4. The first step is to generate the alpha & beta diversity matrices needed for the longitudinal analyses.

qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4only.txt \
  --p-where "[Study_arm_2] = 'Prospective' " \
  --o-filtered-table pk-sequencing-M2M4-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-sequencing-M2M4-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4only.txt \
  --output-dir pk-sequencing-M2M4-only-core-metrics-results
Testing longitudinal analyses for alpha diversity on the asmic oral full dataset
Performing longitudinal and paired sample comparisons with q2-longitudinal
https://docs.qiime2.org/2019.10/tutorials/longitudinal/
Pairwise difference comparisons
Pairwise difference tests determine whether the value of a specific metric changed significantly between pairs of paired samples (e.g., pre- and post-treatment).
This visualizer currently supports comparison of feature abundance (e.g., microbial sequence variants or taxa) in a feature table, or of metadata values in a sample metadata file. Alpha diversity values (e.g., observed sequence variants) and beta diversity values (e.g., principal coordinates) are useful metrics for comparison with these tests, and should be contained in one of the metadata files given as input. In the example below, we will test whether alpha diversity (Shannon diversity index) changed significantly between two different time points in the ECAM data according to delivery mode. (Find the Shannon diversity artifact from your processing)
*Here, I use the metadata I downloaded with Shannon values for all samples as the mapping file
qiime longitudinal pairwise-differences \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-column Site \
  --p-state-column Collection \
  --p-state-1 1 \
  --p-state-2 2 \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-gut-pairwise-differences.qzv

*16SMAR2023
*Had to remove the ‘p-group-column” categorical variable as I am only interested in alpha diversity change over time itself. This changed the hypothesis to (is the change in alpha diversity itself between the timepoints different from 0?)
qiime longitudinal pairwise-differences \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4only.txt \
  --m-metadata-file pk-sequencing-M2M4-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-state-column Time_point \
  --p-state-1 2 \
  --p-state-2 4 \
  --p-individual-id-column Patient_ID \
  --p-replicate-handling random \
  --o-visualization pk-sequencing-M2M4-pairwise-differences.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
**You need to visualize the file then export the raw values as TSV
qiime tools view pk-sequencing-M2M4-pairwise-differences.qzv
Linear mixed effect models
Linear mixed effects (LME) models test the relationship between a single response variable and one or more independent variables, where observations are made across dependent samples, e.g., in repeated-measures sampling experiments. This implementation takes at least one numeric state-column (e.g., Time) and one or more comma-separated group-columns (which may be categorical or numeric metadata columns; these are the fixed effects) as independent variables in a LME model, and plots regression plots of the response variable (“metric”) as a function of the state column and each group column. Additionally, the individual-id-column parameter should be a metadata column that indicates the individual subject/site that was sampled repeatedly. The response variable may either be a sample metadata mapping file column or a feature ID in the feature table. A comma-separated list of random effects can also be input to this action; a random intercept for each individual is included by default, but another common random effect that users may wish to use is a random slope for each individual, which can be set by using the state-column value as input to the random-effects parameter. Here we use LME to test whether alpha diversity (Shannon diversity index) changed over time and in response to delivery mode, diet, and sex in the ECAM data set.
Note
Deciding whether a factor is a fixed effect or a random effect can be complicated. In general, a factor should be a fixed effect if the different factor levels (metadata column values) represent (more or less) all possible discrete values. For example, delivery mode, sex, and diet (dominantly breast-fed or formula-fed) are designated as fixed effects in the example below. Conversely, a factor should be a random effect if its values represent random samples from a population. For example, we could imagine having metadata variables like body-weight, daily-kcal-from-breastmilk, number-of-peanuts-eaten-per-day, or mg-of-penicillin-administered-daily; such values would represent random samples from within a population, and are unlikely to capture all possible values representative of the whole population. Not sure about the factors in your experiment? 🤔 Consult a statistician or reputable statistical tome for guidance. 📚
*I am running a Linear Mixed effect model for the Site variable as a predictor of change in the overall study
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Site \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization oral-gut-shannon-linear-mixed-effects.qzv
*For this model, I am using a specific mapping file where I copied the matched oral shanon diversity values (from DADA2 in R) to the gut samples, and I used sorting by Subject ID and by Collection to make sure I had the matching samples. (I actually exported the values from the oral-gut-core-metrics file to a text file, and used it in R to do the same analysis)
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Map_Shannon_Dada.tsv \
  --m-metadata-file gut-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Shannon_Dada_Oral \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization gut-only-oral-shannon-linear-mixed-effects.qzv
*16SMAR2023 
*Trying a longitudinal model without the categorical variable “  --p-group-columns Site \” as a fixed effect. Alternatively, I can try using time as a fixed effect.
qiime longitudinal linear-mixed-effects \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4only.txt \
   --m-metadata-file pk-sequencing-M2M4-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M2M4-shannon-linear-mixed-effects.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M2M4-shannon-linear-mixed-effects.qzv
The visualizer produced by this command contains several results. First, the input parameters are shown at the top of the visualization for convenience (e.g., when flipping through multiple visualizations it is useful to have a summary). Next, the “model summary” shows some descriptive information about the LME model that was trained. This just shows descriptive information about the “groups”; in this case, groups will be individuals (as set by the --p-individual-id-column). The main results to examine will be the “model results” below the “model summary”. These results summarize the effects of each fixed effect (and their interactions) on the dependent variable (shannon diversity). This table shows parameter estimates, estimate standard errors, z scores, P values (P>|z|), and 95% confidence interval upper and lower bounds for each parameter. We see in this table that shannon diversity is significantly impacted by month of life and by diet, as well as several interacting factors. More information about LME models and the interpretation of these data can be found on the statsmodels LME description page, which provides a number of useful technical references for further reading.
Finally, scatter plots categorized by each “group column” are shown at the bottom of the visualization, with linear regression lines (plus 95% confidence interval in grey) for each group. If --p-lowess is enabled, instead locally weighted averages are shown for each group. Two different groups of scatter plots are shown. First, regression scatterplots show the relationship between state_column (x-axis) and metric (y-axis) for each sample. These plots are just used as a quick summary for reference; users are recommended to use the volatility visualizer for interactive plotting of their longitudinal data. Volatility plots can be used to qualitatively identify outliers that disproportionately drive the variance within individuals and groups, including by inspecting residuals in relation to control limits (see note below and the section on “Volatility analysis” for more details).
The second set of scatterplots are fit vs. residual plots, which show the relationship between metric predictions for each sample (on the x-axis), and the residual or observation error (prediction - actual value) for each sample (on the y-axis). Residuals should be roughly zero-centered and normal across the range of measured values. Uncentered, systematically high or low, and autocorrelated values could indicate a poor model. If your residual plots look like an ugly mess without any apparent relationship between values, you are doing a good job. If you see a U-shaped curve or other non-random distribution, either your predictor variables (group_columns and/or random_effects) are failing to capture all explanatory information, causing information to leak into your residuals, or else you are not using an appropriate model for your data 🙁. Check your predictor variables and available metadata columns to make sure you aren’t missing anything.
Note
If you want to dot your i’s and cross your t’s, residual and predicted values for each sample can be obtained in the “Download raw data as tsv” link below the regression scatterplots. This file can be input as metadata to the volatility visualizer to check whether residuals are correlated with other metadata columns. If they are, those columns should probably be used as prediction variables in your model! Control limits (± 2 and 3 standard deviations) can be toggled on/off to easily identify outliers, which can be particularly useful for re-examining fit vs. residual plots with this visualizer. 🍝
Volatility analysis (from longitudinal analysis tutorial)
The volatility visualizer generates interactive line plots that allow us to assess how volatile a dependent variable is over a continuous, independent variable (e.g., time) in one or more groups. Multiple metadata files (including alpha and beta diversity artifacts) and FeatureTable[RelativeFrequency] tables can be used as input, and in the interactive visualization we can select different dependent variables to plot on the y-axis.
Here we examine how variance in Shannon diversity and other metadata changes across time (set with the state-column parameter) in the ECAM cohort, both in groups of samples (interactively selected as described below) and in individual subjects (set with the individual-id-column parameter).
qiime longitudinal volatility \
  --m-metadata-file ecam-sample-metadata.tsv \
  --m-metadata-file shannon.qza \
  --p-default-metric shannon \
  --p-default-group-column delivery \
  --p-state-column month \
  --p-individual-id-column studyid \
  --o-visualization volatility.qzv

*16SMAR2023
*Again, excluding site as a categorical variable here as we are mainly interested at the change over time itself
qiime longitudinal volatility \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4only.txt \
  --m-metadata-file pk-sequencing-M2M4-only-core-metrics-results/Shannon_vector.qza \
  --p-default-metric shannon \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M2M4-only-volatility.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M2M4-only-volatility.qzv
In the resulting visualization, a line plot appears on the left-hand side of the plot and a panel of “Plot Controls” appears to the right. These “Plot Controls” interactively adjust several variables and parameters. This allows us to determine how groups’ and individuals’ values change across a single independent variable, state-column. Interective features in this visualization include:
	•	The “Metric column” tab lets us select which continuous metadata values to plot on the y-axis. All continuous numeric columns found in metadata/artifacts input to this action will appear as options in this drop-down tab. In this example, the initial variable plotted in the visualization is shannon diversity because this column was designated by the optional default-metric parameter.
	•	The “Group column” tab lets us select which categorical metadata values to use for calculating mean values. All categorical metadata columns found in metadata/artifacts input to this action will appear as options in this drop-down tab. These mean values are plotted on the line plot, and the thickness and opacity of these mean lines can be modified using the slider bars in the “Plot Controls” on the right-hand side of the visualization. Error bars (standard deviation) can be toggled on and off with a button in the “Plot Controls”.
	•	Longitudinal values for each individual subject are plotted as “spaghetti” lines (so-called because this tangled mass of individual vectors looks like a ball of spaghetti). The thickness and opacity of spaghetti can be modified using the slider bars in the “Plot Controls” on the right-hand side of the visualization.
	•	Color scheme can be adjusted using the “Color scheme” tab.
	•	Global mean and warning/control limits (2X and 3X standard deviations from global mean) can be toggled on/off with the buttons in the “Plot Controls”. The goal of plotting these values is to show how a variable is changing over time (or a gradient) in relation to the mean. Large departures from the mean values can cross the warning/control limits, indicating a major disruption at that state; for example, antibiotic use or other disturbances impacting diversity could be tracked with these plots.
	•	Group mean lines and spaghetti can also be modified with the “scatter size” and “scatter opacity” slider bars in the “Plot Controls”. These adjust the size and opacity of individual points. Maximize scatter opacity and minimize line opacity to transform these into longitudinal scatter plots!
	•	Relevant sample metadata at individual points can be viewed by hovering the mouse over a point of interest.
If the interactive features of this visualization don’t quite scratch your itch, click on the “Open in Vega Editor” button at the top of the “Plot Controls” and customize to your heart’s content. This opens a window for manually editing plot characteristics in Vega Editor, a visualization tool external to QIIME2.

Longitudinal Alpha diversity for M2 vs M6. The first step is to generate the alpha & beta diversity matrices needed for the longitudinal analyses.

qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M6only.txt \
  --p-where "[Study_arm_2] = 'Prospective' " \
  --o-filtered-table pk-sequencing-M2M6-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-sequencing-M2M6-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M6only.txt \
  --output-dir pk-sequencing-M2M6-only-core-metrics-results
Testing longitudinal analyses for alpha diversity on the asmic oral full dataset
Performing longitudinal and paired sample comparisons with q2-longitudinal
https://docs.qiime2.org/2019.10/tutorials/longitudinal/
Pairwise difference comparisons
Pairwise difference tests determine whether the value of a specific metric changed significantly between pairs of paired samples (e.g., pre- and post-treatment).
This visualizer currently supports comparison of feature abundance (e.g., microbial sequence variants or taxa) in a feature table, or of metadata values in a sample metadata file. Alpha diversity values (e.g., observed sequence variants) and beta diversity values (e.g., principal coordinates) are useful metrics for comparison with these tests, and should be contained in one of the metadata files given as input. In the example below, we will test whether alpha diversity (Shannon diversity index) changed significantly between two different time points in the ECAM data according to delivery mode. (Find the Shannon diversity artifact from your processing)
*Here, I use the metadata I downloaded with Shannon values for all samples as the mapping file
qiime longitudinal pairwise-differences \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-column Site \
  --p-state-column Collection \
  --p-state-1 1 \
  --p-state-2 2 \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-gut-pairwise-differences.qzv

*16SMAR2023
*Had to remove the ‘p-group-column” categorical variable as I am only interested in alpha diversity change over time itself. This changed the hypothesis to (is the change in alpha diversity itself between the timepoints different from 0?)
qiime longitudinal pairwise-differences \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M6only.txt \
  --m-metadata-file pk-sequencing-M2M6-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-state-column Time_point \
  --p-state-1 2 \
  --p-state-2 6 \
  --p-individual-id-column Patient_ID \
  --p-replicate-handling random \
  --o-visualization pk-sequencing-M2M6-pairwise-differences.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
**You need to visualize the file then export the raw values as TSV
qiime tools view pk-sequencing-M2M6-pairwise-differences.qzv
Linear mixed effect models
Linear mixed effects (LME) models test the relationship between a single response variable and one or more independent variables, where observations are made across dependent samples, e.g., in repeated-measures sampling experiments. This implementation takes at least one numeric state-column (e.g., Time) and one or more comma-separated group-columns (which may be categorical or numeric metadata columns; these are the fixed effects) as independent variables in a LME model, and plots regression plots of the response variable (“metric”) as a function of the state column and each group column. Additionally, the individual-id-column parameter should be a metadata column that indicates the individual subject/site that was sampled repeatedly. The response variable may either be a sample metadata mapping file column or a feature ID in the feature table. A comma-separated list of random effects can also be input to this action; a random intercept for each individual is included by default, but another common random effect that users may wish to use is a random slope for each individual, which can be set by using the state-column value as input to the random-effects parameter. Here we use LME to test whether alpha diversity (Shannon diversity index) changed over time and in response to delivery mode, diet, and sex in the ECAM data set.
Note
Deciding whether a factor is a fixed effect or a random effect can be complicated. In general, a factor should be a fixed effect if the different factor levels (metadata column values) represent (more or less) all possible discrete values. For example, delivery mode, sex, and diet (dominantly breast-fed or formula-fed) are designated as fixed effects in the example below. Conversely, a factor should be a random effect if its values represent random samples from a population. For example, we could imagine having metadata variables like body-weight, daily-kcal-from-breastmilk, number-of-peanuts-eaten-per-day, or mg-of-penicillin-administered-daily; such values would represent random samples from within a population, and are unlikely to capture all possible values representative of the whole population. Not sure about the factors in your experiment? 🤔 Consult a statistician or reputable statistical tome for guidance. 📚
*I am running a Linear Mixed effect model for the Site variable as a predictor of change in the overall study
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Site \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization oral-gut-shannon-linear-mixed-effects.qzv
*For this model, I am using a specific mapping file where I copied the matched oral shanon diversity values (from DADA2 in R) to the gut samples, and I used sorting by Subject ID and by Collection to make sure I had the matching samples. (I actually exported the values from the oral-gut-core-metrics file to a text file, and used it in R to do the same analysis)
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Map_Shannon_Dada.tsv \
  --m-metadata-file gut-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Shannon_Dada_Oral \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization gut-only-oral-shannon-linear-mixed-effects.qzv
*16SMAR2023 
*Trying a longitudinal model without the categorical variable “  --p-group-columns Site \” as a fixed effect. Alternatively, I can try using time as a fixed effect.
qiime longitudinal linear-mixed-effects \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M6only.txt \
   --m-metadata-file pk-sequencing-M2M6-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M2M6-shannon-linear-mixed-effects.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M2M6-shannon-linear-mixed-effects.qzv
The visualizer produced by this command contains several results. First, the input parameters are shown at the top of the visualization for convenience (e.g., when flipping through multiple visualizations it is useful to have a summary). Next, the “model summary” shows some descriptive information about the LME model that was trained. This just shows descriptive information about the “groups”; in this case, groups will be individuals (as set by the --p-individual-id-column). The main results to examine will be the “model results” below the “model summary”. These results summarize the effects of each fixed effect (and their interactions) on the dependent variable (shannon diversity). This table shows parameter estimates, estimate standard errors, z scores, P values (P>|z|), and 95% confidence interval upper and lower bounds for each parameter. We see in this table that shannon diversity is significantly impacted by month of life and by diet, as well as several interacting factors. More information about LME models and the interpretation of these data can be found on the statsmodels LME description page, which provides a number of useful technical references for further reading.
Finally, scatter plots categorized by each “group column” are shown at the bottom of the visualization, with linear regression lines (plus 95% confidence interval in grey) for each group. If --p-lowess is enabled, instead locally weighted averages are shown for each group. Two different groups of scatter plots are shown. First, regression scatterplots show the relationship between state_column (x-axis) and metric (y-axis) for each sample. These plots are just used as a quick summary for reference; users are recommended to use the volatility visualizer for interactive plotting of their longitudinal data. Volatility plots can be used to qualitatively identify outliers that disproportionately drive the variance within individuals and groups, including by inspecting residuals in relation to control limits (see note below and the section on “Volatility analysis” for more details).
The second set of scatterplots are fit vs. residual plots, which show the relationship between metric predictions for each sample (on the x-axis), and the residual or observation error (prediction - actual value) for each sample (on the y-axis). Residuals should be roughly zero-centered and normal across the range of measured values. Uncentered, systematically high or low, and autocorrelated values could indicate a poor model. If your residual plots look like an ugly mess without any apparent relationship between values, you are doing a good job. If you see a U-shaped curve or other non-random distribution, either your predictor variables (group_columns and/or random_effects) are failing to capture all explanatory information, causing information to leak into your residuals, or else you are not using an appropriate model for your data 🙁. Check your predictor variables and available metadata columns to make sure you aren’t missing anything.
Note
If you want to dot your i’s and cross your t’s, residual and predicted values for each sample can be obtained in the “Download raw data as tsv” link below the regression scatterplots. This file can be input as metadata to the volatility visualizer to check whether residuals are correlated with other metadata columns. If they are, those columns should probably be used as prediction variables in your model! Control limits (± 2 and 3 standard deviations) can be toggled on/off to easily identify outliers, which can be particularly useful for re-examining fit vs. residual plots with this visualizer. 🍝
Volatility analysis (from longitudinal analysis tutorial)
The volatility visualizer generates interactive line plots that allow us to assess how volatile a dependent variable is over a continuous, independent variable (e.g., time) in one or more groups. Multiple metadata files (including alpha and beta diversity artifacts) and FeatureTable[RelativeFrequency] tables can be used as input, and in the interactive visualization we can select different dependent variables to plot on the y-axis.
Here we examine how variance in Shannon diversity and other metadata changes across time (set with the state-column parameter) in the ECAM cohort, both in groups of samples (interactively selected as described below) and in individual subjects (set with the individual-id-column parameter).
qiime longitudinal volatility \
  --m-metadata-file ecam-sample-metadata.tsv \
  --m-metadata-file shannon.qza \
  --p-default-metric shannon \
  --p-default-group-column delivery \
  --p-state-column month \
  --p-individual-id-column studyid \
  --o-visualization volatility.qzv

*16SMAR2023
*Again, excluding site as a categorical variable here as we are mainly interested at the change over time itself
qiime longitudinal volatility \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M6only.txt \
  --m-metadata-file pk-sequencing-M2M4M6-only-core-metrics-results/Shannon_vector.qza \
  --p-default-metric shannon \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M2M6-only-volatility.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M2M6-only-volatility.qzv
In the resulting visualization, a line plot appears on the left-hand side of the plot and a panel of “Plot Controls” appears to the right. These “Plot Controls” interactively adjust several variables and parameters. This allows us to determine how groups’ and individuals’ values change across a single independent variable, state-column. Interective features in this visualization include:
	•	The “Metric column” tab lets us select which continuous metadata values to plot on the y-axis. All continuous numeric columns found in metadata/artifacts input to this action will appear as options in this drop-down tab. In this example, the initial variable plotted in the visualization is shannon diversity because this column was designated by the optional default-metric parameter.
	•	The “Group column” tab lets us select which categorical metadata values to use for calculating mean values. All categorical metadata columns found in metadata/artifacts input to this action will appear as options in this drop-down tab. These mean values are plotted on the line plot, and the thickness and opacity of these mean lines can be modified using the slider bars in the “Plot Controls” on the right-hand side of the visualization. Error bars (standard deviation) can be toggled on and off with a button in the “Plot Controls”.
	•	Longitudinal values for each individual subject are plotted as “spaghetti” lines (so-called because this tangled mass of individual vectors looks like a ball of spaghetti). The thickness and opacity of spaghetti can be modified using the slider bars in the “Plot Controls” on the right-hand side of the visualization.
	•	Color scheme can be adjusted using the “Color scheme” tab.
	•	Global mean and warning/control limits (2X and 3X standard deviations from global mean) can be toggled on/off with the buttons in the “Plot Controls”. The goal of plotting these values is to show how a variable is changing over time (or a gradient) in relation to the mean. Large departures from the mean values can cross the warning/control limits, indicating a major disruption at that state; for example, antibiotic use or other disturbances impacting diversity could be tracked with these plots.
	•	Group mean lines and spaghetti can also be modified with the “scatter size” and “scatter opacity” slider bars in the “Plot Controls”. These adjust the size and opacity of individual points. Maximize scatter opacity and minimize line opacity to transform these into longitudinal scatter plots!
	•	Relevant sample metadata at individual points can be viewed by hovering the mouse over a point of interest.
If the interactive features of this visualization don’t quite scratch your itch, click on the “Open in Vega Editor” button at the top of the “Plot Controls” and customize to your heart’s content. This opens a window for manually editing plot characteristics in Vega Editor, a visualization tool external to QIIME2.

Longitudinal Alpha diversity for M1 vs M2. The first step is to generate the alpha & beta diversity matrices needed for the longitudinal analyses.

qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M1M2only.txt \
  --p-where "[Study_arm_2] = 'Prospective' " \
  --o-filtered-table pk-sequencing-M1M2-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-sequencing-M1M2-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M1M2only.txt \
  --output-dir pk-sequencing-M1M2-only-core-metrics-results
Testing longitudinal analyses for alpha diversity 
Performing longitudinal and paired sample comparisons with q2-longitudinal
https://docs.qiime2.org/2019.10/tutorials/longitudinal/
Pairwise difference comparisons
Pairwise difference tests determine whether the value of a specific metric changed significantly between pairs of paired samples (e.g., pre- and post-treatment).
This visualizer currently supports comparison of feature abundance (e.g., microbial sequence variants or taxa) in a feature table, or of metadata values in a sample metadata file. Alpha diversity values (e.g., observed sequence variants) and beta diversity values (e.g., principal coordinates) are useful metrics for comparison with these tests, and should be contained in one of the metadata files given as input. In the example below, we will test whether alpha diversity (Shannon diversity index) changed significantly between two different time points in the ECAM data according to delivery mode. (Find the Shannon diversity artifact from your processing)
*Here, I use the metadata I downloaded with Shannon values for all samples as the mapping file
qiime longitudinal pairwise-differences \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-column Site \
  --p-state-column Collection \
  --p-state-1 1 \
  --p-state-2 2 \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-gut-pairwise-differences.qzv

*16SMAR2023
*Had to remove the ‘p-group-column” categorical variable as I am only interested in alpha diversity change over time itself. This changed the hypothesis to (is the change in alpha diversity itself between the timepoints different from 0?)
qiime longitudinal pairwise-differences \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M1M2only.txt \
  --m-metadata-file pk-sequencing-M1M2-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-state-column Time_point \
  --p-state-1 1 \
  --p-state-2 2 \
  --p-individual-id-column Patient_ID \
  --p-replicate-handling random \
  --o-visualization pk-sequencing-M1M2-pairwise-differences.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
**You need to visualize the file then export the raw values as TSV
qiime tools view pk-sequencing-M1M2-pairwise-differences.qzv
Linear mixed effect models
Linear mixed effects (LME) models test the relationship between a single response variable and one or more independent variables, where observations are made across dependent samples, e.g., in repeated-measures sampling experiments. This implementation takes at least one numeric state-column (e.g., Time) and one or more comma-separated group-columns (which may be categorical or numeric metadata columns; these are the fixed effects) as independent variables in a LME model, and plots regression plots of the response variable (“metric”) as a function of the state column and each group column. Additionally, the individual-id-column parameter should be a metadata column that indicates the individual subject/site that was sampled repeatedly. The response variable may either be a sample metadata mapping file column or a feature ID in the feature table. A comma-separated list of random effects can also be input to this action; a random intercept for each individual is included by default, but another common random effect that users may wish to use is a random slope for each individual, which can be set by using the state-column value as input to the random-effects parameter. Here we use LME to test whether alpha diversity (Shannon diversity index) changed over time and in response to delivery mode, diet, and sex in the ECAM data set.
Note
Deciding whether a factor is a fixed effect or a random effect can be complicated. In general, a factor should be a fixed effect if the different factor levels (metadata column values) represent (more or less) all possible discrete values. For example, delivery mode, sex, and diet (dominantly breast-fed or formula-fed) are designated as fixed effects in the example below. Conversely, a factor should be a random effect if its values represent random samples from a population. For example, we could imagine having metadata variables like body-weight, daily-kcal-from-breastmilk, number-of-peanuts-eaten-per-day, or mg-of-penicillin-administered-daily; such values would represent random samples from within a population, and are unlikely to capture all possible values representative of the whole population. Not sure about the factors in your experiment? 🤔 Consult a statistician or reputable statistical tome for guidance. 📚
*I am running a Linear Mixed effect model for the Site variable as a predictor of change in the overall study
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Site \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization oral-gut-shannon-linear-mixed-effects.qzv
*For this model, I am using a specific mapping file where I copied the matched oral shanon diversity values (from DADA2 in R) to the gut samples, and I used sorting by Subject ID and by Collection to make sure I had the matching samples. (I actually exported the values from the oral-gut-core-metrics file to a text file, and used it in R to do the same analysis)
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Map_Shannon_Dada.tsv \
  --m-metadata-file gut-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Shannon_Dada_Oral \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization gut-only-oral-shannon-linear-mixed-effects.qzv
*16SMAR2023 
*Trying a longitudinal model without the categorical variable “  --p-group-columns Site \” as a fixed effect. Alternatively, I can try using time as a fixed effect.
qiime longitudinal linear-mixed-effects \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M1M2only.txt \
   --m-metadata-file pk-sequencing-M1M2-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M1M2-shannon-linear-mixed-effects.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M1M2-shannon-linear-mixed-effects.qzv
The visualizer produced by this command contains several results. First, the input parameters are shown at the top of the visualization for convenience (e.g., when flipping through multiple visualizations it is useful to have a summary). Next, the “model summary” shows some descriptive information about the LME model that was trained. This just shows descriptive information about the “groups”; in this case, groups will be individuals (as set by the --p-individual-id-column). The main results to examine will be the “model results” below the “model summary”. These results summarize the effects of each fixed effect (and their interactions) on the dependent variable (shannon diversity). This table shows parameter estimates, estimate standard errors, z scores, P values (P>|z|), and 95% confidence interval upper and lower bounds for each parameter. We see in this table that shannon diversity is significantly impacted by month of life and by diet, as well as several interacting factors. More information about LME models and the interpretation of these data can be found on the statsmodels LME description page, which provides a number of useful technical references for further reading.
Finally, scatter plots categorized by each “group column” are shown at the bottom of the visualization, with linear regression lines (plus 95% confidence interval in grey) for each group. If --p-lowess is enabled, instead locally weighted averages are shown for each group. Two different groups of scatter plots are shown. First, regression scatterplots show the relationship between state_column (x-axis) and metric (y-axis) for each sample. These plots are just used as a quick summary for reference; users are recommended to use the volatility visualizer for interactive plotting of their longitudinal data. Volatility plots can be used to qualitatively identify outliers that disproportionately drive the variance within individuals and groups, including by inspecting residuals in relation to control limits (see note below and the section on “Volatility analysis” for more details).
The second set of scatterplots are fit vs. residual plots, which show the relationship between metric predictions for each sample (on the x-axis), and the residual or observation error (prediction - actual value) for each sample (on the y-axis). Residuals should be roughly zero-centered and normal across the range of measured values. Uncentered, systematically high or low, and autocorrelated values could indicate a poor model. If your residual plots look like an ugly mess without any apparent relationship between values, you are doing a good job. If you see a U-shaped curve or other non-random distribution, either your predictor variables (group_columns and/or random_effects) are failing to capture all explanatory information, causing information to leak into your residuals, or else you are not using an appropriate model for your data 🙁. Check your predictor variables and available metadata columns to make sure you aren’t missing anything.
Note
If you want to dot your i’s and cross your t’s, residual and predicted values for each sample can be obtained in the “Download raw data as tsv” link below the regression scatterplots. This file can be input as metadata to the volatility visualizer to check whether residuals are correlated with other metadata columns. If they are, those columns should probably be used as prediction variables in your model! Control limits (± 2 and 3 standard deviations) can be toggled on/off to easily identify outliers, which can be particularly useful for re-examining fit vs. residual plots with this visualizer. 🍝
Volatility analysis (from longitudinal analysis tutorial)
The volatility visualizer generates interactive line plots that allow us to assess how volatile a dependent variable is over a continuous, independent variable (e.g., time) in one or more groups. Multiple metadata files (including alpha and beta diversity artifacts) and FeatureTable[RelativeFrequency] tables can be used as input, and in the interactive visualization we can select different dependent variables to plot on the y-axis.
Here we examine how variance in Shannon diversity and other metadata changes across time (set with the state-column parameter) in the ECAM cohort, both in groups of samples (interactively selected as described below) and in individual subjects (set with the individual-id-column parameter).
qiime longitudinal volatility \
  --m-metadata-file ecam-sample-metadata.tsv \
  --m-metadata-file shannon.qza \
  --p-default-metric shannon \
  --p-default-group-column delivery \
  --p-state-column month \
  --p-individual-id-column studyid \
  --o-visualization volatility.qzv

*16SMAR2023
*Again, excluding site as a categorical variable here as we are mainly interested at the change over time itself
qiime longitudinal volatility \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M1M2only.txt \
  --m-metadata-file pk-sequencing-M1M2-only-core-metrics-results/Shannon_vector.qza \
  --p-default-metric shannon \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M1M2-only-volatility.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M1M2-only-volatility.qzv
In the resulting visualization, a line plot appears on the left-hand side of the plot and a panel of “Plot Controls” appears to the right. These “Plot Controls” interactively adjust several variables and parameters. This allows us to determine how groups’ and individuals’ values change across a single independent variable, state-column. Interective features in this visualization include:
	•	The “Metric column” tab lets us select which continuous metadata values to plot on the y-axis. All continuous numeric columns found in metadata/artifacts input to this action will appear as options in this drop-down tab. In this example, the initial variable plotted in the visualization is shannon diversity because this column was designated by the optional default-metric parameter.
	•	The “Group column” tab lets us select which categorical metadata values to use for calculating mean values. All categorical metadata columns found in metadata/artifacts input to this action will appear as options in this drop-down tab. These mean values are plotted on the line plot, and the thickness and opacity of these mean lines can be modified using the slider bars in the “Plot Controls” on the right-hand side of the visualization. Error bars (standard deviation) can be toggled on and off with a button in the “Plot Controls”.
	•	Longitudinal values for each individual subject are plotted as “spaghetti” lines (so-called because this tangled mass of individual vectors looks like a ball of spaghetti). The thickness and opacity of spaghetti can be modified using the slider bars in the “Plot Controls” on the right-hand side of the visualization.
	•	Color scheme can be adjusted using the “Color scheme” tab.
	•	Global mean and warning/control limits (2X and 3X standard deviations from global mean) can be toggled on/off with the buttons in the “Plot Controls”. The goal of plotting these values is to show how a variable is changing over time (or a gradient) in relation to the mean. Large departures from the mean values can cross the warning/control limits, indicating a major disruption at that state; for example, antibiotic use or other disturbances impacting diversity could be tracked with these plots.
	•	Group mean lines and spaghetti can also be modified with the “scatter size” and “scatter opacity” slider bars in the “Plot Controls”. These adjust the size and opacity of individual points. Maximize scatter opacity and minimize line opacity to transform these into longitudinal scatter plots!
	•	Relevant sample metadata at individual points can be viewed by hovering the mouse over a point of interest.
If the interactive features of this visualization don’t quite scratch your itch, click on the “Open in Vega Editor” button at the top of the “Plot Controls” and customize to your heart’s content. This opens a window for manually editing plot characteristics in Vega Editor, a visualization tool external to QIIME2.

Longitudinal Alpha diversity for M4 vs M6. The first step is to generate the alpha & beta diversity matrices needed for the longitudinal analyses.

qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 43_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M4M6.txt \
  --p-where "[Study_arm_2] = 'Prospective' " \
  --o-filtered-table pk-sequencing-M4M6-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-sequencing-M4M6-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file 43_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M4M6.txt \
  --output-dir pk-sequencing-M4M6-only-core-metrics-results
Testing longitudinal analyses for alpha diversity 
Performing longitudinal and paired sample comparisons with q2-longitudinal
https://docs.qiime2.org/2019.10/tutorials/longitudinal/
Pairwise difference comparisons
Pairwise difference tests determine whether the value of a specific metric changed significantly between pairs of paired samples (e.g., pre- and post-treatment).
This visualizer currently supports comparison of feature abundance (e.g., microbial sequence variants or taxa) in a feature table, or of metadata values in a sample metadata file. Alpha diversity values (e.g., observed sequence variants) and beta diversity values (e.g., principal coordinates) are useful metrics for comparison with these tests, and should be contained in one of the metadata files given as input. In the example below, we will test whether alpha diversity (Shannon diversity index) changed significantly between two different time points in the ECAM data according to delivery mode. (Find the Shannon diversity artifact from your processing)
*Here, I use the metadata I downloaded with Shannon values for all samples as the mapping file
qiime longitudinal pairwise-differences \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-column Site \
  --p-state-column Collection \
  --p-state-1 1 \
  --p-state-2 2 \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-gut-pairwise-differences.qzv

*16SMAR2023
*Had to remove the ‘p-group-column” categorical variable as I am only interested in alpha diversity change over time itself. This changed the hypothesis to (is the change in alpha diversity itself between the timepoints different from 0?)
qiime longitudinal pairwise-differences \
  --m-metadata-file 43_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M4M6.txt \
  --m-metadata-file pk-sequencing-M4M6-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-state-column Time_point \
  --p-state-1 4 \
  --p-state-2 6 \
  --p-individual-id-column Patient_ID \
  --p-replicate-handling random \
  --o-visualization pk-sequencing-M4M6-pairwise-differences.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
**You need to visualize the file then export the raw values as TSV
qiime tools view pk-sequencing-M4M6-pairwise-differences.qzv
Linear mixed effect models
Linear mixed effects (LME) models test the relationship between a single response variable and one or more independent variables, where observations are made across dependent samples, e.g., in repeated-measures sampling experiments. This implementation takes at least one numeric state-column (e.g., Time) and one or more comma-separated group-columns (which may be categorical or numeric metadata columns; these are the fixed effects) as independent variables in a LME model, and plots regression plots of the response variable (“metric”) as a function of the state column and each group column. Additionally, the individual-id-column parameter should be a metadata column that indicates the individual subject/site that was sampled repeatedly. The response variable may either be a sample metadata mapping file column or a feature ID in the feature table. A comma-separated list of random effects can also be input to this action; a random intercept for each individual is included by default, but another common random effect that users may wish to use is a random slope for each individual, which can be set by using the state-column value as input to the random-effects parameter. Here we use LME to test whether alpha diversity (Shannon diversity index) changed over time and in response to delivery mode, diet, and sex in the ECAM data set.
Note
Deciding whether a factor is a fixed effect or a random effect can be complicated. In general, a factor should be a fixed effect if the different factor levels (metadata column values) represent (more or less) all possible discrete values. For example, delivery mode, sex, and diet (dominantly breast-fed or formula-fed) are designated as fixed effects in the example below. Conversely, a factor should be a random effect if its values represent random samples from a population. For example, we could imagine having metadata variables like body-weight, daily-kcal-from-breastmilk, number-of-peanuts-eaten-per-day, or mg-of-penicillin-administered-daily; such values would represent random samples from within a population, and are unlikely to capture all possible values representative of the whole population. Not sure about the factors in your experiment? 🤔 Consult a statistician or reputable statistical tome for guidance. 📚
*I am running a Linear Mixed effect model for the Site variable as a predictor of change in the overall study
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Site \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization oral-gut-shannon-linear-mixed-effects.qzv
*For this model, I am using a specific mapping file where I copied the matched oral shanon diversity values (from DADA2 in R) to the gut samples, and I used sorting by Subject ID and by Collection to make sure I had the matching samples. (I actually exported the values from the oral-gut-core-metrics file to a text file, and used it in R to do the same analysis)
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Map_Shannon_Dada.tsv \
  --m-metadata-file gut-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Shannon_Dada_Oral \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization gut-only-oral-shannon-linear-mixed-effects.qzv
*16SMAR2023 
*Trying a longitudinal model without the categorical variable “  --p-group-columns Site \” as a fixed effect. Alternatively, I can try using time as a fixed effect.
qiime longitudinal linear-mixed-effects \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M4M6only.txt \
   --m-metadata-file pk-sequencing-M4M6-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M4M6-shannon-linear-mixed-effects.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M4M6-shannon-linear-mixed-effects.qzv
The visualizer produced by this command contains several results. First, the input parameters are shown at the top of the visualization for convenience (e.g., when flipping through multiple visualizations it is useful to have a summary). Next, the “model summary” shows some descriptive information about the LME model that was trained. This just shows descriptive information about the “groups”; in this case, groups will be individuals (as set by the --p-individual-id-column). The main results to examine will be the “model results” below the “model summary”. These results summarize the effects of each fixed effect (and their interactions) on the dependent variable (shannon diversity). This table shows parameter estimates, estimate standard errors, z scores, P values (P>|z|), and 95% confidence interval upper and lower bounds for each parameter. We see in this table that shannon diversity is significantly impacted by month of life and by diet, as well as several interacting factors. More information about LME models and the interpretation of these data can be found on the statsmodels LME description page, which provides a number of useful technical references for further reading.
Finally, scatter plots categorized by each “group column” are shown at the bottom of the visualization, with linear regression lines (plus 95% confidence interval in grey) for each group. If --p-lowess is enabled, instead locally weighted averages are shown for each group. Two different groups of scatter plots are shown. First, regression scatterplots show the relationship between state_column (x-axis) and metric (y-axis) for each sample. These plots are just used as a quick summary for reference; users are recommended to use the volatility visualizer for interactive plotting of their longitudinal data. Volatility plots can be used to qualitatively identify outliers that disproportionately drive the variance within individuals and groups, including by inspecting residuals in relation to control limits (see note below and the section on “Volatility analysis” for more details).
The second set of scatterplots are fit vs. residual plots, which show the relationship between metric predictions for each sample (on the x-axis), and the residual or observation error (prediction - actual value) for each sample (on the y-axis). Residuals should be roughly zero-centered and normal across the range of measured values. Uncentered, systematically high or low, and autocorrelated values could indicate a poor model. If your residual plots look like an ugly mess without any apparent relationship between values, you are doing a good job. If you see a U-shaped curve or other non-random distribution, either your predictor variables (group_columns and/or random_effects) are failing to capture all explanatory information, causing information to leak into your residuals, or else you are not using an appropriate model for your data 🙁. Check your predictor variables and available metadata columns to make sure you aren’t missing anything.
Note
If you want to dot your i’s and cross your t’s, residual and predicted values for each sample can be obtained in the “Download raw data as tsv” link below the regression scatterplots. This file can be input as metadata to the volatility visualizer to check whether residuals are correlated with other metadata columns. If they are, those columns should probably be used as prediction variables in your model! Control limits (± 2 and 3 standard deviations) can be toggled on/off to easily identify outliers, which can be particularly useful for re-examining fit vs. residual plots with this visualizer. 🍝
Volatility analysis (from longitudinal analysis tutorial)
The volatility visualizer generates interactive line plots that allow us to assess how volatile a dependent variable is over a continuous, independent variable (e.g., time) in one or more groups. Multiple metadata files (including alpha and beta diversity artifacts) and FeatureTable[RelativeFrequency] tables can be used as input, and in the interactive visualization we can select different dependent variables to plot on the y-axis.
Here we examine how variance in Shannon diversity and other metadata changes across time (set with the state-column parameter) in the ECAM cohort, both in groups of samples (interactively selected as described below) and in individual subjects (set with the individual-id-column parameter).
qiime longitudinal volatility \
  --m-metadata-file ecam-sample-metadata.tsv \
  --m-metadata-file shannon.qza \
  --p-default-metric shannon \
  --p-default-group-column delivery \
  --p-state-column month \
  --p-individual-id-column studyid \
  --o-visualization volatility.qzv

*16SMAR2023
*Again, excluding site as a categorical variable here as we are mainly interested at the change over time itself
qiime longitudinal volatility \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M4M6only.txt \
  --m-metadata-file pk-sequencing-M4M6-only-core-metrics-results/Shannon_vector.qza \
  --p-default-metric shannon \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M4M6-only-volatility.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M4M6-only-volatility.qzv
In the resulting visualization, a line plot appears on the left-hand side of the plot and a panel of “Plot Controls” appears to the right. These “Plot Controls” interactively adjust several variables and parameters. This allows us to determine how groups’ and individuals’ values change across a single independent variable, state-column. Interective features in this visualization include:
	•	The “Metric column” tab lets us select which continuous metadata values to plot on the y-axis. All continuous numeric columns found in metadata/artifacts input to this action will appear as options in this drop-down tab. In this example, the initial variable plotted in the visualization is shannon diversity because this column was designated by the optional default-metric parameter.
	•	The “Group column” tab lets us select which categorical metadata values to use for calculating mean values. All categorical metadata columns found in metadata/artifacts input to this action will appear as options in this drop-down tab. These mean values are plotted on the line plot, and the thickness and opacity of these mean lines can be modified using the slider bars in the “Plot Controls” on the right-hand side of the visualization. Error bars (standard deviation) can be toggled on and off with a button in the “Plot Controls”.
	•	Longitudinal values for each individual subject are plotted as “spaghetti” lines (so-called because this tangled mass of individual vectors looks like a ball of spaghetti). The thickness and opacity of spaghetti can be modified using the slider bars in the “Plot Controls” on the right-hand side of the visualization.
	•	Color scheme can be adjusted using the “Color scheme” tab.
	•	Global mean and warning/control limits (2X and 3X standard deviations from global mean) can be toggled on/off with the buttons in the “Plot Controls”. The goal of plotting these values is to show how a variable is changing over time (or a gradient) in relation to the mean. Large departures from the mean values can cross the warning/control limits, indicating a major disruption at that state; for example, antibiotic use or other disturbances impacting diversity could be tracked with these plots.
	•	Group mean lines and spaghetti can also be modified with the “scatter size” and “scatter opacity” slider bars in the “Plot Controls”. These adjust the size and opacity of individual points. Maximize scatter opacity and minimize line opacity to transform these into longitudinal scatter plots!
	•	Relevant sample metadata at individual points can be viewed by hovering the mouse over a point of interest.
If the interactive features of this visualization don’t quite scratch your itch, click on the “Open in Vega Editor” button at the top of the “Plot Controls” and customize to your heart’s content. This opens a window for manually editing plot characteristics in Vega Editor, a visualization tool external to QIIME2.
Alpha rarefaction plotting
In this section we’ll explore alpha diversity as a function of sampling depth using the qiime diversity alpha-rarefaction visualizer. This visualizer computes one or more alpha diversity metrics at multiple sampling depths, in steps between 1 (optionally controlled with --p-min-depth) and the value provided as --p-max-depth. At each sampling depth step, 10 rarefied tables will be generated, and the diversity metrics will be computed for all samples in the tables. The number of iterations (rarefied tables computed at each sampling depth) can be controlled with --p-iterations. Average diversity values will be plotted for each sample at each even sampling depth, and samples can be grouped based on metadata in the resulting visualization if sample metadata is provided with the --m-metadata-file parameter.
qiime diversity alpha-rarefaction \
  --i-table oral-table.qza \
  --i-phylogeny oral-rooted-tree.qza \
  --p-max-depth 5900 \
  --m-metadata-file Aspirin_Oral_Map.tsv \
  --o-visualization oral-alpha-rarefaction.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view oral-alpha-rarefaction.qzv
The visualization will have two plots. The top plot is an alpha rarefaction plot, and is primarily used to determine if the richness of the samples has been fully observed or sequenced. If the lines in the plot appear to “level out” (i.e., approach a slope of zero) at some sampling depth along the x-axis, that suggests that collecting additional sequences beyond that sampling depth would not be likely to result in the observation of additional features. If the lines in a plot don’t level out, this may be because the richness of the samples hasn’t been fully observed yet (because too few sequences were collected), or it could be an indicator that a lot of sequencing error remains in the data (which is being mistaken for novel diversity).
The bottom plot in this visualization is important when grouping samples by metadata. It illustrates the number of samples that remain in each group when the feature table is rarefied to each sampling depth. If a given sampling depth d is larger than the total frequency of a sample s (i.e., the number of sequences that were obtained for sample s), it is not possible to compute the diversity metric for sample s at sampling depth d. If many of the samples in a group have lower total frequencies than d, the average diversity presented for that group at d in the top plot will be unreliable because it will have been computed on relatively few samples. When grouping samples by metadata, it is therefore essential to look at the bottom plot to ensure that the data presented in the top plot is reliable.
Note
The value that you provide for --p-max-depth should be determined by reviewing the “Frequency per sample” information presented in the table.qzv file that was created above. In general, choosing a value that is somewhere around the median frequency seems to work well, but you may want to increase that value if the lines in the resulting rarefaction plot don’t appear to be leveling out, or decrease that value if you seem to be losing many of your samples due to low total frequencies closer to the minimum sampling depth than the maximum sampling depth.
Question
When grouping samples by “body-site” and viewing the alpha rarefaction plot for the “observed_otus” metric, which body sites (if any) appear to exhibit sufficient diversity coverage (i.e., their rarefaction curves level off)? How many sequence variants appear to be present in those body sites?
Question
When grouping samples by “body-site” and viewing the alpha rarefaction plot for the “observed_otus” metric, the line for the “right palm” samples appears to level out at about 40, but then jumps to about 140. What do you think is happening here? (Hint: be sure to look at both the top and bottom plots.)

Beta diversity for the bray Curtis variable
qiime diversity beta-group-significance \
  --i-distance-matrix oral-gut-pre-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --m-metadata-column Site \
  --o-visualization oral-gut-pre-core-metrics-results/bray-curtis-site-pre-significance.qzv \
  --p-pairwise

qiime diversity beta-group-significance \
  --i-distance-matrix oral-gut-post-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --m-metadata-column Site \
  --o-visualization oral-gut-post-core-metrics-results/bray-curtis-site-post-significance.qzv \
  --p-pairwise

*Repeating the analysis for the dental microbiome paper
qiime diversity beta-group-significance \
  --i-distance-matrix hhridental-noAR1-core-metrics-results2/bray_curtis_distance_matrix.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --m-metadata-column bodysite \
  --o-visualization hhridental-noAR1-core-metrics-results2/bray-curtis-site-significance.qzv \
  --p-pairwise

qiime diversity beta-group-significance \
  --i-distance-matrix hhridental-PLA-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --m-metadata-column Cohort1 \
  --o-visualization hhridental-PLA-core-metrics-results/bray-curtis-site-significance.qzv \
  --p-pairwise

qiime diversity beta-group-significance \
  --i-distance-matrix hhridental-female-core-metrics-results/unweighted_unifrac_distance_matrix.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --m-metadata-column Cohort1 \
  --o-visualization hhridental-female-core-metrics-results/unweighted-unifrac-significance.qzv \
  --p-pairwise

qiime diversity beta-group-significance \
  --i-distance-matrix hhridental-female-core-metrics-results/weighted_unifrac_distance_matrix.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --m-metadata-column Cohort1 \
  --o-visualization hhridental-female-core-metrics-results/weighted-unifrac-significance.qzv \
  --p-pairwise

*16S dataset

qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --output-dir pk-only-core-metrics-results

**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls pk-only-core-metrics-results/

**Getting the Shannon value for all the sample, then creating a new mapping file so you can use that as a covariate
qiime diversity alpha-group-significance \
  --i-alpha-diversity pk-only-core-metrics-results/shannon_vector.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --o-visualization pk-only-core-metrics-results/shannon-group-significance.qzv

**You need to visualize the file then export the raw values as TSV
qiime tools view pk-only-core-metrics-results/shannon-group-significance.qzv

qiime diversity beta-group-significance \
  --i-distance-matrix pk-only-core-metrics-results/weighted_unifrac_distance_matrix.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --m-metadata-column EHR_Cat_invitro \
  --o-visualization pk-only-core-metrics-results/weighted-unifrac-significance.qzv \
  --p-pairwise

Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view oral-gut-pre-core-metrics-results/bray-curtis-site-pre-significance.qzv
qiime tools view oral-gut-post-core-metrics-results/bray-curtis-site-post-significance.qzv

qiime tools view hhridental-noAR1-core-metrics-results2/bray-curtis-site-significance.qzv
qiime tools view hhridental-PLA-core-metrics-results/bray-curtis-site-significance.qzv

qiime tools view hhridental-female-core-metrics-results/unweighted-unifrac-significance.qzv
qiime tools view hhridental-female-core-metrics-results/weighted-unifrac-significance.qzv

qiime tools view pk-only-core-metrics-results/weighted-unifrac-significance.qzv
Question
Are the associations between subjects and differences in microbial composition statistically significant? How about body sites? What specific pairs of body sites are significantly different from each other?

Emperor plot for beta distance matrices (must be a continuous variable)
Trying it out for collection
qiime emperor plot \
  --i-pcoa gut-only-core-metrics-results/bray_curtis_pcoa_results.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map_Shannon_Dada.tsv \
  --p-custom-axes Shannon_Dada_Oral \
  --o-visualization gut-only-core-metrics-results/bray-curtis-emperor-gut-shannon.qzv

qiime emperor plot \
  --i-pcoa hhridental-noAR1-core-metrics-results2/bray_curtis_pcoa_results.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --p-custom-axes age_at_tx \
  --o-visualization hhridental-noAR1-core-metrics-results2/bray-curtis-emperor-dental.qzv

qiime emperor plot \
  --i-pcoa hhridental-PLA-core-metrics-results/bray_curtis_pcoa_results.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --p-custom-axes age_at_tx \
  --o-visualization hhridental-PLA-core-metrics-results/bray-curtis-emperor-plaque.qzv

qiime emperor plot \
  --i-pcoa hhridental-female-core-metrics-results/unweighted_unifrac_pcoa_results.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --o-visualization hhridental-female-core-metrics-results/unweighted-unifrac-emperor2.qzv

qiime emperor plot \
  --i-pcoa hhridental-female-core-metrics-results/weighted_unifrac_pcoa_results.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --o-visualization hhridental-female-core-metrics-results/weighted-unifrac-emperor2.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view gut-only-core-metrics-results/bray-curtis-emperor-gut-shannon.qzv

qiime tools view hhridental-noAR1-core-metrics-results2/bray-curtis-emperor-dental.qzv
qiime tools view hhridental-PLA-core-metrics-results/bray-curtis-emperor-plaque.qzv

qiime tools view hhridental-female-core-metrics-results/unweighted-unifrac-emperor2.qzv
qiime tools view hhridental-female-core-metrics-results/weighted-unifrac-emperor2.qzv
Testing beta longitudinal analyses on the asmic oral full dataset
Performing longitudinal and paired sample comparisons with q2-longitudinal
https://docs.qiime2.org/2019.10/tutorials/longitudinal/

Pairwise distance comparisons

The pairwise-distances visualizer also assesses changes between paired samples from two different “states”, but instead of taking a metadata column or artifact as input, it operates on a distance matrix to assess the distance between “pre” and “post” sample pairs, and tests whether these paired differences are significantly different between different groups, as specified by the group-column parameter. 

qiime longitudinal pairwise-distances \
  --i-distance-matrix oral-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file Aspirin_Oral_Map.tsv \
  --p-group-column Contents \
  --p-state-column Collection \
  --p-state-1 1 \
  --p-state-2 2 \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-bray-pairwise-distances.qzv

qiime longitudinal pairwise-distances \
  --i-distance-matrix oral-gut-asa-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --p-group-column Site \
  --p-state-column Collection \
  --p-state-1 1 \
  --p-state-2 2 \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-gut-asa-pairwise-distances.qzv

qiime longitudinal pairwise-distances \
  --i-distance-matrix oral-gut-pcb-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --p-group-column Site \
  --p-state-column Collection \
  --p-state-1 1 \
  --p-state-2 2 \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-gut-pcb-pairwise-distances.qzv

qiime longitudinal pairwise-distances \
  --i-distance-matrix oral-gut-pre-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --p-group-column Contents \
  --p-state-column Site \
  --p-state-1  'Oral'  \
  --p-state-2 'Gut' \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-gut-pre-pairwise-distances_test.qzv

qiime longitudinal pairwise-distances \
  --i-distance-matrix oral-gut-post-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --p-group-column Contents \
  --p-state-column Site \
  --p-state-1  'Oral'  \
  --p-state-2 'Gut' \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-gut-post-pairwise-distances_test.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view oral-gut-asa-pairwise-distances.qzv
qiime tools view oral-gut-pcb-pairwise-distances.qzv
qiime tools view oral-gut-pre-pairwise-distances_test.qzv
qiime tools view oral-gut-post-pairwise-distances_test.qzv
PCoA-based analyses
We can start by exploring temporal change in the PCoA using the animations tab.
Question
	•	Open the unweighted UniFrac emperor plot and color the samples by mouse id. Click on the “animations” tab and animate using the day_post_transplant as your gradient and mouse_id as your trajectory. Do you observe any clear temporal trends based on the PCoA?
	•	What happens if you color by day_post_transplant? Do you see a difference based on the day? Hint: Trying changing the colormap to a sequential colormap like viridis.
A volatility plot will let us look at patterns of variation along principle coordinate axes starting from the same point. This can be helpful since inter-individual variation can be large and this visualizations lets us focus instead on magnitude of change in each group and in each individual.
Let’s use the q2-longitudinal plugin to look at how samples from an individual move along each PC. The --m-metadata-file column can take several types, including a metadata file (like our metadata.tsv) as well as a SampleData[AlphaDiversity], SampleData[Distance] (files “viewable” as metadata), or a PCoA artifact.
qiime longitudinal volatility \
  --m-metadata-file Aspirin_Oral_Map.tsv \
  --m-metadata-file oral-core-metrics-results/bray_curtis_pcoa_results.qza \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --p-default-group-column 'Contents' \
  --p-default-metric 'Axis 2' \
  --o-visualization oral_bray_curtis_volatility.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view oral_bray_curtis_volatility.qzv
A linear mixed effects (LME) model lets us test whether there’s a relationship between a dependent variable and one or more independent variables in an experiment using repeated measures. Since we’re interested in genotype, we should use this as an independent predictor.
For our experiment, we’re currently interested in the change in distance from the initial timepoint, so we’ll use this as our outcome variable (given by --p-metric).
The linear-mixed-effects action also requires a state column (--p-state-column) which designates the time component in the metadata, and an individual identifier (--p-individual-id-column). Which columns should we use in our data?
We can build a model either using the --p-formula parameter or the --p-group-columns parameter. For this analysis, we’re interested in whether genotype affects the longitudinal change in the microbial community. However, we also know from our cross sectional analysis that donor plays a large role in shaping the fecal community. So, we should also probably include that in this analysis. We may also want to consider cage effect in our experiment, since this is a common confounder in rodent studies. However, the original experimental design here was clever: although cages were grouped by donor (mice are coprophagic), they were of mixed genotype. This partial randomization helps limit some of the cage effects we might otherwise see.
First differencing to track rate of change
Another way to view time series data is by assessing how the rate of change differs over time. We can do this through calculating first differences, which is the magnitude of change between successive time points. If YtYt is the value of metric YY at time tt, the first difference at time tt, ΔYt=Yt−Yt-1ΔYt=Yt−Yt-1. This calculation is performed at fixed intervals, so for each interval ΔYtΔYt is not calculated for subjects that are missing samples at times tt or t−1t−1. This transformation is performed in the first-differences method in q2-longitudinal.
qiime longitudinal first-differences \
  --m-metadata-file ecam-sample-metadata.tsv \
  --m-metadata-file shannon.qza \
  --p-state-column month \
  --p-metric shannon \
  --p-individual-id-column studyid \
  --p-replicate-handling random \
  --o-first-differences shannon-first-differences.qza
This outputs a SampleData[FirstDifferences] artifact, which can then be viewed, e.g., with the volatility visualizer or analyzed with linear-mixed-effects or other methods.
A similar method is first-distances, which instead identifies the beta diversity distances between successive samples from the same subject. The pairwise distance between all samples can already be calculated by the beta or core-metrics methods in q2-diversity, so this method simply identifies the distances between successive samples collected from the same subject and outputs this series of values as metadata that can be consumed by other methods.
qiime longitudinal first-distances \
  --i-distance-matrix oral-core-metrics-results/unweighted_unifrac_distance_matrix.qza \
  --m-metadata-file Aspirin_Oral_Map.tsv \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-first-distances unweighted-first-distances.qza
This output can be used in the same way as the output of first-differences. The output of first-distances is particularly empowering, though, because it allows us to analyze longitudinal changes in beta diversity using actions that cannot operate directly on a distance matrix, such as linear-mixed-effects. *** This does not work with the asmic oral data because you are computing a distance base and the resulting file only has 1 “state” since it is a pre-post analysis
qiime longitudinal linear-mixed-effects \
  --m-metadata-file unweighted-first-distances.qza \
  --m-metadata-file Aspirin_Oral_Map.tsv \
  --p-metric Distance \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --p-group-columns Contents \
  --o-visualization unweighted-first-distances-LME.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view unweighted-first-distances-LME.qzv

Beta diversity for the bray Curtis variable for the PK & Sequencing datasets
qiime diversity beta-group-significance \
  --i-distance-matrix oral-gut-pre-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --m-metadata-column Site \
  --o-visualization oral-gut-pre-core-metrics-results/bray-curtis-site-pre-significance.qzv \
  --p-pairwise

*MAR2023 16S
qiime phylogeny align-to-tree-mafft-fasttree \
  --i-sequences hhri16s-rep-seqs.qza \
  --o-alignment aligned-hhri16s-rep-seqs.qza \
  --o-masked-alignment masked-aligned-hhri16s-rep-seqs.qza \
  --o-tree hhri16s-unrooted-tree.qza \
  --o-rooted-tree hhri16s-rooted-tree.qza
ls

*Making a table for the samples for which we have both sequencing and PK data from Moataz
*Found at “/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/18_HHRI_UMGC_Projects/04_12192022_Israni4_Project_019and020_16sSequencing/Documents/41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt” using the alt command after rightclicking to copy as a pathname.
qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --p-where "[AUC_sequencing] = 'Both' " \
  --o-filtered-table pk-sequencing-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-sequencing-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --output-dir pk-sequencing-only-core-metrics-results

qiime diversity beta-group-significance \
  --i-distance-matrix pk-sequencing-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column Study_arm_2 \
  --o-visualization pk-sequencing-only-core-metrics-results/bray-curtis-pk-sequencing-only-study-significance.qzv \
  --p-pairwise

qiime diversity beta-group-significance \
  --i-distance-matrix pk-sequencing-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_AUC_0_12_dose_adj_cat \
  --o-visualization pk-sequencing-only-core-metrics-results/bray-curtis-pk-sequencing-only-AUC-significance.qzv \
  --p-pairwise

qiime diversity beta-group-significance \
  --i-distance-matrix pk-sequencing-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_EHR_5_12_cat \
  --o-visualization pk-sequencing-only-core-metrics-results/bray-curtis-pk-sequencing-only-EHR-significance.qzv \
  --p-pairwise
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-only-core-metrics-results/bray-curtis-pk-sequencing-only-study-significance.qzv
qiime tools view pk-sequencing-only-core-metrics-results/bray-curtis-pk-sequencing-only-AUC-significance.qzv
qiime tools view pk-sequencing-only-core-metrics-results/bray-curtis-pk-sequencing-only-EHR-significance.qzv
**For the NOV2023 analysis, I will need to create a variable that indicates which participants have both the diarrhea data and the 16S data, as well as the subset for those who have the moataz pk data as well. I created the variable “mosio_sequencing” in column UN, and “mosio_sequencing_pk” in column VI. I saved the mapping file as /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/48_MAP_MISSION_16S_sample_pull_mappingtemplate_NOV2023_PKMAP_nodups_V5.txt

*NOV2023 16S
qiime phylogeny align-to-tree-mafft-fasttree \
  --i-sequences hhri16s-rep-seqs.qza \
  --o-alignment aligned-hhri16s-rep-seqs.qza \
  --o-masked-alignment masked-aligned-hhri16s-rep-seqs.qza \
  --o-tree hhri16s-unrooted-tree.qza \
  --o-rooted-tree hhri16s-rooted-tree.qza
ls

*Making a table for the samples for which we have both sequencing and PK data from Moataz
*Found at “/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/18_HHRI_UMGC_Projects/04_12192022_Israni4_Project_019and020_16sSequencing/Documents/41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt” using the alt command after rightclicking to copy as a pathname.
qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 48_MAP_MISSION_16S_sample_pull_mappingtemplate_NOV2023_PKMAP_nodups_V5.txt\
  --p-where "[mosio_sequencing] = 1 " \
  --o-filtered-table mosio-sequencing-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table! Set the sampling depth to 10000 based on the qzv description after generating the table
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table mosio-sequencing-only-table.qza \
  --p-sampling-depth 10000 \
  --m-metadata-file 48_MAP_MISSION_16S_sample_pull_mappingtemplate_NOV2023_PKMAP_nodups_V5.txt\
  --output-dir mosio-sequencing-only-only-core-metrics-results

**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls mosio-sequencing-only-only-core-metrics-results/

**Getting the Bray-curtis value for all the samples, then creating a new mapping file so you can use that as a covariate. You need a variable name for the beta diversity group significance, or you can go to the core-metrics-results folder above and visualize the emperor-qzv file. I chose to create a categorical version of the everCTCAE21 variable
qiime diversity beta-group-significance \
  --i-distance-matrix mosio-sequencing-only-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 48_MAP_MISSION_16S_sample_pull_mappingtemplate_NOV2023_PKMAP_nodups_V6.txt\
  --m-metadata-column everCTCAE21categ \
  --o-visualization mosio-sequencing-only-only-core-metrics-results/everCTCAE-bray-curtis-group-significance.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
**You need to visualize the file then export the raw values as TSV
qiime tools view pk-sequencing-only-core-metrics-results/shannon-group-significance.qzv
qiime tools view mosio-sequencing-only-only-core-metrics-results/bray_curtis_emperor.qzv
qiime tools view mosio-sequencing-only-only-core-metrics-results/everCTCAE-bray-curtis-group-significance.qzv
**Based on the beta diversity tests above, I will need to rerun the betadiversity subset to only M1 and M2/PK samples.
MOSIO beta diversity M1
*Making a table for the samples for which we have both sequencing and PK data from Moataz
qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --p-where "[mosio_sequencing] = 1 " \
  --o-filtered-table m1-mosio-sequencing-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table! Set the sampling depth to 10000 based on the qzv description after generating the table
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table m1-mosio-sequencing-only-table.qza \
  --p-sampling-depth 25000 \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --output-dir m1-25k-mosio-sequencing-only-only-core-metrics-results

**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls m1-25k-mosio-sequencing-only-only-core-metrics-results/

**Getting the braycurtis value for all the sample, then creating a new mapping file so you can use that as a covariate
qiime diversity beta-group-significance \
  --i-distance-matrix m1-25k-mosio-sequencing-only-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
 --m-metadata-column everdiarrhea1categ \
  --o-visualization m1-25k-mosio-sequencing-only-only-core-metrics-results/everdiarrheacateg-braycurtis-group-significance.qzv

qiime diversity beta-group-significance \
  --i-distance-matrix m1-25k-mosio-sequencing-only-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
 --m-metadata-column everdiarrhea1timeframe \
  --o-visualization m1-25k-mosio-sequencing-only-only-core-metrics-results/everdiarrheatimeframe-braycurtis-group-significance.qzv

qiime diversity beta-group-significance \
  --i-distance-matrix m1-25k-mosio-sequencing-only-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
 --m-metadata-column everCTCAE21categ \
  --o-visualization m1-25k-mosio-sequencing-only-only-core-metrics-results/everCTCAE2-braycurtis-group-significance.qzv
qiime diversity beta-group-significance \
  --i-distance-matrix m1-25k-mosio-sequencing-only-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
 --m-metadata-column everCTCAE21timeframe \
  --o-visualization m1-25k-mosio-sequencing-only-only-core-metrics-results/everCTCAE2timeframe-braycurtis-group-significance.qzv

qiime diversity beta-group-significance \
  --i-distance-matrix m1-25k-mosio-sequencing-only-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
 --m-metadata-column maxCTCAEcateg \
  --o-visualization m1-25k-mosio-sequencing-only-only-core-metrics-results/maxCTCAE-braycurtis-group-significance.qzv

MOSIO beta diversity M2
*Making a table for the samples for which we have both sequencing and PK data from Moataz
qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --p-where "[mosio_sequencing] = 1 " \
  --o-filtered-table m2-mosio-sequencing-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table! Set the sampling depth to 10000 based on the qzv description after generating the table
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table m2-mosio-sequencing-only-table.qza \
  --p-sampling-depth 25000 \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --output-dir m2-25k-mosio-sequencing-only-only-core-metrics-results

**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls m2-25k-mosio-sequencing-only-only-core-metrics-results/

**Getting the Braycurtiscvalue for all the sample, then creating a new mapping file so you can use that as a covariate
qiime diversity beta-group-significance \
  --i-distance-matrix m2-25k-mosio-sequencing-only-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
 --m-metadata-column everdiarrhea1categ \
  --o-visualization m2-25k-mosio-sequencing-only-only-core-metrics-results/everdiarrheacateg-braycurtis-group-significance.qzv

qiime diversity beta-group-significance \
  --i-distance-matrix m2-25k-mosio-sequencing-only-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
 --m-metadata-column everdiarrhea1timeframe \
  --o-visualization m2-25k-mosio-sequencing-only-only-core-metrics-results/everdiarrheatimeframe-braycurtis-group-significance.qzv

qiime diversity beta-group-significance \
  --i-distance-matrix m2-25k-mosio-sequencing-only-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
 --m-metadata-column everCTCAE21categ \
  --o-visualization m2-25k-mosio-sequencing-only-only-core-metrics-results/everCTCAE2-braycurtis-group-significance.qzv

qiime diversity beta-group-significance \
  --i-distance-matrix m2-25k-mosio-sequencing-only-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
 --m-metadata-column everCTCAE21timeframe \
  --o-visualization m2-25k-mosio-sequencing-only-only-core-metrics-results/everCTCAE2timeframe-braycurtis-group-significance.qzv

qiime diversity beta-group-significance \
  --i-distance-matrix m2-25k-mosio-sequencing-only-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
 --m-metadata-column maxCTCAEcateg \
  --o-visualization m2-25k-mosio-sequencing-only-only-core-metrics-results/maxCTCAE-braycurtis-group-significance.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
**You need to visualize the file then export the raw values as TSV
qiime tools view pk-sequencing-only-core-metrics-results/shannon-group-significance.qzv
qiime tools view mosio-sequencing-only-only-core-metrics-results/bray_curtis_emperor.qzv
qiime tools view mosio-sequencing-only-only-core-metrics-results/everCTCAE-bray-curtis-group-significance.qzv

qiime tools view m1-25k-mosio-sequencing-only-only-core-metrics-results/everdiarrheacateg-braycurtis-group-significance.qzv
qiime tools view m1-25k-mosio-sequencing-only-only-core-metrics-results/everdiarrheatimeframe-braycurtis-group-significance.qzv
qiime tools view m1-25k-mosio-sequencing-only-only-core-metrics-results/everCTCAE2-braycurtis-group-significance.qzv
qiime tools view m1-25k-mosio-sequencing-only-only-core-metrics-results/everCTCAE2timeframe-braycurtis-group-significance.qzv
qiime tools view m1-25k-mosio-sequencing-only-only-core-metrics-results/maxCTCAE-braycurtis-group-significance.qzv

qiime tools view m2-25k-mosio-sequencing-only-only-core-metrics-results/everdiarrheacateg-braycurtis-group-significance.qzv
qiime tools view m2-25k-mosio-sequencing-only-only-core-metrics-results/everdiarrheatimeframe-braycurtis-group-significance.qzv
qiime tools view m2-25k-mosio-sequencing-only-only-core-metrics-results/everCTCAE2-braycurtis-group-significance.qzv
qiime tools view m2-25k-mosio-sequencing-only-only-core-metrics-results/everCTCAE2timeframe-braycurtis-group-significance.qzv
qiime tools view m2-25k-mosio-sequencing-only-only-core-metrics-results/maxCTCAE-braycurtis-group-significance.qzv

Beta diversity for the bray Curtis variable for the cross sectional dataset
*16S MAR2023
qiime feature-table filter-samples \
  --i-table pk-sequencing-only-table.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --p-where "[Study_arm_2] = 'Cross-sectional' " \
  --o-filtered-table pk-sequencing-cs-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-sequencing-cs-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --output-dir pk-sequencing-cs-only-core-metrics-results

**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls pk-sequencing-cs-only-core-metrics-results/

qiime diversity beta-group-significance \
  --i-distance-matrix pk-sequencing-cs-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_AUC_0_12_dose_adj_cat \
  --o-visualization pk-sequencing-cs-only-core-metrics-results/bray-curtis-pk-sequencing-only-AUC-significance.qzv \
  --p-pairwise

qiime diversity beta-group-significance \
  --i-distance-matrix pk-sequencing-cs-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_EHR_5_12_cat \
  --o-visualization pk-sequencing-cs-only-core-metrics-results/bray-curtis-pk-sequencing-only-EHR-significance.qzv \
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-cs-only-core-metrics-results/bray-curtis-pk-sequencing-only-AUC-significance.qzv
qiime tools view pk-sequencing-cs-only-core-metrics-results/bray-curtis-pk-sequencing-only-EHR-significance.qzv
Beta diversity for the bray Curtis variable for the prospective dataset
*16S MAR2023
qiime feature-table filter-samples \
  --i-table pk-sequencing-only-table.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --p-where "[Study_arm_2] = 'Prospective' " \
  --o-filtered-table pk-sequencing-ps-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-sequencing-ps-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --output-dir pk-sequencing-ps-only-core-metrics-results

**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls pk-sequencing-ps-only-core-metrics-results/

qiime diversity beta-group-significance \
  --i-distance-matrix pk-sequencing-ps-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_AUC_0_12_dose_adj_cat \
  --o-visualization pk-sequencing-ps-only-core-metrics-results/bray-curtis-pk-sequencing-only-AUC-significance.qzv \
  --p-pairwise

qiime diversity beta-group-significance \
  --i-distance-matrix pk-sequencing-ps-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_EHR_5_12_cat \
  --o-visualization pk-sequencing-ps-only-core-metrics-results/bray-curtis-pk-sequencing-only-EHR-significance.qzv \
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-ps-only-core-metrics-results/bray-curtis-pk-sequencing-only-AUC-significance.qzv
qiime tools view pk-sequencing-ps-only-core-metrics-results/bray-curtis-pk-sequencing-only-EHR-significance.qzv

Emperor plot for beta distance matrices (must be a continuous variable)
Trying it out for AUC and EHR in the PK & Sequencing datasets
qiime emperor plot \
  --i-pcoa gut-only-core-metrics-results/bray_curtis_pcoa_results.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map_Shannon_Dada.tsv \
  --p-custom-axes Shannon_Dada_Oral \
  --o-visualization gut-only-core-metrics-results/bray-curtis-emperor-gut-shannon.qzv

*MAR2023 16S
qiime phylogeny align-to-tree-mafft-fasttree \
  --i-sequences hhri16s-rep-seqs.qza \
  --o-alignment aligned-hhri16s-rep-seqs.qza \
  --o-masked-alignment masked-aligned-hhri16s-rep-seqs.qza \
  --o-tree hhri16s-unrooted-tree.qza \
  --o-rooted-tree hhri16s-rooted-tree.qza
ls

*Making a table for the samples for which we have both sequencing and PK data from Moataz
*Found at “/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/18_HHRI_UMGC_Projects/04_12192022_Israni4_Project_019and020_16sSequencing/Documents/41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt” using the alt command after rightclicking to copy as a pathname.
qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --p-where "[AUC_sequencing] = 'Both' " \
  --o-filtered-table pk-sequencing-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-sequencing-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --output-dir pk-sequencing-only-core-metrics-results

qiime emperor plot \
  --i-pcoa pk-sequencing-only-core-metrics-results/bray_curtis_pcoa_results.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --p-custom-axes MPA_AUC_0_12_dose_adj \
  --o-visualization pk-sequencing-only-core-metrics-results/bray-curtis-emperor-pk-sequencing-only-AUC.qzv

qiime emperor plot \
  --i-pcoa pk-sequencing-only-core-metrics-results/bray_curtis_pcoa_results.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --p-custom-axes MPA_EHR_5_12 \
  --o-visualization pk-sequencing-only-core-metrics-results/bray-curtis-emperor-pk-sequencing-only-EHR.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-only-core-metrics-results/bray-curtis-emperor-pk-sequencing-only-AUC.qzv
qiime tools view pk-sequencing-only-core-metrics-results/bray-curtis-emperor-pk-sequencing-only-EHR.qzv
Emperor plot for beta distance matrices (must be a continuous variable)
Trying it out for AUC and EHR in the cross sectional datasets
qiime emperor plot \
  --i-pcoa gut-only-core-metrics-results/bray_curtis_pcoa_results.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map_Shannon_Dada.tsv \
  --p-custom-axes Shannon_Dada_Oral \
  --o-visualization gut-only-core-metrics-results/bray-curtis-emperor-gut-shannon.qzv
*16S MAR2023
qiime feature-table filter-samples \
  --i-table pk-sequencing-only-table.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --p-where "[Study_arm_2] = 'Cross-sectional' " \
  --o-filtered-table pk-sequencing-cs-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-sequencing-cs-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --output-dir pk-sequencing-cs-only-core-metrics-results

**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls pk-sequencing-cs-only-core-metrics-results/

qiime emperor plot \
  --i-pcoa pk-sequencing-cs-only-core-metrics-results/bray_curtis_pcoa_results.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --p-custom-axes MPA_AUC_0_12_dose_adj \
  --o-visualization pk-sequencing-cs-only-core-metrics-results/bray-curtis-emperor-pk-sequencing-only-AUC.qzv

qiime emperor plot \
  --i-pcoa pk-sequencing-cs-only-core-metrics-results/bray_curtis_pcoa_results.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --p-custom-axes MPA_EHR_5_12 \
  --o-visualization pk-sequencing-cs-only-core-metrics-results/bray-curtis-emperor-pk-sequencing-only-EHR.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-cs-only-core-metrics-results/bray-curtis-emperor-pk-sequencing-only-AUC.qzv
qiime tools view pk-sequencing-cs-only-core-metrics-results/bray-curtis-emperor-pk-sequencing-only-EHR.qzv
Emperor plot for beta distance matrices (must be a continuous variable)
Trying it out for AUC and EHR in the prospective datasets
qiime emperor plot \
  --i-pcoa gut-only-core-metrics-results/bray_curtis_pcoa_results.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map_Shannon_Dada.tsv \
  --p-custom-axes Shannon_Dada_Oral \
  --o-visualization gut-only-core-metrics-results/bray-curtis-emperor-gut-shannon.qzv

*16S MAR2023
qiime feature-table filter-samples \
  --i-table pk-sequencing-only-table.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --p-where "[Study_arm_2] = 'Prospective' " \
  --o-filtered-table pk-sequencing-ps-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-sequencing-ps-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --output-dir pk-sequencing-ps-only-core-metrics-results

**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls pk-sequencing-ps-only-core-metrics-results/
qiime emperor plot \
  --i-pcoa pk-sequencing-ps-only-core-metrics-results/bray_curtis_pcoa_results.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --p-custom-axes MPA_AUC_0_12_dose_adj \
  --o-visualization pk-sequencing-ps-only-core-metrics-results/bray-curtis-emperor-pk-sequencing-only-AUC.qzv

qiime emperor plot \
  --i-pcoa pk-sequencing-ps-only-core-metrics-results/bray_curtis_pcoa_results.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --p-custom-axes MPA_EHR_5_12 \
  --o-visualization pk-sequencing-ps-only-core-metrics-results/bray-curtis-emperor-pk-sequencing-only-EHR.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-ps-only-core-metrics-results/bray-curtis-emperor-pk-sequencing-only-AUC.qzv
qiime tools view pk-sequencing-ps-only-core-metrics-results/bray-curtis-emperor-pk-sequencing-only-EHR.qzv

**You do not need to use a continuous variable in order to vew the PCOA chart, you can just visualize the one that is automatically created when you generate the matrices
qiime tools view /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/pk-sequencing-M2M4M6-only-core-metrics-results/bray_curtis_emperor.qzv
Beta diversity: Performing longitudinal and paired sample comparisons with q2-longitudinal
https://docs.qiime2.org/2019.10/tutorials/longitudinal/

Longitudinal beta (pcoa) diversity for M2 vs M4 vs M6. The first step is to generate the alpha & beta diversity matrices needed for the longitudinal analyses.

qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 43_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4M6.txt \
  --p-where "[Study_arm_2] = 'Prospective' " \
  --o-filtered-table pk-sequencing-M2M4M6-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-sequencing-M2M4M6-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file 43_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4M6.txt \
  --output-dir pk-sequencing-M2M4M6-only-core-metrics-results
Pairwise distance comparisons

The pairwise-distances visualizer also assesses changes between paired samples from two different “states”, but instead of taking a metadata column or artifact as input, it operates on a distance matrix to assess the distance between “pre” and “post” sample pairs, and tests whether these paired differences are significantly different between different groups, as specified by the group-column parameter. 

qiime longitudinal pairwise-distances \
  --i-distance-matrix oral-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file Aspirin_Oral_Map.tsv \
  --p-group-column Contents \
  --p-state-column Collection \
  --p-state-1 1 \
  --p-state-2 2 \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-bray-pairwise-distances.qzv

*16SMAR2023
*Had to include the ‘p-group-column” categorical variable even though I am only interested in alpha diversity change over time itself. This changed the hypothesis from “is the change in alpha diversity itself between the timepoints different from 0?”
qiime longitudinal pairwise-differences \
  --m-metadata-file 43_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4M6.txt \
  --m-metadata-file pk-sequencing-M2M4M6-only-core-metrics-results/bray_curtis_pcoa_results.qza \
  --p-metric shannon \
  --p-state-column Time_point \
  --p-state-1 2 \
  --p-state-2 6 \
  --p-individual-id-column Patient_ID \
  --p-replicate-handling random \
  --o-visualization pk-sequencing-M2M4M6-pcoa-pairwise-differences.qzv

qiime longitudinal pairwise-distances \
  --i-distance-matrix pk-sequencing-M2M4M6-only-core-metrics-results/unweighted_unifrac_distance_matrix.qza \
  --m-metadata-file 43_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4M6.txt \
--p-group-column Unit \
  --p-state-column Time_point \
  --p-state-1 2 \
  --p-state-2 6 \
  --p-individual-id-column Patient_ID \
  --p-replicate-handling random \
  --o-visualization pk-sequencing-M2M4M6-unweightedunifracs-pairwise-distances.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
**You need to visualize the file then export the raw values as TSV
qiime tools view pk-sequencing-M2M4M6-unweightedunifracs-pairwise-distances.qzv
Note
**You do not need to use a continuous variable in order to vew the PCOA chart, you can just visualize the one that is automatically created when you generate the matrices
qiime tools view /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/pk-sequencing-M2M4M6-only-core-metrics-results/bray_curtis_emperor.qzv
Linear mixed effect models
Linear mixed effects (LME) models test the relationship between a single response variable and one or more independent variables, where observations are made across dependent samples, e.g., in repeated-measures sampling experiments. This implementation takes at least one numeric state-column (e.g., Time) and one or more comma-separated group-columns (which may be categorical or numeric metadata columns; these are the fixed effects) as independent variables in a LME model, and plots regression plots of the response variable (“metric”) as a function of the state column and each group column. Additionally, the individual-id-column parameter should be a metadata column that indicates the individual subject/site that was sampled repeatedly. The response variable may either be a sample metadata mapping file column or a feature ID in the feature table. A comma-separated list of random effects can also be input to this action; a random intercept for each individual is included by default, but another common random effect that users may wish to use is a random slope for each individual, which can be set by using the state-column value as input to the random-effects parameter. Here we use LME to test whether alpha diversity (Shannon diversity index) changed over time and in response to delivery mode, diet, and sex in the ECAM data set.
Note
Deciding whether a factor is a fixed effect or a random effect can be complicated. In general, a factor should be a fixed effect if the different factor levels (metadata column values) represent (more or less) all possible discrete values. For example, delivery mode, sex, and diet (dominantly breast-fed or formula-fed) are designated as fixed effects in the example below. Conversely, a factor should be a random effect if its values represent random samples from a population. For example, we could imagine having metadata variables like body-weight, daily-kcal-from-breastmilk, number-of-peanuts-eaten-per-day, or mg-of-penicillin-administered-daily; such values would represent random samples from within a population, and are unlikely to capture all possible values representative of the whole population. Not sure about the factors in your experiment? 🤔 Consult a statistician or reputable statistical tome for guidance. 📚
*I am running a Linear Mixed effect model for the Site variable as a predictor of change in the overall study
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Site \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization oral-gut-shannon-linear-mixed-effects.qzv
*For this model, I am using a specific mapping file where I copied the matched oral shanon diversity values (from DADA2 in R) to the gut samples, and I used sorting by Subject ID and by Collection to make sure I had the matching samples. (I actually exported the values from the oral-gut-core-metrics file to a text file, and used it in R to do the same analysis)
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Map_Shannon_Dada.tsv \
  --m-metadata-file gut-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Shannon_Dada_Oral \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization gut-only-oral-shannon-linear-mixed-effects.qzv
*16SMAR2023 
*Trying a longitudinal model without the categorical variable “  --p-group-columns Site \” as a fixed effect. Alternatively, I can try using time as a fixed effect.
qiime longitudinal linear-mixed-effects \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M1M2only.txt \
   --m-metadata-file pk-sequencing-M1M2-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M1M2-shannon-linear-mixed-effects.qzv

*I also need to check the file names for beta diversity in order to pull the right metric for the LME model (https://forum.qiime2.org/t/p-metric-for-beta-diversity/4415/4)
qiime metadata tabulate --help

qiime metadata tabulate \
--m-input-file pk-sequencing-M1M2-only-core-metrics-results/Shannon_vector.qza \
  --o-visualization pk-sequencing-M2M4M6-metadada-tabulate-shannon-pairwise-differences.qzv

qiime metadata tabulate \
--m-input-file pk-sequencing-M2M4M6-only-core-metrics-results/bray_curtis_pcoa_results.qza\
  --o-visualization pk-sequencing-M2M4M6-metadada-tabulate-braycurtis-pcoa-pairwise-differences.qzv

qiime feature-data summarize --help
*Even when checking the post (https://forum.qiime2.org/t/alpha-and-beta-diversity-explanations-and-commands/2282), for other --p-metric- options, it seems that the main issue is that the file cannot be viewed as metadata directly as it is a distance matrix file
In order to check the format use the following commands

*** In order to tell which file type you have, you can use the qiime tools command
*This shows that the alpha diversity qza file inherently has a different structure from the beta diversity qza file.
qiime tools --help
qiime tools peek table.qza
qiime tools peek pk-sequencing-M1M2-only-core-metrics-results/Shannon_vector.qza
qiime tools peek pk-sequencing-M2M4M6-only-core-metrics-results/unweighted_unifrac_distance_matrix.qza

*Therefore, the solution seems to be using qiime metadata-tabulate to pull the beta diversity axes as qzv files, then merge them to the existing mapping file by sample ID, and finally being able to rerun the qiime longitudinal from a single metadatafile with the right variables

qiime metadata tabulate \
--m-input-file pk-sequencing-M2M4M6-only-core-metrics-results/Shannon_vector.qza \
  --o-visualization pk-sequencing-M2M4M6-metadada-tabulate-shannon-pairwise-differences.qzv

qiime metadata tabulate \
--m-input-file pk-sequencing-M2M4M6-only-core-metrics-results/bray_curtis_pcoa_results.qza\
  --o-visualization pk-sequencing-M2M4M6-metadada-tabulate-braycurtis-pcoa-pairwise-differences.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
**You need to visualize the file then export the raw values as TSV
qiime tools view pk-sequencing-M2M4M6-metadada-tabulate-shannon-pairwise-differences.qzv
qiime tools view pk-sequencing-M2M4M6-metadada-tabulate-braycurtis-pcoa-pairwise-differences.qzv
Note
Now I can move to the actual LME 
qiime longitudinal linear-mixed-effects \
  --m-metadata-file 43_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4M6_V2.txt \
  --p-metric bc_pcoa_Axis1 \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M2M4M6-bc-pcoa-linear-mixed-effects.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M2M4M6-bc-pcoa-linear-mixed-effects.qzv
The visualizer produced by this command contains several results. First, the input parameters are shown at the top of the visualization for convenience (e.g., when flipping through multiple visualizations it is useful to have a summary). Next, the “model summary” shows some descriptive information about the LME model that was trained. This just shows descriptive information about the “groups”; in this case, groups will be individuals (as set by the --p-individual-id-column). The main results to examine will be the “model results” below the “model summary”. These results summarize the effects of each fixed effect (and their interactions) on the dependent variable (shannon diversity). This table shows parameter estimates, estimate standard errors, z scores, P values (P>|z|), and 95% confidence interval upper and lower bounds for each parameter. We see in this table that shannon diversity is significantly impacted by month of life and by diet, as well as several interacting factors. More information about LME models and the interpretation of these data can be found on the statsmodels LME description page, which provides a number of useful technical references for further reading.
Finally, scatter plots categorized by each “group column” are shown at the bottom of the visualization, with linear regression lines (plus 95% confidence interval in grey) for each group. If --p-lowess is enabled, instead locally weighted averages are shown for each group. Two different groups of scatter plots are shown. First, regression scatterplots show the relationship between state_column (x-axis) and metric (y-axis) for each sample. These plots are just used as a quick summary for reference; users are recommended to use the volatility visualizer for interactive plotting of their longitudinal data. Volatility plots can be used to qualitatively identify outliers that disproportionately drive the variance within individuals and groups, including by inspecting residuals in relation to control limits (see note below and the section on “Volatility analysis” for more details).
The second set of scatterplots are fit vs. residual plots, which show the relationship between metric predictions for each sample (on the x-axis), and the residual or observation error (prediction - actual value) for each sample (on the y-axis). Residuals should be roughly zero-centered and normal across the range of measured values. Uncentered, systematically high or low, and autocorrelated values could indicate a poor model. If your residual plots look like an ugly mess without any apparent relationship between values, you are doing a good job. If you see a U-shaped curve or other non-random distribution, either your predictor variables (group_columns and/or random_effects) are failing to capture all explanatory information, causing information to leak into your residuals, or else you are not using an appropriate model for your data 🙁. Check your predictor variables and available metadata columns to make sure you aren’t missing anything.
Note
If you want to dot your i’s and cross your t’s, residual and predicted values for each sample can be obtained in the “Download raw data as tsv” link below the regression scatterplots. This file can be input as metadata to the volatility visualizer to check whether residuals are correlated with other metadata columns. If they are, those columns should probably be used as prediction variables in your model! Control limits (± 2 and 3 standard deviations) can be toggled on/off to easily identify outliers, which can be particularly useful for re-examining fit vs. residual plots with this visualizer. 🍝

Longitudinal Beta diversity for M2 vs M4. The first step is to generate the alpha & beta diversity matrices needed for the longitudinal analyses.

qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4only.txt \
  --p-where "[Study_arm_2] = 'Prospective' " \
  --o-filtered-table pk-sequencing-M2M4-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-sequencing-M2M4-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4only.txt \
  --output-dir pk-sequencing-M2M4-only-core-metrics-results
Pairwise distance comparisons

The pairwise-distances visualizer also assesses changes between paired samples from two different “states”, but instead of taking a metadata column or artifact as input, it operates on a distance matrix to assess the distance between “pre” and “post” sample pairs, and tests whether these paired differences are significantly different between different groups, as specified by the group-column parameter. 

qiime longitudinal pairwise-distances \
  --i-distance-matrix oral-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file Aspirin_Oral_Map.tsv \
  --p-group-column Contents \
  --p-state-column Collection \
  --p-state-1 1 \
  --p-state-2 2 \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-bray-pairwise-distances.qzv

*16SMAR2023
*Had to include the ‘p-group-column” categorical variable even though I am only interested in alpha diversity change over time itself. This changed the hypothesis from “is the change in alpha diversity itself between the timepoints different from 0?”
qiime longitudinal pairwise-differences \
  --m-metadata-file 43_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4M6.txt \
  --m-metadata-file pk-sequencing-M2M4M6-only-core-metrics-results/bray_curtis_pcoa_results.qza \
  --p-metric shannon \
  --p-state-column Time_point \
  --p-state-1 2 \
  --p-state-2 6 \
  --p-individual-id-column Patient_ID \
  --p-replicate-handling random \
  --o-visualization pk-sequencing-M2M4M6-pcoa-pairwise-differences.qzv

qiime longitudinal pairwise-distances \
  --i-distance-matrix pk-sequencing-M2M4-only-core-metrics-results/unweighted_unifrac_distance_matrix.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4only.txt \
--p-group-column Unit \
  --p-state-column Time_point \
  --p-state-1 2 \
  --p-state-2 4 \
  --p-individual-id-column Patient_ID \
  --p-replicate-handling random \
  --o-visualization pk-sequencing-M2M4-only-unweightedunifracs-pairwise-distances.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
**You need to visualize the file then export the raw values as TSV
qiime tools view pk-sequencing-M2M4-only-unweightedunifracs-pairwise-distances.qzv
Note
**You do not need to use a continuous variable in order to vew the PCOA chart, you can just visualize the one that is automatically created when you generate the matrices
qiime tools view /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/pk-sequencing-M2M4-only-core-metrics-results/bray_curtis_emperor.qzv
Linear mixed effect models
Linear mixed effects (LME) models test the relationship between a single response variable and one or more independent variables, where observations are made across dependent samples, e.g., in repeated-measures sampling experiments. This implementation takes at least one numeric state-column (e.g., Time) and one or more comma-separated group-columns (which may be categorical or numeric metadata columns; these are the fixed effects) as independent variables in a LME model, and plots regression plots of the response variable (“metric”) as a function of the state column and each group column. Additionally, the individual-id-column parameter should be a metadata column that indicates the individual subject/site that was sampled repeatedly. The response variable may either be a sample metadata mapping file column or a feature ID in the feature table. A comma-separated list of random effects can also be input to this action; a random intercept for each individual is included by default, but another common random effect that users may wish to use is a random slope for each individual, which can be set by using the state-column value as input to the random-effects parameter. Here we use LME to test whether alpha diversity (Shannon diversity index) changed over time and in response to delivery mode, diet, and sex in the ECAM data set.
Note
Deciding whether a factor is a fixed effect or a random effect can be complicated. In general, a factor should be a fixed effect if the different factor levels (metadata column values) represent (more or less) all possible discrete values. For example, delivery mode, sex, and diet (dominantly breast-fed or formula-fed) are designated as fixed effects in the example below. Conversely, a factor should be a random effect if its values represent random samples from a population. For example, we could imagine having metadata variables like body-weight, daily-kcal-from-breastmilk, number-of-peanuts-eaten-per-day, or mg-of-penicillin-administered-daily; such values would represent random samples from within a population, and are unlikely to capture all possible values representative of the whole population. Not sure about the factors in your experiment? 🤔 Consult a statistician or reputable statistical tome for guidance. 📚
*I am running a Linear Mixed effect model for the Site variable as a predictor of change in the overall study
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Site \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization oral-gut-shannon-linear-mixed-effects.qzv
*For this model, I am using a specific mapping file where I copied the matched oral shanon diversity values (from DADA2 in R) to the gut samples, and I used sorting by Subject ID and by Collection to make sure I had the matching samples. (I actually exported the values from the oral-gut-core-metrics file to a text file, and used it in R to do the same analysis)
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Map_Shannon_Dada.tsv \
  --m-metadata-file gut-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Shannon_Dada_Oral \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization gut-only-oral-shannon-linear-mixed-effects.qzv
*16SMAR2023 
*Trying a longitudinal model without the categorical variable “  --p-group-columns Site \” as a fixed effect. Alternatively, I can try using time as a fixed effect.
qiime longitudinal linear-mixed-effects \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4only.txt \
   --m-metadata-file pk-sequencing-M1M2-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M1M2-shannon-linear-mixed-effects.qzv

*I also need to check the file names for beta diversity in order to pull the right metric for the LME model (https://forum.qiime2.org/t/p-metric-for-beta-diversity/4415/4)
qiime metadata tabulate --help

qiime metadata tabulate \
--m-input-file pk-sequencing-M2M4-only-core-metrics-results/Shannon_vector.qza \
  --o-visualization pk-sequencing-M2M4-metadada-tabulate-shannon-pairwise-differences.qzv

qiime metadata tabulate \
--m-input-file pk-sequencing-M2M4-only-core-metrics-results/bray_curtis_pcoa_results.qza\
  --o-visualization pk-sequencing-M2M4-metadada-tabulate-braycurtis-pcoa-pairwise-differences.qzv

qiime feature-data summarize --help
*Even when checking the post (https://forum.qiime2.org/t/alpha-and-beta-diversity-explanations-and-commands/2282), for other --p-metric- options, it seems that the main issue is that the file cannot be viewed as metadata directly as it is a distance matrix file
In order to check the format use the following commands

*** In order to tell which file type you have, you can use the qiime tools command
*This shows that the alpha diversity qza file inherently has a different structure from the beta diversity qza file.
qiime tools --help
qiime tools peek table.qza
qiime tools peek pk-sequencing-M2M4-only-core-metrics-results/Shannon_vector.qza
qiime tools peek pk-sequencing-M2M4-only-core-metrics-results/unweighted_unifrac_distance_matrix.qza

*Therefore, the solution seems to be using qiime metadata-tabulate to pull the beta diversity axes as qzv files, then merge them to the existing mapping file by sample ID, and finally being able to rerun the qiime longitudinal from a single metadatafile with the right variables

qiime metadata tabulate \
--m-input-file pk-sequencing-M2M4-only-core-metrics-results/Shannon_vector.qza \
  --o-visualization pk-sequencing-M2M4-metadada-tabulate-shannon-pairwise-differences.qzv

qiime metadata tabulate \
--m-input-file pk-sequencing-M2M4-only-core-metrics-results/bray_curtis_pcoa_results.qza\
  --o-visualization pk-sequencing-M2M4-metadada-tabulate-braycurtis-pcoa-pairwise-differences.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
**You need to visualize the file then export the raw values as TSV
qiime tools view pk-sequencing-M2M4-metadada-tabulate-shannon-pairwise-differences.qzv
qiime tools view pk-sequencing-M2M4-metadada-tabulate-braycurtis-pcoa-pairwise-differences.qzv
Note
Now I can move to the actual LME 
qiime longitudinal linear-mixed-effects \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4only_V2.txt \
  --p-metric bc_pcoa_Axis1 \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M2M4-bc-pcoa-linear-mixed-effects.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M2M4-bc-pcoa-linear-mixed-effects.qzv
The visualizer produced by this command contains several results. First, the input parameters are shown at the top of the visualization for convenience (e.g., when flipping through multiple visualizations it is useful to have a summary). Next, the “model summary” shows some descriptive information about the LME model that was trained. This just shows descriptive information about the “groups”; in this case, groups will be individuals (as set by the --p-individual-id-column). The main results to examine will be the “model results” below the “model summary”. These results summarize the effects of each fixed effect (and their interactions) on the dependent variable (shannon diversity). This table shows parameter estimates, estimate standard errors, z scores, P values (P>|z|), and 95% confidence interval upper and lower bounds for each parameter. We see in this table that shannon diversity is significantly impacted by month of life and by diet, as well as several interacting factors. More information about LME models and the interpretation of these data can be found on the statsmodels LME description page, which provides a number of useful technical references for further reading.
Finally, scatter plots categorized by each “group column” are shown at the bottom of the visualization, with linear regression lines (plus 95% confidence interval in grey) for each group. If --p-lowess is enabled, instead locally weighted averages are shown for each group. Two different groups of scatter plots are shown. First, regression scatterplots show the relationship between state_column (x-axis) and metric (y-axis) for each sample. These plots are just used as a quick summary for reference; users are recommended to use the volatility visualizer for interactive plotting of their longitudinal data. Volatility plots can be used to qualitatively identify outliers that disproportionately drive the variance within individuals and groups, including by inspecting residuals in relation to control limits (see note below and the section on “Volatility analysis” for more details).
The second set of scatterplots are fit vs. residual plots, which show the relationship between metric predictions for each sample (on the x-axis), and the residual or observation error (prediction - actual value) for each sample (on the y-axis). Residuals should be roughly zero-centered and normal across the range of measured values. Uncentered, systematically high or low, and autocorrelated values could indicate a poor model. If your residual plots look like an ugly mess without any apparent relationship between values, you are doing a good job. If you see a U-shaped curve or other non-random distribution, either your predictor variables (group_columns and/or random_effects) are failing to capture all explanatory information, causing information to leak into your residuals, or else you are not using an appropriate model for your data 🙁. Check your predictor variables and available metadata columns to make sure you aren’t missing anything.
Note
If you want to dot your i’s and cross your t’s, residual and predicted values for each sample can be obtained in the “Download raw data as tsv” link below the regression scatterplots. This file can be input as metadata to the volatility visualizer to check whether residuals are correlated with other metadata columns. If they are, those columns should probably be used as prediction variables in your model! Control limits (± 2 and 3 standard deviations) can be toggled on/off to easily identify outliers, which can be particularly useful for re-examining fit vs. residual plots with this visualizer. 🍝

Longitudinal Alpha diversity for M2 vs M6. The first step is to generate the alpha & beta diversity matrices needed for the longitudinal analyses.

qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M6only.txt \
  --p-where "[Study_arm_2] = 'Prospective' " \
  --o-filtered-table pk-sequencing-M2M6-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-sequencing-M2M6-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M6only.txt \
  --output-dir pk-sequencing-M2M6-only-core-metrics-results
Pairwise distance comparisons

The pairwise-distances visualizer also assesses changes between paired samples from two different “states”, but instead of taking a metadata column or artifact as input, it operates on a distance matrix to assess the distance between “pre” and “post” sample pairs, and tests whether these paired differences are significantly different between different groups, as specified by the group-column parameter. 

qiime longitudinal pairwise-distances \
  --i-distance-matrix oral-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file Aspirin_Oral_Map.tsv \
  --p-group-column Contents \
  --p-state-column Collection \
  --p-state-1 1 \
  --p-state-2 2 \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-bray-pairwise-distances.qzv

*16SMAR2023
*Had to include the ‘p-group-column” categorical variable even though I am only interested in alpha diversity change over time itself. This changed the hypothesis from “is the change in alpha diversity itself between the timepoints different from 0?”
qiime longitudinal pairwise-differences \
  --m-metadata-file 43_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4M6.txt \
  --m-metadata-file pk-sequencing-M2M4M6-only-core-metrics-results/bray_curtis_pcoa_results.qza \
  --p-metric shannon \
  --p-state-column Time_point \
  --p-state-1 2 \
  --p-state-2 6 \
  --p-individual-id-column Patient_ID \
  --p-replicate-handling random \
  --o-visualization pk-sequencing-M2M4M6-pcoa-pairwise-differences.qzv

qiime longitudinal pairwise-distances \
  --i-distance-matrix pk-sequencing-M2M6-only-core-metrics-results/unweighted_unifrac_distance_matrix.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M6only.txt \
--p-group-column Unit \
  --p-state-column Time_point \
  --p-state-1 2 \
  --p-state-2 6 \
  --p-individual-id-column Patient_ID \
  --p-replicate-handling random \
  --o-visualization pk-sequencing-M2M6-only-unweightedunifracs-pairwise-distances.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
**You need to visualize the file then export the raw values as TSV
qiime tools view pk-sequencing-M2M6-only-unweightedunifracs-pairwise-distances.qzv
Note
**You do not need to use a continuous variable in order to vew the PCOA chart, you can just visualize the one that is automatically created when you generate the matrices
qiime tools view /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/pk-sequencing-M2M6-only-core-metrics-results/bray_curtis_emperor.qzv
Linear mixed effect models
Linear mixed effects (LME) models test the relationship between a single response variable and one or more independent variables, where observations are made across dependent samples, e.g., in repeated-measures sampling experiments. This implementation takes at least one numeric state-column (e.g., Time) and one or more comma-separated group-columns (which may be categorical or numeric metadata columns; these are the fixed effects) as independent variables in a LME model, and plots regression plots of the response variable (“metric”) as a function of the state column and each group column. Additionally, the individual-id-column parameter should be a metadata column that indicates the individual subject/site that was sampled repeatedly. The response variable may either be a sample metadata mapping file column or a feature ID in the feature table. A comma-separated list of random effects can also be input to this action; a random intercept for each individual is included by default, but another common random effect that users may wish to use is a random slope for each individual, which can be set by using the state-column value as input to the random-effects parameter. Here we use LME to test whether alpha diversity (Shannon diversity index) changed over time and in response to delivery mode, diet, and sex in the ECAM data set.
Note
Deciding whether a factor is a fixed effect or a random effect can be complicated. In general, a factor should be a fixed effect if the different factor levels (metadata column values) represent (more or less) all possible discrete values. For example, delivery mode, sex, and diet (dominantly breast-fed or formula-fed) are designated as fixed effects in the example below. Conversely, a factor should be a random effect if its values represent random samples from a population. For example, we could imagine having metadata variables like body-weight, daily-kcal-from-breastmilk, number-of-peanuts-eaten-per-day, or mg-of-penicillin-administered-daily; such values would represent random samples from within a population, and are unlikely to capture all possible values representative of the whole population. Not sure about the factors in your experiment? 🤔 Consult a statistician or reputable statistical tome for guidance. 📚
*I am running a Linear Mixed effect model for the Site variable as a predictor of change in the overall study
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Site \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization oral-gut-shannon-linear-mixed-effects.qzv
*For this model, I am using a specific mapping file where I copied the matched oral shanon diversity values (from DADA2 in R) to the gut samples, and I used sorting by Subject ID and by Collection to make sure I had the matching samples. (I actually exported the values from the oral-gut-core-metrics file to a text file, and used it in R to do the same analysis)
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Map_Shannon_Dada.tsv \
  --m-metadata-file gut-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Shannon_Dada_Oral \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization gut-only-oral-shannon-linear-mixed-effects.qzv
*16SMAR2023 
*Trying a longitudinal model without the categorical variable “  --p-group-columns Site \” as a fixed effect. Alternatively, I can try using time as a fixed effect.
qiime longitudinal linear-mixed-effects \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M6only.txt \
   --m-metadata-file pk-sequencing-M2M6-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M2M6-shannon-linear-mixed-effects.qzv

*I also need to check the file names for beta diversity in order to pull the right metric for the LME model (https://forum.qiime2.org/t/p-metric-for-beta-diversity/4415/4)
qiime metadata tabulate --help

qiime metadata tabulate \
--m-input-file pk-sequencing-M2M6-only-core-metrics-results/Shannon_vector.qza \
  --o-visualization pk-sequencing-M2M6-metadada-tabulate-shannon-pairwise-differences.qzv

qiime metadata tabulate \
--m-input-file pk-sequencing-M2M6-only-core-metrics-results/bray_curtis_pcoa_results.qza\
  --o-visualization pk-sequencing-M2M6-metadada-tabulate-braycurtis-pcoa-pairwise-differences.qzv

qiime feature-data summarize --help
*Even when checking the post (https://forum.qiime2.org/t/alpha-and-beta-diversity-explanations-and-commands/2282), for other --p-metric- options, it seems that the main issue is that the file cannot be viewed as metadata directly as it is a distance matrix file
In order to check the format use the following commands

*** In order to tell which file type you have, you can use the qiime tools command
*This shows that the alpha diversity qza file inherently has a different structure from the beta diversity qza file.
qiime tools --help
qiime tools peek table.qza
qiime tools peek pk-sequencing-M2M6-only-core-metrics-results/Shannon_vector.qza
qiime tools peek pk-sequencing-M2M6-only-core-metrics-results/unweighted_unifrac_distance_matrix.qza

*Therefore, the solution seems to be using qiime metadata-tabulate to pull the beta diversity axes as qzv files, then merge them to the existing mapping file by sample ID, and finally being able to rerun the qiime longitudinal from a single metadatafile with the right variables

qiime metadata tabulate \
--m-input-file pk-sequencing-M2M6-only-core-metrics-results/Shannon_vector.qza \
  --o-visualization pk-sequencing-M2M6-metadada-tabulate-shannon-pairwise-differences.qzv

qiime metadata tabulate \
--m-input-file pk-sequencing-M2M6-only-core-metrics-results/bray_curtis_pcoa_results.qza\
  --o-visualization pk-sequencing-M2M6-metadada-tabulate-braycurtis-pcoa-pairwise-differences.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
**You need to visualize the file then export the raw values as TSV
qiime tools view pk-sequencing-M2M6-metadada-tabulate-shannon-pairwise-differences.qzv
qiime tools view pk-sequencing-M2M6-metadada-tabulate-braycurtis-pcoa-pairwise-differences.qzv
Note
Now I can move to the actual LME 
qiime longitudinal linear-mixed-effects \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M6only_V2.txt \
  --p-metric bc_pcoa_Axis1 \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M2M6-bc-pcoa-linear-mixed-effects.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M2M6-bc-pcoa-linear-mixed-effects.qzv
The visualizer produced by this command contains several results. First, the input parameters are shown at the top of the visualization for convenience (e.g., when flipping through multiple visualizations it is useful to have a summary). Next, the “model summary” shows some descriptive information about the LME model that was trained. This just shows descriptive information about the “groups”; in this case, groups will be individuals (as set by the --p-individual-id-column). The main results to examine will be the “model results” below the “model summary”. These results summarize the effects of each fixed effect (and their interactions) on the dependent variable (shannon diversity). This table shows parameter estimates, estimate standard errors, z scores, P values (P>|z|), and 95% confidence interval upper and lower bounds for each parameter. We see in this table that shannon diversity is significantly impacted by month of life and by diet, as well as several interacting factors. More information about LME models and the interpretation of these data can be found on the statsmodels LME description page, which provides a number of useful technical references for further reading.
Finally, scatter plots categorized by each “group column” are shown at the bottom of the visualization, with linear regression lines (plus 95% confidence interval in grey) for each group. If --p-lowess is enabled, instead locally weighted averages are shown for each group. Two different groups of scatter plots are shown. First, regression scatterplots show the relationship between state_column (x-axis) and metric (y-axis) for each sample. These plots are just used as a quick summary for reference; users are recommended to use the volatility visualizer for interactive plotting of their longitudinal data. Volatility plots can be used to qualitatively identify outliers that disproportionately drive the variance within individuals and groups, including by inspecting residuals in relation to control limits (see note below and the section on “Volatility analysis” for more details).
The second set of scatterplots are fit vs. residual plots, which show the relationship between metric predictions for each sample (on the x-axis), and the residual or observation error (prediction - actual value) for each sample (on the y-axis). Residuals should be roughly zero-centered and normal across the range of measured values. Uncentered, systematically high or low, and autocorrelated values could indicate a poor model. If your residual plots look like an ugly mess without any apparent relationship between values, you are doing a good job. If you see a U-shaped curve or other non-random distribution, either your predictor variables (group_columns and/or random_effects) are failing to capture all explanatory information, causing information to leak into your residuals, or else you are not using an appropriate model for your data 🙁. Check your predictor variables and available metadata columns to make sure you aren’t missing anything.
Note
If you want to dot your i’s and cross your t’s, residual and predicted values for each sample can be obtained in the “Download raw data as tsv” link below the regression scatterplots. This file can be input as metadata to the volatility visualizer to check whether residuals are correlated with other metadata columns. If they are, those columns should probably be used as prediction variables in your model! Control limits (± 2 and 3 standard deviations) can be toggled on/off to easily identify outliers, which can be particularly useful for re-examining fit vs. residual plots with this visualizer. 🍝
Longitudinal Beta diversity for M1 vs M2. The first step is to generate the alpha & beta diversity matrices needed for the longitudinal analyses.

qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M1M2only.txt \
  --p-where "[Study_arm_2] = 'Prospective' " \
  --o-filtered-table pk-sequencing-M1M2-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-sequencing-M1M2-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M1M2only.txt \
  --output-dir pk-sequencing-M1M2-only-core-metrics-results
Pairwise distance comparisons

The pairwise-distances visualizer also assesses changes between paired samples from two different “states”, but instead of taking a metadata column or artifact as input, it operates on a distance matrix to assess the distance between “pre” and “post” sample pairs, and tests whether these paired differences are significantly different between different groups, as specified by the group-column parameter. 

qiime longitudinal pairwise-distances \
  --i-distance-matrix oral-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file Aspirin_Oral_Map.tsv \
  --p-group-column Contents \
  --p-state-column Collection \
  --p-state-1 1 \
  --p-state-2 2 \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-bray-pairwise-distances.qzv

*16SMAR2023
*Had to include the ‘p-group-column” categorical variable even though I am only interested in alpha diversity change over time itself. This changed the hypothesis from “is the change in alpha diversity itself between the timepoints different from 0?”
qiime longitudinal pairwise-differences \
  --m-metadata-file 43_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4M6.txt \
  --m-metadata-file pk-sequencing-M2M4M6-only-core-metrics-results/bray_curtis_pcoa_results.qza \
  --p-metric shannon \
  --p-state-column Time_point \
  --p-state-1 2 \
  --p-state-2 6 \
  --p-individual-id-column Patient_ID \
  --p-replicate-handling random \
  --o-visualization pk-sequencing-M2M4M6-pcoa-pairwise-differences.qzv

qiime longitudinal pairwise-distances \
  --i-distance-matrix pk-sequencing-M1M2-only-core-metrics-results/unweighted_unifrac_distance_matrix.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M1M2only.txt \
--p-group-column Unit \
  --p-state-column Time_point \
  --p-state-1 1 \
  --p-state-2 2 \
  --p-individual-id-column Patient_ID \
  --p-replicate-handling random \
  --o-visualization pk-sequencing-M1M2-only-unweightedunifracs-pairwise-distances.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
**You need to visualize the file then export the raw values as TSV
qiime tools view pk-sequencing-M1M2-only-unweightedunifracs-pairwise-distances.qzv
Note
I will try to change the coding to ensure that the variables test for time effect
qiime longitudinal pairwise-distances --help
*Based on the help file, you can only look at paired difference, not at the time effect
qiime longitudinal pairwise-distances \
  --i-distance-matrix pk-sequencing-M1M2-only-core-metrics-results/unweighted_unifrac_distance_matrix.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M1M2only.txt \
--p-group-column Time_point \
  --p-individual-id-column Patient_ID \
  --p-replicate-handling random \
  --o-visualization test-pk-sequencing-M1M2-only-unweightedunifracs-pairwise-distances.qzv
qiime tools view test-pk-sequencing-M1M2-only-unweightedunifracs-pairwise-distances.qzv

Note
**You do not need to use a continuous variable in order to vew the PCOA chart, you can just visualize the one that is automatically created when you generate the matrices
qiime tools view /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/pk-sequencing-M1M2-only-core-metrics-results/bray_curtis_emperor.qzv
Linear mixed effect models
Linear mixed effects (LME) models test the relationship between a single response variable and one or more independent variables, where observations are made across dependent samples, e.g., in repeated-measures sampling experiments. This implementation takes at least one numeric state-column (e.g., Time) and one or more comma-separated group-columns (which may be categorical or numeric metadata columns; these are the fixed effects) as independent variables in a LME model, and plots regression plots of the response variable (“metric”) as a function of the state column and each group column. Additionally, the individual-id-column parameter should be a metadata column that indicates the individual subject/site that was sampled repeatedly. The response variable may either be a sample metadata mapping file column or a feature ID in the feature table. A comma-separated list of random effects can also be input to this action; a random intercept for each individual is included by default, but another common random effect that users may wish to use is a random slope for each individual, which can be set by using the state-column value as input to the random-effects parameter. Here we use LME to test whether alpha diversity (Shannon diversity index) changed over time and in response to delivery mode, diet, and sex in the ECAM data set.
Note
Deciding whether a factor is a fixed effect or a random effect can be complicated. In general, a factor should be a fixed effect if the different factor levels (metadata column values) represent (more or less) all possible discrete values. For example, delivery mode, sex, and diet (dominantly breast-fed or formula-fed) are designated as fixed effects in the example below. Conversely, a factor should be a random effect if its values represent random samples from a population. For example, we could imagine having metadata variables like body-weight, daily-kcal-from-breastmilk, number-of-peanuts-eaten-per-day, or mg-of-penicillin-administered-daily; such values would represent random samples from within a population, and are unlikely to capture all possible values representative of the whole population. Not sure about the factors in your experiment? 🤔 Consult a statistician or reputable statistical tome for guidance. 📚
*I am running a Linear Mixed effect model for the Site variable as a predictor of change in the overall study
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Site \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization oral-gut-shannon-linear-mixed-effects.qzv
*For this model, I am using a specific mapping file where I copied the matched oral shanon diversity values (from DADA2 in R) to the gut samples, and I used sorting by Subject ID and by Collection to make sure I had the matching samples. (I actually exported the values from the oral-gut-core-metrics file to a text file, and used it in R to do the same analysis)
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Map_Shannon_Dada.tsv \
  --m-metadata-file gut-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Shannon_Dada_Oral \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization gut-only-oral-shannon-linear-mixed-effects.qzv
*16SMAR2023 
*Trying a longitudinal model without the categorical variable “  --p-group-columns Site \” as a fixed effect. Alternatively, I can try using time as a fixed effect.
qiime longitudinal linear-mixed-effects \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M1M2only.txt \
   --m-metadata-file pk-sequencing-M1M2-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M1M2-shannon-linear-mixed-effects.qzv

*I also need to check the file names for beta diversity in order to pull the right metric for the LME model (https://forum.qiime2.org/t/p-metric-for-beta-diversity/4415/4)
qiime metadata tabulate --help

qiime metadata tabulate \
--m-input-file pk-sequencing-M1M2-only-core-metrics-results/Shannon_vector.qza \
  --o-visualization pk-sequencing-M1M2-metadada-tabulate-shannon-pairwise-differences.qzv

qiime metadata tabulate \
--m-input-file pk-sequencing-M1M2-only-core-metrics-results/bray_curtis_pcoa_results.qza\
  --o-visualization pk-sequencing-M1M2-metadada-tabulate-braycurtis-pcoa-pairwise-differences.qzv

qiime feature-data summarize --help
*Even when checking the post (https://forum.qiime2.org/t/alpha-and-beta-diversity-explanations-and-commands/2282), for other --p-metric- options, it seems that the main issue is that the file cannot be viewed as metadata directly as it is a distance matrix file
In order to check the format use the following commands

*** In order to tell which file type you have, you can use the qiime tools command
*This shows that the alpha diversity qza file inherently has a different structure from the beta diversity qza file.
qiime tools --help
qiime tools peek table.qza
qiime tools peek pk-sequencing-M1M2-only-core-metrics-results/Shannon_vector.qza
qiime tools peek pk-sequencing-M1M2-only-core-metrics-results/unweighted_unifrac_distance_matrix.qza

*Therefore, the solution seems to be using qiime metadata-tabulate to pull the beta diversity axes as qzv files, then merge them to the existing mapping file by sample ID, and finally being able to rerun the qiime longitudinal from a single metadatafile with the right variables

qiime metadata tabulate \
--m-input-file pk-sequencing-M1M2-only-core-metrics-results/Shannon_vector.qza \
  --o-visualization pk-sequencing-M1M2-metadada-tabulate-shannon-pairwise-differences.qzv

qiime metadata tabulate \
--m-input-file pk-sequencing-M1M2-only-core-metrics-results/bray_curtis_pcoa_results.qza\
  --o-visualization pk-sequencing-M1M2-metadada-tabulate-braycurtis-pcoa-pairwise-differences.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
**You need to visualize the file then export the raw values as TSV
qiime tools view pk-sequencing-M1M2-metadada-tabulate-shannon-pairwise-differences.qzv
qiime tools view pk-sequencing-M1M2-metadada-tabulate-braycurtis-pcoa-pairwise-differences.qzv
Note
Now I can move to the actual LME 
qiime longitudinal linear-mixed-effects \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M1M2only_V2.txt \
  --p-metric bc_pcoa_Axis1 \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M1M2-bc-pcoa-linear-mixed-effects.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M1M2-bc-pcoa-linear-mixed-effects.qzv
The visualizer produced by this command contains several results. First, the input parameters are shown at the top of the visualization for convenience (e.g., when flipping through multiple visualizations it is useful to have a summary). Next, the “model summary” shows some descriptive information about the LME model that was trained. This just shows descriptive information about the “groups”; in this case, groups will be individuals (as set by the --p-individual-id-column). The main results to examine will be the “model results” below the “model summary”. These results summarize the effects of each fixed effect (and their interactions) on the dependent variable (shannon diversity). This table shows parameter estimates, estimate standard errors, z scores, P values (P>|z|), and 95% confidence interval upper and lower bounds for each parameter. We see in this table that shannon diversity is significantly impacted by month of life and by diet, as well as several interacting factors. More information about LME models and the interpretation of these data can be found on the statsmodels LME description page, which provides a number of useful technical references for further reading.
Finally, scatter plots categorized by each “group column” are shown at the bottom of the visualization, with linear regression lines (plus 95% confidence interval in grey) for each group. If --p-lowess is enabled, instead locally weighted averages are shown for each group. Two different groups of scatter plots are shown. First, regression scatterplots show the relationship between state_column (x-axis) and metric (y-axis) for each sample. These plots are just used as a quick summary for reference; users are recommended to use the volatility visualizer for interactive plotting of their longitudinal data. Volatility plots can be used to qualitatively identify outliers that disproportionately drive the variance within individuals and groups, including by inspecting residuals in relation to control limits (see note below and the section on “Volatility analysis” for more details).
The second set of scatterplots are fit vs. residual plots, which show the relationship between metric predictions for each sample (on the x-axis), and the residual or observation error (prediction - actual value) for each sample (on the y-axis). Residuals should be roughly zero-centered and normal across the range of measured values. Uncentered, systematically high or low, and autocorrelated values could indicate a poor model. If your residual plots look like an ugly mess without any apparent relationship between values, you are doing a good job. If you see a U-shaped curve or other non-random distribution, either your predictor variables (group_columns and/or random_effects) are failing to capture all explanatory information, causing information to leak into your residuals, or else you are not using an appropriate model for your data 🙁. Check your predictor variables and available metadata columns to make sure you aren’t missing anything.
Note
If you want to dot your i’s and cross your t’s, residual and predicted values for each sample can be obtained in the “Download raw data as tsv” link below the regression scatterplots. This file can be input as metadata to the volatility visualizer to check whether residuals are correlated with other metadata columns. If they are, those columns should probably be used as prediction variables in your model! Control limits (± 2 and 3 standard deviations) can be toggled on/off to easily identify outliers, which can be particularly useful for re-examining fit vs. residual plots with this visualizer. 🍝

Longitudinal Beta diversity for M4 vs M6. The first step is to generate the alpha & beta diversity matrices needed for the longitudinal analyses.

qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M4M6only.txt \
  --p-where "[Study_arm_2] = 'Prospective' " \
  --o-filtered-table pk-sequencing-M4M6-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table!
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table pk-sequencing-M4M6-only-table.qza \
  --p-sampling-depth 1000 \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M4M6only.txt \
  --output-dir pk-sequencing-M4M6-only-core-metrics-results
Pairwise distance comparisons

The pairwise-distances visualizer also assesses changes between paired samples from two different “states”, but instead of taking a metadata column or artifact as input, it operates on a distance matrix to assess the distance between “pre” and “post” sample pairs, and tests whether these paired differences are significantly different between different groups, as specified by the group-column parameter. 

qiime longitudinal pairwise-distances \
  --i-distance-matrix oral-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file Aspirin_Oral_Map.tsv \
  --p-group-column Contents \
  --p-state-column Collection \
  --p-state-1 1 \
  --p-state-2 2 \
  --p-individual-id-column Subject_ID \
  --p-replicate-handling random \
  --o-visualization oral-bray-pairwise-distances.qzv

*16SMAR2023
*Had to include the ‘p-group-column” categorical variable even though I am only interested in alpha diversity change over time itself. This changed the hypothesis from “is the change in alpha diversity itself between the timepoints different from 0?”
qiime longitudinal pairwise-differences \
  --m-metadata-file 43_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4M6.txt \
  --m-metadata-file pk-sequencing-M2M4M6-only-core-metrics-results/bray_curtis_pcoa_results.qza \
  --p-metric shannon \
  --p-state-column Time_point \
  --p-state-1 2 \
  --p-state-2 6 \
  --p-individual-id-column Patient_ID \
  --p-replicate-handling random \
  --o-visualization pk-sequencing-M2M4M6-pcoa-pairwise-differences.qzv

qiime longitudinal pairwise-distances \
  --i-distance-matrix pk-sequencing-M4M6-only-core-metrics-results/unweighted_unifrac_distance_matrix.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M4M6only.txt \
--p-group-column Unit \
  --p-state-column Time_point \
  --p-state-1 4 \
  --p-state-2 6 \
  --p-individual-id-column Patient_ID \
  --p-replicate-handling random \
  --o-visualization pk-sequencing-M4M6-only-unweightedunifracs-pairwise-distances.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
**You need to visualize the file then export the raw values as TSV
qiime tools view pk-sequencing-M4M6-only-unweightedunifracs-pairwise-distances.qzv
Note
I will try to change the coding to ensure that the variables test for time effect
qiime longitudinal pairwise-distances --help
*Based on the help file, you can only look at paired difference, not at the time effect
qiime longitudinal pairwise-distances \
  --i-distance-matrix pk-sequencing-M1M2-only-core-metrics-results/unweighted_unifrac_distance_matrix.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M4M6only.txt \
--p-group-column Time_point \
  --p-individual-id-column Patient_ID \
  --p-replicate-handling random \
  --o-visualization test-pk-sequencing-M4M6-only-unweightedunifracs-pairwise-distancesV2.qzv

qiime tools view test-pk-sequencing-M4M6-only-unweightedunifracs-pairwise-distancesV2.qzv

Note
**You do not need to use a continuous variable in order to vew the PCOA chart, you can just visualize the one that is automatically created when you generate the matrices
qiime tools view /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/pk-sequencing-M4M6-only-core-metrics-results/bray_curtis_emperor.qzv
Linear mixed effect models
Linear mixed effects (LME) models test the relationship between a single response variable and one or more independent variables, where observations are made across dependent samples, e.g., in repeated-measures sampling experiments. This implementation takes at least one numeric state-column (e.g., Time) and one or more comma-separated group-columns (which may be categorical or numeric metadata columns; these are the fixed effects) as independent variables in a LME model, and plots regression plots of the response variable (“metric”) as a function of the state column and each group column. Additionally, the individual-id-column parameter should be a metadata column that indicates the individual subject/site that was sampled repeatedly. The response variable may either be a sample metadata mapping file column or a feature ID in the feature table. A comma-separated list of random effects can also be input to this action; a random intercept for each individual is included by default, but another common random effect that users may wish to use is a random slope for each individual, which can be set by using the state-column value as input to the random-effects parameter. Here we use LME to test whether alpha diversity (Shannon diversity index) changed over time and in response to delivery mode, diet, and sex in the ECAM data set.
Note
Deciding whether a factor is a fixed effect or a random effect can be complicated. In general, a factor should be a fixed effect if the different factor levels (metadata column values) represent (more or less) all possible discrete values. For example, delivery mode, sex, and diet (dominantly breast-fed or formula-fed) are designated as fixed effects in the example below. Conversely, a factor should be a random effect if its values represent random samples from a population. For example, we could imagine having metadata variables like body-weight, daily-kcal-from-breastmilk, number-of-peanuts-eaten-per-day, or mg-of-penicillin-administered-daily; such values would represent random samples from within a population, and are unlikely to capture all possible values representative of the whole population. Not sure about the factors in your experiment? 🤔 Consult a statistician or reputable statistical tome for guidance. 📚
*I am running a Linear Mixed effect model for the Site variable as a predictor of change in the overall study
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Shannon_Map.tsv \
  --m-metadata-file oral-gut-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Site \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization oral-gut-shannon-linear-mixed-effects.qzv
*For this model, I am using a specific mapping file where I copied the matched oral shanon diversity values (from DADA2 in R) to the gut samples, and I used sorting by Subject ID and by Collection to make sure I had the matching samples. (I actually exported the values from the oral-gut-core-metrics file to a text file, and used it in R to do the same analysis)
qiime longitudinal linear-mixed-effects \
  --m-metadata-file Aspirin_Oral_Gut_Map_Shannon_Dada.tsv \
  --m-metadata-file gut-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-group-columns Shannon_Dada_Oral \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --o-visualization gut-only-oral-shannon-linear-mixed-effects.qzv
*16SMAR2023 
*Trying a longitudinal model without the categorical variable “  --p-group-columns Site \” as a fixed effect. Alternatively, I can try using time as a fixed effect.
qiime longitudinal linear-mixed-effects \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M4M6only.txt \
   --m-metadata-file pk-sequencing-M4M6-only-core-metrics-results/Shannon_vector.qza \
  --p-metric shannon \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M4M6-shannon-linear-mixed-effects.qzv

*I also need to check the file names for beta diversity in order to pull the right metric for the LME model (https://forum.qiime2.org/t/p-metric-for-beta-diversity/4415/4)
qiime metadata tabulate --help

qiime metadata tabulate \
--m-input-file pk-sequencing-M4M6-only-core-metrics-results/Shannon_vector.qza \
  --o-visualization pk-sequencing-M4M6-metadada-tabulate-shannon-pairwise-differences.qzv

qiime metadata tabulate \
--m-input-file pk-sequencing-M4M6-only-core-metrics-results/bray_curtis_pcoa_results.qza\
  --o-visualization pk-sequencing-M4M6-metadada-tabulate-braycurtis-pcoa-pairwise-differences.qzv

qiime feature-data summarize --help
*Even when checking the post (https://forum.qiime2.org/t/alpha-and-beta-diversity-explanations-and-commands/2282), for other --p-metric- options, it seems that the main issue is that the file cannot be viewed as metadata directly as it is a distance matrix file
In order to check the format use the following commands

*** In order to tell which file type you have, you can use the qiime tools command
*This shows that the alpha diversity qza file inherently has a different structure from the beta diversity qza file.
qiime tools --help
qiime tools peek table.qza
qiime tools peek pk-sequencing-M4M6-only-core-metrics-results/Shannon_vector.qza
qiime tools peek pk-sequencing-M4M6-only-core-metrics-results/unweighted_unifrac_distance_matrix.qza

*Therefore, the solution seems to be using qiime metadata-tabulate to pull the beta diversity axes as qzv files, then merge them to the existing mapping file by sample ID, and finally being able to rerun the qiime longitudinal from a single metadatafile with the right variables

qiime metadata tabulate \
--m-input-file pk-sequencing-M4M6-only-core-metrics-results/Shannon_vector.qza \
  --o-visualization pk-sequencing-M4M6-metadada-tabulate-shannon-pairwise-differences.qzv

qiime metadata tabulate \
--m-input-file pk-sequencing-M4M6-only-core-metrics-results/bray_curtis_pcoa_results.qza\
  --o-visualization pk-sequencing-M4M6-metadada-tabulate-braycurtis-pcoa-pairwise-differences.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
**You need to visualize the file then export the raw values as TSV
qiime tools view pk-sequencing-M4M6-metadada-tabulate-shannon-pairwise-differences.qzv
qiime tools view pk-sequencing-M4M6-metadada-tabulate-braycurtis-pcoa-pairwise-differences.qzv
Note
Now I can move to the actual LME 
qiime longitudinal linear-mixed-effects \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M4M6only_V2.txt \
  --p-metric bc_pcoa_Axis1 \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M4M6-bc-pcoa-linear-mixed-effects.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M4M6-bc-pcoa-linear-mixed-effects.qzv
The visualizer produced by this command contains several results. First, the input parameters are shown at the top of the visualization for convenience (e.g., when flipping through multiple visualizations it is useful to have a summary). Next, the “model summary” shows some descriptive information about the LME model that was trained. This just shows descriptive information about the “groups”; in this case, groups will be individuals (as set by the --p-individual-id-column). The main results to examine will be the “model results” below the “model summary”. These results summarize the effects of each fixed effect (and their interactions) on the dependent variable (shannon diversity). This table shows parameter estimates, estimate standard errors, z scores, P values (P>|z|), and 95% confidence interval upper and lower bounds for each parameter. We see in this table that shannon diversity is significantly impacted by month of life and by diet, as well as several interacting factors. More information about LME models and the interpretation of these data can be found on the statsmodels LME description page, which provides a number of useful technical references for further reading.
Finally, scatter plots categorized by each “group column” are shown at the bottom of the visualization, with linear regression lines (plus 95% confidence interval in grey) for each group. If --p-lowess is enabled, instead locally weighted averages are shown for each group. Two different groups of scatter plots are shown. First, regression scatterplots show the relationship between state_column (x-axis) and metric (y-axis) for each sample. These plots are just used as a quick summary for reference; users are recommended to use the volatility visualizer for interactive plotting of their longitudinal data. Volatility plots can be used to qualitatively identify outliers that disproportionately drive the variance within individuals and groups, including by inspecting residuals in relation to control limits (see note below and the section on “Volatility analysis” for more details).
The second set of scatterplots are fit vs. residual plots, which show the relationship between metric predictions for each sample (on the x-axis), and the residual or observation error (prediction - actual value) for each sample (on the y-axis). Residuals should be roughly zero-centered and normal across the range of measured values. Uncentered, systematically high or low, and autocorrelated values could indicate a poor model. If your residual plots look like an ugly mess without any apparent relationship between values, you are doing a good job. If you see a U-shaped curve or other non-random distribution, either your predictor variables (group_columns and/or random_effects) are failing to capture all explanatory information, causing information to leak into your residuals, or else you are not using an appropriate model for your data 🙁. Check your predictor variables and available metadata columns to make sure you aren’t missing anything.
Note
If you want to dot your i’s and cross your t’s, residual and predicted values for each sample can be obtained in the “Download raw data as tsv” link below the regression scatterplots. This file can be input as metadata to the volatility visualizer to check whether residuals are correlated with other metadata columns. If they are, those columns should probably be used as prediction variables in your model! Control limits (± 2 and 3 standard deviations) can be toggled on/off to easily identify outliers, which can be particularly useful for re-examining fit vs. residual plots with this visualizer. 🍝
*NOV2023
MOSIO beta diversity at M1 & M2
*Making a table for the samples for which we have both sequencing and PK data from Moataz
qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal.txt\
  --p-where "[mosio_sequencing] = 1 " \
  --o-filtered-table m1m2-mosio-sequencing-only-table.qza

***You need to use the non-collapsed version of the filtered table so that the ASV table matches the phylogeny table! Set the sampling depth to 10000 based on the qzv description after generating the table
qiime diversity core-metrics-phylogenetic \
  --i-phylogeny hhri16s-rooted-tree.qza \
  --i-table m1m2-mosio-sequencing-only-table.qza \
  --p-sampling-depth 25000 \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal.txt\
  --output-dir m1m2-25k-mosio-sequencing-only-only-core-metrics-results

**When you run the core diversity metrics results you will get a list of alpha and beta diversity qza file that you can use for your alpha and beta analyses
ls m1m2-25k-mosio-sequencing-only-only-core-metrics-results/

*Testing if the distance matrices trajectories are different by diarrhea outcome
qiime longitudinal pairwise-distances \
  --i-distance-matrix m1m2-25k-mosio-sequencing-only-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --p-group-column everdiarrhea1categ \
  --p-state-column Collection_timepoint\
  --p-state-1 'M1' \
  --p-state-2 'M2_PK' \
  --p-individual-id-column patient_id \
  --p-replicate-handling random \
  --o-visualization bc_m1m2everdiarrhea1categ.qzv

qiime longitudinal pairwise-distances \
  --i-distance-matrix m1m2-25k-mosio-sequencing-only-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --p-group-column everdiarrhea1timeframe \
  --p-state-column Collection_timepoint\
  --p-state-1 'M1' \
  --p-state-2 'M2_PK' \
  --p-individual-id-column patient_id \
  --p-replicate-handling random \
  --o-visualization bc_m1m2everdiarrhea1timeframe.qzv

qiime longitudinal pairwise-distances \
  --i-distance-matrix m1m2-25k-mosio-sequencing-only-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --p-group-column everCTCAE21categ \
  --p-state-column Collection_timepoint\
  --p-state-1 'M1' \
  --p-state-2 'M2_PK' \
  --p-individual-id-column patient_id \
  --p-replicate-handling random \
  --o-visualization bc_m1m2everCTCAE21categ.qzv

qiime longitudinal pairwise-distances \
  --i-distance-matrix m1m2-25k-mosio-sequencing-only-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --p-group-column everCTCAE21timeframe \
  --p-state-column Collection_timepoint\
  --p-state-1 'M1' \
  --p-state-2 'M2_PK' \
  --p-individual-id-column patient_id \
  --p-replicate-handling random \
  --o-visualization bc_m1m2everCTCAE21timeframe.qzv

qiime longitudinal pairwise-distances \
  --i-distance-matrix m1m2-25k-mosio-sequencing-only-only-core-metrics-results/bray_curtis_distance_matrix.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --p-group-column maxCTCAEcateg \
  --p-state-column Collection_timepoint\
  --p-state-1 'M1' \
  --p-state-2 'M2_PK' \
  --p-individual-id-column patient_id \
  --p-replicate-handling random \
  --o-visualization bc_m1m2maxCTCAEcateg.qzv
A volatility plot will let us look at patterns of variation along principle coordinate axes starting from the same point. This can be helpful since inter-individual variation can be large and this visualizations lets us focus instead on magnitude of change in each group and in each individual.
Let’s use the q2-longitudinal plugin to look at how samples from an individual move along each PC. The --m-metadata-file column can take several types, including a metadata file (like our metadata.tsv) as well as a SampleData[AlphaDiversity], SampleData[Distance] (files “viewable” as metadata), or a PCoA artifact.
qiime longitudinal volatility \
  --m-metadata-file Aspirin_Oral_Map.tsv \
  --m-metadata-file oral-core-metrics-results/bray_curtis_pcoa_results.qza \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --p-default-group-column 'Contents' \
  --p-default-metric 'Axis 2' \
  --o-visualization oral_bray_curtis_volatility.qzv

qiime longitudinal volatility \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/bray_curtis_pcoa_results.qza \
  --p-state-column Collection_timepoint_num \
  --p-individual-id-column patient_id \
  --p-default-group-column 'everdiarrhea1categ' \
  --p-default-metric 'Axis 2' \
  --o-visualization bc_volatility_m1m2everdiarrhea1.qzv

qiime longitudinal volatility \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/bray_curtis_pcoa_results.qza \
  --p-state-column Collection_timepoint_num \
  --p-individual-id-column patient_id \
  --p-default-group-column 'everdiarrhea1timeframe' \
  --p-default-metric 'Axis 2' \
  --o-visualization bc_volatility_m1m2everdiarrhea1timeframe.qzv

qiime longitudinal volatility \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/bray_curtis_pcoa_results.qza \
  --p-state-column Collection_timepoint_num \
  --p-individual-id-column patient_id \
  --p-default-group-column 'everCTCAE21categ' \
  --p-default-metric 'Axis 2' \
  --o-visualization bc_volatility_m1m2everCTCAE21.qzv

qiime longitudinal volatility \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/bray_curtis_pcoa_results.qza \
  --p-state-column Collection_timepoint_num \
  --p-individual-id-column patient_id \
  --p-default-group-column 'everCTCAE21timeframe' \
  --p-default-metric 'Axis 2' \
  --o-visualization bc_volatility_m1m2everCTCAE21timeframe.qzv

qiime longitudinal volatility \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1M2_PS_longitudinal_pairwise.txt \
  --m-metadata-file m1m2-25k-mosio-sequencing-only-only-core-metrics-results/bray_curtis_pcoa_results.qza \
  --p-state-column Collection_timepoint_num \
  --p-individual-id-column patient_id \
  --p-default-group-column 'maxCTCAEcateg' \
  --p-default-metric 'Axis 2' \
  --o-visualization bc_volatility_m1m2maxCTCAEcateg.qzv


Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M2M4M6-only-volatility.qzv
qiime tools view bc_m1m2everdiarrhea1categ.qzv
qiime tools view bc_m1m2everdiarrhea1timeframe.qzv
qiime tools view bc_m1m2everCTCAE21categ.qzv
qiime tools view bc_m1m2everCTCAE21timeframe.qzv
qiime tools view bc_m1m2maxCTCAEcateg.qzv

qiime tools view bc_volatility_m1m2everdiarrhea1.qzv
qiime tools view bc_volatility_m1m2everdiarrhea1timeframe.qzv
qiime tools view bc_volatility_m1m2everCTCAE21.qzv
qiime tools view bc_volatility_m1m2everCTCAE21timeframe.qzv
qiime tools view bc_volatility_m1m2maxCTCAEcateg.qzv


Taxonomic analysis
In the next sections we’ll begin to explore the taxonomic composition of the samples, and again relate that to sample metadata. The first step in this process is to assign taxonomy to the sequences in our FeatureData[Sequence] QIIME 2 artifact. We’ll do that using a pre-trained Naive Bayes classifier and the q2-feature-classifier plugin. This classifier was trained on the Greengenes 13_8 99% OTUs, where the sequences have been trimmed to only include 250 bases from the region of the 16S that was sequenced in this analysis (the V4 region, bound by the 515F/806R primer pair). We’ll apply this classifier to our sequences, and we can generate a visualization of the resulting mapping from sequence to taxonomy.
Note
Taxonomic classifiers perform best when they are trained based on your specific sample preparation and sequencing parameters, including the primers that were used for amplification and the length of your sequence reads. Therefore in general you should follow the instructions in Training feature classifiers with q2-feature-classifier to train your own taxonomic classifiers. We provide some common classifiers on our data resources page, including Silva-based 16S classifiers, though in the future we may stop providing these in favor of having users train their own classifiers which will be most relevant to their sequence data.
curl -sL \
  "https://data.qiime2.org/2019.10/common/gg-13-8-99-515-806-nb-classifier.qza" > \
  "gg-13-8-99-515-806-nb-classifier.qza"
ls
Running the taxanomic analysis code
qiime feature-classifier classify-sklearn \
  --i-classifier gg-13-8-99-515-806-nb-classifier.qza \
  --i-reads oral-gut-rep-seqs.qza \
  --o-classification oral-gut-taxonomy.qza

qiime metadata tabulate \
  --m-input-file oral-gut-taxonomy.qza \
  --o-visualization oral-gut-taxonomy.qzv

qiime feature-classifier classify-sklearn \
  --i-classifier gg-13-8-99-515-806-nb-classifier.qza \
  --i-reads hhridental-rep-seqs.qza \
  --o-classification hhridental-taxonomy.qza

qiime metadata tabulate \
  --m-input-file hhridental-taxonomy.qza \
  --o-visualization hhridental-taxonomy.qzv

qiime feature-classifier classify-sklearn \
  --i-classifier gg-13-8-99-515-806-nb-classifier.qza \
  --i-reads hhri16s-rep-seqs.qza \
  --o-classification hhri16s-taxonomy.qza

qiime metadata tabulate \
  --m-input-file hhri16s-taxonomy.qza \
  --o-visualization hhri16s-taxonomy.qzv

*NOV2023
qiime feature-classifier classify-sklearn \
  --i-classifier gg-13-8-99-515-806-nb-classifier.qza \
  --i-reads hhri16s-rep-seqs.qza \
  --o-classification hhri16s-taxonomy.qza

qiime metadata tabulate \
  --m-input-file hhri16s-taxonomy.qza \
  --o-visualization hhri16s-taxonomy.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view oral-gut-taxonomy.qzv

qiime tools view hhridental-taxonomy.qzv

qiime tools view hhri16s-taxonomy.qzv
Question
Recall that our rep-seqs.qzv visualization allows you to easily BLAST the sequence associated with each feature against the NCBI nt database. Using that visualization and the taxonomy.qzv visualization created here, compare the taxonomic assignments with the taxonomy of the best BLAST hit for a few features. How similar are the assignments? If they’re dissimilar, at what taxonomic level do they begin to differ (e.g., species, genus, family, …)?

Next, we can view the taxonomic composition of our samples with interactive bar plots. Generate those plots with the following command and then open the visualization.
qiime taxa barplot \
  --i-table oral-gut-table.qza \
  --i-taxonomy oral-gut-taxonomy.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --o-visualization oral-gut-taxa-bar-plots.qzv

qiime taxa barplot \
  --i-table hhridental-noAR1-table.qza \
  --i-taxonomy hhridental-taxonomy.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --o-visualization hhridental-taxa-bar-plots.qzv

qiime taxa barplot \
  --i-table hhridental-PLA-table.qza \
  --i-taxonomy hhridental-taxonomy.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --o-visualization hhridental-pla-taxa-bar-plots.qzv

qiime taxa barplot \
  --i-table hhridental-female-table.qza \
  --i-taxonomy hhridental-taxonomy.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --o-visualization hhridental-female-taxa-bar-plots.qzv

*16s dataset (full and pk-only-table.qza)
qiime taxa barplot \
  --i-table hhri16s-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --o-visualization hhri16s-taxa-bar-plots.qzv

qiime taxa barplot \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --o-visualization pkonly-taxa-bar-plots.qzv

*16s dataset MAR2023 (full and pk-only-table.qza)
*You can use the download CSV option from the QZV files you generate in this step to also get an excel readout from the same data you are making the barplot from, and QIIME2 will let you output tables at each taxonomic level as well!

qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 35_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP.txt \
  --p-where "[Collection_Point] = 'PK' " \
  --o-filtered-table pk-only-table.qza

qiime taxa barplot \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --m-metadata-file 35_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP.txt \
  --o-visualization pkonly-taxa-bar-plots.qzv

*Making a table for the samples for which we have both sequencing and PK data from Moataz
*Found at “/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/18_HHRI_UMGC_Projects/04_12192022_Israni4_Project_019and020_16sSequencing/Documents/41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt” using the alt command after rightclicking to copy as a pathname.
qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --p-where "[AUC_sequencing] = 'Both' " \
  --o-filtered-table pk-sequencing-only-table.qza

qiime taxa barplot \
  --i-table pk-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-only-taxa-bar-plots.qzv

*16s dataset NOV2023 (all timepoints available for MOSIO and sequencing)
*You can use the download CSV option from the QZV files you generate in this step to also get an excel readout from the same data you are making the barplot from, and QIIME2 will let you output tables at each taxonomic level as well!
*Making a table for the samples for which we have both sequencing and PK data from Moataz
*Found at “/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/18_HHRI_UMGC_Projects/04_12192022_Israni4_Project_019and020_16sSequencing/Documents/41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt” using the alt command after rightclicking to copy as a pathname.
qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --p-where "[AUC_sequencing] = 'Both' " \
  --o-filtered-table pk-sequencing-only-table.qza

qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 48_MAP_MISSION_16S_sample_pull_mappingtemplate_NOV2023_PKMAP_nodups_V6.txt\
  --p-where "[mosio_sequencing] = 1 " \
  --o-filtered-table mosio-sequencing-only-table.qza

qiime taxa barplot \
  --i-table mosio-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --m-metadata-file 48_MAP_MISSION_16S_sample_pull_mappingtemplate_NOV2023_PKMAP_nodups_V6.txt\
  --o-visualization mosio-sequencing-only-taxa-bar-plots.qzv


Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view oral-gut-taxa-bar-plots.qzv
qiime tools view hhridental-taxa-bar-plots.qzv
qiime tools view hhridental-PLA-taxa-bar-plots.qzv
qiime tools view hhridental-female-taxa-bar-plots.qzv

qiime tools view hhri16s-taxa-bar-plots.qzv
qiime tools view pkonly-taxa-bar-plots.qzv
qiime tools view pkonly-taxa-bar-plots.qzv
qiime tools view pk-sequencing-only-taxa-bar-plots.qzv
qiime tools view mosio-sequencing-only-taxa-bar-plots.qzv

How to look at your QIIME2 taxonomic data 
*Example of how to subset the data and then look at it via QZV (this way we can see the OTU table and then export it as a RAW file from the QZV file).

qiime feature-table filter-samples \
  --i-table hhridental-table.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --p-where "[Cohort1] = 'AR1' " \
  --p-exclude-ids \
  --o-filtered-table hhridental-noAR1-table.qza

qiime feature-table summarize \
  --i-table hhridental-noAR1-table.qza \
  --o-visualization hhridental-noAR1-table.qzv \
  --m-sample-metadata-file Dental_HHRI_og_mapping_files_v3.txt

qiime tools view hhridental-noAR1-table.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view table.qzv
qiime tools view rep-seqs.qzv
*** In order to tell which file type you have, you can use the qiime tools command
qiime tools --help
qiime tools peek table.qza
qiime tools peek pkonly-table-l6.qza
*Example of how to export the data as absolute abundances through barplots
https://forum.qiime2.org/t/download-percent-relative-abundance-from-bar-plots/1529


*16s dataset MAR2023 (full and pk-only-table.qza)
*You can use the download CSV option from the QZV files you generate in this step to also get an excel readout from the same data you are making the barplot from, and QIIME2 will let you output tables at each taxonomic level as well!

qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 35_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP.txt \
  --p-where "[Collection_Point] = 'PK' " \
  --o-filtered-table pk-only-table.qza

qiime taxa barplot \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --m-metadata-file 35_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP.txt \
  --o-visualization pkonly-taxa-bar-plots.qzv

*Making a table for the samples for which we have both sequencing and PK data from Moataz
*Found at “/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/18_HHRI_UMGC_Projects/04_12192022_Israni4_Project_019and020_16sSequencing/Documents/41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt” using the alt command after rightclicking to copy as a pathname.
qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --p-where "[AUC_sequencing] = 'Both' " \
  --o-filtered-table pk-sequencing-only-table.qza

qiime taxa barplot \
  --i-table pk-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-only-taxa-bar-plots.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view oral-gut-taxa-bar-plots.qzv
qiime tools view hhridental-taxa-bar-plots.qzv
qiime tools view hhridental-PLA-taxa-bar-plots.qzv
qiime tools view hhridental-female-taxa-bar-plots.qzv

qiime tools view hhri16s-taxa-bar-plots.qzv
qiime tools view pkonly-taxa-bar-plots.qzv
qiime tools view pkonly-taxa-bar-plots.qzv
qiime tools view pk-sequencing-only-taxa-bar-plots.qzv
*Example of how to export the data as relative abundances

https://forum.qiime2.org/t/relative-abundances-of-taxonomy-analysis/4939
https://forum.qiime2.org/t/table-of-relative-abundances/4273/9
The instructions and links I provided above will actually create exactly the type of table you are looking for. The important thing to note is that you should not be downloading the .csv table from taxa bar plots visualizer.
For clarity this is how the work-flow should look like:
First, I'm going to assume you have already acquired your dada2/deblur feature-table and that you have your feature-classifier 53 as well. If these steps have not been performed, I suggest going back to the tutorials and following along those steps.
Ok, now we want to create a feature table that has taxonomy instead of feature IDs, basically what former OTU tables were in qiime1:
qiime taxa collapse \
  --i-table feature-table.qza \
  --i-taxonomy taxonomy.qza \
  --p-level 2 \
  --o-collapsed-table phyla-table.qza 
Where feature-table.qza is from output of dada2/deblur and the taxonomy.qza file comes from the classifier I've linked above.
Now we will convert this new frequency table to relative-frequency:
qiime feature-table relative-frequency \
--i-table phyla-table.qza \
--o-realtive-frequency-table rel-phyla-table.qza
This new artifact now has the relative-abundances we want. To get this into a text file we first export the data which is in biom format:
qiime tools export rel-phyla-table.qza \
--output-dir rel-table
We now have our new relative-frequency table in .biom format. Let's convert this to a text file that we can open easily:
# first move into the new directory
cd rel-table
# note that the table has been automatically labelled feature-table.biom
# You might want to change this filename for calrity
biom convert -i feature-table.biom -o rel-phyla-table.tsv --to-tsv

*16S MAR2023 data
qiime taxa barplot \
  --i-table pk-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-only-taxa-bar-plots.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-only-taxa-bar-plots.qzv
For relative abundances, it looks like I will need to collapse and export each taxonomic level individually
*16S MAR2023 data
*I will collaples the data at L5, L6 and L7

qiime taxa collapse \
  --i-table pk-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 5 \
  --o-collapsed-table pk-sequencing-only-table-l5.qza

qiime taxa collapse \
  --i-table pk-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table pk-sequencing-only-table-l6.qza

qiime taxa collapse \
  --i-table pk-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 7 \
  --o-collapsed-table pk-sequencing-only-table-l7.qza

*Now we will convert this new frequency table to relative-frequency:
qiime feature-table relative-frequency \
--i-table phyla-table.qza \
--o-realtive-frequency-table rel-phyla-table.qza

This new artifact now has the relative-abundances we want. To get this into a text file we first export the data which is in biom format:
qiime tools export rel-phyla-table.qza \
--output-dir rel-table

We now have our new relative-frequency table in .biom format. Let's convert this to a text file that we can open easily:
# first move into the new directory
cd rel-table

# note that the table has been automatically labelled feature-table.biom
# You might want to change this filename for calrity
biom convert -i feature-table.biom -o rel-phyla-table.tsv --to-tsv

*16SMAR2023
*L5 PK and sequencing only
qiime feature-table relative-frequency \
--i-table pk-sequencing-only-table-l5.qza \
--o-relative-frequency-table rel-pk-sequencing-only-table-l5.qza

Qiime tools --help
https://rpubs.com/aschrecengost/794628

qiime tools export \
--input-path /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/rel-pk-sequencing-only-table-l5.qza \
--output-path rel-pk-sequencing-l5-table

cd rel-pk-sequencing-l5-table #skipthis
# note that the table has been automatically labelled feature-table.biom
# You might want to change this filename for calrity
biom convert -i /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/rel-pk-sequencing-l5-table/feature-table.biom -o rel-pk-sequencing-only-l5-table.tsv --to-tsv
*L6 PK and sequencing only
qiime feature-table relative-frequency \
--i-table pk-sequencing-only-table-l6.qza \
--o-relative-frequency-table rel-pk-sequencing-only-table-l6.qza

Qiime tools --help
https://rpubs.com/aschrecengost/794628

qiime tools export \
--input-path /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/rel-pk-sequencing-only-table-l6.qza \
--output-path rel-pk-sequencing-l6-table

cd rel-pk-sequencing-l6-table #skipthis
# note that the table has been automatically labelled feature-table.biom
# You might want to change this filename for calrity
biom convert -i /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/rel-pk-sequencing-l6-table/feature-table.biom -o rel-pk-sequencing-only-l6-table.tsv --to-tsv

*L7 PK and sequencing only
qiime feature-table relative-frequency \
--i-table pk-sequencing-only-table-l7.qza \
--o-relative-frequency-table rel-pk-sequencing-only-table-l7.qza

Qiime tools --help
https://rpubs.com/aschrecengost/794628

qiime tools export \
--input-path /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/rel-pk-sequencing-only-table-l7.qza \
--output-path rel-pk-sequencing-l7-table

cd rel-pk-sequencing-l7-table #skipthis
# note that the table has been automatically labelled feature-table.biom
# You might want to change this filename for calrity
biom convert -i /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/rel-pk-sequencing-l7-table/feature-table.biom -o rel-pk-sequencing-only-l7-table.tsv --to-tsv

*16S NOV2023 data

qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 48_MAP_MISSION_16S_sample_pull_mappingtemplate_NOV2023_PKMAP_nodups_V6.txt\
  --p-where "[mosio_sequencing] = 1 " \
  --o-filtered-table mosio-sequencing-only-table.qza

*I will collapse the data at L5, L6 and L7

qiime taxa collapse \
  --i-table mosio-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 5 \
  --o-collapsed-table mosio-sequencing-only-table-l5.qza

qiime taxa collapse \
  --i-table mosio-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table mosio-sequencing-only-table-l6.qza

qiime taxa collapse \
  --i-table mosio-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 7 \
  --o-collapsed-table mosio-sequencing-only-table-l7.qza

*Now we will convert this new frequency table to relative-frequency:
qiime feature-table relative-frequency \
--i-table phyla-table.qza \
--o-realtive-frequency-table rel-phyla-table.qza

This new artifact now has the relative-abundances we want. To get this into a text file we first export the data which is in biom format:
qiime tools export rel-phyla-table.qza \
--output-dir rel-table

We now have our new relative-frequency table in .biom format. Let's convert this to a text file that we can open easily:
# first move into the new directory
cd rel-table

# note that the table has been automatically labelled feature-table.biom
# You might want to change this filename for calrity
biom convert -i feature-table.biom -o rel-phyla-table.tsv --to-tsv

*16SNOV2023
*L5 mosio and sequencing only
qiime feature-table relative-frequency \
--i-table mosio-sequencing-only-table-l5.qza \
--o-relative-frequency-table rel-mosio-sequencing-only-table-l5.qza

Qiime tools --help
https://rpubs.com/aschrecengost/794628

qiime tools export \
--input-path /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/rel-mosio-sequencing-only-table-l5.qza \
--output-path rel-mosio-sequencing-l5-table

cd rel-pk-sequencing-l5-table #skipthis
# note that the table has been automatically labelled feature-table.biom
# You might want to change this filename for calrity
biom convert -i /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/rel-mosio-sequencing-l5-table/feature-table.biom -o rel-mosio-sequencing-only-l5-table.tsv --to-tsv

*L6 mosio and sequencing only
qiime feature-table relative-frequency \
--i-table mosio-sequencing-only-table-l6.qza \
--o-relative-frequency-table rel-mosio-sequencing-only-table-l6.qza

Qiime tools --help
https://rpubs.com/aschrecengost/794628

qiime tools export \
--input-path /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/rel-mosio-sequencing-only-table-l6.qza \
--output-path rel-mosio-sequencing-l6-table

cd rel-pk-sequencing-l6-table #skipthis
# note that the table has been automatically labelled feature-table.biom
# You might want to change this filename for calrity
biom convert -i /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/rel-mosio-sequencing-l6-table/feature-table.biom -o rel-mosio-sequencing-only-l6-table.tsv --to-tsv

*L7 mosio and sequencing only
qiime feature-table relative-frequency \
--i-table mosio-sequencing-only-table-l7.qza \
--o-relative-frequency-table rel-mosio-sequencing-only-table-l7.qza

Qiime tools --help
https://rpubs.com/aschrecengost/794628

qiime tools export \
--input-path /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/rel-mosio-sequencing-only-table-l7.qza \
--output-path rel-mosio-sequencing-l7-table

cd rel-pk-sequencing-l7-table #skipthis
# note that the table has been automatically labelled feature-table.biom
# You might want to change this filename for calrity
biom convert -i /Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/rel-mosio-sequencing-l7-table/feature-table.biom -o rel-mosio-sequencing-only-l7-table.tsv --to-tsv
NOV2023 analysis
Once I have exported the absolute and relative abundances as TSV files, I will then use the relative abundance file, transpose the observations since biom conversion forces them to put the OTUs in rows, then align it with the absolute abundance file in a single excel spreadsheet. I will also modify the names by using the ="rel_"&A1 formula to append the “rel” prefix for relative abundances, and then remove the “;” and “__” using ctrl+H to replace in bulk. I will also modify the sample-id columns for relative and absolute abundances by using relative-sample-id and absolute-sample-id. These two columns will be used to align the data to the mapping file found at “/Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/48_MAP_MISSION_16S_sample_pull_mappingtemplate_NOV2023_PKMAP_nodups_V6.txt” and create the “/Volumes/RAVPOWER2/ROOK_04_PC/Israni4_Project_020_Datasets/49_MAP_MISSION_16S_shannon_taxonomy_V7.txt” file.

**I had to double check the moatazid alignment for 3524 M1, 4022 M1, 4027 M2.
Differential abundance testing with ANCOM at visit 3 (for the gut samples only)
ANCOM can be applied to identify features that are differentially abundant (i.e. present in different abundances) across sample groups. As with any bioinformatics method, you should be aware of the assumptions and limitations of ANCOM before using it. We recommend reviewing the ANCOM paper before using this method.
Note
Differential abundance testing in microbiome analysis is an active area of research. There are two QIIME 2 plugins that can be used for this: q2-gneiss and q2-composition. This section uses q2-composition, but there is another tutorial which uses gneiss on a different dataset if you are interested in learning more.
ANCOM is implemented in the q2-composition plugin. ANCOM assumes that few (less than about 25%) of the features are changing between groups. If you expect that more features are changing between your groups, you should not use ANCOM as it will be more error-prone (an increase in both Type I and Type II errors is possible). Because we expect a lot of features to change in abundance across body sites, in this tutorial we’ll filter our full feature table to only contain gut samples. We’ll then apply ANCOM to determine which, if any, sequence variants and genera are differentially abundant across the gut samples of our two subjects.
We’ll start by creating a feature table that contains only the gut samples. (To learn more about filtering, see the Filtering Data tutorial.)
In this tutorial, we collapse our feature table at the genus level (i.e. level 6 of the Greengenes taxonomy). It did not work when I tried to filter first then collapse, so I am collapsing the data first then filtering
qiime taxa collapse \
  --i-table gut-only-table.qza \
  --i-taxonomy oral-gut-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table gut-only-table-l6.qza

qiime feature-table filter-samples \
  --i-table gut-only-table-l6.qza \
  --m-metadata-file Aspirin_Oral_Gut_Map.tsv \
  --p-where "[Collection] = '2' " \
  --o-filtered-table gut-post-table.qza

qiime taxa collapse \
  --i-table hhridental-PLA-table.qza \
  --i-taxonomy hhridental-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table hhridental-PLA-table-l6.qza

qiime feature-table filter-samples \
  --i-table hhridental-noAR1-table.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --p-where "[Gender] = 'Female' " \
  --o-filtered-table hhridental-female-table.qza

qiime feature-table filter-samples \
  --i-table hhridental-female-table.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --p-where "[bodysite] = 'PLA' " \
  --o-filtered-table hhridental-female-PLA-table.qza

qiime taxa collapse \
  --i-table hhridental-female-PLA-table.qza \
  --i-taxonomy hhridental-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table hhridental-female-PLA-table-l6.qza

*Collapsing at L5 and L6 for the PK only samples

qiime taxa barplot \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --o-visualization pkonly-taxa-bar-plots.qzv

qiime taxa collapse \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 5 \
  --o-collapsed-table pkonly-table-l5.qza

qiime taxa collapse \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table pkonly-table-l6.qza

*MAR2023 16S collapsing at L6 for the PK only samples

qiime taxa barplot \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --m-metadata-file 35_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP.txt \
  --o-visualization pkonly-taxa-bar-plots.qzv

qiime taxa collapse \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table pkonly-table-l6.qza

*MAR2023 16S collapsing at L6 for the PK and sequencing only samples

qiime taxa barplot \
  --i-table pk-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-only-taxa-bar-plots.qzv

qiime taxa collapse \
  --i-table pk-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table pk-sequencing-only-table-l6.qza

ANCOM operates on a FeatureTable[Composition] QIIME 2 artifact, which is based on frequencies of features on a per-sample basis, but cannot tolerate frequencies of zero. To build the composition artifact, a FeatureTable[Frequency] artifact must be provided to add-pseudocount (an imputation method), which will produce the FeatureTable[Composition] artifact.
qiime composition add-pseudocount \
  --i-table hhridental-PLA-table-l6.qza \
  --o-composition-table comp-hhridental-PLA-table-l6.qza

qiime composition ancom \
  --i-table comp-hhridental-PLA-table-l6.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --m-metadata-column Cohort1 \
  --o-visualization ancom-hhridental-PLA-table-l6.qzv

qiime composition add-pseudocount \
  --i-table hhridental-female-PLA-table-l6.qza \
  --o-composition-table comp-hhridental-female-PLA-table-l6.qza

qiime composition ancom \
  --i-table comp-hhridental-female-PLA-table-l6.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --m-metadata-column Cohort1 \
  --o-visualization ancom-hhridental-female-PLA-table-l6.qzv

*Collapsing at L5 and L6 for the PK only samples, need to filter any observations that are missing from the EHR_Cat_invitro variable for ANCOM

qiime taxa barplot \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --o-visualization pkonly-taxa-bar-plots.qzv

qiime feature-table filter-samples \
  --i-table pk-only-table.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --p-where "[EHR_Cat_invitro] IN ('Low', 'High') " \
  --o-filtered-table pk-only-table.qza

qiime taxa collapse \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 5 \
  --o-collapsed-table pkonly-table-l5.qza

qiime composition add-pseudocount \
  --i-table pkonly-table-l5.qza \
  --o-composition-table comp-pkonly-table-l5.qza

qiime composition ancom \
  --i-table comp-pkonly-table-l5.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --m-metadata-column EHR_Cat_invitro \
  --o-visualization ancom-pkonly-table-l5.qzv

qiime taxa collapse \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table pkonly-table-l6.qza

qiime composition add-pseudocount \
  --i-table pkonly-table-l6.qza \
  --o-composition-table comp-pkonly-table-l6.qza

qiime composition ancom \
  --i-table comp-pkonly-table-l6.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --m-metadata-column EHR_Cat_invitro \
  --o-visualization ancom-pkonly-table-l6.qzv

*Collapsing at L1, L2, L3, L4, L5, L6 and l7 for the PK only samples, need to filter any observations that are missing from the EHR_Cat_invitro variable for ANCOM

qiime taxa collapse \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 1 \
  --o-collapsed-table pkonly-table-l1.qza

qiime composition add-pseudocount \
  --i-table pkonly-table-l1.qza \
  --o-composition-table comp-pkonly-table-l1.qza

qiime composition ancom \
  --i-table comp-pkonly-table-l1.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --m-metadata-column EHR_Cat_invitro \
  --o-visualization ancom-pkonly-table-l1.qzv

qiime taxa collapse \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 2 \
  --o-collapsed-table pkonly-table-l2.qza

qiime composition add-pseudocount \
  --i-table pkonly-table-l2.qza \
  --o-composition-table comp-pkonly-table-l2.qza

qiime composition ancom \
  --i-table comp-pkonly-table-l2.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --m-metadata-column EHR_Cat_invitro \
  --o-visualization ancom-pkonly-table-l2.qzv

qiime taxa collapse \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 3 \
  --o-collapsed-table pkonly-table-l3.qza

qiime composition add-pseudocount \
  --i-table pkonly-table-l3.qza \
  --o-composition-table comp-pkonly-table-l3.qza

qiime composition ancom \
  --i-table comp-pkonly-table-l3.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --m-metadata-column EHR_Cat_invitro \
  --o-visualization ancom-pkonly-table-l3.qzv

qiime taxa collapse \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 4 \
  --o-collapsed-table pkonly-table-l4.qza

qiime composition add-pseudocount \
  --i-table pkonly-table-l4.qza \
  --o-composition-table comp-pkonly-table-l4.qza

qiime composition ancom \
  --i-table comp-pkonly-table-l4.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --m-metadata-column EHR_Cat_invitro \
  --o-visualization ancom-pkonly-table-l4.qzv

qiime taxa collapse \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 7 \
  --o-collapsed-table pkonly-table-l7.qza

qiime composition add-pseudocount \
  --i-table pkonly-table-l7.qza \
  --o-composition-table comp-pkonly-table-l7.qza

qiime composition ancom \
  --i-table comp-pkonly-table-l7.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --m-metadata-column EHR_Cat_invitro \
  --o-visualization ancom-pkonly-table-l7.qzv

*MAR2023 16S. Generating ANCOM Pseudocount artifact at L6 level

qiime taxa barplot \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --m-metadata-file 35_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP.txt \
  --o-visualization pkonly-taxa-bar-plots.qzv

qiime taxa collapse \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table pkonly-table-l6.qza

qiime composition add-pseudocount \
  --i-table pkonly-table-l6.qza \
  --o-composition-table comp-pkonly-table-l6.qza

qiime composition ancom \
  --i-table comp-pkonly-table-l6.qza \
  --m-metadata-file 35_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP.txt \
  --o-visualization ancom-pkonly-table-l6-test.qzv

qiime composition ancom \
  --i-table comp-pkonly-table-l6.qza \
  --m-metadata-file 35_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP.txt \
  --m-metadata-column Unit \
  --o-visualization ancom-pkonly-table-l6.qzv

*MAR2023 16S collapsing for the PK and sequencing only samples at the family, genus and species levels
*Since I didn’t find an association between AUC or EHR as categorical variable, I will test the prospective vs cross-sectional subgroup (Study_arm) to see if there is an association.
*There were some missing observations in the “Study_arm” variable so I created “Study_arm_2” with no missing observations
 
*Family

qiime taxa barplot \
  --i-table pk-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-only-taxa-bar-plots.qzv

qiime taxa collapse \
  --i-table pk-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 5 \
  --o-collapsed-table pk-sequencing-only-table-l5.qza

qiime composition add-pseudocount \
  --i-table pk-sequencing-only-table-l5.qza \
  --o-composition-table comp-pk-sequencing-only-table-l5.qza

*You need to add the column as a metadata option for a categorical variable
qiime composition ancom \
  --i-table comp-pk-sequencing-only-table-l5.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization ancom-pk-sequencing-only-table-l5-test.qzv

qiime composition ancom \
  --i-table comp-pk-sequencing-only-table-l5.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_AUC_0_12_dose_adj_cat \
  --o-visualization ancom-pk-sequencing-only-AUC-table-l5.qzv

qiime composition ancom \
  --i-table comp-pk-sequencing-only-table-l5.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_EHR_5.12_cat \
  --o-visualization ancom-pk-sequencing-only-EHR-table-l5.qzv

qiime composition ancom \
  --i-table comp-pk-sequencing-only-table-l5.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column Study_arm_2 \
  --o-visualization ancom-pk-sequencing-only-arm-table-l5.qzv

*Genus;

qiime taxa barplot \
  --i-table pk-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-only-taxa-bar-plots.qzv

qiime taxa collapse \
  --i-table pk-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table pk-sequencing-only-table-l6.qza

qiime composition add-pseudocount \
  --i-table pk-sequencing-only-table-l6.qza \
  --o-composition-table comp-pk-sequencing-only-table-l6.qza

*You need to add the column as a metadata option for a categorical variable
qiime composition ancom \
  --i-table comp-pk-sequencing-only-table-l6.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization ancom-pk-sequencing-only-table-l6-test.qzv

qiime composition ancom \
  --i-table comp-pk-sequencing-only-table-l6.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_AUC_0_12_dose_adj_cat \
  --o-visualization ancom-pk-sequencing-only-AUC-table-l6.qzv

qiime composition ancom \
  --i-table comp-pk-sequencing-only-table-l6.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_EHR_5.12_cat \
  --o-visualization ancom-pk-sequencing-only-EHR-table-l6.qzv

qiime composition ancom \
  --i-table comp-pk-sequencing-only-table-l6.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column Study_arm_2 \
  --o-visualization ancom-pk-sequencing-only-arm-table-l6.qzv

*Species;
qiime taxa barplot \
  --i-table pk-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-only-taxa-bar-plots.qzv

qiime taxa collapse \
  --i-table pk-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 7 \
  --o-collapsed-table pk-sequencing-only-table-l7.qza

qiime composition add-pseudocount \
  --i-table pk-sequencing-only-table-l7.qza \
  --o-composition-table comp-pk-sequencing-only-table-l7.qza

*You need to add the column as a metadata option for a categorical variable
qiime composition ancom \
  --i-table comp-pk-sequencing-only-table-l7.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization ancom-pk-sequencing-only-table-l7-test.qzv

qiime composition ancom \
  --i-table comp-pk-sequencing-only-table-l7.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_AUC_0_12_dose_adj_cat \
  --o-visualization ancom-pk-sequencing-only-AUC-table-l7.qzv

qiime composition ancom \
  --i-table comp-pk-sequencing-only-table-l7.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_EHR_5.12_cat \
  --o-visualization ancom-pk-sequencing-only-EHR-table-l7.qzv

qiime composition ancom \
  --i-table comp-pk-sequencing-only-table-l7.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column Study_arm_2 \
  --o-visualization ancom-pk-sequencing-only-arm-table-l7.qzv

Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view gut-post-l6-ancom-contents.qzv
qiime tools view ancom-hhridental-PLA-table-l6.qzv
qiime tools view ancom-hhridental-female-PLA-table-l6.qzv
qiime tools view ancom-pkonly-table-l5.qzv
qiime tools view ancom-pkonly-table-l6.qzv

qiime tools view ancom-pkonly-table-l1.qzv
qiime tools view ancom-pkonly-table-l2.qzv
qiime tools view ancom-pkonly-table-l3.qzv
qiime tools view ancom-pkonly-table-l4.qzv
qiime tools view ancom-pkonly-table-l7.qzv

qiime tools view ancom-pkonly-table-l6-test.qzv
qiime tools view ancom-pkonly-table-l6.qzv

MAR2023 analysis

qiime tools view ancom-pk-sequencing-only-AUC-table-l5.qzv
qiime tools view ancom-pk-sequencing-only-EHR-table-l5.qzv
qiime tools view ancom-pk-sequencing-only-arm-table-l5.qzv

qiime tools view ancom-pk-sequencing-only-AUC-table-l6.qzv
qiime tools view ancom-pk-sequencing-only-EHR-table-l6.qzv
qiime tools view ancom-pk-sequencing-only-arm-table-l6.qzv

qiime tools view ancom-pk-sequencing-only-AUC-table-l7.qzv
qiime tools view ancom-pk-sequencing-only-EHR-table-l7.qzv
qiime tools view ancom-pk-sequencing-only-arm-table-l7.qzv
Making 2 subsets from the PK and sequencing data stratified by prospective vs cross sectional

*Example of how to subset the data and then look at it via QZV (this way we can see the OTU table and then export it as a RAW file from the QZV file). 
qiime feature-table filter-samples \
  --i-table hhridental-table.qza \
  --m-metadata-file Dental_HHRI_og_mapping_files_v3.txt \
  --p-where "[Cohort1] = 'AR1' " \
  --p-exclude-ids \
  --o-filtered-table hhridental-noAR1-table.qza

*16S MAR2023
qiime feature-table filter-samples \
  --i-table pk-sequencing-only-table.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --p-where "[Study_arm_2] = 'Cross-sectional' " \
  --p-exclude-ids \
  --o-filtered-table pk-sequencing-cs-only-table.qza

qiime feature-table filter-samples \
  --i-table pk-sequencing-only-table.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --p-where "[Study_arm_2] = 'Prospective' " \
  --p-exclude-ids \
  --o-filtered-table pk-sequencing-ps-only-table.qza

*Generating barplots for the prospective vs cross-sectional datasets.

qiime taxa barplot \
  --i-table pk-sequencing-cs-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-cs-only-taxa-bar-plots.qzv

qiime taxa barplot \
  --i-table pk-sequencing-ps-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-ps-only-taxa-bar-plots.qzv

*Collapsing at the L5, L6 and L7 levels for the PK and sequencing only samples at the family, genus, and species levels

*Family level Cross-sectional
qiime taxa collapse \
  --i-table pk-sequencing-cs-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 5 \
  --o-collapsed-table pk-sequencing-cs-only-table-l5.qza

qiime composition add-pseudocount \
  --i-table pk-sequencing-cs-only-table-l5.qza \
  --o-composition-table comp-pk-sequencing-cs-only-table-l5.qza

qiime composition ancom \
  --i-table comp-pk-sequencing-cs-only-table-l5.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_AUC_0_12_dose_adj_cat \
  --o-visualization ancom-pk-sequencing-cs-only-AUC-table-l5.qzv

qiime composition ancom \
  --i-table comp-pk-sequencing-cs-only-table-l5.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_EHR_5_12_cat \
  --o-visualization ancom-pk-sequencing-cs-only-EHR-table-l5.qzv

*Genus level Cross-sectional

qiime taxa collapse \
  --i-table pk-sequencing-cs-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table pk-sequencing-cs-only-table-l6.qza

qiime composition add-pseudocount \
  --i-table pk-sequencing-cs-only-table-l6.qza \
  --o-composition-table comp-pk-sequencing-cs-only-table-l6.qza

qiime composition ancom \
  --i-table comp-pk-sequencing-cs-only-table-l6.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_AUC_0_12_dose_adj_cat \
  --o-visualization ancom-pk-sequencing-cs-only-AUC-table-l6.qzv

qiime composition ancom \
  --i-table comp-pk-sequencing-cs-only-table-l6.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_EHR_5_12_cat \
  --o-visualization ancom-pk-sequencing-cs-only-EHR-table-l6.qzv

*Species Cross-sectional

qiime taxa collapse \
  --i-table pk-sequencing-cs-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 7 \
  --o-collapsed-table pk-sequencing-cs-only-table-l7.qza

qiime composition add-pseudocount \
  --i-table pk-sequencing-cs-only-table-l7.qza \
  --o-composition-table comp-pk-sequencing-cs-only-table-l7.qza

qiime composition ancom \
  --i-table comp-pk-sequencing-cs-only-table-l7.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_AUC_0_12_dose_adj_cat \
  --o-visualization ancom-pk-sequencing-cs-only-AUC-table-l7.qzv

qiime composition ancom \
  --i-table comp-pk-sequencing-cs-only-table-l7.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_EHR_5_12_cat \
  --o-visualization ancom-pk-sequencing-cs-only-EHR-table-l7.qzv

*Family level prospective
qiime taxa collapse \
  --i-table pk-sequencing-ps-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 5 \
  --o-collapsed-table pk-sequencing-ps-only-table-l5.qza

qiime composition add-pseudocount \
  --i-table pk-sequencing-ps-only-table-l5.qza \
  --o-composition-table comp-pk-sequencing-ps-only-table-l5.qza

qiime composition ancom \
  --i-table comp-pk-sequencing-ps-only-table-l5.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_AUC_0_12_dose_adj_cat \
  --o-visualization ancom-pk-sequencing-ps-only-AUC-table-l5.qzv

qiime composition ancom \
  --i-table comp-pk-sequencing-ps-only-table-l5.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_EHR_5_12_cat \
  --o-visualization ancom-pk-sequencing-ps-only-EHR-table-l5.qzv

*Genus level prospective

qiime taxa collapse \
  --i-table pk-sequencing-ps-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table pk-sequencing-ps-only-table-l6.qza

qiime composition add-pseudocount \
  --i-table pk-sequencing-ps-only-table-l6.qza \
  --o-composition-table comp-pk-sequencing-ps-only-table-l6.qza

qiime composition ancom \
  --i-table comp-pk-sequencing-ps-only-table-l6.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_AUC_0_12_dose_adj_cat \
  --o-visualization ancom-pk-sequencing-ps-only-AUC-table-l6.qzv

qiime composition ancom \
  --i-table comp-pk-sequencing-ps-only-table-l6.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_EHR_5_12_cat \
  --o-visualization ancom-pk-sequencing-ps-only-EHR-table-l6.qzv

*Species prospective

qiime taxa collapse \
  --i-table pk-sequencing-ps-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 7 \
  --o-collapsed-table pk-sequencing-ps-only-table-l7.qza

qiime composition add-pseudocount \
  --i-table pk-sequencing-cs-only-table-l7.qza \
  --o-composition-table comp-pk-sequencing-ps-only-table-l7.qza

qiime composition ancom \
  --i-table comp-pk-sequencing-ps-only-table-l7.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_AUC_0_12_dose_adj_cat \
  --o-visualization ancom-pk-sequencing-ps-only-AUC-table-l7.qzv

qiime composition ancom \
  --i-table comp-pk-sequencing-ps-only-table-l7.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_EHR_5_12_cat \
  --o-visualization ancom-pk-sequencing-ps-only-EHR-table-l7.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view gut-post-l6-ancom-contents.qzv
qiime tools view ancom-hhridental-PLA-table-l6.qzv
qiime tools view ancom-hhridental-female-PLA-table-l6.qzv
qiime tools view ancom-pkonly-table-l5.qzv
qiime tools view ancom-pkonly-table-l6.qzv

*16S MAR2023 stratified analysis

qiime tools view ancom-pk-sequencing-cs-only-AUC-table-l5.qzv
qiime tools view ancom-pk-sequencing-cs-only-EHR-table-l5.qzv

qiime tools view ancom-pk-sequencing-cs-only-AUC-table-l6.qzv
qiime tools view ancom-pk-sequencing-cs-only-EHR-table-l6.qzv

qiime tools view ancom-pk-sequencing-cs-only-AUC-table-l7.qzv
qiime tools view ancom-pk-sequencing-cs-only-EHR-table-l7.qzv

qiime tools view ancom-pk-sequencing-ps-only-AUC-table-l5.qzv
qiime tools view ancom-pk-sequencing-ps-only-EHR-table-l5.qzv

qiime tools view ancom-pk-sequencing-ps-only-AUC-table-l6.qzv
qiime tools view ancom-pk-sequencing-ps-only-EHR-table-l6.qzv

qiime tools view ancom-pk-sequencing-ps-only-AUC-table-l7.qzv
qiime tools view ancom-pk-sequencing-ps-only-EHR-table-l7.qzv
Repeating the analysis using the median cat variable instead of EHR_Cat (MedianCat_612)
*Collapsing at L5 and L6 for the PK only samples, need to filter any observations that are missing from the EHR_Cat_invitro variable for ANCOM

qiime taxa barplot \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --o-visualization pkonly-taxa-bar-plots.qzv

qiime feature-table filter-samples \
  --i-table pk-only-table.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --p-where "[EHR_Cat_invitro] IN ('Low', 'High') " \
  --o-filtered-table pk-only-table.qza

qiime taxa collapse \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 5 \
  --o-collapsed-table pkonly-table-l5.qza

qiime composition add-pseudocount \
  --i-table pkonly-table-l5.qza \
  --o-composition-table comp-pkonly-table-l5.qza

qiime composition ancom \
  --i-table comp-pkonly-table-l5.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --m-metadata-column MedianCat_612\
  --o-visualization ancom-pkonly2-table-l5.qzv

qiime taxa collapse \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table pkonly-table-l6.qza

qiime composition add-pseudocount \
  --i-table pkonly-table-l6.qza \
  --o-composition-table comp-pkonly-table-l6.qza

qiime composition ancom \
  --i-table comp-pkonly-table-l6.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --m-metadata-column MedianCat_612\
  --o-visualization ancom-pkonly2-table-l6.qzv

*Collapsing at L1, L2, L3, L4, L5, L6 and l7 for the PK only samples, need to filter any observations that are missing from the EHR_Cat_invitro variable for ANCOM

qiime taxa collapse \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 1 \
  --o-collapsed-table pkonly-table-l1.qza

qiime composition add-pseudocount \
  --i-table pkonly-table-l1.qza \
  --o-composition-table comp-pkonly-table-l1.qza

qiime composition ancom \
  --i-table comp-pkonly-table-l1.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --m-metadata-column MedianCat_612 \
  --o-visualization ancom-pkonly2-table-l1.qzv

qiime taxa collapse \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 2 \
  --o-collapsed-table pkonly-table-l2.qza

qiime composition add-pseudocount \
  --i-table pkonly-table-l2.qza \
  --o-composition-table comp-pkonly-table-l2.qza

qiime composition ancom \
  --i-table comp-pkonly-table-l2.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --m-metadata-column MedianCat_612\
  --o-visualization ancom-pkonly2-table-l2.qzv

qiime taxa collapse \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 3 \
  --o-collapsed-table pkonly-table-l3.qza

qiime composition add-pseudocount \
  --i-table pkonly-table-l3.qza \
  --o-composition-table comp-pkonly-table-l3.qza

qiime composition ancom \
  --i-table comp-pkonly-table-l3.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --m-metadata-column MedianCat_612\
  --o-visualization ancom-pkonly2-table-l3.qzv

qiime taxa collapse \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 4 \
  --o-collapsed-table pkonly-table-l4.qza

qiime composition add-pseudocount \
  --i-table pkonly-table-l4.qza \
  --o-composition-table comp-pkonly-table-l4.qza

qiime composition ancom \
  --i-table comp-pkonly-table-l4.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --m-metadata-column MedianCat_612\
  --o-visualization ancom-pkonly2-table-l4.qzv

qiime taxa collapse \
  --i-table pk-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 7 \
  --o-collapsed-table pkonly-table-l7.qza

qiime composition add-pseudocount \
  --i-table pkonly-table-l7.qza \
  --o-composition-table comp-pkonly-table-l7.qza

qiime composition ancom \
  --i-table comp-pkonly-table-l7.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --m-metadata-column MedianCat_612\
  --o-visualization ancom-pkonly2-table-l7.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view gut-post-l6-ancom-contents.qzv
qiime tools view ancom-hhridental-PLA-table-l6.qzv
qiime tools view ancom-hhridental-female-PLA-table-l6.qzv
qiime tools view ancom-pkonly2-table-l5.qzv
qiime tools view ancom-pkonly2-table-l6.qzv
qiime tools view ancom-pkonly2-table-l1.qzv
qiime tools view ancom-pkonly2-table-l2.qzv
qiime tools view ancom-pkonly2-table-l3.qzv
qiime tools view ancom-pkonly2-table-l4.qzv
qiime tools view ancom-pkonly2-table-l7.qzv


*NOV2023 MOSIO M1 samples

MOSIO Differential abundance at M1
*Making a table for the samples for which we have both sequencing and PK data from Moataz
qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --p-where "[mosio_sequencing] = 1 " \
  --o-filtered-table m1-mosio-sequencing-only-table.qza

qiime taxa barplot \
  --i-table m1-mosio-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --o-visualization m1-mosio-sequencing-only-table.qzv

qiime tools view m1-mosio-sequencing-only-table.qzv

*I will collapse the data at L5, L6 and L7

qiime taxa collapse \
  --i-table m1-mosio-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 5 \
  --o-collapsed-table m1-mosio-sequencing-only-table-l5.qza

qiime taxa collapse \
  --i-table m1-mosio-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table m1-mosio-sequencing-only-table-l6.qza

qiime taxa collapse \
  --i-table m1-mosio-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 7 \
  --o-collapsed-table m1-mosio-sequencing-only-table-l7.qza

*Level5 analysis

qiime composition add-pseudocount \
  --i-table m1-mosio-sequencing-only-table-l5.qza \
  --o-composition-table comp-m1-mosio-sequencing-only-table-l5.qza

qiime composition ancom \
  --i-table comp-m1-mosio-sequencing-only-table-l5.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --m-metadata-column everdiarrhea1categ \
  --o-visualization ancom-m1-mosio-everdiarrhea-l5.qzv

qiime tools view ancom-m1-mosio-everdiarrhea-l5.qzv

qiime composition ancom \
  --i-table comp-m1-mosio-sequencing-only-table-l5.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --m-metadata-column everdiarrhea1timeframe \
  --o-visualization ancom-m1-mosio-everdiarrheatimeframe-l5.qzv

qiime tools view ancom-m1-mosio-everdiarrheatimeframe-l5.qzv

qiime composition ancom \
  --i-table comp-m1-mosio-sequencing-only-table-l5.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --m-metadata-column everCTCAE21categ \
  --o-visualization ancom-m1-mosio-everCTCAE21-l5.qzv

qiime tools view ancom-m1-mosio-everCTCAE21-l5.qzv

qiime composition ancom \
  --i-table comp-m1-mosio-sequencing-only-table-l5.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --m-metadata-column everCTCAE21timeframe \
  --o-visualization ancom-m1-mosio-everCTCAE21timeframe-l5.qzv

qiime tools view ancom-m1-mosio-everCTCAE21timeframe-l5.qzv

qiime composition ancom \
  --i-table comp-m1-mosio-sequencing-only-table-l5.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --m-metadata-column maxCTCAEcateg \
  --o-visualization ancom-m1-mosio-maxCTCAEcateg-l5.qzv

qiime tools view ancom-m1-mosio-maxCTCAEcateg-l5.qzv

*Level 6 analysis
qiime composition add-pseudocount \
  --i-table m1-mosio-sequencing-only-table-l6.qza \
  --o-composition-table comp-m1-mosio-sequencing-only-table-l6.qza
qiime composition ancom \
  --i-table comp-m1-mosio-sequencing-only-table-l6.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --m-metadata-column everdiarrhea1categ \
  --o-visualization ancom-m1-mosio-everdiarrhea-l6.qzv

qiime tools view ancom-m1-mosio-everdiarrhea-l6.qzv

qiime composition ancom \
  --i-table comp-m1-mosio-sequencing-only-table-l6.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --m-metadata-column everdiarrhea1timeframe \
  --o-visualization ancom-m1-mosio-everdiarrheatimeframe-l6.qzv

qiime tools view ancom-m1-mosio-everdiarrheatimeframe-l6.qzv

qiime composition ancom \
  --i-table comp-m1-mosio-sequencing-only-table-l6.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --m-metadata-column everCTCAE21categ \
  --o-visualization ancom-m1-mosio-everCTCAE21-l6.qzv

qiime tools view ancom-m1-mosio-everCTCAE21-l6.qzv

qiime composition ancom \
  --i-table comp-m1-mosio-sequencing-only-table-l6.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --m-metadata-column everCTCAE21timeframe \
  --o-visualization ancom-m1-mosio-everCTCAE21timeframe-l6.qzv

qiime tools view ancom-m1-mosio-everCTCAE21timeframe-l6.qzv

qiime composition ancom \
  --i-table comp-m1-mosio-sequencing-only-table-l6.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --m-metadata-column maxCTCAEcateg \
  --o-visualization ancom-m1-mosio-maxCTCAEcateg-l6.qzv

qiime tools view ancom-m1-mosio-maxCTCAEcateg-l6.qzv

*Level 7 analysis

qiime composition add-pseudocount \
  --i-table m1-mosio-sequencing-only-table-l7.qza \
  --o-composition-table comp-m1-mosio-sequencing-only-table-l7.qza

qiime composition ancom \
  --i-table comp-m1-mosio-sequencing-only-table-l7.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --m-metadata-column everdiarrhea1categ \
  --o-visualization ancom-m1-mosio-everdiarrhea-l7.qzv

qiime tools view ancom-m1-mosio-everdiarrhea-l7.qzv

qiime composition ancom \
  --i-table comp-m1-mosio-sequencing-only-table-l7.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --m-metadata-column everdiarrhea1timeframe \
  --o-visualization ancom-m1-mosio-everdiarrheatimeframe-l7.qzv

qiime tools view ancom-m1-mosio-everdiarrheatimeframe-l7.qzv

qiime composition ancom \
  --i-table comp-m1-mosio-sequencing-only-table-l7.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --m-metadata-column everCTCAE21categ \
  --o-visualization ancom-m1-mosio-everCTCAE21-l7.qzv

qiime tools view ancom-m1-mosio-everCTCAE21-l7.qzv

qiime composition ancom \
  --i-table comp-m1-mosio-sequencing-only-table-l7.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --m-metadata-column everCTCAE21timeframe \
  --o-visualization ancom-m1-mosio-everCTCAE21timeframe-l7.qzv

qiime tools view ancom-m1-mosio-everCTCAE21timeframe-l7.qzv

qiime composition ancom \
  --i-table comp-m1-mosio-sequencing-only-table-l7.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --m-metadata-column maxCTCAEcateg \
  --o-visualization ancom-m1-mosio-maxCTCAEcateg-l7.qzv

qiime tools view ancom-m1-mosio-maxCTCAEcateg-l7.qzv



MOSIO Differential abundance at M2
*Making a table for the samples for which we have both sequencing and PK data from Moataz
qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --p-where "[mosio_sequencing] = 1 " \
  --o-filtered-table m2-mosio-sequencing-only-table.qzv

qiime taxa barplot \
  --i-table m2-mosio-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --o-visualization m2-mosio-sequencing-only-table.qzv

*I will collapse the data at L5, L6 and L7

qiime taxa collapse \
  --i-table m2-mosio-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 5 \
  --o-collapsed-table m2-mosio-sequencing-only-table-l5.qza

qiime taxa collapse \
  --i-table m2-mosio-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table m2-mosio-sequencing-only-table-l6.qza

qiime taxa collapse \
  --i-table m2-mosio-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 7 \
  --o-collapsed-table m2-mosio-sequencing-only-table-l7.qza


*Level5 analysis

qiime composition add-pseudocount \
  --i-table m2-mosio-sequencing-only-table-l5.qza \
  --o-composition-table comp-m2-mosio-sequencing-only-table-l5.qza

qiime composition ancom \
  --i-table comp-m2-mosio-sequencing-only-table-l5.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --m-metadata-column everdiarrhea1categ \
  --o-visualization ancom-m2-mosio-everdiarrhea-l5.qzv

qiime tools view ancom-m2-mosio-everdiarrhea-l5.qzv

qiime composition ancom \
  --i-table comp-m2-mosio-sequencing-only-table-l5.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --m-metadata-column everdiarrhea1timeframe \
  --o-visualization ancom-m2-mosio-everdiarrheatimeframe-l5.qzv

qiime tools view ancom-m2-mosio-everdiarrheatimeframe-l5.qzv

qiime composition ancom \
  --i-table comp-m2-mosio-sequencing-only-table-l5.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --m-metadata-column everCTCAE21categ \
  --o-visualization ancom-m2-mosio-everCTCAE21-l5.qzv

qiime tools view ancom-m2-mosio-everCTCAE21-l5.qzv

qiime composition ancom \
  --i-table comp-m2-mosio-sequencing-only-table-l5.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --m-metadata-column everCTCAE21timeframe \
  --o-visualization ancom-m2-mosio-everCTCAE21timeframe-l5.qzv

qiime tools view ancom-m2-mosio-everCTCAE21timeframe-l5.qzv

qiime composition ancom \
  --i-table comp-m2-mosio-sequencing-only-table-l5.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --m-metadata-column maxCTCAEcateg \
  --o-visualization ancom-m2-mosio-maxCTCAEcateg-l5.qzv

qiime tools view ancom-m2-mosio-maxCTCAEcateg-l5.qzv

*Level 6 analysis
qiime composition add-pseudocount \
  --i-table m2-mosio-sequencing-only-table-l6.qza \
  --o-composition-table comp-m2-mosio-sequencing-only-table-l6.qza
qiime composition ancom \
  --i-table comp-m2-mosio-sequencing-only-table-l6.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --m-metadata-column everdiarrhea1categ \
  --o-visualization ancom-m2-mosio-everdiarrhea-l6.qzv

qiime tools view ancom-m2-mosio-everdiarrhea-l6.qzv

qiime composition ancom \
  --i-table comp-m2-mosio-sequencing-only-table-l6.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --m-metadata-column everdiarrhea1timeframe \
  --o-visualization ancom-m2-mosio-everdiarrheatimeframe-l6.qzv

qiime tools view ancom-m2-mosio-everdiarrheatimeframe-l6.qzv

qiime composition ancom \
  --i-table comp-m2-mosio-sequencing-only-table-l6.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --m-metadata-column everCTCAE21categ \
  --o-visualization ancom-m2-mosio-everCTCAE21-l6.qzv

qiime tools view ancom-m2-mosio-everCTCAE21-l6.qzv

qiime composition ancom \
  --i-table comp-m2-mosio-sequencing-only-table-l6.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --m-metadata-column everCTCAE21timeframe \
  --o-visualization ancom-m2-mosio-everCTCAE21timeframe-l6.qzv

qiime tools view ancom-m2-mosio-everCTCAE21timeframe-l6.qzv

qiime composition ancom \
  --i-table comp-m2-mosio-sequencing-only-table-l6.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --m-metadata-column maxCTCAEcateg \
  --o-visualization ancom-m2-mosio-maxCTCAEcateg-l6.qzv

qiime tools view ancom-m2-mosio-maxCTCAEcateg-l6.qzv

*Level 7 analysis

qiime composition add-pseudocount \
  --i-table m2-mosio-sequencing-only-table-l7.qza \
  --o-composition-table comp-m2-mosio-sequencing-only-table-l7.qza

qiime composition ancom \
  --i-table comp-m2-mosio-sequencing-only-table-l7.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --m-metadata-column everdiarrhea1categ \
  --o-visualization ancom-m2-mosio-everdiarrhea-l7.qzv

qiime tools view ancom-m2-mosio-everdiarrhea-l7.qzv

qiime composition ancom \
  --i-table comp-m2-mosio-sequencing-only-table-l7.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --m-metadata-column everdiarrhea1timeframe \
  --o-visualization ancom-m2-mosio-everdiarrheatimeframe-l7.qzv

qiime tools view ancom-m2-mosio-everdiarrheatimeframe-l7.qzv

qiime composition ancom \
  --i-table comp-m2-mosio-sequencing-only-table-l7.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --m-metadata-column everCTCAE21categ \
  --o-visualization ancom-m2-mosio-everCTCAE21-l7.qzv

qiime tools view ancom-m2-mosio-everCTCAE21-l7.qzv

qiime composition ancom \
  --i-table comp-m2-mosio-sequencing-only-table-l7.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --m-metadata-column everCTCAE21timeframe \
  --o-visualization ancom-m2-mosio-everCTCAE21timeframe-l7.qzv

qiime tools view ancom-m2-mosio-everCTCAE21timeframe-l7.qzv

qiime composition ancom \
  --i-table comp-m2-mosio-sequencing-only-table-l7.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --m-metadata-column maxCTCAEcateg \
  --o-visualization ancom-m2-mosio-maxCTCAEcateg-l7.qzv

qiime tools view ancom-m2-mosio-maxCTCAEcateg-l7.qzv

*** Since ANCOM seems to impletement a fairly strict criteria for significance, trying to run ALDEX 2 instead https://library.qiime2.org/plugins/q2-aldex2/24/

The next step will be to run ALDEx2. The full pipeline is implemented in the aldex2 function (modularity will be added in the near-future) The input is the FeatureTable[Frequency], as well as a metadata file. The metadata file is necessary for defining the different groups you will be testing. For this tutorial, the groups are identified by the subject column from the metadata file. ALDEx2 automatically adds a prior (see Results here for more technical details of the prior) to remove zeros from the data, and filters any samples with 0 reads.
qiime aldex2 aldex2 \
    --i-table gut-table.qza \
    --m-metadata-file sample-metadata.tsv \
    --m-metadata-column subject \
    --output-dir gut-test

*NOV2023 MOSIO M1 samples

MOSIO Differential abundance at M1
*Making a table for the samples for which we have both sequencing and PK data from Moataz
qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --p-where "[mosio_sequencing] = 1 " \
  --o-filtered-table m1-mosio-sequencing-only-table.qza

qiime taxa barplot \
  --i-table m1-mosio-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --o-visualization m1-mosio-sequencing-only-table.qza

*I will collapse the data at L5, L6 and L7

qiime taxa collapse \
  --i-table m1-mosio-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 5 \
  --o-collapsed-table m1-mosio-sequencing-only-table-l5.qza

qiime taxa collapse \
  --i-table m1-mosio-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table m1-mosio-sequencing-only-table-l6.qza

qiime taxa collapse \
  --i-table m1-mosio-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 7 \
  --o-collapsed-table m1-mosio-sequencing-only-table-l7.qza

*Need to install the aldex2 plugin first (https://library.qiime2.org/plugins/q2-aldex2/24/)
conda install -c dgiguere q2-aldex2

qiime aldex2 aldex2 \
    --i-table m1-mosio-sequencing-only-table-l5.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --m-metadata-column maxCTCAEcateg \
    --output-dir m1-mosio-maxCTCAE

Feature volatility analysis
Note
This pipeline is a supervised regression method. Read the sample classifier tutorial for more details on the general process, outputs (e.g., feature importance scores), and interpretation of supervised regression models.
This pipeline identifies features that are predictive of a numeric metadata column, “state_column” (e.g., time), and plots their relative frequencies across states using interactive feature volatility plots (only important features are plotted). A supervised learning regressor is used to identify important features and assess their ability to predict sample states. state_column will typically be a measure of time, but any numeric metadata column can be used and this is not strictly a longitudinal method, unless if the individual_id_column parameter is used (in which case feature volatility plots will contain per-individual spaghetti lines, as described above). 🍝
qiime longitudinal feature-volatility \
  --i-table oral-table.qza \
  --m-metadata-file Aspirin_Oral_Map.tsv \
  --p-state-column Collection \
  --p-individual-id-column Subject_ID \
  --p-n-estimators 10 \
  --output-dir oral_feature_volatility_test
All of the parameters used in this pipeline are described for other q2-longitudinal actions or in the sample classifier tutorial, so will not be discussed here. This pipeline produces multiple outputs:
	•	volatility-plot contains an interactive feature volatility plot. This is very similar to the plots produced by the volaility visualizer described above, with a couple key differences. First, only features are viewable as “metrics” (plotted on the y-axis). Second, feature metadata (feature importances and descriptive statistics) are plotted as bar charts below the volatility plot. The relative frequencies of different features can be plotted in the volatility chart by either selecting the “metric” selection tool, or by clicking on one of the bars in the bar plot. This makes it convenient to select features for viewing based on importance or other feature metadata. By default, the most important feature is plotted in the volatility plot when the visualization is viewed. Different feature metadata can be selected and sorted using the control panel to the right of the bar charts. Most of these should be self-explanatory, except for “cumulative average change” (the cumulative magnitude of change, both positive and negative, across states, and averaged across samples at each state), and “net average change” (positive and negative “cumulative average change” is summed to determine whether a feature increased or decreased in abundance between baseline and end of study).
	•	accuracy-results display the predictive accuracy of the regression model. This is important to view, as important features are meaningless if the model is inaccurate. See the sample classifier tutorial for more description of regressor accuracy results.
	•	feature-importance contains the importance scores of all features. This is viewable in the feature volatility plot, but this artifact is nonetheless output for convenience. See the sample classifier tutorial for more description of feature importance scores.
	•	filtered-table is a FeatureTable[RelativeFrequency] artifact containing only important features. This is output for convenience.
	•	sample-estimator contains the trained sample regressor. This is output for convenience, just in case you plan to regress additional samples. See the sample classifier tutorial for more description of the SampleEstimator type.
Now we will cover basic interpretation of these data. By looking at the accuracy-results, we see that the regressor model is actually quite accurate, even though only 10 estimators were used for training the regressor (in practice a larger number of estimators should be used, and the default for the --p-n-estimators parameter is 100 estimators; see the sample classifier tutorial for more description of this parameter). Great! The feature importances will be meaningful. Month of life can be accurately predicted based on the ASV composition of these samples, suggesting that a programmatic succession of ASVs occurs during early life in this childhood cohort.
Next we will view the feature volatility plot. We see that the most important feature is more than twice as important as the second most important feature, so this one is really predictive of a subject’s age! Sure enough, looking at the volatility chart we see that this feature is almost entirely absent at birth in most subjects, but increases gradually starting at around 8 months of life. Its average frequency is greater in vaginally born subjects than cesarean-delivered subjects, so could be an interesting candidate for statistical testing, e.g., with linear-mixed-effects. We can also use metadata tabulate to merge the feature importance data with taxonomy assignments to determine the taxonomic classification of this ASV (and other important features).
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view oral_feature_volatility_test/volatility_plot.qzv
**So the “metric_column” variable indicates the OTU ids which are indicative of the largest change between the aspirin and placebo group. It is also saved as a filtered table

Output visualizations:
qiime metadata tabulate \
  --m-input-file oral_feature_volatility_test/filtered_table.qza \
  --o-visualization oral_feature_volatility_test/filtered_table.qzv
 Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view oral_feature_volatility_test/filtered_table.qzv
IF I pull out the taxonomy file, I should in theory be able to match the data
qiime metadata tabulate \
  --m-input-file oral-taxonomy.qza \
  --o-visualization oral-taxonomy.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view oral-taxonomy.qzv
*** The neat trick is that you can view both the oral-taxonomy.qzv file and the filtered-table.qzv file in your browser, and use the search box in the oral-taxonomy.qzv file to identify the top features form the feature volatility analysis. However this is only visual, and you do not get any p-values or beta estimates to go with it from what I can tell
Yay!!! Now looking at balance analyses using gneiss

Differential abundance analysis with gneiss
https://docs.qiime2.org/2018.2/tutorials/gneiss/
https://docs.qiime2.org/2019.10/tutorials/gneiss/
Option 1: Correlation-clustering
This option should be your default option. We will employ unsupervised clustering via Ward’s hierarchical clustering to obtain Principal Balances. In essence, this will define the partitions of microbes that commonly co-occur with each other using Ward hierarchical clustering, which is defined by the following metric.
d(x,y)=V[lnxy]d(x,y)=V[ln⁡xy]
Where xx and yy represent the proportions of two microbes across all of the samples. If two microbes are highly correlated, then this quantity will shrink close to zero. Ward hierarchical cluster will then use this distance metric to iteratively cluster together groups of microbes that are correlated with each other. In the end, the tree that we obtain will highlight the high level structure and identify any blocks within in the data.
qiime gneiss correlation-clustering \
  --i-table oral-table.qza \
  --o-clustering oral-hierarchy.qza

qiime gneiss correlation-clustering \
  --i-table pk-only-table.qza \
  --o-clustering pk-only-hierarchy.qza

*Mar2023 16s Analysis;
qiime gneiss correlation-clustering \
  --i-table pk-sequencing-only-table.qza \
  --o-clustering pk-sequencing-only-hierarchy.qza

An important consideration for downstream analyses is the problem of overfitting. When using gradient-clustering, you are creating a tree to best highlight compositional differences along the metadata category of your choice, and it is possible to get false positives. Use gradient-clustering with caution.
Building linear models using balances
Now that we have a tree that defines our partitions, we can perform the isometric log ratio (ILR) transform. The ILR transform computes the log ratios between groups at each node in the tree.
qiime gneiss ilr-hierarchical \
  --i-table oral-table.qza \
  --i-tree oral-hierarchy.qza \
  --o-balances oral-balances.qza

qiime gneiss ilr-hierarchical \
  --i-table pk-only-table.qza \
  --i-tree pk-only-hierarchy.qza \
  --o-balances pk-only-balances.qza

*Mar2023 16s Analysis;
qiime gneiss ilr-hierarchical \
  --i-table pk-sequencing-only-table.qza \
  --i-tree pk-sequencing-only-hierarchy.qza \
  --o-balances pk-sequencing-only-balances.qza

Now that we have the log ratios of each node of our tree, we can run linear regression on the balances. The linear regression that we will be running is called a multivariate response linear regression, which boils down to performing a linear regression on each balance separately.
We can use this to attempt to associate microbial abundances with environmental variables. Running these models has multiple advantages over standard univariate regression, as it avoids many of the issues associated with overfitting, and can allow us to gain perspective about community-wide perturbations based on environmental parameters.
Since the microbial abundances can be mapped directly to balances, we can perform the multivariate regression directly on the balances. The model that we will be building is represented as follows
y⃗ =β0→+βSubject→Xsubject→+βsex→Xsex→+βage→XAge→+βsCD14ugml→XsCD14ugml→+βLBPugml→XLBPugml→y→=β0→+βSubject→Xsubject→+βsex→Xsex→+βage→XAge→+βsCD14ugml→XsCD14ugml→+βLBPugml→XLBPugml→
Where y⃗ y→ represents the matrix of balances to be predicted, βi→βi→ represents a vector of coefficients for covariate ii and Xi→Xi→ represents the measures for covariate ii.
Remember that ANOVA is a special case of linear regression - every problem that can be solved by ANOVA can be reformulated as a linear regression. See this post for more details. So we can still answer the same sort of differential abundance questions using this technique, and we can start asking more precise questions, controlling for different potential confounding variables or even interaction effects.
qiime gneiss ols-regression \
  --p-formula "gender+age_today+Contents+Collection" \
  --i-table oral-balances.qza \
  --i-tree oral-hierarchy.qza \
  --m-metadata-file Aspirin_Oral_Map.tsv \
  --o-visualization oral-regression_summary.qzv

qiime gneiss ols-regression \
  --p-formula "AUCratio" \
  --i-table pk-only-balances.qza \
  --i-tree pk-only-hierarchy.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --o-visualization pkonly-regression_AUCratiosummary.qzv

qiime gneiss ols-regression \
  --p-formula "EHR_Cat_invitro" \
  --i-table pk-only-balances.qza \
  --i-tree pk-only-hierarchy.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --o-visualization pkonly-regression_EHRCATsummary.qzv

*16S Mar2023

qiime gneiss ols-regression \
  --p-formula "MPA_AUC_0_12_dose_adj" \
  --i-table pk-sequencing-only-balances.qza \
  --i-tree pk-sequencing-only-hierarchy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-only-regression-AUC-summary.qzv

qiime gneiss ols-regression \
  --p-formula "MPA_EHR_5_12" \
  --i-table pk-sequencing-only-balances.qza \
  --i-tree pk-sequencing-only-hierarchy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-only-regression-EHR-summary.qzv

Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view oral-regression_summary.qzv

qiime tools view pkonly-regression_AUCratiosummary.qzv
qiime tools view pkonly-regression_EHRCATsummary.qzv

qiime tools view pk-sequencing-only-regression-AUC-summary.qzv
qiime tools view pk-sequencing-only-regression-EHR-summary.qzv

https://docs.qiime2.org/2018.2/tutorials/gneiss/

Now we have a summary of the regression model. Specifically we want to see which covariates impact the model the most, which balances are meaningful, and how much potential overfitting is going on.
There are a few things to note in the regression summary. There is an R2�2 in the summary, which gives information about the variance in the community is explained by the regression model. From what we can see, the regression can explain about 10% of the community. This is typical for what we see in human gut microbes, since there is a very high amount of confounding variation due to genetics, diet, environment, etc.
To evaluate the explanatory model of a single covariate, a leave-one-variable-out approach is used. One variable is left out, and the change in R2�2 is calculated. The larger the change is, the stronger the effect of the covariate is. In this case, Subject is the largest explanatory factor, explaining 2% of the variation.
To make sure that we aren’t overfitting, 10-fold cross validation is performed. This will split the data into 10 partitions, build the model on 9 of the those partitions and use the remaining partition to measure the prediction accuracy. This process is repeat 10 times, once for each round of cross-validation. The within model error (mse), R2�2 and the prediction accuracy (pred_mse) are reported for each round of cross validation. Here, the prediction accuracy is less than the within model error, suggesting that over fitting is not happening.
Next, we have a heatmap visualizing all of the coefficient p-values for all of the balances. The columns of the heatmap represent balances, and the rows of the heatmap represent covariates. The heatmap is colored by the negative log of the p-value, highlighting potentially significant p-values. A hover tool is enabled to allow for specific coefficient values and their corresponding p-values to be obtained, and zooming is enabled to allow for navigation of interesting covariates and balances.
Next are the prediction and residual plots. Here, only the top two balances are plotted, and the prediction residuals from the model are projected onto these two balances. From these plots we can see that the predicted points lie within the same region as the original communities. However, we can see that the residuals have roughly the same variance as the predictions. This is a little unsettling - but note that we can only explain 10% of the community variance, so these sorts of calculations aren’t completely unexpected.
The branch lengths in the visualized tree are also scaled by the explained sum of squares in the models. The longest branch lengths correspond to the most informative balances. This can allow us to get a high-level overview of the most important balances in the model. From this plot and the above heatmap, we can see that balance y0�0 is important. These balances not only have very small p-values (with p<0.05�<0.05) for differentiating subjects, but they also have the largest branch lengths in the tree diagram. This suggests that these two partitions of microbes could differentiate the CFS patients from the controls.
We can visualize these balances on a heatmap to see which groups of OTUs they represent. By default, the values within the feature table are log-scaled, with the sample means centered around zero.


qiime gneiss dendrogram-heatmap \
  --i-table composition.qza \
  --i-tree hierarchy.qza \
  --m-metadata-file sample-metadata.tsv \
  --m-metadata-column Subject \
  --p-color-map seismic \
  --o-visualization heatmap.qzv

*16sMar2023
qiime gneiss ols-regression \
  --p-formula "MPA_AUC_0_12_dose_adj" \
  --i-table pk-sequencing-only-balances.qza \
  --i-tree pk-sequencing-only-hierarchy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-only-regression-AUC-summary.qzv

qiime gneiss ols-regression \
  --p-formula "MPA_EHR_5_12" \
  --i-table pk-sequencing-only-balances.qza \
  --i-tree pk-sequencing-only-hierarchy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-only-regression-EHR-summary.qzv

qiime gneiss dendrogram-heatmap \
  --i-table pk-sequencing-only-table.qza \
  --i-tree pk-sequencing-only-hierarchy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_AUC_0_12_dose_adj_cat \
  --p-color-map seismic \
  --o-visualization pk-sequencing-only-AUC-heatmap.qzv

qiime gneiss dendrogram-heatmap \
  --i-table pk-sequencing-only-table.qza \
  --i-tree pk-sequencing-only-hierarchy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_EHR_5_12_cat \
  --p-color-map seismic \
  --o-visualization pk-sequencing-only-EHR-heatmap.qzv

Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pkonly_auc_y0_taxa_summary.qzv
qiime tools view pkonly-regression_EHRCATsummary.qzv

qiime tools view pk-sequencing-only-AUC-heatmap.qzv
qiime tools view pk-sequencing-only-EHR-heatmap.qzv

As noted in the legend, the numerators for each balance are highlighted in light red, while the denominators are highlighted in dark red. From here, we can see that the denominator of y0y0 has few OTUs compared to the numerator of y0y0. These taxa in the denominator could be interesting, so let’s investigate the taxonomies making up this balance with balance_taxonomy.
Specifically we’ll plot a boxplot and identify taxa that could be explaining the differences between the control and patient groups.
qiime gneiss balance-taxonomy \
  --i-table composition.qza \
  --i-tree hierarchy.qza \
  --i-taxonomy taxa.qza \
  --p-taxa-level 2 \
  --p-balance-name 'y0' \
  --m-metadata-file sample-metadata.tsv \
  --m-metadata-column Subject \
  --o-visualization y0_taxa_summary.qzv

qiime gneiss balance-taxonomy \
  --i-table pk-only-table.qza \
  --i-tree pk-only-hierarchy.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-taxa-level 6 \
  --p-balance-name 'y0' \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --m-metadata-column AUCratio \
  --o-visualization pkonly_auc_y0_taxa_summary.qzv

*16S MAR2023
qiime gneiss ols-regression \
  --p-formula "MPA_AUC_0_12_dose_adj" \
  --i-table pk-sequencing-only-balances.qza \
  --i-tree pk-sequencing-only-hierarchy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-only-regression-AUC-summary.qzv

qiime gneiss ols-regression \
  --p-formula "MPA_EHR_5_12" \
  --i-table pk-sequencing-only-balances.qza \
  --i-tree pk-sequencing-only-hierarchy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-only-regression-EHR-summary.qzv

qiime gneiss balance-taxonomy \
  --i-table pk-sequencing-only-table.qza \
  --i-tree pk-sequencing-only-hierarchy.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-taxa-level 6 \
  --p-balance-name 'y0' \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_AUC_0_12_dose_adj_cat \
  --o-visualization pk-sequencing-only-auc-y0-l6-summary.qzv

qiime gneiss balance-taxonomy \
  --i-table pk-sequencing-only-table.qza \
  --i-tree pk-sequencing-only-hierarchy.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-taxa-level 6 \
  --p-balance-name 'y0' \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_EHR_5_12_cat \
  --o-visualization pk-sequencing-only-ehr-y0-l6-summary.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pkonly_auc_y0_taxa_summary.qzv
qiime tools view pkonly-regression_EHRCATsummary.qzv

qiime tools view pk-sequencing-only-auc-y0-l6-summary.qzv
qiime tools view pk-sequencing-only-ehr-y0-l6-summary.qzv



Option 2: Gradient-clustering¶
An alternative to correlation-clustering is to create a tree based on a numeric metadata category. With gradient-clustering, we can group taxa that occur in similar ranges of a metadata category. In this example, we will create a tree (hierarchy) using the metadata category Age. Note that the metadata category can have no missing variables, and must be numeric.
qiime gneiss gradient-clustering \
  --i-table table.qza \
  --m-gradient-file sample-metadata.tsv \
  --m-gradient-column Age \
  --o-clustering gradient-hierarchy.qza

*16S MAR2023 
qiime gneiss ols-regression \
  --p-formula "MPA_AUC_0_12_dose_adj" \
  --i-table pk-sequencing-only-balances.qza \
  --i-tree pk-sequencing-only-hierarchy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-only-regression-AUC-summary.qzv

qiime gneiss ols-regression \
  --p-formula "MPA_EHR_5_12" \
  --i-table pk-sequencing-only-balances.qza \
  --i-tree pk-sequencing-only-hierarchy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-only-regression-EHR-summary.qzv





qiime gneiss gradient-clustering \
  --i-table pk-sequencing-only-table.qza \
  --m-gradient-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-gradient-column MPA_AUC_0_12_dose_adj \
  --o-clustering pk-sequencing-only-auc-gradient-hierarchy.qza

qiime gneiss gradient-clustering \
  --i-table pk-sequencing-only-table.qza \
  --m-gradient-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-gradient-column MPA_EHR_5_12 \
  --o-clustering pk-sequencing-only-ehr-gradient-hierarchy.qza

Output artifacts:
	•	gradient-hierarchy.qza: view | download
An important consideration for downstream analyses is the problem of overfitting. When using gradient-clustering, you are creating a tree to best highlight compositional differences along the metadata category of your choice, and it is possible to get false positives. Use gradient-clustering with caution.
Building linear models using balances
Now that we have a tree that defines our partitions, we can perform the isometric log ratio (ILR) transform. The ILR transform computes the log ratios between groups at each node in the tree.
qiime gneiss ilr-hierarchical \
  --i-table table.qza \
  --i-tree hierarchy.qza \
  --o-balances balances.qza

*16S MAR2023
qiime gneiss ilr-hierarchical \
  --i-table pk-sequencing-only-table.qza \
  --i-tree pk-sequencing-only-auc-gradient-hierarchy.qza \
  --o-balances pk-sequencing-only-auc-gradient-balances.qza

qiime gneiss ilr-hierarchical \
  --i-table pk-sequencing-only-table.qza \
  --i-tree pk-sequencing-only-ehr-gradient-hierarchy.qza \
  --o-balances pk-sequencing-only-ehr-gradient-balances.qza	

Output artifacts:
	•	balances.qza: view | download
Now that we have the log ratios of each node of our tree, we can run linear regression on the balances. The linear regression that we will be running is called a multivariate response linear regression, which boils down to performing a linear regression on each balance separately.
We can use this to attempt to associate microbial abundances with environmental variables. Running these models has multiple advantages over standard univariate regression, as it avoids many of the issues associated with overfitting, and can allow us to gain perspective about community-wide perturbations based on environmental parameters.
Since the microbial abundances can be mapped directly to balances, we can perform the multivariate regression directly on the balances. The model that we will be building is represented as follows
y⃗ =β0→+βSubject→Xsubject→+βsex→Xsex→+βage→XAge→+βsCD14ugml→XsCD14ugml→+βLBPugml→XLBPugml→�→=�0→+��������→��������→+����→����→+����→����→+����14����→����14����→+��������→��������→
Where y⃗ �→ represents the matrix of balances to be predicted, βi→��→ represents a vector of coefficients for covariate i� and Xi→��→ represents the measures for covariate i�.
Remember that ANOVA is a special case of linear regression - every problem that can be solved by ANOVA can be reformulated as a linear regression. See this post for more details. So we can still answer the same sort of differential abundance questions using this technique, and we can start asking more precise questions, controlling for different potential confounding variables or even interaction effects.
qiime gneiss ols-regression \
  --p-formula "Subject+Sex+Age+BMI+sCD14ugml+LBPugml+LPSpgml" \
  --i-table balances.qza \
  --i-tree hierarchy.qza \
  --m-metadata-file sample-metadata.tsv \
  --o-visualization regression_summary.qzv


*16S MAR2023
qiime gneiss ols-regression \
  --p-formula "MPA_AUC_0_12_dose_adj" \
  --i-table pk-sequencing-only-auc-gradient-balances.qza \
  --i-tree pk-sequencing-only-auc-gradient-hierarchy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-only-auc-gradient-regression-summary.qzv

qiime gneiss ols-regression \
  --p-formula "MPA_EHR_5_12" \
  --i-table pk-sequencing-only-ehr-gradient-balances.qza \
  --i-tree pk-sequencing-only-ehr-gradient-hierarchy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-only-ehr-gradient-regression-summary.qzv

Output visualizations:
	•	regression_summary.qzv: view | download

Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pkonly_auc_y0_taxa_summary.qzv
qiime tools view pkonly-regression_EHRCATsummary.qzv

qiime tools view pk-sequencing-only-auc-gradient-regression-summary.qzv
qiime tools view pk-sequencing-only-ehr-gradient-regression-summary.qzv

Now we have a summary of the regression model. Specifically we want to see which covariates impact the model the most, which balances are meaningful, and how much potential overfitting is going on.
There are a few things to note in the regression summary. There is an R2�2 in the summary, which gives information about how much of the variance in the community is explained by the regression model. From what we can see, the regression can explain about 10% of the community variation. This is typical for what we see in human gut microbiomes, since there is a very high amount of confounding variation due to genetics, diet, environment, etc.
To evaluate the explanatory model of a single covariate, a leave-one-variable-out approach is used. One variable is left out, and the change in R2�2 is calculated. The larger the change is, the stronger the effect of the covariate is. In this case, Subject is the largest explanatory factor, explaining 2% of the variation.
To make sure that we aren’t overfitting, 10-fold cross validation is performed. This will split the data into 10 partitions, build the model on 9 of the those partitions and use the remaining partition to measure the prediction accuracy. This process is repeat 10 times, once for each round of cross-validation. The within model error (mse), R2�2 and the prediction accuracy (pred_mse) are reported for each round of cross validation. Here, the prediction accuracy is less than the within model error, suggesting that over fitting is not happening.
Next, we have a heatmap visualizing all of the coefficient p-values for each of the balances. The columns of the heatmap represent balances, and the rows of the heatmap represent covariates. The heatmap is colored by the negative log of the p-value, highlighting potentially significant p-values. A hover tool is enabled to allow for specific coefficient values and their corresponding p-values to be obtained, and zooming is enabled to allow for navigation of interesting covariates and balances.
Next are the prediction and residual plots. Here, only the top two balances are plotted, and the prediction residuals from the model are projected onto these two balances. From these plots we can see that the predicted points lie within the same region as the original communities. However, we can see that the residuals have roughly the same variance as the predictions. This is a little unsettling - but note that we can only explain 10% of the community variance, so these sorts of calculations aren’t completely unexpected.
The branch lengths in the visualized tree are also scaled by the explained sum of squares in the models. The longest branch lengths correspond to the most informative balances. This can allow us to get a high-level overview of the most important balances in the model. From this plot and the above heatmap, we can see that balance y0�0 is important. These balances not only have very small p-values (with p<0.05�<0.05) for differentiating subjects, but they also have the largest branch lengths in the tree diagram. This suggests that this partition of microbes could differentiate the CFS patients from the controls.
We can visualize these balances on a heatmap to see which groups of taxa they represent. By default, the values within the feature table are log-scaled, with the sample means centered around zero.
qiime gneiss dendrogram-heatmap \
  --i-table table.qza \
  --i-tree hierarchy.qza \
  --m-metadata-file sample-metadata.tsv \
  --m-metadata-column Subject \
  --p-color-map seismic \
  --o-visualization heatmap.qzv

*16S MAR2023
qiime gneiss dendrogram-heatmap \
  --i-table pk-sequencing-only-table.qza \
  --i-tree pk-sequencing-only-auc-gradient-hierarchy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_AUC_0_12_dose_adj_cat \
  --p-color-map seismic \
  --o-visualization pk-sequencing-only-auc-gradient-heatmap.qzv

qiime gneiss dendrogram-heatmap \
  --i-table pk-sequencing-only-table.qza \
  --i-tree pk-sequencing-only-ehr-gradient-hierarchy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_EHR_5_12_cat \
  --p-color-map seismic \
  --o-visualization pk-sequencing-only-ehr-gradient-heatmap.qzv

Output visualizations:
	•	heatmap.qzv: view | download

Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pkonly_auc_y0_taxa_summary.qzv
qiime tools view pkonly-regression_EHRCATsummary.qzv
qiime tools view pk-sequencing-only-auc-gradient-heatmap.qzv
qiime tools view pk-sequencing-only-ehr-gradient-heatmap.qzv

As noted in the legend, the numerators for each balance are highlighted in light red, while the denominators are highlighted in dark red. From here, we can see that the denominator of y0�0 has few OTUs compared to the numerator of y0�0. These taxa in the denominator could be interesting, so let’s investigate the taxonomies making up this balance with balance_taxonomy.
Specifically we’ll plot a boxplot and identify taxa that could be explaining the differences between the control and patient groups.
qiime gneiss balance-taxonomy \
  --i-table composition.qza \
  --i-tree hierarchy.qza \
  --i-taxonomy taxa.qza \
  --p-taxa-level 2 \
  --p-balance-name 'y0' \
  --m-metadata-file sample-metadata.tsv \
  --m-metadata-column Subject \
  --o-visualization y0_taxa_summary.qzv

*16S MAR2023
qiime gneiss balance-taxonomy \
  --i-table pk-sequencing-only-table.qza \
  --i-tree pk-sequencing-only-auc-gradient-hierarchy.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-taxa-level 6 \
  --p-balance-name 'y0' \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_AUC_0_12_dose_adj_cat \
  --o-visualization pk-sequencing-only-auc-gradient-y0-l6-summary.qzv

qiime gneiss balance-taxonomy \
  --i-table pk-sequencing-only-table.qza \
  --i-tree pk-sequencing-only-ehr-gradient-hierarchy.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-taxa-level 6 \
  --p-balance-name 'y0' \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --m-metadata-column MPA_EHR_5_12_cat \
  --o-visualization pk-sequencing-only-ehr-gradient-y0-l6-summary.qzv
Output visualizations:
	•	y0_taxa_summary.qzv: view | download
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pkonly_auc_y0_taxa_summary.qzv
qiime tools view pkonly-regression_EHRCATsummary.qzv

qiime tools view pk-sequencing-only-auc-gradient-y0-l6-summary.qzv
qiime tools view pk-sequencing-only-ehr-gradient-y0-l6-summary.qzv

In this particular case, the log ratio is lower in the patient group compared to the control group. In essence, this means that the taxa in the y0numerator�0��������� on average are more abundant than the taxa in y0denominator�0����������� in the healthy control group compared to the patient group.
Remember, based on the toy examples given in the beginning of this tutorial, it is not possible to infer absolute changes of microbes in a given sample. Balances will not be able to provide this sort of answer, but it can limit the number of possible scenarios. Specifically, one of the five following scenarios could have happened.
	•	The taxa in the y0numerator�0��������� on average have increased between patient group and the healthy control.
	•	The taxa in the y0denominator�0����������� on average have decreased between patient group and the healthy control.
	•	A combination of the above occurred
	•	Taxa abundances in both y0numerator�0��������� and y0denominator�0����������� both increased, but taxa abundances in y0numerator�0��������� increased more compared to y0denominator�0�����������
	•	Taxa abundances in both y0numerator�0��������� and y0denominator�0����������� both decreased, but taxa abundances in y0denominator�0����������� increased more compared to y0numerator�0���������
To further narrow down these hypothesis, biological prior knowledge or experimental validation will be required.

Yay!!! Now looking at balance analyses using gneiss

Differential abundance analysis with gneiss
https://docs.qiime2.org/2018.2/tutorials/gneiss/
https://docs.qiime2.org/2019.10/tutorials/gneiss/
Option 1: Correlation-clustering
This option should be your default option. We will employ unsupervised clustering via Ward’s hierarchical clustering to obtain Principal Balances. In essence, this will define the partitions of microbes that commonly co-occur with each other using Ward hierarchical clustering, which is defined by the following metric.
d(x,y)=V[lnxy]d(x,y)=V[ln⁡xy]
Where xx and yy represent the proportions of two microbes across all of the samples. If two microbes are highly correlated, then this quantity will shrink close to zero. Ward hierarchical cluster will then use this distance metric to iteratively cluster together groups of microbes that are correlated with each other. In the end, the tree that we obtain will highlight the high level structure and identify any blocks within in the data.
qiime gneiss correlation-clustering \
  --i-table oral-table.qza \
  --o-clustering oral-hierarchy.qza

qiime gneiss correlation-clustering \
  --i-table pk-only-table.qza \
  --o-clustering pk-only-hierarchy.qza

*Mar2023 16s Analysis;
qiime gneiss correlation-clustering \
  --i-table pk-sequencing-only-table.qza \
  --o-clustering pk-sequencing-only-hierarchy.qza

*NOV2023
MOSIO Differential abundance at M1
*Making a table for the samples for which we have both sequencing and PK data from Moataz
qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --p-where "[mosio_sequencing] = 1 " \
  --o-filtered-table m1-mosio-sequencing-only-table.qza

**Need to filter the features so there are no missing balances (https://forum.qiime2.org/t/contingency-based-filtration/9097)

qiime feature-table filter-features
--i-table table.qza
--p-min-samples 2
--o-filtered-table contingency-filtered-table.qza

qiime feature-table filter-features \
--i-table m1-mosio-sequencing-only-table.qza \
--p-min-samples 2 \
--o-filtered-table filtered-m1-mosio-sequencing-only-table.qza

qiime gneiss correlation-clustering \
  --i-table filtered-m1-mosio-sequencing-only-table.qza \
  --o-clustering m1-mosio-sequencing-only-table-hierarchy.qza

qiime taxa collapse \
  --i-table m1-mosio-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table m1-mosio-sequencing-only-table-l6.qza

qiime taxa collapse \
  --i-table m1-mosio-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 7 \
  --o-collapsed-table m1-mosio-sequencing-only-table-l7.qza

MOSIO Differential abundance at M2
*Making a table for the samples for which we have both sequencing and PK data from Moataz
qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --p-where "[mosio_sequencing] = 1 " \
  --o-filtered-table m2-mosio-sequencing-only-table.qzv

qiime feature-table filter-features \
--i-table m2-mosio-sequencing-only-table.qza \
--p-min-samples 2 \
--o-filtered-table filtered-m2-mosio-sequencing-only-table.qza

qiime gneiss correlation-clustering \
  --i-table filtered-m2-mosio-sequencing-only-table.qza \
  --o-clustering m2-mosio-sequencing-only-table-hierarchy.qza

qiime taxa collapse \
  --i-table m2-mosio-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table m2-mosio-sequencing-only-table-l6.qza

qiime taxa collapse \
  --i-table m2-mosio-sequencing-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 7 \
  --o-collapsed-table m2-mosio-sequencing-only-table-l7.qza
An important consideration for downstream analyses is the problem of overfitting. When using gradient-clustering, you are creating a tree to best highlight compositional differences along the metadata category of your choice, and it is possible to get false positives. Use gradient-clustering with caution.
Building linear models using balances
Now that we have a tree that defines our partitions, we can perform the isometric log ratio (ILR) transform. The ILR transform computes the log ratios between groups at each node in the tree.
qiime gneiss ilr-hierarchical \
  --i-table oral-table.qza \
  --i-tree oral-hierarchy.qza \
  --o-balances oral-balances.qza

qiime gneiss ilr-hierarchical \
  --i-table pk-only-table.qza \
  --i-tree pk-only-hierarchy.qza \
  --o-balances pk-only-balances.qza

*Mar2023 16s Analysis;
qiime gneiss ilr-hierarchical \
  --i-table pk-sequencing-only-table.qza \
  --i-tree pk-sequencing-only-hierarchy.qza \
  --o-balances pk-sequencing-only-balances.qza

*NOV2023

MOSIO M1
qiime gneiss ilr-hierarchical \
  --i-table m1-mosio-sequencing-only-table.qza\
  --i-tree m1-mosio-sequencing-only-table-hierarchy.qza \
  --o-balances m1-mosio-sequencing-only-table-balances.qza


MOSIO M2
qiime gneiss ilr-hierarchical \
  --i-table m2-mosio-sequencing-only-table.qza\
  --i-tree m2-mosio-sequencing-only-table-hierarchy.qza \
  --o-balances m2-mosio-sequencing-only-table-balances.qza
Now that we have the log ratios of each node of our tree, we can run linear regression on the balances. The linear regression that we will be running is called a multivariate response linear regression, which boils down to performing a linear regression on each balance separately.
We can use this to attempt to associate microbial abundances with environmental variables. Running these models has multiple advantages over standard univariate regression, as it avoids many of the issues associated with overfitting, and can allow us to gain perspective about community-wide perturbations based on environmental parameters.
Since the microbial abundances can be mapped directly to balances, we can perform the multivariate regression directly on the balances. The model that we will be building is represented as follows
y⃗ =β0→+βSubject→Xsubject→+βsex→Xsex→+βage→XAge→+βsCD14ugml→XsCD14ugml→+βLBPugml→XLBPugml→y→=β0→+βSubject→Xsubject→+βsex→Xsex→+βage→XAge→+βsCD14ugml→XsCD14ugml→+βLBPugml→XLBPugml→
Where y⃗ y→ represents the matrix of balances to be predicted, βi→βi→ represents a vector of coefficients for covariate ii and Xi→Xi→ represents the measures for covariate ii.
Remember that ANOVA is a special case of linear regression - every problem that can be solved by ANOVA can be reformulated as a linear regression. See this post for more details. So we can still answer the same sort of differential abundance questions using this technique, and we can start asking more precise questions, controlling for different potential confounding variables or even interaction effects.
qiime gneiss ols-regression \
  --p-formula "gender+age_today+Contents+Collection" \
  --i-table oral-balances.qza \
  --i-tree oral-hierarchy.qza \
  --m-metadata-file Aspirin_Oral_Map.tsv \
  --o-visualization oral-regression_summary.qzv

qiime gneiss ols-regression \
  --p-formula "AUCratio" \
  --i-table pk-only-balances.qza \
  --i-tree pk-only-hierarchy.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --o-visualization pkonly-regression_AUCratiosummary.qzv

qiime gneiss ols-regression \
  --p-formula "EHR_Cat_invitro" \
  --i-table pk-only-balances.qza \
  --i-tree pk-only-hierarchy.qza \
  --m-metadata-file MISSION_16S_samplepull_mappingtemplateV7.txt \
  --o-visualization pkonly-regression_EHRCATsummary.qzv

*16S Mar2023

qiime gneiss ols-regression \
  --p-formula "MPA_AUC_0_12_dose_adj" \
  --i-table pk-sequencing-only-balances.qza \
  --i-tree pk-sequencing-only-hierarchy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-only-regression-AUC-summary.qzv

qiime gneiss ols-regression \
  --p-formula "MPA_EHR_5_12" \
  --i-table pk-sequencing-only-balances.qza \
  --i-tree pk-sequencing-only-hierarchy.qza \
  --m-metadata-file 41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups.txt \
  --o-visualization pk-sequencing-only-regression-EHR-summary.qzv

*NOV2023

MOSIO M1

qiime gneiss ols-regression \
  --p-formula "everdiarrhea1categ" \
  --i-table m1-mosio-sequencing-only-table-balances.qza \
  --i-tree m1-mosio-sequencing-only-table-hierarchy.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --o-visualization gneiss-m1-everdiarrhea-regression-summary.qzv

qiime tools view ancom-m2-mosio-everdiarrhea-l7.qzv

**Got an error which was answered by checking the post here
https://forum.qiime2.org/t/error-when-applying-qiime-gneiss-ols-regression-on-a-covariate-with-14-categories/6023
and here https://forum.qiime2.org/t/contingency-based-filtration/9097
I will go back and used my filtered table for the gneiss analysis.

qiime gneiss ols-regression \
  --p-formula "everdiarrhea1categ" \
  --i-table m1-mosio-sequencing-only-table-balances.qza \
  --i-tree m1-mosio-sequencing-only-table-hierarchy.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --o-visualization gneiss-m1-everdiarrhea-regression-summary.qzv

qiime tools view gneiss-m1-everdiarrhea-regression-summary.qzv

qiime gneiss ols-regression \
  --p-formula "everdiarrhea1timeframe" \
  --i-table m1-mosio-sequencing-only-table-balances.qza \
  --i-tree m1-mosio-sequencing-only-table-hierarchy.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --o-visualization gneiss-m1-everdiarrheatimeframe-regression-summary.qzv

qiime tools view gneiss-m1-everdiarrheatimeframe-regression-summary.qzv

qiime gneiss ols-regression \
  --p-formula "everCTCAE21categ" \
  --i-table m1-mosio-sequencing-only-table-balances.qza \
  --i-tree m1-mosio-sequencing-only-table-hierarchy.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --o-visualization gneiss-m1-everCTCAE2-regression-summary.qzv

qiime tools view gneiss-m1-everCTCAE2-regression-summary.qzv

qiime gneiss ols-regression \
  --p-formula "everCTCAE21timeframe" \
  --i-table m1-mosio-sequencing-only-table-balances.qza \
  --i-tree m1-mosio-sequencing-only-table-hierarchy.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --o-visualization gneiss-m1-everCTCAE2timeframe-regression-summary.qzv

qiime tools view gneiss-m1-everCTCAE2timeframe-regression-summary.qzv

qiime gneiss ols-regression \
  --p-formula "maxCTCAEcateg" \
  --i-table m1-mosio-sequencing-only-table-balances.qza \
  --i-tree m1-mosio-sequencing-only-table-hierarchy.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --o-visualization gneiss-m1-maxCTCAE-regression-summary.qzv

qiime tools view gneiss-m1-maxCTCAE-regression-summary.qzv

MOSIO M2

qiime gneiss ols-regression \
  --p-formula "everdiarrhea1categ" \
  --i-table m2-mosio-sequencing-only-table-balances.qza \
  --i-tree m2-mosio-sequencing-only-table-hierarchy.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --o-visualization gneiss-m2-everdiarrhea-regression-summary.qzv

qiime tools view gneiss-m2-everdiarrhea-regression-summary.qzv

qiime gneiss ols-regression \
  --p-formula "everdiarrhea1timeframe" \
  --i-table m2-mosio-sequencing-only-table-balances.qza \
  --i-tree m2-mosio-sequencing-only-table-hierarchy.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --o-visualization gneiss-m2-everdiarrheatimeframe-regression-summary.qzv

qiime tools view gneiss-m2-everdiarrheatimeframe-regression-summary.qzv

qiime gneiss ols-regression \
  --p-formula "everCTCAE21categ" \
  --i-table m2-mosio-sequencing-only-table-balances.qza \
  --i-tree m2-mosio-sequencing-only-table-hierarchy.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --o-visualization gneiss-m2-everCTCAE2-regression-summary.qzv

qiime tools view gneiss-m2-everCTCAE2-regression-summary.qzv

qiime gneiss ols-regression \
  --p-formula "everCTCAE21timeframe" \
  --i-table m2-mosio-sequencing-only-table-balances.qza \
  --i-tree m2-mosio-sequencing-only-table-hierarchy.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --o-visualization gneiss-m2-everCTCAE2timeframe-regression-summary.qzv

qiime tools view gneiss-m2-everCTCAE2timeframe-regression-summary.qzv

qiime gneiss ols-regression \
  --p-formula "maxCTCAEcateg" \
  --i-table m2-mosio-sequencing-only-table-balances.qza \
  --i-tree m2-mosio-sequencing-only-table-hierarchy.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt\
  --o-visualization gneiss-m2-maxCTCAE-regression-summary.qzv

qiime tools view gneiss-m2-maxCTCAE-regression-summary.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view oral-regression_summary.qzv
qiime tools view pkonly-regression_AUCratiosummary.qzv
qiime tools view pkonly-regression_EHRCATsummary.qzv
qiime tools view pk-sequencing-only-regression-AUC-summary.qzv
qiime tools view pk-sequencing-only-regression-EHR-summary.qzv
There are a few things to note in the regression summary. There is an R2�2 in the summary, which gives information about how much of the variance in the community is explained by the regression model. From what we can see, the regression can explain about 10% of the community variation. This is typical for what we see in human gut microbiomes, since there is a very high amount of confounding variation due to genetics, diet, environment, etc.
To evaluate the explanatory model of a single covariate, a leave-one-variable-out approach is used. One variable is left out, and the change in R2�2 is calculated. The larger the change is, the stronger the effect of the covariate is. In this case, Subject is the largest explanatory factor, explaining 2% of the variation.
To make sure that we aren’t overfitting, 10-fold cross validation is performed. This will split the data into 10 partitions, build the model on 9 of the those partitions and use the remaining partition to measure the prediction accuracy. This process is repeat 10 times, once for each round of cross-validation. The within model error (mse), R2�2 and the prediction accuracy (pred_mse) are reported for each round of cross validation. Here, the prediction accuracy is less than the within model error, suggesting that over fitting is not happening.
Next, we have a heatmap visualizing all of the coefficient p-values for each of the balances. The columns of the heatmap represent balances, and the rows of the heatmap represent covariates. The heatmap is colored by the negative log of the p-value, highlighting potentially significant p-values. A hover tool is enabled to allow for specific coefficient values and their corresponding p-values to be obtained, and zooming is enabled to allow for navigation of interesting covariates and balances.
Next are the prediction and residual plots. Here, only the top two balances are plotted, and the prediction residuals from the model are projected onto these two balances. From these plots we can see that the predicted points lie within the same region as the original communities. However, we can see that the residuals have roughly the same variance as the predictions. This is a little unsettling - but note that we can only explain 10% of the community variance, so these sorts of calculations aren’t completely unexpected.
The branch lengths in the visualized tree are also scaled by the explained sum of squares in the models. The longest branch lengths correspond to the most informative balances. This can allow us to get a high-level overview of the most important balances in the model. From this plot and the above heatmap, we can see that balance y0�0 is important. These balances not only have very small p-values (with p<0.05�<0.05) for differentiating subjects, but they also have the largest branch lengths in the tree diagram. This suggests that this partition of microbes could differentiate the CFS patients from the controls.
We can visualize these balances on a heatmap to see which groups of taxa they represent. By default, the values within the feature table are log-scaled, with the sample means centered around zero.
qiime gneiss dendrogram-heatmap \
  --i-table table.qza \
  --i-tree hierarchy.qza \
  --m-metadata-file sample-metadata.tsv \
  --m-metadata-column Subject \
  --p-color-map seismic \
  --o-visualization heatmap.qzv

MOSIO M1 
qiime gneiss dendrogram-heatmap \
  --i-table m1-mosio-sequencing-only-table.qza \
  --i-tree m1-mosio-sequencing-only-table-hierarchy.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt \
  --m-metadata-column everdiarrhea1categ \
  --p-color-map seismic \
  --o-visualization dendogram-gneiss-m1-everdiarrhea-regression-summary.qzv

qiime tools view dendogram-gneiss-m1-everdiarrhea-regression-summary.qzv

qiime gneiss dendrogram-heatmap \
  --i-table m1-mosio-sequencing-only-table.qza \
  --i-tree m1-mosio-sequencing-only-table-hierarchy.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt \
  --m-metadata-column everCTCAE21categ \
  --p-color-map seismic \
  --o-visualization dendogram-gneiss-m1-everCTCAE2-regression-summary.qzv

qiime tools view dendogram-gneiss-m1-everCTCAE2-regression-summary.qzv


MOSIO M2

qiime gneiss dendrogram-heatmap \
  --i-table m2-mosio-sequencing-only-table.qza \
  --i-tree m2-mosio-sequencing-only-table-hierarchy.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt \
  --m-metadata-column everdiarrhea1categ \
  --p-color-map seismic \
  --o-visualization dendogram-gneiss-m2-everdiarrhea-regression-summary.qzv

qiime tools view dendogram-gneiss-m2-everdiarrhea-regression-summary.qzv

qiime gneiss dendrogram-heatmap \
  --i-table m2-mosio-sequencing-only-table.qza \
  --i-tree m2-mosio-sequencing-only-table-hierarchy.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt \
  --m-metadata-column everCTCAE21categ \
  --p-color-map seismic \
  --o-visualization dendogram-gneiss-m2-everCTCAE2-regression-summary.qzv

qiime tools view dendogram-gneiss-m2-everCTCAE2-regression-summary.qzv

As noted in the legend, the numerators for each balance are highlighted in light red, while the denominators are highlighted in dark red. From here, we can see that the denominator of y0�0 has few OTUs compared to the numerator of y0�0. These taxa in the denominator could be interesting, so let’s investigate the taxonomies making up this balance with balance_taxonomy.
Specifically we’ll plot a boxplot and identify taxa that could be explaining the differences between the control and patient groups.
qiime gneiss balance-taxonomy \
  --i-table table.qza \
  --i-tree hierarchy.qza \
  --i-taxonomy taxa.qza \
  --p-taxa-level 2 \
  --p-balance-name 'y0' \
  --m-metadata-file sample-metadata.tsv \
  --m-metadata-column Subject \
  --o-visualization y0_taxa_summary.qzv

MOSIO at M1 (you do not want to collapse the table after all, or the balance taxonomy will not work since it uss the full taxonomy table)
qiime gneiss balance-taxonomy \
  --i-table m1-mosio-sequencing-only-table.qza \
  --i-tree m1-mosio-sequencing-only-table-hierarchy.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-taxa-level 5 \
  --p-balance-name 'y3' \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt \
  --m-metadata-column everdiarrhea1categ \
  --o-visualization y3-taxa-dendogram-gneiss-m1-everdiarrhea-genus-regression-summary.qzv

qiime tools view y3-taxa-dendogram-gneiss-m1-everdiarrhea-genus-regression-summary.qzv

qiime gneiss balance-taxonomy \
  --i-table m1-mosio-sequencing-only-table.qza \
  --i-tree m1-mosio-sequencing-only-table-hierarchy.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-taxa-level 5 \
  --p-balance-name 'y8' \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt \
  --m-metadata-column everCTCAE21categ \
  --o-visualization y8-taxa-dendogram-gneiss-m1-everCTCAE2-genus-regression-summary.qzv

qiime tools view y8-taxa-dendogram-gneiss-m1-everCTCAE2-genus-regression-summary.qzv


MOSIO at M2 (you do not want to collapse the table after all, or the balance taxonomy will not work since it uss the full taxonomy table)
qiime gneiss balance-taxonomy \
  --i-table m2-mosio-sequencing-only-table.qza \
  --i-tree m2-mosio-sequencing-only-table-hierarchy.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-taxa-level 5 \
  --p-balance-name 'y3' \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt \
  --m-metadata-column everdiarrhea1categ \
  --o-visualization y3-taxa-dendogram-gneiss-m2-everdiarrhea-genus-regression-summary.qzv
	
qiime tools view y3-taxa-dendogram-gneiss-m2-everdiarrhea-genus-regression-summary.qzv


qiime gneiss balance-taxonomy \
  --i-table m2-mosio-sequencing-only-table.qza \
  --i-tree m2-mosio-sequencing-only-table-hierarchy.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-taxa-level 5 \
  --p-balance-name 'y12' \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M2_PS.txt \
  --m-metadata-column everCTCAE21categ \
  --o-visualization y12-taxa-dendogram-gneiss-m2-everCTCAE2-genus-regression-summary.qzv

qiime tools view y12-taxa-dendogram-gneiss-m2-everCTCAE2-genus-regression-summary.qzv



***Now retesting the balance approach using the post intervention data only
Option 1: Correlation-clustering
This option should be your default option. We will employ unsupervised clustering via Ward’s hierarchical clustering to obtain Principal Balances. In essence, this will define the partitions of microbes that commonly co-occur with each other using Ward hierarchical clustering, which is defined by the following metric.
d(x,y)=V[lnxy]d(x,y)=V[ln⁡xy]
Where xx and yy represent the proportions of two microbes across all of the samples. If two microbes are highly correlated, then this quantity will shrink close to zero. Ward hierarchical cluster will then use this distance metric to iteratively cluster together groups of microbes that are correlated with each other. In the end, the tree that we obtain will highlight the high level structure and identify any blocks within in the data.
qiime gneiss correlation-clustering \
  --i-table oral-post-table.qza \
  --o-clustering oral-post-hierarchy.qza
An important consideration for downstream analyses is the problem of overfitting. When using gradient-clustering, you are creating a tree to best highlight compositional differences along the metadata category of your choice, and it is possible to get false positives. Use gradient-clustering with caution.
Building linear models using balances
Now that we have a tree that defines our partitions, we can perform the isometric log ratio (ILR) transform. The ILR transform computes the log ratios between groups at each node in the tree.
qiime gneiss ilr-hierarchical \
  --i-table oral-post-table.qza \
  --i-tree oral-post-hierarchy.qza \
  --o-balances oral-post-balances.qza
Now that we have the log ratios of each node of our tree, we can run linear regression on the balances. The linear regression that we will be running is called a multivariate response linear regression, which boils down to performing a linear regression on each balance separately.
We can use this to attempt to associate microbial abundances with environmental variables. Running these models has multiple advantages over standard univariate regression, as it avoids many of the issues associated with overfitting, and can allow us to gain perspective about community-wide perturbations based on environmental parameters.
Since the microbial abundances can be mapped directly to balances, we can perform the multivariate regression directly on the balances. The model that we will be building is represented as follows
y⃗ =β0→+βSubject→Xsubject→+βsex→Xsex→+βage→XAge→+βsCD14ugml→XsCD14ugml→+βLBPugml→XLBPugml→y→=β0→+βSubject→Xsubject→+βsex→Xsex→+βage→XAge→+βsCD14ugml→XsCD14ugml→+βLBPugml→XLBPugml→
Where y⃗ y→ represents the matrix of balances to be predicted, βi→βi→ represents a vector of coefficients for covariate ii and Xi→Xi→ represents the measures for covariate ii.
Remember that ANOVA is a special case of linear regression - every problem that can be solved by ANOVA can be reformulated as a linear regression. See this post for more details. So we can still answer the same sort of differential abundance questions using this technique, and we can start asking more precise questions, controlling for different potential confounding variables or even interaction effects.
qiime gneiss ols-regression \
  --p-formula "gender+age_today+Contents" \
  --i-table oral-post-balances.qza \
  --i-tree oral-post-hierarchy.qza \
  --m-metadata-file Aspirin_Oral_Map.tsv \
  --o-visualization oral-post-regression_summary.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view oral-post-regression_summary.qzv

Looking at a crude model

qiime gneiss ols-regression \
  --p-formula "Contents" \
  --i-table oral-post-balances.qza \
  --i-tree oral-post-hierarchy.qza \
  --m-metadata-file Aspirin_Oral_Map.tsv \
  --o-visualization oral-post-regression-crude_summary.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view oral-post-regression-crude_summary.qzv


Testing longitudinal analyses on the asmic oral full dataset
Non-parametric microbial interdependence test (NMIT)
Within microbial communities, microbial populations do not exist in isolation but instead form complex ecological interaction webs. Whether these interdependence networks display the same temporal characteristics within subjects from the same group may indicate divergent temporal trajectories. NMIT evaluates how interdependencies of features (e.g., microbial taxa, sequence variants, or OTUs) within a community might differ over time between sample groups. NMIT performs a nonparametric microbial interdependence test to determine longitudinal sample similarity as a function of temporal microbial composition. For each subject, NMIT computes pairwise correlations between each pair of features. Between-subject distances are then computed based on a distance norm between each subject’s microbial interdependence correlation matrix. For more details and citation, please see Zhang et al., 2017.
Note
NMIT, as with most longitudinal methods, largely depends on the quality of the input data. This method will only work for longitudinal data (i.e., the same subjects are sampled repeatedly over time). To make the method robust, we suggest a minimum of 5-6 samples (time points) per subject, but the more the merrier. NMIT does not require that samples are collected at identical time points (and hence is robust to missing samples) but this may impact data quality if highly undersampled subjects are included, or if subjects’ sampling times do not overlap in biologically meaningful ways. It is up to the users to ensure that their data are high quality and the methods are used in a biologically relevant fashion.
Note
NMIT can take a long time to run on very large feature tables. Removing low-abundance features and collapsing feature tables on taxonomy (e.g., to genus level) will improve runtime.
First let’s download a feature table to test. Here we will test genus-level taxa that exhibit a relative abundance > 0.1% in more than 15% of the total samples.
Now we are ready run NMIT. The output of this command is a distance matrix that we can pass to other QIIME2 commands for significance testing and visualization.
qiime longitudinal nmit \
  --i-table oral-table.qza \
  --m-metadata-file Aspirin_Oral_Map.tsv \
  --p-individual-id-column Subject_ID \
  --p-corr-method pearson \
  --o-distance-matrix oral-nmit-dm.qza
**For this test, we need to use a FeatureTable[RelativeeFrequency] (https://docs.qiime2.org/2018.6/plugins/available/feature-table/relative-frequency/)

qiime feature-table relative-frequency \
  --i-table oral-table.qza \
  --o-relative-frequency-table oral-rel-table.qza
qiime longitudinal nmit \
  --i-table oral-rel-table.qza \
  --m-metadata-file Aspirin_Oral_Map.tsv \
  --p-individual-id-column Subject_ID \
  --p-corr-method pearson \
  --o-distance-matrix oral-nmit-dm.qza
Now let’s put that distance matrix to work. First we will perform PERMANOVA tests to evaluate whether between-group distances are larger than within-group distance.
Note
NMIT computes between-subject distances across all time points, so each subject (as defined the --p-individual-id-column parameter used above) gets compressed into a single “sample” representing that subject’s longitudinal microbial interdependence. This new “sample” will be labeled with the SampleID of one of the subjects with a matching individual-id; this is done for the convenience of passing this distance matrix to downstream steps without needing to generate a new sample metadata file but it means that you must pay attention. For significance testing and visualization, only use group columns that are uniform across each individual-id. DO NOT ATTEMPT TO USE METADATA COLUMNS THAT VARY OVER TIME OR BAD THINGS WILL HAPPEN. For example, in the tutorial metadata a patient is labeled antiexposedall==y only after antibiotics have been used; this is a column that you should not use, as it varies over time. Now have fun and be responsible.
qiime diversity beta-group-significance \
  --i-distance-matrix oral-nmit-dm.qza \
  --m-metadata-file Aspirin_Oral_Map.tsv \
  --m-metadata-column Contents \
  --o-visualization oral-nmit.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view oral-nmit.qzv

Songbird QIIME2 Notes

In a typical  songbird pipeline, we use the following setting

Deblur can be used for denoising (DADA2 works as well)
SEPP is used for phylogeny (I have used a different method for the asmic oral data but it ran without a problem)
HOMD is used for taxonomic assignment of the oral microbiome (I used greengenes but Lisa Marotz (lisa.roth5@gmail.com) mentioned that she would be happy to share the code (and hopefully the qza file) in the future.
Reference frame and balance tree theory
Differential abundance relies on the assumption that the biomass between two samples is similar (in that case, you can use the relative abundance to approximate absolute abundance without being limited by library size or number of reads per samples)
However, due to each sample having different biomass, relative abundance does not match the true proportion as it is based on a subsample, but a ratio of 2 relative abundance is representative of the true proportion. Songbird is the qiime2 plugin used to implement this approach
We use a feature by sample table (can check using qiime tools type) as an input
Then, we use a multinomial regression models (which handles zeros in the ASV or OTU files) and a formula to compute ranked coefficients on the log scale for each taxa, relative to every other taxa in the sample based on a covariate (can be continuous or categorical) 
Once we have computed the ranked coefficient of the log scale (which are only meaningful relative to other taxa in the sample) we can make ratios of taxa using qurro and see if the ratios differ based on a covariate. For example, you can compute the ranked log scale coefficients for your taxa based on disease status in songbird (aka the formula for the multinomial model will have the disease status variable) > then, you can import this into qurro and compute ratios of interest, then test those ratios to see if they are statistically different in the health vs disease group, or any other covariate. 















I have the email conversation about songbird interpretation below

Ryan Demmer

Thanks, Lisa - you're a rock star! 

In some post-tutorial discussion one question I forgot to ask was about the type III SS p-values that the multinomial regression provides - this would be helpful to simply address the null hypothesis test of differential abundance by phenotype. Is that information included in the tsv that gets exported?

Lisa Marotz

No prob - thanks for your patience when I couldn't get zoom to work!!

Are you referring the Pseudo Q^2 between two models? This is provided in the `qiime songbird summarize-paired` output visualization. 
Unfortunately we don't have a way to get that printed out into a .tsv for many different covariates(phenotypes) so it is a bit tedious right now. 
Or maybe I'm not understanding your question correctly?

Ryan Demmer

Tech problems are a modern requirement of meetings!
I’m referring to the column coefficients for variables in any given model....

Lisa Marotz

Yes the differentials output is a list of each feature with the corresponding coefficient for each covariate in your model, not p-values. Can you restate the question I'm not sure I understand it? I'm also looping in Jamie Morton to the chain who might be able to help clarify :)

Ryan Demmer

In a regression framework there should be a coefficient and pvalue for each independent variable in your model. I’m assuming the meta data variables are independent variables so there should be pvalues too. Balance trees analysis has this structure. 

Jamie Morton

In a regression framework there should be a coefficient and pvalue for each independent variable in your model.

There are many different ways to dice up a model - you can look to see if the model as a whole performs better than random, if a covariate is significant, or if a microbe is significantly different for a particular covariate. The first two are likely tractable (we're still working on that) , but the last one is definitely not. That was one of the main messages of our reference frames paper - due to scale invariance of relative abundances, it is not possible to construct a null model for absolute abundance differences. Without a null model - per-microbe pvalues are not useful. 

I’m assuming the meta data variables are independent variables so there should be pvalues too. Balance trees analysis has this structure. 

Balances are very different - you are looking at log ratios rather than individual microbes.  You can transform the multinomial coefficients to get balances (that's in our methods section). 

Ryan Demmer

Thanks Jamie,
I'll need to review again but my understanding is that you're using multinomial regressions with microbes as dependent variables and metadata (e.g., phenotype) as independent variables. This will yield a regression coefficient describing how the independent variable relates to the dependent variable. Additionally, my understanding is that the ratios of microbe A to microbe B (reference frames) do not come into play until after rankings are formed from the multinomial regressions....
If we take figure 4 from the reference frames paper, there must be some hypothesis test (and p-value or CI) that tells you if the Y axis values for each microbe are non-zero (i.e., are they associated with the metadata variables of interest). 

Jamie Morton

There does exist a hypothesis test - the most commonly used one is the Wald test (used by DESeq2 and other tools).

The problem is that it is wrong.  

The reasoning behind this is because of scale invariance.

If the abundances are scale-invariant, and we are using a log-linear model to perform inference -- that means the parameters learned by the log linear model are shift invariant (we just turned scaling operation into a shifting operation through a logarithm).
If your parameters are shift invariant - the concept of zero does not exist anymore.  This is because if you cannot measure the absolute abundance, you can never determine if a species as changed or not.
As a result - all of the multinomial coefficients have an unknown bias (due to the lack of absolute abundance measurements).

This isn't something we covered in great detail in our paper, because it requires a bit of group theory to understand this - something that is a bit outside the scope of a traditional microbiome audience.


Songbird QIIME2 tutorial

https://github.com/biocore/songbird
2. Using Songbird through QIIME 2
Activate the conda environment
Now that you have a QIIME 2 environment, activate it using the environment’s name:
source activate qiime2-2019.10
Installation
First, you'll need to make sure that QIIME 2 (version 2019.7 or later) is installed before installing Songbird. In your QIIME 2 conda environment, you can install Songbird by running:
conda install songbird -c conda-forge

Next, run the following command to get QIIME 2 to "recognize" Songbird:
qiime dev refresh-cache
Importing data into QIIME 2
If your data is not already in QIIME2 format, you can import your BIOM tables into QIIME 2 "Artifacts". Starting from the Songbird root folder, let's import the Red Sea example data into QIIME 2 by running:
qiime tools import \
	--input-path data/redsea/redsea.biom \
	--output-path redsea.biom.qza \
	--type FeatureTable[Frequency]

*** I was not able to find the data folder in the songbird folder (went to http://osxdaily.com/2013/01/17/access-root-directory-mac-os-x/ to find out how to find the root file for QIIME2, then looked up somgbird in mackintosh HD. So it looks like I will have to test directly on the asmic oral qiime2 folder)
Running Songbird
With your QIIME 2-formatted feature table, you can run Songbird through QIIME 2 as follows:
qiime songbird multinomial \
	--i-table redsea.biom.qza \
	--m-metadata-file data/redsea/redsea_metadata.txt \
	--p-formula "Depth+Temperature+Salinity+Oxygen+Fluorescence+Nitrate" \
	--p-epochs 10000 \
	--p-differential-prior 0.5 \
	--p-summary-interval 1 \
	--p-training-column Testing \
	--o-differentials differentials.qza \
	--o-regression-stats regression-stats.qza \
	--o-regression-biplot regression-biplot.qza

Since the files are getting to be large, I am moving the qiime2 file to my external hard drive to prevent size issues

https://discussions.apple.com/thread/3871334
cd /Volumes/ROOK_PC/qiime2/asmic_oral_test
cd /Volumes/ROOK_PC/MAC_Documents/qiime2/asmic_oral_test
Switching back the asmic oral folder, and the info is below and using the latter part where I filtered the data to the post intervention part only

With your QIIME 2-formatted feature table, you can run Songbird through QIIME 2 as follows:

qiime songbird multinomial \
	--i-table oral-post-table.qza \
	--m-metadata-file Aspirin_Oral_Map.tsv \
	--p-formula "Contents" \
	--p-differential-prior 0.5 \
	--p-summary-interval 1 \
	--o-differentials differentials.qza \
	--o-regression-stats regression-stats.qza \
	--o-regression-biplot regression-biplot.qza

*NOV2023

MOSIO Differential abundance at M1
*Making a table for the samples for which we have both sequencing and PK data from Moataz
qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt\
  --p-where "[mosio_sequencing] = 1 " \
  --o-filtered-table m1-mosio-sequencing-only-table.qza

qiime songbird multinomial \
	--i-table m1-mosio-sequencing-only-table.qza \
	  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt \
	--p-formula "everdiarrhea1categ" \
	--p-differential-prior 0.5 \
	--p-summary-interval 1 \
	--o-differentials m1everdiarrheadifferentials.qza \
	--o-regression-stats m1everdiarrhearegression-stats.qza \
	--o-regression-biplot m1everdiarrhearegression-biplot.qza


qiime songbird multinomial \
	--i-table m1-mosio-sequencing-only-table.qza \
	  --m-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt \
	--p-formula "everCTCAE21categ" \
	--p-differential-prior 0.5 \
	--p-summary-interval 1 \
	--o-differentials m1everCTCAE2differentials.qza \
	--o-regression-stats m1everCTCAE2regression-stats.qza \
	--o-regression-biplot m1everCTCAE2regression-biplot.qza

REQUIRED: Checking the fit of your model
You need to diagnose the fit of the model Songbird creates (e.g. to make sure it isn't "overfitting" to the data). When you run Songbird through QIIME 2, it will generate a regression-stats.qza Artifact. This can be visualized by running:
qiime songbird summarize-single \
	--i-regression-stats regression-stats.qza \
	--o-visualization regression-summary.qzv

*NOV2023
qiime songbird summarize-single \
	--i-regression-stats m1everdiarrhearegression-stats.qza \
	--o-visualization m1everdiarrhearegression-summary.qzv

qiime songbird summarize-single \
	--i-regression-stats m1everCTCAE2regression-stats.qza \
	--o-visualization m1everCTCAE2regression-summary.qzv


The resulting visualization (viewable using qiime tools view or at# view.qiime2.org) contains two plots. These plots are analogous to the two plots shown in Tensorboard's interface (the top plot shows cross-validation results, and the bottom plot shows loss information). The interpretation of these plots is the same as with the Tensorboard plots: see this section on interpreting model fitting for details on how to understand these plots.

qiime songbird summarize-single \
	--i-regression-stats regression-stats.qza \
	--o-visualization regression-summary.qzv

qiime tools view regression-summary.qzv
qiime tools view m1everdiarrhearegression-summary.qzv
qiime tools view m1everCTCAE2regression-summary.qzv

Okay! Now to install qurro

https://github.com/biocore/qurro

https://library.qiime2.org/plugins/qurro/22/

https://nbviewer.org/github/biocore/qurro/blob/master/example_notebooks/moving_pictures/moving_pictures.ipynb


install guide:
Run the following to install Qurro via pip. Note that Qurro only supports QIIME 2 versions of 2019.7 or later.
pip install qurro
If you're not in a QIIME 2 conda environment, you may need to also install NumPy before running the above command. (But if you're in a QIIME 2 environment, you should be good.)
Note: Plans for making Qurro installable via conda are in development! Stay tuned.

qiime qurro differential-plot \
  --i-ranks differentials.qza \
  --i-table oral-post-table.qza \
  --m-sample-metadata-file Aspirin_Oral_Map.tsv \
  --m-feature-metadata-file taxonomy.qza \
  --o-visualization qurro.qzv

qiime tools view qurro.qzv

*NOV2023

qiime qurro differential-plot \
  --i-ranks m1everdiarrheadifferentials.qza \
  --i-table m1-mosio-sequencing-only-table.qza \
  --m-sample-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt \
  --m-feature-metadata-file hhri16s-taxonomy.qza \
  --o-visualization m1everdiarrheaqurro.qzv

qiime qurro differential-plot \
  --i-ranks m1everCTCAE2differentials.qza \
  --i-table m1-mosio-sequencing-only-table.qza \
  --m-sample-metadata-file 49_MAP_MISSION_16S_NOV2023_nodups_shannon_taxonomy_V7_M1_PS.txt \
  --m-feature-metadata-file hhri16s-taxonomy.qza \
  --o-visualization m1everCTCAE2qurro.qzv

qiime tools view qurro.qzv
qiime tools view m1everdiarrheaqurro.qzv
qiime tools view m1everCTCAE2qurro.qzv


**After generating the data in QURRO (see https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9021254/), export the balance of interest as a TSV, and modify it (mainly rename the id to sample_id for SAS) before saving it as a CSV for import in SAS




***Running this analysis on the full dataset rather than the post intervention only to test with LME in R	

With your QIIME 2-formatted feature table, you can run Songbird through QIIME 2 as follows:

qiime songbird multinomial \
	--i-table oral-table.qza \
	--m-metadata-file Aspirin_Oral_Map.tsv \
	--p-formula "Contents" \
	--p-differential-prior 0.5 \
	--p-summary-interval 1 \
	--o-differentials differentials_full.qza \
	--o-regression-stats regression-stats_full.qza \
	--o-regression-biplot regression-biplot_full.qza
REQUIRED: Checking the fit of your model
You need to diagnose the fit of the model Songbird creates (e.g. to make sure it isn't "overfitting" to the data). When you run Songbird through QIIME 2, it will generate a regression-stats.qza Artifact. This can be visualized by running:
qiime songbird summarize-single \
	--i-regression-stats regression-stats_full.qza \
	--o-visualization regression-summary_full.qzv

The resulting visualization (viewable using qiime tools view or at# view.qiime2.org) contains two plots. These plots are analogous to the two plots shown in Tensorboard's interface (the top plot shows cross-validation results, and the bottom plot shows loss information). The interpretation of these plots is the same as with the Tensorboard plots: see this section on interpreting model fitting for details on how to understand these plots.

qiime tools view regression-summary_full.qzv
Okay! Now to install qurro

https://github.com/biocore/qurro

https://library.qiime2.org/plugins/qurro/22/

install guide:
Run the following to install Qurro via pip. Note that Qurro only supports QIIME 2 versions of 2019.7 or later.
pip install qurro
If you're not in a QIIME 2 conda environment, you may need to also install NumPy before running the above command. (But if you're in a QIIME 2 environment, you should be good.)
Note: Plans for making Qurro installable via conda are in development! Stay tuned.

qiime qurro differential-plot \
  --i-ranks differentials_full.qza \
  --i-table oral-table.qza \
  --m-sample-metadata-file Aspirin_Oral_Map.tsv \
  --m-feature-metadata-file taxonomy.qza \
  --o-visualization qurro_full.qzv

qiime tools view qurro_full.qzv


	
	
Testing longitudinal analyses on the 16S MAR2023 MISSION dataset

Useful reading

https://docs.qiime2.org/2019.10/tutorials/longitudinal/
https://forum.qiime2.org/t/individual-feature-abundance-over-time/4389/2
https://www.biorxiv.org/content/biorxiv/early/2017/11/22/223974.full.pdf
https://forum.qiime2.org/t/easy-way-to-perform-the-linear-mixed-effects-analysis-on-the-important-features-from-the-feature-volatility-analysis/15827

First, I will check for differential abundance using ANCOM. However, this should NOT be reported as the model does not account for the longitudinal nature of the samples

M2 vs M4 samples

*Collapsing at the L5, L6 and L7 levels for the M2 and M4 only samples at the family, genus, and species levels

qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4only_V2.txt \
  --p-where "[Study_arm_2] = 'Prospective' " \
  --o-filtered-table pk-sequencing-M2M4-only-table.qza

*Family level M2M4
qiime taxa collapse \
  --i-table pk-sequencing-M2M4-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 5 \
  --o-collapsed-table pk-sequencing-M2M4-only-table-l5.qza

qiime composition add-pseudocount \
  --i-table pk-sequencing-M2M4-only-table-l5.qza \
  --o-composition-table comp-pk-sequencing-M2M4-only-table-l5.qza

qiime composition ancom \
  --i-table comp-pk-sequencing-M2M4-only-table-l5.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4only_V2.txt \
  --m-metadata-column Time_point2 \
  --o-visualization ancom-pk-sequencing-M2M4-only-table-l5.qzv

*Genus level M2M4

qiime taxa collapse \
  --i-table pk-sequencing-M2M4-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table pk-sequencing-M2M4-only-table-l6.qza

qiime composition add-pseudocount \
  --i-table pk-sequencing-M2M4-only-table-l6.qza \
  --o-composition-table comp-pk-sequencing-M2M4-only-table-l6.qza

qiime composition ancom \
  --i-table comp-pk-sequencing-M2M4-only-table-l6.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4only_V2.txt \
  --m-metadata-column Time_point2 \
  --o-visualization ancom-pk-sequencing-M2M4-only-table-l6.qzv

*Species M2M4

qiime taxa collapse \
  --i-table pk-sequencing-M2M4-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 7 \
  --o-collapsed-table pk-sequencing-M2M4-only-table-l7.qza

qiime composition add-pseudocount \
  --i-table pk-sequencing-M2M4-only-table-l7.qza \
  --o-composition-table comp-pk-sequencing-M2M4-only-table-l7.qza

qiime composition ancom \
  --i-table comp-pk-sequencing-M2M4-only-table-l7.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4only_V2.txt \
  --m-metadata-column Time_point2 \
  --o-visualization ancom-pk-sequencing-M2M4-only-table-l7.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view ancom-pk-sequencing-M2M4-only-table-l5.qzv
qiime tools view ancom-pk-sequencing-M2M4-only-table-l6.qzv
qiime tools view ancom-pk-sequencing-M2M4-only-table-l7.qzv
**Since I can collapse the tables, I will extract the variable for faecalibacterium prausnitzii at L7 and use it as a predictor in a LME model
*I also need to check the file names for beta diversity in order to pull the right metric for the LME model (https://forum.qiime2.org/t/p-metric-for-beta-diversity/4415/4)
qiime metadata tabulate --help

qiime metadata tabulate \
--m-input-file pk-sequencing-M2M4-only-table-l7.qza \
  --o-visualization pk-sequencing-M2M4-only-table-l7.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M2M4-only-table-l7.qzv
*Based on downloading and checking the TSV vile from the QZV file above, the variable name I need to use is “k__Bacteria;p__Firmicutes;c__Clostridia;o__Clostridiales;f__Ruminococcaceae;g__Faecalibacterium;s__prausnitzii”

*You need to use quotation marks for the ASv variable as there are semicolumns in the variable name.
*The quotations marks are case sensitive

qiime longitudinal linear-mixed-effects \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4only_V2.txt \
   --m-metadata-file pk-sequencing-M2M4-only-table-l7.qza \
  --p-metric 'k__Bacteria;p__Firmicutes;c__Clostridia;o__Clostridiales;f__Ruminococcaceae;g__Faecalibacterium;s__prausnitzii' \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M2M4-fprausnitzii-linear-mixed-effects.qzv

Now testing LME on relative abundance rather than absolute
qiime feature-table relative-frequency \
--i-table pk-sequencing-M2M4-only-table-l7.qza \
--o-relative-frequency-table rel-pk-sequencing-M2M4-only-table-l7.qza

qiime longitudinal linear-mixed-effects \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4only_V2.txt \
   --m-metadata-file rel-pk-sequencing-M2M4-only-table-l7.qza \
  --p-metric 'k__Bacteria;p__Firmicutes;c__Clostridia;o__Clostridiales;f__Ruminococcaceae;g__Faecalibacterium;s__prausnitzii' \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization rel-pk-sequencing-M2M4-fprausnitzii-linear-mixed-effects.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M2M4-fprausnitzii-linear-mixed-effects.qzv
qiime tools view rel-pk-sequencing-M2M4-fprausnitzii-linear-mixed-effects.qzv
*In addition, also running a feature volatility analysis for F.Prausnitzii to complement the LME model (https://forum.qiime2.org/t/individual-feature-abundance-over-time/4389/2)

*You need to use quotation marks for the ASv variable as there are semicolumns in the variable name.
*The quotations marks are case sensitive

qiime longitudinal volatility \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4only_V2.txt \
   --m-metadata-file pk-sequencing-M2M4-only-table-l7.qza \
  --p-default-metric 'k__Bacteria;p__Firmicutes;c__Clostridia;o__Clostridiales;f__Ruminococcaceae;g__Faecalibacterium;s__prausnitzii' \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M2M4-fprausnitzii-volatility.qzv

qiime longitudinal volatility \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M4only_V2.txt \
   --m-metadata-file rel-pk-sequencing-M2M4-only-table-l7.qza \
  --p-default-metric 'k__Bacteria;p__Firmicutes;c__Clostridia;o__Clostridiales;f__Ruminococcaceae;g__Faecalibacterium;s__prausnitzii' \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization rel-pk-sequencing-M2M4-fprausnitzii-volatility.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M2M4-fprausnitzii-volatility.qzv
qiime tools view rel-pk-sequencing-M2M4-fprausnitzii-volatility.qzv

M2 vs M6 samples

*Collapsing at the L5, L6 and L7 levels for the M2 and M4 only samples at the family, genus, and species levels

qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M6only_V2.txt \
  --p-where "[Study_arm_2] = 'Prospective' " \
  --o-filtered-table pk-sequencing-M2M6-only-table.qza

*Family level M2M6
qiime taxa collapse \
  --i-table pk-sequencing-M2M6-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 5 \
  --o-collapsed-table pk-sequencing-M2M6-only-table-l5.qza

qiime composition add-pseudocount \
  --i-table pk-sequencing-M2M6-only-table-l5.qza \
  --o-composition-table comp-pk-sequencing-M2M6-only-table-l5.qza

qiime composition ancom \
  --i-table comp-pk-sequencing-M2M6-only-table-l5.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M6only_V2.txt \
  --m-metadata-column Time_point2 \
  --o-visualization ancom-pk-sequencing-M2M6-only-table-l5.qzv

*Genus level M2M6

qiime taxa collapse \
  --i-table pk-sequencing-M2M6-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table pk-sequencing-M2M6-only-table-l6.qza

qiime composition add-pseudocount \
  --i-table pk-sequencing-M2M6-only-table-l6.qza \
  --o-composition-table comp-pk-sequencing-M2M6-only-table-l6.qza

qiime composition ancom \
  --i-table comp-pk-sequencing-M2M6-only-table-l6.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M6only_V2.txt \
  --m-metadata-column Time_point2 \
  --o-visualization ancom-pk-sequencing-M2M6-only-table-l6.qzv

*Species M2M6

qiime taxa collapse \
  --i-table pk-sequencing-M2M6-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 7 \
  --o-collapsed-table pk-sequencing-M2M6-only-table-l7.qza

qiime composition add-pseudocount \
  --i-table pk-sequencing-M2M6-only-table-l7.qza \
  --o-composition-table comp-pk-sequencing-M2M6-only-table-l7.qza

qiime composition ancom \
  --i-table comp-pk-sequencing-M2M6-only-table-l7.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M6only_V2.txt \
  --m-metadata-column Time_point2 \
  --o-visualization ancom-pk-sequencing-M2M6-only-table-l7.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view ancom-pk-sequencing-M2M6-only-table-l5.qzv
qiime tools view ancom-pk-sequencing-M2M6-only-table-l6.qzv
qiime tools view ancom-pk-sequencing-M2M6-only-table-l7.qzv
**Since I can collapse the tables, I will extract the variable for faecalibacterium prausnitzii at L7 and use it as a predictor in a LME model
*I also need to check the file names for beta diversity in order to pull the right metric for the LME model (https://forum.qiime2.org/t/p-metric-for-beta-diversity/4415/4)
qiime metadata tabulate --help

qiime metadata tabulate \
--m-input-file pk-sequencing-M2M6-only-table-l7.qza \
  --o-visualization pk-sequencing-M2M6-only-table-l7.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M2M6-only-table-l7.qzv
*Based on downloading and checking the TSV vile from the QZV file above, the variable name I need to use is “k__Bacteria;p__Firmicutes;c__Clostridia;o__Clostridiales;f__Ruminococcaceae;g__Faecalibacterium;s__prausnitzii”

*You need to use quotation marks for the ASv variable as there are semicolumns in the variable name.
*The quotations marks are case sensitive

qiime longitudinal linear-mixed-effects \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M6only_V2.txt \
   --m-metadata-file pk-sequencing-M2M6-only-table-l7.qza \
  --p-metric 'k__Bacteria;p__Firmicutes;c__Clostridia;o__Clostridiales;f__Ruminococcaceae;g__Faecalibacterium;s__prausnitzii' \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M2M6-fprausnitzii-linear-mixed-effects.qzv

Now testing LME on relative abundance rather than absolute
qiime feature-table relative-frequency \
--i-table pk-sequencing-M2M6-only-table-l7.qza \
--o-relative-frequency-table rel-pk-sequencing-M2M6-only-table-l7.qza

qiime longitudinal linear-mixed-effects \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M6only_V2.txt \
   --m-metadata-file rel-pk-sequencing-M2M6-only-table-l7.qza \
  --p-metric 'k__Bacteria;p__Firmicutes;c__Clostridia;o__Clostridiales;f__Ruminococcaceae;g__Faecalibacterium;s__prausnitzii' \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization rel-pk-sequencing-M2M6-fprausnitzii-linear-mixed-effects.qzv

Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M2M6-fprausnitzii-linear-mixed-effects.qzv
qiime tools view rel-pk-sequencing-M2M6-fprausnitzii-linear-mixed-effects.qzv
*In addition, also running a feature volatility analysis for F.Prausnitzii to complement the LME model (https://forum.qiime2.org/t/individual-feature-abundance-over-time/4389/2)

*You need to use quotation marks for the ASv variable as there are semicolumns in the variable name.
*The quotations marks are case sensitive

qiime longitudinal volatility \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M6only_V2.txt \
   --m-metadata-file pk-sequencing-M2M6-only-table-l7.qza \
  --p-default-metric 'k__Bacteria;p__Firmicutes;c__Clostridia;o__Clostridiales;f__Ruminococcaceae;g__Faecalibacterium;s__prausnitzii' \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M2M6-fprausnitzii-volatility.qzv

qiime longitudinal volatility \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M2M6only_V2.txt \
   --m-metadata-file rel-pk-sequencing-M2M6-only-table-l7.qza \
  --p-default-metric 'k__Bacteria;p__Firmicutes;c__Clostridia;o__Clostridiales;f__Ruminococcaceae;g__Faecalibacterium;s__prausnitzii' \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization rel-pk-sequencing-M2M6-fprausnitzii-volatility.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M2M6-fprausnitzii-volatility.qzv
qiime tools view rel-pk-sequencing-M2M6-fprausnitzii-volatility.qzv

M1 vs M2 samples

*Collapsing at the L5, L6 and L7 levels for the M2 and M4 only samples at the family, genus, and species levels

qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M1M2only_V2.txt \
  --p-where "[Study_arm_2] = 'Prospective' " \
  --o-filtered-table pk-sequencing-M1M2-only-table.qza

*Family level M1M2

qiime taxa collapse \
  --i-table pk-sequencing-M1M2-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 5 \
  --o-collapsed-table pk-sequencing-M1M2-only-table-l5.qza

qiime composition add-pseudocount \
  --i-table pk-sequencing-M1M2-only-table-l5.qza \
  --o-composition-table comp-pk-sequencing-M1M2-only-table-l5.qza

qiime composition ancom \
  --i-table comp-pk-sequencing-M1M2-only-table-l5.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M1M2only_V2.txt \
  --m-metadata-column Time_point2 \
  --o-visualization ancom-pk-sequencing-M1M2-only-table-l5.qzv

*Genus level M1M2

qiime taxa collapse \
  --i-table pk-sequencing-M1M2-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table pk-sequencing-M1M2-only-table-l6.qza

qiime composition add-pseudocount \
  --i-table pk-sequencing-M1M2-only-table-l6.qza \
  --o-composition-table comp-pk-sequencing-M1M2-only-table-l6.qza

qiime composition ancom \
  --i-table comp-pk-sequencing-M1M2-only-table-l6.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M1M2only_V2.txt \
  --m-metadata-column Time_point2 \
  --o-visualization ancom-pk-sequencing-M1M2-only-table-l6.qzv

*Species M1M2

qiime taxa collapse \
  --i-table pk-sequencing-M1M2-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 7 \
  --o-collapsed-table pk-sequencing-M1M2-only-table-l7.qza

qiime composition add-pseudocount \
  --i-table pk-sequencing-M1M2-only-table-l7.qza \
  --o-composition-table comp-pk-sequencing-M1M2-only-table-l7.qza

qiime composition ancom \
  --i-table comp-pk-sequencing-M1M2-only-table-l7.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M1M2only_V2.txt \
  --m-metadata-column Time_point2 \
  --o-visualization ancom-pk-sequencing-M1M2-only-table-l7.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view ancom-pk-sequencing-M1M2-only-table-l5.qzv
qiime tools view ancom-pk-sequencing-M1M2-only-table-l6.qzv
qiime tools view ancom-pk-sequencing-M1M2-only-table-l7.qzv
**Since I can collapse the tables, I will extract the variable for faecalibacterium prausnitzii at L7 and use it as a predictor in a LME model
*I also need to check the file names for beta diversity in order to pull the right metric for the LME model (https://forum.qiime2.org/t/p-metric-for-beta-diversity/4415/4)
qiime metadata tabulate --help

qiime metadata tabulate \
--m-input-file pk-sequencing-M1M2-only-table-l7.qza \
  --o-visualization pk-sequencing-M1M2-only-table-l7.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M1M2-only-table-l7.qzv
*Based on downloading and checking the TSV vile from the QZV file above, the variable name I need to use is “k__Bacteria;p__Firmicutes;c__Clostridia;o__Clostridiales;f__Ruminococcaceae;g__Faecalibacterium;s__prausnitzii”

*You need to use quotation marks for the ASv variable as there are semicolumns in the variable name.
*The quotations marks are case sensitive

qiime longitudinal linear-mixed-effects \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M1M2only_V2.txt \
   --m-metadata-file pk-sequencing-M1M2-only-table-l7.qza \
  --p-metric 'k__Bacteria;p__Firmicutes;c__Clostridia;o__Clostridiales;f__Ruminococcaceae;g__Faecalibacterium;s__prausnitzii' \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M1M2-fprausnitzii-linear-mixed-effects.qzv

Now testing LME on relative abundance rather than absolute
qiime feature-table relative-frequency \
--i-table pk-sequencing-M1M2-only-table-l7.qza \
--o-relative-frequency-table rel-pk-sequencing-M1M2-only-table-l7.qza

qiime longitudinal linear-mixed-effects \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M1M2only_V2.txt \
   --m-metadata-file rel-pk-sequencing-M1M2-only-table-l7.qza \
  --p-metric 'k__Bacteria;p__Firmicutes;c__Clostridia;o__Clostridiales;f__Ruminococcaceae;g__Faecalibacterium;s__prausnitzii' \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization rel-pk-sequencing-M1M2-fprausnitzii-linear-mixed-effects.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M1M2-fprausnitzii-linear-mixed-effects.qzv
qiime tools view rel-pk-sequencing-M1M2-fprausnitzii-linear-mixed-effects.qzv
*In addition, also running a feature volatility analysis for F.Prausnitzii to complement the LME model (https://forum.qiime2.org/t/individual-feature-abundance-over-time/4389/2)

*You need to use quotation marks for the ASv variable as there are semicolumns in the variable name.
*The quotations marks are case sensitive

qiime longitudinal volatility \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M1M2only_V2.txt \
   --m-metadata-file pk-sequencing-M1M2-only-table-l7.qza \
  --p-default-metric 'k__Bacteria;p__Firmicutes;c__Clostridia;o__Clostridiales;f__Ruminococcaceae;g__Faecalibacterium;s__prausnitzii' \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M1M2-fprausnitzii-volatility.qzv

qiime longitudinal volatility \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M1M2only_V2.txt \
   --m-metadata-file rel-pk-sequencing-M1M2-only-table-l7.qza \
  --p-default-metric 'k__Bacteria;p__Firmicutes;c__Clostridia;o__Clostridiales;f__Ruminococcaceae;g__Faecalibacterium;s__prausnitzii' \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization rel-pk-sequencing-M1M2-fprausnitzii-volatility.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M1M2-fprausnitzii-volatility.qzv
qiime tools view rel-pk-sequencing-M1M2-fprausnitzii-volatility.qzv
M4 vs M6 samples

*Collapsing at the L5, L6 and L7 levels for the M2 and M4 only samples at the family, genus, and species levels

qiime feature-table filter-samples \
  --i-table hhri16s-table.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M4M6only_V2.txt \
  --p-where "[Study_arm_2] = 'Prospective' " \
  --o-filtered-table pk-sequencing-M4M6-only-table.qza

*Family level M4M6

qiime taxa collapse \
  --i-table pk-sequencing-M4M6-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 5 \
  --o-collapsed-table pk-sequencing-M4M6-only-table-l5.qza

qiime composition add-pseudocount \
  --i-table pk-sequencing-M4M6-only-table-l5.qza \
  --o-composition-table comp-pk-sequencing-M4M6-only-table-l5.qza

qiime composition ancom \
  --i-table comp-pk-sequencing-M4M6-only-table-l5.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M4M6only_V2.txt \
  --m-metadata-column Time_point2 \
  --o-visualization ancom-pk-sequencing-M4M6-only-table-l5.qzv

*Genus level M4M6

qiime taxa collapse \
  --i-table pk-sequencing-M4M6-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 6 \
  --o-collapsed-table pk-sequencing-M4M6-only-table-l6.qza

qiime composition add-pseudocount \
  --i-table pk-sequencing-M4M6-only-table-l6.qza \
  --o-composition-table comp-pk-sequencing-M4M6-only-table-l6.qza

qiime composition ancom \
  --i-table comp-pk-sequencing-M4M6-only-table-l6.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M4M6only_V2.txt \
  --m-metadata-column Time_point2 \
  --o-visualization ancom-pk-sequencing-M4M6-only-table-l6.qzv

*Species M4M6

qiime taxa collapse \
  --i-table pk-sequencing-M4M6-only-table.qza \
  --i-taxonomy hhri16s-taxonomy.qza \
  --p-level 7 \
  --o-collapsed-table pk-sequencing-M4M6-only-table-l7.qza

qiime composition add-pseudocount \
  --i-table pk-sequencing-M4M6-only-table-l7.qza \
  --o-composition-table comp-pk-sequencing-M4M6-only-table-l7.qza

qiime composition ancom \
  --i-table comp-pk-sequencing-M4M6-only-table-l7.qza \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M4M6only_V2.txt \
  --m-metadata-column Time_point2 \
  --o-visualization ancom-pk-sequencing-M4M6-only-table-l7.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view ancom-pk-sequencing-M4M6-only-table-l5.qzv
qiime tools view ancom-pk-sequencing-M4M6-only-table-l6.qzv
qiime tools view ancom-pk-sequencing-M4M6-only-table-l7.qzv
**Since I can collapse the tables, I will extract the variable for faecalibacterium prausnitzii at L7 and use it as a predictor in a LME model
*I also need to check the file names for beta diversity in order to pull the right metric for the LME model (https://forum.qiime2.org/t/p-metric-for-beta-diversity/4415/4)
qiime metadata tabulate --help

qiime metadata tabulate \
--m-input-file pk-sequencing-M4M6-only-table-l7.qza \
  --o-visualization pk-sequencing-M4M6-only-table-l7.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M4M6-only-table-l7.qzv
*Based on downloading and checking the TSV vile from the QZV file above, the variable name I need to use is “k__Bacteria;p__Firmicutes;c__Clostridia;o__Clostridiales;f__Ruminococcaceae;g__Faecalibacterium;s__prausnitzii”

*You need to use quotation marks for the ASv variable as there are semicolumns in the variable name.
*The quotations marks are case sensitive

qiime longitudinal linear-mixed-effects \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M4M6only_V2.txt \
   --m-metadata-file pk-sequencing-M4M6-only-table-l7.qza \
  --p-metric 'k__Bacteria;p__Firmicutes;c__Clostridia;o__Clostridiales;f__Ruminococcaceae;g__Faecalibacterium;s__prausnitzii' \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M4M6-fprausnitzii-linear-mixed-effects.qzv

Now testing LME on relative abundance rather than absolute
qiime feature-table relative-frequency \
--i-table pk-sequencing-M4M6-only-table-l7.qza \
--o-relative-frequency-table rel-pk-sequencing-M4M6-only-table-l7.qza

qiime longitudinal linear-mixed-effects \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M4M6only_V2.txt \
   --m-metadata-file rel-pk-sequencing-M4M6-only-table-l7.qza \
  --p-metric 'k__Bacteria;p__Firmicutes;c__Clostridia;o__Clostridiales;f__Ruminococcaceae;g__Faecalibacterium;s__prausnitzii' \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization rel-pk-sequencing-M4M6-fprausnitzii-linear-mixed-effects.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M4M6-fprausnitzii-linear-mixed-effects.qzv
qiime tools view rel-pk-sequencing-M4M6-fprausnitzii-linear-mixed-effects.qzv
*In addition, also running a feature volatility analysis for F.Prausnitzii to complement the LME model (https://forum.qiime2.org/t/individual-feature-abundance-over-time/4389/2)

*You need to use quotation marks for the ASv variable as there are semicolumns in the variable name.
*The quotations marks are case sensitive

qiime longitudinal volatility \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M4M6only_V2.txt \
   --m-metadata-file pk-sequencing-M4M6-only-table-l7.qza \
  --p-default-metric 'k__Bacteria;p__Firmicutes;c__Clostridia;o__Clostridiales;f__Ruminococcaceae;g__Faecalibacterium;s__prausnitzii' \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization pk-sequencing-M4M6-fprausnitzii-volatility.qzv

qiime longitudinal volatility \
  --m-metadata-file 44_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_Prospective_Longitudinal_M4M6only_V2.txt \
   --m-metadata-file rel-pk-sequencing-M4M6-only-table-l7.qza \
  --p-default-metric 'k__Bacteria;p__Firmicutes;c__Clostridia;o__Clostridiales;f__Ruminococcaceae;g__Faecalibacterium;s__prausnitzii' \
  --p-state-column Time_point \
  --p-individual-id-column Patient_ID \
  --o-visualization rel-pk-sequencing-M4M6-fprausnitzii-volatility.qzv
Note
All QIIME 2 visualizers (i.e., commands that take a --o-visualization parameter) will generate a .qzv file. You can view these files with qiime tools view. We provide the command to view this first visualization, but for the remainder of this tutorial we’ll tell you to view the resulting visualization after running a visualizer, which means that you should run qiime tools view on the .qzv file that was generated.
qiime tools view pk-sequencing-M4M6-fprausnitzii-volatility.qzv
qiime tools view rel-pk-sequencing-M4M6-fprausnitzii-volatility.qzv



*** Install Picrust2 on QIIME2 data

https://library.qiime2.org/plugins/q2-picrust2/13/

q2-picrust2
2019.7
Plugin to run the PICRUSt2 pipeline to get EC, KO, and MetaCyc pathway predictions based on 16S data. Either the default PICRUSt2 sequence placement approach or SEPP can be used to place sequences into the required reference phylogeny.

source: https://github.com/gavinmdouglas/q2-picrust2
install guide:
These instructions assume you have a conda environment named qiime2-2019.7 installed. Note that you will likely run into issues installing the plugin with qiime2-2019.10, which will be fixed soon.
	•	Activate this environment:
conda activate qiime2-2019.7
	•	Install q2-picrust2 with conda. This command will automatically install the other requirements, including PICRUSt2. Note that the plugin version for qiime2-2019.7 is specified.
conda install q2-picrust2=2019.7 -c conda-forge -c bioconda -c gavinmdouglas 
If you receive an error about the default reference files missing when you try to run the program you will need to install PICRUSt2 from source instead.

On Mac

Activate the conda environment
Now that you have a QIIME 2 environment, activate it using the environment’s name:
source activate qiime2-2019.10

