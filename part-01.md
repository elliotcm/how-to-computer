# How to computer

Much ado has been made about the teaching of programming over the years.  Many
resources focus on teaching a language and typically open with an introduction
of functions, classes, variables etc.  My own degree did this too, back in the day.
I feel like that's the wrong place to start.

Programming to me is about data.  Yes, programming languages are very interesting
and learning them is important, but they're ultimately tools for creating, moving,
and modifying data.  The families of programming languages are
often defined by their philosophies towards what counts as data, how to package it,
define it, and various structures and restrictions created to those ends.

That's why I'd like to start with an introduction to what data looks like from a
programming perspective, where you're likely to encounter it and in what form,
and the sort of things you're going to need to do with it to get things done as
a programmer.  After that we can talk about how different programming languages
help you do this and get into the specifics of how to use them.

## Binary

There's a whole lot of detail to binary that you don't need to know right now,
and for most people will never need to know, so let's keep it simple.

Regular digital computers can only represent things with zeros and ones.  This is
where the term `digital` comes from, try representing the number 3.5 on your
fingers (digits) without some weird "kinda half extended" finger business.

Each 0 or 1 binary digit (or `bit`) is physically represented somewhere,
usually electrical pulses down wires
or magnetism in hard drives or tiny holes on the surface of a CD, it doesn't
matter as long as you can tell a 1 from a 0.

So we've only got 0 or 1 to work with for technical reasons, well that's a sucky
limitation, but the advantage is we have an awful lot of them.  The computer that
first got us to the moon had around 192,000 of them.  A mid-range 2017 iphone has
1,040,000,000,000 to play with.  But what do we use them for?

## Encoding

Things exist in the real world, like sounds, colours, words.  Somehow we need to
take these complex sensory marvels and represent them using only a mountain of
1s and 0s.  The process of doing so is called `encoding`.  Encoding is a complicated
topic but it's important so I'll keep it as brief as possible.

Beware that "encoding" can mean three different things here:
1. verb, the process of converting something into its binary representation
2. noun, the set of rules used to do the conversion
3. noun, the binary representation itself

When used as a noun here I'm talking about meaning 2.

<aside>Quick aside, encoding and encryption are different things.  Encryption is about
disguising things, not representing them.  We'll get to it later.</aside>

### Character encoding

Let's say you have a really good tweet that you want to send, something cool and
deep like "I ate a tasty sandwich".  How are we gonna get all that into 0s and 1s?

We can start by breaking it up into letters, encoding each letter somehow and
sticking a bunch of them one after the other.  In fact, since we have a few
spaces in there which aren't technically "letters", let's use the word `characters`.

So that's 22 characters to encode, but we can cut out the duplicates as we can use the
same encoding for them each time. That leaves us with 13 unique characters,
but a bit can only represent up to two different things.  We'll have to use more
than one bit in concert to encode our 13 characters.

While we're at it, we're likely to use this `character encoding` (noun) again for other
cool tweets so we may as well include the rest of the alphabet in upper and lowercase,
plus the numbers and any other characters
we think might be helpful like commas and hyphens.

There's 26 letters in the alphabet, 10 numbers, and 32 punctuation marks common
to most keyboards, so that gives us 96 total characters to encode.  Here's where
we introduce the most technical thing about binary you'll need to
know for now: counting in base 2.

### Counting in base 2

If one bit can only be `0` and `1` only two things can be represented, like the letters
`a` and `b`:
```
0   a
1   b
```

Two bits working together have four possible combinations, `00`, `01`, `10`, and `11`.
With those we could represent `a`, `b`, `c`, and `d`:
```
00   a
01   b
10   c
11   d
```

Three bits give us 8 options, and each time we add a bit the number of options we
have doubles.  Continue on this path and you'll see that 6 bits give you enough space for 64
things and 7 gives you 128.  We've got 96 characters to represent so we'll have to go for
7 bits.  I'm sure we can fill the extra space in later.

### Our 7-bit character encoding (noun)

We know now that we have 7 bits with which to encode each of the 96 characters
we want to represent.  That means every combination of 0 and 1 between 0000000
and 1111111 is available to us, we just need to assign each of our characters to
one of those combinations.

How do we decide which character gets which combination?  It doesn't actually
matter.  As long as the person encoding the characters (you) and the people decoding
the characters (your 17 twitter followers) follow the same rules, it's all good.

So let's make one up.  Let's start with the letter first, because we use those
the most so why not.  And we use lowercase the most so let's do those first too.
```
0000000   a
0000001   b
0000010   c
0000011   d
0000100   e
0000101   f
0000110   g
0000111   h
0001000   i
0001001   j
0001010   k
0001011   l
0001100   m
0001101   n
0001110   o
0001111   p
0010000   q
0010001   r
0010010   s
0010011   t
0010100   u
0010101   v
0010110   w
0010111   x
0011000   y
0011001   z
0011010   A
0011011   B
0011100   C
0011101   D
0011110   E
0011111   F
0100000   G
0100001   H
0100010   I
0100011   J
0100100   K
0100101   L
0100110   M
0100111   N
0101000   O
0101001   P
0101010   Q
0101011   R
0101100   S
0101101   T
0101110   U
0101111   V
0110000   W
0110001   X
0110010   Y
0110011   Z
```

Next let's put the numbers in.  Bear in mind that since base 2 is a numbering
system much like our normal base 10 you can represent numbers directly in binary.
Here we're encoding the **character** version of the numbers, i.e. the way they're
written, not their pure mathematical versions.  We'll get to those later.

The character encoding (noun) is arbitrary and picks up from where we left off with the
letters, which is why the character `0` here is `0110100` rather than `0000000`
as you might expect.
```
0110100   0
0110101   1
0110110   2
0110111   3
0111000   4
0111001   5
0111010   6
0111011   7
0111100   8
0111101   9
```

Next take your 32 punctuation characters and assign those
too.  What order?  Doesn't matter, whatever makes sense to you.  Add more if you
like.  As long as you don't use up more space than has been allowed for this encoding
(7 bits or 128 combinations) and you write it down somewhere so it can be decoded
later, it really doesn't matter.  Let's assume space was the first one after the
numbers and is therefore encoded as `0111110`.

Finished?  Congratulations.  You've written a character encoding.

### Encoding our tweet

Now we need to convert our human letters into cold computer bits using our encoding (noun).

```
0100011   I
0111110   [space]
0000000   a
0010011   t
0000100   e
0111110   [space]
0000000   a
0111110   [space]
0010011   t
0000000   a
0010010   s
0010011   t
0011000   y
0111110   [space]
0010010   s
0000000   a
0001101   n
0000011   d
0010110   w
0001000   i
0000010   c
0000111   h
```

The computer can't comprehend all the human junk here so let's string all these
together into one big binary blob:
`0100011011111000000000010011000010001111100000000011111000100110000000001001000100110011000011111000100100000000000110100000110010110000100000000100000111`

Phew.  If you were following along you'll know that encoding process was a massive
pain to do by hand and quite error-prone.  Later on I'll show you how to use a
programming language to get a computer to do it for you.

### Decoding

So what does Dave from accounting do when he wants to read your good tweet?  Well,
decoding (verb) is the inverse of encoding (verb), and uses the same encoding (noun) to
do it.

First you give him your encoding, i.e. the map showing what characters each 7-bit
binary combination represents, and tell him that each character is always 7 bits long.

Dave then splits your 154 bits into 22 groups of 7 and checks the table to get a
character for each.

That's really it.  The tricky part is knowing the structure of the data (a long
string of 7-bit characters) and how to convert it into something useful.

### Other character encodings (noun)

Since an encoding is arbitrary and assignments are picked by whoever makes it,
there will often be multiple ways of encoding the same thing.

In the 50s and 60s there were two competing character encodings,
[ASCII](https://en.wikipedia.org/wiki/ASCII) and [EBCDIC](https://en.wikipedia.org/wiki/EBCDIC).
ASCII used 7 bits like our encoding, while EBCDIC used 8.  The competition between
them was the same as it's been for every format war in history;
think VHS and Betamax, DVD-R and DVD+R, HD-DVD and BluRay.

IBM's EBCDIC-using 8-bit computers became very popular and a group
of 8-bits was soon called a `byte`.  ASCII adapted and became 8 bits (1 byte)
long itself, eventually winning that particular format war for some arbitrary
market-driven reason, who cares.

Eventually the tech world admitted that there were countries other than the US,
and that perhaps the Chinese and Thai might like to type things
in their own language, so [Unicode (UTF-8)](https://en.wikipedia.org/wiki/UTF-8)
was created which allows a variable number of bytes to form a single character,
meaning it can be extended indefinitely and can contain all the letters and numbers
of the world.

## Wrap up

1. Computers can only work with 0s and 1s.  We call this `binary`.
2. If you need to represent more than 2 things at once you need more than one bit
  to do it.
3. Converting pieces of a real-world thing to small chunks of binary is called
  `encoding` (verb).  Going from binary to the real world is called `decoding`.
4. A set of arbitrary rules mapping those small chunks of binary to the pieces of
  the real world is called an `encoding` (noun).
5. Some examples of encodings are ASCII, EBCDIC, Unicode, MP3, JPEG, etc.
6. The blob of binary you get when you encode something is also called an encoding,
  sorry about that.  Broadly it's called `data`.
7. Encodings are just arbitrary stuff someone made up, don't sweat it, as long
  as everyone's working from the same book it works out fine.

If you found any of this difficult to follow or you've spotted any errors, please
get in touch via [elliot.cm@gmail.com](mailto:elliot.cm@gmail.com) or
[@_HowToComputer_](https://twitter.com/_HowToComputer_).

Next time I'll talk about how programming languages store and access these blobs
of binary data in a consistent and reliable way, known as `data structures`.
