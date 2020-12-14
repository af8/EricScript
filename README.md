# EricScript

`EricScript` was developed at
https://sites.google.com/site/bioericscript/
However maintenance was stopped after the last update v0.5.5 (April 2016)

In this project, `EricScript` will be maintained to work.


<b>May 2, 2020</b><br />

The two options `--printdb` and `--downdb` often failed due to lack of `ftp` client in many environment.

## Logic simplification for use with docker and singularity

To check the availability of a reference database in `--dbfolder`, EricScript
would execute an R script (`lib/CheckDB.R`) which would write into a file
`$ericscriptfolder/lib/data/_resources/.flag.dbexists` a code value of **0**
(no existing db) or **1** (existing db). This code was then read from the main
Perl wrapper script `ericscript.pl` to finally continue or quit.

When using singularity containers for EricScript, trying to write to `.flag.dbexists`
would make the execution break as singularity containers are often used in a
read-only mode. Here, we have disabled this behaviour. `.flag.dbexists` will
now always exist in the mentioned location and always contain the value **1**.
`lib/CheckDB.R` now exits with an error code directly propagated to and interpreted
by the main wrapper script.

For the same reason all checks relative to bwa version in `lib/CheckDB.R` (for a
potential auto-update of index files) have been removed.
