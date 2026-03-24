# nf-core/taxprofiler

## Taxonomical profiling using `nf-core/taxprofiler` pipeline

nf-core/taxprofiler is a bioinformatics best-practice analysis pipeline for taxonomic
classification and profiling of shotgun short- and long-read metagenomic data. It allows
for in-parallel taxonomic identification of reads or taxonomic abundance estimation
with multiple classification and profiling tools against multiple databases, and
produces standardised output tables for facilitating results comparison between
different tools and databases.

You can find a more exhaustive description and running instructions in here:
[nf-co.re/taxprofiler](https://nf-co.re/taxprofiler).

Here we provide with a small manual to how to prepare, for running the pipeline and
running it in the Microsoft Azure environment.

## Create `samplesheet.csv` as:

```
sample,run_accession,instrument_platform,fastq_1,fastq_2,fasta
2612,run1,ILLUMINA,2612_run1_R1.fq.gz,,
2612,run2,ILLUMINA,2612_run2_R1.fq.gz,,
2612,run3,ILLUMINA,2612_run3_R1.fq.gz,2612_run3_R2.fq.gz,
...
```

## Create a `databases.csv` sheet as:

```
tool,db_name,db_params,db_path
metaphlan,db1,,az://orange/databases/metaphlan_db
motu,db2,,az://orange/databases/db_mOTU
...
```

The databases will be store in the corresponding data lake folder called databases.
Until then you have to download and prepare the databases yourself.

Files for Metaphlan were download from:
[cmprod1.cibio.unitn.it/databases/MetaPhlAn/metaphlan_databases](http://cmprod1.cibio.unitn.it/databases/MetaPhlAn/metaphlan_databases/)

For mOTUs:
Needed to prepare the mOTUs database as follows:

```bash
conda create --name motus
conda activate motus
conda install -c bioconda motus
motus downloadDB
```

It got copied the database locally in here:
`/Users/apca/anaconda3/envs/motus/lib/python3.9/site-packages/motus/db_mOTU` and
I passed this dir to `databases.csv`.

## Example of Command line to run the pipeline outside Seqera

Using Docker as contanerizing system
Using only Metaphlan as reference database

```bash
nextflow run nf-core/taxprofiler \
   -profile az_test,docker \
   --input samplesheet.csv \
   --databases databases.csv \
   --outdir results_100bp \
   --perform_shortread_qc \
   --shortread_qc_tool adapterremoval \
   --save_analysis_ready_fastqs \
   --shortread_qc_minlength 100 \
   --perform_shortread_complexityfilter \
   --perform_shortread_hostremoval \
   --hostremoval_reference az://masldmice/host_genome/GCF_000001635.27_GRCm39_genomic.fna \
   --perform_runmerging \
   --run_metaphlan \
   --run_profile_standardisation \
   -w az://masldmice/work \
   -with-tower \
   -resume
```

# Worth noticing

- You need to add `--shortread_qc_minlength 100` to require a minimum length for read
  after quality control
- You need to add `--save_analysis_ready_fastqs` to save the qc filetered reads before
  they go into classification or profiling
- You need to add `--perform_runmerging` to merge different lanes of the same sample
- You need to add `--run_profile_standardisation` so that all metaphlan profile of
  each sample get combined in a single report (This may be changed soon, follow issue:
  [taxprofiler#494](https://github.com/nf-core/taxprofiler/issues/494))

## Seqera platform

Notice that one parameter tells the pipeline to be monitored by Seqera platform
(`-with-tower`). To do that login to Seqera platform and create a token (User tokens)
clicking on the User settings button in the upper-right corner.

Once created, copy it and export it in your terminal:

```bash
export TOWER_ACCESS_TOKEN=<your token>
```

## Needed to change vmType that in the nextflow.config:

To meet the requirement of 12 CPUs and 72 GB of memory, we used `Standard_E16s_v3`
with 16 cpus and 128GB memory

```
pools {
    auto {
        autoScale = true
        vmType = 'Standard_E16s_v3'
        }
    }
```

Also had to add the following code to skip a step that does not work well with Metaphlan:

```
process {
    withName: 'TAXPASTA_MERGE' {
        when = false
    }
}
```

## Parameters

These are the parameters of the last succesful run

<details>
<summary>All parameters of nf-core/taxprofiler 1.2.3 (JSON)</summary>

```json
{
  "shortread_complexityfilter_prinseqplusplus_mode": "entropy",
  "custom_config_base": "https://raw.githubusercontent.com/nf-core/configs/master",
  "save_analysis_ready_fastqs": true,
  "malt_generate_megansummary": false,
  "plaintext_email": false,
  "malt_mode": "BlastN",
  "diamond_save_reads": false,
  "shortread_complexityfilter_prinseqplusplus_dustscore": 0.5,
  "kaiju_taxon_rank": "species",
  "standardisation_taxpasta_format": "tsv",
  "databases": "az://seqera/raw/projectname/data/databases.csv",
  "run_kmcp": false,
  "krakenuniq_ram_chunk_size": "16G",
  "version": false,
  "ganon_report_mincount": 0,
  "run_kraken2": false,
  "publish_dir_mode": "copy",
  "input": "az://seqera/raw/projectname/data/Metagenomics2/samplesheet_metagenomics2.csv",
  "perform_shortread_hostremoval": false,
  "krakenuniq_batch_size": 20,
  "shortread_qc_tool": "adapterremoval",
  "run_metaphlan": true,
  "preprocessing_qc_tool": "fastqc",
  "motus_save_mgc_read_counts": false,
  "shortread_qc_mergepairs": false,
  "kraken2_save_readclassifications": false,
  "taxpasta_add_name": false,
  "ganon_report_maxcount": 0,
  "standardisation_motus_generatebiom": false,
  "shortread_complexityfilter_fastp_threshold": 30,
  "shortread_complexityfilter_tool": "bbduk",
  "save_preprocessed_reads": false,
  "diamond_output_format": "tsv",
  "custom_config_version": "master",
  "ganon_report_type": "reads",
  "shortread_qc_minlength": 100,
  "run_centrifuge": false,
  "shortread_qc_skipadaptertrim": false,
  "longread_qc_qualityfilter_minlength": 1000,
  "run_profile_standardisation": false,
  "perform_shortread_qc": true,
  "ganon_report_rank": "default",
  "taxpasta_add_lineage": false,
  "perform_shortread_redundancyestimation": false,
  "run_krona": false,
  "motus_remove_ncbi_ids": false,
  "malt_save_reads": false,
  "save_runmerged_reads": false,
  "outdir": "az://seqera/results/smoke_scrub_malthe/metagenomics2/",
  "pipelines_testdata_base_path": "https://raw.githubusercontent.com/nf-core/test-datasets/",
  "help": false,
  "shortread_complexityfilter_bbduk_mask": false,
  "centrifuge_save_reads": false,
  "save_hostremoval_bam": false,
  "perform_runmerging": false,
  "run_bracken": false,
  "kmcp_save_search": false,
  "shortread_complexityfilter_bbduk_windowsize": 50,
  "help_full": false,
  "monochrome_logs": false,
  "ganon_report_toppercentile": 0,
  "ganon_save_readclassifications": false,
  "max_multiqc_email_size": "25.MB",
  "shortread_qc_dedup": false,
  "save_complexityfiltered_reads": false,
  "longread_filter_tool": "nanoq",
  "validate_params": true,
  "kaiju_expand_viruses": false,
  "run_diamond": false,
  "skip_preprocessing_qc": false,
  "krakenuniq_save_reads": false,
  "perform_longread_qc": false,
  "shortread_qc_includeunmerged": false,
  "trace_report_suffix": "2025-08-29_11-33-52",
  "shortread_redundancyestimation_mode": "kmer",
  "run_ganon": false,
  "longread_adapterremoval_tool": "porechop_abi",
  "taxpasta_ignore_errors": false,
  "longread_qc_skipadaptertrim": false,
  "shortread_complexityfilter_entropy": 0.3,
  "multiqc_title": "multiQC_metagenomics2",
  "kraken2_save_minimizers": false,
  "longread_qc_qualityfilter_minquality": 7,
  "krakenuniq_save_readclassifications": false,
  "longread_qc_skipqualityfilter": false,
  "run_kaiju": false,
  "kraken2_save_reads": false,
  "motus_use_relative_abundance": false,
  "run_motus": false,
  "save_hostremoval_index": false,
  "run_krakenuniq": false,
  "show_hidden": false,
  "longread_qc_qualityfilter_targetbases": 500000000,
  "bracken_save_intermediatekraken2": false,
  "taxpasta_add_rank": false,
  "perform_shortread_complexityfilter": true,
  "taxpasta_add_ranklineage": false,
  "perform_longread_hostremoval": false,
  "save_untarred_databases": false,
  "longread_qc_qualityfilter_keeppercent": 90,
  "taxpasta_add_idlineage": false,
  "run_malt": false,
  "save_hostremoval_unmapped": false
}
```

</details>

## Resolved configuration on Seqera

Seqera give a resolved configuration of the parameters and all settings with respect
to the profile setting up the compute environment.

<details>
<summary>Resolved parameters and profile configuration of a taxprofiler 1.2.3
on Azure</summary>

```
nextflow {
   enable {
      strict = true
      configProcessNamesValidation = false
   }
}

params {
   input = 'az://seqera/raw/smoke_scrub_malthe/data/Metagenomics2/samplesheet_metagenomics2.csv'
   multiqc_config = null
   multiqc_title = 'multiQC_metagenomics2'
   multiqc_logo = null
   max_multiqc_email_size = '25.MB'
   multiqc_methods_description = null
   outdir = 'az://seqera/results/smoke_scrub_malthe/metagenomics2/'
   publish_dir_mode = 'copy'
   email = null
   email_on_fail = null
   plaintext_email = false
   monochrome_logs = false
   hook_url = null
   help = false
   help_full = false
   show_hidden = false
   version = false
   pipelines_testdata_base_path = 'https://raw.githubusercontent.com/nf-core/test-datasets/'
   trace_report_suffix = '2025-08-29_11-33-55'
   config_profile_name = null
   config_profile_description = null
   custom_config_version = 'master'
   shortread_qc_minlength = 100
   perform_shortread_complexityfilter = true
   save_analysis_ready_fastqs = true
   perform_shortread_qc = true
   run_profile_standardisation = false
   run_metaphlan = true
   shortread_qc_tool = 'adapterremoval'
   databases = 'az://seqera/raw/smoke_scrub_malthe/data/databases.csv'
   custom_config_base = 'https://raw.githubusercontent.com/nf-core/configs/master'
   config_profile_contact = null
   config_profile_url = null
   validate_params = true
   save_untarred_databases = false
   skip_preprocessing_qc = false
   preprocessing_qc_tool = 'fastqc'
   shortread_qc_skipadaptertrim = false
   shortread_qc_mergepairs = false
   shortread_qc_includeunmerged = false
   shortread_qc_adapter1 = null
   shortread_qc_adapter2 = null
   shortread_qc_adapterlist = null
   shortread_qc_dedup = false
   perform_longread_qc = false
   longread_adapterremoval_tool = 'porechop_abi'
   longread_qc_skipadaptertrim = false
   longread_qc_skipqualityfilter = false
   longread_filter_tool = 'nanoq'
   longread_qc_qualityfilter_minlength = 1000
   longread_qc_qualityfilter_keeppercent = 90
   longread_qc_qualityfilter_minquality = 7
   longread_qc_qualityfilter_targetbases = 500000000
   save_preprocessed_reads = false
   perform_shortread_redundancyestimation = false
   shortread_redundancyestimation_mode = 'kmer'
   shortread_complexityfilter_tool = 'bbduk'
   shortread_complexityfilter_entropy = 0.3
   shortread_complexityfilter_bbduk_windowsize = 50
   shortread_complexityfilter_bbduk_mask = false
   shortread_complexityfilter_prinseqplusplus_mode = 'entropy'
   shortread_complexityfilter_prinseqplusplus_dustscore = 0.5
   shortread_complexityfilter_fastp_threshold = 30
   save_complexityfiltered_reads = false
   perform_runmerging = false
   save_runmerged_reads = false
   perform_shortread_hostremoval = false
   perform_longread_hostremoval = false
   hostremoval_reference = null
   shortread_hostremoval_index = null
   longread_hostremoval_index = null
   save_hostremoval_index = false
   save_hostremoval_bam = false
   save_hostremoval_unmapped = false
   run_malt = false
   malt_mode = 'BlastN'
   malt_generate_megansummary = false
   malt_save_reads = false
   run_kraken2 = false
   kraken2_save_reads = false
   kraken2_save_readclassifications = false
   kraken2_save_minimizers = false
   run_krakenuniq = false
   krakenuniq_ram_chunk_size = '16G'
   krakenuniq_save_reads = false
   krakenuniq_save_readclassifications = false
   krakenuniq_batch_size = 20
   run_bracken = false
   bracken_save_intermediatekraken2 = false
   run_centrifuge = false
   centrifuge_save_reads = false
   run_kaiju = false
   kaiju_expand_viruses = false
   kaiju_taxon_rank = 'species'
   run_diamond = false
   diamond_output_format = 'tsv'
   diamond_save_reads = false
   run_motus = false
   motus_use_relative_abundance = false
   motus_remove_ncbi_ids = false
   motus_save_mgc_read_counts = false
   run_kmcp = false
   kmcp_save_search = false
   run_ganon = false
   ganon_report_type = 'reads'
   ganon_report_rank = 'default'
   ganon_report_toppercentile = 0
   ganon_report_mincount = 0
   ganon_report_maxcount = 0
   ganon_save_readclassifications = false
   run_krona = false
   krona_taxonomy_directory = null
   standardisation_taxpasta_format = 'tsv'
   taxpasta_taxonomy_dir = null
   taxpasta_add_name = false
   taxpasta_add_rank = false
   taxpasta_add_lineage = false
   taxpasta_add_idlineage = false
   taxpasta_add_ranklineage = false
   taxpasta_ignore_errors = false
   standardisation_motus_generatebiom = false
}

process {
   cpus = {  1    * task.attempt }
   memory = { 6.GB  * task.attempt }
   time = { 4.h   * task.attempt }
   errorStrategy = { task.exitStatus in ((130..145) + 104) ? 'retry' : 'finish' }
   maxRetries = 1
   maxErrors = '-1'
   withLabel:process_single {
      cpus = {  1                   }
      memory = {  1.GB * task.attempt }
      time = {  4.h  * task.attempt }
   }
   withLabel:process_low {
      cpus = { 2     * task.attempt }
      memory = { 12.GB * task.attempt }
      time = { 4.h   * task.attempt }
   }
   withLabel:process_medium {
      cpus = { 6     * task.attempt }
      memory = { 36.GB * task.attempt }
      time = { 8.h   * task.attempt }
   }
   withLabel:process_high {
      cpus = { 12    * task.attempt }
      memory = { 72.GB * task.attempt }
      time = { 16.h  * task.attempt }
   }
   withLabel:process_long {
      time = { 20.h  * task.attempt }
   }
   withLabel:process_high_memory {
      memory = { 200.GB * task.attempt }
   }
   withLabel:error_ignore {
      errorStrategy = 'ignore'
   }
   withLabel:error_retry {
      errorStrategy = 'retry'
      maxRetries = 2
   }
   withName:BRACKEN_BRACKEN {
      errorStrategy = 'ignore'
      tag = {"${meta.db_name}|${meta.id}"}
      ext {
         args = { "${meta.db_params}" }
         prefix = Script678F515BC912152A583104ACD4973692$_run_closure1$_closure34$_closure70@407d2a01
      }
      publishDir = [path:{ "${params.outdir}/bracken/${meta.db_name}/" }, mode:'copy', pattern:'*{.tsv,.txt}']
   }
   withName:CENTRIFUGE_KREPORT {
      errorStrategy = {task.exitStatus == 255 ? 'ignore' : 'retry'}
      tag = {"${meta.db_name}|${meta.id}"}
      ext {
         args = { "${meta.db_params}" }
         prefix = Script678F515BC912152A583104ACD4973692$_run_closure1$_closure46$_closure78@461b38ca
      }
      publishDir = [path:{ "${params.outdir}/centrifuge/${meta.db_name}/" }, mode:'copy', pattern:'*.{txt}']
   }
   withName:KRAKENTOOLS_COMBINEKREPORTS_CENTRIFUGE {
      errorStrategy = { task.exitStatus in [255,1] ? 'ignore' : 'retry' }
      ext {
         prefix = { "centrifuge_${meta.id}_combined_reports" }
      }
      publishDir = [path:{ "${params.outdir}/centrifuge/" }, mode:'copy', pattern:'*.{txt}']
   }
   withName:MEGAN_RMA2INFO_TSV {
      cpus = { 1                   }
      memory = { 6.GB * task.attempt }
      time = { 4.h  * task.attempt }
      tag = {"${meta.db_name}|${meta.id}"}
      ext {
         args = '-c2c Taxonomy'
         prefix = { "${meta.id}" }
      }
      publishDir = [path:{ "${params.outdir}/malt/${meta.db_name}/" }, mode:'copy', pattern:'*.{txt.gz,megan}']
   }
   withName:MEGAN_RMA2INFO_KRONA {
      cpus = { 1                   }
      memory = { 6.GB * task.attempt }
      time = { 4.h  * task.attempt }
      tag = {"${meta.db_name}|${meta.id}"}
      ext {
         args = { "--read2class Taxonomy" }
         prefix = { "${meta.id}_${meta.db_name}" }
      }
   }
   withName:FALCO {
      cpus = { 6                   }
      memory = { 4.GB * task.attempt }
      time = { 4.h  * task.attempt }
      ext {
         prefix = { "${meta.id}_${meta.run_accession}_raw_falco" }
      }
      publishDir = [path:{ "${params.outdir}/falco/raw" }, mode:'copy', pattern:'*.{html,txt,zip}']
   }
   publishDir = [path:{ "${params.outdir}/${task.process.tokenize(':')[-1].tokenize('_')[0].toLowerCase()}" }, mode:'copy', saveAs:{ filename -> filename.equals('versions.yml') ? null : filename }]
   withName:UNTAR {
      ext {
         prefix = { "${archive.simpleName}" }
      }
      publishDir = [path:{ "${params.outdir}/untar/databases" }, mode:'copy', enabled:false]
   }
   withName:FASTQC {
      ext {
         args = '--quiet'
         prefix = { "${meta.id}_${meta.run_accession}_raw" }
      }
      publishDir = [path:{ "${params.outdir}/fastqc/raw" }, mode:'copy', pattern:'*.{html,zip}']
   }
   withName:FASTQC_PROCESSED {
      ext {
         args = '--quiet'
         prefix = { "${meta.id}_${meta.run_accession}_processed" }
      }
      publishDir = [path:{ "${params.outdir}/fastqc/processed" }, mode:'copy', pattern:'*.{html,zip}']
   }
   withName:FALCO_PROCESSED {
      ext {
         prefix = { "${meta.id}_${meta.run_accession}_processed_falco" }
      }
      publishDir = [path:{ "${params.outdir}/falco/processed" }, mode:'copy', pattern:'*.{html,txt,zip}']
   }
   withName:FASTP_SINGLE {
      ext {
         args = '--length_required 100'
         prefix = { "${meta.id}_${meta.run_accession}" }
      }
      publishDir = [[path:{ "${params.outdir}/fastp" }, mode:'copy', pattern:'*.fastq.gz', enabled:false], [path:{ "${params.outdir}/fastp" }, mode:'copy', pattern:'*.{log,html,json}'], [path:{ "${params.outdir}/analysis_ready_fastqs" }, mode:'copy', pattern:'*.fastq.gz', enabled:true, saveAs:{ ( params.perform_runmerging == false || ( params.perform_runmerging && !meta.is_multirun ) ) && !params.perform_shortread_hostremoval && !params.perform_shortread_complexityfilter && params.perform_shortread_qc && params.save_analysis_ready_fastqs ? it : null }]]
   }
   withName:FASTP_PAIRED {
      ext {
         args = '--detect_adapter_for_pe --length_required 100'
         prefix = { "${meta.id}_${meta.run_accession}" }
      }
      publishDir = [[path:{ "${params.outdir}/fastp" }, mode:'copy', pattern:'*.fastq.gz', enabled:false], [path:{ "${params.outdir}/fastp" }, mode:'copy', pattern:'*.{log,html,json}'], [path:{ "${params.outdir}/analysis_ready_fastqs" }, mode:'copy', pattern:'*.fastp.fastq.gz', enabled:true, saveAs:{ ( params.perform_runmerging == false || ( params.perform_runmerging && !meta.is_multirun ) ) && !params.perform_shortread_hostremoval && !params.perform_shortread_complexityfilter && params.perform_shortread_qc && params.save_analysis_ready_fastqs ? it : null }]]
   }
   withName:ADAPTERREMOVAL_SINGLE {
      ext {
         args = '--minlength 100'
         prefix = { "${meta.id}_${meta.run_accession}" }
      }
      publishDir = [[path:{ "${params.outdir}/adapterremoval" }, mode:'copy', pattern:'*.fastq.gz', enabled:false], [path:{ "${params.outdir}/adapterremoval" }, mode:'copy', pattern:'*.settings'], [path:{ "${params.outdir}/analysis_ready_fastqs" }, mode:'copy', pattern:'*truncated.fastq.gz', enabled:true, saveAs:{ ( params.perform_runmerging == false || ( params.perform_runmerging && !meta.is_multirun ) ) && !params.perform_shortread_hostremoval && !params.perform_shortread_complexityfilter && params.perform_shortread_qc && params.save_analysis_ready_fastqs ? it : null }]]
   }
   withName:ADAPTERREMOVAL_PAIRED {
      ext {
         args = '--minlength 100'
         prefix = { "${meta.id}_${meta.run_accession}" }
      }
      publishDir = [[path:{ "${params.outdir}/adapterremoval" }, mode:'copy', pattern:'*.fastq.gz', enabled:false], [path:{ "${params.outdir}/adapterremoval" }, mode:'copy', pattern:'*.settings'], [path:{ "${params.outdir}/analysis_ready_fastqs" }, mode:'copy', pattern:'*{truncated.fastq,singleton.truncated}.gz', enabled:true, saveAs:{ ( params.perform_runmerging == false || ( params.perform_runmerging && !meta.is_multirun ) ) && !params.perform_shortread_hostremoval && !params.perform_shortread_complexityfilter && params.perform_shortread_qc && !params.shortread_qc_mergepairs && params.save_analysis_ready_fastqs ? it : null}]]
   }
   withName:NONPAREIL_NONPAREIL {
      ext {
         prefix = { "${meta.id}_${meta.run_accession}" }
      }
      publishDir = [path:{ "${params.outdir}/nonpareil/" }, mode:'copy', pattern:'*.np{a,c,l,o}']
   }
   withName:NONPAREIL_CURVE {
      ext {
         prefix = { "${meta.id}_${meta.run_accession}" }
      }
      publishDir = [path:{ "${params.outdir}/nonpareil/" }, mode:'copy', pattern:'*.png']
   }
   withName:NONPAREIL_SET {
      ext {
         prefix = { "nonpareil_all_samples_mqc" }
      }
      publishDir = [path:{ "${params.outdir}/nonpareil/" }, mode:'copy', pattern:'*.png']
   }
   withName:NONPAREIL_NONPAREILCURVESR {
      ext {
         prefix = { "nonpareil_all_samples" }
      }
      publishDir = [path:{ "${params.outdir}/nonpareil/" }, mode:'copy', pattern:'*.{json,csv,tsv,pdf}']
   }
   withName:CAT_FASTQ {
      ext {
         prefix = { "${meta.id}_${meta.run_accession}" }
      }
      publishDir = [[path:{ "${params.outdir}/analysis_ready_fastqs" }, mode:'copy', pattern:'*.fastq.gz', enabled:true, saveAs:{ ( params.perform_runmerging == false || ( params.perform_runmerging && !meta.is_multirun ) ) && !params.perform_shortread_hostremoval && !params.perform_shortread_complexityfilter && params.perform_shortread_qc && params.save_analysis_ready_fastqs ? it : null }]]
   }
   withName:PORECHOP_PORECHOP {
      ext {
         prefix = { "${meta.id}_${meta.run_accession}_porechop" }
      }
      publishDir = [[path:{ "${params.outdir}/porechop" }, mode:'copy', pattern:'*_porechop.fastq.gz', enabled:false], [path:{ "${params.outdir}/porechop" }, mode:'copy', pattern:'*.log'], [path:{ "${params.outdir}/analysis_ready_fastqs" }, mode:'copy', pattern:'*_porechop.fastq.gz', enabled:true, saveAs:{ ( params.perform_runmerging == false || ( params.perform_runmerging && !meta.is_multirun ) ) && !params.perform_longread_hostremoval && params.longread_qc_skipqualityfilter && !params.longread_qc_skipadaptertrim && params.perform_longread_qc && params.save_analysis_ready_fastqs ? it : null }]]
   }
   withName:PORECHOP_ABI {
      ext {
         prefix = { "${meta.id}_${meta.run_accession}_porechop_abi" }
      }
      publishDir = [[path:{ "${params.outdir}/porechop_abi" }, mode:'copy', pattern:'*_porechop_abi.fastq.gz', enabled:false], [path:{ "${params.outdir}/porechop_abi" }, mode:'copy', pattern:'*.log'], [path:{ "${params.outdir}/analysis_ready_fastqs" }, mode:'copy', pattern:'*porechop_abi.fastq.gz', enabled:true, saveAs:{ ( params.perform_runmerging == false || ( params.perform_runmerging && !meta.is_multirun ) ) && !params.perform_longread_hostremoval && params.longread_qc_skipqualityfilter && !params.longread_qc_skipadaptertrim && params.perform_longread_qc && params.save_analysis_ready_fastqs ? it : null }]]
   }
   withName:FILTLONG {
      ext {
         args = '--min_length 1000 --keep_percent 90 --target_bases 500000000'
         prefix = { "${meta.id}_${meta.run_accession}_filtered" }
      }
      publishDir = [[path:{ "${params.outdir}/filtlong" }, mode:'copy', pattern:'*.fastq.gz', enabled:false], [path:{ "${params.outdir}/filtlong" }, mode:'copy', pattern:'*.log'], [path:{ "${params.outdir}/analysis_ready_fastqs" }, mode:'copy', pattern:'*.fastq.gz', enabled:true, saveAs:{ ( params.perform_runmerging == false || ( params.perform_runmerging && !meta.is_multirun ) ) && !params.perform_longread_hostremoval && !params.longread_qc_skipqualityfilter && params.perform_longread_qc && params.save_analysis_ready_fastqs ? it : null }]]
   }
   withName:NANOQ {
      ext {
         args = '-vv --min-len 1000 --min-qual 7'
         prefix = { "${meta.id}_${meta.run_accession}_filtered" }
      }
      publishDir = [[path:{ "${params.outdir}/nanoq" }, mode:'copy', pattern:'*_filtered.fastq.gz', enabled:false], [path:{ "${params.outdir}/nanoq" }, mode:'copy', pattern:'*_filtered.stats'], [path:{ "${params.outdir}/analysis_ready_fastqs" }, mode:'copy', pattern:'*_filtered.fastq.gz', enabled:true, saveAs:{ ( params.perform_runmerging == false || ( params.perform_runmerging && !meta.is_multirun ) ) && !params.perform_longread_hostremoval && !params.longread_qc_skipqualityfilter && params.perform_longread_qc && params.save_analysis_ready_fastqs ? it : null }]]
   }
   withName:BBMAP_BBDUK {
      ext {
         args = 'entropy=0.3 entropywindow=50 entropymask=f'
         prefix = { "${meta.id}_${meta.run_accession}" }
      }
      publishDir = [[path:{ "${params.outdir}/bbduk/" }, mode:'copy', pattern:'*.{fastq.gz}', enabled:false], [path:{ "${params.outdir}/bbduk/" }, mode:'copy', pattern:'*.log'], [path:{ "${params.outdir}/analysis_ready_fastqs" }, mode:'copy', pattern:'*.fastq.gz', enabled:true, saveAs:{ ( params.perform_runmerging == false || ( params.perform_runmerging && !meta.is_multirun ) ) && !params.perform_shortread_hostremoval && params.shortread_complexityfilter_tool && params.save_analysis_ready_fastqs ? it : null }]]
   }
   withName:PRINSEQPLUSPLUS {
      ext {
         args = '-lc_entropy=0.3 -trim_qual_left=0 -trim_qual_left=0 -trim_qual_window=0 -trim_qual_step=0'
         prefix = { "${meta.id}_${meta.run_accession}" }
      }
      publishDir = [[path:{ "${params.outdir}/prinseqplusplus/" }, mode:'copy', pattern:'*{_good_out.fastq.gz,_good_out_R1.fastq.gz,_good_out_R2.fastq.gz}', enabled:false], [path:{ "${params.outdir}/prinseqplusplus/" }, mode:'copy', pattern:'*.log'], [path:{ "${params.outdir}/analysis_ready_fastqs" }, mode:'copy', pattern:'*{_good_out.fastq.gz,_good_out_R1.fastq.gz,_good_out_R2.fastq.gz}', enabled:true, saveAs:{ ( params.perform_runmerging == false || ( params.perform_runmerging && !meta.is_multirun ) ) && !params.perform_shortread_hostremoval && params.shortread_complexityfilter_tool && params.save_analysis_ready_fastqs ? it : null }]]
   }
   withName:BOWTIE2_BUILD {
      publishDir = [[path:{ "${params.outdir}/bowtie2/build" }, mode:'copy', pattern:'bowtie2', enabled:false]]
   }
   withName:BOWTIE2_ALIGN {
      ext {
         prefix = { "${meta.id}_${meta.run_accession}" }
      }
      publishDir = [[path:{ "${params.outdir}/bowtie2/align" }, mode:'copy', pattern:'*.log'], [path:{ "${params.outdir}/bowtie2/align" }, mode:'copy', pattern:'*.bam', enabled:false], [path:{ "${params.outdir}/bowtie2/align" }, mode:'copy', pattern:'*.fastq.gz', enabled:false], [path:{ "${params.outdir}/analysis_ready_fastqs" }, mode:'copy', enabled:true, pattern:'*.fastq.gz', saveAs:{ ( params.perform_runmerging == false || ( params.perform_runmerging && !meta.is_multirun ) ) && params.perform_shortread_hostremoval && params.save_analysis_ready_fastqs ? it : null }]]
   }
   withName:MINIMAP2_INDEX {
      ext {
         args = '-x map-ont'
      }
      publishDir = [path:{ "${params.outdir}/minimap2/index" }, mode:'copy', pattern:'*.mmi', enabled:false]
   }
   withName:MINIMAP2_ALIGN {
      ext {
         prefix = { "${meta.id}_${meta.run_accession}" }
      }
      publishDir = [path:{ "${params.outdir}/minimap2/align" }, mode:'copy', pattern:'*.bam', enabled:false]
   }
   withName:SAMTOOLS_VIEW {
      ext {
         args = '-f 4'
         prefix = { "${meta.id}_${meta.run_accession}.unmapped" }
      }
   }
   withName:SAMTOOLS_FASTQ {
      ext {
         prefix = { "${meta.id}_${meta.run_accession}.unmapped" }
      }
      publishDir = [[path:{ "${params.outdir}/samtools/fastq" }, mode:'copy', pattern:'*_other.fastq.gz', enabled:false], [path:{ "${params.outdir}/analysis_ready_fastqs" }, mode:'copy', pattern:'*.fq.gz', enabled:true, saveAs:{ ( params.perform_runmerging == false || ( params.perform_runmerging && !meta.is_multirun) ) && params.perform_longread_hostremoval && params.save_analysis_ready_fastqs ? it : null }]]
   }
   withName:SAMTOOLS_STATS {
      ext {
         prefix = { "${meta.id}_${meta.run_accession}" }
      }
      publishDir = [path:{ "${params.outdir}/samtools/stats" }, mode:'copy', pattern:'*stats']
   }
   withName:MERGE_RUNS {
      ext {
         prefix = { "${meta.id}" }
      }
      publishDir = [[path:{ "${params.outdir}/run_merging/" }, mode:'copy', pattern:'*.fastq.gz', enabled:false], [path:{ "${params.outdir}/analysis_ready_fastqs" }, mode:'copy', pattern:'*.fastq.gz', enabled:false]]
   }
   withName:MALT_RUN {
      tag = {"${meta.db_name}|${meta.id}"}
      ext {
         args = { "${meta.db_params} -m ${params.malt_mode}" }
         prefix = { "${meta.db_name}" }
      }
      publishDir = [path:{ "${params.outdir}/malt/${meta.db_name}/" }, mode:'copy', pattern:'*.{rma6,log,sam}']
   }
   withName:KRAKEN2_KRAKEN2 {
      tag = { "${meta.db_name}|${meta.tool}|${meta.id}" }
      ext {
         args = Script678F515BC912152A583104ACD4973692$_run_closure1$_closure32$_closure64@3881b884
         prefix = Script678F515BC912152A583104ACD4973692$_run_closure1$_closure32$_closure66@1185b0b7
      }
      publishDir = [path:{ "${params.outdir}/kraken2/${meta.db_name}/" }, mode:'copy', pattern:'*.{txt,fastq.gz}', saveAs:{  !params.bracken_save_intermediatekraken2 && meta.tool == "bracken" ? null : it  }]
   }
   withName:KRAKEN2_STANDARD_REPORT {
      tag = { "${meta.db_name}|${meta.tool}|${meta.id}" }
      ext {
         prefix = Script678F515BC912152A583104ACD4973692$_run_closure1$_closure33$_closure68@ec70725
      }
      publishDir = [path:{ "${params.outdir}/kraken2/${meta.db_name}/" }, mode:'copy', pattern:'*.report.txt']
   }
   withName:BRACKEN_COMBINEBRACKENOUTPUTS {
      ext {
         prefix = { "bracken_${meta.id}_combined_reports" }
      }
      publishDir = [path:{ "${params.outdir}/bracken/" }, mode:'copy', pattern:'*.txt']
   }
   withName:KRAKENTOOLS_COMBINEKREPORTS_KRAKEN {
      ext {
         prefix = { "kraken2_${meta.id}_combined_reports" }
      }
      publishDir = [path:{ "${params.outdir}/kraken2/" }, mode:'copy', pattern:'*.txt']
   }
   withName:KRAKENUNIQ_PRELOADEDKRAKENUNIQ {
      tag = { "${meta.db_name}|${task.index}" }
      ext {
         args = { "${meta.db_params}" }
         prefix = { "${meta.db_name}.krakenuniq" }
      }
      publishDir = [path:{ "${params.outdir}/krakenuniq/${meta.db_name}/" }, mode:'copy', pattern:'*.{txt,fastq.gz,fasta.gz}']
   }
   withName:KRAKENTOOLS_KREPORT2KRONA {
      publishDir = [enabled:false, mode:'copy', pattern:'*.txt']
   }
   withName:KRONA_CLEANUP {
      ext {
         prefix = Script678F515BC912152A583104ACD4973692$_run_closure1$_closure39$_closure72@2082e0e4
      }
      publishDir = [path:{ "${params.outdir}/krona/" }, mode:'copy', pattern:'*.{html}']
   }
   withName:KRONA_KTIMPORTTEXT {
      ext {
         prefix = { "${meta.tool}_${meta.id}" }
      }
      publishDir = [path:{ "${params.outdir}/krona/" }, mode:'copy', pattern:'*.{html}']
   }
   withName:KRONA_KTIMPORTTAXONOMY {
      ext {
         args = '-i'
         prefix = { "${meta.tool}_${meta.id}" }
      }
      publishDir = [path:{ "${params.outdir}/krona/" }, mode:'copy', pattern:'*.{html}']
   }
   withName:METAPHLAN_METAPHLAN {
      tag = {"${meta.db_name}|${meta.id}"}
      ext {
         args = { "${meta.db_params}" }
         prefix = Script678F515BC912152A583104ACD4973692$_run_closure1$_closure43$_closure74@15f229e8
      }
      publishDir = [path:{ "${params.outdir}/metaphlan/${meta.db_name}/" }, mode:'copy', pattern:'*.{biom,txt}']
   }
   withName:METAPHLAN_MERGEMETAPHLANTABLES {
      ext {
         prefix = { "metaphlan_${meta.id}_combined_reports" }
      }
      publishDir = [path:{ "${params.outdir}/metaphlan/" }, mode:'copy', pattern:'*.{txt}']
   }
   withName:CENTRIFUGE_CENTRIFUGE {
      tag = {"${meta.db_name}|${meta.id}"}
      ext {
         args = { "${meta.db_params}" }
         prefix = Script678F515BC912152A583104ACD4973692$_run_closure1$_closure45$_closure76@29ce33e9
      }
      publishDir = [path:{ "${params.outdir}/centrifuge/${meta.db_name}/" }, mode:'copy', pattern:'*.{txt,sam,tab,gz}']
   }
   withName:KAIJU_KAIJU {
      tag = {"${meta.db_name}|${meta.id}"}
      ext {
         args = { "${meta.db_params}" }
         prefix = Script678F515BC912152A583104ACD4973692$_run_closure1$_closure48$_closure80@1f86f7da
      }
      publishDir = [path:{ "${params.outdir}/kaiju/${meta.db_name}/" }, mode:'copy', pattern:'*.tsv']
   }
   withName:KAIJU_KAIJU2TABLE_SINGLE {
      tag = {"${meta.db_name}|${meta.id}"}
      ext {
         args = {[
            params.kaiju_expand_viruses ? "-e" : ""
        ].join(' ').trim() }
         prefix = Script678F515BC912152A583104ACD4973692$_run_closure1$_closure49$_closure82@28b4b10e
      }
      publishDir = [path:{ "${params.outdir}/kaiju/${meta.db_name}/" }, mode:'copy', pattern:'*.{txt}']
   }
   withName:KAIJU_KAIJU2TABLE_COMBINED {
      ext {
         prefix = { "kaiju_${meta.id}_combined_reports" }
      }
      publishDir = [path:{ "${params.outdir}/kaiju/" }, mode:'copy', pattern:'*.{txt}']
   }
   withName:KAIJU_KAIJU2KRONA {
      tag = {"${meta.db_name}|${meta.id}"}
      ext {
         args = '-v -u'
      }
      publishDir = [path:{ "${params.outdir}/kaiju/" }, enabled:false]
   }
   withName:DIAMOND_BLASTX {
      tag = {"${meta.db_name}|${meta.id}"}
      ext {
         args = { "${meta.db_params}" }
         prefix = Script678F515BC912152A583104ACD4973692$_run_closure1$_closure52$_closure84@2c0d7099
      }
      publishDir = [path:{ "${params.outdir}/diamond/${meta.db_name}/" }, mode:'copy', pattern:'*.{blast,xml,txt,daa,sam,tsv,paf,log}']
   }
   withName:MOTUS_PROFILE {
      tag = {"${meta.db_name}|${meta.id}"}
      ext {
         args = {
            [
                params.motus_remove_ncbi_ids ? "" : "-p",
                params.motus_use_relative_abundance ? "" : "-c",
                params.motus_save_mgc_read_counts ?  "-M ${task.ext.prefix}.mgc" : ""
            ].join(',').replaceAll(','," ")
            }
         prefix = Script678F515BC912152A583104ACD4973692$_run_closure1$_closure53$_closure86@74844f8a
      }
      publishDir = [path:{ "${params.outdir}/motus/${meta.db_name}/" }, mode:'copy']
   }
   withName:MOTUS_MERGE {
      ext {
         args = { params.standardisation_motus_generatebiom ? "-B" : "" }
         prefix = { "motus_${meta.id}_combined_reports" }
      }
      publishDir = [path:{ "${params.outdir}/motus/" }, mode:'copy']
   }
   withName:KMCP_SEARCH {
      tag = {"${meta.db_name}|${meta.id}"}
      ext {
         args = { "${meta.db_params}" }
         prefix = Script678F515BC912152A583104ACD4973692$_run_closure1$_closure55$_closure88@5c945ee7
      }
      publishDir = [path:{ "${params.outdir}/kmcp/${meta.db_name}/" }, mode:'copy', enabled:false]
   }
   withName:KMCP_PROFILE {
      tag = {"${meta.db_name}|${meta.id}"}
      ext {
         args = { "${meta.db_params}" }
         prefix = Script678F515BC912152A583104ACD4973692$_run_closure1$_closure56$_closure90@51a5a8ba
      }
      publishDir = [path:{ "${params.outdir}/kmcp/${meta.db_name}/" }, mode:'copy', pattern:'*.{profile}']
   }
   withName:GANON_CLASSIFY {
      tag = {"${meta.db_name}|${meta.id}"}
      ext {
         args = Script678F515BC912152A583104ACD4973692$_run_closure1$_closure57$_closure92@16eb93af
         prefix = Script678F515BC912152A583104ACD4973692$_run_closure1$_closure57$_closure94@404566e7
      }
      publishDir = [path:{ "${params.outdir}/ganon/${meta.db_name}/" }, mode:'copy', pattern:'*.{tre,rep,one,all,unc,log}']
   }
   withName:GANON_REPORT {
      tag = {"${meta.db_name}|${meta.id}"}
      ext {
         args = {[
            "--report-type ${params.ganon_report_type}",
            ganon_report_rank != 'default' ? "--ranks ${params.ganon_report_rank}" : "",
            "--top-percentile ${params.ganon_report_toppercentile}",
            "--min-count ${params.ganon_report_mincount}",
            "--max-count ${params.ganon_report_maxcount}"
        ].join(' ').trim() }
         prefix = Script678F515BC912152A583104ACD4973692$_run_closure1$_closure58$_closure96@3ed87b6e
      }
      publishDir = [path:{ "${params.outdir}/ganon/${meta.db_name}/" }, mode:'copy', pattern:'*.{tre}']
   }
   withName:GANON_TABLE {
      ext {
         prefix = { "ganon_${meta.id}_combined_reports" }
      }
      publishDir = [path:{ "${params.outdir}/ganon/" }, mode:'copy', pattern:'*.txt']
   }
   withName:TAXPASTA_MERGE {
      tag = { "${meta.tool}|${meta.id}" }
      ext {
         prefix = { "${meta.tool}_${meta.id}" }
         args = {
            [
                params.taxpasta_add_name ?  "--add-name" : "",
                params.taxpasta_add_rank ? "--add-rank" : "",
                params.taxpasta_add_lineage ? "--add-lineage" : "",
                params.taxpasta_add_idlineage ? "--add-id-lineage" : "",
                params.taxpasta_add_ranklineage ? "--add-rank-lineage" : "",
            ].join(' ').trim()
            }
      }
      publishDir = [path:{ "${params.outdir}/taxpasta/" }, mode:'copy', pattern:'*.{tsv,csv,arrow,parquet,biom}']
   }
   withName:TAXPASTA_STANDARDISE {
      tag = { "${meta.tool}|${meta.id}" }
      ext {
         prefix = { "${meta.tool}_${meta.id}" }
         args = {
                [
                    params.taxpasta_add_name ?  "--add-name" : "",
                    params.taxpasta_add_rank ? "--add-rank" : "",
                    params.taxpasta_add_lineage ? "--add-lineage" : "",
                    params.taxpasta_add_idlineage ? "--add-id-lineage" : "",
                    params.taxpasta_add_ranklineage ? "--add-rank-lineage" : ""
                ].join(' ').trim()
            }
      }
      publishDir = [path:{ "${params.outdir}/taxpasta/" }, mode:'copy', pattern:'*.{tsv,csv,arrow,parquet,biom}']
   }
   withName:MULTIQC {
      ext {
         args = { params.multiqc_title ? "--title \"$params.multiqc_title\"" : '' }
      }
      publishDir = [path:{ "${params.outdir}/multiqc" }, mode:'copy', saveAs:{ filename -> filename.equals('versions.yml') ? null : filename }]
   }
   shell = ['bash', '-C', '-e', '-u', '-o', 'pipefail']
   executor = 'azurebatch'
   queue = 'pool-Standard_E16ds_v4-pack'
}

apptainer {
   registry = 'quay.io'
}

docker {
   registry = 'quay.io'
}

podman {
   registry = 'quay.io'
}

singularity {
   registry = 'quay.io'
}

charliecloud {
   registry = 'quay.io'
}

env {
   PYTHONNOUSERSITE = '1'
   R_PROFILE_USER = '/.Rprofile'
   R_ENVIRON_USER = '/.Renviron'
   JULIA_DEPOT_PATH = '/usr/local/share/julia'
}

timeline {
   enabled = true
   file = 'timeline-23Nhgz2D50ZhCR.html'
}

report {
   enabled = true
   file = 'az://seqera/results/smoke_scrub_malthe/metagenomics2//pipeline_info/execution_report_2025-08-29_11-33-55.html'
}

trace {
   enabled = true
   file = 'az://seqera/results/smoke_scrub_malthe/metagenomics2//pipeline_info/execution_trace_2025-08-29_11-33-55.txt'
}

dag {
   enabled = true
   file = 'az://seqera/results/smoke_scrub_malthe/metagenomics2//pipeline_info/pipeline_dag_2025-08-29_11-33-55.html'
}

manifest {
   name = 'nf-core/taxprofiler'
   author = 'James A. Fellows Yates, Sofia Stamouli, Moritz E. Beber, Lili Andersson-Li, and the nf-core/taxprofiler team'
   contributors = [[name:'James A. Fellows Yates', affiliation:'Leibniz Institute for Natural Product Research and Infection Biology - Hans Knöll Institute, Jena, Germany; Max Planck Institute for Evolutionary Anthropology, Leipzig, Germany', email:'jfy133@gmail.com', github:'https://github.com/jfy133', contribution:['author', 'maintainer'], orcid:'https://orcid.org/0000-0001-5585-6277'], [name:'Sofia Stamouli', affiliation:'Karolinska Institutet/Karolinska University Hospital/Clinical Genomics SciLifeLab, Solna, Sweden', email:'', github:'https://github.com/sofstam', contribution:['author', 'maintainer'], orcid:'https://orcid.org/0009-0006-0893-3771'], [name:'Moritz E. Beber', affiliation:'Unseen Bio ApS, Copenhagen, Denmark​', email:'', github:'https://github.com/Midnighter', contribution:['author', 'maintainer'], orcid:'https://orcid.org/0000-0003-2406-1978'], [name:'Lili Andersson-Li', affiliation:'Karolinska Institutet/Karolinska University Hospital/Clinical Genomics SciLifeLab, Solna, Sweden', email:'', github:'https://github.com/LilyAnderssonLee', contribution:['author', 'maintainer'], orcid:'https://orcid.org/0000-0002-6059-4192'], [name:'and the nf-core/taxprofiler team', affiliation:'nf-core community', email:'', github:'https://github.com/nf-core', contribution:['contributor'], orcid:'']]
   homePage = 'https://github.com/nf-core/taxprofiler'
   description = 'Taxonomic classification and profiling of shotgun short- and long-read metagenomic data'
   mainScript = 'main.nf'
   defaultBranch = 'master'
   nextflowVersion = '!>=24.04.2'
   version = '1.2.3'
   doi = '10.1101/2023.10.20.563221'
}

plugins = ['nf-schema@2.3.0']

validation {
   monochromeLogs = false
   help {
      enabled = true
      command = 'nextflow run nf-core/taxprofiler -profile <docker/singularity/.../institute> --input samplesheet.csv --outdir <OUTDIR>'
      fullParameter = 'help_full'
      showHiddenParameter = 'show_hidden'
      beforeText = '
-[2m----------------------------------------------------[0m-
                                        [0;32m,--.[0;30m/[0;32m,-.[0m
[0;34m        ___     __   __   __   ___     [0;32m/,-._.--~'[0m
[0;34m  |\ | |__  __ /  ` /  \ |__) |__         [0;33m}  {[0m
[0;34m  | \| |       \__, \__/ |  \ |___     [0;32m\`-._,-`-,[0m
                                        [0;32m`._,._,'[0m
[0;35m  nf-core/taxprofiler 1.2.3[0m
-[2m----------------------------------------------------[0m-
'
      afterText = '
* The pipeline
    https://doi.org/10.1101/2023.10.20.563221

* The nf-core framework
    https://doi.org/10.1038/s41587-020-0439-x

* Software dependencies
    https://github.com/nf-core/taxprofiler/blob/master/CITATIONS.md
'
   }
   summary {
      beforeText = '
-[2m----------------------------------------------------[0m-
                                        [0;32m,--.[0;30m/[0;32m,-.[0m
[0;34m        ___     __   __   __   ___     [0;32m/,-._.--~'[0m
[0;34m  |\ | |__  __ /  ` /  \ |__) |__         [0;33m}  {[0m
[0;34m  | \| |       \__, \__/ |  \ |___     [0;32m\`-._,-`-,[0m
                                        [0;32m`._,._,'[0m
[0;35m  nf-core/taxprofiler 1.2.3[0m
-[2m----------------------------------------------------[0m-
'
      afterText = '
* The pipeline
    https://doi.org/10.1101/2023.10.20.563221

* The nf-core framework
    https://doi.org/10.1038/s41587-020-0439-x

* Software dependencies
    https://github.com/nf-core/taxprofiler/blob/master/CITATIONS.md
'
   }
}

azure {
   storage {
      accountName = 'workstoragedls'
   }
   batch {
      location = 'northeurope'
      accountName = 'batchseqeraprod'
      copyToolInstallMode = 'task'
      autoPoolMode = false
      allowPoolCreation = false
      pools {
         'pool-Standard_E16ds_v4-pack' {
            vmType = 'standard_e16ds_v4'
            vmCount = 0
         }
      }
   }
   managedIdentity {
      clientId = '25959159-120b-4e8e-a63a-dd9be7901124'
   }
}

workDir = 'az://seqera/scratch/2Fash9z9564Wul'
runName = 'awesome_goldberg_2'
resume = '22e5d9d9-1203-41a0-b6ff-e986085fd3be'

tower {
   enabled = true
   endpoint = 'https://api.cloud.seqera.io'
}

cloudcache {
   enabled = true
   path = 'az://seqera/.cache'
}

```

</details>
