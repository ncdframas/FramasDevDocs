# Hướng dẫn setup dev environment cho project FramasScanner

## 1. Yêu cầu trước khi bắt đầu

*   Máy đã cài git (https://git-scm.com/install/windows)
*   Có quyền chạy file `.bat`

***

## 2. Các bước setup project

### Bước 1: Tạo thư mục chứa project

*   Tạo một folder mới trên máy, ví dụ:

```text
D:\FramasScanner
```

***

### Bước 2: Copy file setup & unzip

*   Tải về [setup.zip](https://framas365-my.sharepoint.com/:u:/g/personal/congdat_nguyen_framas_com/IQCGoSJ0gzqVTqRoTinSjDc1AYUYxc6f44p5A_RI5QMFzKY?e=HyO3YS)
*   Copy file `setup.zip` vào thư mục vừa tạo
*   Unzip file setup.zip
*   Cấu trúc thư mục sau khi unzip:

```text
D:\FramasScanner
 └── FramasScanner.slnx
 └── README.txt
 └── setup.bat
```

***

### Bước 3: Chạy setup

*   Chạy file `setup.bat`
*   Chờ script chạy và tự động cài đặt / cấu hình môi trường cần thiết

> ⚠️ **Lưu ý**:
>
> *   Không tắt cửa sổ terminal trong quá trình chạy
> *   Nếu có lỗi, chụp log và gửi cho congdat.nguyen@framas.com

***

### Bước 4: Hoàn tất

*   Khi script chạy xong và không báo lỗi → **Setup thành công**
*   Mở FramasScanner.slnx và bắt đầu development
*   Kết quả sau khi setup thành công:

```text
D:\FramasScanner
 └── src 			// folder chưa source code
 └── FramasScanner.slnx	
 └── README.txt
 └── setup.bat
```

✅ **DONE**


# HOW TO: Thêm dev branch vào src folder

### Bước 1: Tạo dev branch nếu chưa có

*   Tạo branch trên github nếu chưa có [https://github.com/ncdframas/FramasScanner](https://github.com/ncdframas/FramasScanner)

> ⚠️ **Lưu ý**:
>
> *   Để đồng bộ và dễ quản lý thì branch name của dev phải bắt đầu bằng `dev_`+{Tên Dev}
> *   Ví dụ: `dev_Dat`

### Bước 2: Fetch remote branch về máy local

*   Tại thư mục chứa `FramasScanner.slnx` mở terminal và run

```bash
git fetch --all
```

### Bước 3: Tải branch về folder src

*   Vào thư mục `src` mở terminal và run

```cmd
git worktree add {BranchName} refs/remotes/origin/{BranchName}
# ví dụ: git worktree add dev_Dat refs/remotes/origin/dev_Dat
```

*   Kết quả sau khi add thành công

```text
D:\FramasScanner
 └── src 			// folder chưa source code
   └── dev_Dat 		// folder của branch dev_Dat
 └── FramasScanner.slnx	
 └── README.txt
 └── setup.bat
```

### Bước 4: Thêm project vào folder dev_Dat

*   Mở `FramasScanner.slnx`
*   Chuột phải vào solution -> Add -> New Project -> Change the location to folder dev_Dat

