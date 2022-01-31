
---
title: C#语言读写Excel文件
description: C#语言读写Excel文件
categories: [运维]
layout: main
---

## {{ page. title }}
{{ page.date | date_to_string }}

创建一个.net C# Console应用程序， 添加NuGet包`EPPlus`.  
![NuGet安装包EPPlus]({{ site.baseurl }}/assets/media/eppplus.PNG)

配置EPPlus的Liscense Context。
如果是商用应用，声明以下代码：
```csharp
// If you are a commercial business and have
// purchased commercial licenses use the static property
// LicenseContext of the ExcelPackage class :
ExcelPackage.LicenseContext = LicenseContext.Commercial;
```

非商业应用， 声明以下代码：
```csharp
// If you use EPPlus in a noncommercial context
// according to the Polyform Noncommercial license:
ExcelPackage.LicenseContext = LicenseContext.NonCommercial;
```

示例代码中使用的`PersonModel`类的声明：
```csharp
    class PeronModel
    {
        public int Id { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
    }
```

示例代码中返回样例数据的代码:
```csharp
    static List<PeronModel> GetSameData()
    {
        List<PeronModel> data = new List<PeronModel>()
        {
            new PeronModel() { Id = 1, FirstName = "Qijie", LastName = "Xue" },
            new PeronModel() { Id = 2, FirstName = "Cooper", LastName = "Xue" },
            new PeronModel() { Id = 3, FirstName = "Tim", LastName = "Smith" }
        };

        return data;
    }
```

示例代码中写入Excel的代码:
```csharp
     private static async Task SaveExcelFile(List<PeronModel> people, FileInfo file)
      {
          using(var package = new ExcelPackage(file))
          {
              var worksheet = package.Workbook.Worksheets["MainReport"];
              if(worksheet == null)
                  worksheet = package.Workbook.Worksheets.Add("MainReport");

              //内容填充
              var range = worksheet.Cells["A2"].LoadFromCollection<PeronModel>(people,true);
              //range.AutoFitColumns();

              //格式设置
              worksheet.Column(1).Style.HorizontalAlignment = OfficeOpenXml.Style.ExcelHorizontalAlignment.Center; //第一列内容居中对齐
              worksheet.Row(2).Style.Font.Bold = true; //第一行内容字体加粗

              //设置表头
              worksheet.Cells["A1"].Value = "The cool report";
              worksheet.Cells["A1:C1"].Merge = true; //合并单元格
              worksheet.Cells["A1"].Style.Font.Size = 24; //设置字体Size
              worksheet.Cells["A1"].Style.HorizontalAlignment = OfficeOpenXml.Style.ExcelHorizontalAlignment.Left;
              worksheet.Cells["A1"].Style.Font.Color.SetColor(Color.Blue); //设置字体颜色

              await package.SaveAsync();
          }
      }
```

写入Excel的内容展示:  
![Excel内容]({{ site.baseurl }}/assets/media/excel-data.PNG)

示例代码中读取Excel内容的代码:
```csharp
     private static async Task<List<PeronModel>> LoadExcelFile(FileInfo file)
      {
          List<PeronModel> output = new List<PeronModel>();
          using(var package = new ExcelPackage(file))
          {
              await package.LoadAsync(file); //从ExcelPackage加载数据

              var worksheet = package.Workbook.Worksheets["MainReport"];
              if (worksheet == null)
                  return null;

              int row = 3; //真实数据从第3行开始
              int col = 1;
              while(string.IsNullOrWhiteSpace(worksheet.Cells[row, col].Value?.ToString()) == false) //如果Id列有数据
              {
                  PeronModel person = new PeronModel();
                  person.Id = int.Parse(worksheet.Cells[row, col].Value.ToString()); //当前行第一列
                  person.FirstName = worksheet.Cells[row, col + 1].Value.ToString(); //当前行第二列
                  person.LastName = worksheet.Cells[row, col + 2].Value.ToString();  //当前行第三列
                  output.Add(person);
                  row += 1; //移动到下一行
              }
          }

          return output;
      }
```

示例代码中客户类的调用：
```csharp
   static async Task Main(string[] args)
    {
        ExcelPackage.LicenseContext = LicenseContext.NonCommercial;

        var file = new FileInfo(@"d:\code\autocreate.xlsx");
        var people = GetSameData();

        await SaveExcelFile(people, file);


        Console.WriteLine("Reading from Excel.");

        var peopleFromExcel = await LoadExcelFile(file);
        foreach (var p in peopleFromExcel)
        {
            Console.WriteLine($"{p.Id}, {p.FirstName}, {p.LastName}");
        }
        Console.WriteLine("complete.");
        Console.ReadLine();
    }
```
