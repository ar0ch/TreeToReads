#TreeToReads

Simulation pipeline to generate next generation sequencing reads from realistic phylogenies.  
Can be used to test effects of model of evolution, rates of evolution, 
genomic distribution of mutations, and phylogenetic relatedness of samples and of reference genome 
on SNP calling and evolutionary inference.  

Inputs are a phylogeny, a genome to be used as a tip in the tree,
and a set of configuration parameters in a control file

Optional inputs can include a sequencing error model parameterized from empirical data,
and a distribution for the distances separating pairs of mutations in the genome.

Outputs are mutated genomes representing all tips in the phylogeny, 
and simulated whole genome sequencing reads representing those genomes. 
These are are useful for testing and comparison of analysis pipelines.
Mutations are currently only single nucleotide variants - no indels or rearrangements.

The code is still in draft format - but testing welcome, and will be supported via email ejmctavish, gmail.  

##Requirements:

Installed globally
-   Seq-Gen
-   Art
(can be run without Art if you want to generate mutated genomes, but not reads)

python packages
-   Dendropy


-------------------------

##To install requirements

##### Install seq-gen, software to simulate mutations (http://tree.bio.ed.ac.uk/software/seqgen/) 
on ubuntu using apt-get: 

    sudo apt-get install seq-gen

on Mac or linux (using homebrew): 

    brew install seq-gen



##### Install ART, software to generate short reads from simulated genomes (http://www.niehs.nih.gov/research/resources/software/biostatistics/art/)

on ubuntu using apt-get: 

    wget http://www.niehs.nih.gov/research/resources/assets/docs/artbinvanillaicecream031114linux64tgz.tgz
    tar -xzvf artbinvanillaicecream031114linux64tgz.tgz
add art_illumina to path (see http://askubuntu.com/questions/60218/how-to-add-a-directory-to-my-path)

on Mac or linux (using homebrew): 

    brew install art


###Install Dendropy

    pip2 dendropy


-----------------------------------------------------------
##Running the simulations (quick version):

    git clone https://github.com/snacktavish/TreeToReads.git
    cd TreeToReads
    python treetoreads.py seqsim.cfg
 
Edit config file, seqsim.cfg, to fit your data.
The script by default look for a file called 'seqsim.cfg'
or first argument can be the path to a control file with any name.

Currently only runs art_illumina and generates paired end illumina data.
Alternatively, genomes can be generated, and ART run separately using any chosen parameters.

### [Full Tutorial](https://github.com/snacktavish/TreeToReads/blob/master/docs/tutorial.md)

---------------------------------------------------------
##Expected output
The script print out the parameter values and some other useful info.
If it runs successfully it will end with
"TreeToReads completed successfully!"

The output files will be in the the output directory specified in the 
seqsim.cfg file, e.g. example_out
and will consist of:

##Key files
seqeunce names are prefixed by 'sim_'  
fasta_files   - a folder containing the simulated genomes for each tip in the tree  
fastq - folder containing folders with the names of each tip from the simulation tree  
mutsites.txt  - unordered list of the locations of mutations in the genome  

###Other files generated by analysis (mostly useless)  
analysis_configuration.cfg - a copy of the control file used for the analysis  
seqgen.out - output messages form the seq-gen software  
simtree.tre.bu - a backup copy of the tree  
simtree.tre - the tree used for simulations: reformatted and polytomies randomly resolved with 0 length branches  
analysis.sh - the bash commands run by the analysis   
These folders contain the simulated read in fastq format  
seqs_sim.txt  - an intermediate file used for generating variable sites  
SNPmatrix - a file in format SEQUENCE, BASE, POSITION describing all variable sites in the genome  
art_log - log messages from ART software  

###Docker container
TreeToReads is also available as a [Docker](https://www.docker.com/) container:

	docker pull crashfrog/treetoreads
	docker run crashfrog/treetoreads seqsim.cfg
	
to run the default example, or

	docker run -v /an/example/path:/a/container/path crashfrog/treetoreads /a/container/path/my_treetoreads_config.cfg
	
to run on real data, where ```/an/example/path/``` contains the file ```my_treetoreads_config.cfg```.

(See the [Docker manual](http://docs.docker.com/engine/reference/run/#volume-shared-filesystems) for more information about mounting host directories in the container.)


