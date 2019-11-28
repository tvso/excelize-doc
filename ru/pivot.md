# Сводная таблица {#PivotTable}

Сводная таблица - это таблица статистики, которая суммирует данные более обширной таблицы (например, из базы данных, электронной таблицы или программы бизнес-аналитики). Эта сводка может включать суммы, средние значения или другие статистические данные, которые сводная таблица группирует значимым образом.

## Создать сводную таблицу {#AddPivotTable}

```go
func (f *File) AddPivotTable(opt *PivotTableOption) error
```

AddPivotTable предоставляет метод для добавления сводной таблицы с помощью заданных опций сводной таблицы. Например, создайте сводную таблицу в области `Sheet1!$G$2:$M$34` с регионом `Sheet1!$A$1:$E$31` в качестве источника данных, суммируйте по сумме продаж:

<p align="center"><img width="1148" src="./images/pivot_table_01.png" alt="создать сводную таблицу с Excelize с помощью Go"></p>

```go
package main

import (
    "fmt"
    "math/rand"

    "github.com/360EntSecGroup-Skylar/excelize"
)

func main() {
    f := excelize.NewFile()
    // Создать некоторые данные на листе
    month := []string{"Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"}
    year := []int{2017, 2018, 2019}
    types := []string{"Meat", "Dairy", "Beverages", "Produce"}
    region := []string{"East", "West", "North", "South"}
    f.SetSheetRow("Sheet1", "A1", &[]string{"Month", "Year", "Type", "Sales", "Region"})
    for i := 0; i < 30; i++ {
        f.SetCellValue("Sheet1", fmt.Sprintf("A%d", i+2), month[rand.Intn(12)])
        f.SetCellValue("Sheet1", fmt.Sprintf("B%d", i+2), year[rand.Intn(3)])
        f.SetCellValue("Sheet1", fmt.Sprintf("C%d", i+2), types[rand.Intn(4)])
        f.SetCellValue("Sheet1", fmt.Sprintf("D%d", i+2), rand.Intn(5000))
        f.SetCellValue("Sheet1", fmt.Sprintf("E%d", i+2), region[rand.Intn(4)])
    }
    err := f.AddPivotTable(&excelize.PivotTableOption{
        DataRange:       "Sheet1!$A$1:$E$31",
        PivotTableRange: "Sheet1!$G$2:$M$34",
        Rows:            []string{"Month", "Year"},
        Columns:         []string{"Type"},
        Data:            []string{"Sales"},
    })
    if err != nil {
        fmt.Println(err)
    }
    err = f.SaveAs("Book1.xlsx")
    if err != nil {
        fmt.Println(err)
    }
}
```