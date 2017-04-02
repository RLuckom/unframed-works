## Portfolio

My Github profile includes 38 repositories. These are the ones to look at:

### [water-shape](https://github.com/RLuckom/water-shape)

Persistence and data access based on a simple schema.

Water-shape arose out of my desire for a simple way to autogenerate
database, server, and API clients in javascript. I started with an idea
for a very simple schema describing the data to be stored. I wanted the 
schema to be specific enough to be translated into a SQLite schema,
but simple enough to allow quick iteration. The best example of a schema
for reference is the [schema used in water-shape's test suite](https://github.com/RLuckom/water-shape/blob/master/spec/unit/testSchema.js). 

Water-shape the library is simply a collection of modules for turning
a water-shape schema object into a water-shape Data Manipulation Interface
(DMI) that can talk to a data store.

A water-shape DMI is simply a standardized tree-like object for CRUD 
operations on data. Its keys are the names of tables. Each key's value
is an object exposing asynchronous `save`, `update`, `delete`, `deleteByID`,
`list`, `getById`, and `search` functions. By calling 

       dmi.<tableName>.save(object, callback);

you save a record in the table, and are notified of success or failure
in `callback`.

On a server, the `sqlite-adapter` module can turn the schema into a file-
backed SQLite DMI object, and the `hapi-adapter` can use the schema and the
SQLite DMI to expose REST endpoints corresponding to the data represented
by the schema, backed by the database. On the client, the `request-adapter`
can take the schema and a base address and turn it into a DMI for use by
UI components.

The guarantee provided by water-shape is that, if your code is written
against water-shape DMI objects, *it doesn't matter what is actually backing
the DMI*. If you write a piece of code that uses the DMI to count how many
users you have in your `users` table, that code will run on the back-end,
where the DMI supplied might be an object accessing the database directly,
or on the front-end, where the DMI supplied is an object that makes REST
requests into a server. This allows you to write your utility code exactly
once, and not worry about maintaining separate libraries for separate
environments. This benefit is used in the tests for water-shape itself--the
core tests are implemented in [just one place](https://github.com/RLuckom/water-shape/blob/master/spec/unit/dataManipulationInterfaceTest.js), and run identically against
each kind of DMI.

Another benefit of consistent DMIs is the ability to create more sophisticated
behavior in a flexible, generic way. While working on my [bucket-brain](https://github.com/RLuckom/bucket-brain) project,
I wanted a way to query a sort of 'view' into the database without having to
make a ton of individual queries. Specifically, I wanted a JSON object that
selected a particular record and also included records referencing it in other
tables. So I wrote a module that takes the schema description of this `TREE` "table",
plus an existing DMI, and returns access methods for the data described in the 
schema description. Using this pattern, I also added the ability to include
validation logic in the schema itself, so that you can validate your data
identically wherever your code is running (subject to race conditions--obviously
front-end validation is best-effort only, to improve UX). I also added
optimizations for constructed tables in the API client layer; when the API
client is querying a `TREE` type table, it first tries to make a simple
`GET` request to the server. If the server knows about `TREE` tables, it will
be faster than the API client trying to make individual REST calls for each
piece of data required. If the server returns an error, the client falls back
on constructing the data one request at a time from the relevant tables.

This project is based around a simple specification for two kinds of JS objects;
the schema definition object and the output DMI. This design allows a huge amount
of extensibility in multiple dimensions:

 1. It would be easy to write a module that would construct a DMI based on a
    message queue, or a websocket, and all code written for a project would
    then work through a websocket or queue.
 2. It is easy to write custom types of data access based on CRUD operations;
    once they are implemented once, they are available in all environments.
 3. Any environment-specific optimization can be implemented only where it
    is relevant; the API client can try to be lazy where it would be more
    efficient for the DB to construct a view; you could also implement caching
    at any layer.

This is not enterprise, production-ready code. It works, and all success paths
are automatically tested by Travis on each check-in. But because I use it only
for my personal projects, I haven't seen fit to do all the work testing error
cases, writing documentation, etc. that I would if it was supporting a real
product. If I had been developing for a real product, I would not have written
this at all; I would have used an off-the-shelf, supported framework.

### [Bucket Brain](https://github.com/RLuckom/bucket-brain) ([Demo](https://rluckom.github.io/bucket-brain/))

Raspberry pi program to control pumps and lights for a hydroponics system.

The raspberry pi is an exciting platform for trying out embedded computing.
In this project, I wanted a minimalistic UI to turn lights and pumps on and
off on a schedule. Bucket Brain defines a minimal schema for representing
the GPIO pins on a raspberry pi and the scheduling primitives for duration-
based an wall-clock-based schedules. It consists of two back-end services
plus a minimal React-based UI. One of the back-end services is a server
that serves the minimal UI and provides a REST API to the SQLite database;
the other is a high-privilege process that controls the GPIO pins based on
the schedules saved in the database.

The main design consideration for Bucket Brain was to separate the GPIO control
process, which requires root privileges to write to the GPIOs, from the server
process, which must be open on port 80 (in a productized version more security
would be needed: https, authentication, etc. But those things can be layered 
onto the server process, whereas the process-separation had to be baked into
the design).

Another major consideration was code reusability. Since the back-end and
front-end are written in javascript, I didn't want to duplicate code for any
piece of functionality. I built the schema for the data I wanted to represent
as a JS object, and then built tools for autogenerating database, server, and
API client code based on that schema. The front-end and both back-end processes
use the same code for data access, including model validation (on the front-end,
the validation is obviously best-effort only, to provide quick user feedback;
all data modification is validated in the back end). In the end, I broke out
the data-interface tools into a separate repo and npm package called water-shape.
One nice result of this is that the UI's API client code is not specifically
tied to REST calls. I added a demo build target that uses an in-memory data
store rather than a server, allowing me to "deploy" a working version of
the UI on the github-pages page for the repo.

This code and its predecessors suceeded in keeping both tomato and pepper plants
alive in indoor hydroponics from seedling to harvest.

### [Decent-Chess](https://github.com/RLuckom/decent-chess) ([king](https://github.com/RLuckom/decent-chess/blob/master/king1.stl)) ([queen](https://github.com/RLuckom/decent-chess/blob/master/queen5.stl)) ([bishop](https://github.com/RLuckom/decent-chess/blob/master/bishop7.stl)) ([knight](https://github.com/RLuckom/decent-chess/blob/master/knight6.stl)) ([rook](https://github.com/RLuckom/decent-chess/blob/master/rook2.stl)) ([pawn](https://github.com/RLuckom/decent-chess/blob/master/pawn1.stl))

I designed a 3d-printable chess set for a Yankee Swap. I've printed about 3
complete sets so far. On a desktop browser, Github actually renders the STL
files, so you can see what the pieces look like.

All the pieces were designed in a combination of OpenScad and inkscape. The
OpenScad code is [here](https://github.com/RLuckom/decent-chess/blob/master/pawn.scad).

### [JsGameOfLife](https://github.com/RLuckom/jsGameOfLife) ([demo](http://rluckom.github.io/jsGameOfLife/))

Two versions of Conway's Game Of Life implemented in vanilla JS. The SVG version
came first but proved slow; the canvas version is fast enough for any size board
I've actually wanted.

### [network-vis](https://github.com/RLuckom/network-vis) ([demo](https://rluckom.github.io/network-vis/#/vis))

Fast prototype UI using a hive plot to display the source and destination of
NetFlow records organized by time.

### [I know True is this much](https://github.com/RLuckom/i-know-true-is-this-much)

Small set of python koans illustrating idiosyncracies of the language. Taken in
the aggregate, I probably spent multiple weeks in my first job learning the answers
to these puzzles the hard way.


### Others

Most of the other repos are either one-offs similar to something above, or abandoned,
or not quite polished enough even to use as examples. Among them are:

 * Circuit board layouts and code for a garden-monitoring device (getting my first custom circuit board printed was cool),
 * BeagleBone Black C and PRU assembly code for a POV LED display
 * A JS library for wax-on / wax-off display of before-and-after images, 
 * Vanilla JS implementation of SVG UI components.
 * Vanilla JS code to draw the Mandelbrot set.
 * Ansible scripts to deploy Wordpress on AWS.
 * Node.js solutions to some of the Matasano Crypto Challenges.
