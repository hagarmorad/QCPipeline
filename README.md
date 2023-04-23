# QCPipeline
### QCPipeline conda environment
* In order to install all dependencies using conda, run: \
    `conda env create -f <your/QCPipeline/path>/env/QCPipeline.yml`
### Usage:
`python QC.py [-h] [-t [CSV] | --template | -r] [-i WD] [-o OUTPUT_PATH]`
#### -r | --reports : QC Reports
Produces fastqc and multiqc reports for all fastq files. \
_-i_ : path to fastq files locations. default: fastq/raw/ \
_-o_ : path to drop the template.csv. default: QC/ \
`python3 QC.py -r -i path/to/fastq/files -o path/for/output/reports` \
To create QC reports with default input and output arguments:
`python3 QC.py -r` 

#### --template : Template of trimmomatic command for each sample
 _-i_ : path to fastq files location. default: fastq/raw/ \
 _-o_ : path to drop the template.csv. default: working directory \
`python3 QC.py --template -i some/path/to/fastq/location/ -o some/path/for/output` \
To create template with default input and output arguments: \
`python3 QC.py --template` 

#### -t|--trim <path/to/csv> <optional-path/to/base/dir> : Trim fastq files with trimmomatic, according to csv file.
_-i_ : path to fastq files location. default: fastq/raw/ \
_-o_ : path to drop the trimmed samples. default: fastq/trimmed \
`python3 QC.py -t path/to/file.csv -i path/to/raw/fastq/files -o path/to/trimmed/fatsq/output` \
To trim fastq files with default input and output arguments: \
`python3 QC.py -t path/to/file.csv ` or
`python3 QC.py --trim path/to/file.csv`\
To trim with the template csv as input, simply run `python3 QC.py -t template.csv`.

### The CSV file:
Template csv header:
>ends, input_forward, input_reverse, phred, threads, ILLUMINACLIP:, SLIDINGWINDOW:, LEADING:, TRAILING:, CROP:,
>HEADCROP:, MINLEN: 

Each line will represent a fastq file.

**Required fields**: ends, input_forward. Make sure those fields are filled in the csv file \
trimmomatic requires output files as well, but those are given automatically in the script. Don't include them in the csv.
* ends`[PE|SE]` - PE:paired end, SE: single end. If you choose `PE`, be sure to include both input_forward **and**  input_reverse! 
* input_forward `<sample_R1.fastq>`: sample name
* input_reverse `<sample_R2.fastq>`: sample name

**Optional fields**: 
Do not remove unwanted fields, just leave them empty and it will be ignored.
* -phred`[33 | 64]` -  -phred33 or -phred64 specifies the base quality encoding. If no quality encoding is specified,
it will be determined automatically 
* threads<int>: indicates the number of threads to use, which improves performance on multi-core
computers. If not specified, it will be chosen automatically. 
* ILLUMINACLIP:`<fastaWithAdaptersEtc>:<seed mismatches>:<palindrome clip
threshold>:<simple clip threshold>` - Cut adapter and other illumina-specific sequences from the read. 
* SLIDINGWINDOW:`<windowSize>:<requiredQuality>` -  Perform a sliding window trimming, cutting once the average quality 
within the window falls below a threshold. 
* LEADING:/TRAILING:`<quality>` - Cut bases off the start/end of a read respectively, if below a threshold quality. 
quality: Specifies the minimum quality required to keep a base.
* CROP:`<length>`  - Removes bases regardless of quality from the end of the read, so that the read has maximally
the specified length after this step has been performed. length: The number of bases to keep, from the start of the read.
* HEADCROP:`<length>` - Removes the specified number of bases, regardless of quality, from the beginning of the read.
length: The number of bases to remove from the start of the read
* MINLEN:`<length>` - Removes reads that fall below the specified minimal length.  If required, it should
normally be after all other processing steps. 

NOTE: When adding new arguments to csv, add the argument as it will appear in the Trimmomatic command,
 including symbols like -, :, etc. 
 
>For more info about Trimmomatic's arguments, see Trimmomatic documentations:
>http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/TrimmomaticManual_V0.32.pdf,
>http://www.usadellab.org/cms/?page=trimmomatic
---------------
