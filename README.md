# algo-colloquium

## Оглавление

1. [Оценка сложности алгоритма по времени]()
2. [Оценка сложности алгоритма по памяти]()
3. [Сортировка вставками](#сортировка-вставками)
4. [Сортировка слиянием](#сортировка-слиянием)
5. [Быстрая сортировка](#быстрая-сортировка)
6. [Сортировка подсчетом]()
7. [Цифровая сортировка]()
8. [Стек]()
9. [Очередь]()
10. [Односвязный список]()
11. [Двусвязный список]()
12. [Циклический список]()
13. [Стек на списках]()
14. [Очередь на списках]()
15. [Бинпоиск]()

## Оценка сложности алгоритма по времени

![оценка по времени](https://raw.githubusercontent.com/hashlag/algo-colloquium/main/time.jpg)

## Оценка сложности алгоритма по памяти

...

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

## Цифровая сортировка a.k.a. Radix sort

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

