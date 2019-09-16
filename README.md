# Detect 16S conserved regions
Given one or more sequences, in FASTA or FASTQ format, this script will align the query against _E. coli_ 16S gene, returning the variable regions covered.

Forked from [16S Region](https://github.com/telatin/16s-region)

### detect_region.pl

Will check an input sequence in FASTA or FASTQ format and detect the hypervariable regions of the reference (_E. coli_) 16S. If the input file contains more
than one sequence it will output the prediction for all of them, and since this can be slow (and useless) it can be avoided with `-m INT` switch 
(maximum number of sequences to parse). Output can be in JSON format with (-j). 

```
   USAGE:
     16S.pl [options] file.fa

   OPTIONS:
     -m, --max  INT        Read 'm' number of sequences and then stop [10]

     -t, --thr  FLOAT      Percentage of coverage of target region
                           (0-100), if below the coverage percentage will
                           be reported [80]
			   
     -s, --min-score FLOAT Minimum alignment score [40]
			 

     -v, --verbose         Prints progess and verbose output

     -j, --json            Prints output in JSON format (pretty print)
     -c, --compact         Compact JSON (i.e. disable pretty print)
     -f, --full            Add running parameters to JSON output

```

The JSON report has an "input_seqs" section, with the alignment read per read, and a "global_seqs" summary with the ratio (0-1) of sequences reported to cover a region.

```	
{
   "input_seqs" : {
      "M02007:34:000000000-AK48W:1:1101:15713:1758" : {
         "detected_regions" : "V3,V4",
         "regions" : {
            "V4" : 100,
            "V3" : 98.47
         },
         "align_score" : 220.9
      },
      "M02007:34:000000000-AK48W:1:1101:17706:1679" : {
         "align_score" : 263.1,
         "regions" : {
            "V4" : 100,
            "V3" : 98.47
         },
         "detected_regions" : "V3,V4"
      },
      "M02007:34:000000000-AK48W:1:1101:11681:1769" : {
         "detected_regions" : "V3,V4",
         "regions" : {
            "V3" : 98.47,
            "V4" : 100
         },
         "align_score" : 210.5
      }
   },
   "global_seqs" : {
      "hit_ratios" : {
         "V4" : 1,
         "V3" : 1
      }
   }
}
```	

