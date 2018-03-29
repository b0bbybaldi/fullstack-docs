# Introduction to Algorithms

## Formal Definition

* Set of **steps** that solve a problem/perform a calculation.
* Algorithms seek to find new ways to solve the same problems:
  - More efficiently.
  - Better handling of scale.

---


## Linear Search [O(n)]

A search algorithm that grows proportionate to the size of the array to be searched.

### Strategy / Pseudocode

1. Iterate over the array (`listOfInts`).
1. Check to see if the current number in the loop (`currentInt`) is equal to (`===`) the number we intended to find (`intToFind`).

### Implementation

```javascript
const listOfInts = [3,44,38,5,47,15,36,26,27,2,46,4,19,50,48];
const intToFind = listOfInts[Math.floor(Math.random() * listOfInts.length)];

function linearSearch(listOfInts, intToFind) {
    listOfInts.forEach(function(currentInt, index) {
        if (currentInt === intToFind) {
            console.log("Found value:", currentInt, "at index:", index);
        }
    });
}

// Test the function.
const listOfInts = [3,44,38,5,47,15,36,26,27,2,46,4,19,50,48];
const intToFind = listOfInts[Math.floor(Math.random() * listOfInts.length)];
linearSearch(listOfInts, intToFind);
```

---

## Binary Search [O(n) / O(n log n)]

**Requires a pre-sorted array.** In each round, we check if the number we're trying is less than or more than the number we are looking for. This strategy allows programmers to effectively "halve" the number of options required in order to locate the target number.

[![bst](images/bst.gif)](https://visualgo.net/en/bst)

### Pseudocode the Steps

1. Sort the array (`listOfInts`).
1. Find the number in middle of the array.
1. Determine if the middle number is less than / more than the number we're attempting to find.
1. Discard the half of the array that does not contain the number we're searching for.
1. Repeat steps 2 through 4 until the target number is located.

### Implementation

```javascript
function binarySearch(listOfInts, intToFind) {
    let minIndex = 0,
        maxIndex = listOfInts.length - 1,
        currentIndex,
        currentInt;

    while (minIndex <= maxIndex) {
        currentIndex = Math.floor((minIndex + maxIndex) / 2);
        currentInt = listOfInts[currentIndex];

        if (currentInt < intToFind) {
            // If the number is LESS THAN the number we're searching,
            // for, then LOOK ABOVE this number in the array.
            minIndex = currentIndex + 1;
        }
        else if (currentInt > intToFind) {
            // If the number is GREATER THAN the number we're searching,
            // for, then LOOK BELOW this number in the array.
            maxIndex = currentIndex - 1;
        }
        else {
            // We found it!
            return currentIndex;
        }

        return false;
    }
}

// Test the function.
const listOfInts = [3,44,38,5,47,15,36,26,27,2,46,4,19,50,48];
const intToFind = listOfInts[Math.floor(Math.random() * listOfInts.length)];
const foundIntAtThisIndex = binarySearch(listOfInts, intToFind);

if (foundIntAtThisIndex) {
    console.log("Found number (", intToFind, ") at index", foundIntAtThisIndex, ".");
}
else {
    console.log("Failed to Find Number (", intToFind, ") in the listOfInts.");
}
```

---

## Selection Sort

[![selection](images/selection-sort.gif)](https://visualgo.net/en/sorting)

### Pseudocode the Steps

1. Iterate over the array (`listOfInts`).
1. Set the first unsorted number as the minimum.
1. If the current number is LESS THAN the current minimum (as set in step 2), then set the current number as the new minimum.
1. Swap the minimum with the first unsorted position in the array.


### Implementation

```javascript
function selectionSort(listOfInts) {
    const len = listOfInts.length;
    let min;

    for (var i = 0; i < len; i++) {
        min = i;    // Set index of minimum to this position

        // Check the rest of the array to see if anything is smaller.
        for (var j = i + 1; j < len; j++) {
            if (listOfInts[j] < listOfInts[min]) {
                min = j;
            }
        }

        if (i !== min) {
            // If the current position isn't the minimum,
            // swap it and the minimum.
            let temp = listOfInts[i];
            listOfInts[i] = listOfInts[min];
            listOfInts[min] = temp;
        }
    }
    return listOfInts;
}
```

---

## Insertion Sort [O(n^2)]

### Pseudocode the Steps

1. **Iterate** over the array (`listOfInts`).
1. **Compare** the number to the **right**, and the number to the **left**, of the target number (`intToFind`).
1. **Swap** if the **left is greater than the right**.
1. **Repeat** steps **2 and 3** until:
    1. The **current number** is **less than** the **number to it's right**.
    1. OR the **current number** is **in** the **last position within the array**.

### Implementation

```javascript
function insertionSort(listOfInts) {
    let currentInt;         // The number currently being compared.
    let unsortedIndex;      // The index into the first for loop -- the unsorted section.
    let sortedIndex;        // The index into the second for loop -- the sorted section.

    for (unsortedIndex = 0; unsortedIndex < listOfInts.length; unsortedIndex++) {
        currentInt = listOfInts[unsortedIndex];

        // Whenever the value in the sorted section is GREATER THAN the value
        // in the unsorted section, shift all items in the sorted section over by 1.
        // This creates space in which to insert the number.
        for (sortedIndex = (unsortedIndex - 1); sortedIndex > -1 && listOfInts[sortedIndex] > currentInt; j--;) {
            listOfInts[sortedIndex + 1] = listOfInts[sortedIndex];
        }
    }

    return listOfInts;
}

// Test the function.
const listOfInts = [3,44,38,5,47,15,36,26,27,2,46,4,19,50,48];
const intToFind = listOfInts[Math.floor(Math.random() * listOfInts.length)];

const sortedInts = insertionSort(listOfInts);
console.log("Unsorted:", listOfInts);
console.log("Sorted:", sortedInts);
```

---

## Quicksort

[![quicksort](images/quicksort.gif)](https://visualgo.net/en/sorting)

### Pseudocode the Steps

1. Select a pivot number from the array.
1. Create an array of values less than the pivot.
1. Create an array of values greater than the pivot.
1. Recursively sort left and right arrays, and insert the pivot into it's final position.

### Implementation

```javascript
function quickSort(array) {
  if (array.length <= 1) {
    return array;
  }

  // Get random pivot element (and remove from array to add back in later)
  const pivot = array.splice(Math.floor(Math.random() * array.length), 1);

  const left = [];      // Create left array (elements LESS THAN OR EQUAL TO pivot)
  const right = [];     // and right array (elements GREATER THAN the pivot)

  // loop through array and create left/right
  array.forEach(function(el) {
    if (el <= pivot) {
      left.push(el);
    }
    else {
      right.push(el);
    }
  });

  // Get the result of recursively sorting the left array (using quicksort), then join that with the // pivot and the result of recursively sorting the right array (using quicksort).
  // Equivalent of `return quicksort(left) + pivot + quicksort (right);` in the pseudocode
  return quickSort(left).concat(pivot, quickSort(right));
}
```

---

## Additional Practice

* [Interactive Quiz - Sorting Algortithms](https://visualgo.net/training?diff=Medium&n=7&tl=0&module=sorting)
* [InterviewBit]()
