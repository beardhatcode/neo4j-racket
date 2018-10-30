# Neo4j driver for Racket

A very rudimentary driver for [Neo4j](https://neo4j.com).

This uses the [Transactional Cypher HTTP endpoint](https://neo4j.com/docs/developer-manual/3.4/http-api/) of Neo4j

## Usage

```racket
#lang racket

(require "./Neo4j.rkt")

(doNeo4j "MATCH (e) WHERE e.term = \"b\" RETURN ID(e)")
; #hasheq((errors . ()) (results . (#hasheq((columns . (ID(e))) (data . (#hasheq((meta . (null)) (row . (9)))))))))

(doNeo4jP
  "MATCH (e) WHERE e.term = {lol} RETURN ID(e)"
  '#hash((lol . "b"))
)
; #hasheq((errors . ()) (results . (#hasheq((columns . (ID(e))) (data . (#hasheq((meta . (null)) (row . (9)))))))))

```

## Configuration

### Ports and location

The default ports and location for localhost are set (`http://localhost:7474`). You can change these in [here](/Neo4j.rkt#L10-L15).

### Authorization

To authorize the current user, you must send the username and password:

```
"Authorization: Basic bmVvNGo6bmVvNGpwYXNz"
```

The above header should hold the 'base64' encoded version of 'username:password'. The example above uses 'neo4j:neo4jpass'. It is obtained with

```bash
$ echo -n "neo4j:neo4jpass" | base64
bmVvNGo6bmVvNGpwYXNz
```

**Note**: The current implementation uses HTTP to log in, Basic Auth is not at all secure with plain HTTP.

## Security

You can use HTTPS by setting `#:port 7473` and `#:ssl? #t`. But this will make it much slower. It might work fast once we don't create a new HTTP session for every query.

## Contribute

Want to help out? At the moment this is a very rudimentary implementation of a driver. A lot of things can be improved:

- [ ] Don't close the HTTP session
- [ ] Allow multiple statements to be sent at once in a session
- [ ] Handle line breaks
- [ ] A better output format
- [ ] ...

Fork and send a pull request.