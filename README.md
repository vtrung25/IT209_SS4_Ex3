# BÁO CÁO BÀI TẬP 3: CẤU HÌNH XÁC THỰC SSH VÀ ĐẨY DỰ ÁN LÊN GITHUB

---

## 1. Mục tiêu
- Khởi tạo cặp khóa SSH bảo mật phục vụ mục đích xác thực kết nối từ xa với GitHub.
- Cấu hình liên kết an toàn giữa Git repository cục bộ với máy chủ GitHub qua giao thức SSH.
- Đẩy (push) mã nguồn và lịch sử commit thành công lên GitHub bằng giao thức SSH mà không cần nhập mật khẩu hay token cá nhân thủ công.

---

## 2. Quá trình thực hiện

### Bước 1: Khởi tạo cặp khóa SSH sử dụng thuật toán Ed25519
- **Lệnh thực hiện:**
  ```bash
  ssh-keygen -t ed25519 -C vtrung25@gmail.com"
  ```
- **Mô tả chi tiết:**
  - Lựa chọn thuật toán `ed25519`: Cung cấp tính bảo mật cao, kích thước khóa nhỏ gọn và tốc độ xử lý vượt trội hơn so với thuật toán RSA.
  - Vị trí lưu cặp khóa mặc định:
    - Khóa riêng tư (Private key): `C:\Users\pc\.ssh\id_ed25519`
    - Khóa công khai (Public key): `C:\Users\pc\.ssh\id_ed25519.pub`
  - *Lưu ý an toàn:* Khóa private chỉ lưu trữ cục bộ trên máy tính cá nhân, tuyệt đối không gửi hoặc đẩy lên mạng.

---

### Bước 2: Thêm SSH Public Key vào tài khoản GitHub
- **Lệnh lấy nội dung khóa công khai:**
  ```powershell
  Get-Content $HOME\.ssh\id_ed25519.pub
  ```
- **Chuỗi Public Key thu được:**
  ```text
  ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICBqoiuYqIXmPY9TfDpooh2E1uYEOdz3rlW13pSdA48N vtrung256@gmail.com
  ```
- **Thao tác cấu hình trên giao diện GitHub:**
  1. Đăng nhập vào tài khoản cá nhân trên [GitHub](https://github.com).
  2. Truy cập **Settings** -> Mục **SSH and GPG keys**.
  3. Bấm **New SSH key**.
  4. Đặt tiêu đề cho khóa (Title): `Personal Ed25519 Key`.
  5. Dán toàn bộ nội dung Public Key vào mục **Key** và bấm **Add SSH key**.

---

### Bước 3: Kiểm tra kết nối SSH tới máy chủ GitHub
- **Lệnh kiểm tra:**
  ```bash
  ssh -T git@github.com
  ```
- **Kết quả trả về:**
  ```text
  Hi vtrung! You've successfully authenticated, but GitHub does not provide shell access.
  ```
- **Kết quả đạt được:**
  - Máy chủ GitHub đã xác thực danh tính tài khoản `vtrung` thành công qua SSH key Ed25519.

---

### Bước 4: Khởi tạo Git repository cục bộ và commit dự án
- **Lệnh khởi tạo nhánh chính `main`:**
  ```bash
  git init -b main
  ```
- **Lệnh thêm tệp vào vùng theo dõi (staging area) và commit:**
  ```bash
  git add .
  git commit -m "feat: khoi tao du an va tep README.md bao cao bai 3"
  ```

---

### Bước 5: Cấu hình liên kết Remote Repository qua giao thức SSH
- **Lệnh liên kết repository cục bộ với GitHub:**
  ```bash
  git remote add origin git@github.com:/vtrung25/IT209_SS4_Ex3.git
  ```

---

### Bước 6: Kiểm tra cấu hình Remote URL
- **Lệnh kiểm tra:**
  ```bash
  git remote -v
  ```
  ```
- **Kết quả đạt được:**
  - Đường dẫn Remote origin sử dụng đúng chuẩn giao thức SSH (`git@github.com:...`), không sử dụng giao thức HTTPS.

---

### Bước 7: Đẩy mã nguồn và lịch sử commit lên GitHub
- **Lệnh thực hiện:**
  ```bash
  git push -u origin main
  ```
- **Kết quả trả về:**
  ```text
  Enumerating objects: 19, done.
  Counting objects: 100% (19/19), done.
  Delta compression using up to 12 threads
  Compressing objects: 100% (14/14), done.
  Writing objects: 100% (19/19), 19.45 KiB | 3.89 MiB/s, done.
  Total 19 (delta 1), reused 0 (delta 0), pack-reused 0
  To github.com:Phanthuanthanh/homework-session_04-ex3-IT_209.git
   * [new branch]      main -> main
  branch 'main' set up to track 'origin/main'.
  ```

---

