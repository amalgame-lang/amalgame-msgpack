# amalgame-msgpack

Pure-Amalgame MessagePack 1.0 subset codec for [Amalgame](https://github.com/amalgame-lang/Amalgame).
Encodes / decodes via the existing `JsonValue` tree
(`Amalgame.Json`), so callers can switch wire formats with a
one-line rename.

Originally bundled in amc's `src/stdlib/msgpack.am`; extracted into
this external package as part of the framework split (post-v0.7.5).

## Install

```bash
amc package add msgpack                  # via the curated index
amc package add github.com/amalgame-lang/amalgame-msgpack@v0.1.0
```

Requires **amc 0.7.7+**.

## Surface

```amalgame
import Amalgame.Collections
import Amalgame.Json
import Amalgame.Formats.MsgPack

class Program {
    public static void Main() {
        let jv = new JsonValue()
        jv.MakeMap()
        let nameV = new JsonValue()
        nameV.SetString("ada")
        jv.MapPut("name", nameV)

        let bytes: List<int> = MsgPack.EncodeJson(jv)
        let back:  JsonValue = MsgPack.DecodeJson(bytes)

        let bn: JsonValue = back.Get("name")
        Console.WriteLine(bn.AsString())   // → ada
    }
}
```

## Coverage

- nil / false / true (0xc0 / 0xc2 / 0xc3)
- positive fixint (0..127) / negative fixint (-32..-1)
- int 8 / int 16 / int 32 (0xd0 / 0xd1 / 0xd2)
- fixstr (0..31 bytes) / str 8 (0..255 bytes)
- fixarray (0..15) / fixmap (0..15)

Larger maps/arrays + binary/extension types are out of scope in
v0.1 — extend the facade if you need them.

## Tests

```bash
./tests/run_tests.sh /path/to/amc
```

## License

Apache-2.0 — see `LICENSE`. No vendored third-party code.
