+++
title = "YAML to Triples Conversion"
insert_anchor_links = "right"
+++

# Summary

As discussed in the [Architecture page](@/internals/architecture.md), PrivGuide stores data from **YAML files** into a **triple store**.
This page will discuss how the conversion is done, which is important to design descriptions and queries that effectively encode and reason with the system descriptions.

# Principles of conversion

Each file will always have a root node named `ROOT`.

Each object is given an identifier, either explicitly with the `id` attribute, or implicitly by incrementing an internal global counter.

Each property name is a triple's predicate, the identifier of the object with the property is the subject, and the value of the property is the object.

For instance, this YAML file...

```yaml
prop: 2
```

...will be converted into the triple:

```
<http://example.org/ROOT> <http://example.org/prop> 2
```

As alluded to before, the object of a triple will be the value of a YAML property.
When such property is an object, the value will instead be the id of said object.

```yml
prop:
  another: 1
  object: 2
```

will be converted into:

```
<http://example.org/ROOT> <http://example.org/prop> <http://example.org/1>
<http://example.org/1> <http://example.org/another> 1
<http://example.org/1> <http://example.org/object> 2
```

When the object is an array, the property will be replicated for each array object according to the above rules.

```yml
letters:
  - a
  - b
  - c
```

will be converted into:

```
<http://example.org/ROOT> <http://example.org/letters> "a"
<http://example.org/ROOT> <http://example.org/letters> "b"
<http://example.org/ROOT> <http://example.org/letters> "c"
```

If the array elements were objects, their identifiers would be used instead of the literals in the example.

The URI base is specified in the `uris.yml` file, which maps the base and abbreviation to the files in which it will be used.

To specify a custom identifier for an object, simply add the `id` property to it.

```yml
objects:
  - id: my id
    property: a
```

will turn into

```
<http://example.org/ROOT> <http://example.org/objects> <http://example.org/my_id>
<http://example.org/my_id> <http://example.org/property> "a"
```

Notice how the `id` property is not itself turned into a triple, but its value is used as the object's identifier instead.
Also of note is that the identifiers' spaces are turned into `_`, however, this has no repercussions on YAML configurations, only when writing queries.

When a property has an identifier of an object, it can be written with the shorthand format `[abbrev]:id`, where `abbrev` is the optional abbreviation of the URI base of the identifier, and `id` is the identifier.

```yml
objs1:
  - id: a
    property: 1
others:
  some obj: :a
```

would be converted into

```
<http://example.org/ROOT> <http://example.org/objs1> <http://example.org/a>
<http://example.org/a> <http://example.org/property> 1
<http://example.org/ROOT> <http://example.org/others> <http://example.org/1>
<http://example.org/1> <http://example.org/some_obj> <http://example.org/a>
```

and the following

```yml
objs1:
  - id: oth:a
    property: 1
others:
  some obj: oth:a
```

would be converted into

```
<http://example.org/ROOT> <http://example.org/objs1> <http://other.org/a>
<http://example.org/a> <http://example.org/property> 1
<http://example.org/ROOT> <http://example.org/others> <http://example.org/1>
<http://example.org/1> <http://example.org/some_obj> <http://other.org/a>
```

Notice how `oth` is the prefix for `other.org`.

## Configuration

A system can have certain variables that are configurable, especially useful when the system is to be exported to third parties as a component of other systems.

Configurations are stored in the `config` directory and are YAML files which contain all the variables that can be changed on system installation.
Each file contains a possible configuration in full.
The configuration itself is a list of variable identifiers and their respective values.

When exporting the system to be integrated within others, 2 things must be provided:

1. the list of configuration variables
2. queries that attest the validity of the configuration and provide scores based on it

