pyNVML
======

**Note**: I'm no longer maintaining this code, so please fork it and improve it even more!



Python bindings to the NVIDIA Management Library
------------------------------------------------

Provides a Python interface to GPU management and monitoring functions.

This is a wrapper around the NVML library.
For information about the NVML library, see the NVML developer page
http://developer.nvidia.com/nvidia-management-library-nvml

Download the latest package from:
http://pypi.python.org/pypi/nvidia-ml-py/

Note this file can be run with `python -m doctest -v README.md`
although the results are system dependent

Requirements
------------
Python 2.5+, or an earlier version with the `ctypes` module.


Installation
------------

```bash
    sudo python setup.py install
```
Or locally:
```bash
    python setup.py install --user
```


Usage
-----

```python
    >>> from pynvml import *
    >>> nvmlInit()
    >>> print "Driver Version:", nvmlSystemGetDriverVersion()
    Driver Version: 352.00
    >>> deviceCount = nvmlDeviceGetCount()
    >>> for i in range(deviceCount):
    ...     handle = nvmlDeviceGetHandleByIndex(i)
    ...     print "Device", i, ":", nvmlDeviceGetName(handle)
    ... 
    Device 0 : Tesla K40c
    
    >>> nvmlShutdown()

```


Additionally, see `nvidia_smi.py`.  A sample application.


Function
---------
Python methods wrap NVML functions, implemented in a C shared library.
Each function's use is the same with the following exceptions:

- Instead of returning error codes, failing error codes are raised as 
  Python exceptions.


```python
    >>> try:
    ...     nvmlDeviceGetCount()
    ... except NVMLError as error:
    ...     print(error)
    ... 
    Uninitialized

```

- C function output parameters are returned from the corresponding
  Python function left to right.

    
```c
    nvmlReturn_t nvmlDeviceGetEccMode(nvmlDevice_t device,
                                      nvmlEnableState_t *current,
                                      nvmlEnableState_t *pending);
```

```python
    >>> nvmlInit()
    >>> handle = nvmlDeviceGetHandleByIndex(0)
    >>> (current, pending) = nvmlDeviceGetEccMode(handle)

```

- C structs are converted into Python classes.

    
```c
    nvmlReturn_t DECLDIR nvmlDeviceGetMemoryInfo(nvmlDevice_t device,
                                                 nvmlMemory_t *memory);
    typedef struct nvmlMemory_st {
        unsigned long long total;
        unsigned long long free;
        unsigned long long used;
    } nvmlMemory_t;
```

```python
    >>> info = nvmlDeviceGetMemoryInfo(handle)
    >>> print "Total memory:", info.total
    Total memory: 5636292608
    >>> print "Free memory:", info.free
    Free memory: 5578420224
    >>> print "Used memory:", info.used
    Used memory: 57872384

```

- Python handles string buffer creation.
    
```c
    nvmlReturn_t nvmlSystemGetDriverVersion(char* version,
                                            unsigned int length);
```

```python
    >>> version = nvmlSystemGetDriverVersion();
    >>> nvmlShutdown()

```

For usage information see the NVML documentation.


Variables
---------
All meaningful NVML constants and enums are exposed in Python.

The `NVML_VALUE_NOT_AVAILABLE` constant is not used.  Instead `None` is mapped to the field.


Release Notes
-------------
Version 2.285.0
- Added new functions for NVML 2.285.  See NVML documentation for more information.
- Ported to support Python 3.0 and Python 2.0 syntax.
- Added nvidia_smi.py tool as a sample app.
Version 3.295.0
- Added new functions for NVML 3.295.  See NVML documentation for more information.
- Updated nvidia_smi.py tool
  - Includes additional error handling
Version 4.304.0
- Added new functions for NVML 4.304.  See NVML documentation for more information.
- Updated nvidia_smi.py tool
Version 4.304.3
- Fixing nvmlUnitGetDeviceCount bug
Version 5.319.0
- Added new functions for NVML 5.319.  See NVML documentation for more information.
Version 6.340.0
- Added new functions for NVML 6.340.  See NVML documentation for more information.
Version 7.346.0
- Added new functions for NVML 7.346.  See NVML documentation for more information.
Version 7.352.0
- Added new functions for NVML 7.352.  See NVML documentation for more information.
