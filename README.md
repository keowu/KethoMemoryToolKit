<h1 align="center"> KethoMemoryToolKit </h1>
<p align="center"> KethoMemoryToolKit is a set of tools used by me to change OPCODES of the memory of any process running in the operating system of specific offsets(RVA) from an ImageBase.</p>
<h2 align="center">Examples of use: </h2>

<h3 align="center">Let's create an instance for KethoLin</h3>

```c++
    /// <summary>
    ///    Create a new instance for KethoLib
    /// </summary>
    KethoLib* ketholib = new KethoLib();
```

<h3 align="center">Let's open a process running on the operating system so that we can freely modify it:</h3>
<ul align="center">
  <li>You need to enter the name of the process</li>
  <li>A Handle will be returned for the process</li>
</ul>

```c++
    /// <summary>
    ///     Opens a process
    /// </summary>
    /// <returns>Returns the Process Handle</returns>
    HANDLE hProcess = ketholib->abrirProcessoPeloNome(L"myprocess.exe");
```

<h3 align="center">We will obtain the moduleBase of the module in which we want to modify the process, for that it is enough that it is loaded in memory, in this case we will obtain the basis of the process in question:</h3>

<ul align="center">
  <li>Use the KethoLib method (getProcessInfo) it will return a DWORD with the process PID</li>
  <li>Finally pass the name of the base you want to load, if it is the base of the process, repeat the name again.</li>
</ul>

```c++
    /// <summary>
    ///     Obtains the base module of any defined module of the process, even of itself
    /// </summary>
    /// <returns>An uintptr_t containing the Module Base</returns>
    uintptr_t modulebase = ketholib->getModuleBaseAddress(ketholib->getProcessInfo(L"myprocess.exe"),
        L"myprocess.exe");
```

<h3 align="center">We will prepare the vectors containing the addresses (RVA) and the modified Opcodes to modify in the process memory:</h3>

<ul align="center">
  <li>Prefer to use the <strong>unsigned int</strong> data type for the vector.</li>
</ul>

```c++
    /// <summary>
    ///     We define the Offsets (RVA), where the Assembly instructions (Opcodes) that we want to modify are present
    /// </summary>
    std::vector<unsigned int> x64PatchAddress = { 0x8E953E, 0x8E953F, 0x8E9540,
                                                  0x8E9541, 0x937565, 0xBBD333,
                                                  0xBBD334, 0xBBD335, 0x1929EF6 };

    
    /// <summary>
    ///     We define the Opcodes that will be modified in memory, one for each equivalent RVA address position.
    /// </summary>
    std::vector<unsigned int>x64AssemblyOpcodes = {
                                                     0xE9, 0x07, 0x01,
                                                     0x00, 0xEB, 0xE9,
                                                     0x8C, 0x00, 0xEB };
```

<h3 align="center">Let's prepare an array to read the Opcodes from the process memory:</h3>

```c++
    /// <summary>
    ///     Vector to store OPCODES (Hex) read from process memory
    /// </summary>
    std::vector<uint8_t>bbyte;
```

<h3 align="center">Let's call the method responsible for reading the opcodes for each RVA in memory individually and retrieving the OPCODES:</h3>

```c++
    /// <summary>
    ///     This method will read opcodes for a given RVA from the process memory
    ///     the result will be stored in std :: vector <uint8_t> passed as a pointer
    /// </summary>
    ketholib->readMemoryOpcodes(hProcess, modulebase, x64PatchAddress, &bbyte);
```

<h3 align="center">We will replace the opcodes for the RVA addresses in question with our new modified Opcodes:</h3>

```c++
    /// <summary>
    ///     This method will replace opcodes for a given RVA from process memory
    ///     addresses must be passed together with their respective opcode patches for each position
    /// </summary>
    ketholib->writeMemoryOpcodes(hProcess, modulebase, x64PatchAddress, x64AssemblyOpcodes);
```

<h3 align="center">Example showing the contents of the OPCODES read from the process memory:</h3>

```c++
    /// <summary>
    ///     We will write on the screen the contents (OPCODES) read from the process memory
    /// </summary>
    std::cout << "\nAssembly Original: \n";

    std::cout << "Byte:: " << std::endl;

    for (int i = 0; i < bbyte.size(); ++i) {
        std::cout << std::hex << unsigned(bbyte.at(i)) << " ";
    }
```

<h3 align="center">For more examples</h3>
<p align="center">See the source file: <strong>KethoLibraryCodeExemple.cpp</strong></p>
<p align="center">Or watch the following video tutorial: <strong>COMING SOON!</strong></p>

<h2 align="center">Copyright (C) Keowu | KethoLib OpenSource | please leave a like in the repository if this library is useful for you</h2>
