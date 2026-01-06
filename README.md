# C Coding Style Guide

A practical and opinionated C coding standard intended for embedded systems,
firmware, and system-level software.  
This guide prioritizes **readability, consistency, maintainability, and safety**.

---

## Table of Contents

1. Formatting & Layout  
2. File Naming  
3. Header Files  
4. Include Order  
5. Macros  
6. Variables  
7. Enumerations  
8. Structures & Unions  
9. Typedefs  
10. Functions  
11. Documentation & Commenting  
12. Modules & Grouping  
13. Common Pitfalls & Best Practices  

---

## 1. Formatting & Layout

### Indentation
- Use **spaces only**, never tabs
- Tab width: **4 spaces**
- One statement per line
- Opening braces `{` on the **next line**
- Always use braces for conditionals, even for single statements

```c
if (isReady)
{
    StartDevice();
}
````

---

## 2. File Naming

* Use **lowercase**
* Separate words using underscores (`_`)
* Keep names descriptive and concise

**Examples**

```
app_log.c
app_log.h
ph_sensor_driver.c
```

---

## 3. Header Files

* Header files use `.h` extension
* Every header **must** have a header guard
* Header guards follow this format:

```c
#ifndef __APP_LOG_H__
#define __APP_LOG_H__

/* Declarations */

#endif /* __APP_LOG_H__ */
```

---

## 4. Include Order

Headers must be included in the following order:

1. Standard library headers
2. Third-party / external headers
3. Project headers

### Example (`app.c`)

```c
#include <stdint.h>
#include <string.h>

#include <glfw/glfw.h>

#include "app_log.h"
```

---

## 5. Macros

### Naming Rules

* Use **UPPER_SNAKE_CASE**
* Prefix with module or file name
* Avoid generic macro names

```c
#define PH_SENSOR_DEVICE_ADDRESS   (0x45)
#define PH_SENSOR_VENDOR_ID        "V_123"
```

### Guidelines

* Prefer `const` variables over macros where possible
* Never use macros for logic if an inline function is safer
* Parenthesize macro parameters

```c
#define MAX(a, b) ((a) > (b) ? (a) : (b))
```

---

## 6. Variables

### General Rules

* Names must be **self-explanatory**
* Use `lowerCamelCase`
* Do **not** start variable names with verbs

❌ `runningDevice`
✅ `isDeviceRunning`

### Prefix Conventions

| Prefix | Meaning        |
| ------ | -------------- |
| `p`    | Pointer        |
| `g`    | Global         |
| `pg`   | Global pointer |

### Scope Rules

* Global variables must be `static`
* Constants must be `static const`
* Declare constants before non-constant variables

### Examples

```c
static const char *MODULE_NAME_STR = "main";
static int gDeviceState = 0;
static int *pgDeviceHandle = NULL;

static bool isDeviceReady = false;
```

### Arrays & Strings

* Arrays must indicate structure: `arr`, `table`, `matrix`
* Strings must include `str` or `string`

```c
static int gValueArr[10];
static const char *deviceNameStr;
```

---

## 7. Enumerations

### Rules

* Enum names: `fileName_PascalCase`
* Enum values: `UPPER_SNAKE_CASE`
* Prefix enum values with enum name

```c
typedef enum
{
    APP_LOG_LEVEL_DEBUG = 0,
    APP_LOG_LEVEL_INFO,
    APP_LOG_LEVEL_WARN,
    APP_LOG_LEVEL_ERROR,
    APP_LOG_LEVEL_MAX
} app_log_Level_t;
```

---

## 8. Structures & Unions

### Naming

* Prefix with file/module name
* Use `PascalCase` for types
* Members use `lowerCamelCase`

```c
typedef struct
{
    int logLevel;
    char logBuffer[100];
} app_log_Context_t;
```

```c
typedef union
{
    int valueInt;
    float valueFloat;
    char valueStr[4];
} app_log_Value_t;
```

---

## 9. Typedefs

### Rules

* Use `PascalCase`
* End with `_t`
* Avoid hiding pointers inside typedefs

```c
typedef uint32_t app_log_Id_t;
```

---

## 10. Functions

### Visibility Rules

* **Public functions** → declared in `.h`
* **Private functions** → `static` in `.c`
* Private functions appear **before** public functions

### Naming Rules

* Prefix public functions with module name
* Use `PascalCase`
* Function names should describe **state or result**, not actions

❌ `StartDevice()`
✅ `IsDeviceStarted()`

### Example

```c
/* app_log.h */
void app_log_Init(void);
void app_log_SetLevel(int level);
```

```c
/* app_log.c */
static void ClearLogBuffer(void);

void app_log_Init(void)
{
    ClearLogBuffer();
}
```

---

## 11. Documentation & Commenting

This project uses **Doxygen** for documentation.

### File Documentation

```c
/**
 * @file app_log.h
 * @brief Logging interface for the application.
 */
```

### Function Documentation

```c
/**
 * @brief Initializes the logging module.
 *
 * @param level Initial log level.
 * @return 0 on success, -1 on failure.
 */
int app_log_Init(int level);
```

### Inline Comments

```c
if (level > APP_LOG_LEVEL_MAX)
{
    return -1; /**< Invalid log level */
}
```

---

## 12. Modules & Grouping

Use Doxygen groups to organize large modules.

```c
/**
 * @defgroup Logging Logging Module
 * @{
 */
```

```c
/**
 * @}
 */
```

---

## 13. Common Pitfalls & Best Practices

### Avoid

* Magic numbers
* Deeply nested conditionals
* Hidden side effects in macros
* Global non-static variables

### Prefer

* Explicit error handling
* Defensive checks
* Small, testable functions
* Compile-time checks (`static_assert`)

---

## Example Usage

```c
/**
 * @example log_example.c
 *
 * @code
 * app_log_Init(APP_LOG_LEVEL_INFO);
 * app_log_SetLevel(APP_LOG_LEVEL_DEBUG);
 * @endcode
 */
```


