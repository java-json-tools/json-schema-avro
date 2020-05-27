[![License LGPLv3][LGPLv3 badge]][LGPLv3]
[![License ASL 2.0][ASL 2.0 badge]][ASL 2.0]
[![Build Status][Travis badge]][Travis]
[![Maven Central][Maven Central badge]][Maven]

## Read me first

This project, as of version 0.1.4, is licensed under both LGPLv3 and ASL 2.0. See
file LICENSE for more details. Versions 0.1.3 and lower are licensed under LGPLv3
only.

**Note the "L" in "LGPL". LGPL AND GPL ARE QUITE DIFFERENT!**

## What this is

This package contains two processors (see
[json-schema-core](https://github.com/java-json-tools/json-schema-core)) to
convert Avro schemas to JSON Schemas, and the reverse.

## Status

### Avro schemas to JSON Schema

This processor can transform **all** Avro schemas you can think of, as long as said schemas
are self contained. The generated JSON Schemas can accurately validate JSON representations of Avro
data with two exceptions:

* as JSON has no notion of order, the `order` property of
  Avro records is not enforced;
* Avro's `float` and `double` are validated as JSON numbers, with no minimum or
  maximum, see below as to why. Note however that `int`
  and `long`'s limits *are* enforced.

Note that this processor is demoed online
[here](http://json-schema-validator.herokuapp.com/avro.jsp).

## JSON Schema to Avro schemas

This processor is not complete yet. It is *much* more difficult to write than the reverse for two
reasons:

* JSON Schema can describe a much broader set of data than Avro (Avro can only have strings in
  enums, for instance, while enums in JSON Schema can have any JSON value);
* but Avro has notions which are not available in JSON (property order in records, binary types).

The generated Avro schemas are however reasonably good, and cover a very large subset of JSON
Schema usages.

This processor is not available online yet; it will soon be.

## Why limits are not enforced on Avro's `float` and `double`

While JSON Schema has `minimum` and `maximum` to enforce the minimum and maximum values of a JSON
number, JSON numbers ([RFC 4627 §2.4](https://tools.ietf.org/html/rfc4627#section-2.4)) do not
define any limit to the scale and/or precision of numbers.

That is a first reason, but then one should ask why then, are there limits for `int` and `long`.
There are two reasons for this:

* JSON Schema defines integer (as a number with no fractional and/or exponent part); integer being
  a discrete domain, such limits can therefore be defined without room for error;
* but Avro's `float` and `double` are IEEE 754 floating point numbers; they do have minimums and
  maximums, but 0.1, for instance, cannot even be represented exactly in a double.

Defining limits would therefore not ensure that the JSON number being validated can indeed fit
into the corresponding Avro type.

## Maven artifact

```xml
<dependency>
    <groupId>com.github.java-json-tools</groupId>
    <artifactId>json-schema-avro</artifactId>
    <version>your-version-here</version>
</dependency>
```

Versions before 0.1.5 are available at `groupId` `com.github.fge`.

[LGPLv3 badge]: https://img.shields.io/:license-LGPLv3-blue.svg
[LGPLv3]: http://www.gnu.org/licenses/lgpl-3.0.html
[ASL 2.0 badge]: https://img.shields.io/:license-Apache%202.0-blue.svg
[ASL 2.0]: http://www.apache.org/licenses/LICENSE-2.0.html
[Travis Badge]: https://travis-ci.com/java-json-tools/json-schema-avro.svg?branch=master
[Travis]: https://travis-ci.com/java-json-tools/json-schema-avro
[Maven Central badge]: https://img.shields.io/maven-central/v/com.github.java-json-tools/json-schema-avro.svg
[Maven]: https://search.maven.org/artifact/com.github.java-json-tools/json-schema-avro
