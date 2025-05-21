
# 数组
## 一维数组
### 声明数组

```c
// 声明一个包含5个整数的数组
int numbers[5];

// 声明并初始化数组
int primes[] = {2, 3, 5, 7, 11};
```

### 初始化方式

```c
// 完全初始化
int arr1[5] = {1, 2, 3, 4, 5};

// 部分初始化（剩余元素自动初始化为0）
int arr2[5] = {1, 2}; // [1, 2, 0, 0, 0]

// 不指定大小（编译器自动计算）
int arr3[] = {1, 2, 3, 4}; // 大小为4
```

### 遍历数组

```c
int arr[5] = {1, 2, 3, 4, 5};

// 使用for循环
for(int i = 0; i < 5; i++) {
    printf("%d ", arr[i]);
}

// 使用指针
int *ptr = arr;
for(int i = 0; i < 5; i++) {
    printf("%d ", *(ptr + i));
}
```

### 注意事项

**1.数组越界**：C 不检查数组边界，访问超出范围的索引会导致未定义行为

```c
int arr[3] = {1, 2, 3};
printf("%d", arr[5]); // 危险！可能崩溃或输出垃圾值
```

**2.数组大小**：可以使用 `sizeof` 运算符计算数组大小

```c
int arr[10];
int size = sizeof(arr) / sizeof(arr[0]); // 计算元素个数
```

**3.数组赋值**：不能直接对整个数组赋值

```c
int a[5], b[5];
// a = b; // 错误！不能这样赋值
// 必须逐个元素复制
```

## 二维数组

### 规则二维数组遍历

```c
#include<stdio.h>

int main()
{
	int arr[3][5] =
	{
		{1,2,3,4,5},
		{11,22,33,44,55},
		{111,222,333,444,555}
	};

	//arr[i]:表示二维数组中的第i个一维数组

	for (int i = 0; i < 3; i++)
	{
		//i:依次表示二维数组中的suoyin
		for (int j = 0; j < 5; j++)
		{
			//j:依次表示一维数组中的索引  内循环：遍历没一个一维数组
			printf("%d ", arr[i][j]);
		}
		printf("\n");
	}

	return 0;
}
```

### 不规则二维数组遍历

```c
#include<stdio.h>

int main()
{
	int arr1[3] = { 1,2,3 };
	int arr2[5] = { 1,2,3,4,5 };
	int arr3[9] = { 1,2,3,4,5,6,7,8,9 };

	int len1 = sizeof(arr1) / sizeof(arr1[0]);
	int len2 = sizeof(arr2) / sizeof(arr2[0]);
	int len3 = sizeof(arr3) / sizeof(arr3[0]);

	int lenArr[3] = { len1,len2,len3 };

	int* arr[3] = { arr1,arr2,arr3 };

	int len = sizeof(arr) / sizeof(arr[0]);

	for (int i = 0; i < len; i++)
	{
		for (int j = 0; j < lenArr[i]; j++) {
			printf("%d ", arr[i][j]);
		}
		printf("\n");
	}
	
	return 0;
}
```

# 指针

## 概念

+ 指针是一个变量，其值是另一个变量的内存地址。

```c
int var = 20;    // 实际变量
int *ptr = &var; // 指针变量，存储var的地址
```

### 指针运算符

- `&` (取地址运算符)：返回变量的内存地址
- `*` (解引用运算符)：访问指针所指向地址的值

## 指针的基本操作

```c
int var = 10;
int *ptr = &var;

printf("变量 var 的值: %d\n", var);      // 输出: 10
printf("变量 var 的地址: %p\n", &var);   // 输出: var的地址
printf("指针 ptr 存储的地址: %p\n", ptr); // 输出: var的地址
printf("指针 ptr 指向的值: %d\n", *ptr);  // 输出: 10
```

## 指针的运算

```c
int arr[] = {10, 20, 30};
int *ptr = arr; // 指向数组第一个元素

printf("%d\n", *ptr);   // 输出: 10
ptr++;
printf("%d\n", *ptr);   // 输出: 20
ptr++;
printf("%d\n", *ptr);   // 输出: 30
```

## 指针与数组

+ 数组名本质上是一个指向数组第一个元素的常量指针。

```c
int arr[5] = {1, 2, 3, 4, 5};
int *ptr = arr;

// 以下两种方式等价
printf("%d\n", arr[2]);    // 输出: 3
printf("%d\n", *(ptr+2));  // 输出: 3
```

## 多级指针

```c
int var = 300;
int *ptr = &var;
int **pptr = &ptr;

printf("var 的值: %d\n", var);         // 300
printf("*ptr 的值: %d\n", *ptr);       // 300
printf("**pptr 的值: %d\n", **pptr);   // 300
```

## 指针数组和数组指针

### 指针数组

+ 指针数组是一个数组，其元素都是指针。

```c
type *array_name[size];
```

### 数组指针

+ 数组指针是一个指针，它指向一个数组。

```c
type (*pointer_name)[size];
```

### 关键区别

| 特性       | 指针数组                        | 数组指针                 |
| :--------- | :------------------------------ | :----------------------- |
| 本质       | 数组，元素是指针                | 指针，指向数组           |
| 声明       | `type *name[size]`              | `type (*name)[size]`     |
| 内存占用   | 多个指针的空间                  | 单个指针的空间           |
| 常见用途   | 字符串数组、命令行参数          | 处理二维数组             |
| 元素访问   | `array[i]` 返回指针             | `(*ptr)[i]` 访问数组元素 |
| sizeof结果 | 数组总大小（指针数量×指针大小） | 指针大小（通常4或8字节） |

- `type *name[size]`：先是一个数组，然后元素是指针 → 指针数组
- `type (*name)[size]`：先是一个指针，然后指向数组 → 数组指针

- 指针数组：`int *arr[3]` → 如果 `arr[0]` 是一个 `int*`
- 数组指针：`int (*arr)[3]` → 如果 `*arr` 是一个 `int[3]` 数组

# 字符串

## str

### 1. strlen - 计算字符串长度

```c
size_t strlen(const char *str);
```

- 返回字符串长度（不包括结尾的null字符）

### 2. strcpy - 字符串复制

```c
char *strcpy(char *dest, const char *src);
```

- 将src字符串复制到dest（包括null字符）
- 需确保dest有足够空间
- 返回dest指针

###  3.strncpy - 安全字符串复制

```c
char *strncpy(char *dest, const char *src, size_t n);
```

- 最多复制n个字符
- 如果src长度小于n，剩余空间用null填充

### 4. strcat - 字符串连接

```c
char *strcat(char *dest, const char *src);
```

- 将src追加到dest末尾
- 需确保dest有足够空间
- 返回dest指针

### 5. strncat - 安全字符串连接

```c
char *strncat(char *dest, const char *src, size_t n);
```

- 最多追加n个字符
- 自动添加null终止符

### 6. strcmp - 字符串比较

```c
int strcmp(const char *str1, const char *str2);
```

- 比较两个字符串
- 返回值：
  - 0：相等
  - 小于0：str1小于str2
  - 大于0：str1大于str2

### 7. strncmp - 安全字符串比较

```c
int strncmp(const char *str1, const char *str2, size_t n);
```

- 比较前n个字符

### 8. strchr - 查找字符首次出现

```c
char *strchr(const char *str, int c);
```

- 查找字符c在str中第一次出现的位置
- 返回指针或NULL

### 9. strrchr - 查找字符最后出现

```c
char *strrchr(const char *str, int c);
```

- 查找字符c在str中最后一次出现的位置

### 10. strstr - 查找子串

```c
char *strstr(const char *haystack, const char *needle);
```

- 在haystack中查找needle子串
- 返回首次出现位置的指针或NULL

## mem

###  memset - 内存设置

```c
void *memset(void *str, int c, size_t n);
```

- 将str的前n个字节设置为c

### memcpy - 内存复制

```c
void *memcpy(void *dest, const void *src, size_t n);
```

- 从src复制n个字节到dest

### memmove - 安全内存复制

```c
void *memmove(void *dest, const void *src, size_t n);
```

- 处理内存重叠情况的复制

### memcmp - 内存比较

```c
int memcmp(const void *str1, const void *str2, size_t n);
```

- 比较前n个字节

### memchr - 内存字符查找

```c
void *memchr(const void *str, int c, size_t n);
```

- **功能**：在str指向的内存块的前n个字节中查找字符c
- **返回值**：找到则返回指向该位置的指针，否则返回NULL

### memccpy - 带条件的内存复制

```c
void *memccpy(void *dest, const void *src, int c, size_t n);
```

- **功能**：复制直到遇到指定字符c或复制了n个字节
- **返回值**：如果找到c，返回dest中c后面位置的指针，否则返回NULL

# 动态内存

## malloc

```c
char* pstr = (char*)malloc(100*sizeof(char));
if(pstr == NULL)  //创建失败
{
    printf("创建失败\n");
    exit(EXIT_FAILURE);  //退出
}
free(pstr);
pstr = NULL;
```

## realloc

### 扩容原理

1. 申请堆区空间够有足够的内存空间，原堆区空间基础上向后进行扩容操作
2. 申请堆区空间后没有足够的内存空间，重新申请一块足够大的内存空间将原来的内存空间拷贝到新的内存相对位置，释放原来的内存空间realloc将新内存地址返回出来，只需将p指向新地址
3. 申请的内存空间后没有足够的内存，其他地方也没有足够的内存，扩容失败realloc返回值返回NULL

### realloc使用步骤

```c
int* p = (int*)malloc(10*sizeof(int));
if(p == NULL)  exit(EXIT_FAILURE);
//需要扩容时
int* tmp = (int*)realloc(p,2*10*sizeof(int));
if(tmp == NULL)
{
    printf("扩容失败");
    free(p);
    p = NULL;
    return;
}
//扩容成功
p=tmp;
free(p);
p = NULL;
```

# 文件操作

## 文本文件

1. 只读 r ：当文件不存在时，打开失败，存在打开成功
2. 只写 w ：当文件不存在时创建新文件，存在时清空原文件内容
3. 追加写 a ：当文件不存在时创建新文件，存在时不会清空源文件，光标末尾追加写入
4. r+：读写模式
5. w+ ：读写模式
6. a+ ：读写模式

## 二进制文件

+ 文本文件+b "rb" "wb" "ab" "rb+" "wb+" "ab+"

## fread

```c
size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream);
```

- `ptr`：指向存放读取数据的内存缓冲区的指针
- `size`：每个数据项的字节大小
- `nmemb`：要读取的数据项数量
- `stream`：文件指针

## fwrite

```c
size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream);
```

- `ptr`：指向要写入数据的内存缓冲区的指针
- `size`：每个数据项的字节大小
- `nmemb`：要写入的数据项数量
- `stream`：文件指针

## fseek

```c
int fseek(FILE *stream, long offset, int whence);
```

- `stream`：文件指针，指向要操作的文件
- `offset`：偏移量（以字节为单位）
- `whence`：基准位置，可以是以下值之一：
  - `SEEK_SET`：从文件开头开始计算偏移
  - `SEEK_CUR`：从当前位置开始计算偏移
  - `SEEK_END`：从文件末尾开始计算偏移

### 示例

```c
FILE *fp = fopen("example.dat", "rb");

// 定位到文件开头后第100个字节处
fseek(fp, 100, SEEK_SET);
// 从当前位置向前移动50字节
fseek(fp, 50, SEEK_CUR);
// 定位到文件末尾前20字节处
fseek(fp, -20, SEEK_END);
```

## sprintf

```c
sprintf(buff,"hello");//将hello输入到buff字符串中储存
```

## fprintf

````c
fprintf(stdout,"hello");//将hello输出到stdout控制台
fprintf(文件指针,"");//将hello输出到文件中储存
````

## fscanf

```c
fscanf(fp,"name:%s",&name);//从fp文件中获取数据 给变量赋值
```

## 示例

### 储存到文件中

```c
//从键盘输入 按照固定格式存储到文件中
int main()
{
	FILE* fp = fopen("1.txt", "a");
	if (fp == NULL)
	{
		printf("文件打开失败\n");
		return -1;
	}

	char name[10], buff[20];
	int age;
	char res[10];
	while (1)
	{
		printf("是否继续输入?(y or n):");
		scanf("%s", &res);
		if (strncmp(res, "no", 1) == 0)break;
		printf("请输入姓名 年龄:");
		scanf("%s%d", &name, &age);

		sprintf(buff, "name:%s age:%d\n", name, age);//将name和age写入到buff数组中
		//fprintf(fp, "name:%s age:%d\n",name, age);//将name和age写入到fp文件中
		fwrite(buff, 1, strlen(buff), fp);
	}

	fclose(fp);

	return 0;
}
```

### 将文件中的数据读入到内存中

```c
void Init()
{
	FILE* fp = fopen("1.txt", "r");
	assert(fp != NULL);
	char buff[20];
	//从文件中获取数据 赋值变量
	for (int i = 0; i < 6; i++)
	{
		fscanf(fp, "name:%s age:%d\n", &stus[i].name, &stus[i].age);
	}
	fclose(fp);
}
```

# 递归

+ 在调用函数的过程中又出现直接或间接地调用该函数本身，称之为函数的递归调用

## 示例

### 整形一维数组顺序输出

```c
void Show(int* arr, int len)
{
	if (len == 0) return;
	printf("%d ", *arr); 
	Show(arr + 1, len-1);  
}
```

### 逆序输出

```c
void Show0(int* arr, int len)
{
	if (len == 0) return;
	Show0(arr + 1, len - 1);
	printf("%d ", *arr);
}
```

# 结构体

## 结构体定义

```c
struct 结构体名 {
    数据类型 成员1;
    数据类型 成员2;
    // ...
};
```

## 声明结构体变量

```c
//使用typedef定义别名
typedef struct {
    int id;
    char name[20];
    float score;
} Student;
```

## 初始化结构体变量

```c
// 顺序初始化
struct Student stu1 = {101, "张三", 85.5};
// 部分初始化
struct Student stu3 = {103}; // 其余成员自动初始化为0或NULL
```

## 结构体成员的访问

### 使用点运算符

```c
struct Student stu;
stu.id = 101;
strcpy(stu.name, "王五");
stu.score = 88.5;

printf("学号: %d\n", stu.id);
printf("姓名: %s\n", stu.name);
printf("成绩: %.1f\n", stu.score);
```

### 使用指针和箭头的运算符

```c
struct Student *pStu = &stu;
pStu->id = 102;
strcpy(pStu->name, "赵六");
pStu->score = 92.5;

printf("学号: %d\n", pStu->id);
printf("姓名: %s\n", pStu->name);
printf("成绩: %.1f\n", pStu->score);
```

## 结构体数组

### 定义和初始化

```c
struct Student class[3] = {
    {101, "张三", 85.5},
    {102, "李四", 90.0},
    {103, "王五", 78.5}
};
```

### 访问结构体数组元素

```c
for(int i = 0; i < 3; i++) {
    printf("学生%d: %s, 成绩: %.1f\n", 
           class[i].id, class[i].name, class[i].score);
}
```

## 结构体的高级用法

### 嵌套结构体

```c
struct Date {
    int year;
    int month;
    int day;
};

struct Employee {
    int id;
    char name[20];
    struct Date birthday;
    float salary;
};
```

### 结构体包含函数指针

```c
struct Calculator {
    int (*add)(int, int);
    int (*sub)(int, int);
};

int add(int a, int b) { return a + b; }
int sub(int a, int b) { return a - b; }

int main() {
    struct Calculator calc = {add, sub};
    printf("5 + 3 = %d\n", calc.add(5, 3));
    printf("5 - 3 = %d\n", calc.sub(5, 3));
    return 0;
}
```

## 结构体指针

### 传递方式

- `void func(S p)`：**值传递**（传值）。函数会获得结构体的完整副本，原结构体不会被修改。
- `void func(S* p)`：**指针传递**（传地址）。函数接收原结构体的内存地址，可直接操作原结构体。

### 使用示例

```c
typedef struct { int x; } S;

void modifyByValue(S p) { p.x = 10; }  // 不影响原结构体
void modifyByPtr(S* p) { p->x = 10; }  // 修改原结构体

int main() {
    S s = {0};
    modifyByValue(s);    // s.x仍为0
    modifyByPtr(&s);     // s.x变为10
}
```

# const

## const int* p

+ 指向常量的指针(指针可以改变，指向内容不可以改变)

```c
int x = 10;
const int* p = &x;  // p 可以指向不同的变量，但不能通过 p 修改 x
*p = 20;            // ❌ 错误：不能修改 *p
p = &y;             // ✅ 可以改变 p 的指向
```

##  int* const p

+ 常量指针(指针本身不能改变，指向内容可以改变)

```c
int x = 10;
int* const p = &x;  // p 不能指向别的变量，但可以修改 x
*p = 20;            // ✅ 可以修改 *p
p = &y;             // ❌ 错误：不能改变 p 的指向
```

## const int* const p

+ 指向常量的常量指针

```c
int x = 10;
const int* const p = &x;  // p 不能变，*p 也不能变
*p = 20;                  // ❌ 错误：不能修改 *p
p = &y;                   // ❌ 错误：不能改变 p 的指向
```

































