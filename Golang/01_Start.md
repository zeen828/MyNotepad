# Golang

## 安裝
> 
>> ### linux
>>     sudo apt-get install golang  # (Linux)
>>     brew install go  # (Mac)
>> 指令安裝
> 
>> ### windows
>> 透過官網下載安裝包
>> [Golang官網](https://golang.org/)
> 

## 環境設定
搜尋`控制台`

開啟控制台面板 => 系統與安全 => 系統 => 進階系統設定

進階 => 環境變數

```
GOPATH => G工作目錄
GOROOT => Go安裝目錄
GOBIN => Go命令目錄
```
安裝完測試，沒反應的話需要重開機

```
>echo %GOPATH%
C:\Users\taki\go

>echo %GOROOT%
C:\Go

>echo %GOBIN%
C:\Go\bin
```
命令指令下輸入可查看結果

## 安裝套件
    go get -u github.com/jackdanger/collectlinks

## 運行
    go run crawler.go

[用Golang写爬虫（一）](https://zhangslob.github.io/2019/01/16/Golang%E5%86%99%E7%88%AC%E8%99%AB/)


[motphjm](https://motphjm.net/)