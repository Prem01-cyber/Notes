
***
###### `Title :- Hashcat Overview`
###### `Created on :- 2023-06-27 - 20:16`
###### `Created by:- Prem J`
***

#### `Hashcat -->`

- [Hashcat](https://hashcat.net/hashcat/) is a popular open-source password cracking tool.
- It can be downloaded through the above website or `sudo apt install hashcat`
- To view the options by hashcat we can use the command `hashcat -h`
- The folder contains 64-bit binaries for both Windows and Linux. The `-a` and `-m`arguments are used to specify the type of attack mode and hash type. `Hashcat`supports the following attack modes

|**#**|**Mode**|
|---|---|
|0|Straight|
|1|Combination|
|3|Brute-force|
|6|Hybrid Wordlist + Mask|
|7|Hybrid Mask + Wordlist|

- The hash type value is based on the algorithm of the hash to be cracked. A complete list of hash types and their corresponding examples can be found [here](https://hashcat.net/wiki/doku.php?id=example_hashes). 
- The table helps in quickly identifying the number for a given hash type. You can also view the list of example hashes via the command line using `hashcat --example-hashes`

#### `Hashcat - Benchmark -->`

- The benchmark test (or performance test) for a particular hash type can be performed using the `-b` flag.

```shell-session
premjampuram@htb[/htb]$ hashcat -b -m 0
hashcat (v6.1.1) starting in benchmark mode...

Benchmarking uses hand-optimized kernel code by default.
You can use it in your cracking session by setting the -O option.
Note: Using optimized kernel code limits the maximum supported password length.
To disable the optimized kernel code in benchmark mode, use the -w option.

OpenCL API (OpenCL 1.2 pocl 1.5, None+Asserts, LLVM 9.0.1, RELOC, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
=============================================================================================================================
* Device #1: pthread-Intel(R) Core(TM) i7-5820K CPU @ 3.30GHz, 4377/4441 MB (2048 MB allocatable), 6MCU

Benchmark relevant options:
===========================
* --optimized-kernel-enable

Hashmode: 0 - MD5


Speed.#1.........:   449.4 MH/s (12.84ms) @ Accel:1024 Loops:1024 Thr:1 Vec:8

Started: Fri Aug 28 21:52:35 2020
Stopped: Fri Aug 28 21:53:25 2020
```

- For example, the hash rate for MD5 on a given CPU is found to be 450.7 MH/s. We can also run `hashcat -b` to run benchmarks for all hash modes.

#### `Hashcat - Optimizations -->`

- Hashcat has two main ways to optimize speed

|Option|Description|
|---|---|
|Optimized Kernels|This is the `-O` flag, which according to the documentation, means `Enable optimized kernels (limits password length)`. The magical password length number is generally 32, with most wordlists won't even hit that number. This can take the estimated time from days to hours, so it is always recommended to run with `-O` first and then rerun after without the `-O` if your GPU is idle.|
|Workload|This is the `-w` flag, which, according to the documentation, means `Enable a specific workload profile`. The default number is `2`, but if you want to use your computer while Hashcat is running, set this to `1`. If you plan on the computer only running Hashcat, this can be set to `3`.|

>[!Danger]
>It is important to note that the use of `--force` should be avoided.
>Using `--force` is discouraged by the tool's developers and should only be used by experienced users or developers.

- While this appears to make `Hashcat` work on certain hosts, it is actually disabling safety checks, muting warnings, and bypasses problems that the tool's developers have deemed to be blockers.
- These problems can lead to false positives, false negatives, malfunctions, etc
- If the tool is not working properly without forcing it to run with `--force` appended to your command, we should troubleshoot the root cause.