# SPIFFSIni

A simple, lightweight library for managing INI-style configuration files in SPIFFS on ESP32 using Arduino IDE.

## Overview

`SPIFFSIni` is a single-header C++ class designed to store and retrieve small sets of parameters in a text file within the SPIFFS filesystem on ESP32. It prioritizes simplicity and portability, requiring no external libraries beyond the Arduino core. Just include the header and start using it!

- **Lightweight**: No dependencies beyond `FS.h` and `SPIFFS.h`.
- **Easy to Use**: Drop the header into your project and manage parameters with minimal setup.
- **Targeted Use Case**: Ideal for small-scale parameter storage (e.g., PID constants, settings).
- **Environment**: Tailored for ESP32 with Arduino IDE.

## Features

- Read parameter values as strings with `read()`.
- Check if a parameter exists with `exist()`.
- Write or update parameters with `write()`, with basic validation to prevent invalid keys.
- INI-style format: `key=value` per line, with optional `#` comments.

## Installation

1. Download `SPIFFSIni.h` from this repository.
2. Place it in your Arduino project folder or library directory.
3. Include it in your sketch:
   ```cpp
   #include "SPIFFSIni.h"
   ```

No additional libraries are required—everything works with the standard Arduino ESP32 core.

## Usage Example

```cpp
#include "SPIFFSIni.h"

void setup() {
    Serial.begin(115200);

    // Initialize with a config file name
    SPIFFSIni config("/config.ini", true);  // true = format SPIFFS if mount fails

    // Write some parameters
    config.write("kp", "1.5");
    config.write("ki", "0.5");

    // Read a parameter
    String kp_value = config.read("kp");
    Serial.println("kp = " + kp_value);  // Prints: kp = 1.5

    // Check existence
    if (config.exist("ki")) {
        Serial.println("ki exists");  // Prints: ki exists
    }
}

void loop() {}
```

This will create or update a file `/config.ini` in SPIFFS with content like:
```
kp=1.5
ki=0.5
```

## API Reference

- **`SPIFFSIni(String file_name, bool format = false)`**
  - Constructor. `file_name` is the path to the INI file (e.g., "/config.ini"). If `format` is `true`, SPIFFS is initialized with formatting on failure.
- **`String read(String param_name)`**
  - Returns the value of `param_name` as a `String`, or an empty `String` if not found or file doesn't exist.
- **`bool exist(String param_name)`**
  - Returns `true` if `param_name` exists in the file, `false` otherwise.
- **`bool write(String param_name, String value)`**
  - Writes or updates `param_name` with `value`. Returns `true` on success, `false` on error or if `param_name` contains `=`.

## Notes

- Designed for small-scale configurations. For complex data structures, consider alternatives like ArduinoJson.
- Parameter names with `=` are rejected by `write()` to prevent format errors.
