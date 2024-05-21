
Nanopre workshop

Minimum lovable product


QC - what do we want
Systemic bias? How does it develop over time?

QC - What do we need after basecalling
Adapter content
Number of reads
Read length
Quality score - average QC score - Q score N50
GC content
After mapping
Depth of coverage 
Percentage of reads mapping

PRD
MVP: fastq input, 
read length - distribution and distribution over time
total number of reads
quality values, N50
Adapter content (nice to have)


# run template to get test output qx
'''bash

nextflow run wf-gobyqc \
--fastq nex-gobyqc/test_data/reads.fastq.gz \
-profile standard

'''
# histogram
plot of the hist file
# total number of reads


# Nanoplot
NanoPlot -t 16 --fastq seqs.fastq.gz --plots kde hex dot



# N50
Count all the nucleotides, and count the read length to achieve half of the nucleotides

process run_nanoplot {
    input
    output
    script
    """
        NanoPlot \
        -t 16 \
        --fastq seqs.fastq.gz \
        --plots kde hex dot
        """

    }

 pipeline.out.nanoplot_results
        | map { [it, "${params.fastq ? "fastq" : "xam"}_nanoplot_results"] }
        | concat (
            pipeline.out.report.concat(pipeline.out.workflow_params)
                | map { [it, null] })
        | output_2
