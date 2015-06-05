***CURRENT STATUS: It doesn't work yet ... ***
===
Just so you know ...











T-Digest Pig UDF
======================

This is a Pig wrapper around [t-digest](https://github.com/tdunning/t-digest).

Build:
------

    git clone https://github.com/nielsbasjes/tdigest-pig
    
Now build and install the java version:

    cd tdigest-pig
    mvn install 

NOTE: You may have to skip the testing in case there is a problem 

    mvn install -DskipTests=true

Usage:
--------
```pig
REGISTER ../target/tdigest-pig-*-udf.jar;

DEFINE TDigestMerge     nl.basjes.pig.tdigest.Merge;
DEFINE TDigestQuantile  nl.basjes.pig.tdigest.Quantile;

Data = LOAD 'data.txt' AS (value:long);

GroupedData = GROUP Data ALL;

TDGroup =
    FOREACH GroupedData
    GENERATE TDigestMerge(Data.value) AS tDigest:bytearray;

TDValues =
    FOREACH     TDGroup
    GENERATE    TDigestQuantile(tDigest,0.9) AS Precentile9:double,
                TDigestQuantile(tDigest,0.99) AS Precentile99:double,
                TDigestQuantile(tDigest,0.999) AS Precentile999:double,
                TDigestQuantile(tDigest,0.5) AS Precentile5:double;

DUMP TDValues;
```

Author:
-------

  * Niels Basjes [@nielsbasjes](https://twitter.com/nielsbasjes)

  This is a trivial interface on top of the T-Digest created by Ted Dunning.
