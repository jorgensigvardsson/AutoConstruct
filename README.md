# AutoConstruct
Tool for auto inject mocks into constructors during test. Minimizes required setup code.

## Rationale

If a class has many constructor dependencies, it becomes increasingly impractical to
unit test the class, as each instantiation will require the setup of many mocks.
It is entirely possible that a method that is being tested, does not depend on all constructor
dependencies. In such cases, having the setup of all those dependencies/mocks is too impractical.

Another advantags is that if the contructor dependencies changes for a class that has many unit tests,
it will still compile, enabling red/green testing without having to "fix" all constructor calls.

# Usage
## Remarks
Once an instance has been created of the specified type, this ConstructorContext is used, and cannot be used again. Each object needs its own ConstructorContext.
    
## Example
This is an example of how the class may be used. Given the types:
    public interface ISomeInterface {
         void SomeOperation();
    }
    
    public class Class {
        private readonly ISomeInterface m_i;
    
        public Class(ISomeInterface i) {
           m_i = i;
        }
    
        public void DoStuff() {
           m_i.SomeOperation();
        }
    } 

We can write tests like this:
    var context = new ConstructorContext<Class>();
    var mock = context.Inject(new Mock<ISomeInterface>());
    
    mock.Setup(m => m.SomeOperation());
    
    var objectUnderTest = context.New();
    objectUnderTest.DoStuff();

# Dependencies
This project depends on Moq.