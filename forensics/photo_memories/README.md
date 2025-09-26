# Photo Memories


I ran a `strings` scan to look for obvious flags or hints:

```bash
strings "bill_harper.jpg" | tail
```

The output included a suspicious token at the end:

```
...
i+-t]z+v
ZU8 
SPL{eeff11779250b3bc2b7ac074f86bd1d7}
```
But it's a decoy flag


I attempted to extract hidden data with steghide

```bash
steghide extract -sf "bill_harper.jpg"
```

`steghide` asked for a passphrase. Because of the `strings` result, I tried using the visible `SPL{eeff...}` token as the passphrase first (just in case). That did not work. I then tried an empty passphrase (just pressing Enter). The extraction succeeded:

```
Enter passphrase:
wrote extracted data to "secret.txt".
```

This confirmed there was embedded data and it did not require the `strings` token as a passphrase.

```bash
cat secret.txt
```

```
SPL{2dfa898b8659508904d0c321d3139e4885a441442d7b587e938b31fa84e0d678}
```
