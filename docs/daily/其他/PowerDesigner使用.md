1. 如何使 model 中 `code` 和 `name` 不做关联

   ![](https://img.mupaie.com/20200609103106.png)

步骤：

Tools -> General Options -> Dialog 

![](https://img.mupaie.com/20200609103336.png)

2. table 显示 comment 注释信息

   ![](https://img.mupaie.com/20200609103526.png)

步骤：

Tools -> Display Preferences -> Table

```
Option   Explicit   
    ValidationMode   =   True   
    InteractiveMode   =   im_Batch
    Dim blankStr
    blankStr   =   Space(1)
    Dim   mdl   '   the   current   model  
      
    '   get   the   current   active   model   
    Set   mdl   =   ActiveModel   
    If   (mdl   Is   Nothing)   Then   
          MsgBox   "There   is   no   current   Model "   
    ElseIf   Not   mdl.IsKindOf(PdPDM.cls_Model)   Then   
          MsgBox   "The   current   model   is   not   an   Physical   Data   model. "   
    Else   
          ProcessFolder   mdl   
    End   If  
      
    Private   sub   ProcessFolder(folder)   
    On Error Resume Next  
          Dim   Tab   'running     table   
          for   each   Tab   in   folder.tables   
                if   not   tab.isShortcut   then   
                      tab.name   =   tab.comment  
                      Dim   col   '   running   column   
                      for   each   col   in   tab.columns   
                      if col.comment = "" or replace(col.comment," ", "")="" Then
                            col.name = blankStr
                            blankStr = blankStr & Space(1)
                      else  
                            col.name = col.comment   
                      end if  
                      next   
                end   if   
          next  
      
          Dim   view   'running   view   
          for   each   view   in   folder.Views   
                if   not   view.isShortcut   then   
                      view.name   =   view.comment   
                end   if   
          next  
      
          '   go   into   the   sub-packages   
          Dim   f   '   running   folder   
          For   Each   f   In   folder.Packages   
                if   not   f.IsShortcut   then   
                      ProcessFolder   f   
                end   if   
          Next   
    end   sub
```

