# React에서 엑셀 파일로 변환 및 다운받기
    
<br />    
    
React에서 데이터를 엑셀 파일로 변환해서 다운로드를 하기 위해서 여러가지 방법이 있지만

아래에 두 npm을 설치하자

XLSX는 복잡한 스프레드시트에서 유용한 데이터를 추출하고 새 스프레드시트를 생성하기 위해 검증된 오픈 소스이다.

FileSaver는 크기 제한보다 더 큰 파일을 저장해야 하거나 RAM이 충분하지 않은 경우 새로운 스트림의 힘으로 비동기식으로 직접 데이터를 저장할 수 있다.
    
<br />
    
```

const excel_down = (param) => {
    const excelFileType = 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet;charset=UTF-8'; // 엑셀 파일의 타입
    const excelFileExtension = '.xlsx'; // 엑셀 파일 확장자
    const excelFileName = `엑셀 데이터 변환`; // 엑셀 파일 파일명
    
    // 엑셀 내부 테이블 제목
    const ws = XLSX.utils.aoa_to_sheet([
        [excelFileName],
        [],
        [
            "날짜",
            "키워드",
            "작성자",
            "등록날짜",
            "사이트",
            "제목"
        ]
    ]);
    
    // 엑셀 내부 테이블 내용
    param.dataList.map((data) => {
        XLSX.utils.sheet_add_aoa(ws, [
            [
                data.date,
                data.filter,
                data.from,
                data.registDate,
                data.taskId,
                data.title,
            ]
        ], {origin: -1});
        
        // 테이블 사이즈
        ws['!cols'] = [
            {
                wpx: 200
            },
            {
                wpx: 200
            },
            {
                wpx: 200
            },
            {
                wpx: 200
            }, {
                wpx: 200
            }, {
                wpx: 200
            }, {
                wpx: 200
            }, {
                wpx: 200
            }, {
                wpx: 200
            }, {
                wpx: 200
            }, {
                wpx: 200
            }, {
                wpx: 200
            }, {
                wpx: 200
            }, {
                wpx: 200
            }, {
                wpx: 200
            }, {
                wpx: 200
            }
        ]
        return false;
    });
    const wb = {
        Sheets: {
            data: ws
        },
        SheetNames: ['data']
    };
    const excelButter = XLSX.write(wb, {
        bookType: 'xlsx',
        type: 'array'
    });
    const excelFile = new Blob([excelButter], {type: excelFileType});
    FileSaver.saveAs(excelFile, excelFileName + excelFileExtension);
    set_loading(false);
}

```
    
<br />
