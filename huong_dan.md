# Hướng dẫn đẩy dự án lên GitHub

## 1. Khởi tạo repository trên GitHub

- Đăng nhập vào GitHub và chọn **New repository**.
- Đặt tên repository (ví dụ: `test-git`), giữ trống các tùy chọn khởi tạo (`README`, `.gitignore`, License).
- Sao chép URL của repository vừa tạo, ví dụ:
  - HTTPS: `https://github.com/<username>/test-git.git`
  - hoặc SSH: `git@github.com:<username>/test-git.git`

## 2. Chuẩn bị dự án trên máy

- Mở Terminal và chuyển đến thư mục dự án:
  ```bash
  cd "/Users/user/Desktop/test git"
  ```
- Kiểm tra xem đã có Git repo chưa:
  ```bash
  git status
  ```
- Nếu chưa có, khởi tạo Git:
  ```bash
  git init
  ```

## 3. Commit lần đầu

- Thêm toàn bộ file vào staging:
  ```bash
  git add .
  ```
- Tạo commit:
  ```bash
  git commit -m "Initial commit"
  ```

## 4. Kết nối tới repository trên GitHub

- Thêm remote (thay URL bằng của bạn):
  ```bash
  git remote add origin https://github.com/<username>/test-git.git
  ```
- Nếu trước đó đã thêm `origin` và muốn thay đổi, dùng:
  ```bash
  git remote set-url origin https://github.com/<username>/test-git.git
  ```

## 5. Đẩy mã nguồn lên GitHub

- Đẩy nhánh chính lần đầu:
  ```bash
  git push -u origin main
  ```
  (Nếu nhánh mặc định là `master`, dùng `git push -u origin master`.)

## 6. Kiểm tra trên GitHub

- Reload trang repository và kiểm tra các file đã xuất hiện.
- Những lần cập nhật tiếp theo:
  ```bash
  git add .
  git commit -m "Mô tả thay đổi"
  git push
  ```

## 7. Tùy chọn sử dụng SSH

- Tạo SSH key (nếu chưa có):
  ```bash
  ssh-keygen -t ed25519 -C "your_email@example.com"
  ```
- Thêm public key (`~/.ssh/id_ed25519.pub`) vào **Settings > SSH and GPG keys** trên GitHub.
- Sử dụng URL SSH khi thêm remote:
  ```bash
  git remote add origin git@github.com:<username>/test-git.git
  ```

## 8. Quy trình làm việc nhóm với nhánh (branch) để tránh xung đột

- **Luôn cập nhật nhánh chính (main/master) trước khi bắt đầu:**

  ```bash
  git checkout main
  git pull origin main
  ```

  (hoặc `master` nếu dự án dùng tên nhánh đó).

- **Tạo nhánh mới cho từng tính năng/sửa lỗi:**

  ```bash
  git checkout -b feature/ten-tinh-nang
  ```

  Đặt tên nhánh rõ nghĩa (`feature/`, `bugfix/`, `hotfix/`...).

- **Làm việc và commit trên nhánh đó:**

  - Chia nhỏ commit, viết thông điệp rõ ràng.
  - Kiểm tra `git status` thường xuyên để tránh quên file.

- **Đẩy nhánh lên GitHub để chia sẻ với team:**

  ```bash
  git push -u origin feature/ten-tinh-nang
  ```

- **Tạo Pull Request (PR):**

  - Vào GitHub → mở repository → chọn "Compare & pull request".
  - Mô tả thay đổi, đề cập người review (reviewer).
  - Gắn nhãn (labels), liên kết issue nếu có.

- **Review chéo:**

  - Người được gán review: đọc code, comment, yêu cầu sửa nếu cần.
  - Tác giả cập nhật nhánh (commit/push thêm).
  - Có thể dùng `git fetch` + `git checkout feature/...` để test branch của người khác.

- **Giữ nhánh cập nhật với nhánh chính để hạn chế xung đột:**

  ```bash
  git checkout feature/ten-tinh-nang
  git fetch origin
  git merge origin/main
  ```

  (hoặc `rebase` nếu team thống nhất: `git rebase origin/main`).
  Giải quyết conflict ngay khi xuất hiện, commit lại.

- **Merge PR vào nhánh chính sau khi được duyệt:**

  - Dùng nút "Merge" hoặc "Rebase and merge" (tùy quy ước).
  - Sau merge, nhánh trên GitHub có thể xóa (Delete branch).

- **Đồng bộ lại máy cá nhân sau mỗi merge:**

  ```bash
  git checkout main
  git pull origin main
  ```

  Xóa nhánh local nếu đã xong:

  ```bash
  git branch -d feature/ten-tinh-nang
  ```

  (dùng `-D` nếu nhánh chưa merge nhưng muốn bỏ).

- **Một số mẹo tránh xung đột:**
  - Không commit file cấu hình cá nhân (IDE, `.env`, file build...).
  - Trao đổi trước khi chỉnh sửa file lớn/thường được dùng chung.
  - Commit sớm, push thường xuyên để mọi người thấy thay đổi.
  - Với conflict khó, thảo luận trên PR hoặc trực tiếp để giải quyết nhanh.
