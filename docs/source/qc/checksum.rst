"Verifying data integrity using MD5 checksum"
============

Check md5 sum 
~~~~~~~~~~~~~
It is important to verify that the data you are using is the same data you wanted. This is easily checked using `md5sum <https://en.wikipedia.org/wiki/Md5sum>`_. Most sequence data files you download from online databases contain an md5 file associated with them. If they don't you might want to consider contacting the admin and requesting one. 

As an example, when I downloaded the ~100 gigabytes of raw illumina data from novogene, there was an additional md5 file containing the below hashes::

    e6a675bf8c4adff503dd7b18048d9670  P_cali_g_USD16092980L_HKG7CDSXX_L1_1.fq.gz
    becc55c2d2406b4df4ad05e8fdc4347c  P_cali_g_USD16092980L_HKG7CDSXX_L1_2.fq.gz

I checked the md5 hashes for the downloaded files using md5sum.::

    md5sum -c *.MD5.txt

    becc55c2d2406b4df4ad05e8fdc4347c  P_cali_g_USD16092980L_HKG7CDSXX_L1_2.fq.gz
    e6a675bf8c4adff503dd7b18048d9670  P_cali_g_USD16092980L_HKG7CDSXX_L1_1.fq.gz


md5 check sums look good. No corruption of data downloaded to this computer. It is generally a good idea to perform this check when copying large files to backups or other hardware. 
