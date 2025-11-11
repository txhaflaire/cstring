# cstring
A commandline utility that's able to efficiently parse strings from a folder containing one to many macho executables and leaving a `common_strings.txt` file in the respective folder that contains strings that are common/existing in every binary.


## how to use this tool
1. Download the installer from [Releases](https://github.com/txhaflaire/cstring/releases) and run the installer (codesigned and notarized) package `csstring-v1.0.0.pkg` 
2. The executable will be put in `/usr/local/bin/cstring`
3. The execucable needs a path to a directory as argument like the example here: `cstring /Users/user/samples/atomic/2025-02-24`
3. a. Once ran it will provide the following output:

![cstring](./img/cstring.gif "cstring")

## Usage
```
OVERVIEW: Generate common_strings files for Mach-O binaries in a folder (optionally recursively).

USAGE: This utility can be helpful after using the symhash CLI that organizes binaries based on their symhash. It will generate a common_strings file for each binary in the given folder, and optionally all its subfolders. This can speed up the process of building YARA rules that are using strings.

ARGUMENTS:
  <folder-path>           Path to a folder containing Mach-O binaries

OPTIONS:
  -r, --recursive         Process subfolders recursively
  --version               Show the version.
  -h, --help              Show help information.
```
  
```
➜  Downloads git:(main) ✗ cstring funky/
▌ ✔ Success 
▌ Processing: 2593b80b7a1bc722b2b003884ccd408c39274d10a89a00ae2c9cee6de78ff34b (Mach-O executable: arm64) 
▌ ✖ Error 
▌ Skipping: .DS_Store (Not a Mach-O executable) 
▌ ✔ Success 
▌ Processing: 295f1aaaddfbb885db2afbf692e333ad4ce560f1bd0160092d132228f5f92671 (Mach-O executable: Fat) 
▌ ✔ Success 
▌ Processing: 289213fe74c50b120084216bd23ce9e820126e4fa605039707f51e01c8080a24 (Mach-O executable: Fat) 
▌ ✔ Success 
▌ Common strings saved in: funky//common_strings.txt 
▌
▌ Recommended next steps: 
▌  ▸ Review the text file and look for strings that might be specific to the mach-o executables
▌  ▸ If it's to generic or you don't find much strings, consider using Symhash CLI to sort executables on symhash and use the 'cstring -r' flag to extract common strings recursively
```

## Tip

If you have my other CLI `symhash` installed which you can download [here](https://github.com/txhaflaire/symhash) which is a CLI that can help calculate symhashes and cdhashes for given executables or a directory of executables but also has features to organize executables in subfolders all linked back to their symhash.

Let's say i would run `symhash /path/to/folder/with/binaries -o` it would create subfolders for each symhash and move the binaries in their symhash related subfolder. What we then can do is run `cstring /path/to/folder/with/binaries/ -r` and for each subfolder the common strings will be calculated and stored in a common_strings.txt file in each subfolder, pretty cool.


```
➜  Downloads git:(main) symhash Funky/ -o
▌ ✖ Error 
▌ Not a valid Mach-O slice: /Users/txhaflaire/Downloads/Funky/.DS_Store 
▌ ✔ Success 
▌ Organization complete 
▌
▌ Recommended next steps: 
▌  ▸ Organized 4 files into symhash-based folders under:
▌  ▸ '/Users/txhaflaire/Downloads/Funky/'
```

```
➜  Downloads git:(main) cstring funky/ -r                             
▌ ✔ Success 
▌ Processing: 2593b80b7a1bc722b2b003884ccd408c39274d10a89a00ae2c9cee6de78ff34b (Mach-O executable: arm64) 
▌ ✖ Error 
▌ Skipping: .DS_Store (Not a Mach-O executable) 
▌ ✖ Error 
▌ Skipping: .DS_Store (Not a Mach-O executable) 
▌ ✔ Success 
▌ Processing: 295f1aaaddfbb885db2afbf692e333ad4ce560f1bd0160092d132228f5f92671 (Mach-O executable: Fat) 
▌ ✔ Success 
▌ Processing: 289213fe74c50b120084216bd23ce9e820126e4fa605039707f51e01c8080a24 (Mach-O executable: Fat) 
▌ ✔ Success 
▌ Recursive extraction complete 
▌
▌ Recommended next steps: 
▌  ▸ Processed 3 Mach-O executables across 2 subfolders.
▌  ▸ Common strings were written to each subfolder’s 'common_strings.txt' file.
▌  ▸ Review results under: '/Users/txhaflaire/Downloads/funky/'
```



