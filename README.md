# base64

> the 64th base of rfc4648

This is a library for base64 encoding and decoding raw data.

## Install

(todo)

## Usage

This library encodes and decodes Byte Arrays but exposes a [typeclass interface](https://github.com/softprops/base64/blob/master/src/main/scala/input.scala#L8-L10) for providing input defined as 

```scala
trait Input[T] {
  def bytes: Array[Byte]
}
```

Instances of this typeclass are defined for `java.nio.ByteBuffer`, `String`, `(String, java.nio.charset.Charset)`, and
`Array[Bytes]`. 

To base64 encode input simply invoke the `Encode` objects `apply` method

```scala
base64.Encode("Man") 
```

This returns a Byte Array. To make this output human readable you may wish to create a String from its output.
When working with web applications its a common need to base64 encode information in a urlsafe way. Do do so
just invoke `urlSafe` with input on the `Encode` object

```scala
new String(base64.Encode.urlSafe("hello world?")) // aGVsbG8gd29ybGQ_
```

A dual for each is provided with the `Decode` object.

```scala
new String(base64.Decode.urlSafe(base64.Encode.urlSafe("hello world?"))) // hello world?
```

## Why

Chances are you probably need a base64 codec.

Chances are you probably don't need everything that came with the library you use to base64 encode data.

This library aims to only do one thing. base64 _. That's it.

A seconday goal was to fully understand [rfc4648](http://www.ietf.org/rfc/rfc4648.txt) from first principals. Implementation is a good learning tool. You should try it.

## Performance

Performance really depends on your usecase, _no matter library you use_. An attempt was made to compare
the encoding and decoding performance with the same input data against apache commons-codec base64 and
netty 4.0.7.final base64.

For encoding and decoding I found the following general repeating performance patterns
when testing [15,000 runs](https://github.com/softprops/base64/blob/master/src/test/scala/base64/bench.scala#L53) for each library for each operation.

```
enc apache commons (byte arrays) took 100 ms
enc netty (byte buf)             took 83 ms
enc ours (byte arrays)           took 142 ms
dec apache commons (byte arrays) took 72 ms
dec netty (byte buf)             took 161 ms
dec ours (byte arrays)           took 89 ms
```

Take this with a grain of salt. None of these will be the performance bottle neck of your application. This was
just a simple measurement test to make sure this library was not doing something totally naive.

### inspiration and learning

taken from

* [Robert Harder's public domain](http://iharder.sourceforge.net/current/java/base64/)
* [netty base64](https://github.com/netty/netty/tree/master/codec/src/main/java/io/netty/handler/codec/base64)
* [haskell base64](https://github.com/bos/base64-bytestring/tree/master/Data/ByteString)
* [@tototoshi](https://github.com/tototoshi/scala-base64)

Doug Tangren (softprops) 2013
