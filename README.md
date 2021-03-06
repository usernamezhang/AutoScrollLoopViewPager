# ScrollLoopViewPager

Android auto scroll viewpager


## ScreenShot

![AutoScrollLoopViewPager](screenshot/shot.gif "AutoScrollLoopViewPager")

## Import
- `maven`

```xml
<dependency>
  <groupId>com.bobomee.android</groupId>
  <artifactId>scrollloopviewpager</artifactId>
  <version>2.0</version>
  <type>aar</type>
</dependency>
```

- `gradle`

```groovy
compile 'com.bobomee.android:scrollloopviewpager:2.0'
```

## Usage


- Simple

```java
      ViewPager lViewPager = ViewFindUtils.find(pView, R.id.viewpager);
      lViewPager.setAdapter(new FragmentStateAdapter(getChildFragmentManager()));

      final BannerController lBannerController = new BannerScroll(getActivity());//use default BannerConfig
      lBannerController.viewPager(lViewPager);//attach viewpager

      lViewPager.setOnTouchListener(new View.OnTouchListener() {
        @Override public boolean onTouch(View v, MotionEvent event) {
          lBannerController.dispatchTouchEvent(event); // dispatch touchevent
          return false;
        }
      });

      lBannerController.startAutoScroll();
```


-  [BannerConfig](https://github.com/BoBoMEe/AutoScrollLoopViewPager/blob/master/scrollloopviewpager/src/main/java/com/bobomee/android/scrollloopviewpager/autoscrollviewpager/BannerConfig.java)

```java
 final BannerConfig lBannerConfig = BannerConfig.sConfig(getContext())
          .autoScrollFactor(0.8f)// scroll factor for auto scroll
          .swipeScrollFactor(1.2f)// scroll factor for swipe scroll
          .interval(800);// auto scroll interval
```

- [BannerController](https://github.com/BoBoMEe/AutoScrollLoopViewPager/blob/master/scrollloopviewpager/src/main/java/com/bobomee/android/scrollloopviewpager/autoscrollviewpager/BannerController.java)

```java
final BannerController lBannerController = new BannerScroll(lBannerConfig);//use custom config
lBannerController.viewPager(lViewPager); // attach viewpager

      lViewPager.setOnTouchListener(new View.OnTouchListener() {
        @Override public boolean onTouch(View v, MotionEvent event) {
          lBannerController.dispatchTouchEvent(event);// dispatchTouchEvent,stop scroll when touch
          return false;
        }
      });
```

- Advance

```java
// pagechange
lViewPager.addOnPageChangeListener(new ViewPager.SimpleOnPageChangeListener() {
        @Override public void onPageSelected(int position) {
          super.onPageSelected(position);
          if (lBannerController.isFirst() || lBannerController.isLast()) {
            BannerConfig lBannerConfig = lBannerController.getConfing();
            lBannerConfig.toggleDirection();//change scroll direction
          }
        }
      });
```
## Extends

However, you can also customize Viewpager

```java
public class InfiniteBanner extends LoopViewPager {

  private BannerScroll mBannerScroll;

  public InfiniteBanner(Context context) {
    super(context);
    init();
  }

  public InfiniteBanner(Context context, AttributeSet attrs) {
    super(context, attrs);
    init();
  }

  private void init() {
    mBannerScroll = new BannerScroll(getContext());
    mBannerScroll.viewPager(this);
    mBannerScroll.startAutoScroll();
  }

  @Override public boolean dispatchTouchEvent(MotionEvent ev) {
    mBannerScroll.dispatchTouchEvent(ev);
    return super.dispatchTouchEvent(ev);
  }

  @Override protected void onDetachedFromWindow() {
    mBannerScroll.onDetachedFromWindow();
    super.onDetachedFromWindow();
  }
}
```

```java
 ViewPager viewPager = ViewFindUtils.find(view, viewpager);
 FragmentStateAdapter fragmentStateAdapter = new FragmentStateAdapter(fragmentManager);
 viewPager.setAdapter(fragmentStateAdapter);
```

## Config Setting

- `interval(long)` set auto scroll time in milliseconds, default is `DEFAULT_INTERVAL`.
- `direction(int)` set auto scroll direction, default is `RIGHT`.
- `autoScrollFactor(double)` set the factor by which the duration of sliding animation will change.
- `stopScrollWhenTouch(boolean)` set whether stop auto scroll when touching, default is true.
- You may need [JakeWharton/ViewPagerIndicator](https://github.com/JakeWharton/Android-ViewPagerIndicator) to implement indicator. 
- Also you can see [DrawableIndicator](https://github.com/BoBoMEe/DrawableIndicator)

## Thanks

*   [imbryk/LoopingViewPager](https://github.com/imbryk/LoopingViewPager)
*   [Trinea/android-auto-scroll-view-pager](https://github.com/Trinea/android-auto-scroll-view-pager)
