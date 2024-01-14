# Software Design Patterns in Python

Within this repository, you'll discover a comprehensive exploration of software design patterns. Included are Python code samples and a comparative analysis highlighting distinctions among various patterns.

## Table of Contents

- [Software Design Patterns in Python](#software-design-patterns-in-python)
- [Definitions](#definitions)
    - [Factory Pattern](#factory-pattern)
    - [Mixin Pattern](#mixin-pattern)
- [Factories vs. Mixins](#factories-vs-mixins)
    - [Example Factory](#example-factory)
    - [Example Mixins](#example-mixins)
    - [Differnces](#differences)
    - [Conclusion](#conclusion)


## Definitions

### Factory Pattern

The Factory Pattern is a creational design pattern that defines an interface for creating objects in a superclass but allows subclasses to alter the type of objects that will be created. It involves declaring an abstract method, known as the factory method, which subclasses implement to produce instances of a class.

### Mixin Pattern

Mixins are a design pattern in object-oriented programming that allows the combination of behaviors from multiple classes in a single class. The idea is to create small, reusable classes that encapsulate a specific behavior or functionality and then combine them to create a new class with a composition of these behaviors.

## Factories vs. Mixins

To compare both approaches, I try illustrate their distinctions through the creation of diverse document types. Initially, we examine the factory method, followed by the implementation of equivalent functionality using the mixin pattern.

### Example Factory

```python
from abc import ABC, abstractmethod

# The Product interface, representing the product created by the factory
class Document(ABC):
    @abstractmethod
    def create_header(self):
        pass

    @abstractmethod
    def create_body(self):
        pass

# Concrete Product 1: Resume
class Resume(Document):
    def create_header(self):
        return "Resume Header"

    def create_body(self):
        return "Resume Body"

# Concrete Product 2: Report
class Report(Document):
    def create_header(self):
        return "Report Header"

    def create_body(self):
        return "Report Body"

# The Creator interface, declaring the factory method for creating products
class DocumentFactory(ABC):
    @abstractmethod
    def create_document(self):
        pass

# Concrete Creator 1: ResumeFactory
class ResumeFactory(DocumentFactory):
    # Factory Method that creates a Resume product
    def create_document(self):
        return Resume()

# Concrete Creator 2: ReportFactory
class ReportFactory(DocumentFactory):
    # Factory Method that creates a Report product
    def create_document(self):
        return Report()

# Client function to generate documents.
def generate_document(factory):
    # Use the factory to create a document
    document = factory.create_document()
    
    header = document.create_header()
    body = document.create_body()
    
    return f"{header}\n{body}"

if __name__ == "__main__":

    resume_factory = ResumeFactory()
    print(generate_document(resume_factory))
    # Output: Resume Header\nResume Body

    report_factory = ReportFactory()
    print(generate_document(report_factory))
     # Output: Report Header\nReport Body
```

### Example Mixins

```python
# Mixin class providing functionality to add a 'source' attribute to a document.
class AddSourceMixin:

    def add_source(self, source):
        self.source = source

# Mixin class providing functionality to add 'certificates' attribute to a document.
class AddCertificatesMixin:

    def add_certificates(self, certificates):
        self.certificates = certificates

# Base class representing a generic document.
class Document:

    def __init__(self, title, content):
        self.title = title
        self.content = content

    def display(self):
        print(f"Title: {self.title}")
        print("Content:")
        print(self.content)

# Class representing a report, inherits from Document and uses AddSourceMixin.
class Report(Document, AddSourceMixin):

    def __init__(self, title, content, author):
        super().__init__(title, content)
        self.author = author

    def display(self):
        super().display()
        print(f"Author: {self.author}")

# Class representing a resume, inherits from Document and uses AddCertificatesMixin.
class Resume(Document, AddCertificatesMixin):

    def __init__(self, title, content, applicant_name):
        super().__init__(title, content)
        self.applicant_name = applicant_name

    def display(self):
        super().display()
        print(f"Applicant Name: {self.applicant_name}")

if __name__ == "__main__":
    report1 = Report("Quarterly Report", "Financial summary for Q1 2024...", "John Doe")
    report1.add_source("Finance Department")
    report1.display()

    resume1 = Resume("Software Developer Resume", "Experienced Python developer...", "Jane Smith")
    resume1.add_certificates(["Python Developer Certificate", "Software Engineering Certificate"])
    resume1.display()

```

### Differences

#### Factory Pattern

- Advantages:
  - Encapsulation of Product Creation:
    The creation of product instances is encapsulated within the factory classes (ResumeFactory and ReportFactory), providing a clear separation of concerns.
  - Scalability:
    It's easy to extend the system by adding new concrete product classes and corresponding factory classes without modifying existing code. This makes the pattern scalable and flexible.
  - Consistency:
    The pattern enforces a consistent interface for creating products, ensuring that all products adhere to the same set of methods.
  - Abstraction:
    The use of abstract classes (e.g., Document, DocumentFactory) allows for the creation of families of related or dependent objects.

- Disadvantages:
  - Complexity:
    Introducing abstract classes and interfaces can make the codebase more complex, especially for small projects where simplicity might be preferred.
  - Boilerplate Code:
    The factory pattern can introduce some boilerplate code due to the definition of abstract classes and interfaces.
  - Learning Curve:
    Developers not familiar with the pattern might find it challenging to understand and implement at first.

#### Mixins

- Advantages:
  - Simplicity:
    The use of mixins is a straightforward way to add specific functionalities to classes without the need for complex hierarchies or abstract classes.
  - Flexibility:
    Mixins allow for a more dynamic composition of functionalities. Classes can easily mix and match different mixins based on their requirements.
  - Readability:
    The code is concise and may be more readable, especially for simpler scenarios.
  - No Abstraction Overhead:
    There is no need for abstract classes or interfaces, reducing the abstraction overhead.

- Disadvantages:
  - Potential for Name Clashes:
    If multiple mixins define methods or attributes with the same name, there might be conflicts and unintended behaviors.
  - Less Strict Contract:
    Mixins don't enforce a strict contract or interface like abstract classes, potentially leading to less predictability in the usage of classes.
  - Limited Abstraction:
    Mixins might not be the best solution for complex systems where a high level of abstraction and structure is needed.
  - Composition Challenges:
    In large projects, managing the composition of multiple mixins can become challenging.

### Conclusion

- Use the factory pattern for larger projects where a structured approach to product creation is needed.
- The factory pattern adds some complexity but provides scalability and maintainability.
- Mixins are suitable for simpler scenarios or when flexibility in composition is essential.
- Mixins provides a more straightforward way to extend functionality without introducing complex hierarchies.
