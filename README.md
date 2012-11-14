pass
===

Perl Answer Set Solver. (c) 2012, Gatlin Johnson <rokenrol@gmail.com>

0. usage
---

    $> cp config.sample config
    $> vim config
    $> pass classdefinition.cl

1. What?
---

`pass` is a service that solves answer set programs. It relies on [Redis][1]
and [clingo][1] being installed on the same system.

Essentially `pass` wraps `clingo` and turns it into a forking network service.
Redis is used as a common incoming job queue so that an arbitrary number of
`pass` instances can work simultaneously and (probably) load balance.

Instance definitions are put in a special Redis list in YAML format. The YAML
document contains

* `command`: currently, the only value is "solve" but there might be other
  commands in the future.

* `instance`: the Answer Set Prolog code which defines the problem *instance*.
  What's fed into pass at startup is merely the class definition. Obviously,
  all of the problem could be sent in as the instance but this makes generating
  the prolog that much simpler and less redundant.

* `id`: a unique id supplied by the client. `pass` will use this to store the
  results in Redis and the client will know to monitor that queue.

2. Why?
---

We want to build robust web services on top of `clingo` to solve more and
interesting problems. Our long term goal is to create an answer set solver from
scratch. In the short term, though, we have a tool which works and we can
server our way out of the problem for some time. `pass` is that duct tape, for
now.

3. Problems
---

See the Issues page on github.

[1]: http://redis.io
[2]: http://potassco.sourceforge.net
