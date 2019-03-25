Suppose I want to understand the "structure" of something. Just what
exactly does this mean?

It means, of course, that I want to make a simple picture of it, which
lets me grasp it as a whole.

And it means, too, that as far as possible, I want to paint this
simple picture out of as few elements as possible. The fewer elements
there are, the richer the relationships between them, and the more of
the picture lies in the "structure" of these relationships.

- Christopher Alexander, The Timeless Way of Building

## One Thousand Words

All documentation is an act of faith. The documentation-
writer's intent is that at some future moment, some other person will
have a question about the subject, discover the documention, read it,
and in it understand the answer to the question. If that process works
perfectly, the document writer won't know about it.

So, often without noticing it, we create user personas for our docs.
A developer in a hurry. A user without access to SSH or source code.
An auditor. A collaborator. Ourselves after we've moved to other projects.
We set down the things we imagine that reader will want to know, in the
way that we imagine that reader will find clearest. Most of the time
we can't make "closing the loop" a priority. Most of us don't work on
systems where every unit of documentation has a "was this helpful?"
opportunity for feedback. So the best we can do is to be mindful of
the assumptions and choices we are making and use our best judgment.

Common wisdom advocates for making pessimistic assumptions. Assume
that your reader is in a hurry. Assume that she does not want to become
familiar with the subject, and only wants enough information to solve
her immediate problem. Assume that they will get confused if you present too
much information. Assume that he expects a finished, production-ready
product for which the documentation is always accurate and up-to-date.

These assumptions have the advantage that they fit
a large number of external documentation readers, if we allow that
"external" has a big overlap with "in a hurry," "critical," "only looking for
superficial understanding" and "unfamiliar with the domain and easily confused."
From these assumptions we get some very effective types of documents.
Runbooks that an on-call engineer can follow to fix a problem she's not
familiar with. Slide decks that explain the central features and concepts
of a new technology to a wide audience. Compliance documents that accurately
describe the security properties of a system to an auditor.

There are also some common document types that make more optimistic assumptions
in one area or another. RFCs assume that their readers have time to read carefully
and a tolerance for complexity as the price of precision. Books on the
philosophy of design and team organization are written broadly, without
trying to anticipate and enumerate each situation a reader might encounter.
Man pages and API docs tend to avoid assumptions about the user's
needs and instead are structured based on the structure of their subject.

### Hypothesis 1: Optimistic assumptions correspond to higher bandwidth; pessimistic assumptions correspond to lower bandwidth
In this context, "optimistic" and "pessimistic" are meant as use-cases, not
value judgements. An "optimistic" assumption is an assumption that a
document author can expect a document reader to engage with a document
in a particular way. A "pessimistic" assumption is an assumption that
the reader will be _unwilling_ to engage with the document in a particular
way.

A runbook author should make mostly pessimistic assumptions about her reader.
The person most qualified to assess the quality of a runbook is the
on-call engineer from a different team who has just been woken up with
an alert at 3AM. As such, the author needs to provide a minimal but complete
decision tree for an engineer with no context to traverse. Other aspects
of the document may be dictated by the organization, such as where it
should be located and how it should be structured. Each pessimistic
assumption corresponds to a constraint on the author's ability to add
more information.

An RFC writer, on the other hand, can make some optimistic assumptions
about their audience. The audience has time to read carefully. An implementor's
interest in specifics, corner cases and fine distinctions. The document
will be given more investment by the reader than a runbook would, and
the writer has a larger "attention budget" to work with.

### Hypothesis 2: The challenges of cross-functional, collaborative system design require some optimistic assumptions and some pessimistic assumptions
By "cross-functional, collaborative system design," I refer to efforts
by multiple people with different backgrounds, expectations, and requirements
to work together to build a system that finds the best compromise between
their priorities.

#### Optimistic Assumptions

* The readers will assume good intent. It's safe to throw anything out there and rely on your collaborators to help you refine.
* The readers are willing to engage deeply with the design. Each person involved is motivated to see their own priorities addressed and will pay close attention to the relevant aspects of the design.

#### Pessimistic Assumptions

* The documents need to keep pace with the work as it happens. It is common for individuals from different groups to enter and leave the effort all the time. New people need to be able to get themselves up to speed. The criteria for old decisions need to stay visible and appropriately flexible in the face of circumstances.
* In general, stakeholders will not agree to slow development to make time for writing docs.
* The "current" version of the design will change all the time, often drastically.

### Hypothesis 3: An evolved, shared symbol language represents a good compromise for that use case

By "evolved, shared symbol language" I mean a set of symbols (assumed to be
visual but not necessarily so) representing the basic objects of the domains in which the
system is being built. Each symbol cooresponds to a particular object,
such as "a compute instance," and the language uses glyphs
in a diagram to illustrate the system, its components, and their relationships
by (visual) analogy. The language should be shared between organizational groups
so that it includes the vocabulary to express any relevant priority or
requirement.

By including only relevant symbols, a symbol language forms a concise
shorthand for design ideas. This allows documents describing the entire
system structure to be written in a few minutes in a way that can be
shared, modified, and commented on. A person familiar with the language can
comprehend the system diagram at a glance, reason about the relationships
between its parts, and notice and discuss potential changes. A symbol language
makes documentation fast enough to become the language in which ideas
are discussed, which satisfies all three of the pessimistic assumptions.
It makes use of the optimistic assumptions to expect that everyone will
be willing and motivated to invest in learning the language of the project--
which is equivalent to understanding the needs, priorities, and concerns
of the other project members.

## Useful Rules for Visual Symbol Languages

## Some Definitions

When I use the word "symbol," I am referring to a visual form with an assigned
meaning. For instance, there is a symbol for "stop sign"--a red octogon with
the word "STOP" written inside it.

When I use the word "glyph," I am referring to a particular, _specific_
visual representation of a symbol. If I drew a stop sign on a piece of paper,
I would refer to that drawing as a glyph of the stop sign symbol.

When I use the word "diagram," I am specifically referring to a type of
document that conveys meaning visually through glyphs, and may also
have some text. I'm not referring to documents that primarily contain
text or other structured data without glyphs. A diagram can be executed
in any medium.

# Rules for Symbols

## Simplicity

The individual symbols should be easy to draw freehand, with a pencil,
pen, or marker. They should be distinguishable from each other whether
drawn large or small. They should also be easy to draw on a computer,
preferably in SVG. Symbols should not rely on differences in size, color,
or texture to be distinguishable from each other. They should not take
long to draw, or require too much precision or care. When distinctions
between similar things are important, such as an IP that can be private or
public, static or ephemeral, a set of symbols should be formed by
taking a single basic symbol and modifying it for each class in
obvious ways. When you decide on a symbol for one thing that performs
a specific function, (such as a "backend service"--a Google Load Balancer
component representing "the group of things that can handle the request")
the symbols you decide on for different things that perform similar functions
(such as a Kubernetes "service" which represents the same idea in Kubernetes)
should be visually similar but distinguishable.

Do not confuse simplicity of description with simplicity of design--it's
actually pretty hard to freehand draw a passable octogon, so "octogon"
is not a very good choice for a symbol in this kind of symbol language,
even though it is easy to recognize and describe.

## Composability

A symbol for a thing should have the same relationship toward other symbols
as the thing it represents has toward other things. The symbol for something
like a Docker container--that is, a black box with "some thing" inside it--
should be represented in a way that it can be enclosed within the symbols
of things that can play host to it: a kubernetes pod, an instance, etc.
The symbol for something that is usually "owned" by something else, as an IP
address is "owned" by some other piece of infrastructure, should be able
to be added to the symbol for the owner, so that the symbol for "a compute
instance with an IP" is the symbol for "compute instance" plus the symbol for
"IP". The symbol for "a number of compute instances" should be a composition
of more than one of the symbol for "compute instance."

## Ease of Annotation

Symbols alone are almost never sufficient. Each symbol should provide
some opportunity for text to be added to it to explain it. For things
that have a small number of simple attributes, such as firewall rules,
the symbol should double as a kind of table for organizing the text of
those attributes in a consistent way. For things that do not have a small
consistently-important set of attributes, like a compute instance, there
should be a way to associate arbitrary text with the symbol. This is
one reason that the symbols must be distinguishable at any size; in one
diagram, some compute instances may be very similar and not much needs
to be said about them, while another compute instance may be different
or important and require more explanation. In a given diagram, you should
use words to highlight important features, note details, and describe
alternate meanings for symbols in your context (such as using a generic
"identity" symbol to refer specifically to a Kubernetes service account).

# Rules for Glyphs

## Connections are Attributes, not Lines

Only the very simplest systems can be accurately represented by drawing
lines between their components. Lines and arrows should not be used to
connect glyphs to show that they are related to each other. Arrows may
be used as a part of a symbol or to denote changes over time. When you
need to show that two glyphs connect to each other, embed a
diminutive form of one of them within the other. This way you preserve
the relationships between things without clutter, and you can arrange
things freely without being forced to emphasize certain relationships
more than others.

## Things that break convention read as important.

Text that runs over a glyph and obscures it has visual authority over
the glyph. When everything in a diagram is one color except for one thing,
that thing stands out. When two things share a color different from everything
else, they appear related. These characteristics--color, size, texture, number,
etc, which may not be used in the design of symbols--were reserved
expressly so that they would be available as dimensions in which to
expand the meanings of glyphs.
