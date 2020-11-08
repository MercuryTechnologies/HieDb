# HIE DB - A tool to index and query .hie files

## Compiling

### Prerequisites

- Recent version of GHC 8.8/HEAD which includes support for .hie files
- cabal >= 2.4.1.0

### Procedure

```
$ cabal new-configure -w <ghc-binary> --allow-newer
$ cabal new-build
```

## Usage

### Generating .hie files

Compile any package with ghc options `-fwrite-ide-info` and optionally,
`-hiedir <dir>`. This will generate `.hie` files and save them in `<dir>`

### Indexing

```
$ hiedb -D <db-loc> index <hiedir>
```

You can omit `<db-loc>`, in which case it will default to the environment variable
`HIEDB`, or if that is not defined, `$XDG_DATA_DIR/default_$DBVERSION.hiedb`

### Querying

- Looking up references for a name(value/data constructor):
  ```
  $ hiedb -D <db-loc> name-refs <NAME> [MODULE]
  ```
- Looking up references for a type:
  ```
  $ hiedb -D <db-loc> type-refs <NAME> [MODULE]
  ```

`MODULE` is the module the name was originaly defined in.

- Looking up references for a symbol at a particular location:
  ```
  $ hiedb -D -<db-loc> point-refs (-f|--hiefile HIEFILE) SLINE SCOL [ELINE] [ECOL]  
  $ hiedb -D -<db-loc> point-refs MODULE [-u|--unit-id UNITID] SLINE SCOL [ELINE] [ECOL]
  ```

You can either lookup references for a Module that is already indexed,
or lookup references for a point in a .hie file directly, which will be
(re)indexed.
