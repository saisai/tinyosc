# TinyOSC

TinyOSC is a minimal [Open Sound Control](http://opensoundcontrol.org/) (OSC) library written in C. The typical use case is to parse a raw received buffer directly from a socket.

## Code Example
```C
#include "tinyosc.h"

tosc_tinyosc osc; // declare the TinyOSC structure
char buffer[1024]; // declare a buffer into which to read the socket contents
int len = 0; // the number of bytes read from the socket

while ((len = READ_BYTES_FROM_SOCKET(buffer)) > 0) {
  // parse the buffer contents (the raw OSC bytes)
  // a return value of 0 indicates no error
  if (!tosc_init(&osc, buffer, len)) {
    printf("Received OSC message: [%i bytes] %s %s ",
        len, // the number of bytes in the OSC message
        osc.address, // the OSC address string, e.g. "/button1"
        osc.format); // the OSC format string, e.g. "f"
    for (int i = 0; osc.format[i] != '\0'; i++) {
      switch (osc.format[i]) {
        case 'f': printf("%g ", tosc_getNextFloat(&osc)); break;
        case 'i': printf("%i ", tosc_getNextInt32(&osc)); break;
        case 's': printf("%s ", tosc_getNextString(&osc)); break;
        default: continue;
      }
    }
    printf("\n");
  }
}
```

## License
TinyOSC is published under the [ISC license](http://opensource.org/licenses/ISC). Please see the `LICENSE` file included in this repository, also reproduced below. In short, you are welcome to use this code for any purpose, including commercial and closed-source use.

```
Copyright (c) 2015, Martin Roth <mhroth@gmail.com>

Permission to use, copy, modify, and/or distribute this software for any
purpose with or without fee is hereby granted, provided that the above
copyright notice and this permission notice appear in all copies.

THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES
WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF
MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR
ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES
WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN
ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF
OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.
```
