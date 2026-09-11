---
tags: [汇编, 系统调用]
---

# DOS 与 BIOS 功能调用

> 由操作系统/固化程序提供的子程序，用 `INT n` 软中断实现调用

## DOS功能调用 — INT 21H

使用方法：
1. `AH ← 功能号`
2. 设置入口参数
3. `INT 21H`
4. 分析出口参数

## 常用DOS功能

### 键盘输入
| 功能号 | 功能 | 入口 | 出口 |
|--------|------|------|------|
| 01H | 输入一个字符（带回显） | — | AL=ASCII码 |
| 0AH | 输入字符串 | DS:DX=缓冲区首址 | 缓冲区含输入串 |

**0AH缓冲区格式：**
```
[缓冲区长度(N1)] [实际字符数(N2)] [N1字节空间...]
```
- N1为最大可输入字符数（超过则不再接收）

### 显示输出
| 功能号 | 功能 | 入口 |
|--------|------|------|
| 02H | 显示一个字符 | DL=字符ASCII |
| 09H | 显示字符串 | DS:DX=字符串首址（**以'$'结尾**） |

### 返回DOS
| 功能号 | 功能 |
|--------|------|
| 4CH | 带返回码退出，返回DOS |

---

## BIOS功能调用
> 固化在EPROM中的基本I/O子程序

| 中断类型 | 功能范围 |
|----------|---------|
| **INT 10H** | 屏幕显示 |
| **INT 13H** | 磁盘操作 |
| **INT 14H** | 串行口操作 |
| **INT 16H** | 键盘操作 |
| **INT 17H** | 打印机操作 |
| **INT 1AH** | 读写时钟参数 |

---

## 典型示例

### 显示 'Hello, World!'
```assembly
data SEGMENT
    Hello DB 'Hello, world!', 0DH, 0AH, '$'
data ENDS

code SEGMENT
    ASSUME CS:code, DS:data
start:
    MOV AX, data
    MOV DS, AX
    LEA DX, Hello     ; 串首地址
    MOV AH, 9         ; 功能号
    INT 21H           ; DOS调用
    MOV AH, 4CH
    INT 21H           ; 返回DOS
code ENDS
    END start
```

### 等待用户输入Y/N
```assembly
GET_KEY:
    MOV AH, 1         ; 等键入
    INT 21H
    CMP AL, 'Y'
    JZ  YES
    CMP AL, 'N'
    JZ  NO
    JMP GET_KEY       ; 都不是则继续等
YES: ...
NO:  ...
```
