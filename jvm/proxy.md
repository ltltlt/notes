# jvm proxy

## AspectJ

Static proxy(compile time), use specific compiler to generate java bytecode.

## JDK native

Dynamic proxy(runtime), rely on interface.

## Cglib, Javassist

Dynamic proxy(runtime), operate source bytecode to generate subclass or whatever. Rely on ASM.

## Spring proxy

Use AspectJ syntax, but use Cglib to do the real thing.
