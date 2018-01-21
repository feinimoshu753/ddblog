---
title: 【android】第一个简单的轮子(多图展示控件--ImageViewGroup)
date: 2015-12-16 22:55:37
tags: android
---
做android开发已经有一段时间了，看了很多别人写的控件，也用了很多别人的东西，总是希望自己也能写出一些好用的东西，终于开始了自己的第一次尝试。

多图展示控件是很常见的控件，像微信朋友圈、qq空间说说等。大家可能有会觉得这个东西很简单，用一个不可滑动gridVIew就可以实现了，没必要重新写一个。

之前也用不可滑动gridView实现过类似的效果，但是使用过程中存在一些问题：

<!-- more -->

1、item之间的间距在不同手机上，适配可能出现问题。

2、gridView的点击事件把没有图片的地方的点击事件拦截了，导致点击空白处跳转无反应

3、我懒得去写gridView的adapter。


废话不多说了，贴上代码。

``` 
public class ImageViewGroup extends LinearLayout implements View.OnClickListener {
    //存图片得数组
    private String[] imageArray;
    //每行的item个数，默认为3
    private int columnSize = 3;
    //默认间距,默认为2dp
    private int SPACING = 2;
    //默认宽度为270dp
    private int viewWith = 270;
    <pre name="code"class="java">    //使用了imageloader来加载图片
    private ImageLoader imageLoader = ImageLoader.getInstance();

    //    需要加载的最大图片数
    private int maxNum;
    private OnItemClickListener onItemClickListener;

    public ImageViewGroup(Context context) {
        super(context);
        initBase();
    }

    public ImageViewGroup(Context context, AttributeSet attrs) {
        super(context, attrs);
        initBase();
    }

    public ImageViewGroup(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        initBase();
    }

//    设置每行图片的个数

    public void setColumnSize(int size) {
        this.columnSize = size;
    }

//    设置上下和左右的间距

    public void setSpacing(int spacing) {
        this.SPACING = DensityTool.px2dip(getContext(), spacing);
    }

//    设置view的宽度

    public void setWidth(int width) {
        this.viewWith = DensityTool.px2dip(getContext(), width);
    }

//    设置显示的最大图片数

    public void setMaxSize(int maxNum) {
        this.maxNum = maxNum;
    }

//    2种添加图片的方法，可以添加列表，数组类型

    public void setImages(List<String> imageList) {
        if (imageList != null && imageList.size() > 0) {
            setVisibility(VISIBLE);
            imageArray = new String[imageList.size()];
            for (int i = 0; i < imageList.size(); i++) {
                imageArray[i] = imageList.get(i);
            }
            initImages(imageArray);
        } else {
            setVisibility(GONE);
        }
    }

    public void setImages(String[] imageArray) {
        if (imageArray != null && imageArray.length > 0) {
            setVisibility(VISIBLE);
            this.imageArray = imageArray;
            initImages(imageArray);
        } else {
            setVisibility(GONE);
        }
    }

//    加载view的大小

    private void initBase() {
        setLayoutParams(new LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT));
        setOrientation(VERTICAL);
        setBackgroundColor(getResources().getColor(R.color.white));
    }

//    加载Images

    private void initImages(String[] imageArray) {
//        检查是否设置了最大图片数量
        imageArray = dealPic(imageArray);
//        item的大小
        int itemSize = (viewWith - SPACING * (columnSize - 1)) / columnSize;
        List<ImageView> imageViews = new ArrayList<>();
        //在添加imageview之前需清除之前的view
        removeAllViews();
        for (int i = 0; i < imageArray.length / columnSize + 1; i++) {
            LinearLayout linearLayout = new LinearLayout(getContext());
            if (i > 0) {
                linearLayout.setPadding(0, DensityTool.dip2px(getContext(), SPACING), 0, 0);
            } else {
                linearLayout.setPadding(0, 0, 0, 0);
            }
            linearLayout.setLayoutParams(new LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT));
            linearLayout.setOrientation(HORIZONTAL);
            addView(linearLayout);
            int length = columnSize > imageArray.length - i * columnSize ? imageArray.length - i * columnSize : columnSize;
            for (int j = 0; j < length; j++) {
                ImageView imageView = new ImageView(getContext());
                imageView.setId(j + i * columnSize);
                imageView.setScaleType(ImageView.ScaleType.CENTER_CROP);
                imageView.setOnClickListener(this);
                imageViews.add(imageView);
                LayoutParams layoutParams = new LayoutParams(DensityTool.dip2px(getContext(), itemSize), DensityTool.dip2px(getContext(), itemSize));
                layoutParams.setMargins(0, 0, DensityTool.dip2px(getContext(), SPACING), 0);
                linearLayout.addView(imageView, layoutParams);
            }
        }
        loadImages(imageViews);
    }

    private void loadImages(List<ImageView> imageViews) {
        if (imageViews == null || imageViews.size() == 0) {
            return;
        }
        for (int i = 0; i < imageViews.size(); i++) {
            if (i >= imageArray.length) {
                break;
            }
            imageLoader.displayImage(imageArray[i], imageViews.get(i));
        }
    }

    //处理设置了最大显示图片的情况
    private String[] dealPic(String[] images) {
        if (maxNum != 0 && images.length > maxNum) {
            String[] pics = new String[maxNum];
            System.arraycopy(images, 0, pics, 0, maxNum);
            return pics;
        }
        return images;
    }

    @Override
    public void onClick(View view) {
        int id = view.getId();
        if (onItemClickListener != null) {
            onItemClickListener.onItemListener(id);
        }
    }

//    item点击事件监听

    public interface OnItemClickListener {
        void onItemListener(int index);
    }

    public void setOnItemClickListener(OnItemClickListener onItemClickListener) {
        this.onItemClickListener = onItemClickListener;
    }
}
```
思路很简单，一个纵向的LinearLayout中包含几个横向的LinearLayout,根据添加的图片数量，为每个横向的LinearLayout中添加若干个ImageView,将添加的imageView加入一个List中，最后通过ImageLoader加载出图片。可以设置每行图片的个数、间距大小、view的宽度、最大显示图片张数。
需要注意的几个问题：

1、在linearLayout中添加ImageView的时候注意不要超过图片张数，否则会出现数组越界，在loadImages方法中也进行了越界判断，防止崩溃。

2、在每次添加横向的linearLayout时记得remove所有的view,避免出现view的多次显示。

3、点击事件是通过图片位置作为图片id，再通过view.getId获取ImageView的位置，也可以通过tag进行获取。