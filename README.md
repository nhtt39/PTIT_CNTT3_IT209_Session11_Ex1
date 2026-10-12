# Session 11 - Exercise 1: Java Spring Boot CI với GitHub Actions & Gradle

## 1. Mục tiêu

- Khởi tạo tệp kịch bản GitHub Actions Workflow cho dự án Java Spring Boot sử dụng Gradle.
- Cấu hình môi trường Java JDK 17 (Temurin).
- Tự động chạy bài kiểm thử JUnit Test bằng Gradle Wrapper (`./gradlew test`) khi có sự kiện `push` lên nhánh `main`.

---

## 2. Cấu trúc dự án

```text
ex1_ss11/
├── .github/
│   └── workflows/
│       └── ci.yml               # Workflow kịch bản GitHub Actions CI
├── gradle/
│   └── wrapper/
│       ├── gradle-wrapper.jar   # Gradle Wrapper JAR
│       └── gradle-wrapper.properties
├── src/
│   ├── main/
│   │   ├── java/com/example/demo/
│   │   │   └── DemoApplication.java
│   │   └── resources/
│   │       └── application.properties
│   └── test/
│       └── java/com/example/demo/
│           └── DemoApplicationTests.java # JUnit test case
├── .gitattributes               # Đảm bảo gradlew dùng LF line-ending
├── .gitignore
├── build.gradle                 # Cấu hình Gradle & dependencies
├── gradlew                      # Script chạy Gradle Wrapper trên Linux/macOS
├── gradlew.bat                  # Script chạy Gradle Wrapper trên Windows
├── settings.gradle
└── README.md
```

---

## 3. Cấu hình GitHub Actions CI (`.github/workflows/ci.yml`)

Workflow được định nghĩa để lắng nghe sự kiện push vào nhánh `main` và chạy các bước kiểm thử trên runner `ubuntu-latest`:

```yaml
name: Java Spring Boot CI (Gradle)

on:
  push:
    branches: [ "main" ]

jobs:
  test:
    name: Run Unit Tests
    runs-on: ubuntu-latest

    steps:
      - name: Checkout mã nguồn
        uses: actions/checkout@v5

      - name: Thiết lập môi trường Java JDK 17
        uses: actions/setup-java@v5
        with:
          java-version: '17'
          distribution: 'temurin'

      - name: Cấp quyền cho Gradle Wrapper
        run: chmod +x gradlew

      - name: Thực thi JUnit Test với Gradle
        run: ./gradlew test
```

### Chi tiết các bước thực hiện trong Job:
1. **Checkout mã nguồn (`actions/checkout@v5`)**: Tải toàn bộ source code của repository về runner ảo `ubuntu-latest`.
2. **Thiết lập môi trường Java JDK 17 (`actions/setup-java@v5`)**: Cài đặt OpenJDK 17 từ bản phân phối Eclipse Temurin (Adoptium), đồng thời cấu hình biến môi trường `JAVA_HOME` và `PATH`.
3. **Cấp quyền cho Gradle Wrapper (`chmod +x gradlew`)**: Đảm bảo tệp thực thi wrapper có quyền thực thi (`execute`) trên hệ điều hành Linux.
4. **Thực thi JUnit Test (`./gradlew test`)**: Kích hoạt tác vụ `test` của Gradle để biên dịch mã nguồn và chạy toàn bộ các bài kiểm thử unit test JUnit 5.

---

## 4. Hướng dẫn khởi tạo Git và Đẩy lên GitHub

Mở PowerShell tại thư mục `ex1_ss11` và thực hiện các lệnh sau:

```bash
# 1. Khởi tạo Git repository cục bộ
git init -b main

# 2. Thêm toàn bộ các tệp vào staging
git add .

# 3. Tạo commit đầu tiên
git commit -m "feat: setup Spring Boot Gradle project and GitHub Actions CI workflow"

# 4. Liên kết với remote repository trên GitHub (thay URL bằng repo của bạn)
# Ví dụ:
git remote add origin https://github.com/nhtt39/PTIT_CNTT3_IT209_Session11_Ex1.git

# 5. Đẩy mã nguồn lên nhánh main
git push -u origin main
```

---

## 5. Kết quả kiểm tra trên GitHub Actions

1. Truy cập vào GitHub Repository của bạn.
2. Nhấp vào tab **Actions**.
3. Bạn sẽ thấy workflow **Java Spring Boot CI (Gradle)** tự động được kích hoạt bởi sự kiện push vào nhánh `main`.
4. Nhấp vào lần chạy (run) gần nhất để kiểm tra chi tiết Job **Run Unit Tests**.
5. Xác nhận tất cả các bước (Checkout, Setup Java 17 Temurin, Chmod gradlew, Run Gradle test) đều hoàn thành với biểu tượng tích xanh (**Success** / `✓`).

---

## 6. Sản phẩm nộp bài

- **Tệp kịch bản workflow:** [`.github/workflows/ci.yml`](.github/workflows/ci.yml)
- **Ảnh chụp màn hình:** Đặt ảnh chụp tab GitHub Actions với Job `Run Unit Tests` tích xanh vào thư mục `images/` hoặc tải lên theo yêu cầu của bài tập.
