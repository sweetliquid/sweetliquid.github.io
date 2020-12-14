---
title: '手写七种排序算法'
date: '2020-11-30'
---

# 工具函数

## Swap

```ts
describe('Swap', () => {
  it('should be swapped', () => {
    const array = [1, 2]
    swap(array, 0, 1)
    expect(array).toStrictEqual([2, 1])
  })
})

function swap(array: unknown[], x: number, y: number) {
  ;[array[x], array[y]] = [array[y], array[x]]
}
```

## Range

```ts
describe('Range', () => {
  it('should create a incremental array', () => {
    const array = range(0, 3)
    expect(array).toStrictEqual([0, 1, 2])
  })
  it('should create a decremental array', () => {
    const array = range(3, 0)
    expect(array).toStrictEqual([3, 2, 1])
  })
})

function range(start: number, end: number) {
  let res = [...Array(Math.abs(start - end)).keys()].map(
    x => x + Math.min(start, end) + (start > end ? 1 : 0)
  )
  if (start > end) {
    res = res.reverse()
  }
  return res
}
```

# 测试

```ts
describe('sort', () => {
  const sorters = new Map([
    ['bubble', bubbleSort],
    ['quick', quickSort],
    ['select', selectSort],
    ['heap', heapSort],
    ['insert', insertSort],
    ['shell', shellSort],
    ['merge', mergeSort],
  ])

  for (const [name, sorter] of sorters) {
    it(name, () => {
      const array = range(10, 0)
      sorter(array)
      expect(array).toStrictEqual(range(1, 11))
    })
  }
})
```

# 排序算法

1. 冒泡排序 Bubble Sort

   ```ts
   function bubbleSort(array: unknown[]) {
     for (const j of range(0, array.length - 1)) {
       for (const i of range(array.length - 1, j)) {
         const ii = i - 1
         if (array[i] < array[ii]) {
           swap(array, i, ii)
         }
       }
     }
   }
   ```

2. 快速排序 Quick Sort

   ```ts
   function quickSort(array: unknown[]) {
     const swapA = (l: number, r: number) => swap(array, l, r)
     function sortInner(left: number, right: number) {
       function partition() {
         // 双指针交换法
         const p = array[left]
         let [l, r] = [left, right]

         while (l < r) {
           while (l < r && p <= array[r]) r--
           while (l < r && array[l] <= p) l++
           if (l < r) swapA(l, r)
         }

         swapA(left, l)
         return l
       }

       if (right <= left) return

       const mid = partition()
       sortInner(0, mid - 1)
       sortInner(mid + 1, right)
     }

     sortInner(0, array.length - 1)
   }
   ```

3. 选择排序 Select Sort

   ```ts
   function selectSort(array: unknown[]) {
     for (const i of range(0, array.length)) {
       for (const ii of range(i + 1, array.length)) {
         if (array[ii] < array[i]) {
           swap(array, i, ii)
         }
       }
     }
   }
   ```

4. 堆排序 Heap Sort

   ```ts
   function heapSort(array: unknown[]) {
     if (array.length < 2) return

     const shiftDown = (current: number, length: number) => {
       const left = current * 2 + 1
       const right = left + 1

       if (length <= left) return

       const child = right < length && array[left] < array[right] ? right : left

       if (array[child] <= array[current]) return

       swap(array, current, child)

       shiftDown(child, length)
     }

     const lastNonLeaf = array.length / 2 - 1

     range(lastNonLeaf, -1).forEach(i => shiftDown(i, array.length - 1))

     range(array.length - 1, 0).forEach(end => {
       swap(array, 0, end)
       shiftDown(0, end)
     })
   }
   ```

5. 插入排序 Insert Sort

   ```ts
   function insert(array: unknown[], current: number, step: number = 1) {
     const left = current - step
     if (left < 0) return
     if (array[left] <= array[current]) return
     swap(array, current, left)
     insert(array, left)
   }

   function insertSort(array: unknown[]) {
     if (array.length < 2) return

     range(1, array.length).forEach(i => insert(array, i))
   }
   ```

6. 希尔排序 Shell Sort

   ```ts
   function shellSort(array: unknown[]) {
     function getSteps(x: number, steps: number[] = []): number[] {
       x /= 2
       x <<= 0
       if (x < 1) return steps
       steps.push(x)
       return getSteps(x, steps)
     }

     getSteps(array.length).forEach(step => {
       range(step, array.length).forEach(i => insert(array, i, step))
     })
   }
   ```

7. 归并排序 Merge Sort

   ```ts
   function mergeSort(array: unknown[]) {
     // TODO
   }
   ```

# 完整代码

<iframe src="https://codesandbox.io/embed/sort-z9cll?autoresize=1&fontsize=14&hidenavigation=1&module=%2Fsrc%2Fsort.ts&previewwindow=tests&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="Sort"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>
