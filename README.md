# Files Backend Benchmark

This experiment is designed to answer the question:
can performance be improved by changing the database to track directories instead of files?

## Experimental Setup

We shall compare 4 backends:

| Name | Description |
|------|-------------|
| Filesystem | Files are stored in directories on the filesystem directly. |
| Swift Storage | Files are stored by OpenStack Swift, and retrieved over an HTTP API. |
| Postgres: FPR | Files are stored in a database, one-file-per-row. |
| Postgres: DPR | Directories are stored in a database, one-directory-per-row. Multiple files under a directory are put into the same row. |

Example of `Postgres: FPR`:

| ID | File |
|----|------|
| 1  | fruit/pear.txt |
| 2  | fruit/apple.txt |
| 3  | veggie/celery.txt |

Example of `Postgres: DPR`:

| ID | Directory | Files |
|----|-----------|-------|
| 1  | fruit     | fruit.txt,pear.txt |
| 2  | veggie    | celery.txt         |

Databases are compared in 3 situations:

- small: 100 files per directory
- medium: 1,000 files per directory
- large: 10,000 files per directory

## Procedure

The command `create_data.py > paths.txt` outputs a list of paths, one per line, to a file called `paths.txt`.

## Benchmark: Insertion

Purpose: Evaluate write-performance of backends.

Action: Add each path in `path.txt` to the backend.

## Benchmark: Query

Purpose: Evaluate read-performance of backends.

Action: For each path in `path.txt`, check for the existence of `path` in the backend.
