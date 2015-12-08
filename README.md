# atom-erlang-autocompile
Auto compiling erlang modules when saving them.

Erlang auto compile attmepts to compile module placed in the file you're editing.
Plugin supports running shell commands after compilation. For exmaple if you want to run tests for a particular module you can do that by extending atom config.

## Messages
Erlang auto compile uses standard atom notifications as feedback channel.
In default configuration on successfull compilation you won't get any message. When you have compilation error/warning it will be shown as notification warning.
Same principle applies for after compile scripts which you may want to run. 
Error notification is shown when stderr occurs.

## Config
### Wildcards 
When file is saved there is three wildcards available which yu can use in oyur config:

* **%module%** - name of the module
* **%cwd%** - path to the file
* **%file%** - file name of the file 

### Adding Custom erlc path
You can configure plugin in many ways. For example you might want to specify path for 'erlc' if is not on your path(which is aparently common case for OS X users). In order to do this add in your config.cson following lines:

```
  erlangAutocompile:
      compileCommand: "/usr/local/bin/erlc"
```

### Adding message on successfull compilation.
```
  erlangAutocompile:
    compileCommand: "/usr/local/bin/erlc"
    onCompile:
      message: "Compiled! : %cwd%%module%"
```

### After compile shell commands

Lets say you have folder with modules which have test() method implemented in ech one of them. You would like to run test() method ech time you save any module in this particular module. Afte compile shell commands may be an option for you:

```
  erlangAutocompile:
    compileCommand: "/usr/local/bin/erlc"
    onCompile:
      message: "Compiled! : %cwd%%module%"
    afterCompile:
      runTests1:
        whenCwd: "/usr/local/fun/"
        command: "/usr/local/bin/erl"
        args: "-noshell -s %module% test -s init stop"
        onSuccess:
          message: "tests succeed at %cwd%%module%"
```

It is possible to run more thn one shell command after compilation:

```
      runTests2:
        whenCwd: "/usr/local/fun/"
        command: "/usr/local/bin/erl"
        args: "-noshell -s %module% test -s init stop"
        onSuccess:
          message: "tests succeed at %cwd%%module%"
```

* runTest2 will run only when runTest1 succeeds basically will run the same test().
* **whenCwd** - if whenCwd provided shell command will run only if **%cwd%** starts from whenCwd. For Example if your file is located as '/usr/local/fun' and whenCwd is '/usr/local/' than command will run. 
