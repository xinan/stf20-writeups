COVID has been caught exfiltrating an encrypted message back to their servers! Let's find out what it is...
-

### Description
After monitoring some network transmissions, we have noticed some encrypted messages being sent from a possibly compromised machine back to COVID's command machine. We have obtained one of these encrypted messages, as well as the access to a web service running on the machine. Can you tell us what does the encrypted messages contain?

### Solution
The attachments are 
- `attacher_rsa.pub` The attacher's RSA public key
- `covid_super_secret.txt` The secret that we are supposed to decrypt

This one was surprisingly easy. I looked up the `n` on factordb, and I got `p` and `q`. The rest is just computing the private key and decrypt the message.

```
sage: from Crypto.Util.number import bytes_to_long, long_to_bytes
sage: import codecs
sage:
sage: n = 91150209829916536965146520317827566881182630249923637533035630164622161072289
sage: p = 1250144956572098069237673985632153337
sage: q = 72911712638389341259648185606860594417897
sage: phi = (p - 1) * (q - 1)
sage: e = 65537
sage: d = inverse_mod(e, phi)
sage:
sage: c = b'arq1ZbigvN73HaydYw5W81vofLMeHCl6Kq62ZLyj3Qw='
sage: long_to_bytes(pow(bytes_to_long(codecs.decode(c, 'base64')), d, n))
b'\x02\x00\x91 cp\xd5:\xd3\x1c\x00L3t$_@tt@ck_Th3_PjDD!'
```

Ignoring the front part which is most likely RSA paddings, the flag is `govtech-csg{L3t$_@tt@ck_Th3_PjDD!}`
