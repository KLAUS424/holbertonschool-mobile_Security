Revealing and Invoking Hidden Functions Report: Apk_task3
Goal

Track down the flag-decryption routines buried inside Apk_task3.apk that nothing in the normal UI flow ever calls, wire up their class dependencies at runtime through Frida, and chain the right decoding calls together to surface the hidden flag.

Findings
Class targeted: com.example.app.SecretUtils
Decryption methods: decryptFlag(String) and decodeBase64Custom(String)
Flag recovered: FLAG{h1dd3n_m3th0d_inv0k3d_3xp0s3d_2026}
Approach

A static pass through jadx surfaced com.example.app.SecretUtils and made it clear its methods sit completely outside the app's reachable UI paths. To get at them anyway, the class was pulled into the live process with Frida's Java.use() and instantiated via its default constructor. Calling decryptFlag() with the expected argument produced an intermediate Base64 string; feeding that into decodeBase64Custom() reversed the byte-level XOR obfuscation and left the plaintext flag exposed.
