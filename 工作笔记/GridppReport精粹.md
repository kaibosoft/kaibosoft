#### 脚本程序：判断是否是数字
    function isNumber(value) {
        var patrn = /^(-)?\d+(\.\d+)?$/;
        if (patrn.exec(value) == null || value == "") {
            return false
        } else {
            return true
        }
    }
    //parseFloat 字符串转换为数字
#### 脚本程序：通过字段的值设置字段显示的值
    //Report.DetailGrid.Recordset.F年月.GetDisplayTextScript(字段.取显示文字脚本)
    function Report.DetailGrid.Recordset.F年月.GetDisplayTextScript(Report, Sender)
    {
        if (Sender.IsNull)
          Sender.DisplayText = "未知";
        else if (Sender.AsString == "01月")
          Sender.DisplayText = "一月";
        else if (Sender.AsString == "02月")
          Sender.DisplayText = "二月";
        else if (Sender.AsString == "03月")
          Sender.DisplayText = "三月";
        else if (Sender.AsString == "04月")
          Sender.DisplayText = "四月";
        else if (Sender.AsString == "05月")
          Sender.DisplayText = "五月";
        else if (Sender.AsString == "06月")
          Sender.DisplayText = "六月";
        else if (Sender.AsString == "07月")
          Sender.DisplayText = "七月";
        else if (Sender.AsString == "08月")
          Sender.DisplayText = "八月";
        else if (Sender.AsString == "09月")
          Sender.DisplayText = "九月";
        else if (Sender.AsString == "10月")
          Sender.DisplayText = "十月";
        else if (Sender.AsString == "11月")
          Sender.DisplayText = "十一月";
        else if (Sender.AsString == "12月")
          Sender.DisplayText = "十二月";
        else
          Sender.DisplayText = "未知";
    }
#### 脚本程序：获取FMore字段对应的分拆
    //Report.DetailGrid.Recordset.BeforePostRecordScript(记录集.提交记录前脚本)
    function Report.DetailGrid.Recordset.BeforePostRecordScript(Report, Sender)
    {
        var MoreField = Sender.Fields.Item("FMore");
        var MoreFieldStr = MoreField.AsString;
        var MoreFields=MoreFieldStr.split(",");
        var MoreCount=MoreFields.length;
        var MoreName="";
        var MoreData="";
        var MoreField01 = Sender.Fields.Item("成型取数");
        var MoreField02 = Sender.Fields.Item("成型厚度");
        var MoreField03 = Sender.Fields.Item("成型着算值");
        var MoreField04 = Sender.Fields.Item("成型着磁时机");
        var MoreField05 = Sender.Fields.Item("成型拨算值");
        var MoreField06 = Sender.Fields.Item("成型下压值");
        var MoreField07 = Sender.Fields.Item("成型压力");
        for(var i=0;i<MoreCount;i++){    
          MoreName=MoreFields[i].split("=")[0];
          MoreData=MoreFields[i].split("=")[1];
          if (MoreName=="成型取数") MoreField01.AsString=MoreData;
          else if (MoreName=="成型厚度") MoreField02.AsString=MoreData; 
          else if (MoreName=="成型着算值") MoreField03.AsString=MoreData; 
          else if (MoreName=="成型着磁时机") MoreField04.AsString=MoreData; 
          else if (MoreName=="成型拨算值") MoreField05.AsString=MoreData; 
          else if (MoreName=="成型下压值") MoreField06.AsString=MoreData; 
          else if (MoreName=="成型压力") MoreField07.AsString=MoreData; 
        }
    }
#### 脚本程序：通过一个参数作为中转计算某个字段的累计值
    //Report.InitializeScript(报表主对象.初始化脚本)
    function Report.InitializeScript(Report, Sender)
    {
        Report.ParameterByName("p累计数量").AsFloat = 0;
    }
    ​
    //Report.DetailGrid.Recordset.ProcessRecordScript(记录集.处理记录脚本)
    function Report.DetailGrid.Recordset.ProcessRecordScript(Report, Sender)
    {
        //把当前"Amount"字段的值累加到参数"SumParam"中
        var SumQty = Report.ParameterByName("p累计数量");
        SumQty.AsFloat = SumQty.AsFloat + Report.FieldByName("F发生数量").AsFloat;
        //给"SumAmount"字段设上累计值
        Sender.Edit();
        Sender.Fields.Item("F结存数量").AsFloat = SumQty.AsFloat;
        Sender.Post();
    }
#### 占列式分组：
    占列式分组其分组头不占据单独的显示行，而是在指定占据的列区域现实分组头信息。可以指定多个占据的列，列名称之间用‘;’隔开。本示例不定义分组头显示部件框，分组头的显示内容来自对应列的内容格。
    >基本使用方法是，在分组头，设置分组单元格合并信息，合并方式为是，设置合并列，支持多列，显示同列设置为是，包括分组尾设置为是。
    >如果需要设置合并列显示多种信息，将分组头扩大，放置多个字段框，竖着排列即可
    >多级分组也可以
    >可将分组设置为页分组，不需要设置分组字段，应用在单据打印很有用
    >将分组头尾的打印输出边框设置为否，则分组头尾便没有了边框线
    >将分组头的每页重复打印设置为是，则上一页没有显示结束的分组，下一页会继续显示在顶部
    >新的分组可重启页号，设置分组头尾的“保持同页”为是，分组尾的换新页使用节后，可在页脚加综合文本框，文本：分组([#Region#])：第[#SystemVar(GroupPageNo,1)#]页/共[#SystemVar(GroupPageCount,1)#]页
    >设置分组的“排序统计框”属性和“排序按升序”属性，表格将按对应统计框的值对分组项数据进行排序。
    计算字段：
    金额(SQL)：计算在SQL语句中实现。
    金额(方式1)：计算在程序中的 BoforePostRecord 报表事件中实现，或者通过脚本实现。
    举例：
    //Report.DetailGrid.Recordset.BeforePostRecordScript(记录集.提交记录前脚本)
    function Report.DetailGrid.Recordset.BeforePostRecordScript(Report, Sender)
    {
        var AmtFld = Sender.Fields.Item("Amount");
        var QtyFld = Sender.Fields.Item("Quantity");
        var PriceFld = Sender.Fields.Item("UnitPrice");
        AmtFld.AsFloat = QtyFld.AsFloat*PriceFld.AsFloat;
    }
​
#### 脚本程序：根据本行是否有颜色，决定按照25公斤一包推算包数
    function Report.DetailGrid.Recordset.BeforePostRecordScript(Report, Sender)
    {
        var ColorFld = Sender.Fields.Item("物料颜色");
        var QtyFld = Sender.Fields.Item("本行数量");
        var BaoFld = Sender.Fields.Item("包数");
        if (ColorFld.asstring.length>0)
        {
          BaoFld.AsFloat = QtyFld.AsFloat/25.0;
        } 
        else
        {
          BaoFld.AsFloat =0;
        }
    }
    //
     var TypeField = Sender.Fields.Item("F单据类型");
    var AmtField = Sender.Fields.Item("F送货部分");
    if (TypeField.asstring=="销售退货")
    {
      AmtField.AsFloat=AmtField.AsFloat*(-1.0);
    } 
    //整改数字
        var SONeedQtyFld = Sender.Fields.Item("F订单缺库");
        var RQNeedQtyFld = Sender.Fields.Item("F缺库数量");
        if (SONeedQtyFld.asfloat>=0)
        {
          SONeedQtyFld.AsFloat = 0;
        } 
        else
        {
          SONeedQtyFld.AsFloat =SONeedQtyFld.AsFloat*(-1.0);
        }
        if (RQNeedQtyFld.asfloat>=0)
        {
          RQNeedQtyFld.AsFloat = 0;
        } 
        else
        {
          RQNeedQtyFld.AsFloat =RQNeedQtyFld.AsFloat*(-1.0);
        }​
#### 脚本程序：累计求和-库存进出累计
    //Report.InitializeScript(报表主对象.初始化脚本)
    function Report.InitializeScript(Report, Sender)
    {
        Report.ParameterByName("p累计数量").AsFloat = 0;
        Report.ParameterByName("p累计段数").AsFloat = 0;
    }
    //Report.DetailGrid.Recordset.ProcessRecordScript(记录集.处理记录脚本)
    function Report.DetailGrid.Recordset.ProcessRecordScript(Report, Sender)
    {
        //把当前"Amount"字段的值累加到参数"SumParam"中
        var SumQty = Report.ParameterByName("p累计数量");
        var SumPackQty = Report.ParameterByName("p累计段数");
        SumQty.AsFloat = SumQty.AsFloat + Report.FieldByName("F发生数量").AsFloat;
        SumPackQty.AsFloat = SumPackQty.AsFloat + Report.FieldByName("F发生段数").AsFloat;
        //给"SumAmount"字段设上累计值
        Sender.Edit();
        Sender.Fields.Item("F累计数量").AsFloat = SumQty.AsFloat;
        Sender.Fields.Item("F累计段数").AsFloat = SumPackQty.AsFloat;
        Sender.Post();
    }
#### 脚本程序：设置文字颜色
    function Report.DetailGrid.ColumnContent.FormatScript(Report, Sender)
    {
        var TextColorField = Report.FieldByName("F字体颜色");
        var TextColor;
        TextColor=TextColorField.AsInteger;
        Sender.SetCellsForeColor(TextColor);
    }
#### 脚本程序：设置背景行颜色
    //Report.DetailGrid.ColumnContent.FormatScript(内容行.格式化脚本)
    function Report.DetailGrid.ColumnContent.FormatScript(Report, Sender)
    {
        var StatusField = Report.FieldByName("F处理状态");
        var BackColor;
        if (trimStr(StatusField.AsString)=="已确认"){
          BackColor=GetColorValue(0, 255, 127);
        } else {
          BackColor=GetColorValue(255, 255, 255);
        }
        Sender.SetCellsBackColor(BackColor);
    ​
        //删除首尾空格
        function trimStr(str)
        {
        return str.replace(/(^\s*)|(\s*$)/g,"");
        }
        //根据三原色求出颜色值
        function GetColorValue(r,g,b)
        {
          return r + g*256 + b*256*256;
        }
    }
#### 脚本程序：根据传递的字段确定某一行是否显示
    function Report.DetailGrid.ColumnContent.FormatScript(Report, Sender)
    {
        var FlagField = Report.FieldByName("FRowSource");
        Sender.Visible =  (FlagField.AsInteger==1);
    }​
#### 脚本程序：在某个字段的脚本
    function Report.DetailGrid.Recordset.字段.GetDisplayTextScript(Report, Sender)
    {
        if (Sender.IsNull)
          Sender.DisplayText = "待定";
        else if (Sender.AsBoolean == true)
          Sender.DisplayText = "xxx";
        else
          Sender.DisplayText = "xxx";
    }​
#### 脚本程序：追加空白行
    function Report.ProcessBeginScript(Report, Sender)
    {
        //假设每页要显示20 行，求出最后要补充的行数
        var AppendRows = 20 - (Report.DetailGrid.Recordset.RecordCount % 20);
        if (AppendRows == 20)
          AppendRows = 0;
        for (i=0; i<AppendRows; ++i)
        {
          Report.DetailGrid.Recordset.Append();
          Report.DetailGrid.Recordset.Post();
        } 
    }​
#### 脚本程序：字体及背景色特殊显示综合脚本，含交叉表类的处理等
    //周六或周日时，使用橙黄色
    //周一到周五时，使用黑色 
    //星期列
    var WeekContentCell = Sender.ContentCells.Item("星期");
    var WeekField = Report.FieldByName("F星期");
    var WeekFontBold;
    var WeekFontItalic;
    var WeekTextColor;
    var WeekBackColor;
    //设置判断
    var RowBackColor;
    var RowForeColor;
    if ((WeekField.AsString == "六")||(WeekField.AsString == "日"))
    {
        WeekFontBold = true;
        WeekFontItalic = false;
        WeekTextColor = GetColorValue(255, 165, 0);
        WeekBackColor = GetColorValue(255, 255, 255);
        RowBackColor = GetColorValue(255, 255, 255);
        RowForeColor = GetColorValue(0, 0, 0);
    }
    else if (WeekField.AsString == "期初")
    {
        WeekFontBold = true;
        WeekFontItalic = false;
        WeekTextColor = GetColorValue(255, 165, 0);
        WeekBackColor = GetColorValue(255, 192, 203);
        RowBackColor = GetColorValue(255, 192, 203);
        RowForeColor = GetColorValue(0, 0, 0);
    }
    else
    {
        WeekFontBold = true;
        WeekFontItalic = false;
        WeekTextColor = GetColorValue(51, 102, 255);
        WeekBackColor = GetColorValue(255, 255, 255);
        RowBackColor = GetColorValue(255, 255, 255);
        RowForeColor = GetColorValue(0, 0, 0);
    }
    //设置整行的背景色
    Sender.SetCellsBackColor(RowBackColor);
    Sender.SetCellsForeColor(RowForeColor);
    Sender.Font.Bold = true;
    //设置某个格子的配置
    WeekContentCell.Font.Bold = WeekFontBold;
    WeekContentCell.Font.Italic = WeekFontItalic;
    WeekContentCell.ForeColor = WeekTextColor;
    WeekContentCell.BackColor = WeekBackColor;
    //纸箱和胶箱前景色
    var ColumnCount=Sender.ContentCells.Count;
    for (Index=3; Index<ColumnCount+1; ++Index)//如果没有汇总列用ColumnCount+1，否则用ColumnCount
    {
      var SomeContentCell = Sender.ContentCells.Item(Index);
      var SomeField = Report.RunningDetailGrid.Recordset.Fields.Item(SomeContentCell.DataField);
      var SomeContentCellName = SomeContentCell.Name;
      if (isContains(SomeContentCellName,"出荷")){
        //SomeContentCell.Font.Bold = FontBold;
        //SomeContentCell.Font.Italic = FontItalic;
        SomeContentCell.ForeColor = GetColorValue(255, 0, 0);
        //SomeContentCell.BackColor = BackColor;
      }
      if (SomeField.AsFloat<0){
        //SomeContentCell.ForeColor = GetColorValue(255, 0, 0);
        SomeContentCell.BackColor = GetColorValue(255, 0, 0);
      }
    }
    //根据三原色求出颜色值
    function GetColorValue(r,g,b)
    {
       return r + g*256 + b*256*256;
    }
    //是否包含子字符串
    function isContains(str, substr) 
    {
       return str.indexOf(substr) >= 0;
    }
#### 脚本程序：根据条件特殊显示字体
    function Report.DetailGrid.ColumnContent.FormatScript(Report, Sender)
    {
        //当Amount字段的值大于等于5000时，将Amount显示为粗体，绿色,背景色为白色
        //当Amount字段的值大于等于1000时，将Amount显示为正常体，黄色,背景色为蓝色
        //当Amount字段的值小于1000时，将Amount显示为正常体，红色，背景色为白色
        var AmountContentCell = Sender.ContentCells.Item("Amount");
        var AmountField = Report.FieldByName("Amount");
        var FontBold;
        var FontItalic;
        var TextColor;
        var BackColor;
        if (AmountField.AsFloat >= 5000)
        {
            FontBold = true;
            FontItalic = false;
            TextColor = GetColorValue(0, 255, 0);
            BackColor = GetColorValue(255, 255, 255);
        }
        else if (AmountField.AsFloat >= 1000)
        {
            FontBold = false;
            FontItalic = false;
            TextColor = GetColorValue(255, 255, 0);
            BackColor = GetColorValue(0, 0, 255);
        }
        else
        {
            FontBold = false;
            FontItalic = true;
            TextColor = GetColorValue(255, 0, 0);
            BackColor = GetColorValue(255, 255, 255);
        }
        ////当为打印输出状态时，文字颜色始终为黑色,背景色始终为白色
        //if (Report.DisplayMode == 2) //grrdmPrintGenerate
        //{
        //    TextColor = GetColorValue(0, 0, 0);
        //    BackColor = GetColorValue(255, 255, 255);
        //}
        AmountContentCell.Font.Bold = FontBold;
        AmountContentCell.Font.Italic = FontItalic;
        AmountContentCell.ForeColor = TextColor;
        AmountContentCell.BackColor = BackColor;
        //根据三原色求出颜色值
        function GetColorValue(r,g,b)
        {
           return r + g*256 + b*256*256;
        }
    }​
#### 脚本程序：根据条件交叉表列特殊显示字体
    //这是一个写在内容行上的脚本，通过改变外观属性实现以不同方式显示不同类别的内容
    //当Amount字段的值大于等于500时，将Amount显示为粗体，绿色,背景色为白色
    //当Amount字段的值大于等于200时，将Amount显示为正常体，黄色,背景色为蓝色
    //当Amount字段的值小于200时，将Amount显示为正常体，红色，背景色为白色
    //交叉出来的明细网格，除了前两列与最后一列不是数据列，中间的都要进行处理
    var ColumnCount=Sender.ContentCells.Count;
    for (Index=3; Index<ColumnCount; ++Index)
    {
      var AmountContentCell = Sender.ContentCells.Item(Index);
      var AmountField = Report.RunningDetailGrid.Recordset.Fields.Item(AmountContentCell.DataField);
      var FontBold;
      var FontItalic;
      var TextColor;
      var BackColor;
      if (AmountField.AsFloat >= 500)
      {
        FontBold = true;
        FontItalic = false;
        TextColor = GetColorValue(0, 255, 0);
        BackColor = GetColorValue(255, 255, 255);
      }
      else if (AmountField.AsFloat >= 200)
      {
        FontBold = false;
        FontItalic = false;
        TextColor = GetColorValue(255, 255, 0);
        BackColor = GetColorValue(0, 0, 255);
      }
      else
      {
         FontBold = false;
         FontItalic = true;
         TextColor = GetColorValue(255, 0, 0);
         BackColor = GetColorValue(255, 255, 255);
      }
    ​
      AmountContentCell.Font.Bold = FontBold;
      AmountContentCell.Font.Italic = FontItalic;
      AmountContentCell.ForeColor = TextColor;
      AmountContentCell.BackColor = BackColor;
    }
    ​
    function GetColorValue(r,g,b)
    {
      return r + g*256 + b*256*256;
    } 
#### 脚本程序：整行突出显示
    //Report.DetailGrid.ColumnContent.FormatScript(内容行.格式化脚本)
    function Report.DetailGrid.ColumnContent.FormatScript(Report, Sender)
    {
        //当Amount字段的值大于等于5000时，文字绿色,背景色为白色
        //当Amount字段的值大于等于1000时，文字黄色,背景色为蓝色
        //当Amount字段的值小于1000时，文字红色，背景色为白色
        var AmountField = Report.FieldByName("Amount");
        var TextColor;
        var BackColor;
        if (AmountField.AsFloat >= 5000)
        {
            TextColor = GetColorValue(0, 255, 0);
            BackColor = GetColorValue(255, 255, 255);
        }
        else if (AmountField.AsFloat >= 1000)
        {
            TextColor = GetColorValue(255, 255, 0);
            BackColor = GetColorValue(0, 0, 255);
        }
        else
        {
            TextColor = GetColorValue(255, 0, 0);
            BackColor = GetColorValue(255, 255, 255);
        }
        Sender.SetCellsBackColor( BackColor );
        Sender.SetCellsForeColor( TextColor );
        //根据三原色求出颜色值
        function GetColorValue(r,g,b)
        {
           return r + g*256 + b*256*256;
        }
    }
    //Report.DetailGrid.Group1.GroupFooter.FormatScript(分组尾.格式化脚本)
    function Report.DetailGrid.Group1.GroupFooter.FormatScript(Report, Sender)
    {
        Report.ControlByName("Summary2").BackColor = 255;
    }​
#### 脚本程序：计算字段
    //Report.DetailGrid.Recordset.BeforePostRecordScript(记录集.提交记录前脚本)
    function Report.DetailGrid.Recordset.BeforePostRecordScript(Report, Sender)
    {
        var AmtFld = Sender.Fields.Item("Amount");
        var QtyFld = Sender.Fields.Item("Quantity");
        var PriceFld = Sender.Fields.Item("UnitPrice");
        AmtFld.AsFloat = QtyFld.AsFloat*PriceFld.AsFloat;
    }​
#### 脚本程序：用脚本实现收发存计算
    //Report.DetailGrid.Recordset.BeforePostRecordScript(记录集.提交记录前脚本)
    function Report.DetailGrid.Recordset.BeforePostRecordScript(Report, Sender)
    {
        //这里的脚本代码仅仅只是为了产生模拟数据，与现实情况可能不符
        //根据日期转换的整数值，根据奇偶条件分别设置收支金额字段的值
        var OrderDate = Sender.Fields.Item("OrderDate").AsInteger;
        var Amount = Sender.Fields.Item("Amount").AsFloat;
        if (OrderDate % 2)
            Sender.Fields.Item("OutAmount").AsFloat = Amount;
        else
            Sender.Fields.Item("InAmount").AsFloat = Amount;
    }
    //Report.DetailGrid.Recordset.ProcessRecordScript(记录集.处理记录脚本)
    function Report.DetailGrid.Recordset.ProcessRecordScript(Report, Sender)
    {
        //把当前"Amount"字段的值累加到参数"SumParam"中
        var SumParam = Report.ParameterByName("SumParam");
        SumParam.AsFloat = SumParam.AsFloat + Report.FieldByName("InAmount").AsFloat - Report.FieldByName("OutAmount").AsFloat;
        //给"SumAmount"字段设上累计值
        Sender.Edit();
        Sender.Fields.Item("SumAmount").AsFloat = SumParam.AsFloat;
        Sender.Post();
    }
    //Report.DetailGrid.Group1.GroupBeginScript(分组.分组开始脚本)
    function Report.DetailGrid.Group1.GroupBeginScript(Report, Sender)
    {
        //开始一个新分组(一个新产品的分组),将统计累计值的参数"SumParam"的值设为0
        Report.ParameterByName("SumParam").AsFloat = 0;
    }​
#### 脚本程序：按条件控制列的可见性
##### 方法一：
    //Report.ProcessBeginScript(报表主对象.开始处理脚本)
    function Report.ProcessBeginScript(Report, Sender)
    {
        //根据条件隐藏列：如果字段值全部为空，则隐藏对应列
        var rs = Report.DetailGrid.Recordset;
        var fld = rs.Fields("传真");
        var show = false;
        //遍历记录集，判断字段是否为空
        rs.First();
        while ( !rs.Eof() )
        {
          if ( !fld.IsNull )
          {
            show = true;
            break;
          }
          rs.Next();
        }
        Report.ColumnByName("FaxColumn").Visible = show;
    }
    //Report.DetailGrid.Recordset.BeforePostRecordScript(记录集.提交记录前脚本)
    function Report.DetailGrid.Recordset.BeforePostRecordScript(Report, Sender)
    {
        //故意设置传真字段的数据全为空
        Report.FieldByName("传真").Clear();
    }
##### 方法二：
    //Report.ProcessBeginScript(报表主对象.开始处理脚本)
    function Report.ProcessBeginScript(Report, Sender)
    {
        //如果整个列都没有数据，则将此列隐藏不显示
        var rs = Report.DetailGrid.Recordset;
        var FieldCount = rs.Fields.Count;
        var FieldVisibles = new Array(FieldCount);
        for (i=0; i<FieldCount; ++i)
          FieldVisibles[i] = false;
        //遍历记录集，判断字段是否为空，并记录在FieldVisibles中
        rs.First();
        while ( !rs.Eof() )
        {
          for (i=1; i<=FieldCount; ++i)
          {
            if ( !rs.Fields.Item(i).IsNull )
              FieldVisibles[i-1] = true;
          }
          rs.Next();
        }
        //根据字段找到对应的显示列，并根据FieldVisibles设置列的可见性
        var ColumnCount = Report.DetailGrid.Columns.Count;
        for (i=1; i<=FieldCount; ++i)
        {
          var FieldName = rs.Fields.Item(i).Name;
          for (j=1; j<=ColumnCount; ++j)
          {
            var Column = Report.DetailGrid.Columns.Item(j);
            var ContentCell = Column.ContentCell;
            if (ContentCell.FreeCell)
              continue;
            if (ContentCell.DataField == FieldName)
            {
              Column.Visible = FieldVisibles[i-1]; 
              continue; 
            }
          }
        }
    }​
#### 脚本程序：表格最后页空白画斜线
    //Report.DetailGrid.Group1.GroupFooter.FormatScript(分组尾.格式化脚本)
    function Report.DetailGrid.Group1.GroupFooter.FormatScript(Report, Sender)
    {
        //根据页面剩余高度设置分组尾的显示高度
        var bh = Report.PageBlankHeight - Report.PixelsToUnit(2);  //Report.PixelsToUnit(2)是为了减去边框的宽度
        Sender.Height = bh;
    }
    //Report.DetailGrid.Group1.GroupFooter.Group1.StaticBox12.CustomDrawScript(静态框.自绘脚本)
    function Report.DetailGrid.Group1.GroupFooter.Group1.StaticBox12.CustomDrawScript(Report, Sender)
    {
        //Sender.DrawDefault(); //默认绘制，这里不需要，只要画一条斜线
        var Graphics = Report.Graphics;
        //+4 -8 是为了留出一定的边距
        var left = Graphics.Left + 4;
        var top = Graphics.Top + 4;
        var right = left + Graphics.Width - 8;
        var bottom = top + Graphics.Height - 8;
        //设定绘出线型
        Graphics.SelectPen(1, 0/*black*/, 0/*grpsSolid*/);
        //画从下到上的斜线
        Graphics.MoveTo(left, bottom);
        Graphics.LineTo(right, top);
        //恢复绘出线型设定，SelectPen调用之后，必须对应调用RestorePen
        Graphics.RestorePen();
    }​
#### 脚本程序：多行交替色显示明细行
    //Report.DetailGrid.ColumnContent.FormatScript(内容行.格式化脚本)
    function Report.DetailGrid.ColumnContent.FormatScript(Report, Sender)
    {
        //每隔3行实现背景色变换
        var RecordNo = Report.SystemVarValue(4)-1; //grsvRecordNo 4 明细记录的当前记录号，从1开始计数。 
        var TextColor;
        var BackColor;
        var Odd = (Math.floor(RecordNo/3) % 2 == 0);
        if (Odd == true)
        {
            TextColor = GetColorValue(255, 0, 0);
            BackColor = GetColorValue(255, 255, 255);
        }
        else
        {
            TextColor = GetColorValue(0, 0, 0);
            BackColor = GetColorValue(196, 255, 255);
        }
        Sender.SetCellsBackColor( BackColor );
        Sender.SetCellsForeColor( TextColor );
        //根据三原色求出颜色值
        function GetColorValue(r,g,b)
        {
           return r + g*256 + b*256*256;
        }
    }​
#### 脚本程序：交叉表脚本控制动态列突出显示
    //Report.DetailGrid.ColumnContent.FormatScript(内容行.格式化脚本)
    function Report.DetailGrid.ColumnContent.FormatScript(Report, Sender)
    {
        //这是一个写在内容行上的脚本，通过改变外观属性实现以不同方式显示不同类别的内容
        //当Amount字段的值大于等于500时，将Amount显示为粗体，绿色,背景色为白色
        //当Amount字段的值大于等于200时，将Amount显示为正常体，黄色,背景色为蓝色
        //当Amount字段的值小于200时，将Amount显示为正常体，红色，背景色为白色
        //交叉出来的明细网格，除了前两列与最后一列不是数据列，中间的都要进行处理
        var ColumnCount=Sender.ContentCells.Count;
        for (Index=3; Index<ColumnCount; ++Index)
        {
          var AmountContentCell = Sender.ContentCells.Item(Index);
          var AmountField = Report.RunningDetailGrid.Recordset.Fields.Item(AmountContentCell.DataField);
          var FontBold;
          var FontItalic;
          var TextColor;
          var BackColor;
          if (AmountField.AsFloat >= 500)
          {
            FontBold = true;
            FontItalic = false;
            TextColor = GetColorValue(0, 255, 0);
            BackColor = GetColorValue(255, 255, 255);
          }
          else if (AmountField.AsFloat >= 200)
          {
            FontBold = false;
            FontItalic = false;
            TextColor = GetColorValue(255, 255, 0);
            BackColor = GetColorValue(0, 0, 255);
          }
          else
          {
             FontBold = false;
             FontItalic = true;
             TextColor = GetColorValue(255, 0, 0);
             BackColor = GetColorValue(255, 255, 255);
          }
          AmountContentCell.Font.Bold = FontBold;
          AmountContentCell.Font.Italic = FontItalic;
          AmountContentCell.ForeColor = TextColor;
          AmountContentCell.BackColor = BackColor;
        }
        function GetColorValue(r,g,b)
        {
          return r + g*256 + b*256*256;
        } 
    }​
#### 脚本程序：明细表格数据载入图表
    //Report.ProcessEndScript(报表主对象.结束处理脚本)
    function Report.ProcessEndScript(Report, Sender)
    {
        var AmtChart = Report.ControlByName("AmtChart").AsChart;
        var QtyChart = Report.ControlByName("QtyChart").AsChart;
        var Recordset = Report.DetailGrid.Recordset;
        var fldMonth = Report.FieldByName("OrderMonth");
        var fldProductID = Report.FieldByName("ProductID");
        var fldProductName = Report.FieldByName("ProductName");
        var fldAmt = Report.FieldByName("Amount");
        var fldQty = Report.FieldByName("Quantity");
        AmtChart.GroupCount = 12;
        AmtChart.SeriesCount = 4;
        QtyChart.GroupCount = 12;
        QtyChart.SeriesCount = 4;
        for (i=1; i<=12; ++i)
        {
          AmtChart.GroupLabel(i-1) = i + "月";
          QtyChart.GroupLabel(i-1) = i + "月";
        }
        //设置图表数据
        AmtChart.EmptyValues();
        QtyChart.EmptyValues();
        Recordset.First();
        while (!Recordset.Eof())
        {
          var Month = fldMonth.AsInteger;
          var ProductID = fldProductID.AsInteger;
          AmtChart.Value(ProductID-1, Month-1) = fldAmt.AsFloat;
          QtyChart.Value(ProductID-1, Month-1) = fldQty.AsFloat;
          AmtChart.SeriesLabel(ProductID-1) = fldProductName.AsString;
          QtyChart.SeriesLabel(ProductID-1) = fldProductName.AsString;
          Recordset.Next();
        }
    }​
#### 脚本程序：四舍五入明细字段值
    //Report.DetailGrid.Recordset.BeforePostRecordScript(记录集.提交记录前脚本)
    function Report.DetailGrid.Recordset.BeforePostRecordScript(Report, Sender)
    {
        //将字段值调用 NumberRound45 方法进行四舍五入
        var f = Sender.Fields.Item("Amount2");
        f.AsFloat = Report.Utility.NumberRound45(f.AsFloat, 2);
    }​
#### 脚本程序：明细文字缩进且隐藏单行分组明细
##### 方法一：
    实现要点：
    1、在字段的“取显示文字脚本”中补充空格字符实现文字缩进。
    2、用 SystemVarValue 方法获取当前明细行在当前分组中的行号，如果是第一行，显示文字前补充“其中”二字。
    3、在分组头中放置一个不显示的统计框用来统计当前分组中明细个数，在内容行的“格式化脚本”中获取此统计框的值，据此判断当前明细是否显示，实现单行分组明细不显示。
    //Report.DetailGrid.Recordset.CompanyName.GetDisplayTextScript(字段.取显示文字脚本)
    function Report.DetailGrid.Recordset.CompanyName.GetDisplayTextScript(Report, Sender)
    {
        //在文字前面补充几个空格字符，实现缩进效果
        //获取当前行在分组中的行号，如果是首行，在前面增加“其中”二字
        var GroupRowNo = Report.SystemVarValue2(22, 1);
        if (GroupRowNo == 1)
          Sender.DisplayText = "    其中:" + Sender.AsString;
        else
          Sender.DisplayText = "    " + Sender.AsString;
    }
    //Report.DetailGrid.ColumnContent.FormatScript(内容行.格式化脚本)
    function Report.DetailGrid.ColumnContent.FormatScript(Report, Sender)
    {
        //从统计框中获取当前分组的明细个数，据此确定是否隐藏明细行
        var GroupRowCount = Report.SystemVarValue2(23, 1); //23=grsvGroupRowCount
        Sender.Visible = (GroupRowCount > 1);
    }
##### 方法二：
    //Report.DetailGrid.Group1.GroupFooter.FormatScript(分组尾.格式化脚本)
    function Report.DetailGrid.Group1.GroupFooter.FormatScript(Report, Sender)
    {
        //取得分组项的记录行数，在分组尾取当前分组项行数
        var GroupRowCount = Report.SystemVarValue2(23, 1); //grsvGroupRowCount 23 分组项行数，某个分组项包含的明细记录(行)数。
        //根据当前分组项中的记录行数确定是否显示本分组尾行
        Sender.Visible = (GroupRowCount>1);
    }
    ​
    脚本程序：主表记录填入子表
    //Report.DetailGrid.Recordset.FetchRecordScript(记录集.取记录脚本)
    function Report.DetailGrid.Recordset.FetchRecordScript(Report, Sender)
    {
        if (ParentReport != null)
        {
            var pr = ParentReport.DetailGrid.Recordset;
            var mr = Sender;
            var FldCount = mr.Fields.Count;
            pr.First();
            while (pr.Eof() == false)
            {
                mr.Append();
                for (i=1; i<=FldCount; i++)
                {
                    mr.Fields.Item(i).Value = pr.Fields.Item(i).Value;
                }
                mr.Post();
                pr.Next();
            }
        }
    }​
#### 脚本程序：自由格文字按条件突出显示
    //Report.DetailGrid.ColumnContent.FormatScript(内容行.格式化脚本)
    function Report.DetailGrid.ColumnContent.FormatScript(Report, Sender)
    {
        //当Amount字段的值大于等于5000时，将Amount显示为粗体，绿色,背景色为白色
        //当Amount字段的值大于等于1000时，将Amount显示为正常体，黄色,背景色为蓝色
        //当Amount字段的值小于1000时，将Amount显示为正常体，红色，背景色为白色
        var AmountContentCell = Sender.ContentCells.Item("Amount");
        var ControlInCell = Report.ControlByName("MemoBox2");
        var AmountField = Report.FieldByName("Amount");
        var FontBold;
        var FontItalic;
        var TextColor;
        var BackColor;
        if (AmountField.AsFloat >= 5000)
        {
            FontBold = true;
            FontItalic = false;
            TextColor = GetColorValue(0, 255, 0);
            BackColor = GetColorValue(255, 255, 255);
        }
        else if (AmountField.AsFloat >= 1000)
        {
            FontBold = false;
            FontItalic = false;
            TextColor = GetColorValue(255, 255, 0);
            BackColor = GetColorValue(0, 0, 255);
        }
        else
        {
            FontBold = false;
            FontItalic = true;
            TextColor = GetColorValue(255, 0, 0);
            BackColor = GetColorValue(255, 255, 255);
        }
        AmountContentCell.Font.Bold = FontBold;
        AmountContentCell.Font.Italic = FontItalic;
        ControlInCell.ForeColor = TextColor; //AmountContentCell.ForeColor = TextColor; 自由格不允许设置前景色，在设计器也可发现其前景色属性不可用
        AmountContentCell.BackColor = BackColor;
        //根据三原色求出颜色值
        function GetColorValue(r,g,b)
        {
           return r + g*256 + b*256*256;
        }
    }​
#### 脚本程序：字符串转换为日期
    //Report.DetailGrid.Recordset.BeforePostRecordScript(记录集.提交记录前脚本)
    function Report.DetailGrid.Recordset.BeforePostRecordScript(Report, Sender)
    {
        var s = Report.FieldByName("OrderDateText").AsString;
        var y = s.substring(0, 4);
        var m = s.substring(4, 6);
        var d = s.substring(6, 8);
        var date = Report.Utility.CreateDateTime();
        date.ValueFromDate(y, m, d);
        Report.FieldByName("OrderDate").AsFloat = date.AsFloat;
    }​
#### 脚本程序：分组计算字段
    //Report.DetailGrid.PageGroup.GroupEndScript(分组.分组结束脚本)
    function Report.DetailGrid.PageGroup.GroupEndScript(Report, Sender)
    {
        var q = Report.ParameterByName("AccQty");
        var a = Report.ParameterByName("AccAmt");
        q.AsFloat += Report.ControlByName("Summary1").AsSummaryBox.Value;
        a.AsFloat += Report.ControlByName("Summary2").AsSummaryBox.Value;
    }
    //Report.DetailGrid.GroupByProduct.GroupHeader.FormatScript(分组头.格式化脚本)
    function Report.DetailGrid.GroupByProduct.GroupHeader.FormatScript(Report, Sender)
    {
        //必须在每个普通分组头输出时，将累计参数的值重新归零
        Report.ParameterByName("AccQty").AsFloat = 0;
        Report.ParameterByName("AccAmt").AsFloat = 0;
    }​
#### 脚本程序：
##### 1、计算字段
    //Report.DetailGrid.Recordset.BeforePostRecordScript(记录集.提交记录前脚本)
    function Report.DetailGrid.Recordset.BeforePostRecordScript(Report, Sender)
    {
        var AmtFld = Sender.Fields.Item("Amount");
        var QtyFld = Sender.Fields.Item("Quantity");
        var PriceFld = Sender.Fields.Item("UnitPrice");
        AmtFld.AsFloat = QtyFld.AsFloat*PriceFld.AsFloat;
    }
##### 2、显示字段值改变
    //Report.DetailGrid.Recordset.BeforePostRecordScript(记录集.提交记录前脚本)
    function Report.DetailGrid.Recordset.BeforePostRecordScript(Report, Sender)
    {
        Sender.Fields.Item("UnitsOnOrder").AsFloat = Sender.Fields.Item("UnitsInStock").AsFloat;
    }
    //Report.DetailGrid.Recordset.Discontinued.GetDisplayTextScript(字段.取显示文字脚本)
    function Report.DetailGrid.Recordset.Discontinued.GetDisplayTextScript(Report, Sender)
    {
        if (Sender.AsBoolean == true)
          Sender.DisplayText = "热卖中";
        else
          Sender.DisplayText = "停止销售";
    }
##### 3、追加空白行
    //Report.ProcessBeginScript(报表主对象.开始处理脚本)
    function Report.ProcessBeginScript(Report, Sender)
    {
        //假设每页要显示20 行，求出最后要补充的行数
        var AppendRows = 20 - (Report.DetailGrid.Recordset.RecordCount % 20);
        if (AppendRows == 20)
          AppendRows  = 0;
        for (i=0; i<AppendRows; ++i)
        {
          Report.DetailGrid.Recordset.Append();
          Report.DetailGrid.Recordset.Post();
        }
    }
##### 4、预览前处理
    //Report.ShowPreviewWndScript(报表主对象.预览前脚本)
    function Report.ShowPreviewWndScript(Report, Sender)
    {
        //在“预览前脚本”上将打印显示器的“EditMode”属性设置为“grpemClickToEdit(对应值为2)”，这样就会开启预览时可编辑文字功能
        Sender.EditMode=2;
    }
##### 5、增加一个计算字段，在记录集的“提交记录前脚本”中将计算字段的值设为对应字段的首字母。
    //Report.DetailGrid.Recordset.BeforePostRecordScript(记录集.提交记录前脚本)
    function Report.DetailGrid.Recordset.BeforePostRecordScript(Report, Sender)
    {
        Sender.Fields.Item("GroupFld").AsString = Sender.Fields.Item("客户编号").AsString.substring(0,1);
    }
##### 6、系统变量框显示文本自写
    //Report.DetailGrid.ColumnContent.FormatScript(内容行.格式化脚本)
    function Report.DetailGrid.ColumnContent.FormatScript(Report, Sender)
    {
            var FlagField = Report.FieldByName("FCSID");
            Sender.Visible =  (FlagField.AsInteger>0);
    }
    //Report.DetailGrid.ColumnContent.序号.SystemVarBox1.GetDisplayTextScript(系统变量框.取显示文字脚本)
    function Report.DetailGrid.ColumnContent.序号.SystemVarBox1.GetDisplayTextScript(Report, Sender)
    {
        var rowno = Sender.DisplayText;
        Sender.DisplayText = (parseInt(rowno)-1).toString();
    }​
##### 报表应用：页码
    第[#SystemVar(PageNumber)#]页/共[#SystemVar(PageCount)#]页​
#### 脚本程序：在记录集的 ProcessRecordScript 事件中根据条件产生分组
    //Report.DetailGrid.Recordset.ProcessRecordScript(记录集.处理记录脚本)
    function Report.DetailGrid.Recordset.ProcessRecordScript(Report, Sender)
    {
        var Amount = Report.FieldByName("Amount").AsFloat;
        var NewCatalog = FindCatalogByAmount(Amount);
        if (Report.ParameterByName("CurCatalogID").AsInteger != NewCatalog)
        {
            Report.DetailGrid.StartNewGroup(1);
        }
        function FindCatalogByAmount(Amount)
        {
            var Catalog;
            if (Amount < 5000)
                Catalog = 1;
            else if (Amount < 20000)
                Catalog = 2;
            else
                Catalog = 3;
            return Catalog;
        }
    }
    //Report.DetailGrid.Group1.GroupBeginScript(分组.分组开始脚本)
    function Report.DetailGrid.Group1.GroupBeginScript(Report, Sender)
    {
        var Amount = Report.FieldByName("Amount").AsFloat;
        var CurCatalogID = FindCatalogByAmount(Amount);
        var CurCatalogText;
        if (CurCatalogID == 1)
            CurCatalogText = "滞销";
        else if (CurCatalogID == 2)
            CurCatalogText = "一般";
        else
            CurCatalogText = "畅销";
        Report.Parameters.Item("CurCatalogID").AsInteger = CurCatalogID;
        Report.Parameters.Item("Catalog").AsString = CurCatalogText;
        function FindCatalogByAmount(Amount)
        {
            var Catalog;
            if (Amount < 5000)
                Catalog = 1;
            else if (Amount < 20000)
                Catalog = 2;
            else
                Catalog = 3;
            return Catalog;
        }
    }
    //Report.DetailGrid.Group1.GroupFooter.FormatScript(分组尾.格式化脚本)
    function Report.DetailGrid.Group1.GroupFooter.FormatScript(Report, Sender)
    {
        var GreenShapeBox = Report.ControlByName("GreenShapeBox");
        var YellowShapeBox = Report.ControlByName("YellowShapeBox");
        var RedShapeBox = Report.ControlByName("RedShapeBox");
        //运行时按条件确定部件框的隐藏与显示
        GreenShapeBox.Visible = false;
        YellowShapeBox.Visible = false;
        RedShapeBox.Visible = false;
        var Amount = Report.FieldByName("Amount").AsFloat;
        var CurCatalog = FindCatalogByAmount(Amount);
        if (CurCatalog == 1)
        {
            RedShapeBox.Visible = true;
            Report.ControlByName("PictureBox1").AsPictureBox.ImageIndex = 3;
        }
        else if (CurCatalog == 2)
        {
            YellowShapeBox.Visible = true;
            Report.ControlByName("PictureBox1").AsPictureBox.ImageIndex = 2;
        }
        else
        {
            GreenShapeBox.Visible = true;
            Report.ControlByName("PictureBox1").AsPictureBox.ImageIndex = 1;
        }
        function FindCatalogByAmount(Amount)
        {
            var Catalog;
            if (Amount < 5000)
                Catalog = 1;
            else if (Amount < 20000)
                Catalog = 2;
            else
                Catalog = 3;
            return Catalog;
        }
    }​
#### 脚本程序：医院体温记录单(脚本)
    //Report.ReportHeader2.sbTiWenDan.CustomDrawScript(静态框.自绘脚本)
    function Report.ReportHeader2.sbTiWenDan.CustomDrawScript(Report, Sender)
    {
        //由三原色值合成颜色整数值
        function ColorFromRGB(red, green, blue)
        {
            return red + green*256 + blue*256*256;
        }
        var Left = Graphics.Left;
        var Top = Graphics.Top;
        var Width = Graphics.Width;
        var Height = Graphics.Height;
        var Bottom = Top + Height;
        var Right = Left + Width;
        var Days = Report.ParameterByName("Days").AsInteger;     //天数
        var BeginDate = Report.ParameterByName("BeginDate").AsInteger; //开始日期
        var BeginHour = Report.ParameterByName("BeginHour").AsInteger; //开始小时
        var HourSpan = 4; //每次之间小时间隔
        var MaxTemp = 42; //最大温度
        var MinTemp = 34; //最小温度
        var TempScale = 5;   //1度的刻度数 
        var HourScale = 24 / HourSpan; //每天的小时刻度数
        var RowHeight = 20; //默认的行高度，底部文字行的高度
        var MeiBoColWidth = 40; //脉搏列的宽度
        var TempColWidth = 75;  //温度列的宽度
        var OutColWidth = 20;   //出量列的宽度
        var BottomRows = 11;   //下部行数，不包括呼吸次数行
        var TitleRowHeight = RowHeight + 10; //表头(脉搏、体温与小时文字)行高
        var FuxiRowHeight = RowHeight + 10;  //呼吸行的高度
        var GridCols = Days * (24 / HourSpan);            //网格列数
        var GridRows = (MaxTemp - MinTemp) * TempScale + 3; //网格行数，顶部多3行
        var TitleTop = Top + RowHeight;                    //标题(即日期行)的上部位置
        var FuxiBottom = Top + (Height - BottomRows*RowHeight); //呼吸行的底部位置
        var GridLeft = Left + MeiBoColWidth + TempColWidth; //网格左边位置
        var GridRight = Left + Width;                       //网格右边位置
        var GridTop = TitleTop + TitleRowHeight;         //网格顶边位置      
        var GridBottom = FuxiBottom - FuxiRowHeight;     //网格底边位置      
        var GridWidth = GridRight - GridLeft;            //网格的宽度    
        var GridHeight = GridBottom - GridTop;           //网格的高度        
        var GridRowHeight = GridHeight / GridRows;       //网格的行宽度    
        var GridColWidth = GridWidth / GridCols;         //网格的列宽度   
        var SquareSize = 8;  //数据点的正方形区域边长
        var TextFormat = Report.Utility.CreateTextFormat();
        var i;
        var j;
        var x, y, w, h; //位置输出的左，上，宽，高变量
        var x2; 
        var w2;
        var xPrior;
        var yPrior;
        var ColNo;
        var val;
        var OutText;
        var Recordset = Report.DetailGrid.Recordset;
        var riqi = Report.FieldByName("riqi");
        var times = Report.FieldByName("times");
        var tiwen = Report.FieldByName("tiwen");
        var maibo = Report.FieldByName("maibo");
        var fuxi = Report.FieldByName("fuxi");
        var beizhu = Report.FieldByName("beizhu");
        var tszl = Report.FieldByName("tszl");
        var dbcs = Report.FieldByName("dbcs");
        var cll = Report.FieldByName("cll");
        var ctl = Report.FieldByName("ctl");
        var cyll = Report.FieldByName("cyll");
        var cotl = Report.FieldByName("cotl");
        var czl = Report.FieldByName("czl");
        var rl = Report.FieldByName("rl");
        var xueya = Report.FieldByName("xueya");
        var tizhong = Report.FieldByName("tizhong");
        var ssts = Report.FieldByName("ssts");
        var BeginDateParam = Report.ParameterByName("BeginDate");
        var TempDateParam = Report.ParameterByName("TempDate");
        var TempHourParam = Report.ParameterByName("TempHour");
        var TempFloatParam = Report.ParameterByName("TempFloat");
        //{{画表格黑色线//////////////////////////////////////////////////
        Graphics.SelectPen(0.5, ColorFromRGB(0, 0, 0), 0); //grpsSolid
        //首先画表格头横线
        Graphics.MoveTo(Left, TitleTop);
        Graphics.LineTo(GridRight, TitleTop);
        Graphics.MoveTo(Left, GridTop);
        Graphics.LineTo(GridRight, GridTop);
        //<<画底部的行线
        //画呼吸次数下的行线
        y = GridBottom;
        Graphics.MoveTo(Left, y);
        Graphics.LineTo(GridRight, y);
        y = FuxiBottom;
        for (i=0; i<BottomRows; ++i)
        {
            x = Left;
            if (i>=3 && i<8)
              x += OutColWidth;
            Graphics.MoveTo(x, y);
            Graphics.LineTo(GridRight, y);
            y += RowHeight;
        }
        //画"出量"列的竖线,奇怪，这里输出了出异常
        x = Left + OutColWidth;
        Graphics.MoveTo(x, FuxiBottom + RowHeight*2);
        Graphics.LineTo(x, FuxiBottom + RowHeight*8);
        //>>画底部的行线
        //画前2列的竖线
        x = Left + MeiBoColWidth;
        Graphics.MoveTo(x, TitleTop);
        Graphics.LineTo(x, GridBottom);
        x += TempColWidth;
        Graphics.MoveTo(x, Top);
        Graphics.LineTo(x, Bottom);
        Graphics.RestorePen();
        //}}画表格黑色线//////////////////////////////////////////////////
        //{{画Grid的线段//////////////////////////////////////////////////
        Graphics.SelectPen(0.5, ColorFromRGB(0, 0, 0), 0); //grpsSolid
        //画细横线
        y = GridTop + GridRowHeight;
        for (i=1; i<GridRows; ++i)
        {
            if ((i+2)%TempScale != 0)
            {
                Graphics.MoveTo(GridLeft, y);
                Graphics.LineTo(GridRight, y);
            }
            y += GridRowHeight;
        }
        //画细竖线
        x = GridLeft + GridColWidth;
        for (i=1; i<GridCols; ++i)
        {
            if (i%HourScale != 0)
            {
                Graphics.MoveTo(x, TitleTop);
                Graphics.LineTo(x, FuxiBottom);
            }
            x += GridColWidth;
        }
        Graphics.RestorePen();
        //画粗横线
        Graphics.SelectPen(1.5, ColorFromRGB(0, 0, 0), 0);
        y = GridTop + GridRowHeight*3;
        for (i=3; i<GridRows; i+=TempScale)
        {
            Graphics.MoveTo(GridLeft, y);
            Graphics.LineTo(GridRight, y);
            y += GridRowHeight*TempScale;
        }
        Graphics.RestorePen();
        //画粗竖线
        Graphics.SelectPen(1.5, ColorFromRGB(255, 0, 0), 0);
        x = GridLeft + GridColWidth*HourScale;
        for (i=HourScale; i<GridCols; i+=HourScale)
        {
            Graphics.MoveTo(x, Top);
            Graphics.LineTo(x, Bottom);
            x += GridColWidth*HourScale;
        }
        Graphics.RestorePen();
        //}}画Grid的线段//////////////////////////////////////////////////
        //{{输出静态文字//////////////////////////////////////////////////
        //<<第一行,即日期行
        x = Left;
        y = Top;
        w = GridLeft;
        h = RowHeight;
        Graphics.DrawText("日  期", x, y, w, h, 34, false);
        TempDateParam.AsDateTime = BeginDateParam.AsDateTime;
        var year = "";
        var month = "";
        x = GridLeft;
        w = GridColWidth * HourScale;
        for (i=0; i<Days; ++i)
        {
            var DateText = TempDateParam.DisplayText;
        var cur_year = DateText.substr(0, 4);
            var cur_month = DateText.substr(4, 2);
            var cur_day = DateText.substr(6);
            DateText = "";
            if (year != cur_year)
            {
                year = cur_year;
                DateText = year + ".";
            }
            if (month != cur_month)
            {
                month = cur_month;
                DateText += month + ".";
            }
            DateText += cur_day;
            
        Graphics.DrawText(DateText, x, y, w, h, 34, false);
            x += w;
        TempDateParam.AsInteger += 1;
        }
        //>>第一行,即日期行
        //<<第二行,即脉搏、体温，小时行
        x = Left;
        y = TitleTop;
        w = MeiBoColWidth;
        h = TitleRowHeight;
        Graphics.DrawText("脉搏", x, y, w, h, 34, false);
        x = Left + MeiBoColWidth;
        w = TempColWidth;
        Graphics.DrawText("体  温", x, y, w, h, 34, false);
        //小时,这里需要改小字体
        var Font = Sender.Font; //.UsingOleFont;
        Font.Point = 7.5;  //Font.Size = 7.5; //6.75; //67500;  //67500比较合适
        Graphics.SelectFont( Font );
        w = GridColWidth;
        for (i=0; i<Days; ++i)
        {
        x = GridLeft + GridColWidth*i*HourScale;
        TempHourParam.AsInteger = BeginHour;
        for (j=0; j<HourScale; ++j)
        {
        OutText = TempHourParam.DisplayText;
        Graphics.DrawText(OutText, x, y, w, h, 34, false);
        x += GridColWidth;
        TempHourParam.AsInteger += HourSpan;
        }
        }
        Graphics.RestoreFont();
        //>>第二行,即脉搏、体温，小时行
        //<<脉搏文字列: 画出脉搏列的度量文字
        Font.Point = 9; //7.5; //75000;
        Graphics.SelectFont( Font );
        Graphics.SelectTextColor( ColorFromRGB(255, 0 ,0) );
        x = Left;
        w = MeiBoColWidth;
        h = GridRowHeight*2;
        y = GridTop + GridRowHeight*2;
        TempHourParam.AsInteger = 180;
        for (i=0; i<=MaxTemp-MinTemp; ++i)
        {
        var HourText = TempHourParam.DisplayText;
        Graphics.DrawText(HourText, x, y, w, h, 34, false);
        y += GridRowHeight*TempScale;
        if (i+1 == MaxTemp-MinTemp)
        y -= GridRowHeight;
        TempHourParam.AsInteger -= 20;
        }
        Graphics.RestoreTextColor();
        Graphics.RestoreFont();
        //>>脉搏文字列: 画出脉搏列的度量文字
        //<<体温文字列: 画出体温的度量文字
        x = Left + MeiBoColWidth;
        w = TempColWidth*3/5;
        h = GridRowHeight*2;
        y = GridTop;
        x2 = x + w; 
        w2 = TempColWidth - w;
        Graphics.DrawText("F", x, y, w, h, 34, false);
        Graphics.DrawText("C", x2, y, w2, h, 34, false);
        y += h;
        for (i=MaxTemp; i>=MinTemp; --i)
        {
        TempFloatParam.AsFloat = i*1.8 + 32;// ℃ × 1.8 + 32
        OutText = TempFloatParam.DisplayText;
        Graphics.DrawText(OutText, x, y, w, h, 34, false);
        TempFloatParam.AsFloat = i;
        OutText = TempFloatParam.DisplayText;
        Graphics.DrawText(OutText, x2, y, w2, h, 34, false);
        y += GridRowHeight*TempScale;
        if (i-1 == MinTemp)
        y -= GridRowHeight;
        }
        //>>体温文字列: 画出体温的度量文字
        //}}输出静态文字//////////////////////////////////////////////////
        //{{输出动态数据//////////////////
        //呼吸次数前面文字
        x = Left;
        y = GridBottom;
        w = MeiBoColWidth + TempColWidth;
        h = FuxiRowHeight;
        Graphics.DrawText("呼吸(次/分)", x, y, w, h, 34, false);
        //输出呼吸次数与备注数据
        Font.Point = 7.5; //67500;  //67500比较合适
        Graphics.SelectFont( Font );
        w = GridColWidth;
        h = FuxiRowHeight / 2;
        y = GridBottom;
        Recordset.First();
        while ( !Recordset.Eof() )
        {
        ColNo = (riqi.AsInteger - BeginDate)*HourScale + times.AsInteger - 1;
        x = GridLeft + GridColWidth*ColNo;
        //呼吸次数数据
        if (fuxi.AsInteger > 0)
        {
        TempHourParam.AsInteger = fuxi.AsInteger;
        y = GridBottom;
        if (ColNo%2 != 0)
        y += h;
        OutText = TempHourParam.DisplayText;
        Graphics.DrawText(OutText, x+1, y, w, h, 34, false);
        }
        //输出备注文字数据
        if ( !beizhu.IsNull ) //(OutText != "")
        {
        OutText = beizhu.AsString;
        y = GridTop + 3*GridRowHeight + 2;
        TextFormat.TextOrientation = 5; //grtoU2DL2R0 5 
        TextFormat.TextAlign = 17; //grtaTopLeft  17 
        Graphics.DrawFormatText(OutText, x+2, y, w, GridHeight, TextFormat);
        }
        Recordset.Next();
        }
        Graphics.RestoreFont();
        //<<输出脉搏图形
        Graphics.SelectPen(1, ColorFromRGB(255, 0, 0), 0);
        Graphics.SelectFillColor( ColorFromRGB(255, 0, 0) );
        xPrior = 0;
        yPrior = 0;
        Recordset.First();
        while ( !Recordset.Eof() )
        {
        val = maibo.AsFloat;
        if (val > 0)
        {
        ColNo = (riqi.AsInteger - BeginDate)*HourScale + times.AsInteger - 1;
        x = GridLeft + GridColWidth*ColNo + GridColWidth/2;
        y = GridTop + ((180.0 - val)*TempScale/20 + 3) * GridRowHeight;
        //连线
        if (xPrior > 0)
        {
        Graphics.MoveTo(xPrior, yPrior);
        Graphics.LineTo(x, y);
        }
        xPrior = x;
        yPrior = y;
        }
        Recordset.Next();
        }
        Recordset.First();
        while ( !Recordset.Eof() )
        {
        val = maibo.AsFloat;
        if (val > 0)
        {
        ColNo = (riqi.AsInteger - BeginDate)*HourScale + times.AsInteger - 1;
        x = GridLeft + GridColWidth*ColNo + GridColWidth/2;
        y = GridTop + ((180.0 - val)*TempScale/20 + 3) * GridRowHeight;
                Graphics.Ellipse(x-SquareSize/2, y-SquareSize/2, SquareSize, SquareSize, true);
        }
        Recordset.Next();
        }
        Graphics.RestoreFillColor();
        Graphics.RestorePen();
        //>>输出脉搏图形
        //<<输出体温图形
        Graphics.SelectPen(1, ColorFromRGB(0, 0, 0), 0);
        xPrior = 0;
        yPrior = 0;
        Recordset.First();
        while ( !Recordset.Eof() )
        {
        val = tiwen.AsFloat;
        if (val > 0)
        {
        ColNo = (riqi.AsInteger - BeginDate)*HourScale + times.AsInteger - 1;
        x = GridLeft + GridColWidth*ColNo + GridColWidth/2;
        y = GridTop + ((MaxTemp - val)*TempScale + 3) * GridRowHeight;
        //连线
        y += SquareSize/2;
        if (xPrior > 0)
        {
        Graphics.MoveTo(xPrior, yPrior);
        Graphics.LineTo(x, y);
        }
        xPrior = x + SquareSize;
        yPrior = y;
        }
        Recordset.Next();
        }
        Recordset.First();
        while ( !Recordset.Eof() )
        {
        val = tiwen.AsFloat;
        if (val > 0)
        {
        ColNo = (riqi.AsInteger - BeginDate)*HourScale + times.AsInteger - 1;
        x = GridLeft + GridColWidth*ColNo + GridColWidth/2;
        y = GridTop + ((MaxTemp - val)*TempScale + 3) * GridRowHeight;
                x -= SquareSize/2; 
                y -= SquareSize/2; 
                Graphics.FillRect(x-1, y-1, SquareSize+2, SquareSize+2, ColorFromRGB(255, 255, 255));
                Graphics.MoveTo(x, y);
                Graphics.LineTo(x+SquareSize, y+SquareSize);
                Graphics.MoveTo(x+SquareSize, y);
                Graphics.LineTo(x, y+SquareSize);
        }
        Recordset.Next();
        }
        Graphics.RestorePen();
        //>>输出体温图形
        //输出底部行的文字
        x = Left;
        y = FuxiBottom;
        w = MeiBoColWidth + TempColWidth;
        h = RowHeight;
        Graphics.DrawText("特 殊 治 疗", x, y + RowHeight*0, w, h, 34, false);
        Graphics.DrawText("大 便 次 数", x, y + RowHeight*1, w, h, 34, false);
        Graphics.DrawText("尿 量(毫升)", x+OutColWidth, y + RowHeight*2, w, h, 34, false);
        Graphics.DrawText("痰 量(毫升)", x+OutColWidth, y + RowHeight*3, w, h, 34, false);
        Graphics.DrawText("引流量(毫升)", x+OutColWidth, y + RowHeight*4, w, h, 34, false);
        Graphics.DrawText("呕吐量(毫升)", x+OutColWidth, y + RowHeight*5, w, h, 34, false);
        Graphics.DrawText("总 量(毫升)", x+OutColWidth, y + RowHeight*6, w, h, 34, false);
        Graphics.DrawText("入 量(毫升)", x, y + RowHeight*7, w, h, 34, false);
        Graphics.DrawText("血压(mmHg)", x, y + RowHeight*8, w, h, 34, false);
        Graphics.DrawText("体 重(Kg)", x, y + RowHeight*9, w, h, 34, false);
        Graphics.DrawText("手术后天数", x, y + RowHeight*10, w, h, 34, false);
        x = Left;
        y = FuxiBottom + RowHeight*2;
        w = OutColWidth;
        h = RowHeight*5;
        TextFormat.TextOrientation = 5; //grtoU2DL2R0 5 
        TextFormat.TextAlign = 17; //grtaTopLeft  17 
        Graphics.DrawFormatText("出    量", x+4, y+20, w, GridHeight, TextFormat);
        //输出体温图形
        Recordset.First();
        while ( !Recordset.Eof() )
        {
        //数据假设都是记录在每天的第一次数据上
        if (times.AsInteger == 1)
        {
        x = GridLeft + GridColWidth * (riqi.AsInteger - BeginDate)*HourScale;
        y = FuxiBottom;
        w = GridColWidth * HourScale;
        h = RowHeight;
        if ( !tszl.IsNull )
        Graphics.DrawText(tszl.AsString, x, y + RowHeight*0, w, h, 34, false);
        if ( !dbcs.IsNull )
        Graphics.DrawText(dbcs.AsString, x, y + RowHeight*1, w, h, 34, false);
        if ( !cll.IsNull )
        Graphics.DrawText(cll.AsString, x, y + RowHeight*2, w, h, 34, false);
        if ( !ctl.IsNull )
        Graphics.DrawText(ctl.AsString, x, y + RowHeight*3, w, h, 34, false);
        if ( !cyll.IsNull )
        Graphics.DrawText(cyll.AsString, x, y + RowHeight*4, w, h, 34, false);
        if ( !cotl.IsNull )
        Graphics.DrawText(cotl.AsString, x, y + RowHeight*5, w, h, 34, false);
        if ( !czl.IsNull )
        Graphics.DrawText(czl.AsString, x, y + RowHeight*6, w, h, 34, false);
        if ( !rl.IsNull )
        Graphics.DrawText(rl.AsString, x, y + RowHeight*7, w, h, 34, false);
        if ( !xueya.IsNull )
        Graphics.DrawText(xueya.AsString, x, y + RowHeight*8, w, h, 34, false);
        if ( !tizhong.IsNull )
        Graphics.DrawText(tizhong.AsString, x, y + RowHeight*9, w, h, 34, false);
        if ( !ssts.IsNull )
        Graphics.DrawText(ssts.AsString, x, y + RowHeight*10, w, h, 34, false);
        }
        Recordset.Next();
        }
        //}}输出动态数据//////////////////
    }​
#### 脚本程序：部件自绘
    function GetColorValue(r,g,b)
    {
       return r + g*256 + b*256*256;
    }
    var Graphics = Report.Graphics;
    var x = Graphics.Left;
    var y = Graphics.Top;
    var width = Graphics.Width;
    var height = Graphics.Height;
    var PartSize = height/3;
    var DrawLeft = x + (width - PartSize)/2;
    var DrawRight = DrawLeft + PartSize;
    var DrawXCenter = (DrawLeft + DrawRight)/2;
    var DrawTop = y;
    var DrawBottom = y + height;
    //设定绘出线型
    Graphics.SelectPen(2, GetColorValue(255, 0, 0), 0/*grpsSolid*/);
    //设定填充色
    Graphics.SelectFillColor( GetColorValue(0, 255, 255) );
    //画箭头两边斜线
    Graphics.MoveTo(DrawLeft, DrawTop+PartSize);
    Graphics.LineTo(DrawXCenter, DrawTop);
    Graphics.LineTo(DrawRight, DrawTop+PartSize);
    //画箭头竖线
    Graphics.MoveTo(DrawXCenter, DrawTop);
    Graphics.LineTo(DrawXCenter, DrawTop+PartSize*2);
    //画出圆圈
    Graphics.Ellipse(DrawLeft, DrawTop+PartSize*2, PartSize, PartSize, true);
    //恢复填充色设定，SelectFillColor调用之后，必须对应调用RestoreFillColor
    Graphics.RestoreFillColor();
    //恢复绘出线型设定，SelectPen调用之后，必须对应调用RestorePen
    Graphics.RestorePen();
    ​
    脚本程序：部件自绘
    //Report.ReportHeader1.sbScriptDraw.CustomDrawScript(静态框.自绘脚本)
    function Report.ReportHeader1.sbScriptDraw.CustomDrawScript(Report, Sender)
    {
        function GetColorValue(r,g,b)
        {
           return r + g*256 + b*256*256;
        }
        var Graphics = Report.Graphics;
        var x = Graphics.Left;
        var y = Graphics.Top;
        var width = Graphics.Width;
        var height = Graphics.Height;
        //设定绘出线型
        Graphics.SelectPen(2, GetColorValue(255, 0, 0), 0/*grpsSolid*/);
        //设定填充色
        Graphics.SelectFillColor( GetColorValue(0, 255, 255) );
        var cx = x + width/2;
        var cy = y + height/2;
        Graphics.Arc(cx, cy, height/2, 30, 270);
        //恢复填充色设定，SelectFillColor调用之后，必须对应调用RestoreFillColor
        Graphics.RestoreFillColor();
        //恢复绘出线型设定，SelectPen调用之后，必须对应调用RestorePen
        Graphics.RestorePen();
    }
    //Report.ReportHeader1.sbEventDraw.CustomDrawScript(静态框.自绘脚本)
    function Report.ReportHeader1.sbEventDraw.CustomDrawScript(Report, Sender)
    {
        function GetColorValue(r,g,b)
        {
           return r + g*256 + b*256*256;
        }
        var Graphics = Report.Graphics;
        var x = Graphics.Left;
        var y = Graphics.Top;
        var width = Graphics.Width;
        var height = Graphics.Height;
        var PartSize = height/3;
        var DrawLeft = x + (width - PartSize)/2;
        var DrawRight = DrawLeft + PartSize;
        var DrawXCenter = (DrawLeft + DrawRight)/2;
        var DrawTop = y;
        var DrawBottom = y + height;
        //设定绘出线型
        Graphics.SelectPen(2, GetColorValue(255, 0, 0), 0/*grpsSolid*/);
        //设定填充色
        Graphics.SelectFillColor( GetColorValue(0, 255, 255) );
        var cx = x + width/2;
        var cy = y + height/2;
        Graphics.Pie(cx, cy, height/2, 30, 270, true);
        //恢复填充色设定，SelectFillColor调用之后，必须对应调用RestoreFillColor
        Graphics.RestoreFillColor();
        //恢复绘出线型设定，SelectPen调用之后，必须对应调用RestorePen
        Graphics.RestorePen();
    }
    //Report.ReportHeader1.StaticBox5.CustomDrawScript(静态框.自绘脚本)
    function Report.ReportHeader1.StaticBox5.CustomDrawScript(Report, Sender)
    {
        function GetColorValue(r,g,b)
        {
           return r + g*256 + b*256*256;
        }
        var Graphics = Report.Graphics;
        var x = Graphics.Left;
        var y = Graphics.Top;
        var width = Graphics.Width;
        var height = Graphics.Height;
        var PartSize = height/3;
        var DrawLeft = x + (width - PartSize)/2;
        var DrawRight = DrawLeft + PartSize;
        var DrawXCenter = (DrawLeft + DrawRight)/2;
        var DrawTop = y;
        var DrawBottom = y + height;
        //设定绘出线型
        Graphics.SelectPen(2, GetColorValue(255, 0, 0), 0/*grpsSolid*/);
        //设定填充色
        Graphics.SelectFillColor( GetColorValue(0, 255, 255) );
        var cx = x + width/2;
        var cy = y + height/2;
        Graphics.Pie(cx, cy, height/2, 30, 270, false);
        //恢复填充色设定，SelectFillColor调用之后，必须对应调用RestoreFillColor
        Graphics.RestoreFillColor();
        //恢复绘出线型设定，SelectPen调用之后，必须对应调用RestorePen
        Graphics.RestorePen();
    }
    //Report.ReportHeader1.StaticBox6.CustomDrawScript(静态框.自绘脚本)
    function Report.ReportHeader1.StaticBox6.CustomDrawScript(Report, Sender)
    {
        function GetColorValue(r,g,b)
        {
           return r + g*256 + b*256*256;
        }
        var Graphics = Report.Graphics;
        var x = Graphics.Left;
        var y = Graphics.Top;
        var width = Graphics.Width;
        var height = Graphics.Height;
        //设定绘出线型
        Graphics.SelectPen(2, GetColorValue(255, 0, 0), 0/*grpsSolid*/);
        //设定填充色
        Graphics.SelectFillColor( GetColorValue(0, 255, 255) );
        Graphics.EllipseArc(x, y, width, height, 30, 270);
        //恢复填充色设定，SelectFillColor调用之后，必须对应调用RestoreFillColor
        Graphics.RestoreFillColor();
        //恢复绘出线型设定，SelectPen调用之后，必须对应调用RestorePen
        Graphics.RestorePen();
    }
    //Report.ReportHeader1.StaticBox7.CustomDrawScript(静态框.自绘脚本)
    function Report.ReportHeader1.StaticBox7.CustomDrawScript(Report, Sender)
    {
        function GetColorValue(r,g,b)
        {
           return r + g*256 + b*256*256;
        }
        var Graphics = Report.Graphics;
        var x = Graphics.Left;
        var y = Graphics.Top;
        var width = Graphics.Width;
        var height = Graphics.Height;
        //设定绘出线型
        Graphics.SelectPen(2, GetColorValue(255, 0, 0), 0/*grpsSolid*/);
        //设定填充色
        Graphics.SelectFillColor( GetColorValue(0, 255, 255) );
        Graphics.EllipsePie(x, y, width, height, 30, 270, false);
        //恢复填充色设定，SelectFillColor调用之后，必须对应调用RestoreFillColor
        Graphics.RestoreFillColor();
        //恢复绘出线型设定，SelectPen调用之后，必须对应调用RestorePen
        Graphics.RestorePen();
    }
    //Report.ReportHeader1.StaticBox8.CustomDrawScript(静态框.自绘脚本)
    function Report.ReportHeader1.StaticBox8.CustomDrawScript(Report, Sender)
    {
        function GetColorValue(r,g,b)
        {
           return r + g*256 + b*256*256;
        }
        var Graphics = Report.Graphics;
        var x = Graphics.Left;
        var y = Graphics.Top;
        var width = Graphics.Width;
        var height = Graphics.Height;
        //设定绘出线型
        Graphics.SelectPen(2, GetColorValue(255, 0, 0), 0/*grpsSolid*/);
        //设定填充色
        Graphics.SelectFillColor( GetColorValue(0, 255, 255) );
        Graphics.EllipsePie(x, y, width, height, 30, 270, true);
        //恢复填充色设定，SelectFillColor调用之后，必须对应调用RestoreFillColor
        Graphics.RestoreFillColor();
        //恢复绘出线型设定，SelectPen调用之后，必须对应调用RestorePen
        Graphics.RestorePen();
    }​
#### 脚本程序：图像自绘
    //Report.DetailGrid.ColumnContent.Picture.StaticBox2.CustomDrawScript(静态框.自绘脚本)
    function Report.DetailGrid.ColumnContent.Picture.StaticBox2.CustomDrawScript(Report, Sender)
    {
        var pic = Report.Utility.CreatePicture(); //创建图像对象
        var Binary = Report.Utility.CreateBinaryObject();
        //用当前记录号模拟出一些条件参数
        var RecordNo = Report.SystemVarValue(4);  //GRSystemVarType.grsvRecordNo 当前记录号
        var ImageCount = (RecordNo-1)%3 + 1; //图像幅数
        var x = Graphics.Left;
        var y = Graphics.Top;
        var width = (Graphics.Width - ImageCount*8 + 8) / ImageCount;
        var height = Graphics.Height;
        Binary.LoadFromField( Report.FieldByName("Picture") ); //从字段中载入图像
        pic.LoadFromBinary(Binary); //载入图像
        Report.Graphics.DrawPicture(pic, x, y, width, height, 3, 2, 1);
        if (ImageCount >= 2)
        { 
          pic.LoadFromFile("C:\\Grid++Report 5.0\\Samples\Data\\Picture\\" + Report.FieldByName("PictureFile").AsString); //从文件中载入图像，PictureFile字段中存储图像文件的名称 
          x += width + 8;
          Report.Graphics.DrawPicture(pic, x, y, width, height, 3, 2, 1);
        }
        if (ImageCount == 3)
        { 
          //绘制报表图像集合中的图像，根据当前记录号确定图像序号
          var ImageIndex = Report.SystemVarValue(4)%5 + 1;  //GRSystemVarType.grsvRecordNo
          pic = Report.ImageList.Item(ImageIndex);
          x += width + 8;
          Report.Graphics.DrawPicture(pic, x, y, width, height, 3, 2, 1);
        }
    }​
#### 脚本程序：自绘突出行线
    1、自由格
    2、设置部件框的“自绘”属性
    3、在部件框上写“自绘脚本”
    //Report.DetailGrid.ColumnContent.UnitPrice.FieldBox1.CustomDrawScript(字段框.自绘脚本)
    function Report.DetailGrid.ColumnContent.UnitPrice.FieldBox1.CustomDrawScript(Report, Sender)
    {
        Sender.DrawDefault();
        var Amt = Report.FieldByName("Amount").AsFloat;
        if (Amt > 3000)
        {
          var x1 = Graphics.Left;
          var x2 = x1 + Graphics.Width;
          var y = Graphics.Top + Graphics.Height - 1;
          //设定绘出线型
          Graphics.SelectPen(2, GetColorValue(0, 0, 0), 0); //0=grpsSolid
          Graphics.MoveTo(x1,  y);
          Graphics.LineTo(x2,  y);
          //恢复绘出线型设定，SelectPen调用之后，必须对应调用RestorePen
          Graphics.RestorePen();
        }
        function GetColorValue(r,g,b)
        {
           return r + g*256 + b*256*256;
        }
    }
    //Report.DetailGrid.ColumnContent.DisCountAmt.FieldBox2.CustomDrawScript(字段框.自绘脚本)
    function Report.DetailGrid.ColumnContent.DisCountAmt.FieldBox2.CustomDrawScript(Report, Sender)
    {
        Sender.DrawDefault();
        var Amt = Report.FieldByName("Amount").AsFloat;
        if (Amt > 3000)
        {
          var x1 = Graphics.Left;
          var x2 = x1 + Graphics.Width;
          var y = Graphics.Top + Graphics.Height - 1;
          //设定绘出线型
          Graphics.SelectPen(2, GetColorValue(255, 0, 0), 0/*grpsSolid*/);
          Graphics.MoveTo(x1,  y);
          Graphics.LineTo(x2,  y);
          //恢复绘出线型设定，SelectPen调用之后，必须对应调用RestorePen
          Graphics.RestorePen();
        }
        function GetColorValue(r,g,b)
        {
           return r + g*256 + b*256*256;
        }
    }
​
*** 
![颜色对照表](https://github.com/kaibosoft/kaibosoft/blob/master/工作笔记/Pictures/颜色对照表.png)​
