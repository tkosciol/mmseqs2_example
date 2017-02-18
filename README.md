$ cd examples
$ awk '/^>/{seqCount++;} {if (seqCount <= 19000) {print $0;}}' DB.fasta > DB_trimmed.fasta

$ mmseqs createdb DB.fasta DB_new
$ mmseqs createdb DB_trimmed.fasta DB_trimmed
$ mkdir tmp
$ mmseqs cluster DB_trimmed DB_trimmed_clu tmp

$ rm tmp/*
$ mmseqs clusterupdate DB_trimmed DB_new DB_trimmed_clu DB_clusterupdate tmp

$ rm -r tmp/*
$ mmseqs cluster DB_new DB_new_clu tmp

$ wc -l DB_new_clu.index DB_clusterupdate.index DB_trimmed_clu.index

# test more extreme case (10k sequences)
$ awk '/^>/{seqCount++;} {if (seqCount <= 10000) {print $0;}}' DB.fasta > DB_trimmed10k.fasta

$ mmseqs createdb DB_trimmed10k.fasta DB_trimmed10k
$ rm -r tmp/*
$ mmseqs cluster DB_trimmed10k DB_trimmed10k_clu tmp

$ rm tmp/*
$ mmseqs clusterupdate DB_trimmed10k DB_new DB_trimmed10k_clu DB_clusterupdate10k tmp

$ wc -l DB_new_clu.index DB_clusterupdate10k.index DB_trimmed10k_clu.index
