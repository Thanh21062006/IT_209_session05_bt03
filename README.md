# Báo cáo Bài 3: Xử lý xung đột phức tạp trong quá trình Rebase

## 1. Mục tiêu
- Hiểu được sự khác biệt giữa giải quyết xung đột khi Merge và khi Rebase.
- Áp dụng quy trình giải quyết xung đột từng bước (step-by-step conflict resolution) trong Rebase.
- Đưa lịch sử nhánh tính năng lên trên đầu nhánh chính một cách thẳng hàng.

## 2. Quá trình thực hiện
Trong bài tập này, nhánh `main` và nhánh `feature-api` đều có sự sửa đổi trên cùng file `config.json`.
- `main` sửa `"port": 8081` và thêm `"env": "production"`.
- `feature-api` sửa `"port": 9000` và `"debug": true` qua 2 commit liên tiếp.

Khi thực hiện lệnh `git rebase main` từ nhánh `feature-api`, tiến trình bị dừng lại 2 lần (do 2 commit của nhánh tính năng đều gây conflict với nội dung mới nhất của `main`).

### Chặng 1: Conflict ở commit "feat: change port"
Khi git apply commit đầu tiên của `feature-api`, nó phát hiện xung đột ở giá trị `port` và `env`.
- **Cách giải quyết:** Em đã mở file `config.json` ra sửa thủ công. Vì muốn giữ lại cổng của nhánh tính năng và các cấu hình mới của `main`, em sửa thành:
  ```json
  {
    "port": 9000,
    "debug": false,
    "env": "production"
  }
  ```
- Sau đó, em đánh dấu đã giải quyết và tiếp tục rebase:
  ```bash
  git add config.json
  git rebase --continue
  ```

### Chặng 2: Conflict ở commit "feat: enable debug"
Ngay sau chặng 1, git apply tiếp commit thứ hai và lại báo conflict ở phần `debug`.
- **Cách giải quyết:** Em tiếp tục sửa file `config.json` để giữ giá trị `"debug": true`:
  ```json
  {
    "port": 9000,
    "debug": true,
    "env": "production"
  }
  ```
- Đánh dấu hoàn tất chặng 2 và kết thúc rebase:
  ```bash
  git add config.json
  git rebase --continue
  ```

## 3. Kết quả
Sau quá trình giải quyết xung đột từng bước, lệnh `git status` báo nhánh làm việc đã clean. Rebase thành công giúp bảo toàn toàn bộ code mà không tạo ra merge commit rác.

### Ảnh chụp đồ thị Git Log
*(Sử dụng lệnh `git log --graph --oneline`)*

![ImgEx]()
