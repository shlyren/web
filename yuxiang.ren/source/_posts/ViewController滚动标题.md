---
title: ViewController滚动标题
date: 2017-08-10 17:20:52
categories: Objective-C
tags: Objective-C
---



默认情况下控制器标题过长导致无法全部显示, 解决此问题就要让标题可以滚动.

实现很简单, 只要把`navigationItem.titleView`赋值一个`UIScrollView`, 并且让这个`UIScrollView`自动滚动就可以了.

<!-- more -->

# 一. 定义一个创建一个继承与UIScrollerView的类

1. 头文件

   ```objc
   @interface CTScrollTitleView : UIScrollView

     /** 文字 */
   @property (nonatomic, strong, readonly) NSString *title;

   /** 实例化对象 */
   + (instancetype)titleViewWithTitle:(NSString *)title;
   - (instancetype)initWithTitle:(NSString *)title;

   @end
   ```

2. .m文件

   ```objc
   @implementation CTScrollTitleView

   + (instancetype)titleViewWithTitle:(NSString *)title
   {
      return [[self alloc] initWithTitle:title];
   }

   - (instancetype)initWithTitle:(NSString *)title
   {
      if (title==nil) return nil;

      if (self = [super initWithFrame:CGRectZero]) {

          _title = title;
          self.frame = CGRectMake(0, 0, screenWidth-128, 44);
          self.scrollEnabled = false;
          self.showsVerticalScrollIndicator = false;
          self.showsHorizontalScrollIndicator = false;

          UILabel *label = [UILabel new];
          label.font = [UIFont boldSystemFontOfSize:16];
          label.frame = self.bounds;
          label.textAlignment = NSTextAlignmentCenter;
          label.text = title;
          [self addSubview: label];

        // 计算文字的宽度
      CGFloat width = [label.text boundingRectWithSize:CGSizeMake(MAXFLOAT, label.height)
                                               options:
                        NSStringDrawingTruncatesLastVisibleLine |
                        NSStringDrawingUsesLineFragmentOrigin |
                        NSStringDrawingUsesFontLeading
                                            attributes:@{NSFontAttributeName: label.font}
                                               context:nil].size.width;
        // 只有文字宽度大于scrollview的宽度才需要滚动
          if (width > self.frame.size.width) { 
              CGRect frame = label.frame;
              frame.size.width = width;
              label.frame = frame;

              CGSize contentSize = self.contentSize;
              contentSize.width = width;
              self.contentSize = contentSize;

              [self startAnimate];
          }
      }

      return self;
   }

   /** 滚动动画 */
   - (void)startAnimate {

      NSTimeInterval time = self.contentSize.width * 0.01;

      CGPoint offset = self.contentOffset;
      offset.x = self.contentSize.width - self.frame.size.width;

      __weak typeof(self)weakSelf = self;
    // 从左向右
      [UIView animateWithDuration:time 
                            delay:2.0 
                          options:0 		
                       animations:^{
          weakSelf.contentOffset = offset;
                     } completion:^(BOOL finished) {
          // 从右向左
          [UIView animateWithDuration:time 
                                delay:2.0
                              options:0 															  animations:^{
             weakSelf.contentOffset = CGPointZero;

                         } completion:^(BOOL finished) {
              [weakSelf startAnimate];
          }];

      }];
   }

   - (void)dealloc{
      NSLog(@"%s", __func__)
   }
   ```


# 二. 使用

1. 导入头文件 

2. 实例化`CTScrollTitleView`并赋值给`navigationItem.titleView`

   ```objc
   self.navigationItem.titleView = [CTScrollTitleView titleViewWithTitle:@"标题标题标题标题标题标题标题标题"];
   ```

# 三. 简化版

1. 为了开发方便, 比如直接谁用 self.scrollTitle = @"标题标题标题标题标题标题标题标题标题标题", 可做以下处理

2. 创建`UIViewController`分类: `UIViewController+ScrollTitle.h`

3. 在头文件声明

   ```objc
   @interface UIViewController (ScrollTitle)
   /** 滚动标题 */
   @property (nonatomic, strong) NSString *zy_scrollTitle;

   @end
   ```

4. .m文件实现

   ```objc
   #import <objc/runtime.h>
   #import "CTScrollTitleView.h"

   @interface UIViewController()
   @property (nonatomic, weak) CTScrollTitleView *zy_titleView;
   @end

   @implementation UIViewController (ScrollTitle)

   - (void)setZy_scrollTitle:(NSString *)zy_scrollTitle
   {
       CTScrollTitleView *titleView = [CTScrollTitleView titleViewWithTitle:zy_scrollTitle];
       self.navigationItem.titleView = titleView;
       self.zy_titleView = titleView;
   }
   - (NSString *)zy_scrollTitle
   {
       return self.zy_titleView.title;
   }

   static char ZYScrollTitleViewKey = '\0';
   - (void)setZy_titleView:(CTScrollTitleView *)zy_titleView
   {
       objc_setAssociatedObject(self, &ZYScrollTitleViewKey, zy_titleView, OBJC_ASSOCIATION_ASSIGN);
   }

   - (CTScrollTitleView *)zy_titleView
   {
       return objc_getAssociatedObject(self, &ZYScrollTitleViewKey);
   }
   ```

   ​

5. 直接使用`self.zy_scrollTitle`即可