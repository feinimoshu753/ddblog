---
title: 【android】拼图实现浅谈（类似美图秀秀拼图功能)
date: 2016-02-28 18:14:55
tags: android
---
又好久没有写东西。本来这个东西在两个月之前就该记录下来的，拖了那么久，醉过醉过。

今天介绍的是android实现一个基础的拼图功能。之前项目中需要到拼图的功能，首先就是第一反应就是geogle有没有可以使用的控件，但是查了半天，没有找到一个合适的。没有的话就只有自己动手了。好了，废话不说了，现在开始进入正题。

<!-- more -->

实现思路：

1、将拼图界面的每个小块用坐标记录下来，用Json格式存储在FIle中。

2、拼图绘制之前把数据从文件中读取放入list。

3、用canvas和paint根据坐标绘制出效果

4、在onTouchEvent中处理各图片拖动



储存坐标：

把拼图背景区域的左上角坐标设为（0，0）,其他点坐标进行计算。（代码中的坐标都是用dp进行表示的，不是px），下面贴上代码。
``` 
{
  "style": [
    {
      "pic": [
        {
          "coordinates": [
            {
              "x": 0,
              "y": 0
            },
            {
              "x": 320,
              "y": 0
            },
            {
              "x": 320,
              "y": 225
            },
            {
              "x": 0,
              "y": 225
            }
          ]
        },
        {
          "coordinates": [
            {
              "x": 0,
              "y": 230
            },
            {
              "x": 320,
              "y": 230
            },
            {
              "x": 320,
              "y": 450
            },
            {
              "x": 0,
              "y": 450
            }
          ]
        }
      ]
    },
    {
      "pic": [
        {
          "coordinates": [
            {
              "x": 0,
              "y": 0
            },
            {
              "x": 320,
              "y": 0
            },
            {
              "x": 320,
              "y": 165
            },
            {
              "x": 0,
              "y": 165
            }
          ]
        },
        {
          "coordinates": [
            {
              "x": 0,
              "y": 170
            },
            {
              "x": 320,
              "y": 170
            },
            {
              "x": 320,
              "y": 450
            },
            {
              "x": 0,
              "y": 450
            }
          ]
        }
      ]
    }
  ]
}
``` 

数据读取：

文件是放在assets文件夹中，通过getAssets方法，然后读取文件内容。下面贴上代码。

``` 
 //读取模板路径文件
    public String readAsset(String fileName) {
        AssetManager am = context.getAssets();

        String data = "";

        InputStream is = null;
        try {
            is = am.open(fileName);
        } catch (IOException e) {
            e.printStackTrace();
        }
        data = readDataFromInputStream(is);
        try {
            if (is != null) {
                is.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

        return data;
    }

    private String readDataFromInputStream(InputStream is) {
        BufferedInputStream bis = new BufferedInputStream(is);

        String str = "", s = "";

        int c = 0;
        byte[] buf = new byte[1024];
        while (true) {
            try {
                c = bis.read(buf);
            } catch (IOException e) {
                e.printStackTrace();
            }

            if (c == -1)
                break;
            else {
                try {
                    s = new String(buf, 0, c, "UTF-8");
                } catch (UnsupportedEncodingException e) {
                    e.printStackTrace();
                }
                str += s;
            }
        }

        try {
            bis.close();
        } catch (IOException e) {
            e.printStackTrace();
        }

        return str;
    }
```     
绘制图片：

主要用到了android的相关绘图知识，不太熟悉的同学可以先去了解了解。主要方式就是将拼图每块通过path按照给定的坐标进行图案的基本绘制，然后通过canvas的drawBitmap方法将图片绘制上去，形成各部分的形状。

``` 
 private void initPath() {
        path = new Path[pathNum];
        for (int i = 0; i < pathNum; i++) {
            path[i] = new Path();
        }
        bitmapsFlag = new boolean[pathNum];

        pathOffset = new float[pathNum][2];
        for (int i = 0; i < pathNum; i++) {
            bitmapsFlag[i] = false;
                    pathOffset[i][0] = 0f;
            pathOffset[i][1] = 0f;
        }

        for (int i = 0; i < pathNum; i++) {
            for (int j = 0; j < coordinateSetList.get(i).getCoordinates().size(); j++) {
                float x = coordinateSetList.get(i).getCoordinates().get(j).getX();
                float y = coordinateSetList.get(i).getCoordinates().get(j).getY();
                if (j == 0) {
                    path[i].moveTo(dp2px(x), dp2px(y));
                } else {
                    path[i].lineTo(dp2px(x), dp2px(y));
                }
            }
            path[i].close();
        }

        // get bitmap
        bitmaps = new Bitmap[pathNum];
        for (int i = 0; i < pathNum; i++) {
            BitmapFactory.Options opt = new BitmapFactory.Options();
            opt.inJustDecodeBounds = true;
            BitmapFactory.decodeFile(pics.get(i).path, opt);

            int bmpWdh = opt.outWidth;
            int bmpHgt = opt.outHeight;

            Coordinates coordinate = caculateViewSize(coordinateSetList.get(i).getCoordinates());
            int size = caculateSampleSize(bmpWdh, bmpHgt, dp2px(coordinate.getX()), dp2px(coordinate.getY()));
            opt.inJustDecodeBounds = false;
            opt.inSampleSize = size;

            bitmaps[i] = scaleImage(BitmapFactory.decodeFile(pics.get(i).path, opt), dp2px(coordinate.getX()), dp2px(coordinate.getY()));
        }
    }

    private Coordinates caculateViewSize(List<Coordinates> list) {

        float viewWidth;
        float viewHeight;

        viewWidth = caculateMaxCoordinateX(list) - caculateMinCoordinateX(list);
        viewHeight = caculateMaxCoordinateY(list) - caculateMinCoordinateY(list);

        return new Coordinates(viewWidth, viewHeight);
    }


    private int caculateSampleSize(int picWdh, int picHgt, int showWdh,
                                   int showHgt) {
        // 如果此时显示区域比图片大，直接返回
        if ((showWdh < picWdh) && (showHgt < picHgt)) {
            int wdhSample = picWdh / showWdh;
            int hgtSample = picHgt / showHgt;
            // 利用小的来处理
            int sample = wdhSample > hgtSample ? hgtSample : wdhSample;
            int minSample = 2;
            while (sample > minSample) {
                minSample *= 2;
            }
            return minSample >> 1;
        } else {
            return 0;
        }
    }

    private float caculateMinCoordinateX(List<Coordinates> list) {

        float minX;
        minX = list.get(0).getX();
        for (int i = 1; i < list.size(); i++) {
            if (list.get(i).getX() < minX) {
                minX = list.get(i).getX();
            }
        }
        return minX;
    }

    private float caculateMaxCoordinateX(List<Coordinates> list) {

        float maxX;
        maxX = list.get(0).getX();
        for (int i = 1; i < list.size(); i++) {
            if (list.get(i).getX() > maxX) {
                maxX = list.get(i).getX();
            }
        }
        return maxX;
    }

    private float caculateMinCoordinateY(List<Coordinates> list) {

        float minY;
        minY = list.get(0).getY();
        for (int i = 1; i < list.size(); i++) {
            if (list.get(i).getY() < minY) {
                minY = list.get(i).getY();
            }
        }
        return minY;
    }

    private float caculateMaxCoordinateY(List<Coordinates> list) {

        float maxY;
        maxY = list.get(0).getY();
        for (int i = 1; i < list.size(); i++) {
            if (list.get(i).getY() > maxY) {
                maxY = list.get(i).getY();
            }
        }
        return maxY;
    }

    //图片缩放
    private static Bitmap scaleImage(Bitmap bm, int newWidth, int newHeight) {
        if (bm == null) {
            return null;
        }
        int width = bm.getWidth();
        int height = bm.getHeight();
        float scaleWidth = ((float) newWidth) / width;
        float scaleHeight = ((float) newHeight) / height;
        float scale = 1;
        if (scaleWidth >= scaleHeight) {
            scale = scaleWidth;
        } else {
            scale = scaleHeight;
        }
        Matrix matrix = new Matrix();
        matrix.postScale(scale, scale);
        Bitmap newbm = Bitmap.createBitmap(bm, 0, 0, width, height, matrix,
                true);
        return newbm;
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        // TODO Auto-generated method stub
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        setMeasuredDimension(viewWdh, viewHgt);
    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        canvas.drawColor(Color.TRANSPARENT);// 显示背景颜色
        Paint paint = new Paint();
        paint.setAntiAlias(true);
        paint.setColor(Color.WHITE);
        startDraw(canvas, paint);
    }


    private void startDraw(Canvas canvas, Paint paint) {
        for (int i = 0; i < pathNum; i++) {
            canvas.save();
            drawScene(canvas, paint, i);
            canvas.restore();
        }
    }

    private void drawScene(Canvas canvas, Paint paint, int idx) {
        canvas.clipPath(path[idx]);
        canvas.drawColor(Color.GRAY);
        if (bitmapsFlag[idx]) {
            canvas.drawBitmap(bitmaps[idx], dp2px(caculateMinCoordinateX(coordinateSetList.get(idx).getCoordinates())) + pathOffsetX + pathOffset[idx][0],
                    dp2px(caculateMinCoordinateY(coordinateSetList.get(idx).getCoordinates())) + pathOffsetY + pathOffset[idx][1], paint);
        } else {
            canvas.drawBitmap(bitmaps[idx], dp2px(caculateMinCoordinateX(coordinateSetList.get(idx).getCoordinates())) + pathOffset[idx][0],
                    dp2px(caculateMinCoordinateY(coordinateSetList.get(idx).getCoordinates())) + pathOffset[idx][1], paint);
        }
    }
``` 
事件处理：

这里只处理各部分的移动事件，放大图片和交换图片位置就要大家自己去实现了。处理移动的第一部分就是判断手指落下的地方是属于哪个区域，主要是通过Region中的contains方法去判断落点。第二个部分就是处理图片移动超过图片大小的情况，通过pathOffset这个数组来记录移动的距离，如果超过图片大小，就回到图片最大宽度的范围。

``` 
    float ptx, pty;
    float pathOffsetX, pathOffsetY;

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        switch (event.getAction()) {
            case MotionEvent.ACTION_DOWN:
                for (int i = 0; i < pathNum; i++) {
                    bitmapsFlag[i] = false;
                }
                ptx = event.getRawX() - dp2px(leftMargin);
                pty = event.getRawY() - dp2px(MARGIN_HEIGHT);
                pathOffsetX = 0;
                pathOffsetY = 0;
                int cflag = 0;
                for (cflag = 0; cflag < pathNum; cflag++) {
                    if (contains(path[cflag], ptx, pty)) {
                        bitmapsFlag[cflag] = true;
                        break;
                    }
                }
                break;
            case MotionEvent.ACTION_MOVE:
                pathOffsetX = event.getRawX() - dp2px(leftMargin) - ptx;
                pathOffsetY = event.getRawY() - dp2px(MARGIN_HEIGHT) - pty;
                invalidate();
                break;
            case MotionEvent.ACTION_POINTER_DOWN:
                break;
            case MotionEvent.ACTION_UP:
                for (int i = 0; i < pathNum; i++) {
                    if (bitmapsFlag[i]) {
                        pathOffset[i][0] += event.getRawX() - dp2px(leftMargin) - ptx;
                        pathOffset[i][1] += event.getRawY() - dp2px(MARGIN_HEIGHT) - pty;

                        if (pathOffset[i][0] > 0) {
                            pathOffset[i][0] = 0;
                        }
                        if (pathOffset[i][0] < -(bitmaps[i].getWidth() - getViewWidth(coordinateSetList.get(i).getCoordinates()))) {
                            pathOffset[i][0] = -(bitmaps[i].getWidth() - getViewWidth(coordinateSetList.get(i).getCoordinates()));
                        }
                        if (pathOffset[i][1] > 0) {
                            pathOffset[i][1] = 0;
                        }
                        if (pathOffset[i][1] < -(bitmaps[i].getHeight() - getViewHeight(coordinateSetList.get(i).getCoordinates()))) {
                            pathOffset[i][1] = -(bitmaps[i].getHeight() - getViewHeight(coordinateSetList.get(i).getCoordinates()));
                        }
                        bitmapsFlag[i] = false;
                        break;
                    }
                }
                invalidate();
                break;
            default:
                break;
        }

        return true;
    }

    private boolean contains(Path parapath, float pointx, float pointy) {
        RectF localRectF = new RectF();
        parapath.computeBounds(localRectF, true);
        Region localRegion = new Region();
        localRegion.setPath(parapath, new Region((int) localRectF.left,
                (int) localRectF.top, (int) localRectF.right,
                (int) localRectF.bottom));
        return localRegion.contains((int) pointx, (int) pointy);
    }

    private float getViewWidth(List<Coordinates> list) {

        return dp2px(caculateMaxCoordinateX(list) - caculateMinCoordinateX(list));
    }

    private float getViewHeight(List<Coordinates> list) {

        return dp2px(caculateMaxCoordinateY(list) - caculateMinCoordinateY(list));
    }
``` 

拼图的形状不局限于长方形，多边形都是可以的，坐标计算正确就成。目前这个拼图功能比较简陋，只实现了一些最基础的功能，大家如果需要更多功能，就要自己去实现啦，也可以留言，大家多多交流。

ps:转载请注明出处哦。

附上项目的github地址:https://github.com/feinimoshu753/puzzleview-android
