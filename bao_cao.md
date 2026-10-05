# Báo cáo Bài 2: Quản lý nhánh và Giải quyết xung đột (Merge Conflict)

**Họ tên:** ...........................
**Ngày thực hiện:** ...........................

## 1. Mục tiêu

- Tạo và chuyển đổi giữa các nhánh cục bộ.
- Cố ý tạo xung đột gộp nhánh trên `README.md` và xử lý thủ công.
- Hoàn thành commit gộp và hiểu cơ chế 3-Way Merge.

## 2. Các bước thực hiện

### Bước 1: Khởi tạo repo và commit đầu tiên trên `main`

```bash
git init
git branch -M main
echo "# Dự án Git Bài 2" > README.md
echo "Dòng mô tả: Phiên bản gốc" >> README.md
git add README.md
git commit -m "Initial commit: tao README.md"
```

`README.md` có 2 dòng. Dòng 2 là dòng sẽ gây xung đột.

### Bước 2: Tạo nhánh `feature-update` và sửa dòng 2

```bash
git checkout -b feature-update
```

Sửa dòng 2 thành `Dòng mô tả: Cập nhật từ nhánh feature-update`, rồi commit:

```bash
git add README.md
git commit -m "feature-update: sua dong mo ta"
```

### Bước 3: Quay về `main` và sửa cùng dòng 2

```bash
git checkout main
```

Sửa dòng 2 thành `Dòng mô tả: Cập nhật từ nhánh main`, rồi commit:

```bash
git add README.md
git commit -m "main: sua dong mo ta"
```

Lúc này hai nhánh cùng sửa một dòng theo hai cách khác nhau.

### Bước 4: Gộp nhánh để tạo xung đột

```bash
git merge feature-update --no-ff
```

Git báo:

```
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

Lệnh `git status` hiển thị `You have unmerged paths` và `both modified: README.md`.

> Lưu ý: trong lúc đang xung đột, lệnh `git checkout feature-update` bị từ chối với lỗi
> `error: you need to resolve your current index first`. Merge được thực hiện ngay trên `main`
> nên không cần chuyển nhánh, chỉ cần giải quyết xung đột rồi commit.

### Bước 5: Giải quyết xung đột thủ công

Mở `README.md`, nội dung có các ký hiệu xung đột:

```
# Dự án Git Bài 2
<<<<<<< HEAD
Dòng mô tả: Cập nhật từ nhánh main
=======
Dòng mô tả: Cập nhật từ nhánh feature-update
>>>>>>> feature-update
```

| Ký hiệu | Ý nghĩa |
|---|---|
| `<<<<<<< HEAD` | Bắt đầu phần của nhánh hiện tại (`main`) |
| `=======` | Ngăn cách hai phiên bản |
| `>>>>>>> feature-update` | Kết thúc phần của nhánh được gộp vào |

Cách xử lý thủ công:

1. Xóa cả 3 dòng ký hiệu `<<<<<<< HEAD`, `=======`, `>>>>>>> feature-update`.
2. Viết lại nội dung cuối cùng, kết hợp thông tin từ cả hai nhánh.
3. Lưu file.

Nội dung `README.md` sau khi giải quyết:

```
# Dự án Git Bài 2
Dòng mô tả: Cập nhật từ nhánh main và nhánh feature-update (đã giải quyết xung đột)
```

Kiểm tra không còn ký hiệu xung đột (trên Windows cmd, lệnh không in ra dòng nào là đạt):

```cmd
findstr /n "<<<<<<< ======= >>>>>>>" README.md
```

Lần kiểm tra đầu, lệnh còn in ra các dòng 2, 4, 6 (chưa xóa ký hiệu). Sau khi sửa và lưu file, lệnh không còn in gì.

### Bước 6: Hoàn tất commit gộp

```bash
git add README.md
git commit -m "Merge branch 'feature-update' into main - giai quyet xung dot README.md"
```

`git add` đánh dấu file đã được giải quyết xung đột, `git commit` tạo merge commit.

### Bước 7: Kiểm tra kết quả

```bash
git log --graph --oneline
```

Kết quả (mã hash có thể khác):

```
*   a1b2c3d (HEAD -> main) Merge branch 'feature-update' into main - giai quyet xung dot README.md
|\
| * 4d5e6f7 (feature-update) feature-update: sua dong mo ta
* | 8a9b0c1 main: sua dong mo ta
|/
* 2e3f4a5 Initial commit: tao README.md
```

Ảnh chụp màn hình:
<img width="1281" height="180" alt="image" src="https://github.com/user-attachments/assets/6384fc14-7439-4d2c-aab8-ae0e701d65b8" />


