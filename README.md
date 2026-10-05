# Báo cáo Bài 5: Khôi phục trạng thái và Đảo ngược commit (Reset vs Revert)

## 1. Trường hợp 1: Khôi phục cục bộ bằng `git reset` (Chưa Push lên Remote)
- **Bối cảnh:** Khi viết code ở nhánh cá nhân (local) và commit một tính năng bị lỗi nhưng chưa push lên nhánh chung.
- **Thực thi:** Tôi sử dụng lệnh `git reset HEAD~1` (mặc định là cờ `--mixed`).
- **Kết quả:** Con trỏ HEAD và nhánh hiện tại lùi lại 1 commit. Tuy nhiên, nội dung file bị lỗi vẫn được giữ nguyên trong Working Directory ở trạng thái Modified (chưa add). Nhờ đó, tôi có thể tiếp tục sửa đổi file và tạo lại một commit mới hoàn chỉnh, giữ cho lịch sử commit gọn gàng.

## 2. Trường hợp 2: Đảo ngược an toàn bằng `git revert` (Đã Push lên Remote / Làm việc nhóm)
- **Bối cảnh:** Khi lỗi đã được push lên Remote (đặc biệt là các nhánh chung như `main` hay `develop`), việc dùng `reset` sẽ phá hỏng lịch sử chung của cả nhóm, gây ra tình trạng bất đồng bộ (out of sync).
- **Thực thi:** Tôi sử dụng lệnh `git revert HEAD --no-edit` để đảo ngược thay đổi của commit lỗi gần nhất.
- **Kết quả:** Git không xóa commit lỗi mà tạo ra một **commit mới** hoàn toàn với nội dung thay đổi đảo ngược lại những gì commit lỗi đã làm. Thông điệp mặc định bắt đầu bằng chữ `Revert "..."`. Cách này an toàn tuyệt đối khi làm việc nhóm vì lịch sử không bị bóp méo, mọi người đều nhìn thấy vết tích sửa lỗi.

## 3. Hình chụp lịch sử commit kiểm chứng
Dưới đây là kết quả của lệnh `git log --oneline -n 5` chứng minh đã thực hiện hai trường hợp:

```text
1a40658 Revert "Bad commit for revert"
03af362 Bad commit for revert
e7baf45 Corrected commit after reset
db35296 Initial commit
```
*(Có thể thấy commit được tạo bằng Revert xuất hiện trên đỉnh nhánh, trong khi commit bị Reset trước đó đã biến mất hoàn toàn khỏi lịch sử và được thay thế bằng commit "Corrected commit after reset")*
