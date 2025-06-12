# RDF/POST

**[RDF/POST](https://atomgraph.github.io/RDF-POST/)** is a compact syntax for serialising an RDF graph inside a classic HTML `application/x-www-form-urlencoded` payload, so it works with *any* browser’s GET or POST mechanisms—no JavaScript required.

## Why use it?

* **Plain‑HTML forms** – build RDF edit or create interfaces with ordinary `<form>` elements.
* **Human + machine friendly** – the same URL or form post doubles as both a web page and a programmatic API.
* **Short & safe** – based on Turtle’s abbreviated tree form but avoids punctuation, keeping every component URL‑safe and well under typical 2 KB URL limits.

## TL;DR syntax

| Part          | Key(s)                                                                                                                                       | Meaning                             |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------- |
| `rdf=`        | (first pair)                                                                                                                                 | marks the start of an RDF/POST body |
| `&v=`         | default namespace IRI                                                                                                                        |                                     |
| `&n=…&v=`     | extra **n**amespace prefix + IRI                                                                                                             |                                     |
| **Subject**   | `&sb=` blank ‖ `&su=` full IRI ‖ `&sv=` suffix ‖ `&sn=…&sv=` prefixed suffix                                                                 |                                     |
| **Predicate** | `&pv=` suffix ‖ `&pn=…&pv=` prefixed suffix ‖ `&pu=` full IRI                                                                                |                                     |
| **Object**    | `&ob=` blank ‖ `&ou=` full IRI ‖ `&ov=` suffix ‖ `&on=…&ov=` prefixed suffix ‖ `&ol=` literal (plus optional `&lt=` datatype \| `&ll=` lang) |                                     |

*(See the [specification](https://atomgraph.github.io/RDF-POST/) for the full EBNF grammar.)*

## Minimal example

```html
<form action="/post" method="post">
  <!-- start -->
  <input type="hidden" name="rdf" value="">
  <!-- namespaces -->
  <input type="hidden" name="v" value="http://xmlns.com/foaf/0.1/">
  <input type="hidden" name="n" value="dc">
  <input type="hidden" name="v" value="http://purl.org/dc/elements/1.1/">
  <!-- triples -->
  <input type="hidden" name="sb" value="o">
  <input type="hidden" name="pv" value="givenName">
  <input type="text"   name="ol" value="Ora">
  …
</form>
```

When submitted, the browser sends a single line such as

```
rdf=&v=http://xmlns.com/foaf/0.1/&n=dc&v=http://purl.org/dc/elements/1.1/&sb=o&pv=givenName&ol=Ora …
```

which is equivalent to the Turtle below:

```turtle
_:o foaf:givenName "Ora" ;
    dc:creator         _:b .
```

## Media type & file extension

* Default: `application/x-www-form-urlencoded`
* RDF‑specific (proposed): `application/rdf+x-www-form-urlencoded`
* Recommended extension: `.rpo`

## Implementations

* [**RDFPostReader**](https://github.com/AtomGraph/Core/blob/master/src/main/java/com/atomgraph/core/riot/lang/RDFPostReader.java) – streaming parser for Apache Jena (AtomGraph Core)

## Credits

Specification by **Sergei Egorov**, maintained by [**AtomGraph**](https://atomgraph.com/).
