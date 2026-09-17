Network Interception & Cryptographic Decryption Report: Apk_task2
Goal

Intercept the encrypted HTTPS traffic flowing between Apk_task2.apk and its backend, bypass the app's runtime SSL pinning, pull the symmetric key and IV out of the decompiled source, and use them to decrypt the flag being transmitted.

Findings & Cryptographic Parameters
Interception tooling: Burp Suite Community / Objection
Pinning bypass: runtime instrumentation via Frida / objection android sslpinning disable
Cipher: AES/CBC/PKCS5Padding
Secret key: 321c_s3cr3t_k3y!
IV: 1234567890abcdef
Flag recovered: FLAG{a3s_cbc_n3tw0rk_d3cryp110n_succ3ss_2026}
Approach

The Burp CA certificate was pushed onto the test device to route outbound HTTPS through the proxy, and Objection was used to knock out the OkHttp/TrustManager pinning logic at runtime. Once traffic became visible, the APK was pulled apart in jadx-gui, leading to com.example.cryptoapp.network.CryptoManager as the class handling the response. The AES-128 key and IV turned out to be sitting right there as hardcoded constants in that class. From there, the Base64-encoded payload from the server was decoded and run back through AES-CBC offline using those parameters, exposing the plaintext flag.
