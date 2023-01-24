# <p >mseedscan2dali 
###  Scan miniSEED files and send to DataLink server</p>

1. [Name](#)
1. [Synopsis](#synopsis)
1. [Description](#description)
1. [Options](#options)
1. [Notes](#notes)
1. [Author](#author)

## <a id='synopsis'>Synopsis</a>

<pre >
mseedscan2dali [options] [-d dir] ... ORB
</pre>

## <a id='description'>Description</a>

<p ><b>mseedscan2dali</b> recursively scans directories and sends any miniSEED data records it finds to a DataLink server.  During each pass of the directory tree new files are discovered and previously seen files are checked for appended records.</p>

<p >See the <b>NOTES</b> section for more details.</p>

## <a id='options'>Options</a>

<b>-V</b>

<p style="padding-left: 30px;">Print the program version and exit.</p>

<b>-h</b>

<p style="padding-left: 30px;">Print the program help/usage and exit.</p>

<b>-v</b>

<p style="padding-left: 30px;">Be more verbose.  This flag can be used multiple times ("-v -v" or "-vv") for more verbosity.</p>

<b>-n</b>

<p style="padding-left: 30px;">During the initial scan of files do not send data to the ORB from files not found in the state file.  In other words, skip the data records in any newly discovered file during the initial scan.  This is useful for getting the scanning back up to the current state of the files quickly.  As this option has the potential to skip data it should be used with caution.</p>

<b>-z</b>

<p style="padding-left: 30px;">During the initial scan of files do not send data to the ORB from any files if a state file is specified but not found.  In other words, skip all data records discovered during the initial scan.  This is useful for getting the scanning back up to the current state of the files quickly.  As this option has the potential to skip data it should be used with caution.</p>

<b>-1</b>

<p style="padding-left: 30px;">Quit after the first scan of all target directories.</p>

<b>-L </b><u>scans</u>

<p style="padding-left: 30px;">Print I/O statistics after the specified number of <u>scans</u>. Essentially this is a count of the number of files processed, records read from the files and written to the ORB.</p>

<b>-BL </b><u>days</u>

<p style="padding-left: 30px;">Skip files with a BUD file name scheme where the file name indicates that the data is more then \fIdays\R old.  When reading BUD-style files this saves stat() calls on many files that are older than desired, thus reducing overall system I/O.</p>

<b>-s </b><u>secs</u>

<p style="padding-left: 30px;">Specify interval to sleep in-between scans in seconds, default is none.</p>

<b>-sz </b><u>secs</u>

<p style="padding-left: 30px;">Specify interval to sleep in-between scans (in seconds) when no records were read during the previous scan, default is none.</p>

<b>-i </b><u>delay</u>

<p style="padding-left: 30px;">Check idle files every 'delay' intervals, default is 60 intervals. See <b>NOTES</b> section for a description of idle files.</p>

<b>-I </b><u>secs</u>

<p style="padding-left: 30px;">Treat a file as idle when it has not been modified for this many seconds, default is 7200 seconds.  See <b>NOTES</b> section for more a description of idle files.</p>

<b>-Q </b><u>secs</u>

<p style="padding-left: 30px;">Treat a file as quiet when it has not been modified for this many seconds, default is to never consider files quiet.  Warning, quiet files will never be checked again.  See <b>NOTES</b> section for a description of quiet files.</p>

<b>-T </b><u>nsecs</u>

<p style="padding-left: 30px;">Specify the number of nanoseconds to sleep after sending each miniSEED record to the ORB, default is 100 nanoseconds.</p>

<b>-f </b><u>max</u>

<p style="padding-left: 30px;">Specify the maximum number of records to read from a give file during a scan, default is 100 records, 0 to disable.  This limit keeps the program from dumping the entire contents of a file with many new records into the ORB at once.  This option will not cause any records to be skipped.</p>

<b>-r </b><u>level</u>

<p style="padding-left: 30px;">Specify the maximum number of directories to recurse into, default is 3 levels.</p>

<b>-M </b><u>match</u>

<p style="padding-left: 30px;">Specify a regular expression that must match any given file name for it to be processed.  File names are relative to the base directory under which they are found.  By default all files found are processed.</p>

<b>-R </b><u>reject</u>

<p style="padding-left: 30px;">Specify a regular expression that must <i>not</i> match any given file name for it to be processed.  File names are relative to the base directory under which they are found.  By default all files found are processed.</p>

<b>-S </b><u>statefile[:int]</u>

<p style="padding-left: 30px;">If a <u>statefile</u> is specified, the state of the program will be saved during an orderly shutdown.  On startup, the program will attempt to read the specified <u>statefile</u> allowing it to start scanning files where it left off.  Intermediate state saves can be controlled with the <u>:int</u> value which specifies the interval in seconds at which to save the state information, default interval is 300 seconds.</p>

<b></b><u>ringserver</u>

<p style="padding-left: 30px;">A required argument.  Specifies the location of the DataLink ringserver in [host]:[port] format.  If 'host' is omitted 'localhost' is assumed, if 'port' is omitted port 16000 is assumed.</p>

## <a id='notes'>Notes</a>

<p >This program is inteded to be run continuously on data files which might be growing (i.e. data records are being appended).  The program will not perform any kind of sorting, data records are forwarded as the files they are contained in are discovered.  In order to feed data in time-order to the ringserver the program must discover the files in the proper order.  In a real-time collection system writing miniSEED files as the data is collected this characteristic is a natural coincidence.</p>

<p >The file scanning logic employs the concept of active, idle and quiet files.  Files that have not been modified for specified amounts of time are considered idle or quiet.  The idle files will not be checked on every scan, instead a specified number of scans will be skipped for each idle file.  The quiet files will never be checked ever again.</p>

<p >Any files discovered that did not contain any miniSEED records are logged and the file is ignored during any subsequent scans.</p>

<p >Broken symbolic links are quietly skipped.  If they are eventually re-connected to something the something will be scanned as expected.</p>

<p >The directory separator is assumed to be '/'.</p>

<p >Currently, this program will only work with 512-byte miniSEED records.</p>

<p >Upon receiving a SIGUSR1 signal the program will print the current file list to standard error.</p>

## <a id='author'>Author</a>

<pre >
Chad Trabant
IRIS Data Management Center
</pre>


(man page 2008/01/17)
