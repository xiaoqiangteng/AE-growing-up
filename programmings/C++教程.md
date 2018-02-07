# C++教程

## 动态内存分配

1. new/delete

	- 在堆上生成对象时，需要自动调用构造函数
	- 在堆上生成的对象，在释放时需要自动调用析构函数
	- new/delete, malloc/free需要配对使用
	- new[]/delete[]生成和释放对象数组
	- new/delete是运算符，malloc/free是函数调用

    例子：
    ```
    Class Test{
    };
    
    Test* pVal = new Test();
    delete pVal;
    pVal = NULL;
    
    int *p = (int \*) malloc(sizeof(int));
    free(p);
    p = NULL;
    
    Test *pArray = new Test[2];
    delete[] pArray;
    ```
    
2. Bss段

