Here are 20 C programming error-solving questions, ranging from easy to hard. Each question contains a code snippet with errors that participants need to fix.

---

### **Easy Level**

#### **1. Missing Semicolon**
```c
#include <stdio.h>

int main() {
    printf("Hello, World!")
    return 0;
}
```

#### **2. Incorrect Data Type**
```c
#include <stdio.h>

int main() {
    int num = 3.14;
    printf("%d", num);
    return 0;
}
```

#### **3. Undeclared Variable**
```c
#include <stdio.h>

int main() {
    x = 10;
    printf("%d", x);
    return 0;
}
```

#### **4. Incorrect Format Specifier**
```c
#include <stdio.h>

int main() {
    float num = 5.5;
    printf("%d", num);
    return 0;
}
```

#### **5. Array Index Out of Bounds**
```c
#include <stdio.h>

int main() {
    int arr[3] = {1, 2, 3};
    printf("%d", arr[3]);
    return 0;
}
```

---

### **Medium Level**

#### **6. Division by Zero**
```c
#include <stdio.h>

int main() {
    int a = 10, b = 0;
    printf("%d", a / b);
    return 0;
}
```

#### **7. Infinite Loop**
```c
#include <stdio.h>

int main() {
    int i = 0;
    while (i < 5) {
        printf("%d", i);
    }
    return 0;
}
```

#### **8. Incorrect String Handling**
```c
#include <stdio.h>

int main() {
    char str[5];
    str = "Hello";
    printf("%s", str);
    return 0;
}
```

#### **9. Memory Leak**
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *ptr = (int *)malloc(sizeof(int));
    *ptr = 10;
    printf("%d", *ptr);
    return 0;
}
```

#### **10. Uninitialized Pointer**
```c
#include <stdio.h>

int main() {
    int *ptr;
    printf("%d", *ptr);
    return 0;
}
```

#### **11. Incorrect Function Return Type**
```c
#include <stdio.h>

void sum(int a, int b) {
    return a + b;
}

int main() {
    printf("%d", sum(5, 10));
    return 0;
}
```

#### **12. Buffer Overflow**
```c
#include <stdio.h>

int main() {
    char name[5];
    scanf("%s", name);
    printf("Hello, %s", name);
    return 0;
}
```

#### **13. Logical Error**
```c
#include <stdio.h>

int main() {
    int a = 5, b = 10;
    if (a > b)
        printf("a is greater");
    else
        printf("b is greater");
    return 0;
}
```

---

### **Hard Level**

#### **14. Dangling Pointer**
```c
#include <stdio.h>

int* getPointer() {
    int num = 10;
    return &num;
}

int main() {
    int *ptr = getPointer();
    printf("%d", *ptr);
    return 0;
}
```

#### **15. Segmentation Fault**
```c
#include <stdio.h>

int main() {
    char *str;
    str = "Hello";
    str[0] = 'M';
    printf("%s", str);
    return 0;
}
```

#### **16. Incorrect Array Initialization**
```c
#include <stdio.h>

int main() {
    int arr[3];
    arr = {1, 2, 3};
    printf("%d", arr[0]);
    return 0;
}
```

#### **17. Incorrect Use of `fgets`**
```c
#include <stdio.h>

int main() {
    char str[10];
    fgets(str, 10, stdin);
    printf("%s", str);
    return 0;
}
```

#### **18. Use of Uninitialized Structure**
```c
#include <stdio.h>

struct Student {
    char name[20];
    int age;
};

int main() {
    struct Student s;
    printf("%s %d", s.name, s.age);
    return 0;
}
```

#### **19. Invalid Memory Access**
```c
#include <stdio.h>

int main() {
    int arr[5] = {1, 2, 3, 4, 5};
    printf("%d", arr[10]);
    return 0;
}
```

#### **20. Stack Overflow**
```c
#include <stdio.h>

void func() {
    int arr[1000000];
    func();
}

int main() {
    func();
    return 0;
}
```

---

These questions progressively test debugging skills, logical thinking, and understanding of C programming concepts. Let me know if you need modifications! 🚀
