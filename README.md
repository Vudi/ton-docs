## TL-B Language

TL-B (Type Language - Binary) serves to describe the type system, constructors, and existing functions. For example we can use TL-B schemes to build binary structures associated with the TON blockchain. Special TL-B parsers can read schemes to deserialize binary data into different objects.

### Table of contents
- [TL-B Language](#tl-b-language)
  - [Table of contents](#table-of-contents)
  - [Overview](#overview)
  - [Comments](#comments)
  - [Library usage](#library-usage)
  - [Useful sources](#useful-sources)

### Overview

We refer to any set of TL-B constructs as TL-B documents. A TL-B document usually consists of two sections, which are separated by the `---functions---` keyword. The first section consists of declarations of types (i.e. their constructors). The second section consists of the declared functions, i.e. functional combinators. 

We may not follow the separation principle in small documents with no more than one functional combinator.

The first and second sections consist of combinator declarations, each ending with a semicolon(`;`). However, the first section contains only constructors, while the second section only involves functions. Each combinator is declared using a “combinator declaration” in the format explained above. However, the combinator number and field names may be explicitly assigned.

If additional type declarations are required after functions have been declared, the keyword (section divider)  
`---types---` is used. Furthermore, a functional combinator may be declared in the type section if its result type begins with an exclamation point (in fact, when the function section is interpreted, this exclamation point is added automatically).

To explicitly define 32-bit names of combinators, a hash mark (`#`) is added immediately after the combinator’s name, followed by 8 hexadecimal digits. In this documentation we will look at an example of how to correctly define a 32-bit combinator name for an internal message in the TON blockchain.

Here is an example of a possible TL-B document.    
We used the symbols `|`, `_`, `↓` for terminology - they are not part of the TL-B scheme.
```
| combinator declaration |
|                   _____|
|__________________|
rop:uint32 data:Any = MsgBody;
                     | type
                     | combinator name


| combinator declaration                    |   | end symbol |
|___________________________________________|         |
notification#0x3f5476ca comment:StartComment          ↓
    real_body:(Either MsgBody ^MsgBody)      = Request;
   |________| |_______________________|       |_______|
   | const- | | constructor body      |       | functional
   | ructor                           |       | combinator name
   | name                             |
   |__________________________________|
   | constructor
```


### Comments

Comments are the same as in C++
```
/* 
This is
a comment 
*/

// This is one line comment
```

### Library usage

You can use TL-B libraries to extend your documents and to avoid writing repetitive schemes. We have prepared a set of ready-made libraries that you can use. They are mostly based on block.tlb, but we have also added some combinators of our own.

- `tonstdlib.tlb`
- `tonextlib.tlb`
- `hashmap.tlb`

In TL-B libraries there is no concept of cyclic import. Just indicate the dependency on some other document (library) at the top of the document with the keyword `dependson`. For example:

file `mydoc.tlb`:
```c
//
// dependson "libraries/tonstdlib.tlb"
//

op:uint32 data:Any = MsgBody;
something$0101 data:(Maybe ^MsgBody) = SomethingImportant;
```

In dependencies, you are required to specify the correct relative path. The example above is located in such a tree:

```
.
├── mydoc.tlb
├── libraries
│   ├── ...
│   └── tonstdlib.tlb
└── ...
```

### Useful sources

- [Telegram Open Network Virtual Machine](https://newton-blockchain.github.io/docs/tvm.pdf)
- [A description of an older version of TL](https://core.telegram.org/mtproto/TL)
- [block.tlb](https://github.com/newton-blockchain/ton/blob/master/crypto/block/block.tlb)
