## Lệnh trong git
```
git init
git status
git pull
git commit
git add .
git commit -m "Message"
git push origin <name_branch>
git log
git diff

```
## Thiết lập chứng thực cá nhân
```
git config --global user.name "User Name"
git config --global user.email "username@gmail.com"
```
## Tải về 
`git clone <path> -b <branch>`
## nhánh
```
git branch //kiểm tra nhánh hiện tại
git switch <banch> //chuyển nhánh
git checkout <name_branch> // chuyển nhánh
git branch <name_branch> // tạo nhánh 
git branch -b <name_branch> //tạo mới và chuyển đế

```
## merge với nhánh khác
`git merge <new_branch>`
## Xóa Commit
### Bỏ tất cả những commit local
`git reset --hard HEAD~1`

### Bỏ những thay đổi local chưa commit
`git reset --soft HEAD~1`
