
## pokenamehex

Meant for figuring out what the hex bytes should be for inputting
pokemon names in PKSM. Takes an arbitrary unicode string (or multiple)
and encodes them (to UTF-16 little-endian by default, used in the
pokemon file format), then dumps the bytes (right zero-padded to
24 hexpairs by default).

You could use this for other things I guess. The default encoding and buffer
length defaults are just chosen for hex editing pokemon names because that's
why I wrote it. It was slightly easier and nicer than writing a file with a
name and setting the encoding and hexdumping it and probably discarding some
extraneous stuff.

### Usage

Put it on your PATH somewhere. Or don't. See if I care!

Straight ouf ot `-h`, have fun.

```
    Show hexdump for unicode strings, after encoding them (UTF-16LE
    by default), suitable for hex editing pokemon names (i.e. in PKSM) or for
    whatever other reason you want a hexdump of encoded unicode strings I guess.

    Usage:
      pokenamehex [-h|--help]     you're looking at it
      pokenamehex [STRING ...]    show hexpairs for STRINGs
      pokenamehex -               read strings from STDIN

    ENV vars:
      BUF_LEN               buffer length (default 24 bytes)
      ENCODING              encoding (default UTF-16LE, see Ruby encoding names)
```

### Examples

Pass strings as arguments:

```terminal
vaz@nil $ pokenamehex '소원의별' 'Silph' 'Ｐスクラップ'
8c c1 d0 c6 58 c7 c4 bc 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    소원의별
53 00 69 00 6c 00 70 00 68 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    Silph
30 ff b9 30 af 30 e9 30 c3 30 d7 30 00 00 00 00 00 00 00 00 00 00 00 00    Ｐスクラップ
```

Or put names in a file and pipe it in:

```terminal
vaz@nil $ cat names
소원의별
PKSM
Ｐスクラップ
ポケセン
blahblah.tv

vaz@nil $ pokenamehex - < names
8c c1 d0 c6 58 c7 c4 bc 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    소원의별
50 00 4b 00 53 00 4d 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    PKSM
30 ff b9 30 af 30 e9 30 c3 30 d7 30 00 00 00 00 00 00 00 00 00 00 00 00    Ｐスクラップ
dd 30 b1 30 bb 30 f3 30 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ポケセン
62 00 6c 00 61 00 68 00 62 00 6c 00 61 00 68 00 2e 00 74 00 76 00 00 00    blahblah.tv
```

It's smart about chomping off newlines or you'd probably see `0a` for `\n` and/or
`0d` for `\r` trailing all the names and you don't want those probably.
