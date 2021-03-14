#  目录
- [目录](#目录)
- [自动化办公_PDF篇](#自动化办公_pdf篇)
  - [提取pdf中的文字和表格](#提取pdf中的文字和表格)
    - [pdfplumber 库](#pdfplumber-库)
    - [实例: 提取文字](#实例-提取文字)
    - [实例: 提取表格](#实例-提取表格)
  - [拆分/合并/添加水印/密码](#拆分合并添加水印密码)
    - [PdfFileReader,PdfFileWriter库](#pdffilereaderpdffilewriter库)
    - [实例: 拆分pdf](#实例-拆分pdf)
    - [实例: 合并pdf](#实例-合并pdf)
    - [实例: 加水印](#实例-加水印)
  - [提取pdf中的所有图片](#提取pdf中的所有图片)
  - [批量将word转pdf](#批量将word转pdf)


# 自动化办公_PDF篇
导入库:
import pdfplumber
import PyPDF2
import os
import pandas as pd

准备路径:
path = "C:/Users/Minyan/Desktop/py/Auto/pdf/练习文件"
os. chdir(path)

## 提取pdf中的文字和表格
### pdfplumber 库
按页来处理 pdf 的，可以获得页面的所有文字，并且提供的单独的方法用于提取表格，对于合并单元格等提取也存在问题。
*  `with pdfplumber.open(path) as pdf`  打开
* pdf.pages  返回列表,例如:\[<Page:1>, <Page:2>, <Page:3>\]
* page.extract_text()  提取某页的文字
* page.extract_tables()  提取某页的所有表格.返回列表
### 实例: 提取文字
```py
with pdfplumber.open('文字.pdf') as pdf:
    first_page = pdf.pages[0]
    text = first_page.extract_text()
    f = open('文字.txt',mode='w')
    f.write(text)
    f.close()
```
### 实例: 提取表格
```py
count = 1
with pdfplumber.open('表1.pdf') as pdf:
    with pd.ExcelWriter('表1.xlsx') as writer:
        for page in pdf.pages:
            for table in page.extract_tables():
                data = pd.DataFrame(table[1:], columns=table[0])
                data = data.set_index('姓名')  # 修改index为'姓名'
                data['年龄'] = pd.to_numeric(data['年龄'])  # 把'年龄'改为数值类型,默认为string
                print(data)
                data.to_excel(writer, sheet_name=f'sheet_{count}')
                count += 1
```

## 拆分/合并/添加水印/密码
### PdfFileReader,PdfFileWriter库
from PyPDF2 import PdfFileReader,PdfFileWriter
* `PdfFileReader.getNumPages(self)`: 计算页数.Calculates the number of pages in this PDF file. return number of pages
* `PdfFileReader.getPage(self, pageNumber)`:  提取页面.Retrieves a page by number from this PDF file.
* `PdfFileWriter.addPage(self,page)`: 增加页.Adds a page to this PDF file.  The page is usually acquired from a class:`PdfFileReader<PdfFileReader>` instance.
* `PdfFileWriter.appendPagesFromReader(self, reader)`:增加全部读取页.Copy pages from reader to writer.
* `page_Retrieved.rotateClockwise(angle)`:旋转页面. method of PyPDF2.pdf.PageObject instance Rotates a page clockwise by increments of 90 degrees. 同理rotateCounterClockwise()
* `PageObject1.mergePage(PageObject2)`: 合并页. PageObject2's content will be "on top" of the PageObject1.
* `PdfFileWriter.encrypt(self, user_pwd, owner_pwd=None)`: 加密.默认owner password等于user password
* `PdfFileReader.decrypt(password)`: 输入密码

### 实例: 拆分pdf
```py
from PyPDF2 import PdfFileReader,PdfFileWriter 
PDFreader = PdfFileReader('表格.pdf')
num_pages = PDFreader.getNumPages()
for page in range(num_pages): #  range(0, num_pages, 2) 奇数页; range(num_pages-1,-1,-1)倒序页
    PDFwriter = PdfFileWriter()  # 实例化对象
    page_Retrieved = PDFreader.getPage(page)
    PDFwriter.addPage(PDFreader.getPage(page)) # 将遍历出的每一页添加到实例化对象中
    with open(f'{page+1}.pdf', "wb") as pdf:  # wb 写入数据流
        PDFwriter.write(pdf)
```
### 实例: 合并pdf
```py
PDFwriter = PdfFileWriter()
for filename in range(1,4):
    PDFreader = PdfFileReader(f'{filename}.pdf') # 循环读取每一个PDF
    PDFwriter.appendPagesFromReader(PDFreader)  # 
with open("合并后的.pdf", "wb") as pdf:
    PDFwriter.write(pdf)
```
    
### 实例: 加水印
```py
from copy import copy 
wm = PdfFileReader("水印_一休猫.pdf").getPage(0) # 读取水印pdf,提取出第一页
PDFreader = PdfFileReader("笔记.pdf") # 读要添加水印的文件
PDFwriter = PdfFileWriter()  # 实例化对象
num_pages = PDFreader.getNumPages()
for page in range(num_pages):
    page_Retrieved = PDFreader.getPage(page)
    wm_page = copy(wm)  # 用浅拷贝,不能直接引用拷贝
    wm_page.mergePage(page_Retrieved)  # 提取出来的文件页,加在水印页上方
    PDFwriter.addPage(wm_page)  # 写入
with open("带一休猫水印的笔记.pdf", "wb") as pdf:
    PDFwriter.write(pdf)
```

## 提取pdf中的所有图片
```py
import fitz

def find_imag(pdf_path, img_path):
    doc = fitz.open(pdf_path)
    for i in range(len(doc)):
        count = 0
        for img in doc.getPageImageList(i):
            count += 1
            pix = fitz.Pixmap(doc, img[0])
            dest_file = img_path + "/p%s-%s.png" % (i, count)
            if pix.n < 5:       # this is GRAY or RGB
                pix.writePNG(dest_file)
            else:               # CMYK: convert to RGB first
                pix1 = fitz.Pixmap(fitz.csRGB, pix)
                pix1.writePNG(dest_file)
                pix1 = None
            pix = None

pdf_path = '笔记.pdf'
img_path = '图片'
if os.path.exists(img_path):
    print("文件夹已存在，请重新创建新文件夹！")
    raise SystemExit
else:
    os.mkdir(img_path)
    print(img_path + '/1.png')
m = find_imag(pdf_path, img_path)
```

## 批量将word转pdf
```py
import sys
import os
import comtypes.client

def word2pdf(doc_name,pdf_name):
	in_file=doc_name
	out_file=pdf_name
	# create COM object
	word=comtypes.client.CreateObject('Word.Application')
	doc=word.Documents.Open(in_file)
	doc.SaveAs(out_file,FileFormat=17)  # wdFormatPDF = 17
	doc.Close()
	word.Quit()

file_path= 'C:/Users/Minyan/Desktop/py/Auto/pdf/练习文件/word文档/'
file_list=os.listdir(file_path)
for word_path in file_list:
    if word_path.endswith('.docx'):
        doc_name = file_path + word_path
        pdf_name=file_path+word_path.split(".")[0]+".pdf"
        dord2pdf(doc_name,pdf_name)
        print(word_path, '已转换为pdf')
```

