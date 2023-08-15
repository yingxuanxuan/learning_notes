# notepad++

### 删除整行

```
查找目标：.*string.*\r\n
替换为：空白 
```

### 使用notepad++运行python
``` powershell
# 运行 /K 保持窗口 -u unbuffered
cmd /K py -3 -u $(FULL_CURRENT_PATH)

# pdb -m module
cmd /K py -3 -u -m pdb $(FULL_CURRENT_PATH)

# 交互运行 -i interactive 
cmd /K py -3 -u -i $(FULL_CURRENT_PATH)
```
