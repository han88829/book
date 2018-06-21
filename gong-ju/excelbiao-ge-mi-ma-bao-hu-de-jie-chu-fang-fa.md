

表格受密码保护时，我们修改数据Excel弹出“您试图更改的单元格或图表受保护，因而是只读的。

若要修改受保护单元格或图表，请先使用‘撤消工作表保护’命令\(在‘审阅’选项卡的‘更改’组中\)来取消保护。

可能会提示输入密码。这时候我们可以用VBA宏代码破解法来破解表格保护密码：



  第一步：打开该文件，先解除默认的“宏禁用”状态，方法是点击工具栏下的“选项”状态按钮，

         打开“Microsoft Office安全选项”窗口，选择其中的“启用此内容”，“确定”

         再切换到“视图”选项卡，点击“宏”→“录制宏”，出现“录制新宏”窗口，在“宏名”定义一个名称为：  

         PasswordBreaker，点击“确定”退出；

  第二步：再点击“宏”→“查看宏”，选择“宏名”下的“PasswordBreaker”并点击“编辑”，

         打开“Microsoft Visual Basic”编辑器，用如下内容替换右侧窗口中的所有代码：



```
Sub PasswordBreaker()
    Dim i As Integer, j As Integer, k As Integer
    Dim l As Integer, m As Integer, n As Integer
    Dim i1 As Integer, i2 As Integer, i3 As Integer
    Dim i4 As Integer, i5 As Integer, i6 As Integer
    On Error Resume Next
    For i = 65 To 66: For j = 65 To 66: For k = 65 To 66
    For l = 65 To 66: For m = 65 To 66: For i1 = 65 To 66
    For i2 = 65 To 66: For i3 = 65 To 66: For i4 = 65 To 66
    For i5 = 65 To 66: For i6 = 65 To 66: For n = 32 To 126
    ActiveSheet.Unprotect Chr(i) & Chr(j) & Chr(k) & Chr(l) & Chr(m) & Chr(i1) & Chr(i2) & Chr(i3) & Chr(i4) & Chr(i5) & Chr(i6) & Chr(n)
    If ActiveSheet.ProtectContents = False Then
    MsgBox "One usable password is " & Chr(i) & Chr(j) & Chr(k) & Chr(l) & Chr(m) & Chr(i1) & Chr(i2) & Chr(i3) & Chr(i4) & Chr(i5) & Chr(i6) & Chr(n)
    ActiveWorkbook.Sheets(1).Select
    Range("a1").FormulaR1C1 = Chr(i) & Chr(j) & Chr(k) & Chr(l) & Chr(m) & Chr(i1) & Chr(i2) & Chr(i3) & Chr(i4) & Chr(i5) & Chr(i6) & Chr(n)
    Exit Sub
    End If
    Next: Next: Next: Next: Next: Next
    Next: Next: Next: Next: Next: Next
End Sub
```



 第三步：再点击“宏”→“查看宏”，选择“宏名”下的“PasswordBreaker”并点击“执行”，密码就现形了



  第四步：点击“撤消工作表保护”，然后输入密码即可解除锁定；





测试结果，密码破解可用，但是很晕的是破解的密码跟原来的密码有很大的差距，想不明白了，反正能用就好。

