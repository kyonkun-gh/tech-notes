# 排序(Sorting)

- **氣泡排序法 (Bubble Sort)**： 每次從頭開始比較 2個相鄰元素，將較大元素往後移動，若此次沒有交換，可以提前結束
  - 時間雜度：O(n<sup>2</sup>)
  - 空間雜度：O(1)
```Java
public int[] bubbleSort(int[] arr) {
    int n = arr.length;
    boolean hasSwap = true;     // 判斷是否有交換
    while ( hasSwap ) {
        hasSwap = false;        // 修改為未交換
        for ( int i = 1; i < n; i++ ) {
            if ( arr[i-1] > arr[i] ) {  // 將較大元素往後移動
                swap(arr, i-1, i);
                hasSwap = true;
            }
        }
    }

    return arr;
}
```

- **插入排序法 (Insertion Sort)**： 每次由當前元素開始，往前尋找適合插入此元素的位置
  - 時間雜度：O(n<sup>2</sup>)
  - 空間雜度：O(1)
```Java
public int[] insertionSort(int[] arr) {
    int n = arr.length;
    for ( int i = 0; i < n; i++ ) {
        int cur = arr[i];
        int j = i-1;
        while ( j >= 0 && cur < arr[j] ) {  // 比較第 j個元素是否比目前元素大
            arr[j+1] = arr[j];              // 如果第 j個元素比較大，將第 j個元素往後移動
            j--;
        }

        arr[j+1] = cur;     // 跳出 while loop時，表示找到合適位置
    }

    return arr;
}
```

- **選擇排序法 (Selection Sort)**： 由當前元素開始，往後選擇最小的元素位置，與目前位置交換
  - 時間雜度：O(n<sup>2</sup>)
  - 空間雜度：O(1)
```Java
public int[] selectionSort(int[] arr) {
    int n = arr.length;
    for ( int i = 0; i < n; i++ ) {
        int minIdx = i;     // 初始設定最小元素為 i
        for ( int j = i+1; j < n; j++ ) {
            if ( arr[j] < arr[minIdx] ) {       // 第 j個元素是否小於 minIdx元素
                minIdx = j;
            }
        }

        swap(arr, i, minIdx);
    }

    return arr;
}
```
