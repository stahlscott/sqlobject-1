++++
News
++++

.. contents:: Contents:
   :backlinks: none

SQLObject 0.15.1
================

Released 22 Mar 2011.

* A bug was fixed in MSSQLConnection.

* A minor bug was fixed in sqlbuilder.Union.

SQLObject 0.15.0
================

Released 6 Dec 2010.

Features & Interface
--------------------

* Major API change: all signals are sent with the instance (or the class)
  as the first parameter. The following signals were changed:
  RowCreateSignal, RowCreatedSignal, DeleteColumnSignal.

* Major API change: post-processing functions for all signals are called
  with the instance as the first parameter. The following signals were
  changed: RowUpdatedSignal, RowDestroySignal, RowDestroyedSignal.

SQLObject 0.14.2
================

* A minor bug was fixed in sqlbuilder.Union.

SQLObject 0.14.1
================

Released 15 Oct 2010.

* A bugfix was ported from `SQLObject 0.13.1`_.

SQLObject 0.14.0
================

Released 10 Oct 2010.

Features & Interface
--------------------

* The lists of columns/indices/joins are now sorted according to the order
  of declaration.

* ``validator2`` was added to all columns; it is inserted at the beginning
  of the list of validators, i.e. its ``from_python()`` method is called
  first, ``to_python()`` is called last, after all validators in the list.

* SQLiteConnection's parameter ``use_table_info`` became boolean with default
  value True; this means the default schema parser is now based on ``PRAGMA
  table_info()``.

* Major API change: attribute ``dirty`` was moved to sqlmeta.

SQLObject 0.13.1
================

Released 15 Oct 2010.

* A bug was fixed in a subtle case when a per-instance connection is not
  passed to validators.

SQLObject 0.13.0
================

Released 11 Aug 2010.

Features & Interface
--------------------

* SQLObject instances that don't have a per-instance connection can be
  pickled and unpickled.

* Validators became stricter: StringCol and UnicodeCol now accept only str,
  unicode or an instance of a class that implements __unicode__ (but not
  __str__ because every object has a __str__ method); BoolCol accepts only
  bool or int or an instance of a class that implements __nonzero__; IntCol
  accepts int, long or an instance of a class that implements __int__ or
  __long__; FloatCol accepts float, int, long or an instance of a class
  that implements __float__, __int__ or __long__.

* Added a connection class for rdbhost.com (commercial Postgres-over-Web
  service).

Small Features
--------------

* Added TimedeltaCol; currently it's only implemented on PostgreSQL as an
  INTERVAL type.

* Do not pollute the base sqlmeta class to allow Style to set idName. In
  the case of inherited idName inherited value takes precedence; to allow
  Style to set idName reset inherited idName to None.

* Better handling of circular dependencies in sqlobject-admin -
  do not include the class in the list of other classes.

* Renamed db_encoding to dbEncoding in UnicodeStringValidator.

* A new parameter ``sslmode`` was added to PostgresConnection.

* Removed SQLValidator - its attemptConvert was never called because in
  FormEncode it's named attempt_convert.

SQLObject 0.12.5
================

* ``backend`` parameter was renamed to ``driver``. To preserve backward
  compatibility ``backend`` is still recognized and will be recognized
  for some time.

SQLObject 0.12.4
================

Released 5 May 2010.

* Bugs were fixed in calling from_python().

* A bugfix was ported from `SQLObject 0.11.6`_.

SQLObject 0.12.3
================

Released 11 Apr 2010.

* A bugfix ported from `SQLObject 0.11.5`_.

SQLObject 0.12.2
================

Released 4 Mar 2010.

* Bugfixes ported from `SQLObject 0.11.4`_.

SQLObject 0.12.1
================

Released 8 Jan 2010.

* Fixed three bugs in PostgresConnection.

* A number of changes ported from `SQLObject 0.11.3`_.

SQLObject 0.12
==============

Released 20 Oct 2009.

Features & Interface
--------------------

* .selectBy(), .deleteBy() and .by*() methods pass all values through
  .from_python(), not only unicode.

* The user can choose a DB API driver for SQLite by using a ``backend``
  parameter in DB URI or SQLiteConnection that can be a comma-separated
  list of backend names. Possible backends are: ``pysqlite2`` (alias
  ``sqlite2``), ``sqlite3``, ``sqlite`` (alias ``sqlite1``). Default is
  to test pysqlite2, sqlite3 and sqlite in that order.

* The user can choose a DB API driver for PostgreSQL by using a ``backend``
  parameter in DB URI or PostgresConnection that can be a comma-separated
  list of backend names. Possible backends are: ``psycopg2``, ``psycopg1``,
  ``psycopg`` (tries psycopg2 and psycopg1), ``pygresql``. Default is
  ``psycopg``.
  WARNING: API change! PostgresConnection's parameter
  ``usePygresql`` is now replaced with ``backend=pygresql``.

* The user can choose a DB API driver for MSSQL by using a ``backend``
  parameter in DB URI or MSSQLConnection that can be a comma-separated
  list of backend names. Possible backends are: ``adodb`` (alias
  ``adodbapi``) and ``pymssql``. Default is to test adodbapi and pymssql
  in that order.

* alternateMethodName is defined for all unique fields, not only alternateID;
  this makes SQLObject create .by*() methods for all unique fields.

* SET client_encoding for PostgreSQL to the value of ``charset`` parameter
  in DB URI or PostgresConnection.

* TimestampCol() can be instantiated without any defaults, in this case
  default will be None (good default for TIMESTAMP columns in MySQL).

Small Features
--------------

* Imported DB API drivers are stored as connection instance variables, not
  in global variables; this allows to use different DB API drivers at the
  same time; for example, PySQLite2 and sqlite3.

* Removed all deprecated attributes and functions.

* Removed the last remained string exceptions.

SQLObject 0.11.6
================

Released 5 May 2010.

* A bug was fixed in SQLiteConnection.columnsFromSchema(): pass None as
  size/precision to DecimalCol; DecimalCol doesn't allow default values (to
  force user to pass meaningful values); but columnsFromSchema() doesn't
  implement proper parsing of column details.

SQLObject 0.11.5
================

Released 11 Apr 2010.

* Fixed a bug in replacing _connection in a case when no previous
  _connection has been set.

SQLObject 0.11.4
================

Released 4 Mar 2010.

* Fixed a bug in inheritance - if creation of the row failed and if the
  connection is not a transaction and is in autocommit mode - remove
  parent row(s).

* Do not set _perConnection flag if get() or _init() is passed the same
  connection; this is often the case with select().

SQLObject 0.11.3
================

Released 8 Jan 2010.

* Fixed a bug in col.py and dbconnection.py - if dbEncoding is None suppose
  it's 'ascii'.

* Fixed a bug in FirebirdConnection.

* The cache culling algorithm was enhanced to eliminate memory leaks by
  removing references to dead objects; tested on a website that runs around
  4 million requests a day.

SQLObject 0.11.2
================

Released 30 Sep 2009.

* Fixed a bug in logging to console - convert unicode to str.

* Fixed an obscure bug in ConnectionHub triggered by an SQLObject class
  whose instances can be coerced to boolean False.

SQLObject 0.11.1
================

Released 20 Sep 2009.

* Fixed a bug: Sybase tables with identity column fire two identity_inserts.

* Fixed a bug: q.startswith(), q.contains() and q.endswith() escape (with a
  backslash) all special characters (backslashes, underscores and percent
  signs).

SQLObject 0.11.0
================

Released 12 Aug 2009.

Features & Interface
--------------------

* Dropped support for Python 2.3. The minimal version of Python for
  SQLObject is 2.4 now.

* Dropped support for PostgreSQL 7.2. The minimal supported version of
  PostgreSQL is 7.3 now.

* New magic attribute ``j`` similar to ``q`` was added that
  automagically does join with the other table in MultipleJoin or
  RelatedJoin.

* SQLObject can now create and drop a database in MySQL, PostgreSQL, SQLite
  and Firebird/Interbase.

* Added some support for schemas in PostgreSQL.

* Added DecimalStringCol - similar to DecimalCol but stores data as strings
  to work around problems in some drivers and type affinity problem in
  SQLite.

* Added sqlobject.include.hashcol.HashCol - a column type that automatically
  hashes anything going into it, and returns out an object that hashes
  anything being compared to itself. Basically, it's good for really simple
  one-way password fields, and it even supports the assignment of None to
  indicate no password set. By default, it uses the md5 library for
  hashing, but this can be changed in a HashCol definition.

* RowDestroyedSignal and RowUpdatedSignal were added.

Minor features
--------------

* Use reversed() in manager/command.py instead of .__reversed__().

* Minor change in logging to console - logger no longer stores the output
  file, it gets the file from module sys every time by name; this means
  logging will use new sys.stdout (or stderr) in case the user changed
  them.

* Changed the order of testing of SQLite modules - look for external
  PySQLite2 before sqlite3.

`Older news`__

.. __: News3.html

.. image:: https://sourceforge.net/sflogo.php?group_id=74338&type=10
   :target: https://sourceforge.net/projects/sqlobject
   :class: noborder
   :align: center
   :height: 15
   :width: 80
   :alt: Get SQLObject at SourceForge.net. Fast, secure and Free Open Source software downloads
