# Striver A2Z DSA Sheet — My Solutions & Notes

Solving [Striver's A2Z DSA Sheet](https://takeuforward.org/strivers-a2z-dsa-course/strivers-a2z-dsa-course-sheet-2/), one problem at a time — with notes on approach, complexity, patterns, and mistakes for each.

## 📊 Progress

**Total Problems Solved: 9**

| Topic     | Problems Solved | Folder                     |
|-----------|:----------------:|-----------------------------|
| Arrays    | 9                | [`Arrays/`](./Arrays)       |
| String    | 0                | [`String/`](./String)       |
| DP        | 0                | [`DP/`](./DP)                |
| Queue     | 0                | [`Queue/`](./Queue)          |
| **Total** | **9**            |                              |

> Update this table each time a new topic folder is added or a problem is solved. Counts are manual for now — see [Auto-updating this table](#auto-updating-this-table-optional) below if you want to automate it later.

---

## 📁 Arrays (9)

| # | Problem | Difficulty | Notes |
|---|---------|:----------:|-------|
| 1 | Largest Element in Array | Easy | [`largest-element-in-array.md`](./Arrays/largest-element-in-array.md) |
| 2 | Second Largest Element in Array | Easy | [`second-largest-element.md`](./Arrays/second-largest-element.md) |
| 3 | Check if Array is Sorted | Easy | [`check-sorted-array.md`](./Arrays/check-sorted-array.md) |
| 4 | Check if Array Is Sorted and Rotated | Easy | [`check-sorted-rotated-array.md`](./Arrays/check-sorted-rotated-array.md) |
| 5 | Move Zeroes | Easy | [`move-zeroes.md`](./Arrays/move-zeroes.md) |
| 6 | Linear Search | Easy | [`linear-search.md`](./Arrays/linear-search.md) |
| 7 | Missing Number in Array | Easy | [`missing-number.md`](./Arrays/missing-number.md) |
| 8 | Union of Two Sorted Arrays | Easy | [`union-of-two-sorted-arrays.md`](./Arrays/union-of-two-sorted-arrays.md) |
| 9 | Max Consecutive Ones | Easy | [`max-consecutive-ones.md`](./Arrays/max-consecutive-ones.md) |

## 📁 String (0)

_Not started yet._

## 📁 DP (0)

_Not started yet._

## 📁 Queue (0)

_Not started yet._

---

## 🗂 Repo Structure

```
repo/
├── README.md              # this file — progress tracker
├── Arrays/
│   ├── largest-element-in-array.md
│   ├── second-largest-element.md
│   └── ...
├── String/
├── DP/
├── Queue/
```

Each `problem_name.md` follows a consistent notes template: problem statement, multiple approaches with time/space complexity, key patterns, edge cases, related problems, and mistakes made.

---

## Auto-updating this table (optional)

Since every topic is a folder and every problem is one `.md` file, the count per topic is just the number of `.md` files in that folder. If you want this table to update itself instead of by hand, a small script (run manually or as a GitHub Action) can regenerate this section:

```bash
for dir in */; do
  topic="${dir%/}"
  [ "$topic" = ".git" ] && continue
  count=$(find "$dir" -maxdepth 1 -name "*.md" ! -name "README.md" | wc -l)
  echo "$topic: $count"
done
```

Happy to help wire this into a GitHub Action that regenerates the table on every push, if you want it fully automated later.
