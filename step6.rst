|CyVerse logo|_

|Home_Icon|_
`Learning Center Home <http://learning.cyverse.org/>`_


Walkthrough of DNA Subway Green Line
------------------------------------
The Green line runs within CyVerse DNA Subway and was developed leverages
powerful computing and data storage infrastructure and uses the `Stampede <https://www.tacc.utexas.edu/systems/stampede>`_
supercomputer cluster to provide a high performance analytical platform with a
simple user interface suitable for both teaching and research.

**Some things to remember about the platform**

- You must be a registered CyVerse user to use Green Line
- The Green line was designed to make RNA-Seq data analysis "simple". However,
  we ask that users thoughtfully decide what "jobs" they want to submit.
- A single Green Line project may take a week to process since HPC computing is
  subject to queues which hundreds of other jobs may be staging for. Additionally
  these systems undergo regular maintenance and are subject to periodic disruption.
- DNA Subway implements the `Tuxedo Protocol <https://www.nature.com/nprot/journal/v7/n3/fig_tab/nprot.2012.016_F2.html>`_;
  RNA-Seq is a rapidly evolving method, and we anticipate upgrades to newer more
  efficient protocols. The important concepts behind RNA-Seq are still embodied
  in the current Subway architecture.


----

*DNA Subway Green Line - Create an RNA-Seq Project to Examine Differential Expression*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**A. Create a project in Subway**

  1. Log-in to `DNA Subway <https://dnasubway.cyverse.org/>`_ - unregistered users may NOT use Green Line.

  2. Click on the Green "Next Generation Sequencing" square to start a Green Line project.

  3. For 'Select Project Type' select either Single End Reads or Paired End Reads

  4. For 'Select an Organism' select a species and genome build.

     .. tip::
         If you don't see a desired species/genome `contact us <https://dnasubway.cyverse.org/feedback.html>`_ to have it added

  5. Enter a project title, and description; click 'Continue'

**B. Upload Read Data to CyVerse Data Store**
The sequence read files used in these experiments are too large to upload using
the Subway internet interface. You must upload your files (either .fastq or .fastq.gz)
directly to the CyVerse Data Store.

  1. Upload your reads to the CyVerse Data Store using Cyberduck. See instructions:
     `CyVerse Data Store Guide <https://cyverse-data-store-guide.readthedocs-hosted.com/en/latest/step1.html>`_


----

*DNA Subway Green Line - Manage Data and Check Quality with FASTQC*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**A. Select and pair files**

  1. Click on the “Manage Data” stop: this opens a Data store window that says
     "Select your FASTQ files from the Data Store" (if you are not logged in to
     CyVerse, it will ask you to do so)
  2. Click on the folder that matches your CyVerse username and Navigate to the
     folder where your sequencing files are located.
  3. Select the sequencing files you want to analyze (either .fastq or .fastq.gz
     format).
  4. If working with paired-end reads, click the 'Pair Mode' button to toggle to
     on; check each pair of sequencing files to pair them.

**B. Check sequencing quality with FastQC**
It is important to only work with high quality data. `FastQC <http://www.bioinformatics.babraham.ac.uk/projects/fastqc/>`_ is a popular tool
for determining sequencing quality.

  1. Once files have been loaded, in the 'Manage Data' window, click the 'Run'
     link in the 'QC' column to run FastQC.
  2. One the job is complete, click the 'View' link to view repeat_results


----

*DNA Subway Green Line - Trim and Filter Reads with FastX Toolkit*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Raw reads are first "quality trimmed" (remove poor quality bases off the end(s)
of a read) and then are "quality filtered" (filter out entire poor quality reads)
prior to aligning to the genome. After trimming and filtering, FastQC is run
on the trimmed/filtered files.

  1. Click “FastX ToolKit” to open the FastX Toolkit panel for all your data.
  2. For each file, under 'Basic', Click 'Run' to filter the reads using default
     parameters or click 'Advanced' to run with desired parameters; repeat this
     process for all the FASTQ files in your dataset.
  3. Once the job completes, click the 'View' link to view a generated FastQC
     report.

     .. tip::

         - Starting at this step, DNA Subway results are labeled with a job ID
           (e.g. fx####). These jobs are available in a 'DNASubway' folder
           in the home directory of your CyVerse Data Store.
         - Starting at this step, you may see 'Basic' and 'Advanced' options
           for analyses. Clicking the 'Advanced' option allows you to set
           parameters. The Parameters shown in the 'Advanced' step are the defaults
           used in the 'Basic' option.
  4. Since you may trim reads multiple times to achieve the desired quality of data
     record the job IDs (e.g. fx####) that you wish to use in the subsequent steps.


----

*DNA Subway Green Line - Map Reads to Genome with TopHat*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
TopHat is the first component of the Tuxedo Protocol. This program aligns
individual RNA-Seq reads to a previously assembled “reference” genome using a
component program called Bowtie. TopHat then uses information from the newly
mapped reads to determine what the intron/exon boundaries are for mapped
transcripts, determining their splice sites.


**TopHat Basic**

  1. For each file, under 'Basic', Click 'Run' to begin the alignment using
     default parameters. (The reads will be aligned to the reference genome
     you selected when you created your project)

  2. Repeat this process for all the FASTQ files in your dataset.


**TopHat Advanced**

  1. Click 'TopHat' to open the TopHat panel for all your data.

  2. Under 'Advanced' Click 'Run'.

  3. Set the parameters as desired; Click 'Submit' to begin the alignment using
     default parameters. (The reads will be aligned to the reference genome
     you selected when you created your project).

  4. Repeat this process for all the FASTQ files in your dataset.

    .. tip::
        We generally recommend selecting the 'No novel junctions' option
        unless you have very high-coverage data (e.g. >100 million reads for
        a ~3Gb genome).

When this step completed you can view the summary mapping statistics, or view
the aligned reads using the Integrated Genome Viewer (IGV).

----

*DNA Subway Green Line - Assemble Transcripts with Cufflinks*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Cufflinks assembles or “links” the RNA-Seq alignments into a set of transcripts
which are best estimates (determined by parsimony) of your sample’s actual
transcripts. In other words, Cufflinks makes hypotheses about how related reads
could be merged into transcripts. Cufflinks also makes estimates about the
relative abundance of each transcript.

  .. note::

    This step is optional, and can be skipped

**Cufflinks Basic**

  1. Click 'Cufflinks' to open the Cufflinks panel for all your data.
  2. For each file, under 'Basic', Click 'Run' to begin the assembly using
     default parameters. (The reads will be assembled using the reference
     genome you selected when you created your project).
  3. Repeat this process for all the FASTQ files in your dataset.

**Cufflinks Advanced**

  1. Click 'Cufflinks' to open the Cufflinks panel for all your data.
  2. Under 'Advanced' Click 'Run'
  3. Set the parameters as desired; Click 'Submit' to begin the assembly using
    default parameters. (The reads will be aligned to the reference genome
    you selected when you created your project).
  4. Repeat this process for all the FASTQ files in your dataset.

*DNA Subway Green Line - Examine Differential Expression with CuffDiff*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Cuffdiff uses the Cufflinks output (and/or or reference genome) to calculate
gene and transcript expression levels in one or more condition and tests them
for significant differences. Depending on how many replicates and conditions you
have, you may ultimately create several Cuffdiff jobs to test your desired
combinations.

  1. Click 'Cuffdiff' to open the Cuffdiff panel for all your data.
  2. Under 'Assign TopHat alignment files to samples and replicates' assign all
     of your samples (e.g. wild type, time point 1, control, etc.) to a
     grouping (e.g. 'Sample 1', 'Sample 2', etc.)
  3. For each sample, select from the drop-down menu the TopHat job
     (previously TopHat mapped reads) and their replicates that belong with
     that sample group. (you may need to review the TopHat job names from
     the TopHat step).
  4. Either click 'Submit' (Basic) to run with default parameters, or
     use the 'Advanced' link to adjust parameters.

For the result you wish to examine, click the graph icon to view a collection of
graphs that illustrate differences in expression between samples. You can also
view a table of the results, including expression levels and comparison for
each annotated gene.


More help and additional information
`````````````````````````````````````

..
    Short description and links to any reading materials

Search for an answer:
    `CyVerse Learning Center <http://learning.cyverse.org>`_ or
    `CyVerse Wiki <https://wiki.cyverse.org>`_

Post your question to the user forum:
    `Ask CyVerse <http://ask.iplantcollaborative.org/questions>`_

----

**Fix or improve this documentation:**

- On Github: `Repo link <https://github.com/CyVerse-learning-materials/dnasubway_guide>`_
- Send feedback: `Tutorials@CyVerse.org <Tutorials@CyVerse.org>`_

----

.. |CyVerse logo| image:: ./img/cyverse_rgb.png
    :width: 500
    :height: 100
.. _CyVerse logo: http://learning.cyverse.org/
.. |Home_Icon| image:: ./img/homeicon.png
    :width: 25
    :height: 25
.. _Home_Icon: http://learning.cyverse.org/
