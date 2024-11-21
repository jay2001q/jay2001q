Pinba 1.2.0      09 Sep 2016
----------------------------
- Added pinba_log_level configuration directive to configure log verbosity.
- Histograms are now dynamic and can be configured in my.cnf using pinba_histogram_size directive. 
- Fixed FreeBSD compatibility.
- Added multiple reader threads with separate data pools to speed up reading and decoding incoming data.
- Added active_reports table to track reports that produce most CPU load.
- Switched to Google dense/sparse hash instead of Judy where possible.
- Removed libevent dependency.
- Optimized locking and increased overall performance.

Pinba 1.1.0      12 Feb 2015
----------------------------
- Added schema field (mostly to distinguish between HTTPS/HTTP traffic).
- Added memory footprint field to request.
- Added timestamp field to request.
- Added nested messages to allow packet data 'buffering' without reinventing data streaming.
- Added request tags (used to filter requests).
- Added tagN_reports supporting arbitrary number of timer tags.
- Added 'histograms' - a way to look beyond the 'average' numbers.
- Added support of percentiles in reports.
- Added rusage to timers and timer reports.

- Greatly improved locking and performance.
- All reports now support conditions (min_time/max_time and request tags).
- All CPU-intensive operations with package data are now done using the thread pool (spreading across all available CPU cores).
- Switched to Protobuf-C version to improve performance (this also removes one more library dependency).
- Fixed bug in tag report generation (some data was added, but never deleted from the report).

- Dropped pinba_tag_report_timeout (make sure to remove it from my.cnf, or MySQL won't start!).

Pinba 1.0.0      17 Aug 2012
----------------------------
- Added tag_report2 and tag2_report2 mostly replicating their first versions, 
  but with additional grouping by server name and hostname. (Denis Yeldandi)

- Implemented thread pool.
  * The threads are allocated at the startup and killed only on shutdown;
  * The number of threads is equal to the number of CPUs available (or 8 by default);
  * Incoming GPB packages are decoded using the thread pool;
  * Tag reports are updated using the thread pool.

- Implemented timer pool with caching.
  * Timers are reused in order to avoid unnecessary malloc()'s.
  * Reimplemented reading from the 'timers' table, simplified the code.

  Together with using the thread pool, this means a huge performance gain, 
  though Pinba is now a lot more memory hungry since none of the timers are ever freed.

- Fixed reading from timertag table by key.
- Fixed race condition when reading timer tag values.
- Fixed MySQL 5.5 build.

- Fixed reading from timertag table by key.
- Fixed race condition when reading timer tag values.
- Fixed MySQL 5.5 build.

Pinba 0.0.6      26 Nov 2010
----------------------------
- Added conditional tag reports. 

- Added missing primary key to timer table.
- Increased default hostname size to 32 chars.
- Changed protobuf proto to use lite runtime (and link against libprotobuf-lite).
- Removed wrong index on tag_id in timertag table.
- Fixed build with MySQL 5.5 (hopefully, as there is no way to know what they'll break next).
- Fixed potential problem with LDFLAGS in configure. (Andrew Sitnikov)

Pinba 0.0.5      19 Oct 2009
----------------------------
- Added rusage to timers (not aggregated on server (yet?)).

- Fixed possible crash caused by long script names.
- Disabled query cache for Pinba engine.

Pinba 0.0.4      26 Aug 2009
----------------------------
- Added HTTP response status to the response data.
- Added support of Google Protocol Buffers 2.1.0+.

- Improved configure checks for MySQL sources and libevent.
- Fixed build on MacOS X.
- Fixed build on FreeBSD.
- Fixed build with static libevent of latest versions.

Pinba 0.0.3      04 May 2009
----------------------------
Initial release.
