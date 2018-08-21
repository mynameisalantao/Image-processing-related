使用Python 用於圖像二值化 20180620
==================================
![Imgur](https://i.imgur.com/5yMFtlD.png)
套件PIL可對圖片進行縮放、切割、旋轉等各類操作<br/>
前面要先載入pillow<br/>
具體做法為使用命令視窗，並輸入:
<pre><code>python -m pip install -U pip
pip install pillow</pre></code>
接著開始程式部分，首先載入模塊
<pre><code>from PIL import Image  
import numpy as np  
import scipy  </pre></code>
  

讀取圖片 現在im即可代表這張圖片 open()函數打開這張圖片<br/>
如此處的路徑為C:\python36/101.jpg其中101為圖檔名稱，以jpg形式儲存
<pre><code>im_color = Image.open("C:\python36/101.jpg")  </pre></code>
  
show()函數顯示圖片 會調用操作系統自帶的圖片瀏覽器打開圖片，有時不太方便
<pre><code>#im_color.show()</pre></code>

此處使用matplotlib來顯示圖片
<pre><code>import matplotlib.pyplot as plt</pre></code>

貼出彩圖部分<br/>
其貼圖方式如同matlab一樣，放置圖片的方式為1列3行的第1個位置
<pre><code>plt.subplot(1,3,1)
plt.plot([0,1],[0,1])
plt.imshow(im_color)</pre></code>


將圖片轉換成灰階圖<br/>
對於彩色圖像，不管其圖像格式是PNG，還是BMP，或者JPG，在PIL中<br/>
使用Image模塊的open()函數打開後，返回的圖像對象的模式都是“RGB”<br/>

對於灰度圖像，不管其圖像格式是PNG，還是BMP，或者JPG，打開後，其模式為“L”
<pre><code>im_gray=Image.open("C:\python36/101.jpg").convert('L')</pre></code>

貼出灰圖部分，放置圖片的方式為1列3行的第2個位置
<pre><code>plt.subplot(1,3,2)
plt.plot([0,1],[0,2])
plt.imshow(im_gray)</pre></code>



將灰階圖以矩陣形式指定給img
<pre><code>img=np.array(im_gray)</pre></code>

接著是二值化的部分<br/>
先取得圖片的pixel列數與行數分別指定給變數rows,cols<br/>
接著使用兩個for迴圈取得在每個pixel上他的灰階為多少<br/>
可以依照圖片的亮暗程度來設置閥值<br/>
<pre><code>rows,cols=img.shape
for i in range(rows):
    for j in range(cols):
        if (img[i,j]<=100):
            img[i,j]=0
        else:
            img[i,j]=1</pre></code>

貼出二值化以後的圖，放置圖片的方式為1列3行的第3個位置
<pre><code>plt.subplot(1,3,3)
plt.plot([0,1],[0,3])
plt.imshow(img,cmap='gray')</pre></code>
最後並排顯示3張圖
<pre><code>plt.show()</pre></code>

使用Matlab 用於圖像處理 20180821
================================


1.將圖片載入，並顯現
<pre><code>mikasa=imread('mikasa.jpg');
figure
imshow(mikasa);</pre></code>


2.轉灰階，並顯現
<pre><code>mikasa=imread('mikasa.jpg');
gray_mikasa=rgb2gray(mikasa);
figure
imshow(gray_mikasa);</pre></code>


3.顯示灰階的直方圖 <br/>
imhist(I) calculates the histogram for the intensity image I and displays a plot of the histogram. <br/>
The number of bins in the histogram is determined by the image type.

<pre><code>mikasa=imread('mikasa.jpg');
gray_mikasa=rgb2gray(mikasa);
histogram=imhist(gray_mikasa);
figure
bar(histogram)
xlim([0 255])
xlabel('pixel value')
ylabel('Number')</pre></code>


4.依據global閥值來進行二值化
<pre><code>mikasa=imread('mikasa.jpg');
gray_mikasa=rgb2gray(mikasa);
threshold = graythresh(gray_mikasa);
%得到的threshold為0.6157
binary = imbinarize(gray_mikasa, threshold);
figure
imshow(binary)</pre></code>


5.Histogram Adjustment <br/>
J = imadjust(I) maps the intensity values in grayscale image I to new values in J.  <br/>
By default, imadjust saturates the bottom 1% and the top 1% of all pixel values.  <br/>
This operation increases the contrast of the output image J.

<pre><code>mikasa=imread('mikasa.jpg');
gray_mikasa=rgb2gray(mikasa);
after_adjust= imadjust(gray_mikasa);</pre></code>

可以與未經過Histogram Adjustment的灰階圖進行比較
![Imgur](https://i.imgur.com/znImRqV.png)
重點更為凸顯
![Imgur](https://i.imgur.com/EmsuswZ.png)
直方圖中每個灰階更分散<br/>

接著為局部Histogram Adjustment比較
<pre><code>mikasa=imread('mikasa.jpg');
gray_mikasa=rgb2gray(mikasa);
histogram1=imhist(gray_mikasa);
subplot(1,2,1);
bar(histogram)
xlim([0 255])
xlabel('pixel value')
ylabel('Number')
hold on;
after_adjust=imadjust(gray_mikasa,[30 80]/255,[]);
histogram2=imhist(gray_mikasa);
subplot(1,2,2);
bar(histogram2)
xlim([0 255])
xlabel('pixel value')
ylabel('Number')
figure
imshow(after_adjust);</pre></code>
![Imgur](https://i.imgur.com/UHieRo4.png)
原圖看不太出來差異<br/>
將其在[30 80]範圍內放大，可以發現右圖更為均勻

6.Histogram Equalization
<pre><code>mikasa=imread('mikasa.jpg');
gray_mikasa=rgb2gray(mikasa);
histogram1=imhist(gray_mikasa);
subplot(1,2,1);
bar(histogram)
xlim([0 255])
xlabel('pixel value')
ylabel('Number')
hold on;
after_equal = histeq(gray_mikasa);
histogram2=imhist(after_equal);
subplot(1,2,2);
bar(histogram2)
xlim([0 255])
xlabel('pixel value')
ylabel('Number')</pre></code>
![Imgur](https://i.imgur.com/v2EOTPD.png)




