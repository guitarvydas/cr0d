---
What is Kagi?
* paid search engine
* est. 15,000 customers
* 70K LoC, Crystal, 3 years

---
Overview
* 1 novel structured concurrency
* 2 lightning quick tips

---
Classic Diagram Dilemma
* Engineer draw diagrams to communicate complex systems
* The diagrams slowly drift away from the truth
* What if diagrams could be the source of truth?


---
Abstraction
* FIFO queue on each end of a black box
* Mesage type: tag + data
* Routing table: From->To map for Messages 

---
4
Realization
* draw.io
* Imagine a syntax
* Export to XML
* Compile XML to crystal data structures
* Runtime

---
Syntax 5
* Component: rectangle
* Port: rhombus
* Connection: arrow

---
6
__screen shot of diagrammatic code__

---
7
Component Interface
* `def handle(msg)`
* `send(...)`
---
8
Component Registry
* map name:template
---
9
Runtime
* runtime instantiation of tree of Components
* run Component in a fiber
* when done, empty its output queue via routing table
* repeat

---
10
Result
* High level explanation of architecture, no messy details
* diagrams are the code - no more out-of-sync
* Palette of code to pick from, usable by other developers
* change architecture by moving rectangles and arrows on diagrams
* runtime less than 200 LOC 
* Development is playful again
* Debugging Tooling
* Hot Loading

---
11
Pioneers & References
* FBP (1970s), J. P. Morrison, Canadian banking software
* Statecharts (1987), David Harel
* DRAKON (1996), part of Buran space program
* 0d (2022-present) Paul Tarvydas

---
12
Tips & Tricks - Jinja
---
13
Tips & Tricks - Config Panel
---
14
Tips & Tricks - Debug Pages
---
15
Tips & Tricks - Replay Testing
---
16
Tips & Tricks - DB Practices
---
17
Tips & Tricks - Self Profiling
---










3 Easy Steps

Step 1: Tweak our black box abstraction
Stick a FIFO queue on each end
Step 2: Define a mesage type as a tag + data
Step 3: Create a routing table
Describes which queues to send messages to as they become available
Done!
... 4 Easy Steps (Ok, maybe more)

We still need something to draw diagrams with
draw.io
(We picked this one)
Excalidraw
Imagine a syntax
Boxes for components
Some shapes for ports
Arrows for connections
(show images)
Export to XML
Write an interpreter
XML parser in std 👍
"Pattern match" on the diagram tree
The Runtime

Component interface
def handle(msg)
Quick access to "hot" stuff...
@ctx.in/out; request/response
@ctx.q; query
Component registry
Hash table of component name to an instance
Load the diagram to construct the routing table
We do this once at runtime(*)
Request flow
Create the first message
Route it to the first component's input queue
Run the component in a fiber
Once it's done, empty its output queue
Route those messages to the next component
(Repeat until network is done)
Optimizations
Graph transformations
Pre-allocate a bunch of stuff
FIFOs
Optimized FIFO "arena" based on SOA memory model
2.5-3.5x faster than std Deque for our use case
TBD: Re-using fibers in linear pipelines
In practice though, this is not far off from what we had been "hardcoding" by hand!
Demo (120s)

...

The Result

Design
High level starting point for architecture design away from messy details
But it's authoritative! No more out-of-sync.
Agree on how a system should work before implementing anything; then just fill in the blanks.
Documentation
Anyone can derive meaning from the diagram without knowing anything about our code
draw.io allows us to include rich media in the diagram itself
Code re-use.. Control-flow re-use!
Create a "palette" of code to pick from
Wrap control flow up in containers, re-use them anywhere
Black box abstraction makes everything highly flexible, decoupled, and concurrent by default
Multiple targets
You can write a runtime in less than 200 LOC
C, Odin, Go, Python
Interpreter only needs to be implemented once
Share diagrams between pieces of infra
Port systems to another language, keeping the exact same control flow
Development is playful again
Diagram can be hot-reloaded for development
Play with different routings instantly without recompiling
Quickly experiment with different network configurations
Debugging tooling
Diagram can be hot-reloaded in production
Upload your own personal diagram to test different networks on staging or production environments, isolated to your account
Use the same debugging tooling as local dev
Update the "primary" diagram used by the server to fix things or route around buggy components
Create new syntaxes to express whatever we would like
Pioneers & References

FBP (1970s)
J. P. Morrison
Canadian banking software
Statecharts (1987)
David Harel
State machines
HSMs, Orthogonal/concurrent states
DRAKON (1996)
Visual programming and modeling language developed as part of Buran space program
0d (2022-present)
Paul Tarvydas
PART 2

(Lightning section of tricks we picked up along the way)

jinja
ECR came with its costs at compile time
runtime cost of something like jinja well worth it in terms of dev productivity
config panel
simple schema for auditable, versioned config changes
debug pages
approx. 40 endpoints of various things
data pages
utilities
bare html, minimal css
really quick to make, great for quick insights / "levers" to pull
RPC
replay testing
(not currently done with crystal)
record browser actions, save to file
play them back over webdriver
diff the screenshots, ping CI when images fall below tolerance levels
cheap way to create integration tests
if test falls out of date, just delete it & record a new one
good for catching bugs, & verifying new features, and supporting refactors
db practices
no ORM
driver is fantastic
cloud sql metrics
local read replicas
at our currenty scale, we essentially use the cheapest cloud sql option, with 20-30% resource usage
self profiling
timelines
simple way to review what a complex system decides to do
