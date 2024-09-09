# 面試 C/C++ 觀念整理

作者: Skyler Chen  
發佈時間: Mar 2, 2020

## 面試重點題型整理

---

### 1. Explain `#error`?
**解釋**:  
`#error` 是預處理指令，當預處理階段遇到 `#error`，會生成錯誤提示訊息並停止編譯。

---

### 2. `const` 的使用與意義
- **const int a**: 一個常數型整數，無法修改數值。
- **int const a**: 同上，表示一個不可改變的整數。
- **const int *a**: 一個指向常數整數的指標，整數不可修改，但指標可以。
- **int *const a**: 一個指向整數的常數型指標，指標不可修改，但整數可以。
- **int const *a const**: 指向常數整數的常數型指標，兩者皆不可改變。

---

### 3. `struct` 和 `union` 的區別
- **struct**: 結構體，包含多個不同資料型態的變數，變數佔用不同的內存空間。
- **union**: 聯合體，可以包含不同資料型態的變數，但所有變數共享相同的內存空間。

---

### 4. 解釋 `volatile` 並說明是否能與 `const` 同時使用？
**volatile** 表示變量可能被外部修改，編譯器每次使用該變量時都會重新讀取它的值。常見使用情境如硬體暫存器、ISR 中的非自動變數或多執行緒共享的變數。  
- **volatile** 和 **const** 可以同時存在，例如只讀的硬體狀態寄存器，該寄存器可能被硬體修改（volatile），但程式不應該修改它（const）。
- 可以在指標中使用 **volatile**，例如 ISR 中的緩衝區指標。

---

### 5. `Inline Function` vs `Macro`
- **Inline Function**: 在編譯時直接取代 function 呼叫，增加執行效率，但不會改變運算優先級。
- **Macro**: 預處理階段進行文字替換，可能引起運算優先級問題。

```cpp
inline int square(int x) { return x * x; }
#define SQUARE(x) (x * x)
```

---

### 6. 解釋 `static`
- **變數**: 局部變數加上 `static` 後，變數離開作用域後仍然存在。
- **全域變數**: `static` 變數只限定於該檔案中，不會在其他檔案中衝突。
- **類別成員變數（C++）**: 屬於類別而非實例，所有實例共用。
- **類別成員函式（C++）**: 屬於類別而非實例，無需實例化即可使用。

---

### 7. Stack 和 Heap 的區別
- **Stack**: 儲存函式參數與局部變數，速度快，不需要手動釋放，但空間有限。
- **Heap**: 由程式設計師分配與管理，沒有大小限制，但速度較慢且可能會發生記憶體碎片問題。

---

### 8. 解釋 `thread` 和 `process`
- **Process**: 程式的執行實例，執行時間較長，與其他程序不共享記憶體空間。
- **Thread**: 在一個進程內的執行路徑，執行速度較快，並與其他執行緒共享記憶體空間。

---

### 9. 各種變數宣告方法
- 整數: `int a;`
- 指向整數的指標: `int *a;`
- 指向整數指標的指標: `int **a;`
- 包含 10 個整數的陣列: `int a[10];`
- 包含 10 個指向整數的指標陣列: `int *a[10];`
- 指向包含 10 個整數的陣列的指標: `int (*a)[10];`
- 指向一個接受整數並返回整數的函數的指標: `int (*a)(int);`
- 包含 10 個指向接受整數並返回整數的函數指標的陣列: `int (*a[10])(int);`

---

### 10. Swap 兩個整數而不使用暫存變數
```cpp
int a = 1, b = 2;
a = a ^ b;
b = a ^ b;
a = a ^ b;
```

---

### 11. 判斷數字是否為 2 的次方
```cpp
isPow2 = x && !( (x-1) & x );
```

---

### 12. 計算整數中 1 的位數
```cpp
#include <iostream>
using namespace std;
int main() {
    int x, count = 0;
    cin >> x;
    while (x > 0) {
        count++;
        x = x & (x - 1);
    }
    cout << count << endl;
    return 0;
}
```

---

### 13. 反轉字串
```cpp
void reverseStr(string& str) {
    int n = str.length();
    for (int i = 0; i < n / 2; i++) {
        swap(str[i], str[n - i - 1]);
    }
}
```

---

### 14. 找到單向鏈結串列的中間節點
```cpp
ListElement* getMiddle(ListElement* first) {
    if (!first) return NULL;
    int n = 1;
    ListElement *middle = first, *current = first;
    while (current) {
        current = current->next;
        ++n;
        if (n % 2 == 1) middle = middle->next;
    }
    return middle;
}
```

---

### 15. 反轉鏈結串列
```cpp
void reverse_list(ListNode* head) {
    ListNode *curr = head, *prev = NULL, *next = NULL;
    while (curr) {
        next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }
    head = prev;
}
```

---

### 16. 變數宣告與定義的區別
- **宣告**: 提供符號的基本屬性（類型和名稱）。
- **定義**: 提供符號的所有詳細資訊（包括內存分配）。

---

### 17. 求解「n 一定是 1 到 5 其中一個數」的問題
```cpp
typedef void (*fp)(void);
fp fpa[5];
fpa[1] = func1;
fpa[2] = func2;
fpa[3] = func3;
fpa[4] = func4;
fpa[5] = func5;

void main(int n) {
    (*fpa[n])();
}
```

---

**參考來源**: 面試觀念整理