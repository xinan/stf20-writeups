COViD is able to compromise our IoT encryption system, how come?
-

### Description
We discovered 200 power consumption traces and its corresponding plaintexts that COViD had gathered in order to compromise our IoT encryption device. Our device uses stream cipher for encryption with a fixed 8-bytes secret key. We know for stream cipher encryption, we shouldn’t reuse the key but if the attacker can only send plaintext into the device without knowing its corresponding ciphertext, it should be fine right? With only the power consumption traces and plaintexts, can you recovered the 8-bytes secret key that stored in our IoT encryption device?

### Solution
No one solved this one apparently. If I'm the only one submitting a "write-up", albeit empty, do I get a prize?
