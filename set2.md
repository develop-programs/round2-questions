Below are 20 C error-solving questions tailored for a first-year college-level competition. Each snippet is deliberately longer (15–20 lines) and includes multiple issues—memory management, pointer misuse, logic errors, and more. Participants must debug these real-world–style code examples.

---

### **1. Use-After-Free and Dangling Pointer**
```c
#include <stdio.h>
#include <stdlib.h>

int processData(int *data, int size) {
    int sum = 0;
    for (int i = 0; i < size; i++) {
        sum += data[i];
    }
    return sum;
}

int main() {
    int *arr = (int *)malloc(5 * sizeof(int));
    if (arr == NULL) return 1;
    for (int i = 0; i < 5; i++) {
        arr[i] = i * 10;
    }
    int total = processData(arr, 5);
    free(arr);
    // Error: Using freed memory in further processing.
    printf("Total: %d\n", processData(arr, 5)); 
    return 0;
}
```
*Issue:* Accessing memory after free.

---

### **2. Double Free and Memory Corruption**
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    char *buffer = (char *)malloc(50 * sizeof(char));
    if(buffer == NULL) return 1;
    
    for (int i = 0; i < 49; i++) {
        buffer[i] = 'A';
    }
    buffer[49] = '\0';
    
    printf("Buffer: %s\n", buffer);
    free(buffer);
    // Error: Second free on same pointer.
    free(buffer);
    
    // Additional operations using freed pointer
    buffer = NULL;
    return 0;
}
```
*Issue:* Double free leads to memory corruption.

---

### **3. Deep Recursion Causing Stack Overflow**
```c
#include <stdio.h>

void recursiveFunc(int n) {
    int arr[5000] = {0}; // Large local array each call
    printf("n = %d\n", n);
    if(n <= 0) return;
    // Error: Missing proper termination; also heavy stack usage.
    recursiveFunc(n - 1);
    // Extra logic that never gets executed due to recursion depth.
    arr[0] = n;
    printf("Value: %d\n", arr[0]);
}

int main() {
    // Error: Very high recursion depth.
    recursiveFunc(10000);
    return 0;
}
```
*Issue:* Deep recursion and heavy stack allocation lead to overflow.

---

### **4. Incorrect Pointer Arithmetic and Out-of-Bounds Access**
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int size = 10;
    int *data = (int *)malloc(size * sizeof(int));
    if(data == NULL) return 1;
    
    // Populate array
    for(int i = 0; i < size; i++) {
        data[i] = i + 1;
    }
    
    // Error: Pointer arithmetic goes out-of-bounds.
    int *ptr = data + 15;  
    printf("Value: %d\n", *ptr);
    
    free(data);
    return 0;
}
```
*Issue:* Invalid pointer arithmetic beyond allocated memory.

---

### **5. Uninitialized Pointer Usage**
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *ptr; // Declared but uninitialized
    // Error: Dereferencing an uninitialized pointer.
    *ptr = 100;
    printf("Value: %d\n", *ptr);
    
    // Later initialization is forgotten
    ptr = (int *)malloc(sizeof(int));
    if(ptr) {
        *ptr = 200;
        printf("New Value: %d\n", *ptr);
    }
    free(ptr);
    return 0;
}
```
*Issue:* Using an uninitialized pointer before allocation.

---

### **6. Buffer Overflow in String Manipulation**
```c
#include <stdio.h>
#include <string.h>

int main() {
    char input[10];
    char buffer[10] = "Hello";
    printf("Enter a string: ");
    // Error: Using gets() or unchecked input can overflow buffer.
    scanf("%s", input);
    
    // Concatenate strings without checking lengths.
    strcat(buffer, input);
    printf("Resulting string: %s\n", buffer);
    
    // Loop to print characters (potential overflow in loop condition)
    for (int i = 0; i <= strlen(buffer); i++) {
        printf("%c ", buffer[i]);
    }
    printf("\n");
    return 0;
}
```
*Issue:* Buffer overflow from unchecked string operations.

---

### **7. Mismatched Function Declaration and Definition**
```c
#include <stdio.h>

// Function declaration with wrong return type
int computeSum(int a, int b);

int main() {
    int result = computeSum(5, 15);
    printf("Sum: %d\n", result);
    return 0;
}

// Function defined with void instead of int, and missing return.
void computeSum(int a, int b) {
    int sum = a + b;
    printf("Computed Sum: %d\n", sum);
}
```
*Issue:* Declaration and definition mismatch; wrong return type and missing return.

---

### **8. Returning Address of Local Variable**
```c
#include <stdio.h>

int* getNumber() {
    int num = 42; // Local variable on stack
    // Error: Returning address of a local variable.
    return &num;
}

int main() {
    int *value = getNumber();
    // Undefined behavior: accessing a dangling pointer.
    printf("Number: %d\n", *value);
    
    // Additional processing
    for (int i = 0; i < 5; i++) {
        printf("Loop %d\n", i);
    }
    return 0;
}
```
*Issue:* Returning a pointer to a local variable.

---

### **9. Modifying a String Literal**
```c
#include <stdio.h>

int main() {
    // Error: string literal is read-only.
    char *str = "College";
    // Attempt to modify the literal.
    str[0] = 'M';
    
    // Loop through characters.
    for (int i = 0; i < 7; i++) {
        printf("%c ", str[i]);
    }
    printf("\n");
    return 0;
}
```
*Issue:* Attempting to modify read-only memory.

---

### **10. Concurrency Issue: Race Condition in Threads**
```c
#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>

int sharedCounter = 0;

void* increment(void *arg) {
    for (int i = 0; i < 100000; i++) {
        // Error: No synchronization; race condition exists.
        sharedCounter++;
    }
    return NULL;
}

int main() {
    pthread_t t1, t2;
    
    if(pthread_create(&t1, NULL, increment, NULL) != 0) {
        perror("Thread creation error");
        exit(1);
    }
    if(pthread_create(&t2, NULL, increment, NULL) != 0) {
        perror("Thread creation error");
        exit(1);
    }
    
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);
    
    // Expected value should be 200000 but may vary.
    printf("Shared Counter: %d\n", sharedCounter);
    return 0;
}
```
*Issue:* Race condition due to missing synchronization.

---

### **11. Deadlock with Mutex Locks**
```c
#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>

pthread_mutex_t mutexA, mutexB;

void* task1(void *arg) {
    pthread_mutex_lock(&mutexA);
    // Simulate work.
    for (volatile int i = 0; i < 100000; i++);
    pthread_mutex_lock(&mutexB);
    printf("Task1 completed\n");
    pthread_mutex_unlock(&mutexB);
    pthread_mutex_unlock(&mutexA);
    return NULL;
}

void* task2(void *arg) {
    pthread_mutex_lock(&mutexB);
    // Simulate work.
    for (volatile int i = 0; i < 100000; i++);
    pthread_mutex_lock(&mutexA);
    printf("Task2 completed\n");
    pthread_mutex_unlock(&mutexA);
    pthread_mutex_unlock(&mutexB);
    return NULL;
}

int main() {
    pthread_t thread1, thread2;
    pthread_mutex_init(&mutexA, NULL);
    pthread_mutex_init(&mutexB, NULL);
    
    pthread_create(&thread1, NULL, task1, NULL);
    pthread_create(&thread2, NULL, task2, NULL);
    
    pthread_join(thread1, NULL);
    pthread_join(thread2, NULL);
    
    // Error: Program may deadlock due to circular locking order.
    pthread_mutex_destroy(&mutexA);
    pthread_mutex_destroy(&mutexB);
    return 0;
}
```
*Issue:* Deadlock caused by inconsistent lock ordering.

---

### **12. Integer Overflow in Arithmetic Operations**
```c
#include <stdio.h>
#include <limits.h>

int main() {
    int a = INT_MAX - 1;
    int b = 5;
    // Error: Overflow can occur.
    int result = a + b;
    printf("Result: %d\n", result);
    
    // Loop to simulate more arithmetic operations.
    for (int i = 0; i < 5; i++) {
        result += i;
        printf("Updated Result: %d\n", result);
    }
    return 0;
}
```
*Issue:* Addition may cause overflow beyond integer limits.

---

### **13. Memory Leak with Improper Freeing**
```c
#include <stdio.h>
#include <stdlib.h>

void allocateArray() {
    int *arr = (int *)malloc(20 * sizeof(int));
    if(arr == NULL) return;
    for (int i = 0; i < 20; i++) {
        arr[i] = i * 2;
    }
    // Error: Forgetting to free memory here.
    printf("Array allocated. First element: %d\n", arr[0]);
    // Further processing that ignores the allocated memory.
}

int main() {
    for (int j = 0; j < 10; j++) {
        allocateArray();
    }
    // Error: Memory leak because malloc-ed memory is never freed.
    return 0;
}
```
*Issue:* Memory leak due to not freeing dynamically allocated memory.

---

### **14. Uninitialized Structure Members**
```c
#include <stdio.h>
#include <string.h>

struct Student {
    char name[30];
    int roll;
    float marks;
};

int main() {
    struct Student s;
    // Error: Structure members are not initialized.
    strcpy(s.name, "Alice");
    // Missing initialization for s.roll and s.marks.
    printf("Name: %s\n", s.name);
    printf("Roll: %d, Marks: %.2f\n", s.roll, s.marks);
    
    // Additional dummy processing
    for (int i = 0; i < 3; i++) {
        s.roll += i;
        s.marks += 2.5;
    }
    return 0;
}
```
*Issue:* Using uninitialized structure members can cause unpredictable output.

---

### **15. Infinite Recursion Due to Incorrect Base Case**
```c
#include <stdio.h>

int factorial(int n) {
    // Error: Base case is incorrect or missing for n == 0.
    if(n < 0) return -1;
    if(n == 1) return 1;
    // Error: For n==0, recursion never terminates.
    return n * factorial(n - 1);
}

int main() {
    int num = 0; // Testing for 0, which is not handled correctly.
    int fact = factorial(num);
    printf("Factorial of %d is %d\n", num, fact);
    
    // Additional loop for demonstration.
    for (int i = 0; i < 3; i++) {
        printf("Processing iteration %d\n", i);
    }
    return 0;
}
```
*Issue:* Incorrect/missing base case causes infinite recursion for n = 0.

---

### **16. Off-by-One Error in Array Loop**
```c
#include <stdio.h>

int main() {
    int arr[10];
    // Populate array with error: loop iterates one too many times.
    for (int i = 0; i <= 10; i++) {
        arr[i] = i * 3; // Error: arr[10] is out-of-bounds.
    }
    // Print array elements.
    for (int i = 0; i < 10; i++) {
        printf("%d ", arr[i]);
    }
    printf("\nEnd of array processing.\n");
    return 0;
}
```
*Issue:* Off-by-one error leads to out-of-bound access.

---

### **17. File Handling Errors**
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *fp;
    char filename[] = "data.txt";
    char buffer[50];
    
    // Error: Opening file in read mode that may not exist.
    fp = fopen(filename, "r");
    if(fp == NULL) {
        printf("Error opening file.\n");
        // Not handling file creation.
    }
    
    // Attempt to read file without checking if fp is valid.
    while(fgets(buffer, sizeof(buffer), fp) != NULL) {
        printf("%s", buffer);
    }
    // Error: Not closing the file properly.
    fclose(fp);
    
    // Extra processing after file operations.
    for (int i = 0; i < 5; i++) {
        printf("Processing line %d\n", i);
    }
    return 0;
}
```
*Issue:* File may not exist and error handling is incomplete.

---

### **18. Improper Use of Pointer-to-Pointer and Memory Allocation**
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int **matrix;
    int rows = 3, cols = 3;
    
    // Allocate memory for pointer-to-pointer.
    matrix = (int **)malloc(rows * sizeof(int *));
    if(matrix == NULL) return 1;
    for(int i = 0; i < rows; i++) {
        matrix[i] = (int *)malloc(cols * sizeof(int));
        // Error: Not checking allocation for each row.
    }
    
    // Fill matrix.
    for(int i = 0; i < rows; i++) {
        for(int j = 0; j < cols; j++) {
            matrix[i][j] = i + j;
        }
    }
    
    // Print matrix.
    for(int i = 0; i < rows; i++) {
        for(int j = 0; j < cols; j++) {
            printf("%d ", matrix[i][j]);
        }
        printf("\n");
    }
    
    // Free memory incorrectly (possible memory leak if not freed properly).
    free(matrix);
    return 0;
}
```
*Issue:* Not freeing each allocated row; only the top-level pointer is freed.

---

### **19. Incorrect Type Conversion and Assignment**
```c
#include <stdio.h>

int main() {
    double x = 3.14159;
    // Error: Implicit conversion might lose precision.
    int y = x;
    
    // More complex logic with type mismatch.
    float arr[5] = {0};
    for (int i = 0; i < 5; i++) {
        // Error: Assigning double value to float without explicit conversion.
        arr[i] = x * i;
    }
    
    printf("Integer y: %d\n", y);
    for (int i = 0; i < 5; i++) {
        printf("arr[%d] = %.2f\n", i, arr[i]);
    }
    return 0;
}
```
*Issue:* Implicit type conversions and potential precision loss.

---

### **20. Wrong Format Specifiers in Print Statements**
```c
#include <stdio.h>

int main() {
    int a = 25;
    float b = 3.14;
    char c = 'Z';
    char str[20] = "CollegeC";
    
    // Error: Using wrong format specifiers.
    printf("Integer: %f\n", a);
    printf("Float: %d\n", b);
    printf("Character: %s\n", c);
    printf("String: %c\n", str);
    
    // Additional logic to print variable addresses.
    printf("Address of a: %p\n", (void*)&a);
    printf("Address of b: %p\n", (void*)&b);
    printf("Address of str: %p\n", (void*)str);
    return 0;
}
```
*Issue:* Mismatched format specifiers causing undefined output.

---

Each question is designed to have multiple lines and complexities that require careful analysis. Contestants must identify issues ranging from memory mismanagement to logical errors and synchronization issues. Good luck debugging and learning!
