# Temporary-simple-fix-error-unresolved-symbol-for-building-custom-rom

Here is the solution with a simple addition to `Android.bp`.

### **Simple Solution: Allowing Undefined Symbols**

#### **Example Case:**
The error appears due to **unresolved symbols** when building `android.hardware.keymaster@4.1.nos`. The solution is to add the `allow_undefined_symbols: true` property in `Android.bp` to allow undefined symbols.

#### **Placement:**
Example file path to edit:
```
device/google/sunfish/Android.bp
```

#### **Code to Add:**
```bp
cc_library_shared {
    name: "android.hardware.keymaster@4.1.nos",
    srcs: ["keymaster_nos.cpp"],
    shared_libs: [
        "libkeymaster4support",
        "libkeymaster4_1support",
    ],
    allow_undefined_symbols: true,
}
```

#### **Explanation:**
- **`allow_undefined_symbols: true`**: This property allows the library to have undefined symbols during the linking process. It helps resolve `unresolved symbols` errors that occur when certain symbols are not found during the build.

#### **Note:**
Using `allow_undefined_symbols: true` is a quick solution; however, for a long-term fix, it is advisable to investigate why the symbols are missing and correct the corresponding library or dependency.

With this addition, you should be able to proceed with the build without encountering `unresolved symbols` errors in the logs as indicated by the specified path.
