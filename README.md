# algo-colloquium

## Оглавление

1. [Оценка сложности алгоритма по времени](#оценка-сложности-алгоритма-по-времени)
2. [Оценка сложности алгоритма по памяти](#оценка-сложности-алгоритма-по-памяти)
3. [Сортировка вставками](#сортировка-вставками)
4. [Сортировка слиянием](#сортировка-слиянием)
5. [Быстрая сортировка](#быстрая-сортировка)
6. [Сортировка подсчетом](#сортировка-подсчетом)
7. [Цифровая сортировка](#цифровая-сортировка)
8. [Стек](#стек)
9. [Очередь](#очередь)
10. [Односвязный список](#односвязный-список)
11. [Двусвязный список](#двусвязный-список)
12. [Циклический список](#циклический-список)
13. [Стек на списках](#стек-на-списках)
14. [Очередь на списках](#очередь-на-списках)
15. [Бинпоиск](#бинпоиск)
16. [Куча и приоритетная очередь](#куча-и-приоритетная-очередь)
17. [Пирамидальная сортировка](#пирамидальная-сортировка)

## Оценка сложности алгоритма по времени

![оценка по времени](https://raw.githubusercontent.com/hashlag/algo-colloquium/main/time.jpg)

![](https://raw.githubusercontent.com/hashlag/algo-colloquium/main/types.png)

## Оценка сложности алгоритма по памяти

![](https://raw.githubusercontent.com/hashlag/algo-colloquium/main/mem.png)

##

![](https://raw.githubusercontent.com/hashlag/algo-colloquium/main/funcs.png)

## Сортировка вставками

Задача заключается в следующем: есть часть массива, которая уже отсортирована, и требуется вставить остальные элементы массива в отсортированную часть, сохранив при этом упорядоченность. Для этого на каждом шаге алгоритма мы выбираем один из элементов входных данных и вставляем его на нужную позицию в уже отсортированной части массива, до тех пор пока весь набор входных данных не будет отсортирован. Метод выбора очередного элемента из исходного массива произволен, однако обычно (и с целью получения устойчивого алгоритма сортировки), элементы вставляются по порядку их появления во входном массиве.

Сортировка вставками: принцип
- Первый элемент считаем отсортированной частью
- Начиная со второго:
- - просматриваем элементы слева направо от i к i+1
- - выполняем вставку просматриваемого элемента в
отсортированную часть
- - после вставки увеличиваем размер отсортированной части

### Пример кода

```c++
// Function to sort an array using
// insertion sort
void insertionSort(int arr[], int n)
{
    int i, key, j;
    for (i = 1; i < n; i++) {
        key = arr[i];
        j = i - 1;
 
        // Move elements of arr[0..i-1],
        // that are greater than key, 
        // to one position ahead of their
        // current position
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j = j - 1;
        }
        arr[j + 1] = key;
    }
}
```

### Оценка по времени

| Best | Avg | Worst |
| --- | --- | --- |
| O(n) | O(n^2) | O(n^2) |

### Оценка по памяти (доп.)

| Best | Avg | Worst |
| --- | --- | --- |
| O(1) | O(1) | O(1) |

## Сортировка слиянием

Алгоритм использует принцип «разделяй и властвуй»: задача разбивается на подзадачи меньшего размера, которые решаются по отдельности, после чего их решения комбинируются для получения решения исходной задачи. Конкретно процедуру сортировки слиянием можно описать следующим образом:

1. Если в рассматриваемом массиве один элемент, то он уже отсортирован — алгоритм завершает работу.

2. Иначе массив разбивается на две части, которые сортируются рекурсивно.

3. После сортировки двух частей массива к ним применяется процедура слияния, которая по двум отсортированным частям получает исходный отсортированный массив.

### Пример кода

```c++
// Merges two subarrays of array[].
// First subarray is arr[begin..mid]
// Second subarray is arr[mid+1..end]
void merge(int array[], int const left, int const mid,
           int const right)
{
    int const subArrayOne = mid - left + 1;
    int const subArrayTwo = right - mid;
 
    // Create temp arrays
    auto *leftArray = new int[subArrayOne],
         *rightArray = new int[subArrayTwo];
 
    // Copy data to temp arrays leftArray[] and rightArray[]
    for (auto i = 0; i < subArrayOne; i++)
        leftArray[i] = array[left + i];
    for (auto j = 0; j < subArrayTwo; j++)
        rightArray[j] = array[mid + 1 + j];
 
    auto indexOfSubArrayOne = 0, indexOfSubArrayTwo = 0;
    int indexOfMergedArray = left;
 
    // Merge the temp arrays back into array[left..right]
    while (indexOfSubArrayOne < subArrayOne
           && indexOfSubArrayTwo < subArrayTwo) {
        if (leftArray[indexOfSubArrayOne]
            <= rightArray[indexOfSubArrayTwo]) {
            array[indexOfMergedArray]
                = leftArray[indexOfSubArrayOne];
            indexOfSubArrayOne++;
        }
        else {
            array[indexOfMergedArray]
                = rightArray[indexOfSubArrayTwo];
            indexOfSubArrayTwo++;
        }
        indexOfMergedArray++;
    }
 
    // Copy the remaining elements of
    // left[], if there are any
    while (indexOfSubArrayOne < subArrayOne) {
        array[indexOfMergedArray]
            = leftArray[indexOfSubArrayOne];
        indexOfSubArrayOne++;
        indexOfMergedArray++;
    }
 
    // Copy the remaining elements of
    // right[], if there are any
    while (indexOfSubArrayTwo < subArrayTwo) {
        array[indexOfMergedArray]
            = rightArray[indexOfSubArrayTwo];
        indexOfSubArrayTwo++;
        indexOfMergedArray++;
    }
    delete[] leftArray;
    delete[] rightArray;
}
 
// begin is for left index and end is right index
// of the sub-array of arr to be sorted
void mergeSort(int array[], int const begin, int const end)
{
    if (begin >= end)
        return;
 
    int mid = begin + (end - begin) / 2;
    mergeSort(array, begin, mid);
    mergeSort(array, mid + 1, end);
    merge(array, begin, mid, end);
}
```

### Оценка по времени

| Best | Avg | Worst |
| --- | --- | --- |
| O(n*log n) | O(n*log n) | O(n*log n) |

### Оценка по памяти (доп.)

| Best | Avg | Worst |
| --- | --- | --- |
| O(n) | O(n) | O(n) |

## Быстрая сортировка

Быстрый метод сортировки функционирует по принципу "разделяй и властвуй".

1. Массив `a[l…r]`
 типа T
 разбивается на два (возможно пустых) подмассива `a[l…q]`
 и `a[q+1…r]`, таких, что каждый элемент `a[l…q]` меньше или равен `a[q]`, который в свою очередь, не превышает любой элемент подмассива `a[q+1…r]`
 Индекс вычисляется в ходе процедуры разбиения.

2. Подмассивы `a[l…q]`
 и `a[q+1…r]`
 сортируются с помощью рекурсивного вызова процедуры быстрой сортировки.

3. Поскольку подмассивы сортируются на месте, для их объединения не требуются никакие действия: весь массив `a[l…r]`
 оказывается отсортированным.

## Пример кода

```c++
int partition(int arr[], int start, int end)
{
 
    int pivot = arr[start];
 
    int count = 0;
    for (int i = start + 1; i <= end; i++) {
        if (arr[i] <= pivot)
            count++;
    }
 
    // Giving pivot element its correct position
    int pivotIndex = start + count;
    swap(arr[pivotIndex], arr[start]);
 
    // Sorting left and right parts of the pivot element
    int i = start, j = end;
 
    while (i < pivotIndex && j > pivotIndex) {
 
        while (arr[i] <= pivot) {
            i++;
        }
 
        while (arr[j] > pivot) {
            j--;
        }
 
        if (i < pivotIndex && j > pivotIndex) {
            swap(arr[i++], arr[j--]);
        }
    }
 
    return pivotIndex;
}
 
void quickSort(int arr[], int start, int end)
{
 
    // base case
    if (start >= end)
        return;
 
    // partitioning the array
    int p = partition(arr, start, end);
 
    // Sorting the left part
    quickSort(arr, start, p - 1);
 
    // Sorting the right part
    quickSort(arr, p + 1, end);
}
```

### Разбиение Хоара

### Разбиение Ломуто

### Оценка по времени

| Best | Avg | Worst |
| --- | --- | --- |
| O(n*log n) | O(n*log n) | O(n^2) |

### Оценка по памяти (доп.)

| Best | Avg | Worst |
| --- | --- | --- |
| O(log n) | O(log n) | O(n) |


## Сортировка подсчетом

Алгоритм сортировки целых чисел в диапазоне от 0
до некоторой константы k
или сложных объектов, работающий за линейное время.

```c++
#include <bits/stdc++.h>
using namespace std;

vector<int> countSort(vector<int>& inputArray)
{

	int N = inputArray.size();

	// Finding the maximum element of array inputArray[].
	int M = 0;

	for (int i = 0; i < N; i++)
		M = max(M, inputArray[i]);

	// Initializing countArray[] with 0
	vector<int> countArray(M + 1, 0);

	// Mapping each element of inputArray[] as an index
	// of countArray[] array

	for (int i = 0; i < N; i++)
		countArray[inputArray[i]]++;

	// Calculating prefix sum at every index
	// of array countArray[]
	for (int i = 1; i <= M; i++)
		countArray[i] += countArray[i - 1];

	// Creating outputArray[] from countArray[] array
	vector<int> outputArray(N);

	for (int i = N - 1; i >= 0; i--)

	{
		outputArray[countArray[inputArray[i]] - 1]
			= inputArray[i];

		countArray[inputArray[i]]--;
	}

	return outputArray;
}

// Driver code
int main()

{

	// Input array
	vector<int> inputArray = { 4, 3, 12, 1, 5, 5, 3, 9 };

	// Output array
	vector<int> outputArray = countSort(inputArray);

	for (int i = 0; i < inputArray.size(); i++)
		cout << outputArray[i] << " ";

	return 0;
}
```

### Асимптотическая оценка

![](https://raw.githubusercontent.com/hashlag/algo-colloquium/main/count_compl.png)

## Цифровая сортировка

### a.k.a. Radix sort

Имеем множество последовательностей одинаковой длины, состоящих из элементов, на которых задано отношение линейного порядка. Требуется отсортировать эти последовательности в лексикографическом порядке.

По аналогии с разрядами чисел будем называть элементы, из которых состоят сортируемые объекты, разрядами. Сам алгоритм состоит в последовательной сортировке объектов какой-либо устойчивой сортировкой по каждому разряду, в порядке от младшего разряда к старшему, после чего последовательности будут расположены в требуемом порядке.

### LSD

Анализ значений разрядов от младших к старшим

```c++
// C++ implementation of Radix Sort

#include <iostream>
using namespace std;

// A utility function to get maximum
// value in arr[]
int getMax(int arr[], int n)
{
	int mx = arr[0];
	for (int i = 1; i < n; i++)
		if (arr[i] > mx)
			mx = arr[i];
	return mx;
}

// A function to do counting sort of arr[]
// according to the digit
// represented by exp.
void countSort(int arr[], int n, int exp)
{

	// Output array
	int output[n];
	int i, count[10] = { 0 };

	// Store count of occurrences
	// in count[]
	for (i = 0; i < n; i++)
		count[(arr[i] / exp) % 10]++;

	// Change count[i] so that count[i]
	// now contains actual position
	// of this digit in output[]
	for (i = 1; i < 10; i++)
		count[i] += count[i - 1];

	// Build the output array
	for (i = n - 1; i >= 0; i--) {
		output[count[(arr[i] / exp) % 10] - 1] = arr[i];
		count[(arr[i] / exp) % 10]--;
	}

	// Copy the output array to arr[],
	// so that arr[] now contains sorted
	// numbers according to current digit
	for (i = 0; i < n; i++)
		arr[i] = output[i];
}

// The main function to that sorts arr[]
// of size n using Radix Sort
void radixsort(int arr[], int n)
{

	// Find the maximum number to
	// know number of digits
	int m = getMax(arr, n);

	// Do counting sort for every digit.
	// Note that instead of passing digit
	// number, exp is passed. exp is 10^i
	// where i is current digit number
	for (int exp = 1; m / exp > 0; exp *= 10)
		countSort(arr, n, exp);
}

// A utility function to print an array
void print(int arr[], int n)
{
	for (int i = 0; i < n; i++)
		cout << arr[i] << " ";
}

// Driver Code
int main()
{
	int arr[] = { 543, 986, 217, 765, 329 };
	int n = sizeof(arr) / sizeof(arr[0]);

	// Function Call
	radixsort(arr, n);
	print(arr, n);
	return 0;
}
```

### MSD

Анализ значений разрядов от старших к младшим

```c++
// C++ implementation of MSD Radix Sort
// of MSD Radix Sort using counting sort()
#include <iostream>
#include <math.h>
#include <unordered_map>

using namespace std;

// A utility function to print an array
void print(int* arr, int n)
{
	for (int i = 0; i < n; i++) {
		cout << arr[i] << " ";
	}
	cout << endl;
}

// A utility function to get the digit
// at index d in a integer
int digit_at(int x, int d)
{
	return (int)(x / pow(10, d - 1)) % 10;
}

// The main function to sort array using
// MSD Radix Sort recursively
void MSD_sort(int* arr, int lo, int hi, int d)
{

	// recursion break condition
	if (hi <= lo) {
		return;
	}

	int count[10 + 2] = { 0 };

	// temp is created to easily swap strings in arr[]
	unordered_map<int, int> temp;

	// Store occurrences of most significant character
	// from each integer in count[]
	for (int i = lo; i <= hi; i++) {
		int c = digit_at(arr[i], d);
		count++;
	}

	// Change count[] so that count[] now contains actual
	// position of this digits in temp[]
	for (int r = 0; r < 10 + 1; r++)
		count[r + 1] += count[r];

	// Build the temp
	for (int i = lo; i <= hi; i++) {
		int c = digit_at(arr[i], d);
		temp[count++] = arr[i];
	}

	// Copy all integers of temp to arr[], so that arr[] now
	// contains partially sorted integers
	for (int i = lo; i <= hi; i++)
		arr[i] = temp[i - lo];

	// Recursively MSD_sort() on each partially sorted
	// integers set to sort them by their next digit
	for (int r = 0; r < 10; r++)
		MSD_sort(arr, lo + count[r], lo + count[r + 1] - 1,
				d - 1);
}

// function find the largest integer
int getMax(int arr[], int n)
{
	int mx = arr[0];
	for (int i = 1; i < n; i++)
		if (arr[i] > mx)
			mx = arr[i];
	return mx;
}

// Main function to call MSD_sort
void radixsort(int* arr, int n)
{
	// Find the maximum number to know number of digits
	int m = getMax(arr, n);

	// get the length of the largest integer
	int d = floor(log10(abs(m))) + 1;

	// function call
	MSD_sort(arr, 0, n - 1, d);
}

// Driver Code
int main()
{
	// Input array
	int arr[] = { 9330, 9950, 718, 8977, 6790,
				95, 9807, 741, 8586, 5710 };

	// Size of the array
	int n = sizeof(arr) / sizeof(arr[0]);

	printf("Unsorted array : ");

	// Print the unsorted array
	print(arr, n);

	// Function Call
	radixsort(arr, n);

	printf("Sorted array : ");

	// Print the sorted array
	print(arr, n);

	return 0;
}
```

### Асимптотическая оценка

![](https://raw.githubusercontent.com/hashlag/algo-colloquium/main/radix_compl.png)

## Стек

Стек (от англ. stack — стопка) — структура данных, представляющая из себя упорядоченный набор элементов, в которой добавление новых элементов и удаление существующих производится с одного конца, называемого вершиной стека. Притом первым из стека удаляется элемент, который был помещен туда последним, то есть в стеке реализуется стратегия «последним вошел — первым вышел» (last-in, first-out — **LIFO**). Примером стека в реальной жизни может являться стопка тарелок: когда мы хотим вытащить тарелку, мы должны снять все тарелки выше.

Операции:

- `empty`
 — проверка стека на наличие в нем элементов,
- `push`
 (запись в стек) — операция вставки нового элемента,
- `pop`
 (снятие со стека) — операция удаления нового элемента.

Возможные реализации:

- на массиве
- на динамическом массиве
- на списке

### Оценка

![](https://raw.githubusercontent.com/hashlag/algo-colloquium/main/stack.png)

## Очередь

Очередь (англ. queue)  — это структура данных, добавление и удаление элементов в которой происходит путём операций `push`
и `pop`
соответственно. Притом первым из очереди удаляется элемент, который был помещен туда первым, то есть в очереди реализуется принцип «первым вошел — первым вышел» (англ. first-in, first-out — **FIFO**). У очереди имеется голова (англ. head) и хвост (англ. tail). Когда элемент ставится в очередь, он занимает место в её хвосте. Из очереди всегда выводится элемент, который находится в ее голове. Очередь поддерживает следующие операции:

- `empty`
 — проверка очереди на наличие в ней элементов,
- `push`
 (запись в очередь) — операция вставки нового элемента,
- `pop`
 (снятие с очереди) — операция удаления нового элемента,
- `size`
 — операция получения количества элементов в очереди.

### Оценка

![](https://raw.githubusercontent.com/hashlag/algo-colloquium/main/q.png)

## Односвязный список

Связный список (англ. List) — структура данных, состоящая из элементов, содержащих помимо собственных данных ссылки на следующий и/или предыдущий элемент списка. С помощью списков можно реализовать такие структуры данных как стек и очередь.

## Односвязный список

Простейшая реализация списка. В узлах хранятся данные и указатель на следующий элемент в списке.

![](https://raw.githubusercontent.com/hashlag/algo-colloquium/main/SimpleSpisok.png)

## Двусвязный список

Также хранится указатель на предыдущий элемент списка, благодаря чему становится проще удалять и переставлять элементы.

![](https://raw.githubusercontent.com/hashlag/algo-colloquium/main/dllist.png)

## XOR-связный список

В некоторых случаях использование двусвязного списка в явном виде является нецелесообразным. В целях экономии памяти можно хранить только результат выполнения операции Xor над адресами предыдущего и следующего элементов списка. Таким образом, зная адрес предыдущего элемента, мы можем вычислить адрес следующего элемента.

## Циклический список

Первый элемент является следующим для последнего элемента списка.

![](https://raw.githubusercontent.com/hashlag/algo-colloquium/main/CircleSpisok.png)

### Алгоритм Флойда для поиска цикла в связном списке

## Стек на списках

Стек можно реализовать и на списке. Для этого необходимо создать список и операции работы стека на созданном списке. Ниже представлен пример реализации стека на односвязном списке. Стек будем "держать" за голову. Добавляться новые элементы посредством операции `push`
 будут перед головой, сами при этом становясь новой головой, а элементом для изъятия из стека с помощью `pop`
 будет текущая голова. После вызова функции `push`
 текущая голова уже станет старой и будет являться следующим элементом за добавленным, то есть ссылка на следующий элемент нового элемента будет указывать на старую голову. После вызова функции `pop`
 будет получена и возвращена информация, хранящаяся в текущей голове. Сама голова будет изъята из стека, а новой головой станет элемент, который следовал за изъятой головой.

Заведем конструктор вида `ListItem(next : ListItem, data : T)`

Ключевые поля:

- head.data
 — значение в верхушке стека,
- head.next
 — значение следующее за верхушкой стека.

(осторожно, псевдокод с нирка!)

```js
function push(element : T):
  head = ListItem(head, element)
```

```js
T pop():
  data = head.data
  head = head.next
  return data
```

В реализации на списке, кроме самих данных, хранятся указатели на следующие элементы, которых столько же, сколько и элементов, то есть, так же n. Стоит заметить, что стек требует `O(n)`
дополнительной памяти на указатели в списке.

## Очередь на списках

Для данной реализации очереди необходимо создать список list
 и операции работы на созданном списке.

Реализация очереди на односвязном списке:

### List

- ListItem(data : T, next : ListItem) — конструктор,
- x.value
 — поле, в котором хранится значение элемента,
- x.next
 — указатель на следующий элемент очереди.

### push

```js
function push(x : T):
  element = tail
  tail = ListItem(x, NULL)
  if size == 0
    head = tail
  else 
    element.next = tail
  size++
```

### pop

```js
T pop(): 
  size--
  element = head
  head = head.next
  return element
```

### empty

```js
boolean empty():
  return head == tail
```

Плюсы:

- каждая операция выполняется за время O(1)

Минусы:

- память фрагментируется гораздо сильнее и последовательная итерация по такой очереди может быть ощутимо медленнее, нежели итерация по очереди реализованной на массиве.

## Бинпоиск

Алгоритм поиска объекта по заданному признаку в множестве объектов, упорядоченных по тому же самому признаку, работающий за логарифмическое время.

Двоичный поиск заключается в том, что на каждом шаге множество объектов делится на две части и в работе остаётся та часть множества, где находится искомый объект. Или же, в зависимости от постановки задачи, мы можем остановить процесс, когда мы получим первый или же последний индекс вхождения элемента. Последнее условие — это левосторонний/правосторонний двоичный поиск.

Правосторонний бинарный поиск (англ. rightside binary search) — бинарный поиск, с помощью которого мы ищем max (i ∈ [−1,n−1]) {i ∣ a[i] ⩽ x}
, где a
 — массив, а x
 — искомый ключ

Левосторонний бинарный поиск (англ. leftside binary search) — бинарный поиск, с помощью которого мы ищем min i ∈ [0,n] {i ∣ a[i] ⩾ x}
, где a
 — массив, а x
 — искомый ключ

Идея поиска заключается в том, чтобы брать элемент посередине, между границами, и сравнивать его с искомым. Если искомое больше(в случае правостороннего — не меньше), чем элемент сравнения, то сужаем область поиска так, чтобы новая левая граница была равна индексу середины предыдущей области. В противном случае присваиваем это значение правой границе. Проделываем эту процедуру до тех пор, пока правая граница больше левой более чем на 1.

### Левосторонний (код)

```c
int binSearch(int[] a, int key):   // Запускаем бинарный поиск
    int l = -1                      // l, r — левая и правая границы
    int r = len(a)    
    while l < r - 1                // Запускаем цикл
        m = (l + r) / 2            // m — середина области поиска
        if a[m] < key
            l = m
        else 
            r = m                  // Сужение границ
    return r
```

**В случае правостороннего поиска изменится знак сравнения при сужении границ на a[m] ⩽ k, а возвращаемой переменной станет l.**

## Куча и приоритетная очередь

Двоичная куча или пирамида (англ. Binary heap) — такое двоичное подвешенное дерево, для которого выполнены следующие три условия:
- Значение в любой вершине не больше (если куча для минимума), чем значения её потомков.
- На i-ом слое 2i
 вершин, кроме последнего. Слои нумеруются с нуля.
- Последний слой заполнен слева направо (как показано на рисунке)

Удобнее всего двоичную кучу хранить в виде массива a[0..n−1]
, у которого нулевой элемент, a[0]
 — элемент в корне, а потомками элемента a[i]
 являются a[2i+1]
 и a[2i+2]
. Высота кучи определяется как высота двоичного дерева. То есть она равна количеству рёбер в самом длинном простом пути, соединяющем корень кучи с одним из её листьев. Высота кучи есть O(logn)
, где n
 — количество узлов дерева.

Чаще всего используют кучи для минимума (когда предок не больше детей) и для максимума (когда предок не меньше детей).

Двоичные кучи используют, например, для того, чтобы извлекать **минимум из набора** чисел за O(logn)
. Они являются **частным случаем приоритетных очередей**.

### `siftDown`

Если значение измененного элемента увеличивается, то свойства кучи восстанавливаются функцией siftDown.

Работа процедуры: если `i-й` элемент меньше, чем его сыновья, всё поддерево уже является кучей, и делать ничего не надо. В противном случае меняем местами `i-й` элемент с наименьшим из его сыновей, после чего выполняем `siftDown`
 для этого сына. Процедура выполняется за время `O(logn)`.

### `siftUp`

Если значение измененного элемента уменьшается, то свойства кучи восстанавливаются функцией `siftUp`.

Работа процедуры: если элемент больше своего отца, условие 1 соблюдено для всего дерева, и больше ничего делать не нужно. Иначе, мы меняем местами его с отцом. После чего выполняем `siftUp`
 для этого отца. Иными словами, слишком маленький элемент всплывает наверх. Процедура выполняется за время `O(logn)`. 

![](https://raw.githubusercontent.com/hashlag/algo-colloquium/main/heapops.png)

[Прекрасная лекция Павла Маврина](https://www.youtube.com/watch?v=WghPolCTknc)

## Пирамидальная сортировка

[**Как сделать Heap-Sort за O(1) по памяти, объяснение**](https://youtu.be/WghPolCTknc?t=3715)

Необходимо отсортировать массив `A`
, размером `n`
. Построим на базе этого массива за `O(n)`
 кучу для максимума. Так как максимальный элемент находится в корне, то если поменять его местами с `A[n−1]`
, он встанет на своё место. Далее вызовем процедуру `siftDown(0)`
, предварительно уменьшив `heapSize`
 на 1
. Она за `O(logn)`
 просеет `A[0]`
 на нужное место и сформирует новую кучу (так как мы уменьшили её размер, то куча располагается с `A[0]`
 по `A[n−2]`
, а элемент `A[n−1]`
 находится на своём месте). Повторим эту процедуру для новой кучи, только корень будет менять местами не с `A[n−1]`
, а с `A[n−2]`
. Делая аналогичные действия, пока heapSize
 не станет равен 1
, мы будем ставить наибольшее из оставшихся чисел в конец не отсортированной части. Очевидно, что таким образом, мы получим отсортированный массив.

- `A`
 — массив, который необходимо отсортировать
- `n`
 — количество элементов в нём
- `buildHeap(A)`
 — процедура, которая строит из передаваемого массива кучу для максимума в этом же массиве
- `siftDown(A,i,len)`
 — процедура, которая просеивает вниз элемент `A[i]`
 в куче из `len`
 элементов, находящихся в начале массива `A`

```go
func heapSort(A : list <T>):
   buildHeap(A)
   heapSize = A.size
   for i = 0 to n - 1
     swap(A[0], A[n - 1 - i])
     heapSize--
     siftDown(A, 0, heapSize)
```

### Асимптотика

![](https://raw.githubusercontent.com/hashlag/algo-colloquium/main/hipposort.png)

**По памяти O(1)**