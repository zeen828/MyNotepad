# Git的指令備忘錄

## 對專案建立Git Repository(檔案庫)
    git init

---

## 查看工作環境Git狀態
    git status
看到紅色是有異動的或新檔案

看到綠色是有已加入未commits(提交)的

---

## 查詢差異
    git diff
查詢全部差異
    git diff README.md
查詢指定檔案差異

---

## 檔案加入Git版控
    git add .
全部檔案加入Git版控

    git add README.md
指定檔案加入Git版控

---

## commit(提交)
    git commit -m "description"
提交一個異動

---

## 查詢Git版本
    git --version

---