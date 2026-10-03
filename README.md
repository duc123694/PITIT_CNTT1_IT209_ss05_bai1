# Bài 1: Khôi phục commit đã mất bằng Git Reflog

## 1. Mục tiêu

* Hiểu cách Git lưu lại lịch sử hoạt động bằng Reflog.
* Sử dụng lệnh `git reflog` để tìm lại commit đã bị mất.
* Khôi phục commit sau khi thực hiện `git reset --hard HEAD~1`.
* Thực hành commit và đưa các file bài tập lên GitHub theo đúng thứ tự.

---

## 2. Khởi tạo Git Repository

Mở PowerShell và di chuyển đến thư mục bài tập:

```powershell
cd "C:\Users\Admin BVCN88 02\Favorites\Links"
```

Kiểm tra Git:

```powershell
git --version
```

Khởi tạo Git Repository:

```powershell
git init
```

---

## 3. Tạo commit đầu tiên

Repository ban đầu có các file:

```text
bai tap 1.txt
bai tap 2.txt
bai tap 3.txt
```

Thêm các file vào Git:

```powershell
git add "bai tap 1.txt"
git add "bai tap 2.txt"
git add "bai tap 3.txt"
```

Tạo commit:

```powershell
git commit -m "up"
```

Kiểm tra lịch sử:

```powershell
git log --oneline
```

Kết quả ban đầu:

```text
5881a17 up
```

---

## 4. Tạo commit chứa tính năng quan trọng

Tạo file `feature.txt`:

```powershell
"Day la tinh nang quan trong" | Out-File -Encoding utf8 feature.txt
```

Kiểm tra nội dung:

```powershell
Get-Content feature.txt
```

Kết quả:

```text
Day la tinh nang quan trong
```

Thêm file:

```powershell
git add feature.txt
```

Tạo commit:

```powershell
git commit -m "Them tinh nang quan trong"
```

Kiểm tra lịch sử:

```powershell
git log --oneline
```

Kết quả có dạng:

```text
<HASH> Them tinh nang quan trong
5881a17 up
```

---

## 5. Giả lập việc mất commit

Để mô phỏng việc vô tình xóa commit quan trọng, thực hiện:

```powershell
git reset --hard HEAD~1
```

Lệnh này đưa nhánh hiện tại quay về commit trước đó.

Kiểm tra lịch sử:

```powershell
git log --oneline
```

Lúc này commit:

```text
Them tinh nang quan trong
```

không còn xuất hiện trong `git log`.

File `feature.txt` cũng bị xóa khỏi working directory do sử dụng tùy chọn `--hard`.

---

## 6. Sử dụng Git Reflog để tìm commit bị mất

Thực hiện:

```powershell
git reflog
```

Kết quả có dạng:

```text
5881a17 HEAD@{0}: reset: moving to HEAD~1
<HASH> HEAD@{1}: commit: Them tinh nang quan trong
5881a17 HEAD@{2}: commit: up
```

Dòng cần tìm là:

```text
<HASH> HEAD@{1}: commit: Them tinh nang quan trong
```

Mã `<HASH>` chính là mã commit đã bị mất.

---

## 7. Khôi phục commit

Sử dụng mã hash tìm được trong Reflog:

```powershell
git reset --hard <HASH>
```

Ví dụ:

```powershell
git reset --hard abc1234
```

Trong đó `abc1234` là mã hash thực tế tìm được bằng `git reflog`.

---

## 8. Kiểm tra commit đã được khôi phục

Kiểm tra lịch sử:

```powershell
git log --oneline
```

Kết quả:

```text
<HASH> Them tinh nang quan trong
5881a17 up
```

Kiểm tra lại file:

```powershell
Get-Content feature.txt
```

Kết quả:

```text
Day la tinh nang quan trong
```

Kiểm tra trạng thái:

```powershell
git status
```

Nếu thành công:

```text
nothing to commit, working tree clean
```

---

## 9. Đưa 3 file bài tập lên Git theo đúng thứ tự

Ba file bài tập gồm:

```text
bai tap 1.txt
bai tap 2.txt
bai tap 3.txt
```

Để tạo lịch sử commit theo đúng thứ tự, thực hiện từng file.

### Commit bài tập 1

```powershell
git add "bai tap 1.txt"
git commit -m "Them bai tap 1"
```

### Commit bài tập 2

```powershell
git add "bai tap 2.txt"
git commit -m "Them bai tap 2"
```

### Commit bài tập 3

```powershell
git add "bai tap 3.txt"
git commit -m "Them bai tap 3"
```

Kiểm tra lịch sử:

```powershell
git log --oneline
```

Kết quả sẽ có dạng:

```text
<HASH3> Them bai tap 3
<HASH2> Them bai tap 2
<HASH1> Them bai tap 1
...
```

Git hiển thị commit mới nhất ở phía trên nên thứ tự thực hiện thực tế là:

```text
1. Them bai tap 1
2. Them bai tap 2
3. Them bai tap 3
```

---

## 10. Kết nối Repository với GitHub

Nếu Repository GitHub đã được tạo, thêm remote:

```powershell
git remote add origin git@github.com:USERNAME/REPOSITORY.git
```

Kiểm tra:

```powershell
git remote -v
```

Đổi tên nhánh thành `main`:

```powershell
git branch -M main
```

---

## 11. Push code lên GitHub

Thực hiện:

```powershell
git push -u origin main
```

Sau khi push thành công, các commit và file sẽ được đưa lên Repository GitHub.

---

## 12. Cấu trúc bài nộp

Thư mục bài tập:

```text
homework/
└── session_05/
    └── ex1/
        ├── README.md
        ├── bai tap 1.txt
        ├── bai tap 2.txt
        └── bai tap 3.txt
```

---

## 13. Các lệnh quan trọng đã sử dụng

### Kiểm tra lịch sử commit

```powershell
git log --oneline
```

### Xem Reflog

```powershell
git reflog
```

### Giả lập mất commit

```powershell
git reset --hard HEAD~1
```

### Khôi phục commit

```powershell
git reset --hard <HASH>
```

### Kiểm tra trạng thái

```powershell
git status
```

### Thêm file

```powershell
git add <file>
```

### Tạo commit

```powershell
git commit -m "message"
```

### Đẩy code lên GitHub

```powershell
git push -u origin main
```

---

## 14. Kết luận

Qua bài thực hành, em đã hiểu cách Git sử dụng Reflog để lưu lại lịch sử thay đổi của các tham chiếu như `HEAD`.

Mặc dù commit không còn xuất hiện trong `git log` sau khi sử dụng:

```powershell
git reset --hard HEAD~1
```

commit vẫn có thể được tìm thấy thông qua:

```powershell
git reflog
```

Sau khi lấy được mã hash của commit bị mất, có thể sử dụng:

```powershell
git reset --hard <HASH>
```

để khôi phục lại commit và các file đi kèm.

Ngoài ra, em đã thực hành tạo commit riêng cho từng file `bai tap 1.txt`, `bai tap 2.txt`, `bai tap 3.txt` và push các thay đổi lên GitHub.
