# Example setup for Apache Fuseki RDF SPARQL server with test data

    ./fuseki-server --file RDF/fromdbpedia.ttl /news

Hit the URL:

    http://localhost:3030/#/dataset/news/query

For my own research I usually use the DBPedia and WikiData SPARQL endpoints but I also need to serve my own RDF assets during development.

I created this simple repo (that is really just the standard Apache Jena Fuseki binary distribution with my notes and data files added) for quick setup and testing on my laptops or leased servers.

## Resources

- TBD: used in examples in my book "Loving Common Lisp, or the Savvy Programmer's Secret Weapon" https://leanpub.com/lovinglisp

- TBD: used in exampes in my book "A Lisp Programmer Living in Python-Land: The Hy Programming Language" https://leanpub.com/hy-lisp-python

- TBD: used in exampes in my book "Practical Artificial Intelligence Programming With Java" https://leanpub.com/javaai
- TBD: used in exampes in my book "Haskell Tutorial and Cookbook" https://leanpub.com/haskell-cookbook

I cover various semantic web and linked data topics in many of my books, but suually use public SPARQL endpoints for examples. I plan on extending these examples using local Apache Jena Fuseki SPARQL endpoints.

My personal web site: https://markwatson.com

## Getting sample data from DBPedia

I use a scratch graph **FromDBPedia** in GraphDB I use for collecting data from DBPedia. I start by removing all triples (assuming **FromDBPedia** is the graph I have selected):

```sparql
DELETE  WHERE { ?object ?property ?value } 
```

Before inserting new triples, we can test the selection pattern with a CONSTRUCT query like:

```sparql
CONSTRUCT { <http://dbpedia.org/resource/Microsoft> ?p ?o } WHERE {
  SERVICE <https://dbpedia.org/sparql> {
      <http://dbpedia.org/resource/Microsoft> ?p ?o
  }
} LIMIT 120
```

To get just english languae labels, comments, etc.:

```sparql
CONSTRUCT { <http://dbpedia.org/resource/Microsoft> ?p ?o } WHERE {
      <http://dbpedia.org/resource/Microsoft> ?p ?o FILTER (lang(?o) = 'en')
} LIMIT 700
```

If we like the results, then insert the selected triples from DBPedia into our local graph:

