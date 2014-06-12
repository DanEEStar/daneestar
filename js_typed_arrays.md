# JavaScript Typed Arrays

to read bytes from a buffer with different types you can use a DataView.
DataView does not support to read UTF-8 data directly from it.
https://github.com/timkurvers/byte-buffer
https://github.com/davidflanagan/BufferView/blob/master/BufferView.js

The Buffer-Class in Node.js seems to support UTF-8 directly
http://nodejs.org/api/buffer.html

In id3.js the code for `getString()` converts the data into a buffer to read it when running under node
id3 spec: http://id3.org/id3v2.4.0-structure
