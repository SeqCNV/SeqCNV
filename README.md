The manual for SeqCNV.

## Download SeqCNV tool

Please download the file: <a href="SeqCNV.tar.gz">SeqCNV.tar.gz</a>
## Install
To install, you should run:

1. tar -zxvf SeqCNV.tar.gz
2. cd SeqCNV
3. make

No need to do "make install".

## Run SeqCNV
SeqCNV running is simple and quick. Only one step to run SeqCNV.

To start, you can run `perl run_CNV.pl -h` to see usage and options.

```txt
Usage: perl
	-h Prints this message
	-p toolpath: [Required] The path to seqcnv tools (Required tools: separate_chrom.pl, run_seqcnv_allchr.pl, report_from_tsv.pl)
	-c control: [Required] A .bam file containing the control sample.
	-t case: [Required] A .bam file containg the case sample.
	-o out_dir: [Required] The directory for all output (should be seperate for each case/control pair).
	-r target: [Required] A .bed file contaning the capture target information.
```

## An example
Suppose I have installed SeqCNV. 

- Path to my case bam file is `/home/case.bam`. 
- Path to my control bam file is `/home/control.bam`. 
- Path to my target file is `/home/target.bed`
- My SeqCNV program folder is `/home/SeqCNV`
- I set the output path to `/home/output`

Then my running command is

```sh
perl /home/SeqCNV/run_CNV.pl -c /home/control.bam -t /home/case.bam -o /home/output -r /home/target.bed -p /home/SeqCNV
```

Then I can enjoy my coffee to wait for the results.
## Result
Result tree for SeqCNV is:

```txt
output  
   |---tumor
   |---control
   |---blk
   |---tsv
   |---report
```

- The folder 'tumor' contains candidate break points for each chromosome in case. 
- The folder 'control' contains candidate break points for each chromosome in control. 
- Files in folder 'blk' are blocks converted from candidate break points for case and control. All of them are in binary format. You can not see any effective information if you open them using text editor.
- The folder 'tsv' contains temporaty output for results. 
- The file 'CNV_report.txt' in 'report' is the final **result for SeqCNV**.

### Format for CNV_report.txt
Records in `CNV_report.txt` follows the format below:

```txt
#chr start_pos end_pos length(kb) c_on t_on r_on
chr1	152553771	152591451	37.681	1229	2272	0.524
chr1	196716471	196826740	110.270	11142	5874	1.836
chr1	207697490	207700191	2.702	1087	481	2.186
chr1	222375628	222380569	4.942	0	163	0.003
chr1	248636545	248636641	0.097	7	332	0.022
.
.
.
```

- `chr`: The chromosome name for this CNV event.
- `start_pos`: Start position for this CNV event.
- `end_pos`: End position for this CNV event.
- `length(kb)`: Length for this CNV event.
- `c_on`: Total number of reads in control for this CNV event.
- `t_on`: Total number of reads in case for this CNV event.
- `r_on`: The value of maximum penalty log-likelihood for thie CNV event.

Note if `r_on` value is below 0.6, type for the CNV event is **<font color="red">Copy LOSS</font>**. If `r_on` value is above 1.4, type for the CNV event is **<font color="red">Copy GAIN</font>**. 

