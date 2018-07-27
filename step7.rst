|CyVerse logo|_

|Home_Icon|_
`Learning Center Home <http://learning.cyverse.org/>`_


Walkthrough of DNA Subway Green Line: Kallisto/Sleuth (*Beta*)
---------------------------------------------------------------
The Green line runs within CyVerse DNA Subway and leverages
powerful computing and data storage infrastructure and uses the `Stampede2 <https://www.tacc.utexas.edu/systems/stampede2>`_
supercomputer cluster to provide a high performance analytical platform with a
simple user interface suitable for both teaching and research. `Kallisto <https://pachterlab.github.io/kallisto/about>`_
is a quick, highly-efficient software for quantifying transcript abundances in
an RNA-Seq experiment. Even on a typical laptop, Kallisto can quantify 30
million reads in less than 3 minutes. Integrated into CyVerse, you can take
advantage of CyVerse DNA Subway to process your reads, do the
Kallisto quantification, and analyze reads
with the Kallisto companion software `Sleuth <https://pachterlab.github.io/sleuth/about>`_
in an R-Shiny app.

**Some things to remember about the platform**

- You must be a registered CyVerse user to use Green Line.
- The Green line was designed to make RNA-Seq data analysis "simple". However,
  we ask that users thoughtfully decide what "jobs" they want to submit.
- A single Green Line project may take a week to process since HPC computing is
  subject to queues which hundreds of other jobs may be staging for. Additionally
  these systems undergo regular maintenance and are subject to periodic disruption.

----

     .. admonition:: Sample data

       **How to use provided sample data**
      In this guide, we will use an RNA-Seq dataset (*"Sorghum drought"* );
      this data compares RNA from both drought-stressed and
      well-watered Sorghum. You can read more about the experimental conditions
      `here <https://bmcplantbiol.biomedcentral.com/articles/10.1186/s12870-016-0800-x>`_.
      Where appropriate, a note (in this orange colored backgroud) in the
      instructions will indicate which options to select to make use of this
      provided dataset.

*DNA Subway Green Line: Kallisto/Sleuth - Create an RNA-Seq Project to Examine Differential Expression*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**A. Create a project in Subway**

  1. Log-in to DNA Subway - unregistered users may NOT use Green Line.

     .. warning::

        These directions apply to a "Beta Release" of the Green Line.
        available at: `https://glkallisto.ngrok.io/ <https://glkallisto.ngrok.io/>`_
        Email dnalcadmin @ cshl.edu for questions.

  2. Click on the Green "Next Generation Sequencing" square to start a Green Line project.

  3. For 'Select Project Type' select either "Single End Reads" or "Paired End Reads".

    .. admonition:: Sample data

      *"Sorghum Drought"* dataset: select **Paired End Reads**
  4. For 'Select an Organism' select a species and genome build.

    .. admonition:: Sample data

      *"Sorghum Drought"* dataset: select **Sorghum bicolor - Ensemble 22 Sorbi1**
    .. tip::
         If you don't see a desired species/genome `contact us <https://dnasubway.cyverse.org/feedback.html>`_ to have it added

  5. Enter a project title, and description; click 'Continue'

**B. Upload Read Data to CyVerse Data Store**
The sequence read files used in these experiments are too large to upload using
the Subway internet interface. You must upload your files (either .fastq or .fastq.gz)
directly to the CyVerse Data Store.

  1. Upload your reads to the CyVerse Data Store using Cyberduck. See instructions:
     `CyVerse Data Store Guide <https://cyverse-data-store-guide.readthedocs-hosted.com/en/latest/step1.html>`_

     .. tip::
         This step is not directly connected with DNA Subway. You can use any
         data uploaded to the CyVerse Data Store.

----

*DNA Subway Green Line: Kallisto/Sleuth - Manage Data and Check Quality with FASTQC*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**A. Select and pair files**

  1. Click on the “Manage Data” step: this opens a Data store window that says
     "Select your FASTQ files from the Data Store" (if you are not logged in to
     CyVerse, it will ask you to do so).

  2. Click on the folder that matches your CyVerse username and Navigate to the
     folder where your sequencing files are located.

     .. admonition:: Sample data

       *"Sorghum Drought"* dataset: select **Sample Data**.

  3. Select the sequencing files you want to analyze (either .fastq or .fastq.gz
     format).

     .. admonition:: Sample data

       *"Sorghum Drought"* dataset: You will be presented
       with the following 12 files; check-select all of the files and click the
       :guilabel:`&Add files` button:

       - IS20351_DS_1_1.fastq.gz
       - IS20351_DS_1_2.fastq.gz
       - IS20351_DS_2_1.fastq.gz
       - IS20351_DS_2_2.fastq.gz
       - IS20351_DS_3_1.fastq.gz
       - IS20351_DS_3_2.fastq.gz
       - IS20351_WW_1_1.fastq.gz
       - IS20351_WW_1_2.fastq.gz
       - IS20351_WW_2_1.fastq.gz
       - IS20351_WW_2_2.fastq.gz
       - IS20351_WW_3_1.fastq.gz
       - IS20351_WW_3_2.fastq.gz

       The "DS" files are 3 replicates (paired-end) of the drought-stressed samples
       and the "WW" files are 3 replicates (paired-end) of the well-watered samples.

  4. If working with paired-end reads, click the 'Pair Mode' button to toggle to
     on; check each pair of sequencing files to pair them.

     .. admonition:: Sample data

       *"Sorghum Drought"* dataset: Right reads end in "_1" and left reads end in
       "_2". click the :guilabel:`&Pair Mode On` button to turn pairing on, and
       check-select each of the paired samples (e.g. IS20351_DS_1_1.fastq.gz and
       IS20351_DS_1_2.fastq.gz).

**B. Check sequencing quality with FastQC**

It is important to only work with high quality data. `FastQC <http://www.bioinformatics.babraham.ac.uk/projects/fastqc/>`_ is a popular tool
for determining sequencing quality.

     .. tip::
         This step takes place in the same **Manage data** window as the steps
         above.

  1. Once files have been loaded, in the 'Manage Data' window, click the 'Run'
     link in the 'QC' column to run FastQC.
  2. One the jobs are complete, click the 'View' link to view the results.

     .. tip::
         You can see a description and explanation of the FastQC report `here <https://cyverse-fastqc-quickstart.readthedocs-hosted.com/en/latest/#summary>`_
         on the CyVerse Learning Center and a more detailed set of explanations
         on the `FastQC website <https://www.bioinformatics.babraham.ac.uk/projects/fastqc/>`_


----

*DNA Subway Green Line: Kallisto/Sleuth - Trim and Filter Reads with FastX Toolkit*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Raw reads are first "quality trimmed" (remove poor quality bases off the end(s)
of a read) and then are "quality filtered" (filter out entire poor quality reads)
prior to aligning to the transcriptome. After trimming and filtering, FastQC is run
on the trimmed/filtered files.

  1. Click “FastX ToolKit” to open the FastX Toolkit panel for all your data.
  2. For each file, under 'Basic', Click 'Run' to filter the reads using default
     parameters or click 'Advanced' to run with desired parameters; repeat this
     process for all the FASTQ files in your dataset.

     .. admonition:: Sample data

       *"Sorghum Drought"* dataset: The quality of the reads in this dataset is
       relatively good. You can skip the FastX Toolkit step for this dataset.

     .. tip::
         The 'Basic' setting for FastX Toolkit uses the same settings as the
         defaults in the 'Advanced' run:

           - **quality_trimmer: minimum quality**: 20
           - **quality_trimmer: minimum trimmed read length**: 20
           - **quality_filter: minimum quality**: 20
           - **quality_filter: minimum quality**: 50


  3. Once the job completes, click the 'View' link to view a generated FastQC
     report.

  4. Since you may trim reads multiple times to achieve the desired quality of data
     record the job IDs (e.g. fx####) that you wish to use in the subsequent steps.


----

*DNA Subway Green Line: Kallisto/Sleuth - Quantify reads with Kallisto*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Kallisto uses a ‘hash-based’ pseudo alignment to deliver extremely fast matching
of RNA-Seq reads against the transcriptome index (which was selected when you
created your Green Line project). A Kallisto analysis must be run for each
mapping of RNA-Seq reads to the index. In this tutorial, we have 12 fastQ files
(6 pairs), so you will need to launch 6 Kallisto analyses.

  .. tip::
     You can find a detailed video series on the science behind the Kallisto
     software and pseudoalignment `here <https://www.youtube.com/playlist?list=PL-0S9LiUi0vhjynujVZw34RKmUo6vPmVd>`_.

  1. Click the "Quantification" step and enter a sample and condition name for
     each of your samples. You will typically have several replicates (at least
     3 minimum) for each sample. For your condition, our implementation of the
     Kallisto/Sleuth workflow supports **two conditions**.

    .. warning::
     When naming your samples and conditions, avoid spaces and special characters
     (e.g. !#$%^&/, etc.). Also be sure to be consistent with spelling.

    .. admonition:: Sample data

       *"Sorghum Drought"* dataset: We suggest the following names for this dataset:

         .. list-table::
           :header-rows: 1

           * - Left/Right Pair
             - Sample name
             - Condition
           * - IS20351_DS_1_1.fastq.gz IS20351_DS_1_2.fastq.gz
             - drought1
             - drought
           * - IS20351_DS_2_1.fastq.gz IS20351_DS_2_2.fastq.gz
             - drought2
             - drought
           * - IS20351_DS_3_1.fastq.gz IS20351_DS_3_2.fastq.gz
             - drought3
             - drought
           * - IS20351_WW_1_1.fastq.gz IS20351_WW_1_2.fastq.gz
             - watered1
             - watered
           * - IS20351_WW_2_1.fastq.gz IS20351_WW_2_2.fastq.gz
             - watered2
             - watered
           * - IS20351_WW_3_1.fastq.gz IS20351_WW_3_2.fastq.gz
             - watered3
             - watered

  2. After naming the samples and conditions, click the :guilabel:`&Submit` button
     to submit a job. Typically, within ~1 minute you will be provided with a
     job number. The job will be entered into the queue at the TACC Stampede
     supercomputing system. You can come back and click the Quantification stop
     to see the status of the job. The indication for the quantification stop
     will show "R" (running) while the job is running.

       .. tip::

          You can select some of the advanced option for your Kallisto job by
          clicking the "Parameters" link in the Quantification stop. See more
          about these advanced parameters in the `Kallisto manual <https://pachterlab.github.io/kallisto/manual>`_.



----

*DNA Subway Green Line: Kallisto/Sleuth- Visualize data using IGV and Sleuth*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
In the "View Results" steps you have access to alignment visualizations, data
download, and interactive visualization of your differental expression results.

  1. Click the "View results" step and choose one of the following options:

**IVG - Integrated Genome Viewer**

  1. Click the icon in the "IGV" column to view a visualization of your reads
     pseudoaligned to the reference transcriptome. You will need to click the
     :guilabel:`&Make it public` button (and possibly be re-directed to the
     CyVerse Discovery Enviornment). Afer making the data "public" which allows
     DNA Subway to access your files on the CyVerse Data Store, you must also
     select a memory size to launch this Java application. If you are not sure
     of which value to select, use the default 750MB option.

     .. warning::
        Using IGV requires Java software. Java is increasingly unsupported for
        security reasons on the internet. For more info on dealing with Java
        issues see `this page <https://dnasubway.cyverse.org/about/help.html>`_ for tips.


**Download Data - Abundance**

  1. Click the folder icon to be redirected to the CyVerse Discovery Enviornment
     (you may be required to log in). You will be directed to all outputs from
     you Kallisto analysis. You may preview them in the Discovery Enviornment or
     use the path listed to download the files using Cyberduck (see
     `Cyberduck download instructions <https://cyverse-data-store-guide.readthedocs-hosted.com/en/latest/step1.html#download-from-data-store-to-local-computer-using-cyberduck>`_).
     A tab-separated file of abundances for each sequence pair is available at
     the download link.

**Differential analysis - Shiny App**

  1. Click the "Launch Shiny App" link to launch an interactive window which
     contains data and graphics from your analysis.

     **R Shiny App Walkthrough**

     The R Shiny App allows you to explore your differential expression results
     as generated by the `Sleuth R package <https://pachterlab.github.io/sleuth/>`_.
     We will cover highlights for each menu in the app.

     **Results Menu**

     |sleuth_results_1|

    This menu is an interactive table of your results. You can choose which
    columns to display in the table using the checkboxes on the left of the screen.
    Several important values selected by default include:

    - **Target_id**: This is the name of the transcript (gene) from the selected
      reference transcriptome.
    - **qval**: This is a corrected (for multiple testing) p-value indicating the
      significance test of diffefrential abundance. Lower numbers indicate greater
      significance.
    - **b**: This is an estimate of the fold change between the conditions
    - **ext_gene**: If available, these are gene names pulled from Ensemble

        .. tip::
          Click the :guilabel:`&Download` button to download these results.

     **Bootstrap**

     |sleuth_bootstrap_1|

     This menu will display a boxplot that indicates the difference in expression
     between conditions. The boxplots themselves indicate variation between
     replicates as estimated by bootstrap sampling of the reads. A dropbox enables
     you to select any transcript. Clicking the "Show genes" will load alternative
     gene names if available.

        .. tip::
          Right-click a graph to download this and other images

     **PCA**

     |sleuth_pca_1|

     This graph displays principle components of each of the conditions/replicates.
     In general replicates of the same condition should cluter closely together.

     **Volcano Plot**

     |sleuth_volcano_1|

     This scatter plot displays all transcripts colored by significance of
     differential abundance. Hovering your mouse over a given point provides
     additional information. You may also use menu on the left of the screen
     to highlight specific genes/transcripts or previously set filters from the
     results menu.

     **Loadings**

     |sleuth_loadings_1|

     This barplot indicates which genes/transcripts explain most of the varience
     computed in the principle components analysis.

     **Heatmap**

     |sleuth_heatmap_1|

     This heatmap gives a measure of the similarity between the possible comparison
     of the samples and their replicates.

----

**Summary**: Together, Kallisto and Sleuth are quick, powerful ways to analyze RNA-Seq data.

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
.. |sleuth_results_1| image:: ./img/dna_subway/sleuth_results_1.png
    :width: 800
    :height: 400
.. |sleuth_bootstrap_1| image:: ./img/dna_subway/sleuth_bootstrap_1.png
    :width: 800
    :height: 400
.. |sleuth_pca_1| image:: ./img/dna_subway/sleuth_pca_1.png
    :width: 800
    :height: 400
.. |sleuth_volcano_1| image:: ./img/dna_subway/sleuth_volcano_1.png
    :width: 800
    :height: 400
.. |sleuth_loadings_1| image:: ./img/dna_subway/sleuth_loadings_1.png
    :width: 800
    :height: 400
.. |sleuth_heatmap_1| image:: ./img/dna_subway/sleuth_heatmap_1.png
    :width: 800
    :height: 400
