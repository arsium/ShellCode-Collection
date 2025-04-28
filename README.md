# My personal repository for shellcode creation

This repository contains my personal created shellcodes for many tasks in reverse engineering or evasion/obfuscation. All shellcodes are designed to work in x64 (PE32+) executable. Also, some of them require RWX memory region. This repository will be updated at any time, when I find a shellcode useful in my journey. Source code won't be published, so feel free to reverse them :)



## 1. Write File

This shellcode is designed to write some text in a file or even console from a file handle and is compatible with multi-thread app.

1. Signature of shellcode : 

```c
typedef ULONG_PTR(*WriteFileHandle)(HANDLE file_handle, char* buffer, size_t sizeOfBuffer, size_t count, const char* format, ...);
```

2. Procedures called : 

* _vsnprintf_s
* NtLockFile
* NtWriteFile
* NtUnlockFile

3. Entry point : base address + 0x180

4. Returns : always 0x0

5. Example : 

   ```c
   typedef ULONG_PTR(*WriteFileHandle)(HANDLE file_handle, char* buffer, size_t sizeOfBuffer, size_t count, const char* format, ...);
   WriteFileHandle write_file_shell = (WriteFileHandle)(&write_file_shell[0x180]);
   char test[20];
   write_file_shell(((PPEB)NtCurrentPeb())->ProcessParameters->StandardOutput, & test[0], 20, 20, "Hello %s !", "world");
   ```

   

## 2. Create File

This shellcode is designed to create a ".txt" file with the following format "pîd_tid.txt". This file could be used to log some information about context of execution

1. Signature of shellcode :

```c
typedef ULONG_PTR (*CreateFileHandle)(void);
```



2. Procedures called : 

   * wcsncpy
   * swprintf
   * NtCreateFile

3. Entry point : base address + 0x180

4. Returns : file handle to newly created file

5. Example :

   ```c
   typedef ULONG_PTR (*CreateFileHandle)(void);
   CreateFileHandle create_file_shell = (CreateFileHandle)(&create_file_shell[0x180]);
   HANDLE file_handle = create_file_shell();
   ```

   
