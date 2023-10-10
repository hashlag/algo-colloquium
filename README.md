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

...

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

## Оценка по времени

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

### Оценка по времени

| Best | Avg | Worst |
| --- | --- | --- |
| O(n*log n) | O(n*log n) | O(n^2) |

### Оценка по памяти (доп.)

| Best | Avg | Worst |
| --- | --- | --- |
| O(log n) | O(log n) | O(n) |