---
layout: post
title:  "커스텀 문법 전체 변경"
categories: document
---

우리는 한의학고전DB를 구축할 때 고문헌 콘텐츠에 특화된 문법을 사용합니다. 이번에 확장성과 유연성을 확보하기 위하여 기존 문법을 정비하고 새로운 요소를 추가했습니다. 그 대략을 소개합니다. 



***

## 데이터 기본 형태

데이터의 항목은 문단 단위로 작성하고, 각 항목은 빈줄로 구분합니다.

- 항목의 데이터는 줄마다 키(key)-값(value) 형태로 구성하며 탭문자로 구분합니다.
- 모든 항목은 ID와 META를 필수로 포함해야 하고, META는 TAG를 필수로 포함해야 합니다.
- META에서 키는 대문자, 값은 소문자를 권장합니다.
- 메모나 작업자의 의견은 `#` 기호 뒤에 넣을 수 있고, DB에는 반영되지 않습니다.
- 콘텐츠에 들어가는 인라인 태그는 모두 `ㄴ...｝｝` 기호로 작성합니다.

```
ID	YnabT7yEpP8nkQ
META	{ TAG: h2, CLASS: [intro, outline-none] }
OR/HZ	東醫寶鑑序
TR/KO	동의보감 서문 #메모나 의견은 샾 기호 사용
TR/EN	Preface

ID	_dWliTEcBQKptw
META	{ TAG: p }
OR/HZ	醫者雅言軒岐, 軒岐上窮天紀, 下極人理...
TR/KO	의사들은 항상 헌원(軒轅)과 기백(岐伯)을 말합니다. 헌원과 기백...
TR/EN	Those of the medical profession often mention Huangdi a...
```

***

## 콘텐츠

### 1. 텍스트

#### ① 형태 표시

텍스트의 형태를 나타내는 태그는 `｛｛sm/...｝｝`처럼 소문자 2글자를 사용하며, `+`를 사용하여 조합할 수 있습니다.

- 작은 글자 (small) : ｛｛sm/當歸｝｝ → `<span class="size-sm">當歸</span> `
- 큰 글자 (large) : ｛｛lg/當歸｝｝ → `<span class="size-lg">當歸</span>`
- 양각 (positive / relief) : ｛｛ps/當歸｝｝ → `<span class="print-ps">當歸</span>`
- 음각 (negative / intaglio) : ｛｛ng/當歸｝｝ → `<span class="print-ng">當歸</span>`
- 작은 글자면서 양각 : ｛｛sm+ps/當歸｝｝ → `<span class="size-sm print-ps">當歸</span>`
- 작은 글자면서 저본에 포함된 주석 (annotation) : ｛｛an+sm/李曰...｝｝ → `<span class="size-sm annotation">李曰...</span>`

#### ② 교감기 및 역자주

역자주는 `｛｛주석 대상 : 설명 ※출전｝｝`처럼 자유롭게 작성하고, 교감기는 `｛｛대상 = 교감글자1 ※출전 ☆ *교감글자2 ※출전｝｝`처럼 작성합니다.

- 기존에는 주석 대상 텍스트가 주석 기호 안에 있었으나, 앞으로는 주석 기호 밖에도 반드시 적어야 합니다.
- 교감기에서 올바른 글자라고 판단하면 `*` 기호를 붙여서 표시할 수 있습니다.
- 주석 대상은 가능한 단어 단위 이상으로 묶어서 작성하도록 권장합니다.
- 주석 대상과 출전은 가능한 명시하면 좋겠지만 여의치 않을 때는 생략할 수 있습니다.
- 주석 내용에 구분(줄바꿈)이 필요하면 `☆`을 사용합니다.

```
ID	YSx5hSdbFw3_kw
META	{ TAG: p, CLASS: indent1 }
OR/HZ	孫眞人曰, 天地之內, 以人爲貴｛｛以人爲貴: 《주해상한론》에는 以天爲貴로 되어 있다.\n번역은 원본을 따랐다.｝｝, 頭圓象天, 足方象地, 天有四時, 人有四肢, ....
TR/KO	손진인이, "천지에서 존재하는 것 가운데 사람이 가장 귀중하다. 둥근 머리는 하늘을 닮았고 네모난 발은 땅을 닮았다.｛｛이 부분은 《주역》을 인용한 것이다.｝｝ 하늘에 사시가 있듯이...
TR/EN	There is a saying by <i>Sun Zhenren (孫眞人)</i>, "Mankind is the most precious of all living things in the universe...
```

#### ③ 기타
- 이미지 글자(최소한으로 사용) : ｛｛chr/A001_0001.png｝｝
- 파자, 신출자 : ｛｛chr/广+付｝｝, ｛｛chr/★｝｝
- 영문에서 이탤릭 : `<i>...</i>`
- 문단 내 줄바꿈
	* `☆` 기호를 사용하거나(1줄에 작성)
	* 아래 예시와 같이 `|-` 사용 후 자유롭게 엔터를 사용할 수 있습니다.(여러 줄에 작성)

```
ID	igeU4d-vz0oTpg
META	{ TAG: p, CLASS: poem }
OR/HZ	|-
  養子須調護
  看承莫縱馳
  乳多終損胃
  食壅卽傷脾
TR/KO	아이는 모름지기 잘 돌보며 길러야 하니☆잘 지켜보아 함부로 돌아다니지 못하게 해야 하네☆젖을 많이 먹이면 끝내 위장이 상하며☆밥에 체하면 비장을 상하네
```

### 2. 삽화

삽화는 이미지와 텍스트를 각각의 항목으로 입력합니다.

- 이미지 항목은 META에 파일명(SRC)을 필수로 입력해야 합니다.
- 삽화에 포함된 단어는 이미지 항목에 입력하여 이미지 대체 텍스트(ALT)로 활용하며, 생략해도 무방합니다.
- 삽화의 제목이나 포함된 문장은 아래처럼 별도의 텍스트 항목으로 입력합니다.

```
ID	_itdjcvqEScw4w
META	{TAG: img, CLASS: pic, SRC: 신형장부도.png, ALT: True }
OR/HZ	泥丸宮, 髓海腦, 玉枕關, 轆轤關, 尾閭關...
TR/KO	니환궁, 수해뇌, 옥침관, 녹로관, 미려관...
TR/EN	blabla..

ID	uAmMgyPhsm3_Kw
META	{ TAG: p, CLASS: pic-title }
OR/HZ	身形藏府圖
TR/KO	신형장부도
TR/EN	Picture of the overall body, viscera, and bowels

ID	VYeU4d-vz0oTpg
META	{ TAG: p, CLASS: pic-p }
OR/HZ	孫眞人曰, 天地之內, 以人爲貴...
TR/KO	손진인이, "천지에서 존재하는 것 가운데 사람이 가장 귀중하다...
TR/EN	There is a saying by Sun Zhenren (孫眞人)...
```

### 3. 표

표는 [GitHub Flavored Markdown(GFM)](https://github.github.com/gfm/#tables-extension-) 문법을 사용하고, 복잡한 표는 html을 사용합니다.

```
ID	_dWliTEcBQKptw
META	{ TAG: table, CLASS: header-none }
OR/HZ	|-
  |一日|十日|十九日|二十八日|三十七日|忌左脚足|
  |---|---|---|---|---|---|
  |二日|十一日|二十日|二十九日|三十八日|忌頭首喉膺|
  |三日|十二日|二十一日|三十日|三十九日|忌腰尻下竅|
TR/KO	|-
  |1일|10일|19일|28일|37일|왼쪽 다리와 발을 조심한다.|
  |---|---|---|---|---|---|
  |2일|11일|20일|29일|38일|머리ㆍ목구멍ㆍ가슴을 조심한다.|
  |3일|12일|21일|30일|39일|허리ㆍ꽁무니ㆍ전음ㆍ후음을 조심한다.|
TR/EN	<table><thead><tr><th>1st day</th><th>10th day</th>...</table>
```

### 4. 기타

#### ① 인클루드(include) 문법

반복되는 주석이나 설명, 복잡한 html 표 등에 사용하며, 별도의 YAML 파일로 작성된 텍스트를 끼워 넣을 수 있습니다.

- YAML 파일은 id와 text의 쌍으로 이루어져야 합니다.
- 콘텐츠에는 text의 값이 들어가게 됩니다.

```
ID	7TzSab027AVVBQ
META	{ TAG: h3 }
OR/HZ	形氣之始
TR/KO	형기｛｛include/abc.yaml?id=12｝｝의 시작
TR/EN	The Beginning of Form and Qi

# abc.yaml 파일
- id: 12
  text: ｛｛형기(形氣)란 육체와 정신을 아우르는 표현이다.｝｝
```

#### ② 처방 구성의 표준 형식

전형적인 처방의 구성은 아래 형식을 권장합니다.

- 포제는 괄호로 묶습니다.
- 용량은 앞에 공백 1칸을 넣고, 뒤에 쉼표나 마침표를 넣습니다.
- 본초의 나열은 가운뎃점 `ㆍ`(아래아, u+318D)을 넣습니다.
- 번역문에서 숫자를 표기할 때는 `7.5푼`처럼 가급적 연결하여 씁니다.
- 용량에서 자주 사용하는 `斤, 兩, 錢, 分`은 `근, 냥, 돈, 푼`으로 표기하고, 이외에는 `자(字)`처럼 한자 병기를 권장합니다.
- 관습적으로 단위 없이 사용하는 `薑三棗二` 등은 붙여쓸 수 있습니다.

```
OR/HZ	白芍藥(酒炒)␣二錢, 當歸ㆍ人參ㆍ白朮ㆍ黃芪(蜜炒)ㆍ肉桂ㆍ陳皮ㆍ甘草(灸)␣各一錢, 熟地黃ㆍ五味子ㆍ防風␣各七分半, 遠志␣五分. 右剉, 作一貼, 薑三棗二, 水煎服.
TR/KO	백작약(술에 축여 볶는다) 2돈, 당귀ㆍ인삼ㆍ백출ㆍ황기(꿀에 축여 볶는다)ㆍ육계ㆍ진피ㆍ감초(굽는다) 각 1돈, 숙지황ㆍ오미자ㆍ방풍 각 7.5푼, 원지 5푼. 이 약들을 썰어 1첩으로 하여 생강 3쪽, 대추 2개를 넣어 물에 달여 먹는다.
```

***

## 키(Key) 목록

- ID : 문단 고유값
- META : 메타데이터
	- TAG : 태그
	- CLASS : 클래스
	- SRC : 이미지명
	- ALT : 이미지 대체 텍스트(True/False)
- OR : 원본
- TR : 번역문
- DS : 현대의 설명
- HZ : 한문
- KO : 현대 한국어
- EN : 현대 영어
- AK : 옛한글
- HA : 한문+옛한글
- ZH : 현대 중국어
- JA : 현대 일본어

## 메타데이터(META)의 태그(TAG) 목록

- h1 : 권명
- h2 : 제목(1수준), 서발문, 범례 등
- h3 : 제목(2수준)
- h4 : 제목(3수준)
- h5 : 제목(4수준)
- h6 : 제목(5수준)
- p : 본문
- img : 삽화
- table : 표

## 메타데이터(META)의 클래스(CLASS) 목록

### 1. 텍스트

- intro : 서발문 등 본문 이외의 내용
- temp-title : 가독성을 위한 임의로 묶은 제목
- formula-title : 처방명
- formula-p : 처방 내용
- herb-title : 본초명
- acupoint-title : 경혈명
- outline-none : 개요번호를 매기지 않음
- traditional-comment : 원본에 있는 주석
- modern-comment : 현대인의 해설
- description : 제목에 대한 설명, 본문 이외의 텍스트
- indent0 : 내어쓰기(대두, 평출)
- indent1 : 들여쓰기(1칸)
- indent2 : 들여쓰기(2칸)
- indent3 : 들여쓰기(3칸)
- poem : 한시(漢詩)나 가결(歌訣) 형식
- four-six : 변려문(騈儷文)이나 대구(對句) 형식

### 2. 삽화

- pic : 삽화
- pic-title : 삽화의 제목
- pic-p : 삽화의 텍스트

### 3. 표

- header-top : 상단 헤더
- header-left : 좌측 헤더
- header-none : 헤더 없음
