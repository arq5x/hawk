Synopsis
--------
hawk (header-awk) is meant to extend the incredible power of awk by allowing one to *name* columns directly rather than reference them by number.

Usage
-----
Like awk itself, hawk is best described by example. Basically, the intent is that anything you can do in awk, you can do in hawk.  The difference is that hawk allows one to refer to columns by name instead of merely by number (e.g., $sample instead of $1). The column names are defined in a header line, which should be the first line in the file/stream. 

Here's an example::

    cat test.bed
    chrom	start	end
    chr1	10	20
    chr1	30	40

Notice the header line in the file above. Now we can use hawk to filter based on column names::

    cat test.bed | hawk '$chrom=="chr1" && $start>=10'
    chr1	10	20
    chr1	30	40

Now working with the file, instead of a stream::

    hawk '$chrom=="chr1" && $start>=10' test.bed
    chr1	10	20
    chr1	30	40

More in this vein::

    hawk '$chrom=="chr1" && $start<30' test.bed 
    chr1	10	20



Advantages
----------
1. *Convenience*.  Many formats that one routinely works with have well-defined structures with columns that have an inherent meaning or name.  hawk is an attempt to leverage these definitions in our day-to-day code.
2. *Self-documenting code*.  By naming columns directly, it is much easier to assess what one was doing in a script.
3. *Built-in formats*.  For example, we could have sets of known column names/numbers for certain formats (e.g. BED).  This way, the header is unnecessary, we can just reference column names without actually having a header.



Open issues
===========
1. Proper command line parsing.  We currently just read from ARGV, and are unable to detect -v and -F arguments.
2. We should have a mode (-H) whereby the user tells us there is no header and we are just running vanilla awk.  Alternatively, this could be accomplished by just using awk itself.
3. Speed.  How much does the pre-processing of the column names slow things down.  Should be minimal.  If not, we need to eventually implement this in C.
