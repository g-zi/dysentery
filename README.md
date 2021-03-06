# dysentery
Exploring ways to participate in a Pioneer Pro DJ Link network.

[![License](https://img.shields.io/badge/License-Eclipse%20Public%20License%201.0-blue.svg)](#license)

### Status

This is in no way a sanctioned implementation of the protocols. It
should be obvious, but:

> Use at your own risk!

While these techniques appear to work for us so far, there are many
gaps in our knowledge, and things could change at any time with new
releases of hardware or even firmware updates from Pioneer.

That said, if you find anything wrong, or discover anything new,
*please* open an issue or submit a pull request so we can all improve
our understanding together.

Dysentery is currently being developed as a
[Clojure](http://clojure.org/) library, because I find that to be the
most powerful development environment available to me at the moment.
When released, it will be usable as a standard Java package on Maven
Central, but for now if you want to play with it you'll need to learn
a little bit about Clojure.

The only feature implemented so far is the ability to watch for DJ
Link traffic on all your network interfaces, and tell you what devices
have been noticed, and the local and broadcast addresses you will want
to use when creating a virtual CDJ device to participate in that
network. Here is an example of trying that out by running Dysentery as
an executable jar on my network at home:

```
> java -jar target/dysentery.jar
Looking for DJ Link devices...
Found:
   CDJ-2000nexus /172.16.42.5
   DJM-2000nexus /172.16.42.3
   CDJ-2000nexus /172.16.42.4

To communicate create a virtual CDJ with address /172.16.42.2
and use broadcast address /172.16.42.255
```

To build it yourself, and play with it interactively, you will need to
clone this repository and install [Leiningen](http://leiningen.org).
Then, within the directory into which you cloned the repo, you can
type `lein repl` to enter a Clojure Read-Eval-Print-Loop with the
project loaded:

```
> lein repl
nREPL server started on port 53806 on host 127.0.0.1 - nrepl://127.0.0.1:53806
REPL-y 0.3.7, nREPL 0.2.12
Clojure 1.8.0
Java HotSpot(TM) 64-Bit Server VM 1.8.0_77-b03
dysentery loaded.
dysentery.core=> 
```

At that point, you can evalute Clojure expressions:

```clojure
(find-devices)
;; => Looking for DJ Link devices...
;; => Found:
;; =>   CDJ-2000nexus /172.16.42.5
;; =>   DJM-2000nexus /172.16.42.3
;; =>   CDJ-2000nexus /172.16.42.4
;; =>
;; => To communicate create a virtual CDJ with address /172.16.42.2
;; => and use broadcast address /172.16.42.255
nil
```

To build the executable jar:

```
> lein uberjar
Compiling dysentery.core
Compiling dysentery.finder
Compiling dysentery.util
Created /Users/james/git/dysentery/target/dysentery-0.1.0-SNAPSHOT.jar
Created /Users/james/git/dysentery/target/dysentery.jar
```

There will soon be much more here, and much more documentation, if all
goes as planned.

### History

This research began in the summer of 2015 as I was trying to figure
out a reliable way to synchronize
[Afterglow](https://github.com/brunchboy/afterglow#afterglow) light
shows with performances on my CDJs. I broke out
[Wireshark](https://www.wireshark.org) and after staring at packet
captures over a weekend, I was able to identify how to track the
current BPM and beat locations by passively watching broadcast
traffic, which was my main goal. I still could not get a lock on where
the down beat fell, because I could not tell which player was the
Master.

In the spring of 2016 I saw a posting on the original
[VJ Forums thread](http://vjforums.info/threads/cdj-2000-ethernet-protocol-for-live-bpm-sync.39265/page-2#post-295258)
where we had been discussing this, announcing that
[Diogo Santos](mailto:diogommsantos@gmail.com) had made an important
breakthrough. By broadcasting packets that pretended to be a CDJ, his
software was able to get the other players to start sending it more
details, including information I had not been able to find in other
ways. He was kind enough to share his code, and that was the impetus
behind starting this project, to consolidate what people are learning
about this protocol, and make it available for other projects to
benefit from.

### Why Dysentery?

The name of this project is a reference to one of the infamous hazards faced in
[The Oregon Trail](https://en.wikipedia.org/wiki/The_Oregon_Trail_%28video_game%29),
a game which helped many students in the eighties and nineties understand what life
was like for pioneers exploring the American West. Since we are exploring the
protocol used by Pioneer gear, it seemed at least slightly appropriate. And, ok, I
have a hard time resisting forced puns. Let's hope none of us see:

[![You have died of dysentery](http://www.strangeloopgames.com/wp-content/uploads/2014/01/Dysentary.png)](http://www.strangeloopgames.com/you-have-died-of-dysentery-how-games-will-revolutionize-education/)

## License

<img align="right" alt="Deep Symmetry" src="doc/assets/DS-logo-bw-200-padded-left.png">
Copyright © 2016 [Deep Symmetry, LLC](http://deepsymmetry.org)

Distributed under the
[Eclipse Public License 1.0](http://opensource.org/licenses/eclipse-1.0.php),
the same as Clojure. By using this software in any fashion, you are
agreeing to be bound by the terms of this license. You must not remove
this notice, or any other, from this software. A copy of the license
can be found in
[LICENSE](https://rawgit.com/brunchboy/dysentery/master/LICENSE)
within this project.
