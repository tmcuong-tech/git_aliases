# Cấu hình Git đầy đủ để Backup lên GitHub

Hướng dẫn này gồm: thông tin user, branch mặc định, alias `tree`, các alias thường dùng, màu sắc, pager và một số thiết lập tiện dụng — mỗi mục đều có ví dụ cụ thể.

---

## 1. Cấu hình thông tin Git

Thay `Tên Của Bạn` và `email@example.com` bằng thông tin thật của bạn:

```bash
git config --global user.name "Tên Của Bạn"
git config --global user.email "email@example.com"
```

**Ví dụ:**

```bash
git config --global user.name "Nguyen Van Cuong"
git config --global user.email "cuong@example.com"
```

Kiểm tra lại:

```bash
git config --global --list
```

---

## 2. Cấu hình branch mặc định là `main`

```bash
git config --global init.defaultBranch main
```

**Ví dụ sử dụng:** sau khi cấu hình, mỗi lần tạo repo mới:

```bash
mkdir my-project && cd my-project
git init
```

→ branch mặc định sẽ là `main` thay vì `master`.

---

## 3. Cấu hình màu sắc

```bash
git config --global color.ui auto
git config --global color.branch auto
git config --global color.status auto
git config --global color.diff auto
```

**Ví dụ:** sau khi bật, chạy `git status` hoặc `git diff` sẽ hiển thị màu (đỏ = xóa, xanh = thêm) thay vì chữ trắng đen.

---

## 4. Cấu hình `git tree` đẹp và đầy đủ

```bash
git config --global alias.tree "log --graph --all --decorate --date=relative --pretty=format:'%C(auto)%C(bold yellow)%h%C(reset) %C(auto)%d%C(reset) %C(green)(%cr)%C(reset) %s %C(dim white)- %an%C(reset)'"
```

**Ví dụ sử dụng:**

```bash
git tree
```

Kết quả đơn giản (1 branch):

```text
* 275c120 (HEAD -> main, origin/main) (18 minutes ago) update - cuong
* 0dbb652 (20 minutes ago) update - cuong
* 955d461 (27 minutes ago) python - cuong
```

Kết quả khi có nhiều branch:

```text
*   a1b2c3d (HEAD -> main) (2 hours ago) Merge branch 'feature' - cuong
|\
| * e4f5g6h (feature) (3 hours ago) add login - cuong
| * i7j8k9l (4 hours ago) update UI - cuong
* | m1n2o3p (5 hours ago) update README - cuong
|/
* q4r5s6t (1 day ago) initial commit - cuong
```

---

## 5. Tắt pager riêng cho `git tree`

Nếu máy thiếu `less`, output của `git tree` sẽ bị treo màn hình. Tắt pager cho riêng lệnh này:

```bash
git config --global pager.tree false
```

**Ví dụ:**

```bash
git tree
```

→ In thẳng ra terminal, không cần nhấn phím để cuộn.

Nếu muốn cài `less` sau này (trên Arch/Manjaro):

```bash
sudo pacman -Syu less
```

---

## 6. Các alias Git cơ bản

```bash
git config --global alias.st status
git config --global alias.s status
git config --global alias.co checkout
git config --global alias.sw switch
git config --global alias.br branch
git config --global alias.ci commit
```

**Ví dụ sử dụng:**

```bash
git st           # thay cho: git status
git s             # thay cho: git status (ngắn hơn)
git co main       # thay cho: git checkout main
git sw feature    # thay cho: git switch feature
git br            # thay cho: git branch
```

---

## 7. Alias xem log

### Log dạng cây, ngắn gọn

```bash
git config --global alias.lg "log --graph --all --decorate --oneline"
```

**Ví dụ:**

```bash
git lg
```

```text
* 275c120 (HEAD -> main) update - cuong
* 0dbb652 update - cuong
```

### 10 commit gần nhất

```bash
git config --global alias.recent "log --oneline -10"
```

**Ví dụ:**

```bash
git recent
```

→ Hiển thị 10 commit gần nhất, mỗi commit 1 dòng.

### Commit cuối cùng

```bash
git config --global alias.last "log -1 HEAD"
```

**Ví dụ:**

```bash
git last
```

→ Hiển thị đầy đủ thông tin commit mới nhất (tác giả, ngày, message).

### Xem tất cả branch (kể cả remote)

```bash
git config --global alias.branches "branch -a"
```

**Ví dụ:**

```bash
git branches
```

```text
* main
  feature/login
  remotes/origin/main
```

---

## 8. Alias quản lý thay đổi

### Unstage file

```bash
git config --global alias.unstage "restore --staged"
```

**Ví dụ:**

```bash
git unstage app.py
```

→ Đưa `app.py` ra khỏi staging area (bỏ khỏi vùng chuẩn bị commit) mà không xóa nội dung file.

### Xem diff

```bash
git config --global alias.d diff
git config --global alias.dc "diff --cached"
```

**Ví dụ:**

```bash
git d       # xem thay đổi chưa add
git dc      # xem thay đổi đã add (staged), chưa commit
```

---

## 9. Alias commit nhanh

```bash
git config --global alias.cm "commit -m"
```

**Ví dụ:**

```bash
git cm "fix bug đăng nhập"
```

→ Tương đương `git commit -m "fix bug đăng nhập"`.

---

## 10. Alias add

```bash
git config --global alias.a add
git config --global alias.aa "add ."
```

**Ví dụ:**

```bash
git a file.cpp     # add 1 file cụ thể
git aa             # add toàn bộ file đã thay đổi trong thư mục hiện tại
```

---

## 11. Alias backup nhanh (add + commit + push trong 1 lệnh)

```bash
git config --global alias.backup "!f() { git add . && git commit -m \"${1:-backup}\" && git push; }; f"
```

**Ví dụ:**

```bash
git backup "update project"
```

Tương đương với 3 lệnh:

```bash
git add .
git commit -m "update project"
git push
```

Nếu không truyền message:

```bash
git backup
```

→ Commit sẽ tự dùng message mặc định là `"backup"`.

---

## 12. Alias pull an toàn hơn (rebase thay vì merge)

```bash
git config --global alias.update "pull --rebase"
```

**Ví dụ:**

```bash
git update
```

→ Kéo code mới về và rebase lên nhánh hiện tại, tránh tạo commit merge thừa.

---

## 13. Một số cấu hình hữu ích khác

**Tự động setup remote tracking khi push branch mới:**

```bash
git config --global push.autoSetupRemote true
```

*Ví dụ:* khi tạo branch mới `feature/x` và chạy `git push`, Git tự thiết lập upstream mà không cần gõ `git push -u origin feature/x`.

**Hiển thị tên branch hiện tại trong `git status`:**

```bash
git config --global status.branch true
```

**Tự động gợi ý sửa lệnh gõ sai (typo):**

```bash
git config --global help.autocorrect prompt
```

*Ví dụ:* gõ nhầm `git stauts` → Git sẽ hỏi có phải bạn muốn chạy `git status` không.

**Dùng tên branch hiện tại khi push (không cần chỉ định remote/branch):**

```bash
git config --global push.default current
```

*Ví dụ:* đứng ở branch `feature/login`, chỉ cần gõ `git push` là đủ, không cần `git push origin feature/login`.

---

## 14. Kiểm tra toàn bộ cấu hình

```bash
git config --global --list
```

Hoặc xem trực tiếp file cấu hình:

```bash
cat ~/.gitconfig
```

---

## 15. Bộ lệnh hoàn chỉnh (copy chạy 1 lần)

```bash
# =========================
# USER INFORMATION
# =========================
git config --global user.name "Tên Của Bạn"
git config --global user.email "email@example.com"

# =========================
# DEFAULT SETTINGS
# =========================
git config --global init.defaultBranch main
git config --global color.ui auto

# =========================
# GIT TREE
# =========================
git config --global alias.tree "log --graph --all --decorate --date=relative --pretty=format:'%C(auto)%C(bold yellow)%h%C(reset) %C(auto)%d%C(reset) %C(green)(%cr)%C(reset) %s %C(dim white)- %an%C(reset)'"
git config --global pager.tree false

# =========================
# BASIC ALIASES
# =========================
git config --global alias.st status
git config --global alias.s status
git config --global alias.co checkout
git config --global alias.sw switch
git config --global alias.br branch
git config --global alias.branches "branch -a"
git config --global alias.ci commit
git config --global alias.cm "commit -m"

# =========================
# ADD
# =========================
git config --global alias.a add
git config --global alias.aa "add ."

# =========================
# DIFF
# =========================
git config --global alias.d diff
git config --global alias.dc "diff --cached"

# =========================
# LOG
# =========================
git config --global alias.lg "log --graph --all --decorate --oneline"
git config --global alias.recent "log --oneline -10"
git config --global alias.last "log -1 HEAD"

# =========================
# RESTORE / UNSTAGE
# =========================
git config --global alias.unstage "restore --staged"

# =========================
# UPDATE
# =========================
git config --global alias.update "pull --rebase"

# =========================
# BACKUP
# =========================
git config --global alias.backup "!f() { git add . && git commit -m \"${1:-backup}\" && git push; }; f"

# =========================
# PUSH SETTINGS
# =========================
git config --global push.autoSetupRemote true
git config --global push.default current

# =========================
# STATUS
# =========================
git config --global status.branch true

# =========================
# TYPO CORRECTION
# =========================
git config --global help.autocorrect prompt
```

---

## 16. Quy trình backup project sau khi cấu hình

### Lần đầu tiên (khởi tạo repo)

```bash
cd ~/duong-dan/toi/project

git init
git add .
git commit -m "Initial commit"
git remote add origin <repository-url>
git branch -M main
git push -u origin main
```

**Ví dụ cụ thể:**

```bash
cd ~/projects/my-app

git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/username/my-app.git
git branch -M main
git push -u origin main
```

### Các lần backup sau

Cách nhanh nhất — dùng alias `backup`:

```bash
git backup "mô tả thay đổi"
```

**Ví dụ:**

```bash
git backup "add login feature"
```

Hoặc làm theo quy trình chuẩn từng bước:

```bash
git aa
git cm "update project"
git push
```

**Ví dụ:**

```bash
git aa
git cm "fix bug hiển thị avatar"
git push
```

> ⚠️ **Lưu ý:** alias `backup` rất tiện, nhưng trước khi chạy `git add .`, nên kiểm tra trước bằng:
>
> ```bash
> git st
> ```
>
> để chắc chắn không vô tình đưa các file nhạy cảm như `.env`, API key, mật khẩu, hoặc file build lên GitHub.
