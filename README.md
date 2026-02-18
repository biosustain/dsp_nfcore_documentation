# Nextflow pipeline documentation at BRIGHT

## Basic lauch on Seqera of a pipeline

- pipeline runs are triggered through the website, not manually
- tower is a Seqera tool and is configured through environment variables on Seqera

```
nextflow run 'https://github.com/nf-core/quantms'
		 -name my_unique_name
		 -params-file params.json
		 -with-tower
		 -r 1.6.0
		 -profile docker,wave
```
