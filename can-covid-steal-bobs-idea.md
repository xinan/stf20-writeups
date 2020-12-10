Can COViD steal Bob's idea?
-

### Description
Bob wants Alice to help him design the stream cipher's keystream generator base on his rough idea. Can COViD steal Bob's "protected" idea?

### Solution
The attachment is a `pcap`. Following the TCP streams in Wireshark. We get:
1. Bob's message to Alice:
```
p = 298161833288328455288826827978944092433
g = 216590906870332474191827756801961881648
g^a = 181553548982634226931709548695881171814
g^b = 64889049934231151703132324484506000958
Hi Alice, could you please help me to design a keystream generator according to the file I share in the file server so that I can use it to encrypt my 500-bytes secret message? Please make sure it run with maximum period without repeating the keystream. The password to protect the file is our shared Diffie-Hellman key in digits. Thanks.
```
2. Alice's FTP command history and a file she downloaded.

Using the `file` command we know that the downloaded file is a zip archive. It prompts for password. Bob mentioned that the password is their shared Diffie-Hellman key in digits.

The DH public parameters are given at the start of Bob's message. The numbers look small. Running them through `discrete_log` in `sage` immediately gives us the private key.

```
sage: p = 298161833288328455288826827978944092433
sage: g = 216590906870332474191827756801961881648
sage: ga = 181553548982634226931709548695881171814
sage: gb = 64889049934231151703132324484506000958
sage: b = discrete_log(Mod(gb, p), Mod(g, p))
sage: gab = pow(ga, b, p)
sage: gab
246544130863363089867058587807471986686
```

This `gab` is the shared DH key. I entered it into Mac's native unzip tools, it didn't work. I then tried `7z` and it worked.

It unzips to a png file describing Bob's crypto design. I stared at that image for a while, ran some steg tools over it, revisited the pcap file, didn't really find any flag.

In the end there was an announcement saying that the password to the zip is the flag. 😐
