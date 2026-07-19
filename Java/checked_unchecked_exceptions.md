### Exceptions in Java

- Checked  -> Compile time exceptions which can be checked by compiler. Subclasses of Exception class.
- Unchecked -> Run time excpetions. Subclasses of RuntimeException class.

- finally block will be executed no matter what - whether exception comes or not - doesn't matter if it is caught.
- finally block will not be executed if system.exit() is called in catch block.
