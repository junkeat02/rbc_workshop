__Name: Lee Jun Keat__
__Course: Mechanical engineering 3rd year__
__Department: Software (Ros)__


# Python beginner workshop

![PYTHON ICON](https://www.python.org/static/img/python-logo@2x.png)

## Fundamentals

### 1. Syntax
i. ***Comment:***

To add a note in your code to explain the sections and have no effect to your execution or interpretation.

    # Comments 

ii. ***Operation:***

    addition = 1 + 1
    subtration = 1 - 1
    multiplication = 1 * 1
    division = 1 / 1
    power = 1 ** 1
    modulus = 1 // 1

iii. ***Print function:***

    Display result in the terminal.

    Exp: print("hello world")

iv. ***Index:***

    Start from 0.

<hr>

### 2. Loop

<hr>

__For loop:__<br>

It is a **numerical count** loop.

Exp:
```
for x in range(3):
    print(x)

for x in ["you", "me", "I"]:
    print(x)
```

Expected result:<br>
0<br>
1<br>
2

<hr>

__While loop:__<br>

It is a **conditional** loop.
<br><br>
Exp of infinite loop:
```
import time
n = 0
while True:
    n += 1
    print(n)
    time.sleep(1)
```
Expected result:<br>
1<br>
2<br>
3<br>
4<br>
...

<hr>

### 3. Array
<hr>

![arrays](https://www.devopsschool.com/blog/wp-content/uploads/2022/10/python-list-tuple-set-array-dict-6-1024x543.jpg)

__List__ <br>
    

```
myList = [1, 34, 2, 4]

print(myList[0])
print(myList[-1])

myList.append(5)
print(myList)

myList.pop(-1)
print(myList)

myList[0] = 0
print(myList)

```

__Tuple__
```
myTuple = (2, 3, 41, 4)
print(myTuple[0])

# immutable
# cannot add or remove elements of tuple
# myTuple.append(33)
# print(myTuple)

# cannot change the element in the tuple
# myTuple[0] = 1
# print(myTuple)
```
__Set__
```
# not iterable
mySet = {2, 3, 21, 2}
print(mySet)

# can add item
mySet.add(4)
print(mySet)

# can remove item
mySet.pop()
print(mySet)
```

__Dictionary__

```
myDict = {"MyName": "Lee", "Age": 22}
print(myDict)

print(f"My name is {myDict["MyName"]}")
print(f"I'm {myDict['Age']} years old.")

myDict["Marriage status"] = "Single"
print(myDict)
```

### 4. Function
```
# In this case, username, id, and age are the parameter for the profile function
def profile(username, id, age):  
    user = username
    user_id = id
    user_age = age
    return f"Username: {user}. \nUser ID: {user_id}. \nUser's Age: {age}. \n"


if __name__ == "__main__":
    
    # The "Lee", 1, and 22 will be argument corresponds to the respective parameter of the profile function
    
    # Positional argument
    myProfile = profile("Lee", 1, 22)
    
    # Keyword argument
    myProfile = profile(username="Lee", id=1, age=22)
    
    # Hybrid argument
    # Keyword argument must be after the positional argument
    myProfile = profile("Lee", id=1, age=22)
    
    print(myProfile)
```
### 5. OOP

## Exercise
### 1. Calculator


# C++ beginner workshop
<!-- ![C++ ICON](https://upload.wikimedia.org/wikipedia/commons/thumb/1/18/ISO_C%2B%2B_Logo.svg/459px-ISO_C%2B%2B_Logo.svg.png) -->

## Fundamentals

### 1. Syntax
i. ***Comment:***

    // comments

ii. ***using namespce std:***

std is a namespace that stands for "standard." It contains all the standard C++ library functions, objects, and classes, like cout, cin, vector, and many others.

```
cout << "Hello world" << endl;
```


iii. ***Operation:***

- additional
- subtraction
- multiplication
- division

iv. ***cmath library:***

A library that contains most of the mathematical tools like power of and sin, cos, tan.

v. ***Datatype:***

- int
- float
- char
- string

<br><br>

Fundamental code:

```
#include <iostream>
#include <cmath>
using namespace std;
int main() {
    // Write C++ code here
    cout << "Hello everyone." << endl;

    // Addititon
    int addition = 1 + 1;
    
    // Subtraction
    int subtraction = 1 - 1;
    
    // Multiplication
    int multiplication = 1 * 2;
    
    // Division
    int division = 10 / 2;

    // Power 
    int base = 2;
    int exponent = 2;
    int power = pow(base, exponent);

    cout << addition << endl;
    cout << subtraction << endl;
    cout << multiplication << endl;
    cout << division << endl;
    cout << power << endl;

    return 0;
}
```


### 2. Loop

```
#include <iostream>

using namespace std;

int main(){
    
    // For loop
    int no_of_loops = 5;
    cout << "For loop: " << no_of_loops << " loops" <<endl;
    for (int i = 0; i < no_of_loops; i++){
        cout << i << endl;
    }

    cout << endl;

    // While loop
    no_of_loops = 4;
    cout << "While loop that loop when n is less then " << no_of_loops << endl;
    int n = 0;
    while (n < 5){
        cout << n << endl;
        n++;
    }

    return 0;
}
```

### 3. Array

```
#include <iostream>

using namespace std;

int main(){

    int array1[3] = {1, 2, 3};
    cout << array1[0] << endl;
    cout << array1[2] << endl;

    return 0;
}
```

### 4. Function

```
#include <iostream>

using namespace std;

// declaration - for those function or class below our main function
int state_int(int);


// void function (no need return)
void say_hello(){
    cout << "Hello" << endl;
}


int main(){
    say_hello();
    state_int(1);
    return 0;
}

// datatype function (need to return something)
int state_int(int n){
    cout << n << endl;
    return n;
}
```

### 5. OOP 
```
#include <iostream>

using namespace std;

class myProfile{
    private:
        string myname = "Lee";
        int age = 22;
        string marriage_status = "single";
    public:
        string appearance = "not handsome";
        void get_name(){
            cout << myname << endl;
        }
        
        int get_age(){
            cout << age << endl;
            return age;
        }

        void get_marriage_status(){
            cout << marriage_status << endl;
        }

};


int main(){
    myProfile profile;

    profile.get_age();
    profile.get_name();

    return 0;
}
```
