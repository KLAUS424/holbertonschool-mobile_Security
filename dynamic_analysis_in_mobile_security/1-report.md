Native Hooking Report: Apk_task1
Goal

Extract the flag that gets decrypted and briefly kept in memory by libnative-lib.so, via its JNI entry point Java_com_example_app_Native_getSecretMessage.

Findings & Memory Analysis
Library targeted: libnative-lib.so
Function targeted: Java_com_example_app_Native_getSecretMessage
Return type: jstring
Flag extracted: FLAG{jn1_n4t1v3_h00k_succ3ss_2026}
Approach

Instead of relying on hardcoded offsets, the exported JNI symbol was located dynamically at runtime using Frida's Module.findExportByName. Once the address was resolved, an Interceptor.attach hook was placed on it. From the onLeave callback, the jstring handle being returned was converted into a readable string via Java.vm.getEnv().getStringUtfChars() — capturing the plaintext straight off the heap before the string object could go out of scope or get swept up by garbage collection.
