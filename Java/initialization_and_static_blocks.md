### Instance Intialization Block

- Used to initialize the data members generally of an object.
- Called once per object created
- called before any constructor (so that it is able to house common constructor initialization code)
- Constructore hence, can easily override the instance initialization block.
- Importantly, it is called after the super constructor - if there is parent-child relationship.

Super constructor --> instance initializer block --> constructor 

