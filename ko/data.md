# 데이터

## 데이터 유효성 검사 추가 {#AddDataValidation}

```go
func (f *File) AddDataValidation(sheet string, dv *DataValidation)
```

AddDataValidation는 주어진 데이터 유효성 검사 개체 및 워크 시트 이름으로 워크 시트 범위에 대한 집합 데이터 유효성 검사를 제공합니다. 데이터 유효성 검사 개체는 `NewDataValidation` 함수로 만들 수 있습니다. 데이터 유효성 검사 유형 및 연산자는 [상수](constants.md) 섹션에서 찾을 수 있습니다.

예제 1, 유효성 검사 조건 설정으로 `Sheet1!A1:B2` 에 대한 데이터 유효성 검사를 설정하고, 잘못된 데이터가 "Stop" 스타일 및 사용자 지정 제목 "error body" 로 입력된 후 오류 경고를 표시합니다:

<p align="center"><img width="654" src="./images/data_validation_01.png" alt="Data validation"></p>

```go
dv := excelize.NewDataValidation(true)
dv.SetSqref("A1:B2")
dv.SetRange(10, 20, excelize.DataValidationTypeWhole, excelize.DataValidationOperatorBetween)
dv.SetError(excelize.DataValidationErrorStyleStop, "error title", "error body")
f.AddDataValidation("Sheet1", dv)
```

예제 2, 에서는 유효성 검사 조건 설정을 사용하여 `Sheet1!A3:B4` 에 대한 데이터 유효성 검사를 설정하고 셀을 선택할 때 입력 메시지를 표시합니다:

<p align="center"><img width="654" src="./images/data_validation_02.png" alt="Data validation"></p>

```go
dv = excelize.NewDataValidation(true)
dv.SetSqref("A3:B4")
dv.SetRange(10, 20, excelize.DataValidationTypeWhole, excelize.DataValidationOperatorGreaterThan)
dv.SetInput("input title", "input body")
f.AddDataValidation("Sheet1", dv)
```

예제 3, 유효성 검사 기준 설정을 사용 하 여 `Sheet1!A5:B6` 에 데이터 유효성 검사를 설정, 목록 소스를 허용 하 여 셀 내 드롭다운을 만듭니다:

<p align="center"><img width="654" src="./images/data_validation_03.png" alt="Data validation"></p>

```go
dv = excelize.NewDataValidation(true)
dv.SetSqref("A5:B6")
dv.SetDropList([]string{"1", "2", "3"})
f.AddDataValidation("Sheet1", dv)
```

데이터 유효성 검사 대화 상자 (구분된 목록) 에 항목을 입력하는 경우 구분 기호를 포함하여 255 자로 제한됩니다. 데이터 유효성 검사 목록 소스 수식이 최대 길이 제한을 초과하는 경우 워크시트 셀에 허용되는 값을 설정하고 `SetSqrefDropList` 함수를 사용하여 해당 셀에 대한 참조를 설정하세요.

예제 4, 유효성 검사 기준 소스 `Sheet1!E1:E3` 설정으로 `Sheet1!A7:B8` 에 대한 데이터 유효성 검사를 설정하고 목록 소스를 허용하여 셀 내 드롭다운을 만듭니다:

<p align="center"><img width="654" src="./images/data_validation_04.png" alt="Data validation"></p>

```go
dv := excelize.NewDataValidation(true)
dv.SetSqref("A7:B8")
dv.SetSqrefDropList("E1:E3")
f.AddDataValidation("Sheet1", dv)
```

데이터 유효성 검사 드롭다운 목록에 표시되는 항목 수에는 제한이 있습니다. 목록은 워크시트 목록의 최대 32768 개 항목을 표시할 수 있습니다. 그보다 더 많은 항목이 필요한 경우 카테고리별로 분류된 종속 드롭다운 목록을 만들 수 있습니다.

## 데이터 유효성 검사 받기 {#GetDataValidations}

```go
func (f *File) GetDataValidations(sheet string) ([]*DataValidation, error)
```

GetDataValidations 는 지정된 워크시트 이름별로 데이터 유효성 검사 목록을 반환합니다.

## 데이터 유효성 검사 삭제 {#DeleteDataValidation}

```go
func (f *File) DeleteDataValidation(sheet string, sqref ...string) error
```

DeleteDataValidation 은 주어진 워크시트 이름과 참조 순서로 데이터 유효성 검사를 삭제합니다. 참조 시퀀스 매개변수를 지정하지 않으면 워크시트의 모든 데이터 유효성 검사가 삭제됩니다.

## 슬라이서 추가 {#AddSlicer}

`SlicerOptions` 는 슬라이서의 설정을 나타냅니다.

```go
type SlicerOptions struct {
    Name          string
    Table         string
    Cell          string
    Caption       string
    Macro         string
    Width         uint
    Height        uint
    DisplayHeader *bool
    ItemDesc      bool
    Format        GraphicOptions
}
```

`Name` 은 슬라이서 이름을 지정하며 해당 테이블 또는 피벗 테이블의 기존 필드 이름이어야 하며 이 설정이 필요합니다.

`Table` 은 테이블 또는 피벗 테이블의 이름을 지정하며 이 설정은 필수입니다.

`Cell` 은 왼쪽 상단 셀 좌표를 슬라이서 삽입 위치를 지정하므로 이 설정이 필요합니다.

`Caption` 은 슬라이서의 캡션을 지정하며 이 설정은 선택 사항입니다.

`Macro` 은 슬라이서의 매크로를 지정하며, 통합 문서 확장은 XLSM 또는 XLTM 이어야 합니다.

`Width` 는 슬라이서의 너비를 지정하며 이 설정은 선택 사항입니다.

`Height` 는 슬라이서의 높이를 지정하며 이 설정은 선택 사항입니다.

`DisplayHeader` 는 슬라이서의 헤더 표시 여부를 지정합니다. 이 설정은 선택 사항이며 기본 설정은 표시입니다.

`ItemDesc` 는 내림차순 (Z-A) 항목 정렬을 지정합니다. 이 설정은 선택 사항이며 기본 설정은 `false` (오름차순을 나타냄) 입니다.

`Format` 은 슬라이서의 형식을 지정하며 이 설정은 선택 사항입니다.

```go
func (f *File) AddSlicer(sheet string, opts *SlicerOptions) error
```

AddSlicer 함수는 워크시트 이름과 슬라이서 설정을 제공하여 슬라이서를 삽입합니다. 예를 들어 `Table1` 이라는 테이블에 대해 `Column1` 필드가 있는 `Sheet1!E1` 에 슬라이서를 삽입합니다.

```go
err := f.AddSlicer("Sheet1", &excelize.SlicerOptions{
    Name:       "Column1",
    Cell:       "E1",
    TableSheet: "Sheet1",
    TableName:  "Table1",
    Caption:    "Column1",
    Width:      200,
    Height:     200,
})
```

## 슬라이서를 받으세요 {#GetSlicers}

```go
func (f *File) GetSlicers(sheet string) ([]SlicerOptions, error)
```

GetSlicers 는 주어진 워크시트 이름으로 워크시트의 모든 슬라이서를 가져오는 방법을 제공합니다. 이 함수는 현재 슬라이서 모양의 높이, 너비 및 그래픽 옵션을 가져오는 것을 지원하지 않는다는 점에 유의하세요.

## 슬라이서 삭제 {#DeleteSlicer}

```go
func (f *File) DeleteSlicer(name string) error
```

DeleteSlicer 는 주어진 슬라이서 이름으로 슬라이서를 삭제하는 메서드를 제공합니다.
