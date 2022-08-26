# Aula02: Introdução aos sistemas operacionais

### Objectives
* Identify services provided by an operating system
* Illustrate how system calls are used to provide operating system services
* Compare and contrast monolithic, layered, microkernel, modular, and hybrid strategies for designing operating systems
* Illustrate the process for booting an operating system
* Apply tools for monitoring operating system performance
* Design and implement kernel modules for interacting with a Linux kernel

### Operating Systems Services
* OS's provide an enveironment for execution of programs and services to programs and users
* One set of OS services provides functions that are helpful to the user
    * **User interface**: almost all OS have a user interface (UI)
        * Varies between **command-line (CLI), GUI, touch-screen, Batch**
    * **Program execution**: the system must be able to load a program into memory and to run that program, and execution, either normally or abnormally
* One set of OS services provides functions that are helpful to the user:
    * **Communications**: processes may exchange informateion, on the same computer or between computers over a network
    * **Error detection**: OS needs to be constantly aware of possible errors

### Command Line interpreter
* CLI allows direct command entry
* Sometimes implemented in kernel, sometimes by systems program
* Sometimes multiple flavours implemented: **shells**
* Primarily fetches a command from user and exectutes it
* Sometimes commands built-in, sometimes just names of programs
    * If the latter, adding new features doesn't require shell modification

### System calls
* Programming interface to the services provided by the OS
* Typically written in a high-level language (C or C++)
* Mostly accessed by programs via a high-level **Application Programming Interface (API)** rather than direct system call use

### Anotações
* Apenas conversamos com o sistema operacional através de _syscall_ por segurança
* MacOS é mais microkernel
* BIOS: 
    * ao ligar uma máquina, o processador deve apontar a algum endereço
    * o primeiro endereço que o processador aponta é o endereço da BIOS
    * na BIOS, estão declaradas os endereços das configurações de inicialização 
    * a BIOS fica na home
    * está sendo substituída pelo UEFI
* GRUB:
    * gerenciador de boot (bootloader/bootstrap)
    * precisa ser carregado na RAM
* Processador não executa serviços em disco, ele sempre precisa carregar na memória
* Checar: 
    * _gprof_
    * _kprintf_
* O SO acessa o disco pelos device drivers
* Cada arquivo no sistema tem um número único de identificação
* O SO possui vários serviços; cada serviço é identificado por um número de system call
* **Modo kernel**:
    * O SO seta um bit para identificar se está em modo kernel ou não
    * Não tem como acessar diretamente uma região de memória do kernel sem o modo kernel
    * Quando você cai em em modo kernel, você vai para outra região do processador
    * 
* Três modos de passagem de parâmetros:
    * Mais simples: por registradores
* Bootloader: 
    * carrega um programa
    * fica escutando para ver se o usuário quer carregar algum programa
    * muitos sistemas não usam SO, usam **HAL**:
        * Um conjunto de bibliotecas estático
* Objeto != Executável:
    * Um objeto já é um assembly, mas não está bem definido o endereçamento, tanto para definições de variáveis quanto funções
