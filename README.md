# Ring buffer for byte oriented data

> A bare bones ring/circular buffer implementation for byte oriented data.

# Whats a Ring Buffer?

From wikipedia:

> A circular buffer, circular queue, cyclic buffer or ring buffer is a data structure that uses a single, fixed-size buffer as if it were connected end-to-end. This structure lends itself easily to buffering data streams.


## API:

`init*[T](b: type[RingBuffer[T]], size = DefaultSize)`:

Create and initialize the ring buffer. Takes an optional maximum ``size``
parameter, otherwise ``size`` will default to ``DefaultSize`` which is set to 1024.

```nim
  # create a buffer with 5
  var buff = RingBuffer[byte].init(5)
  buff.add(@['a', 'b', 'c', 'd', 'e'])
  var data = newSeq[char](5)
  discard buff.read(data)
  echo data # prints @['a', 'b', 'c', 'd', 'e']
```

`append*[T](b: var RingBuf[T], data: openarray[T])`:

Append data to the end of the buffer. ``data`` will be ``shallowCopy``ed into the buffer to overcome Nim's copy semantics for ``seq``.

```nim
    buff.append(@['a', 'b', 'b', 'c', 'd'])
```

`read*[T](b: var RingBuffer[T], data: var openArray[T], size: int = -1): int`:

Read up to ``size`` bytes/chars from the front of the buffer into the ``data`` argument.

Returns an int indicating the amount of bytes/chars read.

Note that ``size`` is the maximum amount of bytes/chars to read, if not enough data is available read will return what it can. If ``size`` is not provided, then the ``len`` field of the ``data`` argument will be used instead.

```nim
# read 5 chars from the buffer
var data = newSeq[char](10)
assert(buff.read(data, 5) == 5)
```

`reset*[T](b: var RingBuffer[T])`:

Reset the internal state of the buffer. The internal buffer
itself will not be cleared, but all internal pointers will be
which allows reusing the buffer as if new.

`clear*[T](b: var RingBuffer[T])`:

Reset and clear the buffer.

## License:
- MIT
