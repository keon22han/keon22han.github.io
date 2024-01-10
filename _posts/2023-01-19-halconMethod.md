---
title: "Halcon Method Code Review"
categories: coding
tag: [Halcon]
header:
    overlay_image: assets/images/halcon.png
    overlay_filter: 0.5
    teaser: assets/images/halcon.png
---

# HALCON Syntex

## **`for`**

- **`for Index := Start to End by Step`**
- 일반적으로 고정된 반복 횟수 동안 실행되는 루프 블록을 시작합니다.
- 매개변수
  
  
    | 변수명 | 입출력 | 타입 | 설명 |
    | --- | --- | --- | --- |
    | None | None | None | None |
- 예시

```jsx
I:=[]
  for Index := 1.3 to 1.6 by 0.1
    I := [I,Index]
  endfor
```

---

# HALCON Methods

## **`dev_update_off`**

- `dev_update_off()`
- dev_update_pc, dev_update_var 및 dev_update_window를 'off'로 전환합니다.
- 매개변수
  
  
    | 변수명 | 입출력 | 타입 | 설명 |
    | --- | --- | --- | --- |
    | None | None | None | None |
- dev_update_pc, dev_update_var 및 dev_update_window의 매개 변수 표시 모드를 'off'로 설정합니다.
- 반환값 X
- 예시

```jsx
dev_update_off ()
* do something
dev_update_on ()
```

## **`dev_update_on`**

- `dev_update_on()`
- dev_update_pc, dev_update_var 및 dev_update_window를 'on'으로 전환합니다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| None | None | None | None |
- dev_update_pc, dev_update_var 및 dev_update_window의 매개 변수 표시 모드를 'on'으로 설정합니다.
- 반환값 X
- 예시

```jsx
dev_update_off ()
* do something
dev_update_on ()
```

## **`list_image_files`**

- `list_image_files(ImageDirectory,Extensions,Options,ImageFiles)`
- 지정된 경로에 있는 모든 이미지 파일을 가져옵니다
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| ImageDirectory | input_control | filename.dir(-array) → (string) | 이미지 경로 |
| Extensions | input_control | string(-array) → (string) | 찾을 확장자가 포함된 문자열 튜플(예: ['png',tif',jpg') 또는 기타 항목) |
| Options | input_control | string(-array) → (string) | 처리 옵션 |
| ImageFiles | output_control | filename.read(-array) → (string) | 발견된 모든 이미지 파일 이름 튜플 |
- 지정된 디렉토리 ImageDirectory의 모든 파일을 확장명에 지정된 접미사 중 하나와 함께 반환합니다.  여러 디렉토리가 있는 튜플을 입력 이미지 디렉토리로 사용할 수 있습니다. 로컬에서 디렉토리를 찾을 수 없는 경우 해당 디렉토리는 %HALCONIMAGES%/ImageDirectory에서 검색됩니다. 확장이 'default' 또는 빈 문자열 '로 설정된 경우 HALCON에서 지원하는 모든 이미지 접미사가 사용됩니다. 매개 변수 옵션은 'files' 옵션이 항상 사용되는 것을 제외하고 연산자 list_files(자세한 내용은 list_files 참조)에서와 같이 사용됩니다. '디렉토리' 옵션은 파일만 반환되므로 런타임이 증가합니다.
- 예시

```jsx
list_image_files ('.',[] ,[] , ImageFiles)
for Index := 1 to |ImageFiles|  by 1
    read_image (Image,ImageFiles[Index-1])
*     do something
endfor
```

> 원형: `list_image_files( : : ImageDirectory, Extensions, Options : ImageFiles)`
> 
> 
> 의역: `list_image_files( : : 이미지 디렉토리 경로, 파일 형식, Options : 컨트롤 변수 이름)`
> 
> <input_control>
> 
> 지정된 디렉토리 경로의 모든 파일을 확장명에 지정된 접미사(Extensions)중 하나와 함께 반환
> 
> (''(공백) 이라면 해당 HDEV 파일(할콘 코드 파일)의 경로를 말함) (filename.dir(-array) → (string))
> 
> 여러 디렉토리가 있는 튜플을 입력 이미지 디렉토리로 사용 가능
> 
> 만약 ImageDirectory를 경로에서 찾을 수 없다면 %HALCONIMAGES%/ImageDirectory으로 검색
> 
> Extensions이 'default' 또는 빈 문자열로 설정된 경우 HALCON에서 지원하는 모든 이미지 접미사가 사용됩니다.
> 
> 지원하는 이미지 접미사: ['png','tif',jpg'] or others (string(-array) → (string))
> 
> 추천하는 값들: 'ima', 'bmp', 'jpg', 'png', 'tiff', 'tif', 'gif', 'jpeg', 'pcx', 'pgm', 'ppm', 'pbm', 'xwd', 'pnm'
> 
> Options 디폴트 값은 [] 추천하는 값들: 'recursive', 'follow_link', 'max_depth 5' (string(-array) → (string))
> 
> <output_control>
> 
> ImageFiles: 컨트롤 변수 이름 (filename.read(-array) → (string))
> 
> 해당 경로에서 찾은 모든 이미지 파일의 경로와 이름을 튜플 형태로 저장하는 변수 이름
> 

> 
> **지정한 경로에서 지정한 타입의 이미지 정보를 지정한 컨트롤 변수 이름으로 가져오기**
> 


## **`read_image`**

- `read_image( Image , FileName )`
- 파일 형식이 다른 이미지를 읽습니다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Image | output_object | image(-array) | 이미지를 저장할 변수명 |
| FileName | input_control | filename.read(-array) → (string) | 읽을 이미지의 파일명 |
- read_image 연산자는 백그라운드 저장소에서 표시된 이미지 파일을 읽고 이미지를 생성합니다. 파일 이름에 하나 이상의 파일 이름을 전달할 수 있습니다. 둘 이상의 파일 이름을 전달하면 해당하는 수의 이미지 개체가 있는 이미지 개체 튜플이 반환됩니다.
- 매개 변수가 올바르면 연산자 read_image가 값 2(H_MSG_TRUE)를 반환합니다. 그렇지 않으면 예외가 발생합니다.
- 예시

```jsx
* Reading an image:
  read_image(Image,'mreut')

* Reading 3 images into an image array:
  read_image(Images,['ic0','ic1','ic2'])

* Setting of search path for images on '/mnt/images' and '/home/images':
  set_system('image_dir','/mnt/images:/home/images')
```

## **`rgb1_to_gray`**

- `rgb1_to_gray(RGBImage , GrayImage )`
- RGB 이미지를 그레이 스케일 이미지로 변환합니다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| RGBImage | input_object | (multichannel-)image(-array) → object (byte / int2 / uint2 / real) | 3채널 RBG 영상 |
| GrayImage | output_object | singlechannelimage(-array) → object (byte / int2 / uint2 / real) | 그레이 스케일 이미지 |
- rgb1_to_gray는 RGB 이미지를 그레이 스케일 이미지로 변환합니다. RGB 영상의 3개 채널은 입력 영상의 처음 3개 채널로 전달됩니다. 영상은 다음 공식에 따라 변환됩니다.                                                                         `회색 = 0.165 * 빨간색 + 0.587 * 녹색 + 0.114 * 파란색`                                            RGBImage의 입력 영상 중 하나가 단일 채널 영상인 경우 기준이 출력 GrayImage에 복사됩니다.
- 예시

```jsx
* Transformation from rgb to gray
read_image(Image,'patras')
dev_display(Image)
rgb1_to_gray(Image,GrayImage)
dev_display(GrayImage)
```

## **`dev_get_window`**

- `dev_get_window( WindowHandle )`
- 활성 그래픽 창의 핸들을 반환합니다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| WindowHandle | output_control | window → (handle) | 그래픽 창의 창 핸들 |
- dev_get_messages는 활성 그래픽 창의 창 핸들을 반환합니다. 현재 열려 있는 그래픽 창이 없으면 반환 값은 -1입니다.
- 지정된 매개 변수의 값이 올바르면 dev_get_window는 2(H_MSG_TRUE)를 반환합니다. 그렇지 않으면 예외가 발생하고 오류 코드가 반환됩니다.
- 예시

```jsx
read_image (Image,'mreut')
threshold (Image, Region, 100, 200)
dev_open_window (1, 1, 200, 200, 'black', WindowID1)
dev_open_window (1, 220, 200, 200, 'black', WindowID2)
dev_get_window (CurrentWindowID)
dev_set_window (WindowID1)
dev_set_color ('blue')
dev_display (Image)
dev_display (Region)
dev_set_window(CurrentWindowID)
```

## **`dev_set_draw`**

- `dev_set_draw( DrawMode )`
- 영역 채우기 모드를 정의합니다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| DrawMode | input_control | string → (string) | 영역 출력의 채우기 모드입니다.

기본값: 'fill'
값 목록: 'fill', 'margin' |
- dev_set_draw는 영역의 채우기 모드를 정의합니다. 그리기 모드를 'fill'으로 설정하면 영역이 채워지고, 'margin'으로 설정하면 윤곽선만 표시됩니다. 'margin' 모드에서 윤곽선의 모양은 dev_set_line_width 및 set_line_style에 의해 영향을 받을 수 있습니다.
- 지정된 매개 변수의 값이 올바르면 dev_set_draw는 2(H_MSG_TRUE)를 반환합니다. 그렇지 않으면 예외가 발생하고 오류 코드가 반환됩니다.
- 예시

```jsx
read_image(Image,'monkey')
threshold(Image,Region,128,255)
dev_clear_window ()
dev_set_color('red')
dev_set_draw('fill')
dev_display(Region)
dev_set_color('white')
dev_set_draw('margin')
dev_display(Region)
```

## `create_metrology_model`

- `create_metrology_model( MetrologyHandle )`
- 기하학적 형상을 측정하는 데 필요한 데이터 구조를 생성한다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| MetrologyHandle | output_control | metrology_model → (handle) | metrology model 의 핸들 |
- create_metrology_model은 2D metrology를 통해 특정 기하학적 모양(메트릭 객체)으로 객체를 측정하는 데 필요한 데이터 구조인 metrology model 을 생성하고 이를 Metrology Handle 핸들에 반환한다.
- 매개 변수가 유효하면 연산자 create_metrology_model은 값 2(H_MSG_TRUE)를 반환한다. 필요한 경우 예외가 발생한다.
- 예시

```jsx
read_image (Image, 'fabrik')
create_metrology_model (MetrologyHandle)
get_image_size (Image, Width, Height)
set_metrology_model_image_size (MetrologyHandle, Width, Height)
add_metrology_object_rectangle2_measure (MetrologyHandle, 270, 230, 0, 30, \
                                  25, 10, 2, 1, 30, [], [], Index)
apply_metrology_model (Image, MetrologyHandle)
get_metrology_object_result (MetrologyHandle, Index, 'all', 'result_type', \
                      'all_param', Rectangle)
get_metrology_object_result_contour (Contour, MetrologyHandle, \
                                    Index, 'all', 1.5)
```

## **`gen_empty_obj`**

- `gen_empty_obj( EmptyObject )`
- 빈 개체 튜플을 만듭니다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| EmptyObject | output_object | object → object | Object (X) |
- 연산자 gen_empty_obj는 빈 튜플을 생성한다. 즉, 출력 매개 변수에 개체가 포함되어 있지 않다. 따라서 연산자 count_obj는 0을 반환한다. 그러나 출력에 대해 clear_obj를 호출할 수 있다. 어떤 물체도 빈 영역과 혼동해서는 안 된다는 점에 유의해야 한다. 빈 영역인 경우, 즉 0픽셀 count_obj가 있는 영역은 값 1을 반환한다.

## **`select_obj`**

- `select_obj(Objects , ObjectSelected , Index )`
- 개체 튜플에서 개체를 선택합니다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Objects | input_object | object(-array) → object | 입력 개체 |
| ObjectSelected | output_object | object(-array) → object | 선택한 개체 |
| Index | input_control | integer(-array) → (integer) | 선택할 개체의 인덱스.                   기본값: 1
권장 값: 1, 2, 3, 4, 5, 6, 8, 9, 10, 50, 100, 200, 500, 1000, 2000, 5000
제한: 지수 >= 1 |
- select_obj는 아이콘 입력 객체 튜플 Objects에서 출력 객체 ObjectSelected로 인덱스(1로 시작)가 지정한 인덱스를 사용하여 아이콘 객체를 복사합니다. 영역 및 이미지에 할당된 새 스토리지가 없습니다. 대신 기존 개체에 대한 참조가 포함된 새 개체가 생성됩니다. 개체 튜플의 개체 수는 연산자 count_obj를 사용하여 쿼리할 수 있습니다.
- select_obj는 모든 개체가 HALCON 데이터베이스에 포함되어 있고 모든 매개 변수가 올바른 경우 2(H_MSG_TRUE)를 반환합니다. 입력이 비어 있으면 set_system(: 'no_object_result', <Result>:)을 통해 동작을 설정할 수 있습니다. 필요한 경우 예외가 발생합니다.
- 예제

```jsx
/* Access to all Regions */

count_obj(Regions,&Num);
for (i=1; i<=Num; i++)
{
  select_obj(Regions,&Single,i);
  T_get_region_polygon(Single,5.0,&Row,&Column);
  T_disp_polygon(WindowHandleTuple,Row,Column);
  destroy_tuple(Row);
  destroy_tuple(Column);
}
```

## **`get_image_size`**

- `get_image_size(Image , Width , Height)`
- 이미지 크기를 반환합니다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Image | input_object | (multichannel-)image(-array) → object (byte / direction / cyclic / int1 / int2 / uint2 / int4 / int8 / real / complex / vector_field) | 입력 이미지 |
| Width | output_control | extent.x(-array) → (integer) | 이미지의 너비 |
| Height | output_control | extent.y(-array) → (integer) | 이미지의 높이 |
- get_image_size 연산자는 입력 이미지의 이미지 크기(폭 및 높이)를 반환합니다.
- 연산자 get_image_size는 값 2(H_MSG_TRUE)를 반환합니다. 빈 입력(사용 가능한 입력 이미지 없음)의 경우 동작은 연산자 set_system('no_object_result', <Result>)을 통해 설정됩니다. 필요한 경우 예외가 발생합니다.

## **`dev_display`**

- `dev_display( Object )`
- 현재 그래픽 창에 이미지 개체를 표시합니다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Object | input_object | object(-array) → object | 표시할 이미지 개체입니다. |
- dev_display는 활성 그래픽 창에 아이콘 객체(이미지, 영역 또는 XLD)를 표시합니다. 이는 변수 창 내부의 아이콘 변수를 두 번 클릭하는 것과 같습니다.
- 지정된 매개 변수의 값이 올바르면 dev_display는 2(H_MSG_TRUE)를 반환합니다. 그렇지 않으면 예외가 발생하고 오류 코드가 반환됩니다.
- 예시

```jsx
read_image (Image, 'fabrik')
regiongrowing (Image, Regions, 3, 3, 6, 100)
dev_clear_window ()
dev_display (Image)
dev_set_colored (12)
dev_set_draw ('margin')
dev_display (Regions)
```

## **`threshold`**

- `threshold( Image , Region , MinGray , MaxGray )`
- 전역 임계값을 사용하여 이미지를 세그먼트화합니다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Image | input_object | singlechannelimage(-array) → object (byte / direction / cyclic / int1 / int2 / uint2 / int4 / int8 / real / vector_field) | 입력 이미지 |
| Region | output_object | region(-array) → object | 세그먼트 영역. |
| MinGray | input_control | number(-array) → (real / integer / string) | 회색 값 또는 'min'에 대한 하한 임계값.
기본값: 128.0
권장 값: 0.0, 10.0, 30.0, 64.0, 128.0, 200.0, 220.0, 255.0, 'min' |
| MaxGray | input_control | number(-array) → (real / integer / string) | 회색 값의 상한 임계값 또는 'max'입니다.
기본값: 255.0
권장 값: 0.0, 10.0, 30.0, 64.0, 128.0, 200.0, 220.0, 255.0, 'max'
제한사항 : MaxGray >= MinGray |
- threshold(임계값)는 입력 영상에서 회색 값 g이 다음 조건을 충족하는 픽셀을 선택합니다.                            `MinGray≤ g ≤ MaxGray` . 조건을 충족하는 이미지의 모든 포인트가 하나의 영역으로 반환됩니다. 둘 이상의 회색 값 간격(MinGray 및 MaxGray의 경우 튜플)을 통과하면 각 간격에 대해 하나의 개별 영역이 반환됩니다. 벡터 필드 영상의 경우 임계값이 회색 값이 아니라 벡터의 길이에 적용됩니다. MinGray 및 MaxGray 파라미터를 각각 'min' 또는 'max'로 설정하여 하한 및 상한을 개방 상태로 유지할 수 있습니다.
- threshold는 모든 파라미터가 올바른 경우 2(H_MSG_TRUE)를 반환합니다. 입력 영상 및 출력 영역에 대한 동작은 'no_object_result', 'empty_region_result' 및 'store_empty_region' 플래그의 값을 set_system과 함께 설정하여 결정할 수 있습니다. 필요한 경우 예외가 발생합니다.
- 예시

```jsx
read_image(Image,'fabrik')
sobel_dir(Image,EdgeAmp,EdgeDir,'sum_abs',3)
threshold(EdgeAmp,Seg,50,255)
skeleton(Seg,Rand)
connection(Rand,Lines)
select_shape(Lines,Edges,'area','and',10,1000000)
```

## **`closing_circle`**

- `closing_circle( Region , RegionClosing , Radius )`
- 원형 구조 요소로 영역을 닫습니다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Region | input_object | region(-array) → object | 닫힐 영역. |
| RegionClosing | output_object | region(-array) → object | 닫힌 영역. |
| Radius | input_control | real → (real / integer) | 원형 구조 요소의 반지름입니다.
기본값: 3.5
권장 값: 1.5, 2.5, 3.5, 4.5, 5.5, 7.5, 9.5, 12.5, 15.5, 19.5, 25.5, 33.5, 45.5, 60.5, 110.5
일반적인 값 범위: 0.5 ≤ 반지름 ≤ 511.5 (lin)
최소 증분: 1.0
권장 증분: 1.0 |
- closing_circle은 닫힘과 유사하게 동작한다. 즉, 영역의 경계가 평활되고 반지름의 원형 구조 요소보다 작은 영역 내의 구멍이 닫힌다. closing_circle 연산은 동일한 원형 구조 요소를 가진 민코프스키 뺄셈 뒤의 확장으로 정의된다.
- closing_circle은 모든 매개 변수가 올바른 경우 2(H_MSG_TRUE)를 반환합니다. 입력 영역이 비어 있거나 없는 경우의 동작은 다음을 통해 설정할 수 있습니다:                                                                                               영역 없음: set_system('no_object_result', <RegionResult>)                                                                               빈 영역: set_system('empty_region_result', <RegionResult>)

       그렇지 않으면 예외가 발생합니다.

- 예시

```jsx
my_closing_circle(Hobject In, double Radius, Hobject *Out)
{
  Hobject  tmp, StructElement;
  gen_circle(StructElement,100.0,100.0,Radius);
  dilation1(In,StructElement,&tmp,1);
  minkowski_sub1(tmp,StructElement,Out,1);
}
```

## **`fill_up`**

- `fill_up(Region , RegionFillUp )`
- 지정한 영역의 안쪽을 채웁니다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Region | input_object | region(-array) → object | 지정한 영역 안쪽의 포함된 입력 영역입니다. |
| RegionFillUp | output_object | region(-array) → object | 지정한 영역이 아닌 영역입니다. |
- fill_up은 지정한 영역 안쪽을 채웁니다. 지역 수는 변경되지 않았습니다. 이웃 유형은 set_system('neighborhood', <4/8>)(기본값: 8-neighborhood)을 통해 설정됩니다.
- fill_up은 모든 매개 변수가 올바른 경우 2(H_MSG_TRUE)를 반환합니다. 빈 입력의 경우 동작(영역이 지정되지 않음)은 set_system('no_object_result', <Result>)을 통해 설정하고 빈 입력 영역의 경우 동작('empty_region_result', <Result>)을 통해 설정할 수 있습니다. 필요한 경우 예외가 발생합니다.

## **`difference`**

- `difference( Region , Sub , RegionDifference )`
- 두 영역의 차이를 계산합니다. (단 Region을 기준으로)
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Region | input_object | region(-array) → object | 처리할 영역. |
| Sub | input_object | region(-array) → object | 이 Region들의 Union은 지역에서 차감 |
| RegionDifference | output_object | region(-array) → object | 결과 영역 |
- 차분은 두 영역의 집합론적 차이를 계산합니다:                                                                                                             (지역) - (하위 지역)                                                                                                                                                                     결과 영역은 Sub에서 모든 점이 제거된 입력 영역(Region)으로 정의됩니다. 내부적으로 Sub의 모든 영역은 Region의 개별 영역과 United Region의 차이가 계산되기 전에 하나의 영역으로 통합됩니다.
- difference는 항상 값 2(H_MSG_TRUE)를 반환합니다. 빈 입력의 경우 동작(영역이 지정되지 않음)은 set_system('no_object_result', <Result>)을 통해 설정하고 빈 입력 영역의 경우 동작('empty_region_result', <Result>)을 통해 설정할 수 있습니다. 필요한 경우 예외가 발생합니다.
- 예시

```jsx
* provides the region X without the points in Y
difference(X,Y,RegionDifference)
```

## **`smallest_rectangle2`**

- `smallest_rectangle2(Regions, Row, Column, Phi, Length1, Length2)`
- 해당 아이코닉 변수의 행, 렬, Phi, 길이, 너비 값을 콘트롤 변수에 저장
- 매개변수
  
  
    | 변수명 | 입출력 | 타입 | 설명 |
    | --- | --- | --- | --- |
    | Regions | input_control | region(-array) → object | 대상 아이코닉 변수 |
    | Row | output_control | rectangle2.center.y(-array) → (real) | 행 값을 저장 할 변수 |
    | Column | output_control | rectangle2.center.x(-array) → (real) | 열 값을 저장 할 변수 |
    | Phi | output_control | rectangle2.angle.rad(-array) → (real) | Phi 값을 저장 할 변수  |
    | Length1 | output_control | rectangle2.hwidth(-array) → (real) | 길이 값을 저장 할 변수 |
    | Length2 | output_control | rectangle2.hheight(-array) → (real) | 너비 값을 저장 할 변수 |
- 연산자 minest_rectangle2는 영역의 가장 작은 주변 직사각형, 즉 영역을 포함하는 모든 직사각형 중 가장 작은 영역을 갖는 직사각형을 결정한다. 그 후 직사각형의 경우 중심, 기울기 및 두 반지름 가장 작은 직사각형을 기준으로 계산됩니다. 직사각형의 계산은 영역 픽셀의 중심 좌표에 기초한다.
- 멀티스레딩 유형: 다시 입력(비독점 연산자와 병렬로 실행됨).
- 멀티스레딩 범위: 글로벌(모든 스레드에서 호출할 수 있음).
- 입력이 비어 있지 않으면 연산자 minest_rectangle2가 값 2(H_MSG_TRUE)를 반환합니다. 빈 입력(사용 가능한 입력 영역 없음)의 경우 동작은 연산자 set_system('no_object_result', <Result>)을 통해 설정됩니다. 빈 영역의 경우 동작(영역은 빈 집합)은 set_system('empty_region_result', <Result>)을 통해 설정됩니다. 필요한 경우 예외가 발생합니다..
- 예시
  
    ```jsx
    read_image(Image,'fabrik')
    regiongrowing(Image,Regions,5,5,6,100)
    smallest_rectangle2(Regions,Row,Column,Phi,Length1,Length2)
    gen_rectangle2(Rectangle,Row,Column,Phi,Length1,Length2)
    dev_set_draw ('margin')
    dev_display(Rectangle)
    ```
    
    ​    

## **`add_metrology_object_rectangle2_measure`**

- `add_metrology_object_rectangle2_measure(MetrologyHandle, Row, Column, Phi, Length1, Length2, MeasureLength1, MeasureLength2, MeasureSigma, MeasureThreshold, GenParamName, GenParamValue, Index)`
- 매트릭 핸들에 직사각형 모델을 추가 후 해당 메트릭 개체의 인덱스 정보를 담은 컨트롤 변수 하나 생성
- 매개변수
  
  
    | 변수명 | 입출력 | 타입 | 설명 | 권장 값 |
    | --- | --- | --- | --- | --- |
    | MetrologyHandle | input_control, state is modified | metrology_model → (handle) | 메트릭 객체 |  |
    | Row | input_control | rectangle2.center.y(-array) → (real / integer) | 직사각형 중심의 행(또는 Y) 좌표 |  |
    | Column  | input_control | rectangle2.center.x(-array) → (real / integer) | 직사각형 중심의 열(또는 X) 좌표입니다. |  |
    | Phi | input_control | rectangle2.angle.rad(-array) → (real / integer) | 주축 [rad]의 방향. |  |
    | Length1 | input_control | rectangle2.hwidth(-array) → (real / integer) | Length of the larger half edge of the rectangle. |  |
    | Length2 | input_control | rectangle2.hheight(-array) → (real / integer) | Length of the smaller half edge of the rectangle. |  |
    | MeasureLength1 | input_control | number → (real / integer) | Half length of the measure regions perpendicular to the boundary. | 10.0, 20.0, 30.0 |
    | MeasureLength2 | input_control | number → (real / integer) | Half length of the measure regions tangential to the boundary. | 3.0, 5.0, 10.0 |
    | MeasureSigma | input_control | number → (real / integer) | Sigma of the Gaussian function for the smoothing. | 0.4, 0.6, 0.8, 1.0, 1.5, 2.0, 3.0, 4.0, 5.0, 7.0, 10.0 |
    | MeasureThreshold | input_control | number → (real / integer) | Minimum edge amplitude. | 5.0, 10.0, 20.0, 30.0, 40.0, 50.0, 60.0, 70.0, 90.0, 110.0 |
    | GenParamName | input_control | attribute.name-array → (string) | Names of the generic parameters. |  |
    | GenParamValue | input_control | attribute.value-array → (real / integer / string) | Values of the generic parameters. | 1, 2, 3, 4, 5, 10, 20, 'all', 'true', 'false', 'first', 'last', 'positive', 'negative', 'uniform', 'nearest_neighbor', 'bilinear', 'bicubic’ |
    | Index | output_control | integer → (integer) | 생성된 메트릭 개체의 인덱스 |  |
- 도량형 모델은 도량형 핸들에 의해 정의됩니다. 매개 변수 색인은 정보가 액세스되는 메트릭 개체를 결정합니다. Index를 'all'로 설정하면 모든 Metrology 객체의 파라미터에 액세스할 수 있습니다.
- 멀티스레딩 유형: 다시 입력(비독점 연산자와 병렬로 실행됨).
- 멀티스레딩 범위: 글로벌(모든 스레드에서 호출할 수 있음).
- 매개 변수가 유효한 경우 연산자 get_metrography_object_param은 값 2(H_MSG_TRUE)를 반환합니다. 필요한 경우 예외가 발생합니다.

## **`apply_metrology_model`**

- `apply_metrology_model(Image, MetrologyHandle)`
- 모서리 위치에 기하학적 모양 맞추기**.**
- 매개변수
  
  
    | 변수명 | 입출력 | 타입 | 설명 |
    | --- | --- | --- | --- |
    | Image | input_control | singlechannelimage → object (byte / uint2 / real) | 대상 아이코닉 변수 |
    | MetrologyHandle | input_control, state is modified | metrology_model → (handle) | 메트릭 객체 |
- 입력 이미지의 너비 또는 높이가 미터법 개체에 저장된 너비 및 높이와 같지 않은 경우(예: set_metroology_model_image_size로 설정) 모든 미터법 개체의 모든 측정 영역을 다시 계산해야 합니다. 이로 인해 작업자의 실행 시간이 길어집니다.
- 멀티스레딩 유형: 다시 입력(비독점 연산자와 병렬로 실행됨).
- 멀티스레딩 범위: 글로벌(모든 스레드에서 호출할 수 있음).
- MetrologyHandle 연산자를 실행하는 동안 이 매개 변수가 여러 스레드에서 사용되는 경우 이 매개 변수의 값에 대한 액세스를 동기화해야 합니다.

## **`get_metrology_object_param`**

- `get_metrology_object_param(MetrologyHandle, Index, GenParamName, GenParamValue)`
- 원하는 메트릭 객체(MetrologyHandle)의 파라미터(PK_Index)에 액세스.
- 매개변수
  
  
    | 변수명 | 입출력 | 타입 | 설명 | 권장 값 |
    | --- | --- | --- | --- | --- |
    | MetrologyHandle | input_control | metrology_model → (handle) | 메트릭 객체 |  |
    | Index | input_control | integer(-array) → (string / integer) | 매트릭 객체의 인덱스 | 'all', 0, 1, 2 |
    | GenParamName | input_control | attribute.name-array → (string) | 일반 매개 변수의 이름입니다. | 'column', 'column_begin', 'x_end', 'y','y_end’ …. |
    |  GenParamValue | output_control | attribute.value-array → (string / real / integer) | 일반 매개 변수의 값입니다. |  |
- 도량형 모델은 도량형 핸들에 의해 정의됩니다. 매개 변수 색인은 정보가 액세스되는 메트릭 개체를 결정합니다. Index를 'all'로 설정하면 모든 도량형 객체의 파라미터에 액세스할 수 있습니다.
- 멀티스레딩 유형: 다시 입력(비독점 연산자와 병렬로 실행됨).
- 멀티스레딩 범위: 글로벌(모든 스레드에서 호출할 수 있음).
- 매개 변수가 유효한 경우 연산자 get_metrography_object_param은 값 2(H_MSG_TRUE)를 반환합니다. 필요한 경우 예외가 발생합니다.

## `get_metrology_object_result_contour`

- `get_metrology_object_result_contour(Contour, MetrologyHandle, Index, Instance, Resolution)`
- 선택한 메트릭 객체(들)의 이미지 좌표에서 측정된 윤곽선 이미지를 아이코닉 변수로 생성
- 매개변수
  
  
    | 변수명 | 입출력 | 타입 | 설명 | 권장 값 |  |
    | --- | --- | --- | --- | --- | --- |
    | Contour | output_object | xld_cont(-array) → object | 이미지를 저장할 변수명 |  |  |
    | MetrologyHandle | input_control | metrology_model → (handle) | 메트릭 객체 |  |  |
    | Index | input_control | integer(-array) → (integer / string) | 매트릭 객체의 인덱스 | 'all', 0, 1, 2 |  |
    | Instance | input_control | integer(-array) → (string / integer) | Instance of the metrology object. | 'all', 0, 1, 2 |  |
    | Resolution | input_control | real → (real) | Distance between neighboring contour points. | >= 1.192e-7 |  |
- 도량형 모델은 도량형 핸들에 의해 정의됩니다. 매개변수 색인은 결과 등고선을 쿼리할 메트릭 개체를 지정합니다. '모두'로 설정된 색인의 경우 모든 도량형 객체의 결과 윤곽선이 반환됩니다. 도량형 객체에 대해 여러 결과(인스턴스)가 계산된 경우, 결과 윤곽선이 반환되는 경우에 대해 인스턴스(instance) 매개변수가 지정합니다. 모든 인스턴스의 결과 윤곽선은 Instance를 'all'로 설정하여 가져옵니다.
- 매개 변수가 유효한 경우 연산자 get_metrology_object_result_contour는 값 2(H_MSG_TRUE)를 반환합니다. 필요한 경우 예외가 발생합니다.
- 결과 윤곽선의 해상도는 픽셀의 인접 윤곽점 사이의 유클리드 거리를 포함하는 해상도를 통해 제어된다. 입력 값이 가능한 최소 값(1.192e-7) 아래로 떨어지면 분해능이 내부적으로 최소 유효 값으로 설정됩니다.


## `binary_threshold`

- `binary_threshold(Image , Region , Method , LightDark , UsedThreshold)`
- 이진 임계값을 사용하여 이미지를 분할한다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Image | input_object | singlechannelimage(-array) → object (byte / uint2) | 입력 이미지 |
| Region | output_object | region(-array) → object | 분할되어있는 결과 Region |
| Method | input_control | string → (string) | 분할 방법.

기본값: 'max_separability'
값 목록: 'max_separability', 'smooth_histo' |
| LightDark | input_control | string → (string) | 전경이나 배경을 추출하시겠습니까?

기본값: 'dark'
값 목록: 'dark', 'light' |
| UsedThreshold | output_control | number(-array) → (integer / string) | 사용된 threshold값. |
- binary_threshold는 자동으로 결정된 글로벌 threshold값을 사용하여 단일 채널 이미지를 분할하고 영역에서 분할된 영역을 반환합니다. 이는 예를 들어 균일하게 조명된 배경의 문자를 분할하는 데 유용하다. 또한 binary_threshold는 UsedThreshold에서 사용된 threshold값을 반환한다.사용된 임계값은 Method에 지정된 방법에 따라 결정된다. 현재 연산자는 'max_separability'와 'smooth_histo'의 두 가지 방법을 제공한다. 두 가지 방법은 모두 히스토그램이 바이모달인 영상에만 사용해야 한다.'smooth_histo' 메서드는 연산자 bin_threshold에서 제공한 것과 동일한 기능을 제공한다. max_separability' 메서드는 UsedThreshold에 대한 더 작은 값을 결정하는 경향이 있다. 게다가, 그것은 나머지 스펙트럼에서 멀리 떨어져 있는 히스토그램의 얇은 고립 피크에 덜 민감하며 종종 'smooth_histo'보다 빠르다.
- 이해를 돕기 위한 예시
  
    

## `opening_circle`

- `opening_circle( Region , RegionOpening , Radius )`
- 원형 구조 요소가 있는 영역을 연다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Region | input_object | region(-array) → object | 열릴 Regions |
| RegionOpening | output_object | region(-array) → object | 열린 Regions |
| Radius | input_control | real → (real / integer) | 원형 구조 요소의 반지름.

기본값: 3.5
권장 값: 1.5, 2.5, 3.5, 4.5, 5.5, 7.5, 9.5, 12.5, 15.5, 19.5, 25.5, 33.5, 45.5, 60.5, 110.5
일반적인 값 범위: 0.5 ≤ 반지름 ≤ 511.5 (lin)
최소 증분: 1.0
권장 증분: 1.0 |
- opening_circle은 순환 구조 요소를 가진 민코프스키 덧셈에 이은 침식으로 정의된다. 개방은 (원형 구조 요소보다 더 큰) 작은 영역을 제거하고 영역의 경계를 평활화하는 역할을 한다.
- opening_circle은 모든 매개 변수가 올바른 경우 2(H_MSG_TRUE)를 반환한다. 입력 영역이 비어 있거나 없는 경우의 동작은 다음을 통해 설정할 수 있다:                                                                                                              영역 없음: set_system('no_object_result', <RegionResult>)                                                                                                빈 영역: set_system('empty_region_result', <RegionResult>)                                                                                            그렇지 않으면 예외가 발생한다.
- 예시
  
    ```jsx
    * Large regions in an aerial picture (beech trees or meadows):
    read_image (Image, 'forest_road')
    threshold (Image, Region, 120, 255)
    * Close the small gaps.
    closing_circle (Region, RegionClosing, 3.5)
    * Select the large regions.
    opening_circle (RegionClosing, RegionOpening, 19.5)
    ```
    
    

## `connection`

- `connection( Region , ConnectedRegions )`
- Region의 연결된 구성 요소를 계산하여 튜플 형식의 아이코닉 변수를 만든다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Region | input_object | region(-array) → object | 입력 영역 |
| ConnectedRegions | output_object | region-array → object | 연결된 구성 요소 |
- connection은 Region에 지정된 입력 영역의 연결된 구성 요소를 결정한다. 이를 위해 사용되는 근린은 set_system('근린', <4/8>)을 통해 설정할 수 있다. 기본값은 8-근접이며, 이는 전경의 연결된 구성요소를 결정하는 데 유용하다. 연결을 통해 반환되는 연결된 구성 요소의 최대 수는 set_system('max_connection', <Num>)을 통해 설정할 수 있다. 기본값 0을 사용하면 연결된 모든 구성 요소가 반환된다. 연결의 역 연산자는 union1이다.
- connection은 항상 값 2(H_MSG_TRUE)를 반환한다. 빈 입력의 경우 동작(영역이 지정되지 않음)은 set_system('no_object_result', <Result>)을 통해 설정할 수 있으며, 빈 입력 영역의 경우 동작은 set_syst를 통해 설정할 수 있다.
- 예시

```jsx
read_image(Image,'clip')
dev_set_colored(12)
threshold(Image,Dark,0,150)
count_obj(Dark,NumThresholded)
dev_display (Dark)
connection(Dark,ConnectedRegions)
count_obj(ConnectedRegions,NumConnected)
dev_display (ConnectedRegions)
```

## `select_shape_std`

- `select_shape_std( Regions , SelectedRegions , Shape , Percent )`
- 지정된 도형의 영역 선택
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Regions | input_object | region(-array) → object | 선택될 입력 regions |
| SelectedRegions | output_object | region(-array) → object | 원하는 모양의 Regions |
| Shape | input_control | string → (string) | 검사할 Shape Features.

기본값: 'max_area'
값 목록: 'max_area', '직사각1', '직사각2' |
| Percent | input_control | real → (real) | 유사도 측도.

기본값: 70.0
권장 값: 10.0, 30.0, 50.0, 60.0, 70.0, 80.0, 90.0, 95.0, 100.0
일반적인 값 범위: 0.0 ≤ 백분율 ≤ 100.0 (lin)
최소 증분: 0.1
권장 증분: 10.0 |
- 연산자 select_shape_std는 지정된 영역의 모양을 기본 모양과 비교합니다. 영역의 모양이 유사한 경우 출력에 채택됩니다. 형상에 사용할 수 있는 값은 다음과 같다.

**'max_area'**

가장 큰 영역이 선택된다.

**'rectangle1'**

좌표축에 평행한 주변 직사각형은 minest_rectangle1 연산자를 통해 결정됩니다. 백분율의 면적 차이가 백분율보다 클 경우 지역이 채택된다.

**'retangle2’**

방향이 있는 가장 작은 주변 직사각형은 minest_rectangle2 연산자를 통해 결정됩니다. 백분율의 면적 차이가 백분율보다 클 경우 지역이 채택됩니다. 보다 강력한 대안으로 Feature가 '사각성'으로 설정된 select_shape 연산자를 대신 사용할 수 있다.

## **`smallest_rectangle2`**

- `smallest_rectangle2(Regions, Row, Column, Phi, Length1, Length2)`
- 해당 아이코닉 변수의 행, 렬, Phi, 길이, 너비 값을 컨트롤 변수에 저장
- 매개변수
  
  
    | 변수명 | 입출력 | 타입 | 설명 |
    | --- | --- | --- | --- |
    | Regions | input_control | region(-array) → object | 대상 아이코닉 변수 |
    | Row | output_control | rectangle2.center.y(-array) → (real) | 행 값을 저장 할 변수 |
    | Column | output_control | rectangle2.center.x(-array) → (real) | 열 값을 저장 할 변수 |
    | Phi | output_control | rectangle2.angle.rad(-array) → (real) | Phi 값을 저장 할 변수  |
    | Length1 | output_control | rectangle2.hwidth(-array) → (real) | 길이 값을 저장 할 변수 |
    | Length2 | output_control | rectangle2.hheight(-array) → (real) | 너비 값을 저장 할 변수 |
- 연산자 minest_rectangle2는 영역의 가장 작은 주변 직사각형, 즉 영역을 포함하는 모든 직사각형 중 가장 작은 영역을 갖는 직사각형을 결정한다. 그 후 직사각형의 경우 중심, 기울기 및 두 반지름 가장 작은 직사각형을 기준으로 계산됩니다. 직사각형의 계산은 영역 픽셀의 중심 좌표에 기초한다.
- 멀티스레딩 유형: 다시 입력(비독점 연산자와 병렬로 실행됨).
- 멀티스레딩 범위: 글로벌(모든 스레드에서 호출할 수 있음).
- 입력이 비어 있지 않으면 연산자 minest_rectangle2가 값 2(H_MSG_TRUE)를 반환합니다. 빈 입력(사용 가능한 입력 영역 없음)의 경우 동작은 연산자 set_system('no_object_result', <Result>)을 통해 설정됩니다. 빈 영역의 경우 동작(영역은 빈 집합)은 set_system('empty_region_result', <Result>)을 통해 설정됩니다. 필요한 경우 예외가 발생합니다..
- 예시
  
    ```jsx
    read_image(Image,'fabrik')
    regiongrowing(Image,Regions,5,5,6,100)
    smallest_rectangle2(Regions,Row,Column,Phi,Length1,Length2)
    gen_rectangle2(Rectangle,Row,Column,Phi,Length1,Length2)
    dev_set_draw ('margin')
    dev_display(Rectangle)
    ```


## `gen_polygons_xld`

- `gen_polygons_xld( Contours , Polygons , Type , Alpha )`
- 다각형별 대략적인 XLD 윤곽선.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Contours | input_object | xld_cont-array → object | 근사할 등고선. |
| Polygons | output_object | xld_poly-array → object | 다각형에 근사한(?) |
| Type | input_control | string → (string) | 근사치 유형.

기본값: 'ramer'
값 목록: 'ramer' |
| Alpha | input_control | number → (real / integer) | 근사치에 대한 임계값.
기본값: 2.0
권장 값: 1.0, 1.5, 2.0, 3.0, 4.0
제한: 알파 > 0.0 |
- gen_polygons_xld는 다각형별로 XLD 등고선(윤곽선)을 근사합니다. 근사치의 유형은 유형별로 설정할 수 있습니다. 근사치의 임계값은 알파를 통해 설정됩니다. 이 절차는 열린 윤곽선과 닫힌 윤곽선을 처리할 수 있습니다. 결과적으로 대략적인 XLD 다각형이 다각형으로 반환됩니다.                                                                  
- 윤곽선은 Ramer 알고리즘으로 근사할 수 있으며, 이 알고리즘은 윤곽선에 대한 근사 다각형의 유클리드 거리가 최대 알파 픽셀 단위가 되도록 근사합니다.

## `split_contours_xld`

- `split_contours_xld(Polygons , Contours , Mode , Weight , Smooth )`
- 지배적인 점에서 XLD 윤곽을 분할.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Polygons | input_object | xld_poly(-array) → object | 해당 윤곽선을 분할할 다각형. |
| Contours | output_object | xld_cont(-array) → object | 윤곽선을 분할하는 역할 |
| Mode | input_control | string → (string) | 등고선 분할 모드.

기본값: 'polygon'
값 목록: '지배적', '다각형' |
| Weight | input_control | integer → (integer) | 민감성을 위한 가중치.

기본값: 1 |
| Smooth | input_control | integer → (integer) | 스무딩 마스크의 너비입니다.

기본값: 5 |
- split_contours_xld는 눈에 띄는 점에서 다각형을 생성하는 데 사용된 윤곽선을 분할한다. 모드 'polygon'을 선택하면 윤곽선이 다각형의 제어점에서 분할된다. 결정된 방향은 너비가 부드러운 가우스로 평활된다. 무게는 작업자의 민감도에 대한 가중치 요소이다. 가중치를 선택할수록 우세한 점이 덜 발견된다.

## `length_xld`

- `length_xld(XLD , Length)`
- 해당 이미지의 등고선 또는 다각형의 길이들을 튜플 형식으로 생성
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| XLD | input_object | xld(-array) → object | 검사할 윤곽선 또는 다각형. |
| Length | output_control | real(-array) → (real) | 등고선 또는 다각형의 길이입니다.

주장: 길이 >= 0 |
- length_xld는 등고선 또는 다각형 XLD의 길이를 계산한다. 길이는 등고선 또는 다각형에서 연속되는 점들의 유클리드 거리의 합으로 계산된다. 둘 이상의 윤곽선 또는 다각형을 통과하면 결과가 XLD의 각 윤곽선 또는 다각형과 동일한 순서로 튜플에 저장된다.
- length_xld는 입력이 비어 있지 않으면 2(H_MSG_TRUE)를 반환한다. 입력이 비어 있으면 set_system(: 'no_object_result', <Result>:)을 통해 동작을 설정할 수 있다. 필요한 경우 예외가 발생한다.
- 이해를 돕기 위한 예시
  


## `area_center_points_xld`

- `area_center_points_xld(XLD , Area , Row , Column)`
- point clouds로 처리된 등고선과 다각형의 면적과 무게 중심(중심)이다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| XLD | input_object | xld(-array) → object | 등고선 또는 다각형의 형태로 검사할 point clouds. |
| Area | output_control | real(-array) → (real) | point cloud의 영역 |
| Row | output_control | point.y(-array) → (real) | 중심의 행 좌표. |
| Column | output_control | point.x(-array) → (real) | 중심의 열 좌표. |
- area_center_points_xld는 등고선 또는 다각형 XLD에 의해 주어진 point clouds 의 면적과 무게 중심(중심)을 계산합니다(즉, 등고선 또는 다각형의 점 순서는 고려되지 않음). 영역은 point clouds 의 점 개수에 해당합니다. 중심은 모든 점의 산술 평균에 의해 주어진다. 윤곽선 또는 다각형이 닫힌 경우(끝점 = 시작점) 윤곽선 또는 다각형의 끝점은 다른 점의 두 배 무게를 받는 것을 방지하기 위해 고려되지 않습니다.                                  area_center_points_xld는 윤곽선 XLD가 자체적으로 교차하거나 자체 교차 없이 끝에서 시작점까지의 선을 사용하여 윤곽선을 닫을 수 없는 경우에 사용해야 합니다. 이 경우 area_center_xld는 유용한 결과를 산출하지 못하기 때문입니다. 등고선 또는 다각형이 자체적으로 교차하는지 여부를 테스트하려면 test_self_intersection_xld를 사용할 수 있습니다.
- area_center_points_xld는 입력이 비어 있지 않으면 2(H_MSG_TRUE)를 반환합니다. 입력이 비어 있으면 set_system(: 'no_object_result', <Result>:)을 통해 동작을 설정할 수 있습니다. 필요한 경우 예외가 발생합니다.

## `dev_disp_text`

- `dev_disp_text( String, CoordSystem, Row, Column, Color, GenParamName, GenParamValue )`
- 현재 그래픽 창에 텍스트를 표시한다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| String | input_control | string(-array) → (string) | 표시할 텍스트 메시지를 포함하는 문자열 튜플입니다. 튜플의 각 값이 한 줄로 표시됩니다.

기본값: 'hello' |
| CoordSystem | input_control | string → (string) | '윈도우'로 설정하면 윈도우 좌표계에 대해 텍스트 위치가 제공됩니다. 'image'로 설정하면 영상 좌표가 사용됩니다(확대된 영상에서 유용할 수 있음).

기본값: 'window'
값 목록: '이미지', '창' |
| Row | input_control | point.y(-array) → (integer / real / string) | 원하는 텍스트 위치의 수직 텍스트 정렬 또는 행 좌표.
기본값: 12
값 목록: 12, '아래쪽', '중앙', '위쪽' |
| Column | input_control | point.x(-array) → (integer / real / string) | 원하는 텍스트 위치의 수평 텍스트 정렬 또는 열 좌표.
기본값: 12
값 목록: 12, '가운데', '왼쪽', '오른쪽' |
| Color | input_control | string(-array) → (string) | 텍스트 색상을 정의하는 문자열의 튜플입니다.

기본값: '검은색'
값 목록: '검은색', '파란색', '코랄', '시안', '숲 녹색', '녹색', '라임 녹색', '마젠타', '빨간색', '슬레이트 블루', '노란색' |
| GenParamName | input_control | attribute.name(-array) → (string) | 일반 매개 변수 이름입니다.

기본값: []

값 목록: 'border_radius', 'box_color', 'box_pading', 'shadow_color', 'shadow_dx', 'shadow_dy', 'shadow_sigma' |
| GenParamValue | input_control | attribute.value(-array) → (string / integer / real) | 일반 매개 변수 값입니다.

기본값: []

값 목록: 5.0, '검은색', '파란색', '거짓', '숲 녹색', '빨간색', '참', '흰색' |

dev_disp_text는 현재 그래픽 창의 위치(행, 열)에 텍스트를 표시합니다.

단일 위치만 정의된 경우 String의 각 요소에 대해 하나의 텍스트 줄이 표시됩니다. 또한 '\n'은 줄 바꿈 문자로 해석됩니다. 즉, 줄 바꿈이 수행됩니다.

여러 위치가 정의된 경우 각 위치에 대해 단일 문자열 또는 하나의 문자열만 String에서 허용됩니다. 이 경우 '\n'을 사용하여 줄 바꿈을 수행해야 합니다.

문자열 끝의 새 줄('\n')은 무시됩니다.

텍스트의 위치는 윈도우 좌표(CoordSystem = '이미지') 또는 영상 좌표(CoordSystem = '이미지')로 지정할 수 있으며, 이는 확대된 영상을 사용할 때 유용합니다.

(Row, Column) 좌표를 제공할 뿐만 아니라 미리 정의된 값을 Row 및 Column에 전달하여 창에서 고정된 위치에 텍스트를 표시할 수도 있습니다(CoordSystem = 'context'인 경우에만 해당).:

| 'top', 'left' |  'top', 'center' | 'top', 'right' |
| --- | --- | --- |
| 'center', 'left' | 'center', 'center' | 'center', 'right' |
| 'bottom', 'left’ |  'bottom', 'center' | 'bottom', 'right' |

색상 매개변수에는 값의 튜플도 사용할 수 있습니다. 이 경우 지정된 색상은 모든 새 텍스트 위치에 대해 또는 단일 위치가 사용되는 경우 모든 새 텍스트 줄에 대해 주기적으로 사용됩니다.

**일반 매개 변수**

disp_text는 상자 안에 문자열을 표시할 수 있습니다(기본값). 이 동작과 상자 모양은 GenParamName 및 GenParamValue의 일반 매개 변수로 정의됩니다.

**'box'**

'box'를 'true'로 설정하면 텍스트가 상자 안에 기록됩니다. 상자 모양과 선택적 그림자는 아래의 일반 매개 변수로 구성할 수 있습니다.

가능한 값: 'true' 및 'false'

기본값: 'true'

**'box_color'**

상자의 색상을 설정합니다.

가능한 값: 색상 이름을 포함하는 문자열(예: '흰색', '빨간색' 또는 '#a00bba0')

기본값: '#fce9d4'(밝은 주황색)

**'shadow’**

'shadow'를 'true'로 설정하면 상자 아래에 추가 그림자가 표시됩니다('box'가 'true'인 경우).

가능한 값: 'true' 및 'false'

기본값: 'box_color'가 알파 값이 없는 색상으로 설정된 경우 'true', 그렇지 않은 경우 'false'

**'shadow_color'**

'shadow'가 'true'인 경우 그림자의 색상을 설정합니다.

가능한 값: 색상 이름을 포함하는 문자열(예: '검은색', '빨간색' 또는 '#a00bba0')

기본값: '#f28d26'(더 어두운 오렌지색) 'box_color'가 설정되어 있지 않으면 'white'

**'border_radius'**

상자 모서리의 원형도를 제어합니다. 날카로운 모서리의 경우 이 값을 0으로 설정하여 모서리를 더 높은 값으로 부드럽게 만듭니다.

가능한 값: 양의 실수 또는 0

기본값: 2

**'box_padding'**

상자가 텍스트 주위로 확장되는 픽셀 단위의 양을 제어합니다.

가능한 값: 양의 실수

기본값: 0

**'shadow_sigma'**

상자 아래의 그림자가 흐려지는 양을 제어합니다. 선명한 그림자를 위해 0으로 설정합니다.

가능한 값: 양의 실수 또는 0

기본값: 1.5

**'shadow_dx' 및 'shadow_dy'**

픽셀 단위의 열('shadow_dx') 및 행('shadow_dy') 방향의 오프셋을 제어합니다.

가능한 값: 임의의 실수

기본값: 2

---

- 지정된 매개 변수의 값이 올바르면 dev_disp_text는 2(H_MSG_TRUE)를 반환합니다. 그렇지 않으면 예외가 발생하고 오류 코드가 반환됩니다.
- 예시

```jsx
dev_open_window (0, 0, 512, 512, 'black', WindowHandle)
dev_disp_text ('Display some text in a box', 'window', 12, 12, \
               'black', [], [])
```

## `dev_set_color`

- `dev_set_color( ColorName )`
- 하나 이상의 출력 색상을 설정합니다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| ColorName | input_control | string(-array) → (string) | 색상 이름을 출력합니다.

기본값: '흰색'
권장 값: 'white', 'black', 'gray', 'red', 'green', 'blue', '#003075', '#e53019', '#fb529' |
- dev_set_color는 그래픽 창에 영역, XLD 및 기타 기하학적 객체를 표시하는 데 사용되는 색상을 정의합니다. 사용 가능한 색상은 query_color 연산자로 쿼리할 수 있습니다. 또한 ColorName은 '#rrggbb' 및 '#rrggbaa' 형식으로 16진수 RGB 트리플렛 또는 RGBA 쿼드플렛으로 지정할 수 있습니다. '00', 'gg', 'bb', 'aa'는 각각 '00'과 'ff' 사이의 16진수 숫자입니다. 'aa'는 색상의 알파 값을 나타내며 투명한 영역을 표시하는 데 사용할 수 있습니다. 이러한 색상 설정은 dev_set_color 또는 dev_set_color를 호출하거나 색상 설정을 대화식으로 수정할 때까지 유효합니다.
- 지정된 매개 변수의 값이 올바르면 dev_set_color는 2(H_MSG_TRUE)를 반환합니다. 그렇지 않으면 예외가 발생하고 오류 코드가 반환됩니다.
- 예시

```jsx
read_image(Image,'mreut')
dev_set_draw('fill')
dev_set_color('red')
threshold(Image,Region,180,255)
dev_set_color('green')
threshold(Image,Region,0,179)
```

## `dev_display`

- `dev_display( Object )`
- 현재 그래픽 창에 이미지 개체를 표시합니다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Object | input_object | object(-array) → object | 표시할 이미지 개체 |
- dev_display는 활성 그래픽 창에 아이콘 객체(이미지, 영역 또는 XLD)를 표시합니다. 이는 변수 창 내부의 아이콘 변수를 두 번 클릭하는 것과 같습니다.
- 지정된 매개 변수의 값이 올바르면 dev_display는 2(H_MSG_TRUE)를 반환합니다. 그렇지 않으면 예외가 발생하고 오류 코드가 반환됩니다.
- 예시

```jsx
read_image (Image, 'fabrik')
regiongrowing (Image, Regions, 3, 3, 6, 100)
dev_clear_window ()
dev_display (Image)
dev_set_colored (12)
dev_set_draw ('margin')
dev_display (Regions)
```

## `gen_region_contour_xld`

- `gen_region_contour_xld( Contour , Region , Mode )`
- XLD 등고선에서 영역을 만듭니다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Contour | input_object | xld_cont(-array) → object | 입력 윤곽선(들) |
| Region | output_object | region(-array) → object | 만들어진 영역(들) |
| Mode | input_control | string → (string) | 영역의 채우기 모드.

기본값: '채움'
권장 값: '채움', '마진' |
- gen_region_contour_xld는 하위 픽셀 XLD 윤곽선에서 영역 영역을 생성합니다. 윤곽선은 브레센햄 알고리즘에 따라 샘플링되며 set_system 연산자의 매개 변수 'neighborhood'의 영향을 받는다. 열린 등고선은 영역으로 변환하기 전에 닫힙니다. 마지막으로, 파라미터 Mode는 영역이 채워졌는지('fill'), 윤곽선에 의해 반환되었는지('margin')을 정의합니다.                                                                                                                                              변환 중에 윤곽선 점의 좌표는 가장 가까운 정수 픽셀 좌표로 반올림됩니다. 이로 인해 연산자                                               gen_contour_region_xld에서 gen_region_contour_xld로 얻은 윤곽선을 전달할 때 예기치 않은 결과가 발생할 수 있습니다: gen_contour_region_xld의 Mode를 'margin'으로 설정하면 gen_contour_region_xld의 입력 영역과 gen_region_contour_xld의 출력 영역이 다릅니다. 예를 들어 gen_contour_region_xld의 입력 영역이 단일 픽셀(1,1)로 구성되어 있다고 가정합니다. 그런 다음 모드를 '경계'로 설정한 상태에서 gen_contour_region_xld를 호출할 때 얻는 결과 윤곽선은 5개의 점(0.5,0.5), (0.5,1.5), (1.5,0.5) 및 (0.5,0.5)으로 구성됩니다. 따라서 이 윤곽선을 gen_region_contour_xld로 다시 전달하면 결과 영역은 (1,1), (1,2), (2,2) 및 (2,1) 점으로 구성됩니다.

## `erosion_rectangle1`

- `erosion_rectangle1(Region , RegionErosion , Width , Height )`
- 직사각형 구조 요소가 있는 영역을 약화시킨다. (약화 -> 줄인다)
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Region | input_object | region(-array) → object | 약화시킬 영역. |
| RegionErosion | output_object | region(-array) → object | 약화시킨 영역. |
| Width | input_control | extent.x → (integer) | 구조 사각형의 너비입니다.

기본값: 11

권장 값: 1, 2, 3, 4, 5, 11, 15, 21, 31, 51, 71, 101, 151, 201
일반적인 값 범위: 1≤ 폭 ≤ 511(lin)
최소 증분: 1
권장 증가량: 1 |
| Height | input_control | extent.y → (integer) | 구조 사각형의 높이입니다.

기본값: 11

권장 값: 1, 2, 3, 4, 5, 11, 15, 21, 31, 51, 71, 101, 151, 201
일반적인 값 범위: 1 ≤ 높이 ≤ 511 (lin)
최소 증분: 1
권장 증가량: 1 |
- erosion_rectangle1은 입력 영역에 직사각형 구조 요소가 있는 erosion 을 적용합니다. 구조 사각형의 크기는 너비 x 높이다. 연산자는 영역을 줄이고 직사각형 마스크보다 작은 영역을 제거한다. erosion_filename1은 매우 빠른 연산인데, 왜냐하면 직사각형의 높이는 런타임 복잡도에 로그로만 입력되는 반면 너비는 전혀 입력되지 않기 때문이다. 이는 매우 큰 직사각형(에지 길이 > 100)의 경우에도 탁월한 런타임 효율성으로 이어진다. 큰 영역 사이의 작은 연결 스트립을 포함하는 영역은 외관상으로만 분리됩니다. 그들은 논리적으로 하나의 지역으로 남아있다.
- erosion_rectangle1은 모든 매개 변수가 올바른 경우 2(H_MSG_TRUE)를 반환한다. 입력 영역이 비어 있거나 없는 경우의 동작은 다음을 통해 설정할 수 있다:                                                                                                                             영역 없음: set_system('no_object_result', <RegionResult>)                                                                                    빈 영역: set_system('empty_region_result', <RegionResult>)                                                                                                 그렇지 않으면 예외가 발생한다.

    

## `union2`

- `union2( Region1 , Region2 , RegionUnion )`
- 두 지역의 결합을 반환한다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Region1 | input_object | region(-array) → object | Region2 의 모든 영역과의 결합을 계산할 영역. |
| Region2 | input_object | region(-array) → object | Region1 에 추가해야 하는 영역. |
| RegionUnion | output_object | region(-array) → object | 결과 영역.

요소 수 : RegionUnion == Region1 |
- union2는 Region1의 영역과 Region2의 모든 영역의 결합을 계산한다. 내부적으로 Region2 의 모든 지역은 individual regions of Region1 이 이미 통합된 Region과 통합되기 전에 united region으로 통합된다. 이것은 union2가 교환되지 않는다는 것을 의미한다.
- union2는 항상 2(H_MSG_TRUE)를 반환합니다. 빈 입력의 경우 동작(영역이 지정되지 않음)은 set_system('no_object_result', <Result>)을 통해 설정하고 빈 입력 영역의 경우 동작('empty_region_result', <Result>)을 통해 설정할 수 있습니다. 필요한 경우 예외가 발생합니다.

## `reduce_domain`

- `reduce_domain( Image , Region , ImageReduced )`
- 이미지의 domain을 축소한다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Image | input_object | (multichannel-)image(-array) → object (byte / direction / cyclic / int1 / int2 / uint2 / int4 / int8 / real / complex / vector_field) | 입력 이미지 |
| Region | input_object | region → object | 새롭게 정의된 domain |
| ImageReduced | output_object | image(-array) → object (byte / direction / cyclic / int1 / int2 / uint2 / int4 / int8 / real / complex / vector_field) | 축소된 정의 domain을 가지고 있는 이미지. |
- reduce_domain 연산자는 지정된 이미지의 정의 도메인을 지정된 영역으로 축소한다. 새 정의 도메인은 이전 정의 도메인과 영역의 교차점으로 계산된다. 따라서 새 정의 도메인은 영역의 하위 집합이 될 수 있다. 행렬의 크기는 변경되지 않는다.
- Image에서 Region 영역의 범위 만큼을 가지고 온다.

## `scale_image_max`

- `scale_image_max( Image , ImageScaleMax )`
- 값 범위 0 ~ 255에서 확산되는 최대 회색 값.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Image | input_object | (multichannel-)image(-array) → object (byte / int2 / uint2 / int4 / int8 / real) | 조정될 이미지 |
| ImageScaleMax | output_object | image(-array) → object (byte) | 향상된 이미지를 대조한 출력값 |
- scale_image_max 연산자는 최소값과 최대값을 계산하고 바이트 이미지의 최대값 범위로 이미지의 배율을 조정한다. 이러한 방식으로 역학(값 범위)을 최대한 활용할 수 있다. 다른 회색 눈금의 수는 변경되지 않지만 일반적으로 시각적 인상이 향상된다. 'real', 'int2', 'uint2', 'int4' 및 'int8' 유형의 영상의 회색 값은 0 ~ 255 범위로 조정되고 '바이트' 영상으로 반환된다.

## `closing_rectangle1`

- `closing_rectangle1(Region , RegionClosing , Width , Height )`
- 직사각형 구조 요소로 영역을 닫는다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Region | input_object | region(-array) → object | 닫힐 Regions |
| RegionClosing | output_object | region(-array) → object | 닫힌 Regions |
| Width | input_control | extent.x → (integer) | 구조 사각형의 너비.

기본값: 10
권장 값: 1, 2, 3, 4, 5, 7, 9, 12, 15, 19, 25, 33, 45, 60, 110, 150, 200
일반적인 값 범위: 1≤ 폭 ≤ 511(lin)
최소 증분: 1
권장 증가량: 1 |
| Height | input_control | extent.y → (integer) | 구조 사각형의 높이.

기본값: 10
권장 값: 1, 2, 3, 4, 5, 7, 9, 12, 15, 19, 25, 33, 45, 60, 110, 150, 200
일반적인 값 범위: 1 ≤ 높이 ≤ 511 (lin)
최소 증분: 1
권장 증가량: 1 |
- closing_rectangle1은 확장_rectangle1에 이어 입력 영역에서 roomation_rectangle1을 수행합니다. 직사각형 구조 요소의 크기는 너비 및 높이 매개변수에 의해 결정됩니다. 모든 닫힘 변형의 경우와 마찬가지로 영역의 경계가 평활되고 직사각형 구조 요소보다 작은 영역 내의 구멍이 닫힙니다.
- closing_rectangle1은 모든 매개 변수가 올바른 경우 2(H_MSG_TRUE)를 반환한다. 입력 영역이 비어 있거나 없는 경우의 동작은 다음을 통해 설정할 수 있다:                                                                                                            영역 없음: set_system('no_object_result', <RegionResult>)                                                                                          빈 영역: set_system('empty_region_result', <RegionResult>)                                                                                                                    그렇지 않으면 예외가 발생합니다.
- 이해를 돕기 위한 이미지
  


## `area_center`

- `area_center( Regions , Area , Row , Column )`
- 영역 및 Regions의 중심.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Regions | input_object | region(-array) → object | 검사할 영역 |
| Area | output_control | integer(-array) → (integer) | Region의 영역 |
| Row | output_control | point.y(-array) → (real) | 중심의 행 인덱스. |
| Column | output_control | point.x(-array) → (real) | 중심의 열 인덱스. |
- 연산자 area_center는 입력 영역의 면적과 중심을 계산한다. 영역은 영역의 픽셀 수로 정의된다. 중심은 모든 픽셀에 대해 각각 선 또는 열 좌표의 평균 값으로 계산된다.이 장의 설명서(지역/기능)에서 영역이 다양한 지역을 설명하는 이미지를 찾을 수 있다.                                                                                                               둘 이상의 영역을 통과하면 결과가 입력 영역의 인덱스에 해당하는 튜플의 값 인덱스인 튜플에 저장된다. 빈 영역의 경우 다른 동작이 설정되지 않은 경우 모든 매개 변수 값이 0.0이다(set_system 참조).
- 연산자 area_center는 입력이 비어 있지 않으면 값 2(H_MSG_TRUE)를 반환한다. 빈 입력(사용 가능한 입력 영역 없음)의 경우 동작은 연산자 set_system('no_object_result', <Result>)을 통해 설정된다. 빈 영역의 경우 동작(영역은 빈 집합)은 set_system('empty_region_result', <Result>)을 통해 설정된다. 필요한 경우 예외가 발생한다.
- 예시

```jsx
#include "HIOStream.h"
#if !defined(USE_IOSTREAM_H)
using namespace std;
#endif
#include "HalconCpp.h"
using namespace Halcon;

main()
{
  Tuple   area, row, column;

  HImage   img ("monkey");
  HWindow  w;

  img.Display (w);
  w.Click ();

  HRegionArray   reg = (img >= 164).Connection ();

  reg.Display (w);
  w.Click ();

  area = reg.AreaCenter (&row, &column);

  for (int i = 0; i < reg.Num (); i++)
  {
    cout << "Row    [" << i << "]" << "= " << row[i].D ();
    cout << "\t\tColumn [" << i << "]" << "= " << column[i].D () << endl;
  }

  cout << "Total number of regions: " << reg.Num () << endl;
  return(0);
}
```

## `select_shape`

- `select_shape(Regions , SelectedRegions , Features , Operation , Min , Max )`
- shape features 를 사용하여 영역을 선택한다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Regions | input_object | region-array → object | 검사할 영역. |
| SelectedRegions | output_object | region-array → object | 조건을 충족하는 지역. |
| Features | input_control | string(-array) → (string) | 검사할 shape features.

기본값: '영역'
값 목록: '비등식', '면적', '면적', '벌크함', '원형', '칼럼1', '칼럼2', '콤팩트함', '연결_num', '컨트렝스', '볼록함', 'dist_deviation', 'dist_mean', 'leer_number', '높이', 'inner_moments', 'moments', 'moments', 'moments', 'moments', 'moments', 'ines', 'ts_i4', 'slot_ia', 'lots_m02_invar', 'lots_m03_invar', 'lots_m11_invar', 'lots_m12_invar', 'lots_m20', 'lots_m20_invar', 'lots_m21', 'lots_m_invar_invar_invar_invar_invar_invar', 'lots_invar_inva_invar_m, 'rect_psi3', 'ret_psi4', 'num_factor', 'orientation', 'ret_filen', 'phi', 'ra', 'ret', 'rect2_len1', 'rect2_len2', 'rect2_phi', 'runcularity', 'roundness', 'row1', 'row2', 'row2', 'row_fact_fact_fact_fact_fact_fact', 'wident', 'wid |
| Operation | input_control | string → (string) | 개별 feature의 연결 유형입니다.

기본값: 'and'
값 목록: 'and', 'or' |
| Min | input_control | real(-array) → (real / integer / string) | shape의 하한 또는 '최소'.

기본값: 150.0
일반적인 값 범위: 0.0 ≤ 최소 ≤ 9999.0
최소 증분: 0.001
권장 증분: 1.0 |
| Max | input_control | real(-array) → (real / integer / string) | shape의 상한 또는 '최대'.

기본값: 9999.0
일반적인 값 범위: 0.0 ≤ Max ≤ 9999.0
최소 증분: 0.001
권장 증분: 1.0
제한: 최대 >= 최소 |
- select_shape 연산자는 모양에 따라 영역을 선택한다. 영역의 각 입력 영역에 대해 표시된 형상(Features)이 계산된다. 계산된 각 기능(작동 = 'and') 또는 적어도 하나(작동 = 'or')가 기본 한계(최소, 최대) 내에 있으면 영역이 출력에 적용된다(복사됨). 파라미터 Min 및 Max를 각각 'min' 또는 'max'로 설정하여 하한 및 상한을 개방 상태로 유지할 수 있다.
- 연산자 select_shape는 입력이 비어 있지 않으면 값 2(H_MSG_TRUE)를 반환한다. 빈 입력(사용 가능한 입력 개체가 없음)의 경우 동작은 연산자 set_system('no_object_result', <Result>)을 통해 설정된다. 빈 영역의 경우 동작(영역은 빈 집합)은 set_system('empty_region_result', <Result>)을 통해 설정된다. 필요한 경우 예외가 발생한다.
- 예시

```jsx
* Where are the eyes of the Mandrill?
read_image(Image,'monkey')
threshold(Image,Region,128,255)
connection(Region,ConnectedRegions)
select_shape(ConnectedRegions,Eyes,['area','max_diameter'],\
             'and',[500,30.0],[1000,50.0])
dev_display(Eyes)
```

## `obj_diff`

- `obj_diff(Objects , ObjectsSub , ObjectsDiff )`
- 두 개체 튜플의 차이를 계산한다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Objects | input_object | object(-array) → object | Object tuple 1 |
| ObjectsSub | input_object | object(-array) → object | Object tuple 2 |
| ObjectsDiff | output_object | object(-array) → object | ObjectsSub의 일부가 아닌 Objects의 Objects 이다. |
- obj_tuples는 두 객체 튜플의 집합론적 차이를 계산한다:                                                                                                  (개체의 개체) - (개체 하위의 개체)                                                                                                                                    결과 개체 튜플 ObjectsDiff는 ObjectsSub에서 모든 개체가 제거된 입력 튜플 개체로 정의된다.
- obj_diff는 항상 2(H_MSG_TRUE)를 반환한다.

## `count_obj`

- `count_obj(Objects , Number)`
- 튜플의 개체 수.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Objects | input_object | object-array → object | 검사할 개체 |
| Number | output_control | integer → (integer) | 튜플 Objects 의 Object 수 |
- 연산자 count_obj는 객체 매개 변수 Objects에 포함된 객체 수를 결정한다. 이 연결에서는 개체가 연결 구성 요소와 같지 않다는 점에 유의해야 한다(연결 참조). 예를 들어, 연결되지 않은 세 부분으로 구성된 한 영역의 객체 수는 1이다.
- count_obj는 값 2(H_MSG_TRUE)를 반환한다.

## `gen_measure_rectangle2`

- `gen_measure_rectangle2( Row , Column , Phi , Length1 , Length2 , Width , Height , Interpolation , MeasureHandle )`
- 직사각형에 수직인 직선 모서리의 추출을 준비한다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Row | input_control | rectangle2.center.y → (real / integer) | 사각형 중심의 행 좌표.

기본값: 300.0
권장 값: 10.0, 20.0, 50.0, 100.0, 200.0, 300.0, 400.0, 500.0
일반적인 값 범위: 0.0 ≤ 행 ≤ 511.0 (lin)
최소 증분: 1.0
권장 증분: 10.0 |
| Column | input_control | rectangle2.center.x → (real / integer) | 사각형 중심의 열 좌표.

기본값: 200.0
권장 값: 10.0, 20.0, 50.0, 100.0, 200.0, 300.0, 400.0, 500.0
일반적인 값 범위: 0.0 ≤ 열 ≤ 511.0 (lin)
최소 증분: 1.0
권장 증분: 10.0 |
| Phi | input_control | rectangle2.angle.rad → (real / integer) | 직사각형의 세로 축이 수평(라디안)으로 향하는 각도.

기본값: 0.0
권장 값: -1.178097, -0.785398, -0.392699, 0.0, 0.392699, 0.785398, 1.178097
일반적인 값 범위: -1.178097 ≤ Phi ≤ 1.178097 (lin)
최소 증분: 0.001
권장 증분: 0.1                                    제한사항: pi < Phi && Phi <= pi                                    |
| Length1 | input_control | rectangle2.hwidth → (real / integer) | 직사각형의 절반 너비.

기본값: 100.0
권장 값: 3.0, 5.0, 10.0, 15.0, 20.0, 50.0, 100.0, 200.0, 300.0, 500.0
일반적인 값 범위: 1.0 ≤ 길이 1 ≤ 511.0 (lin)
최소 증분: 1.0
권장 증분: 10.0
제한: 길이 1 >= 1.0 |
| Length2 | input_control | rectangle2.hheight → (real / integer) | 직사각형의 절반 높이.

기본값: 20.0
권장 값: 1.0, 2.0, 3.0, 5.0, 10.0, 15.0, 20.0, 50.0, 100.0, 200.0
일반적인 값 범위: 0.0 ≤ 길이 2 ≤ 511.0 (lin)
최소 증분: 1.0
권장 증분: 10.0
제한: 길이 2 >= 0.0 |
| Width | input_control | extent.x → (integer) | 이후에 처리할 이미지의 너비.

기본값: 512
권장 값: 128, 160, 192, 256, 320, 384, 512, 640, 768
일반적인 값 범위: 0 ≤ 폭 ≤ 1024 (lin)
최소 증분: 1
권장 증가량: 16 |
| Height | input_control | extent.y → (integer) | 이후에 처리할 이미지의 높이.

기본값: 512
권장 값: 120, 128, 144, 240, 256, 288, 480, 512, 576
일반적인 값 범위: 0 ≤ 높이 ≤ 1024 (lin)
최소 증분: 1
권장 증가량: 16 |
| Interpolation | input_control | string → (string) | 사용할 보간 유형.

기본값: 'nearest_neighbor'
값 목록: 'bicubic', 'bilinar', 'nearest_neighbor' |
| MeasureHandle | output_control | measure → (handle) | 측정 객체 핸들 |

- gen_measure_tft2는 직사각형의 장축에 수직인 직선 모서리의 추출을 준비한다. 직사각형의 중심은 Row와 Column 매개변수, Phi에서 직사각형의 장축 방향, Length1과 Length2에서 두 축의 길이, 즉 직사각형의 직경의 절반을 지난다.
    - 에지 추출 알고리즘은 연산자 measure_pos의 설명서에 설명되어 있습니다. 여기서 논의된 바와 같이, 1차원 회색 값 프로파일의 계산에 다른 유형의 보간법을 사용할 수 있다. 보간 = 'detail_details'의 경우 측정의 회색 값은 가장 가까운 픽셀의 회색 값, 즉 일정한 보간을 통해 얻습니다. 보간 = '이중선형'의 경우 이중선형 보간이 사용되고, 보간 = '이중 입방형'의 경우 이중 입방형 보간이 사용된다.
    - 최적의 속도로 실제 측정을 수행하려면 여러 측정에 사용할 수 있는 모든 계산이 이미 gen_measure_rectangle2 연산자에서 수행된다. 이를 위해 MeasureHandle에서 최적화된 데이터 구조, 이른바 Measure 객체를 구성하여 반환한다. 측정을 수행할 영상의 크기는 Width(폭) 및 Height(높이) 파라미터에 지정해야 한다.
    - 시스템 파라미터 'int_zooming'(set_system 참조)은 측정 객체를 구성하는 데 사용되는 계산의 정확도와 속도에 영향을 미친다. 'int_zooming'을 'true'로 설정하면 고정점 산술을 사용하여 내부 계산이 수행되므로 실행 시간이 훨씬 짧아진다. 그러나 이 모드에서는 기하학적 정확도가 약간 낮다. 'int_zooming'을 'false'로 설정하면 부동소수점 연산을 사용하여 내부 계산이 수행되어 기하학적 정확도가 극대화되지만 실행 시간도 크게 증가한다.
- 파라미터 값이 올바르면 연산자 gen_measure_rectangle2가 값 2(H_MSG_TRUE)를 반환한다. 그렇지 않으면 예외가 발생한다.

## `measure_pairs`

- `measure_pairs(Image , MeasureHandle, Sigma, Threshold, Transition, Select , RowEdgeFirst, ColumnEdgeFirst, AmplitudeFirst, RowEdgeSecond, ColumnEdgeSecond, AmplitudeSecond, IntraDistance, InterDistance)`
- 직사각형 또는 고리모양 호에 수직인 직선 모서리 쌍을 추출한다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Image | input_object | singlechannelimage → object (byte / uint2 / real) | 입력 이미지 |
| MeasureHandle | input_control | measure → (handle) | 측정 객체 핸들 |
| Sigma | input_control | number → (real) | 가우스 평활의 시그마.

기본값: 1.0
권장 값: 0.4, 0.6, 0.8, 1.0, 1.5, 2.0, 3.0, 4.0, 5.0, 7.0, 10.0
일반적인 값 범위: 0.4 ≤ Sigma ≤ 100 (lin)
최소 증분: 0.01
권장 증분: 0.1
제한: 시그마 >= 0.4 |
| Threshold | input_control | number → (real) | 최소 edge amplitude.

기본값: 30.0
권장 값: 5.0, 10.0, 20.0, 30.0, 40.0, 50.0, 60.0, 70.0, 90.0, 110.0
일반적인 값 범위: 1≤ 임계값 ≤ 255(lin)
최소 증분: 0.5
권장 증가량: 2 |
| Transition | input_control | string → (string) | 모서리가 모서리 쌍으로 그룹화되는 방법을 결정하는 회색 값 전환 유형.

기본값: 'all'
값 목록: 'all', 'all_strongest', 'negative', 'negative_strongest', 'positive', 'positive_strongest' |
| Select | input_control | string → (string) | 가장자리 쌍 선택.

기본값: 'all'
값 목록: '모두', '첫 번째', '마지막' |
| RowEdgeFirst | output_control | point.y-array → (real) | 첫 번째 가장자리 중심의 행 좌표. |
| ColumnEdgeFirst | output_control | point.x-array → (real) | 첫 번째 가장자리 중심의 열 좌표. |
| AmplitudeFirst | output_control | real-array → (real) | 첫 번째 에지의 에지 진폭(부호 포함). |
| RowEdgeSecond | output_control | point.y-array → (real) | 두 번째 가장자리 중심의 행 좌표. |
| ColumnEdgeSecond | output_control | point.x-array → (real) | 두 번째 모서리의 중심에 대한 열 좌표. |
| AmplitudeSecond | output_control | real-array → (real) | 두 번째 에지의 에지 진폭(부호 포함). |
| IntraDistance | output_control | real-array → (real) | 모서리 쌍의 모서리 사이의 거리. |
| InterDistance | output_control | real-array → (real) | 연속된 에지 쌍 사이의 거리. |
- measure_discolor는 직사각형이나 고리형 호의 장축에 수직인 직선 모서리 쌍을 추출하는 역할을 한다.
    - measure_pairs의 추출 알고리즘은 measure_pos와 동일합니다. 또한 에지는 쌍으로 그룹화됩니다. 전환 = '양'인 경우 직사각형의 장축 방향으로 어둡게 빛으로 전환된 에지 포인트가 RowEdgeFirst 및 ColumnEdgeFirst에 반환됩니다. 이 경우 밝은 색에서 어두운 색으로 전환된 해당 에지는 RowEdgeSecond 및 ColumnEdgeSecond로 반환됩니다. 전환 = '음'이면 동작이 정확히 반대입니다. Transition = 'all'인 경우 첫 번째로 검출된 에지는 RowEdgeFirst 및 ColumnEdgeFirst의 전환을 정의합니다. 즉, 측정 물체의 위치에 따라 밝은-어두운-빛 전환을 가진 가장자리 쌍 또는 어두운-빛-빛-빛 전환을 가진 가장자리 쌍이 반환된다. 이는 예를 들어 배경과 관련된 밝기가 다른 물체를 측정하는 데 적합하다.
    - 동일한 전환을 가진 연속된 에지가 두 개 이상 발견되면 첫 번째 에지가 쌍 요소로 사용됩니다. 이 동작은 동일한 전환의 연속적인 에지를 억제할 수 있을 만큼 충분히 높은 임계값을 선택할 수 없는 응용 프로그램에서 문제를 발생시킬 수 있습니다. 이러한 애플리케이션의 경우 연속적인 상승 및 하강 에지 시퀀스의 각각의 가장 강한 에지만 선택하는 두 번째 페어링 모드가 존재합니다. 이 모드는 위의 전환 모드(예: 'negative_strongest')에 '_strongest'를 추가하여 선택합니다. 마지막으로 반환되는 에지 쌍을 선택할 수 있습니다. Select를 'all'로 설정하면 모든 에지 쌍이 반환됩니다. 'first'로 설정된 경우 추출된 에지 쌍 중 첫 번째 에지 쌍만 반환되고 'last'로 설정된 경우 마지막 에지 쌍만 반환됩니다.
    - 추출된 모서리는 직사각형의 장축에 있는 단일 점으로 반환됩니다. 해당 에지 진폭은 Amplitude First(진폭 우선) 및 Amplitude Second(진폭 초)로 반환됩니다. 또한 각 에지 쌍 간의 거리는 IntraDistance로 반환되고 연속된 에지 쌍 간의 거리는 InterDistance로 반환됩니다. 여기서 IntraDistance[i]는 EdgeFirst[i]와 EdgeSecond[i] 사이의 거리에 해당하는 반면 InterDistance[i]는 EdgeSecond[i]와 EdgeFirst[i+1] 사이의 거리에 해당합니다. 즉, 튜플 InterDistance는 에지 쌍의 튜플보다 하나 적은 요소를 포함합니다.
- 매개 변수 값이 올바르면 measure_pairs 연산자는 값 2(H_MSG_TRUE)를 반환합니다. 그렇지 않으면 예외가 발생한다.

## `tuple_concat`

- `tuple_concat( T1 , T2 , Concat )`
- 두 튜플을 새 튜플에 연결.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| T1 | input_control | tuple(-array) → (integer / real / string) | 입력 tuple 1 |
| T2 | input_control | tuple(-array) → (integer / real / string) | 입력 tuple 2 |
| Concat | output_control | tuple(-array) → (integer / real / string) | 입력 튜플의 연결. |
- tuple_concat는 입력 튜플 T1과 T2를 새 튜플 Concat에 연결한다. 콩카트의 첫 번째 원소는 T1의 원소와 일치하고 나머지 콩카트의 원소는 T2의 원소와 일치한다.

## `concat_obj`

- `concat_obj( Objects1 , Objects2 , ObjectsConcat )`
- 두 개의 아이코닉 개체 튜플을 연결한다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Objects1 | input_object | object(-array) → object | 객체 tuple 1 |
| Objects2 | input_object | object(-array) → object | 객체 tuple 2 |
| ObjectsConcat | output_object | object-array → object | Concatenated Objects |
- concat_obj는 두 개의 상징적 객체 Objects1과 Objects2를 새로운 상징적 객체 Concat으로 연결한다. 따라서 이 튜플은 두 개의 입력 튜플의 모든 아이콘 객체를 포함한다:
    - 객체 일치 = [Objects1,Objects2]
    - ObjectsConcat에서는 Objects1의 객체가 먼저 저장된 후 Objects2의 객체, 즉 객체의 순서가 보존된다. 해당 이미지 및 영역에 대한 참조만 ObjectsConcat에 저장됩니다. 즉, 새 메모리가 할당되지 않습니다. 또한, 이는 set_grayval, overpaint_gray 또는 overpaint_region과 같은 입력 이미지의 수정이 출력 튜플 ObjectsConcat의 이미지에 직접적으로 영향을 미치고 그 반대의 경우도 마찬가지임을 의미한다.
    - concat_obj는 영역이 병합되는 union1이나 union2, 즉 객체의 수가 수정되는 union2와 혼동되어서는 안 된다.
    - concat_obj는 서로 다른 이미지 개체 유형(예: 이미지 및 XLD 윤곽선)의 개체를 단일 개체로 연결하는 데 사용할 수 있습니다. 예를 들어, 이미지 처리 시퀀스의 결과와 같이 단일 개체 변수에 누적해야 하는 경우에만 이 방법을 사용하는 것이 좋습니다. 이러한 혼합형 객체 튜플을 처리할 수 있는 연산자는 concat_obj, copy_obj, select_obj, disp_obj뿐이라는 점에 유의해야 한다.
- concat_obj는 모든 개체가 HALCON 데이터베이스에 포함된 경우 2(H_MSG_TRUE)를 반환한다. 입력이 비어 있으면 set_system(: 'no_object_result', <Result>:)을 통해 동작을 설정할 수 있다. 필요한 경우 예외가 발생한다.
- 예시

```jsx
/* generate a tuple of a circle and a rectangle */

gen_circle(&Circle,200.0,400.0,23.0);
gen_rectangle1(&Rectangle,23.0,44.0,203.0,201.0);
concat_obj(Circle,Rectangle,&CirclAndRectangle);
disp_region(CircleAndRectangle,WindowHandle);
```

## `gen_cross_contour_xld`

- `gen_cross_contour_xld( Cross , Row, Col , Size , Angle )`
- 각 입력점에 대해 XLD 윤곽선을 십자 모양으로 하나 생성한다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Cross | output_object | xld_cont(-array) → object | 생성된 XLD contours |
| Row | input_control | point.y(-array) → (real / integer) | 입력점의 행 좌표 |
| Col | input_control | point.x(-array) → (real / integer) | 입력점의 열 좌표.

제한사항: 번호(콜) == 번호(행) |
| Size | input_control | number → (real / integer) | 가로 막대의 길이.

기본값: 6.0
권장 값: 4.0, 6.0, 8.0, 10.0
제한사항 : 0.0 <= 사이즈 |
| Angle | input_control | angle.rad → (real) | 십자가의 방향.

기본값: 0.785398
권장 값: 0.0, 0.785398 |
- gen_cross_contour_xld는 각 입력점(행, 열)에 대해 십자 모양의 XLD 등고선을 생성한다. 개념적으로 윤곽선은 입력 지점에서 정확히 교차하는 두 개의 길이 크기 선으로 구성된다. 각도에 따라 방향이 결정된다. 십자가는 십자가로 반환된다. 처리할 점이 여러 개인 경우 좌표를 튜플로 전달해야 한다.
- gen_cross_contour_xld는 모든 매개 변수가 올바르고 실행 중에 오류가 발생하지 않으면 2(H_MSG_TRUE)를 반환한다.

## `gen_arrow_contour_xld`

- `gen_arrow_contour_xld(Arrow, Row1, Column1, Row2, Column2, HeadLength, HeadWidth)`
- 각 입력점에 대해 화살표 모양의 XLD 등고선을 생성합니다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Arrow | output_object | xld_cont(-array) → object | 생성된 XLD contours |
| Row1 | input_control | point.y(-array) → (real / integer) | 입력점의 행 좌표 |
| Column1 | input_control | point.x(-array) → (real / integer) | 입력점의 열 좌표.

제한사항: 번호(콜) == 번호(행) |
| Row2 | input_control | point.y(-array) → (integer / real) | 가로 막대의 길이.

기본값: 6.0
권장 값: 4.0, 6.0, 8.0, 10.0
제한사항 : 0.0 <= 사이즈 |
| Column2 | input_control | point.x(-array) → (integer / real) | 십자가의 방향.

기본값: 0.785398
권장 값: 0.0, 0.785398 |
| HeadLength | input_control | number(-array) → (integer / real) | 화살표 머리 길이(픽셀)

기본값:5

추천하는 값들: [2,3,5,10,20] |
| HeadWidth | input_control | number(-array) → (integer / real) | 화살표 머리 너비(픽셀)

기본값: 5

추천하는 값들: [2,3,5,10,20] |
- 시작점과 끝점이 동일한 경우 단일 점으로 구성된 윤곽선이 반환됩니다.
- 입력 튜플 행 1, 열 1, 행 2 및 열 2는 길이가 같아야 합니다. 머리 길이 및 머리 너비는 행 1, 열 1, 행 2 및 열 2와 길이가 같거나 단일 요소여야 합니다. 위의 제한 사항 중 하나를 위반하면 오류가 발생합니다.
- 예시

```jsx
StartPointRows:=[100,100]
StartPointColumns:=[100,100]
EndPointRows:=[200,50]
EndPointColumns:=[200,150]
dev_set_colored (3)
gen_arrow_contour_xld (Arrow, StartPointRows, StartPointColumns, EndPointRows, EndPointColumns, [10,20], [20,10])
```

## `clear_metrology_object`

- `clear_metrology_object( MetrologyHandle , Index )`
- 메트릭 개체를 삭제하고 할당된 메모리를 확보한다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| MetrologyHandle | input_control, state is modified | metrology_model → (handle) | metrology model에 대한 핸들 |
| Index | input_control | integer(-array) → (string / integer) | 메트릭 개체의 인덱스.

기본값: 'all'
권장 값: '모두', 0, 1, 2 |
- clear_metrology_object는 예를 들어, add_metrology_object_object_ellipse_measure, add_metrology_object_line_measure 또는 add_metrology_object_object_mote2_measure를 통해 생성된 도량형 모델 개체에서 삭제한다.
    - 도량형 개체에서 사용하는 모든 메모리가 해제된다. 도량형 모델의 핸들은 도량형 핸들에서 전달된다. 도량형 객체의 색인은 색인에서 전달된다. 색인을 '모두'로 설정하면 모든 도량형 개체가 삭제된다. 조작자가 미터법 개체를 호출한 후에는 유효하지 않는다.
- 매개 변수가 유효하면 연산자 clear_metrology_object는 값 2(H_MSG_TRUE)를 반환한다. 필요한 경우 예외가 발생한다.

## `close_measure`

- `close_measure( MeasureHandle )`
- measure object를 삭제합니다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| MeasureHandle | input_control, state is modified | measure → (handle) | Measure object handle |
- close_measure는 MeasureHandle에서 지정한 측정 개체를 삭제한다. 이로서 측정 개체에 사용된 메모리가 확보되었다.
- 매개 변수 값이 올바르면 연산자 close_measure가 값 2(H_MSG_TRUE)를 반환한다. 그렇지 않으면 예외가 발생한다.

## `stop`

- `stop()`
- 프로그램 실행을 중지한다.
- 매개변수 X
- 중지 연산자는 HDevelop 프로그램의 연속적인 프로그램 실행을 중지한다. 이 경우 PC는 수많은 코멘트나 다른 실행 불가능한 프로그램 라인이 뒤따르더라도 프로그램 중단의 이유를 직접 보여주기 위해 (다음 실행 프로그램 라인에 배치되는 대신) stop 문에 남아 있다.
    - 연산자는 메뉴 모음의 Stop(중지) 동작(F9)에 해당한다. (par_start 한정자를 통해) 병렬 실행을 사용하지 않는 한, 실행 작업(F5)으로 프로그램을 쉽게 계속할 수 있다. HD 개발 사용 설명서의 "병렬 실행"도 참조하라.
    - 기본 설정 대화 상자에서 시간 매개 변수를 설정하여 동작을 재정의할 수 있다. 이 경우 실행은 중지되지 않고 지정된 시간 동안 기다린 후 계속된다. 이 시간 내에 F9로 프로그램을 중단하거나 실행 명령 중 하나로 계속할 수 있다. 이것은 프로그램 창의 첫 번째 열에 아이콘으로 표시된다.
- 프로그램이 Stop 문에서 중지되면 이전 연산자의 반환 상태가 유지된다. 프로그램이 중지 연산자를 사용하여 계속되면 중지는 항상 2(H_MSG_TRUE)를 반환한다.
- 예시

```jsx
read_image (Image, 'fabrik')
regiongrowing (Image, Regions, 3, 3, 6, 100)
count_obj (Regions, Number)
dev_update_window ('off')
for i := 1 to Number by 1
  select_obj (Regions, RegionSelected, i)
  dev_clear_window ()
  dev_display (RegionSelected)
  stop ()
endfor
```

## `deg`

- `deg(phi)`
- radians to degrees ( 평면 각을 도로 바꿉니다)
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| phi | input_control |  | phi 값 |
- f1으로 설명이 나오지 않아 수작업
- 예시

```jsx
PK_Phi := 1.5708
PackagePhi := deg(PK_Phi)$'.2f' // .2f의 의미는 소수점의 2번째 자리 까지만 표시하라
// PackagePhi 값은 '90.00'
```

## `min`

- `min(Tuple)`
- 튜플에서 가장 작은 값을 리턴 합니다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Tuple | input_control | tuple | 튜플 형식의 변수 |
- f1으로 설명이 나오지 않아 수작업
- 예시

```jsx
Length := [131,121,15,71]
minlength := min(Length)
// minlength 값은 15
```

## `max`

- `max(Tuple)`
- 튜플에서 가장 큰 값을 리턴 합니다.
- 매개변수

| 변수명 | 입출력 | 타입 | 설명 |
| --- | --- | --- | --- |
| Tuple | input_control | tuple | 튜플 형식의 변수 |
- f1으로 설명이 나오지 않아 수작업
- 예시

```jsx
Length := [131,121,15,71]
maxlength := max(Length)
// maxlength 값은 131
```