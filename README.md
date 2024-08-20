# Result<T,E> in C++
One of the most useful types included in the Rust standard library is the Result type.  

This allows for easy handling of errors as values, and this file allows for the use of this type in a C++ environment!

This is a single header file library, so to include it in your project just yank the `result.h` header file and place it in your desired directory!

## Usage
The API follows the Rust Result<T,E> api (here)[https://doc.rust-lang.org/std/result/].
Here is a basic function that returns a Result:
```c++
enum class Error {
    BAD_NAME,
    BAD_AGE,
};

struct Person {
    Person(std::string name, int age)
    : name(name), age(age) {}
    
    std::string name;
    int age;
};

std::set<std::string> name_list;

result::Result<Person, Error> new_person(const std::string& name, int age) {
    // Check for errors
    if (name_list.find(name) != name_list.end()) {
        return result::Err(Error::BAD_NAME);
    }

    if (age < 0) {
        return result::Err(Error::BAD_AGE);
    }

    // Return the Ok result with the Person
    return result::Ok(Person(name, age));
}
```

Here is how you would go about using a received Result:
```c++
void print() {
    result::Result<Person, Error> person_res = new_person("Tyrell", 22);

    // No match statement in the C++ standard so we use if-else :(
    if (person_res.is_ok()) {
        // Get the Ok value from the result
        Person p = person_res.unwrap();
        printf("Name: %s. Age: %d\n", p.name.c_str(), p.age);
    } else if (person_res.is_err()) {
        // You can retrieve the error 
        // from the Result like below.
        Error err = person_res.unwrap_err();
        switch(err) {
            case Error::BAD_NAME:
                printf("Bad Name\n");
                break;
            case Error::BAD_AGE:
                printf("Bad Age\n");
                break;
        }
    }
}
```
