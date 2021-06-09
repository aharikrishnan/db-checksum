# db-checksum

Simple script to verify if primary and secondary databases are in sync

## Usage

```bash
  $ ./db-checksum -h

   ./db-checksum --database instant_gem \
    --primary "-D instant_gem -uroot -proot -h 127.0.0.1 --ssl-mode=DISABLED" \
    --secondary "-D instant_gem -uroot -proot -h 127.0.0.1 --ssl-mode=DISABLED" \
    --limit 1000 --offset 2000  --tables "'sites', 'purchases'"

  Required parameters:
     -p, --primary       mysql connection string to connect primary DB
     -s, --secondary     mysql connection string to connect secondary DB
     -D, --database      database name

  Optional arguments:
     -T, --tables=tables  table names, as quoted CSV
                          eg., "'sites', 'purchases'"
     -l, --limit=limit    limit records to scan for checksum
     -o, --offset=offset  read records from offset
                          If the value is negative, reads last 'n' records
     -m, --mode=mode      mode of running checksum, default is row
                          available options are [ row | chunk | table ]
                          row: picks 'limit' records of a table and do row wise md5
                          chunk: picks 'limit' records of a table and do grouped md5
                          table: does table checksum
     -h, --help           show this help message and exit
     -v, --verbose        increase the verbosity of the bash script
     --recent, -r         does checksum for recent records (order by id desc)
     --dry-run            do a dry run, dont change any files

```

## Functional requirements

1. Correctness
2. Robust. support for table whitelisting, row / chunk/ table based checksum
3. Hackable.
4. Debuggable. the intermediate files stores the SQL for verification
5. Minimal dependency (mysql client & shell)
6. Interoperability
7. Performance

## Dependencies

- mysql client
- shell (tested with bash)
