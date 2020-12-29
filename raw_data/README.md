# Directory information

Owing to the extremely-large size of the raw data, we present one raw data example here for reference. The rest of raw data (download link) can be requested via author's email.

## Decompress command and outputs

Our characterization data are compressed by fio (specifically, **fio 3.6**) built-in compression. Note that fio's compression is **not** backward-compatable; so, other fio version may not be able to decompress our characterization successally. A failed decompression can show wrong read latecny values. 

Here is the command to decompress our characterization data.

```
./fio --inflate-log=5620_b_16k_adata_0_lat.1.log.fz
```

The file naming rule is as follow:
```
<batch id>_b_<read size>_<SSD short name>_<rounds in a batch>_lat.1.log.fz
```

\<batch id\> starts from 10, and it increases by 10 for the following batch. \<read size\> can range from 2K to 512K, but most of our characterization use 16K as read size. \<SSD short name\> are the short name for each SSD. For example, the short name for SSD-B is adata. Other short names for the other SSDs can be found in the corresponding directories. \<rounds in a batch\> can only be 0 to 4. That is, our example is actually (5,620 / 10 - 1) * 5 + 0 = 2,805 rounds (starting from 0) of the entire characterization data. Note that some batch ids are skipped due to some collection errors.


Here is the output of the example:

```
0, 850771, 0, 16384, 79024177152
1, 220763, 0, 16384, 56591482880
1, 246966, 0, 16384, 76303794176
1, 222473, 0, 16384, 27924054016
1, 223365, 0, 16384, 33761607680
2, 223152, 0, 16384, 33416921088
2, 220533, 0, 16384, 57549897728
2, 177744, 0, 16384, 56863604736
2, 183330, 0, 16384, 2770059264
2, 270234, 0, 16384, 84810203136
.......
```

The format of the output
```
<elapsed time in ms>, <read latency in ns>, <thread id>, <read size in bytes>, <logical address in bytes> 
```
\<thread id\> can only be 0, because there is always only one thread (thread 0). \<read size in bytes\> are always the same in the same file. \<logical address in bytes\> are different across the entire file since every address are only read once. One may find out that some of logical addresses are missing in the file. This is becuase those logical addresses can no longer be read from the disk owing to too many uncorrectable bit errors in the data.

