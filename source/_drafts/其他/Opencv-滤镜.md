---
title: Opencv-滤镜
categories:
  - Opencv
tags:
  - Opencv
  - 图像处理
mathjax: true
date: 2025-04-02 11:37:49
---

![](file-20250402123710368.png)
## 1.毛玻璃

**1）运行界面**
![](file-20250402123717362.png)

**2）代码实现**
原理：通过在输入图像的每个像素周围添加随机偏移量来模拟图像被玻璃片扭曲的效果。
- 计算扰动范围 `n`，并确保为奇数。
- 使用 `copyMakeBorder` 处理边界问题。
- 对每个像素 `(i, j)` 计算一个随机偏移 `(dx, dy)`，从 `src1` 取样。
- 生成类似毛玻璃散射光线的模糊效果。

```c++
Mat Filter::glass(Mat src, int n) {
    int h = src.rows, w = src.cols;
    int m = min(h, w);

    // 计算扰动范围 n（按图像尺寸比例调整）
    n = n * m / 100;
    n = (n / 2) * 2 + 1; // 确保 n 为奇数，保证扰动对称性

    Mat dst(h, w, CV_8UC3, Scalar(0, 0, 0)); // 初始化目标图像
    if (n == 0) {
        return src; // 若 n 为 0，直接返回原图
    }

    Mat src1;
    // 进行边界扩展，防止随机扰动导致越界
    copyMakeBorder(src, src1, n / 2, n / 2, n / 2, n / 2, BORDER_REFLECT);

    // 遍历图像，对每个像素进行随机偏移
    for (int i = 0; i < h; ++i) {
        for (int j = 0; j < w; ++j) {
            int index = n / 2 - rand() % n; // 计算随机偏移量
            dst.at<Vec3b>(i, j) = src1.at<Vec3b>(i + index + n / 2, j + index + n / 2); // 赋值扰动后像素
        }
    }

    return dst; // 返回毛玻璃效果图
}

```

## 2.素描

**1）运行界面**
![](file-20250402123731229.png)

**2）代码实现**

原理：对灰度图像进行高斯模糊和反转操作，并与原始灰度图像相除，最后将结果转换回彩色图像以产生素描效果。
- **灰度化**：将彩色图像转换为灰度图。
- **高斯模糊**：对灰度图进行模糊处理，平滑细节。
- **反转图像**：将模糊后的图像取反，使暗处变亮，亮处变暗。
- **除法增强边缘**：用原始灰度图除以反转模糊图像，使边缘更突出，形成素描效果。
- **转换回彩色**：最终将处理后的图像转换回 BGR 格式。
```C++
Mat Filter::sketch(Mat src, int n1, int n2) {
    if (n1 == 0 || n2 == 0) {
        return src; // 参数为0时直接返回原图
    } else {
        Mat gray;
        cvtColor(src, gray, COLOR_BGR2GRAY); // 1. 转换为灰度图
        n1 = (n1 / 2) * 2 + 1; // 确保 n1 为奇数，适用于高斯模糊
        Mat gauss;
        GaussianBlur(gray, gauss, Size(n1, n1), 0); // 2. 进行高斯模糊
        Mat inverGauss = 255 - gauss; // 3. 反转模糊后的图像
        Mat dst;
        divide(gray, inverGauss, dst, n2); // 4. 除法运算，增强边缘对比度，产生素描效果
        Mat dst1;
        cvtColor(dst, dst1, cv::COLOR_GRAY2BGR); // 5. 转换回 BGR 以匹配原图格式
        return dst1; // 返回最终的素描效果图
    }
}

```

## 3.浮雕
**1）运行界面**
![](file-20250402123742041.png)

**2）代码实现**
原理：将图像任意点$(i,j)$赋值为$I(i-1,j-1)-I(i+1,j+1)+n$。
- **灰度化**：将彩色图转换为灰度图，以便进行浮雕处理。
- **计算浮雕效果**：
    - 采用 **斜对角梯度**（左上角像素 - 右下角像素）来计算边缘信息，模拟光照方向的浮雕感。
    - 结果加上 `n` 进行亮度调整，并裁剪到 `[0, 255]` 范围内。
- **转换回彩色**：将结果转换回 BGR 格式，以匹配原始图像格式。

``` c++
Mat Filter::relief(Mat src, int n) {
    int h = src.rows, w = src.cols;
    
    Mat gray;
    cvtColor(src, gray, cv::COLOR_BGR2GRAY); // 1. 转换为灰度图
    
    Mat dst(h, w, CV_8UC1, Scalar(0)); // 2. 初始化目标灰度图

    // 3. 计算浮雕效果（遍历图像，忽略边界）
    for (int i = 1; i < h - 1; ++i) {
        for (int j = 1; j < w - 1; ++j) {
            // 计算左上角和右下角像素的梯度
            int edge = (int)(gray.at<uchar>(i - 1, j - 1)) - (int)(gray.at<uchar>(i + 1, j + 1));
            int val = edge + n; // 加上亮度偏移 n
            
            // 限制像素值在 [0, 255] 之间
            val = min(max(val, 0), 255);
            dst.at<uchar>(i, j) = static_cast<uchar>(val);
        }
    }

    Mat dst1;
    cvtColor(dst, dst1, cv::COLOR_GRAY2BGR); // 4. 转换回 BGR 格式
    return dst1; // 返回浮雕效果图
}

```


## 4.雕刻
**1）运行界面 **
![](file-20250402123750828.png)

**2）代码实现**
原理：将图像任意点$(i,j)$赋值为$I(i+1,j+1)-I(i-1,j-1)+n$。
- **灰度化**：将彩色图像转换为灰度图，便于进行雕刻效果处理。
- **计算雕刻效果**：
    - 采用 **斜对角梯度**（右下角像素 - 左上角像素），与浮雕效果相反，模拟反向光照雕刻感。
    - 结果加上 `n` 进行亮度调整，并裁剪到 `[0, 255]` 范围。
- **转换回彩色**：最终转换回 BGR 格式，匹配原始图像格式。

```C++
Mat Filter::scuplture(Mat src, int n) {
    int h = src.rows, w = src.cols;
    
    Mat gray;
    cvtColor(src, gray, cv::COLOR_BGR2GRAY); // 1. 转换为灰度图
    
    Mat dst(h, w, CV_8UC1, Scalar(0)); // 2. 初始化目标灰度图

    // 3. 计算雕刻效果（遍历图像，忽略边界）
    for (int i = 1; i < h - 1; ++i) {
        for (int j = 1; j < w - 1; ++j) {
            // 计算右下角和左上角像素的梯度（与浮雕方向相反）
            int edge = (int)(gray.at<uchar>(i + 1, j + 1)) - (int)(gray.at<uchar>(i - 1, j - 1));
            int val = edge + n; // 加上亮度偏移 n
            
            // 限制像素值在 [0, 255] 之间
            val = min(max(val, 0), 255);
            dst.at<uchar>(i, j) = static_cast<uchar>(val);
        }
    }

    Mat dst1;
    cvtColor(dst, dst1, cv::COLOR_GRAY2BGR); // 4. 转换回 BGR 格式
    return dst1; // 返回雕刻效果图
}
```

## 5.油画
**1）运行界面**
![](file-20250402123800251.png)
**2）代码实现**
原理：将在计算某点像素值时，通过计算周围区域的某些像素值得到本点像素值
- **灰度化**：将彩色图转换为灰度图，用于亮度分层。
- **统计灰度级别**：
    - 选取 `radius × radius` 范围内的像素，并按亮度 `level` 分组（默认 `level=8`，即 `灰度值/32`）。
    - 统计每个亮度分组的像素数量，找出最多的亮度等级。    
- **确定像素颜色**：
    - 在相同 `radius × radius` 范围内，选取属于该亮度等级的像素，并用其颜色填充中心像素。
    - 这样，每个像素的颜色由其邻域内的最主要亮度决定，形成油画效果。

``` C++
Mat oil1(Mat src, int level, int radius) {
    int h = src.rows, w = src.cols;
    
    Mat oilImg(h, w, CV_8UC3, Scalar(0, 0, 0)); // 1. 初始化目标图像
    Mat gray;
    cvtColor(src, gray, COLOR_BGR2GRAY); // 2. 转换为灰度图
    
    // 3. 遍历图像（忽略边界）
    for (int i = 2; i < h - 2; ++i) {
        for (int j = 2; j < w - 2; ++j) {
            int quant[8] = { 0 }; // 亮度分层统计数组
            
            // 4. 统计当前邻域内各亮度级别的数量
            for (int k = -2; k < 2; ++k) {
                for (int t = -2; t < 2; ++t) {
                    int levelIndex = gray.at<uchar>(i + k, j + t) / 32; // 计算亮度级别
                    quant[levelIndex]++;
                }
            }
            
            // 5. 找到数量最多的亮度级别
            int valMax = *max_element(begin(quant), end(quant));
            int valIndex = std::distance(std::begin(quant), std::find(std::begin(quant), std::end(quant), valMax));
            
            uchar b = 0, g = 0, r = 0;
            
            // 6. 在邻域内找到该亮度级别对应的像素颜色
            for (int k = -2; k < 2; ++k) {
                for (int t = -2; t < 2; ++t) {
                    int grayVal = gray.at<uchar>(i + k, j + t);
                    if (grayVal >= valIndex * 32 && grayVal < (valIndex + 1) * 32) {
                        Vec3b intensity = src.at<Vec3b>(i + k, j + t);
                        b = intensity.val[0];
                        g = intensity.val[1];
                        r = intensity.val[2];
                    }
                }
            }
            
            // 7. 赋值给目标图像
            oilImg.at<Vec3b>(i, j) = Vec3b(b, g, r);
        }
    }
    
    return oilImg; // 返回油画效果图像
}

```


## 6.马赛克
**1）运行界面
![](file-20250402123812188.png)

**2）代码实现**
原理：通过将图像分割成多个小块，然后对每个小块内的像素进行像素化处理，即用块内的某个颜色代替所有像素的颜色，从而实现图像的模糊和抽象化效果。
1. **分块**：将图像按 `n×n` 大小的网格划分成多个小块，每个小块作为一个区域（ROI）。
2. **像素化处理**：
    - 选取小块内的某个像素（例如左上角像素）作为代表色。
    - 用该代表色填充整个小块，实现马赛克效果。
3. **返回处理后的图像**，使得图像变得模糊和抽象化。
``` C++
Mat Filter::myMask(Mat src, int n) {
    if (n <= 1) {
        return src; // 1. 如果 n <= 1，则无需处理，直接返回原图
    }
    
    Mat dst = src.clone(); // 2. 复制原图
    int h = src.rows, w = src.cols;
    
    // 3. 以 n×n 为单位划分图像
    for (int i = 0; i < h; i += n) {
        for (int j = 0; j < w; j += n) {
            
            // 4. 确定当前 n×n 小块的区域，避免超出边界
            Rect roi(j, i, min(n, w - j), min(n, h - i));
            Mat roiImg = dst(roi);
            
            // 5. 选取左上角像素作为代表色（也可以改为小块均值）
            Vec3b avgColor = src.at<Vec3b>(i, j);
            
            // 6. 用选定颜色填充整个小块
            roiImg.setTo(avgColor);
        }
    }

    return dst; // 7. 返回马赛克处理后的图像
}

```
## 7.卡通
**1）运行界面**
![](file-20250402123821298.png)
**2）代码实现**
原理：卡通效果的原理是通过减少图像的细节，从而使图像看起来更加平滑和简化，就像是手绘的卡通画一样。
- **平滑图像（减少细节）**
    - 使用 **双边滤波（bilateralFilter）** 多次平滑图像，减少噪声，同时保留主要色块和边缘。
- **提取边缘（增强轮廓）**
    - 转换为灰度图像并使用 **中值模糊（medianBlur）** 进一步去噪。
    - 采用 **自适应阈值（adaptiveThreshold）** 提取边缘，使其呈现二值化的手绘线条效果。
- **融合平滑图像和边缘**
    - 通过 **按位与操作（bitwise_and）** 将平滑的色彩图像与二值化边缘图结合，形成卡通风格的输出。
``` C++
Mat Filter::cartoon(Mat src, int res) {
    Mat gray;
    cvtColor(src, gray, COLOR_BGR2GRAY); // 1. 转换为灰度图
    int h = src.rows, w = src.cols;
    
    // 2. 确定双边滤波的次数
    int n = (res / 2) * 2 + 1;
    int num_bilateral = n;
    Mat img_color = src;

    // 3. 使用多次双边滤波（减少颜色细节但保留边缘）
    for (int i = 0; i < num_bilateral; i++) {
        Mat temp;
        bilateralFilter(img_color, temp, 9, n, 7);
        img_color = temp;
    }

    // 4. 进行中值模糊，去除小的噪点
    medianBlur(gray, gray, 9);
    
    // 5. 使用自适应阈值检测边缘
    Mat img_edge;
    adaptiveThreshold(gray, img_edge, 255, ADAPTIVE_THRESH_MEAN_C, THRESH_BINARY, 9, 2);
    
    // 6. 转换边缘图为三通道，以便与彩色图像融合
    cvtColor(img_edge, img_edge, COLOR_GRAY2BGR);
    
    // 7. 通过按位与运算，融合色彩图和边缘
    Mat img_cartoon;
    bitwise_and(img_color, img_edge, img_cartoon);

    return img_cartoon; // 8. 返回卡通化后的图像
}

```
## 8.光照
**1）运行界面**
![](file-20250402123829928.png)
**2）代码实现**
原理：通过计算像素与光源之间的距离，根据距离和光照强度调整像素的颜色，从而实现了基于距离的光照效果。距离越近的像素颜色会受到更大程度的调整，使其看起来更加明亮；而距离越远的像素则保持原有的颜色，从而产生了光照的效果。
1. **确定光源中心**
    - 设定光源位于图像中心 `(centerX, centerY)`，并计算光照影响半径 `radius`。    
2. **遍历图像像素，计算光照影响**
    - 计算当前像素 `(i, j)` 到光源中心的欧几里得距离 `distance`。        
    - 如果 `distance` 小于 `radius²`，计算 **光照增强量**：   $result = n \times \left( 1 - \frac{\sqrt{\text{distance}}}{\text{radius}} \right)$
    - 根据 `result` 计算新的 RGB 值，并进行 `0~255` 限幅。
3. **生成新的光照调整后的图像**
    - 颜色增强的部分会形成类似光照的视觉效果，越靠近中心，光照越强。

``` C++
Mat Filter::illumination(Mat src, int h, int w, int n) {
    if (n == 0) {
        return src;  // 如果光照强度为 0，直接返回原图
    }

    // 1. 确定光源中心和影响半径
    double centerX = h / 2.0;
    double centerY = w / 2.0;
    double radius = min(centerX, centerY);

    // 2. 创建目标图像
    Mat dst(h, w, CV_8UC3, Scalar(0, 0, 0));

    // 3. 遍历图像，计算光照效果
    for (int i = 0; i < h; ++i) {
        for (int j = 0; j < w; ++j) {
            // 计算当前像素到光源的平方距离
            double distance = pow(centerY - j, 2) + pow(centerX - i, 2);

            // 获取原图像素的 BGR 值
            int B = src.at<Vec3b>(i, j)[0];
            int G = src.at<Vec3b>(i, j)[1];
            int R = src.at<Vec3b>(i, j)[2];

            // 如果在光照影响范围内，则增强亮度
            if (distance < radius * radius) {
                int result = static_cast<int>(n * (1.0 - sqrt(distance) / radius));
                B = min(255, max(0, B + result));
                G = min(255, max(0, G + result));
                R = min(255, max(0, R + result));
            }

            // 赋值给新图像
            dst.at<Vec3b>(i, j) = Vec3b(B, G, R);
        }
    }

    return dst;  // 返回光照增强后的图像
}

```
## 9.模糊
**1）运行界面**
![](file-20250402123842295.png)
**2）代码实现**
原理：高斯滤波
``` C++
//模糊
//高斯滤波
Mat Filter::dim(Mat src, int h, int w, int res) {
    int n = (res / 2) * 2 + 1;
    Mat source;
    Mat result;
    GaussianBlur(src, result, Size(n, n), 0);
    return result;
}
```
## 10.怀旧
**1）运行界面**
![](file-20250402123852967.png)
**2）代码实现**
原理：怀旧特效，是基于 心理学公式对原图像三个色彩通道进行变换和低通滤波，产生怀旧的光影效果。
``` C++
Mat Filter::old(Mat src, int h, int w) {
   
    Mat oldImg(h, w, CV_8UC3, Scalar(0, 0, 0));
    for (int i = 0; i < h; ++i) {
        for (int j = 0; j < w; ++j) {
            double b = 0.272 * src.at<Vec3b>(i, j)[2] + 0.534 * src.at<Vec3b>(i, j)[1] + 0.131 * src.at<Vec3b>(i, j)[0];
            double g = 0.349 * src.at<Vec3b>(i, j)[2] + 0.686 * src.at<Vec3b>(i, j)[1] + 0.168 * src.at<Vec3b>(i, j)[0];
            double r = 0.393 * src.at<Vec3b>(i, j)[2] + 0.769 * src.at<Vec3b>(i, j)[1] + 0.189 * src.at<Vec3b>(i, j)[0];
            b = min(b, 255.0);
            g = min(g, 255.0);
            r = min(r, 255.0);
            oldImg.at<Vec3b>(i, j) = Vec3b((uchar)(b), (uchar)(g), (uchar)(r));
        }
    }
    return oldImg;
   
}
```
## 11.单色
**1）运行界面**
![](file-20250402123904626.png)
**2）代码实现**
原理：只保留输入图像中指定通道（R、G 或 B）的颜色，将其他通道的颜色置为0。
```C++
Mat Filter::homochromy(Mat src, int n) {
    int h = src.rows;
    int w = src.cols;
    Mat dst = src.clone();
    for (int i = 0; i < h; i++) {
        for (int j = 0; j < w; j++) {
            for (int k = 0; k < 3; k++) {
                if (k != n) {
                    dst.at<Vec3b>(i, j)[k] = 0;
                }
            }
        }
    }
    return dst;
}
```
## 12.熔铸
**1）运行界面**
![](file-20250402123916017.png)
**2）代码实现**
原理：通过对图像rgb三个分量的调整变化，可以得到熔铸滤镜的效果。以下是调整的公式。
![](file-20250402123953781.png)
```C++
Mat Filter::casting(Mat src, int h, int w) {
    Mat  dst(h, w, CV_8UC3, Scalar(0, 0, 0));
    for (int i = 0; i < h; i++) {
        for (int j = 0; j < w; j++) {
            dst.at<Vec3b>(i, j)[0] = min(255, max(0, (128 * src.at<Vec3b>(i, j)[0] / (src.at<Vec3b>(i, j)[1] + src.at<Vec3b>(i, j)[2] + 1))));
            dst.at<Vec3b>(i, j)[1] = min(255, max(0, (128 * src.at<Vec3b>(i, j)[1] / (src.at<Vec3b>(i, j)[0] + src.at<Vec3b>(i, j)[2] + 1))));
            dst.at<Vec3b>(i, j)[2] = min(255, max(0, (128 * src.at<Vec3b>(i, j)[2] / (src.at<Vec3b>(i, j)[0] + src.at<Vec3b>(i, j)[1] + 1))));
        }
    }
    return dst;
}

```
## 13.冰冻
**1）运行界面**
![](file-20250402123924038.png)
**2）代码实现**
原理：通过以下公式，对图像rgb三个分量进行调整，可以到达冰冻的滤镜特效。
![[Pasted image 20250402101543.png]]
```C++
Mat Filter::frozen(Mat src, int h, int w) {
    Mat  dst(h, w, CV_8UC3, Scalar(0, 0, 0));
    for (int i = 0; i < h; i++) {
        for (int j = 0; j < w; j++) {
            dst.at<Vec3b>(i, j)[0] = (min(255, max(0, abs(src.at<cv::Vec3b>(i, j)[0] - src.at<Vec3b>(i, j)[1] - src.at<Vec3b>(i, j)[2]) * 3 >> 2)));
            dst.at<Vec3b>(i, j)[1] = (min(255, max(0, abs(src.at<cv::Vec3b>(i, j)[1] - src.at<Vec3b>(i, j)[0] - src.at<Vec3b>(i, j)[2]) * 3 >> 2)));
            dst.at<Vec3b>(i, j)[2] = (min(255, max(0, abs(src.at<cv::Vec3b>(i, j)[2] - src.at<Vec3b>(i, j)[0] - src.at<Vec3b>(i, j)[1]) * 3 >> 2)));
        }
    }
    return dst;
}
```
## 14.连环画
**1）运行界面**
![](file-20250402123932712.png)
**2）代码实现**
原理：连环画滤镜的公式为：
![](file-20250402123937755.png)
```C++
Mat Filter::comic(Mat src, int h, int w) {
    Mat  dst(h, w, CV_8UC3, Scalar(0, 0, 0));
    for (int i = 0; i < h; i++) {
        for (int j = 0; j < w; j++) {
            int r = src.at<Vec3b>(i, j)[2];
            int g = src.at<Vec3b>(i, j)[1];
            int b = src.at<Vec3b>(i, j)[0];

            int R = abs(g - b + g + r) * r / 256;
            int G = abs(b - g + b + r) * r / 256;
            int B = abs(b - g + b + r) * g / 256;

            dst.at<cv::Vec3b>(i, j)[0] = (min(255, max(0, B)));
            dst.at<cv::Vec3b>(i, j)[1] = (min(255, max(0, G)));
            dst.at<cv::Vec3b>(i, j)[2] = (min(255, max(0, R)));
        }
    }
    return dst;
}
```
## 15.羽化
**1）运行界面**
![](file-20250402124013310.png)
**2）代码实现**
原理：在图像中心以外的像素上，通过对rgb值增加额外的V值实现朦胧效果。中心点到图像边缘的距离的平方 **s1**，每个像素到图像中心的距离 **s2**。
![](file-20250402124020167.png)

```C++
Mat Filter::feather(Mat src, int h, int w, double n) {
    Mat dst(h, w, CV_8UC3, Scalar(0, 0, 0));

    // 1. 计算图像中心
    int center_x = w / 2;
    int center_y = h / 2;

    // 计算最大距离平方（用于归一化）
    int s1 = center_x * center_x + center_y * center_y;

    // 计算宽高比例，保持羽化效果均匀
    double ratio = (w > h) ? static_cast<double>(h) / w : static_cast<double>(w) / h;

    // 2. 遍历每个像素
    for (int i = 0; i < h; i++) {
        for (int j = 0; j < w; j++) {
            // 计算像素到中心的 dx、dy
            double dx = abs(center_x - j);
            double dy = abs(center_y - i);

            // 依据宽高比例调整 dx 和 dy，保证羽化均匀
            if (center_x > center_y)
                dx *= ratio;
            else
                dy *= ratio;

            // 计算当前像素到中心的平方距离 s2
            double s2 = dx * dx + dy * dy;

            // 计算亮度增强值 v
            double v = 255 * s2 / (s1 * n);

            // 读取原图像素值
            int b = src.at<Vec3b>(i, j)[0];
            int g = src.at<Vec3b>(i, j)[1];
            int r = src.at<Vec3b>(i, j)[2];

            // 3. 计算新颜色值，并进行 0~255 限制
            dst.at<Vec3b>(i, j)[0] = saturate_cast<uchar>(b + v);
            dst.at<Vec3b>(i, j)[1] = saturate_cast<uchar>(g + v);
            dst.at<Vec3b>(i, j)[2] = saturate_cast<uchar>(r + v);
        }
    }
    return dst;
}

```
## 16.黑白
**1）运行界面**
![](file-20250402124027462.png)
**2）代码实现**
原理：设置一个阈值，大于该阈值的为白色，小于该阈值的为黑色
```C++
//黑白
Mat Filter::whiteAndBlack(Mat src, int n) {
    Mat grey;
    cvtColor(src, grey, cv::COLOR_BGR2GRAY);
    Mat dst = grey.clone();
    Mat lookupTable(1, 256, CV_8U);
    uchar* p = lookupTable.data;
    for (int i = 0; i < 256; i++) {
        if (i > n)p[i] = 255;
        else p[i] = 0;
    }
    LUT(grey, lookupTable, dst);
    Mat dst1;
    cvtColor(dst, dst1, COLOR_GRAY2BGR);
    return dst1;
}

```
## 17.流年
**1）运行界面**
![](file-20250402124034856.png)
**2）代码实现**
原理：据像素的蓝色通道强度来调整像素的亮度，获取当前像素的蓝色通道值，并计算其平方根后乘以系数 **n**，从而产生一种模糊和柔和的效果。
- **获取图像的 BGR 通道**
    - 读取原图 `src`，按 `(i, j)` 遍历所有像素。   
- **调整蓝色通道**
    - 计算蓝色分量： $b = \sqrt{B} \times n$
    - `sqrt(B)` 使低亮度的蓝色增加更多（非线性增强）。
    - 乘以 `n` 进行整体调节。
- **保持 G、R 通道不变**
    - 只对蓝色通道进行变化，保持绿色 `G` 和红色 `R` 不变。
- **防止溢出**
    - 由于 `b` 可能超过 `255`，使用 `min(b, 255.0)` 限制最大值。
```C++
Mat Filter::fleet(Mat src, int h, int w, int n) {
    Mat dst(h, w, CV_8UC3, Scalar(0, 0, 0));

    // 遍历每个像素
    for (int i = 0; i < h; ++i) {
        for (int j = 0; j < w; ++j) {
            // 获取原始像素值
            int B = src.at<Vec3b>(i, j)[0];  // 蓝色通道
            int G = src.at<Vec3b>(i, j)[1];  // 绿色通道
            int R = src.at<Vec3b>(i, j)[2];  // 红色通道

            // 计算新的蓝色通道值
            double b = sqrt(B) * n;  
            b = min(b, 255.0);  // 限制最大值

            // 赋值给新图像
            dst.at<Vec3b>(i, j) = Vec3b((uchar)b, (uchar)G, (uchar)R);
        }
    }
    return dst;
}

```
## 18.水彩
**1）运行界面**
![](file-20250402124044677.png)
**2）代码实现**
原理：调用 **stylization** 函数来对输入图像进行风格化处理（使用双边滤波器在保持边缘清晰的同时对图像进行平滑处理。计算原始图像与风格化后的图像之间的细节差异。对细节图像处理后，将细节图像增强到原始图像上）
``` C++
Mat Filter::watercolor(Mat src, int n, float m) {
    Mat res;
    stylization(src, res, n, m);
    return res;
}
```
## 19.描线
**1）运行界面**
![](file-20250402124050982.png)
**2）代码实现**
原理：canny边缘检测
- **转换为灰度图**：
    - `cvtColor(src, gray, COLOR_BGR2GRAY);`
    - 只保留亮度信息，减少计算量。
- **高斯模糊**：
    - `GaussianBlur(gray, gaussian, Size(3, 3), 0);`
    - 作用：平滑图像，减少噪声对边缘检测的影响。
- **Canny 边缘检测**：
    - `Canny(gaussian, canny, 50, 140);`
    - 作用：检测边缘，阈值 `50-140` 控制边缘强度。
- **反转二值化（黑白反转）**
    - `threshold(canny, result, 90, 255, THRESH_BINARY_INV);`
    - 作用：
        - `THRESH_BINARY_INV` 让背景变白，边缘变黑，符合素描风格。
- **转换回彩色图**：
    - `cvtColor(result, dst1, COLOR_GRAY2BGR);`
    - 作用：保持 OpenCV 处理的 **BGR 格式** 兼容性。
```C++
Mat Filter::linedraw(Mat src) {
    Mat gray, gaussian, canny, result, dst1;

    // 1. 转换为灰度图
    cvtColor(src, gray, COLOR_BGR2GRAY);

    // 2. 高斯模糊去噪
    GaussianBlur(gray, gaussian, Size(3, 3), 0);

    // 3. Canny 边缘检测
    Canny(gaussian, canny, 50, 140);

    // 4. 反转二值化，背景变白，线条变黑
    threshold(canny, result, 90, 255, THRESH_BINARY_INV);

    // 5. 转换为 BGR 格式（兼容显示）
    cvtColor(result, dst1, COLOR_GRAY2BGR);

    return dst1;
}

```
## 20.凸透镜
**1）运行界面**
![](file-20250402124059484.png)
**2）代码实现**
原理：当使用凸透镜中心观察一幅图像时，被观察的图像域将按照一定比例进行放大；相应地，这个区域的周围区域将被压缩。如果目标图像中的某一个像素与目标图像中心之间的距离的平方不大于凸透镜的半径的平方（两个整数进行比较，保证比较结果的精确度），就使用映射函数对这个像素的横、纵坐标进行映射处理。
- **确定中心点**：
    - `double center_x = h / 2.0;`
    - `double center_y = w / 2.0;`
    - 作用：将**图像中心**作为凸透镜的光学中心。
- **计算像素与中心的距离**：
    - `double distance = (i - center_x)² + (j - center_y)²;`
    - `double new_dist = sqrt(distance);`
    - 作用：
        - 计算像素点到中心的**欧几里得距离**。
        - `new_dist` 作为变换后的参考。
- **凸透镜变换（位置映射）**：
    - `int new_i = floor(new_dist * (i - center_x) / radius + center_x);`
    - `int new_j = floor(new_dist * (j - center_y) / radius + center_y);`
    - 作用
        - 使靠近中心的像素点**被拉近（放大）**，远离中心的像素点**轻微缩小（压缩）**。
        - 形成类似 **凸透镜光学变形** 的效果。
- **边界处理**：
    - `if (new_i < 0 || new_i >= w) new_i = 0;`
    - `if (new_j < 0 || new_j >= h) new_j = 0;`
    - 作用：
        - 防止像素点超出图像边界，导致访问非法内存。
``` C++
Mat Filter::convex(Mat src, int n) {
    if (n == 0) return src; // 无变换直接返回

    int h = src.rows, w = src.cols;
    int channel = src.channels();
    Mat dst(h, w, src.type(), Scalar(0, 0, 0));

    double center_x = h / 2.0, center_y = w / 2.0;
    double radius = n; // 透镜半径

    for (int i = 0; i < h; ++i) {
        for (int j = 0; j < w; ++j) {
            // 计算当前像素点到中心的距离
            double distance = pow(i - center_x, 2) + pow(j - center_y, 2);
            double new_dist = sqrt(distance);

            // 默认直接拷贝原始像素
            dst.at<Vec3b>(i, j) = src.at<Vec3b>(i, j);

            // 仅在透镜范围内执行变换
            if (distance <= radius * radius) {
                int new_i = floor(new_dist * (i - center_x) / radius + center_x);
                int new_j = floor(new_dist * (j - center_y) / radius + center_y);

                // 边界检查，防止访问越界
                new_i = max(0, min(new_i, h - 1));
                new_j = max(0, min(new_j, w - 1));

                dst.at<Vec3b>(i, j) = src.at<Vec3b>(new_i, new_j);
            }
        }
    }
    return dst;
}

```
## 21.凹透镜
**1）运行界面**
![](file-20250402124108764.png)
**2）代码实现**
原理：将原图中的像素值，坐标转换后的位置映射到新的图像中，得到一个凹透镜的滤镜效果。
1. **确定图像中心**：
    - `int cx = w / 2;`
    - `int cy = h / 2;`
    - 作用：把**图像中心**作为凹透镜的光学中心。
2. **计算像素到中心的偏移量**：
    - `int dx = i - cx;`
    - `int dy = j - cy;`
    - 作用：计算当前像素到中心的**水平 & 垂直偏移量**。
3. **计算新坐标（凹透镜变换）**：
    - `double distance = sqrt(dx * dx + dy * dy);`
    - `double new_distance = sqrt(sqrt(distance)) * n;`
    - `int x = cx + new_distance * cos(atan2(dy, dx));`
    - `int y = cy + new_distance * sin(atan2(dy, dx));`
    - 作用：
        - 计算当前像素到中心的**欧几里得距离**。
        - **凹透镜效应**：通过 `sqrt(sqrt(distance))` 让像素点更接近中心，形成缩小效果。
4. **边界处理**：
    - `if (x < 0 || x >= w) x = 0;`
    - `if (y < 0 || y >= h) y = 0;`
    - 作用：防止访问非法内存。


```C++
Mat Filter::concave(Mat img, int n) {
    if (n == 0) return img; // 无变换时直接返回原图

    int h = img.rows, w = img.cols;
    int cx = w / 2, cy = h / 2;
    Mat new_img = img.clone();

    for (int i = 0; i < w; ++i) {
        for (int j = 0; j < h; ++j) {
            // 计算像素到中心的偏移量
            int dx = i - cx;
            int dy = j - cy;

            // 计算径向距离，并进行非线性收缩
            double r = sqrt(dx * dx + dy * dy);
            double factor = sqrt(sqrt(r)) * n; // 控制变形幅度

            // 计算映射的新坐标
            int x = cx + factor * cos(atan2(dy, dx));
            int y = cy + factor * sin(atan2(dy, dx));

            // 边界检查，防止访问越界
            x = max(0, min(x, w - 1));
            y = max(0, min(y, h - 1));

            // 映射像素
            new_img.at<Vec3b>(j, i) = img.at<Vec3b>(y, x);
        }
    }
    return new_img;
}

```
## 22.霓虹
**1）运行界面**
![](file-20250402124118829.png)
**2）代码实现**
原理：将当前像素与其同列正下方和右方的像素的RGB分量分别做梯度运算（差的平方和的平方根），运算结果作为当前的像素值。为了使图像轮廓边缘发光的效果更明显，可以将运算结果乘以一个常数。
$R = i*sqrt( (r1-r2) * (r1-r2) + (r1-r3) * (r1-r3) )$
$G = i*sqrt( (g1-g2) * (g1-g2) + (g1-g3) * (g1-g3) )$
$B = i*sqrt( (b1-b2) * (b1-b2) + (b1-b3) * (b1-b3) )$
```C++
Mat Filter::neon(Mat src, int n) {
    int h = src.rows;
    int w = src.cols;
    Mat dst = src.clone();
    if (n != 0) {
        for (int i = 0; i < h - 1; i++) {
            for (int j = 0; j < w - 1; j++) {
                int b1 = dst.at<Vec3b>(i, j)[0];
                int g1 = dst.at<Vec3b>(i, j)[1];
                int r1 = dst.at<Vec3b>(i, j)[2];

                int b2 = dst.at<Vec3b>(i, j + 1)[0];
                int g2 = dst.at<Vec3b>(i, j + 1)[1];
                int r2 = dst.at<Vec3b>(i, j + 1)[2];

                int b3 = dst.at<Vec3b>(i + 1, j)[0];
                int g3 = dst.at<Vec3b>(i + 1, j)[1];
                int r3 = dst.at<Vec3b>(i + 1, j)[2];
                int R = n * sqrt((r1 - r2) * (r1 - r2) + (r1 - r3) * (r1 - r3));
                int G = n * sqrt((g1 - g2) * (g1 - g2) + (g1 - g3) * (g1 - g3));
                int B = n * sqrt((b1 - b2) * (b1 - b2) + (b1 - b3) * (b1 - b3));
                dst.at<Vec3b>(i, j)[0] = max(0, min(B, 255));;
                dst.at<Vec3b>(i, j)[1] = max(0, min(G, 255));;
                dst.at<Vec3b>(i, j)[2] = max(0, min(R, 255));;
            }
        }
    }
    return dst;
}
```
## 23.重影
**1）运行界面**
![](file-20250402124127001.png)
**2）代码实现**
原理：将图像创建四个相互偏移的副本，叠加之后产生类似重影的效果。偏移方向为左上，左下，右上，右下，偏移角度为45度。将四个方向的偏移量累加求平均值作为中心点像素的值。各个方向的偏移量可以相同也可以不同。先来看相同的情况。
- **创建四个偏移副本**：
    - 偏移方向为 **左上、左下、右上、右下**，角度**45°**。
    - `OffsetJ[4] = { n, -n, -n, n };` → 影响**行方向**（y 坐标）。
    - `OffsetI[4] = { -n, -n, n, n };` → 影响**列方向**（x 坐标）。
- **像素融合（均值计算）**：
    - 遍历图像的每个像素：
        - 计算该像素的四个偏移位置的 RGB 值总和。
        - 取**均值**作为新的 RGB 值，实现**模糊混合**。
- **边界处理**：
    - 偏移像素点可能超出边界，使用 `max(0, min(...))` 限制在有效范围内。
```C++
Mat Filter::ghost(Mat src, int n) {
    if (n == 0) return src; // 无重影变换则直接返回原图

    int h = src.rows, w = src.cols;
    Mat dst = src.clone();

    int OffsetJ[4] = { n, -n, -n, n };
    int OffsetI[4] = { -n, -n, n, n };

    for (int j = 0; j < h; j++) {
        for (int i = 0; i < w; i++) {
            int sumB = 0, sumG = 0, sumR = 0;

            // 遍历四个偏移位置
            for (int k = 0; k < 4; k++) {
                int jj = max(0, min(h - 1, j + OffsetJ[k]));
                int ii = max(0, min(w - 1, i + OffsetI[k]));

                Vec3b pixel = src.at<Vec3b>(jj, ii);
                sumB += pixel[0];
                sumG += pixel[1];
                sumR += pixel[2];
            }

            // 计算平均值并赋值
            dst.at<Vec3b>(j, i) = Vec3b(sumB / 4, sumG / 4, sumR / 4);
        }
    }
    return dst;
}

```
## 24.灰度
**1）运行界面**
![](file-20250402124135629.png)
**2）代码实现**
原理：使用opencv自带的灰度转换。
```C++
Mat Filter::grey(Mat src) {
    Mat dst;
    cvtColor(src, dst, COLOR_BGR2GRAY);
    Mat dst1;
    cvtColor(dst, dst1, COLOR_GRAY2BGR);
    return dst1;
}
```