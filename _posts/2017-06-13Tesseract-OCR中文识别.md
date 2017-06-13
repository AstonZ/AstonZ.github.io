# Tesseract 

## 集成过程

## 集成官方教程地址：https://github.com/gali8/Tesseract-OCR-iOS/wiki/Installation
## 官方Demo Tesseract-OCR-iOS-master  貌似并不能编译

## 注意点：
### 1.  新建工程 pod 'TesseractOCRiOS', '4.0.0'
官方Demo中是直接引用工程文件，教程中有这行。
### 2. 编译设置 bitcode = NO
### 3. 教程提到的libz.dylib 没有找到，那就添加libz.tbd正常应该编译通过了。

### 4. 把 Tesseract-OCR-iOS-master 的ViewController 和main.storybord的内容拷贝过来
设置自己的开发者证书，可以真机跑了。
```
 import <TesseractOCR/TesseractOCR.h>
```
### 5.英文语言包
把官方Demo的英文包拿来用了，速度杠杠的。

### 6.中文语言包 就比较曲折了

官方教程语言包下载地址：https://code.google.com/p/tesseract-ocr/downloads/list
是不存在的。

又去地址：https://github.com/tesseract-ocr/langdata
简体中文选择：chi_sim，点开之后有10个文件，
无法单独下载，只能一个个下载，或者所有语言一起下载。一共200M+，其中chi_sim50M+
然后把包放进去运行，开始识别会崩溃。

```
actual_tessdata_num_entries_ <= TESSDATA_NUM_ENTRIES:Error:Assert failed:in file tessdatamanager.cpp, line 53 
```
无奈，包版本不一致喽。继续寻找可用语言包。

然后根据帖子：http://www.cocoachina.com/bbs/read.php?tid-123463-page-1.html
去网盘下载了：https://pan.baidu.com/share/link?shareid=391619&uk=3742563016&errno=0&errmsg=Auth%20Login%20Sucess&&bduss=&ssnerror=0 
18M+。 感觉不是很靠谱。

结果很惨烈
原始图片：
![中文识别对象](/resources/20170613OCR/chixt.png)
  
![识别结果](/reousrces/201706113OCR/chitxt_result.png)

### 6. 识别结果处理
把Tesseract-OCR-iOS-master 用的图片sample-image拷贝到项目中。
运行可以识别。

识别结果保存在G8Tesseract 对象中，
如果要获取单纯文本，直接 tesseract.recognizedText 即可。

如果想要获取更多信息，识别完成之后会生成一段HTML，可以这样获取
NSString *regString = [tesseract recognizedHOCRForPageNumber:0];
·																					
包含位置信息，一个词是一个div，大致如下:

```
  <div class='ocr_page' id='page_1' title='image ""; bbox 0 0 1121 156; ppageno 0'>
   <div class='ocr_carea' id='block_1_1' title="bbox 32 20 1091 148">
    <p class='ocr_par' dir='ltr' id='par_1_1' title="bbox 32 20 1091 148">
     <span class='ocr_line' id='line_1_1' title="bbox 32 20 1091 148; baseline -0.008 0">
      <span class='ocrx_word' id='word_1_1' title='bbox 32 20 1091 148; x_wconf 77' lang='eng' dir='ltr'>1234567890</span> 
     </span>
    </p>
   </div>
  </div>
  
```



## Case 手机拍摄取像 

崩溃：
```
CGImageCreate: invalid image bits/pixel or bytes/row.
Jun 13 14:42:27  OCR-Google[1548] <Error>: CGImageCreate: invalid image bits/pixel or bytes/row.
actual_tessdata_num_entries_ <= TESSDATA_NUM_ENTRIES:Error:Assert failed:in file tessdatamanager.cpp, line 53

```


