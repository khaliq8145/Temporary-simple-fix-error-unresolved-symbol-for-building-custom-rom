Here's an updated guide including a note to follow the path shown in the error log.

# Temporary Simple Fix for Unresolved Symbol Errors in Custom ROM Building

This guide provides a quick and simple solution for **unresolved symbol** errors during the build process by making a small addition to the `Android.bp` file. It can be applied to any device and any module causing this issue.

### **Overview of the Solution**
The **unresolved symbols** error occurs when certain symbols are not found during the linking stage of the build process. To temporarily resolve this, we can instruct the build system to allow undefined symbols using the `allow_undefined_symbols: true` property.

### **How to Apply the Fix**

1. **Identify the Error:**
   - Review your build logs to find the unresolved symbol error. The log will typically indicate a missing symbol and provide the path to the `Android.bp` file where the issue originates.

   **Example Error Log:**
   ```
   <path_to_error>/android.hardware.rebootescrow-service.citadel: error: Unresolved symbol: <symbol_name>
   note: Please re-build the prebuilt file or add `allow_undefined_symbols: true` in your Android.bp.
   ```

2. **Follow the Path in the Error Log:**
   - Use the file path specified in the error log to locate the `Android.bp` file. For instance:
     ```
     cd <path_to_error>
     ```
   - Example:
     ```
     cd device/google/sunfish
     ```

3. **Edit the `Android.bp` File:**
   - Open the `Android.bp` file in a text editor.

4. **Add the Fix:**
   - Locate the module causing the issue (often a `cc_library_shared` or `cc_library_static` block).
   - Add the `allow_undefined_symbols: true` property to the module configuration.

   **Example Code:**
   ```bp
   cc_library_shared {
       name: "<module_name>",
       srcs: ["<source_file>.cpp"],
       shared_libs: [
           "<dependency1>",
           "<dependency2>",
       ],
       allow_undefined_symbols: true,
   }
   ```
   - Replace `<module_name>`, `<source_file>.cpp`, and dependencies with the specific details found in the error log.

### **Explanation:**
- **`allow_undefined_symbols: true`**: This property tells the build system to ignore undefined symbols during the linking process. It is a temporary workaround to bypass the `unresolved symbols` error, allowing the build to continue.

### **Example Use Case:**
If the error log indicates an issue with `android.hardware.keymaster@4.1.nos`, the fix in `Android.bp` would look like this:
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

### **Important Note:**
- **Follow the Error Path**: Always locate the exact path indicated in your error log to identify where the fix needs to be applied.
- **Temporary Solution**: This is a temporary workaround. To ensure long-term stability:
  - Investigate why the symbols are missing.
  - Verify that all necessary dependencies are properly included in the build configuration.
  - Correct any missing or incorrect references in the codebase or dependencies.

By following these steps and applying the fix at the path indicated in the error logs, you can bypass unresolved symbol errors and proceed with building your custom ROM for any device.
