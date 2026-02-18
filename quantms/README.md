# Bigbio/Quantms

https://docs.quantms.org/

## QuantMS inputs

1. `samplesheet` file – Sample Data Relationship Format (SDRF) defines the
   massspectrometry input files,
   [example](https://github.com/biosustain/dsp_course_proteomics_intro/blob/main/data/PXD040621/PXD040621.sdrf.tsv):

- source name
- biological replicate
- modification parameters
- fragment mass tolerance
- data file

The construction of the SDRF can be done using a script, the lessSDRF online tool or
directly using Excel or similar spread sheet tools

2. Fasta file with protein sequences – defines the search space (for the recorded samples by the masspectrometer)
   - typically protein sequences from one species whose proteome was analyzed
   - typically from UNIPROT

3. `raw`, `.d` or `mzML` spectra files

Examples can be found in the proteomics introduction course, see: https://biosustain.github.io/dsp_course_proteomics_intro/material/quantms_input_files.html

## Parameters

> Some parameters are overwritten based on the SDRF settings. SDRF file level parameter
> settings always have prevalance over the pipeline parameters.

<details>
<summary>All parameters of quantms 1.6.0 (JSON)</summary>

```python
{
  "custom_config_base": "https://raw.githubusercontent.com/nf-core/configs/master",
  "min_peptide_length": 6,
  "alignment_order": "star",
  "fdr_level": "psm_level_fdrs",
  "msstatslfq_removeFewMeasurements": true,
  "plaintext_email": false,
  "luciphor_debug": 0,
  "isotope_correction": false,
  "extractpsmfeature_debug": 0,
  "ms2_fragment_method": "HCD",
  "msstatsiso_rmpsm_withfewmea_withinrun": true,
  "subset_max_train": 300000,
  "protein_score": "best",
  "feature_with_id_min_score": 0.1,
  "ms2rescore": false,
  "shuffle_sequence_identity_threshold": 0.5,
  "protein_inference_method": "aggregation",
  "reindex_mzml": true,
  "min_reporter_intensity": 0,
  "normalize": false,
  "skip_preliminary_analysis": false,
  "description_correct_features": 0,
  "decoy_string": "DECOY_",
  "variable_mods": "Oxidation (M)",
  "fragment_mass_tolerance": 0.03,
  "msstatslfq_quant_summary_method": "TMP",
  "skip_factor_validation": true,
  "psm_level_fdr_cutoff": 0.01,
  "skip_table_plots": false,
  "scan_window_automatic": true,
  "enable_diann_mztab": true,
  "pmultiqc_idxml_skip": true,
  "version": false,
  "openms_peakpicking": false,
  "publish_dir_mode": "copy",
  "input": "az://pxd040621-heweb/PXD040621/mzML/PXD040621.sdrf.tsv",
  "feature_without_id_min_score": 0.75,
  "msstatsiso_remove_norm_channel": true,
  "min_precursor_charge": 2,
  "consensusid_algorithm": "best",
  "protein_quant": "unique_peptides",
  "quant_activation_method": "HCD",
  "min_pr_mz": 400,
  "max_fr_mz": 1800,
  "min_peptides_per_protein": 1,
  "precursor_isotope_deviation": 10,
  "num_hits": 1,
  "precursor_mass_tolerance": 5,
  "average": "median",
  "decoy_method": "reverse",
  "allowed_missed_cleavages": 2,
  "max_peptide_length": 40,
  "iso_normalization": false,
  "protein_level_fdr_cutoff": 0.01,
  "random_preanalysis": false,
  "diann_debug": 3,
  "mass_acc_automatic": true,
  "custom_config_version": "master",
  "update_PSM_probabilities": false,
  "feature_generators": "deeplc,ms2pip",
  "msstats_remove_one_feat_prot": true,
  "top": 3,
  "fixed_mods": "Carbamidomethyl (C)",
  "msstatsiso_summaryformultiple_psm": "sum",
  "msstats_plot_profile_qc": false,
  "root_folder": "az://pxd040621-heweb/PXD040621/mzML",
  "pp_debug": 0,
  "email": "heweb@dtu.dk",
  "fix_peptides": false,
  "pg_level": 2,
  "use_ols_cache_only": true,
  "IL_equivalent": true,
  "acquisition_method": "dda",
  "empirical_assembly_ms_n": 200,
  "peakpicking_inmemory": false,
  "run_fdr_cutoff": 0.1,
  "lfq_intensity_threshold": 1000,
  "protein_inference_debug": 0,
  "local_input_type": "mzML",
  "quantification_method": "feature_intensity",
  "enable_pmultiqc": true,
  "outdir": "az://pxd040621-heweb/PXD040621/results-demo-compenv",
  "use_shared_peptides": true,
  "pipelines_testdata_base_path": "https://raw.githubusercontent.com/nf-core/test-datasets/",
  "sage_processes": 1,
  "help": false,
  "psm_clean": false,
  "min_precursor_purity": 0,
  "enable_mod_localization": false,
  "train_FDR": 0.05,
  "skip_ms_validation": false,
  "export_mztab": true,
  "klammer": false,
  "search_engines": "comet,sage",
  "force_model": false,
  "diann_report_decoys": false,
  "idfilter_debug": 0,
  "min_fr_mz": 100,
  "msstats_threshold": 0.05,
  "monochrome_logs": false,
  "diann_normalize": true,
  "test_FDR": 0.05,
  "find_best_model": true,
  "precursor_mass_tolerance_unit": "ppm",
  "consider_modloss": false,
  "protocol": "automatic",
  "skip_experimental_design_validation": false,
  "diann_export_xic": false,
  "add_triqler_output": false,
  "targeted_only": true,
  "max_multiqc_email_size": "25.MB",
  "msstatsiso_useunique_peptide": true,
  "min_consensus_support": 0,
  "max_precursor_charge": 4,
  "validate_params": true,
  "consensusid_debug": 0,
  "min_peaks": 10,
  "isotope_error_range": "0,1",
  "best_charge_and_fraction": false,
  "met_excision": true,
  "mod_localization": "Phospho (S),Phospho (T),Phospho (Y)",
  "performance_mode": true,
  "num_enzyme_termini": "fully",
  "add_decoys": true,
  "percolator_debug": 0,
  "fragment_mass_tolerance_unit": "Da",
  "msstatsiso_summarization_method": "msstats",
  "max_pr_mz": 2400,
  "trace_report_suffix": "2025-11-26_15-18-48",
  "db_debug": 0,
  "min_precursor_intensity": 1,
  "export_decoy_psm": true,
  "species_genes": false,
  "picked_fdr": true,
  "scan_window": 8,
  "iso_debug": 0,
  "quick_mass_acc": true,
  "msstatsiso_global_norm": true,
  "msstatslfq_feature_subset_protein": "top3",
  "ms2rescore_fragment_tolerance": 0.05,
  "mass_recalibration": false,
  "shuffle_max_attempts": 30,
  "protein_quant_debug": 0,
  "id_only": false,
  "database": "az://pxd040621-heweb/PXD040621/mzML/merged_ecoli_with_contaminants.fasta",
  "reporter_mass_shift": 0.002,
  "rescore_range": "independent_run",
  "random_preanalysis_seed": 42,
  "calibration_set_size": 0.15,
  "quantify_decoys": false,
  "reference_channel": "126",
  "contrasts": "pairwise",
  "idmapper_debug": 0,
  "ratios": false,
  "enzyme": "Trypsin",
  "include_all": true,
  "ms2_model": "HCD2021",
  "add_snr_feature_percolator": false,
  "validate_ontologies": true,
  "consensusid_considered_top_hits": 0,
  "top_PSMs": 1,
  "convert_dotd": false,
  "unmatched_action": "warn",
  "mzml_features": false,
  "skip_rescoring": false,
  "msstatsiso_reference_normalization": true,
  "idscoreswitcher_debug": 0,
  "skip_post_msstats": true,
  "decoy_string_position": "prefix",
  "max_mods": 3,
  "decoydatabase_debug": 0,
  "plfq_debug": 0
}
```

</details>

and highlighting the ones that were explicitly set (through the UI, as yaml or json):

```json
{
  // definitely change and check
  "root_folder": "az://pxd040621-heweb/PXD040621/mzML",
  "input": "az://pxd040621-heweb/PXD040621/mzML/PXD040621.sdrf.tsv",
  "outdir": "az://pxd040621-heweb/PXD040621/results-demo-compenv",
  "database": "az://pxd040621-heweb/PXD040621/mzML/merged_ecoli_with_contaminants.fasta",
  "add_decoys": true,
  // most likely to double check
  "acquisition_method": "dda",
  "search_engines": "comet,sage",
  "local_input_type": "mzML",
  "use_ols_cache_only": true,
  "reindex_mzml": true,
  "skip_post_msstats": true,

  // not changed by let's highlight
  "fragment_mass_tolerance": 0.03,
  "psm_level_fdr_cutoff": 0.01,
  "feature_without_id_min_score": 0.75,
  "min_precursor_charge": 2,
  "protein_quant": "unique_peptides",
  "quant_activation_method": "HCD",
  "min_pr_mz": 400,
  "max_fr_mz": 1800,
  "min_peptides_per_protein": 1,
  "precursor_isotope_deviation": 10,
  "num_hits": 1,
  "precursor_mass_tolerance": 5,
  "average": "median",
  "decoy_method": "reverse",
  "allowed_missed_cleavages": 2,
  "max_peptide_length": 40,
  "protein_level_fdr_cutoff": 0.01,
  "feature_generators": "deeplc,ms2pip",
  "fixed_mods": "Carbamidomethyl (C)",
  "lfq_intensity_threshold": 1000,
  "quantification_method": "feature_intensity",
  "use_shared_peptides": true,
  "precursor_mass_tolerance_unit": "ppm",
  "targeted_only": true,
  "min_consensus_support": 0,
  "max_precursor_charge": 4,
  "min_peaks": 10,
  "mod_localization": "Phospho (S),Phospho (T),Phospho (Y)",
  "add_decoys": true,
  "fragment_mass_tolerance_unit": "Da",
  "max_pr_mz": 2400,
  "enzyme": "Trypsin",
  "include_all": true,
  "ms2_model": "HCD2021",
  "max_mods": 3
}
```

## Resolved configuration on Seqera

Seqera give a resolved configuration of the parameters and all settings with respect
to the profile setting up the compute environment.

<details>
<summary>Resolved parameters and profile configuration of a quantms 1.6.0 (JSON)
on Azure using the docker profile</summary>

```
params {
   root_folder = 'az://pxd040621-heweb/PXD040621/mzML'
   local_input_type = 'mzML'
   database = 'az://pxd040621-heweb/PXD040621/mzML/merged_ecoli_with_contaminants.fasta'
   acquisition_method = 'dda'
   id_only = false
   input = 'az://pxd040621-heweb/PXD040621/mzML/PXD040621.sdrf.tsv'
   validate_ontologies = true
   skip_ms_validation = false
   skip_factor_validation = true
   skip_experimental_design_validation = false
   use_ols_cache_only = true
   add_decoys = true
   skip_rescoring = false
   search_engines = 'comet,sage'
   sage_processes = 1
   run_fdr_cutoff = 0.10
   protein_level_fdr_cutoff = 0.01
   psm_level_fdr_cutoff = 0.01
   psm_clean = false
   decoydatabase_debug = 0
   pp_debug = 0
   extractpsmfeature_debug = 0
   idfilter_debug = 0
   idscoreswitcher_debug = 0
   iso_debug = 0
   db_debug = 0
   percolator_debug = 0
   consensusid_debug = 0
   idmapper_debug = 0
   luciphor_debug = 0
   protein_inference_debug = 0
   plfq_debug = 0
   protein_quant_debug = 0
   decoy_string = 'DECOY_'
   decoy_string_position = 'prefix'
   decoy_method = 'reverse'
   shuffle_max_attempts = 30
   shuffle_sequence_identity_threshold = 0.5
   openms_peakpicking = false
   peakpicking_inmemory = false
   peakpicking_ms_levels = null
   reindex_mzml = true
   mzml_features = false
   labelling_type = null
   isotope_correction = false
   plex_corr_matrix_file = null
   reference_channel = '126'
   min_precursor_intensity = 1.0
   reporter_mass_shift = 0.002
   quant_activation_method = 'HCD'
   iso_normalization = false
   min_reporter_intensity = 0.0
   min_precursor_purity = 0.0
   precursor_isotope_deviation = 10.0
   enzyme = 'Trypsin'
   met_excision = true
   num_enzyme_termini = 'fully'
   allowed_missed_cleavages = 2
   precursor_mass_tolerance = 5
   precursor_mass_tolerance_unit = 'ppm'
   fixed_mods = 'Carbamidomethyl (C)'
   variable_mods = 'Oxidation (M)'
   enable_mod_localization = false
   mod_localization = 'Phospho (S),Phospho (T),Phospho (Y)'
   fragment_mass_tolerance = 0.03
   fragment_mass_tolerance_unit = 'Da'
   ms2_fragment_method = 'HCD'
   isotope_error_range = '0,1'
   instrument = null
   protocol = 'automatic'
   min_precursor_charge = 2
   max_precursor_charge = 4
   min_peptide_length = 6
   max_peptide_length = 40
   num_hits = 1
   max_mods = 3
   min_peaks = 10
   min_pr_mz = 400
   max_pr_mz = 2400
   min_fr_mz = 100
   max_fr_mz = 1800
   ms2rescore = false
   ms2_model_dir = null
   rescore_range = 'independent_run'
   ms2_model = 'HCD2021'
   feature_generators = 'deeplc,ms2pip'
   calibration_set_size = 0.15
   ms2rescore_fragment_tolerance = 0.05
   find_best_model = true
   force_model = false
   consider_modloss = false
   add_snr_feature_percolator = false
   IL_equivalent = true
   unmatched_action = 'warn'
   export_decoy_psm = true
   train_FDR = 0.05
   test_FDR = 0.05
   fdr_level = 'psm_level_fdrs'
   klammer = false
   description_correct_features = 0
   subset_max_train = 300000
   consensusid_algorithm = 'best'
   min_consensus_support = 0
   consensusid_considered_top_hits = 0
   luciphor_neutral_losses = null
   luciphor_decoy_mass = null
   luciphor_decoy_neutral_losses = null
   top_PSMs = 1
   update_PSM_probabilities = false
   picked_fdr = true
   protein_score = 'best'
   min_peptides_per_protein = 1
   use_shared_peptides = true
   top = 3
   average = 'median'
   best_charge_and_fraction = false
   normalize = false
   ratios = false
   fix_peptides = false
   include_all = true
   export_mztab = true
   protein_inference_method = 'aggregation'
   protein_quant = 'unique_peptides'
   quantification_method = 'feature_intensity'
   mass_recalibration = false
   alignment_order = 'star'
   add_triqler_output = false
   quantify_decoys = false
   targeted_only = true
   feature_with_id_min_score = 0.10
   feature_without_id_min_score = 0.75
   lfq_intensity_threshold = 1000
   convert_dotd = false
   diann_debug = 3
   scan_window = 8
   scan_window_automatic = true
   performance_mode = true
   quick_mass_acc = true
   mass_acc_automatic = true
   pg_level = 2
   species_genes = false
   diann_normalize = true
   diann_speclib = null
   diann_report_decoys = false
   diann_export_xic = false
   skip_preliminary_analysis = false
   empirical_assembly_log = null
   random_preanalysis = false
   random_preanalysis_seed = 42
   empirical_assembly_ms_n = 200
   enable_diann_mztab = true
   msstats_remove_one_feat_prot = true
   ref_condition = null
   contrasts = 'pairwise'
   msstats_threshold = 0.05
   skip_post_msstats = true
   msstatslfq_removeFewMeasurements = true
   msstatslfq_feature_subset_protein = 'top3'
   msstatslfq_quant_summary_method = 'TMP'
   msstatsiso_useunique_peptide = true
   msstatsiso_rmpsm_withfewmea_withinrun = true
   msstatsiso_summaryformultiple_psm = 'sum'
   msstatsiso_summarization_method = 'msstats'
   msstatsiso_global_norm = true
   msstatsiso_remove_norm_channel = true
   msstatsiso_reference_normalization = true
   msstats_plot_profile_qc = false
   enable_pmultiqc = true
   pmultiqc_idxml_skip = true
   multiqc_config = null
   multiqc_title = null
   multiqc_logo = null
   skip_table_plots = false
   max_multiqc_email_size = '25.MB'
   multiqc_methods_description = null
   outdir = 'az://pxd040621-heweb/PXD040621/results-demo-compenv'
   publish_dir_mode = 'copy'
   email = 'heweb@dtu.dk'
   email_on_fail = null
   plaintext_email = false
   monochrome_logs = false
   hook_url = null
   help = false
   version = false
   pipelines_testdata_base_path = 'https://raw.githubusercontent.com/nf-core/test-datasets/'
   trace_report_suffix = '2025-11-26_15-18-50'
   config_profile_name = null
   config_profile_description = null
   custom_config_version = 'master'
   custom_config_base = 'https://raw.githubusercontent.com/nf-core/configs/master'
   config_profile_contact = null
   config_profile_url = null
   validate_params = true
}

process {
   cpus = { 1      * task.attempt }
   memory = { 6.GB   * task.attempt }
   time = { 4.h    * task.attempt }
   errorStrategy = { task.exitStatus in ((130..145) + 104 + 175) ? 'retry' : 'finish' }
   maxRetries = 1
   maxErrors = '-1'
   withLabel:process_tiny {
      cpus = { 1                   }
      memory = { 1.GB * task.attempt }
      time = { 1.h  * task.attempt }
   }
   withLabel:process_single {
      cpus = { 1                   }
      memory = { 6.GB * task.attempt }
      time = { 4.h  * task.attempt }
   }
   withLabel:process_low {
      cpus = { 4     * task.attempt }
      memory = { 12.GB * task.attempt }
      time = { 6.h   * task.attempt }
   }
   withLabel:process_very_low {
      cpus = { 2     * task.attempt}
      memory = { 4.GB  * task.attempt}
      time = { 3.h   * task.attempt}
   }
   withLabel:process_medium {
      cpus = { 8     * task.attempt }
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
   withLabel:process_gpu {
      ext {
         use_gpu = { workflow.profile.contains('gpu') }
      }
      accelerator = { workflow.profile.contains('gpu') ? 1 : null }
   }
   shell = ['bash', '-C', '-e', '-u', '-o', 'pipefail']
   withName:'BIGBIO_QUANTMS:QUANTMS:CUSTOM_DUMPSOFTWAREVERSIONS' {
      publishDir = [path:{ "${params.outdir}/pipeline_info" }, mode:'copy', pattern:'*_versions.yml']
   }
   withName:'BIGBIO_QUANTMS:QUANTMS:SUMMARY_PIPELINE' {
      publishDir = [path:{ "${params.outdir}/pmultiqc" }, mode:'copy', saveAs:{ filename -> filename.equals('versions.yml') ? null : filename }]
   }
   withName:'BIGBIO_QUANTMS:QUANTMS:INPUT_CHECK:SAMPLESHEET_CHECK' {
      publishDir = [path:{ "${params.outdir}/sdrf" }, mode:'copy', saveAs:{ filename -> filename.equals('versions.yml') ? null : filename }]
   }
   withName:'.*:SDRF_PARSING' {
      publishDir = [path:{ "${params.outdir}/sdrf" }, mode:'copy', saveAs:{ filename -> filename.equals('versions.yml') ? null : filename }]
   }
   withName:'.*:MSSTATS_LFQ|MSSTATS_TMT' {
      publishDir = [path:{ "${params.outdir}/msstats" }, mode:'copy', saveAs:{ filename -> filename.equals('versions.yml') ? null : filename }]
   }
   withName:'.*:PROTEOMICSLFQ|PROTEIN_QUANTIFIER|MSSTATS_CONVERTER|FINAL_QUANTIFICATION|CONVERT_RESULTS' {
      publishDir = [path:{ "${params.outdir}/quant_tables" }, mode:'copy', saveAs:{ filename -> filename.equals('versions.yml') ? null : filename }]
   }
   withName:'.*:PSM_CONVERSION' {
      publishDir = [path:{ "${params.outdir}/psm_tables" }, mode:'copy', saveAs:{ filename -> filename.equals('versions.yml') ? null : filename }]
   }
   withName:'.*:FDR_CONSENSUSID' {
      ext {
         args = '-PSM true -protein false'
      }
   }
   withName:'.*:TMT:.*:ID_MAPPER' {
      ext {
         args = '-debug 0'
      }
   }
   level = 'proteingroup'
   decoys_present = '-remove_decoys'
   withName:'.*:TMT:PROTEIN_INFERENCE:ID_FILTER' {
      ext {
         args = '-score:proteingroup "0.01" -score:psm "0.01" -delete_unreferenced_peptide_hits -remove_decoys'
         suffix = '.consensusXML'
      }
   }
   withName:'.*:TMT:PROTEIN_QUANT:PROTEIN_QUANTIFIER' {
      ext {
         args = '-debug 0'
      }
   }
   withName:'.*:TMT:PROTEIN_QUANT:MSSTATS_CONVERTER' {
      ext {
         args = '-debug 0'
      }
   }
   withName:'.*:PERCOLATOR' {
      ext {
         args = '-debug 0'
      }
   }
   withName:'.*:PROTEIN_INFERENCE' {
      ext {
         args = '-debug 0'
      }
   }
   withName:'.*:ID:PSM_FDR_CONTROL:ID_FILTER' {
      ext {
         args = '-score:psm "0.10"'
         suffix = '.idXML'
      }
   }
   withName:'.*:DDA_ID:PSM_FDR_CONTROL:ID_FILTER' {
      ext {
         args = '-score:psm "0.10"'
         suffix = '.idXML'
      }
   }
   withName:'.*:LFQ:PROTEOMICSLFQ' {
      ext {
         args = '-debug 0'
      }
   }
   withName:'.*:DIA:INSILICO_LIBRARY_GENERATION' {
      ext {
         args = '--met-excision'
      }
   }
   withName:'.*:DIA:PRELIMINARY_ANALYSIS' {
      ext {
         args = '--quick-mass-acc --min-corr 2 --corr-diff 1 --time-corr-only'
      }
   }
   withName:MSRESCORE_FEATURES {
      ext {
         args = '--ms2_model HCD2021 --calibration_set_size 0.15 --feature_generators deeplc,ms2pip'
      }
   }
   withName:'.*:DDA_ID:PHOSPHO_SCORING:ID_SCORE_SWITCHER' {
      ext {
         args = '-new_score_orientation lower_better -old_score "q-value" -new_score_type "Posterior Error Probability" -debug 0'
      }
   }
   executor = 'azurebatch'
   queue = 'pool-pxd040621-heweb'
}

apptainer {
   registry = 'quay.io'
   enabled = false
}

docker {
   registry = 'quay.io'
   enabled = true
   runOptions = '-u $(id -u):$(id -g)'
}

podman {
   registry = 'quay.io'
   enabled = false
}

singularity {
   registry = 'quay.io'
   enabled = false
}

charliecloud {
   registry = 'quay.io'
   enabled = false
}

env {
   PYTHONNOUSERSITE = 1
   R_PROFILE_USER = '/.Rprofile'
   R_ENVIRON_USER = '/.Renviron'
   JULIA_DEPOT_PATH = '/usr/local/share/julia'
}

nextflow {
   enable {
      configProcessNamesValidation = false
   }
}

timeline {
   enabled = true
   file = 'timeline-3i5kEjsJrY9iFc.html'
}

report {
   enabled = true
   file = 'az://pxd040621-heweb/PXD040621/results-demo-compenv/pipeline_info/execution_report_2025-11-26_15-18-50.html'
}

trace {
   enabled = true
   file = 'az://pxd040621-heweb/PXD040621/results-demo-compenv/pipeline_info/execution_trace_2025-11-26_15-18-50.txt'
}

dag {
   enabled = true
   file = 'az://pxd040621-heweb/PXD040621/results-demo-compenv/pipeline_info/pipeline_dag_2025-11-26_15-18-50.html'
}

manifest {
   name = 'bigbio/quantms'
   homePage = 'https://github.com/bigbio/quantms'
   contributors = [[name:'Yasset Perez-Riverol', affiliation:'European Bioinformatics Institute (EMBL-EBI), Cambridge, UK', email:'ypriverol@gmail.com', github:'ypriverol', contribution:['maintainer', 'author'], orcid:'0000-0001-6579-6941'], [name:'Dai Chengxin', affiliation:'State Key Laboratory of Medical Proteomics, Beijing Proteome Research Center, Beijing, China', email:'daichengxin999@gmail.com', github:'daichengxin', contribution:['author', 'maintainer'], orcid:'0000-0001-6943-5211'], [name:'Julianus Pfeuffer', affiliation:'Algorithmic Bioinformatics, Freie Universität Berlin, Berlin, Germany', email:'jule.pf@gmail.com', github:'jpfeuffer', contribution:['author', 'maintainer'], orcid:'0000-0001-8948-9209'], [name:'Dongze He', affiliation:'Altos Labs, Inc.', email:'dongzehe.zaza@gmail.com', github:'DongzeHe', contribution:['contributor'], orcid:'0000-0001-8259-7434'], [name:'Henry Webel', affiliation:'DTU biosustain, Technical University of Denmark, Lyngby, Denmark', email:'heweb@dtu.dk', github:'enryh', contribution:['contributor'], orcid:'0000-0001-8833-7617'], [name:'Fabian Egli', github:'fabianegli', contribution:['contributor'], orcid:'0000-0001-5294-401X']]
   description = 'Quantitative Mass Spectrometry nf-core workflow'
   mainScript = 'main.nf'
   defaultBranch = 'master'
   nextflowVersion = '!>=24.10.5'
   version = '1.6.0'
   doi = '10.5281/zenodo.15573386'
}

plugins = ['nf-schema@2.4.2']

validation {
   defaultIgnoreParams = ['genomes']
   monochromeLogs = false
   help {
      enabled = true
      command = 'nextflow run bigbio/quantms -profile <docker/singularity/.../institute> --input samplesheet.csv --outdir <OUTDIR>'
      fullParameter = 'help_full'
      showHiddenParameter = 'show_hidden'
      beforeText = '
-[2m----------------------------------------------------[0m-
                                       [0;32m,--.[0;30m/[0;32m,-.[0m
[0;34m        ___     __   __   __   ___     [0;32m/,-._.--~'[0m
[0;34m  |\ | |__  __ /  ` /  \ |__) |__         [0;33m}  {[0m
[0;34m  | \| |       \__, \__/ |  \ |___     [0;32m\`-._,-`-,[0m
                                       [0;32m`._,._,'[0m
[0;35m  bigbio/quantms 1.6.0[0m
-[2m----------------------------------------------------[0m-
'
      afterText = '
* The pipeline
   https://doi.org/10.5281/zenodo.15573386

* The nf-core framework
   https://doi.org/10.1038/s41587-020-0439-x

* Software dependencies
   https://github.com/bigbio/quantms/blob/master/CITATIONS.md
'
   }
   summary {
      beforeText = '
-[2m----------------------------------------------------[0m-
                                       [0;32m,--.[0;30m/[0;32m,-.[0m
[0;34m        ___     __   __   __   ___     [0;32m/,-._.--~'[0m
[0;34m  |\ | |__  __ /  ` /  \ |__) |__         [0;33m}  {[0m
[0;34m  | \| |       \__, \__/ |  \ |___     [0;32m\`-._,-`-,[0m
                                       [0;32m`._,._,'[0m
[0;35m  bigbio/quantms 1.6.0[0m
-[2m----------------------------------------------------[0m-
'
      afterText = '
* The pipeline
   https://doi.org/10.5281/zenodo.15573386

* The nf-core framework
   https://doi.org/10.1038/s41587-020-0439-x

* Software dependencies
   https://github.com/bigbio/quantms/blob/master/CITATIONS.md
'
   }
}

conda {
   enabled = false
}

shifter {
   enabled = false
}

azure {
   storage {
      accountName = 'stheweba6bed813'
   }
   batch {
      location = 'westeurope'
      accountName = 'batchpxd040621'
      copyToolInstallMode = 'task'
      autoPoolMode = false
      allowPoolCreation = false
      pools {
         'pool-pxd040621-heweb' {
            vmType = 'standard_d4s_v3'
            vmCount = 0
         }
      }
   }
   managedIdentity {
      clientId = '50eae5e0-e75f-4bd4-a965-8a78b9a7587a'
   }
}

workDir = 'az://pxd040621-heweb/scratch/3i5kEjsJrY9iFc'
runName = 'hungry_stallman'

tower {
   enabled = true
   endpoint = 'https://api.cloud.seqera.io'
}

cloudcache {
   enabled = true
   path = 'az://pxd040621-heweb/.cache'
}

```

</details>

## Slide decks

The proteomics data analysis course has a set of slides with a quantms section.
see [pdf](https://biosustain.github.io/dsp_course_proteomics_intro/_downloads/9f8a58b01827cf178640e8665af56ecc/introduction.pdf)

Latest version can be found on course website:
[dsp_course_proteomics_intro/](https://biosustain.github.io/dsp_course_proteomics_intro/)
