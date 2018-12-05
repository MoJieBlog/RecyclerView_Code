### 1.继承类和实现接口

注意，这里我查看的源码是api=26
#### 继承类和实现接口
```java
/**
* 继承ViewGroup,实现ScrollingView和NestedScrollingChild2
*/
public class RecyclerView extends ViewGroup implements ScrollingView, NestedScrollingChild2 {
    
}
```
#### 1.1 ScrollingView
##### 1.1.1 ScrollingView
```java
public interface ScrollingView {
    //计算水平滚动的距离
    int computeHorizontalScrollRange();
    //水平偏移量
    int computeHorizontalScrollOffset();
    //水平滚动长度--暂时先这么理解
    int computeHorizontalScrollExtent();
    //竖直滚动距离
    int computeVerticalScrollRange();
    //竖直偏移量
    int computeVerticalScrollOffset();
    //竖直滚动长度--暂时先这么理解
    int computeVerticalScrollExtent();
}
```
接下来看看RecyclerView是怎么实现这个接口的
```java

    public int computeHorizontalScrollRange() {
        //这里的mLayout,其实就是LayoutManager
        if (this.mLayout == null) {
            return 0;
        } else {
            return this.mLayout.canScrollHorizontally() ? this.mLayout.computeHorizontalScrollRange(this.mState) : 0;
        }
    }
```
再来看看LayoutManager中是怎么写的
```java
    public int computeHorizontalScrollRange(@NonNull RecyclerView.State state) {
            return 0;
        }
```
可以看到直接返回0。如果自己写过自定义的LayoutManager,或者了解LayoutManager，可以知道，这几个方法是要重写的，不然在页面中laoutManager.computeHorizontalScrollRange返回一直是0。当然系统提供的几个LayoutManager这几个方法已经重写好了。我们平时可以直接用。
<br><br>
接口中的其他方法，跟这个类似，不再一一举例

##### 1.1.2 NestedScrollingChild2
```java
public interface NestedScrollingChild2 extends NestedScrollingChild {
    boolean startNestedScroll(int var1, int var2);

    void stopNestedScroll(int var1);

    boolean hasNestedScrollingParent(int var1);

    boolean dispatchNestedScroll(int var1, int var2, int var3, int var4, @Nullable int[] var5, int var6);

    boolean dispatchNestedPreScroll(int var1, int var2, @Nullable int[] var3, @Nullable int[] var4, int var5);
}
```
可以发现是继承NestedScrollingChild的一个接口。我们知道NestedScrollingChild配合NestedScrollingParent接口，可以实现嵌套滚动。这里先暂时理解到是用来处理嵌套滚动的。至于嵌套滚动，网上有很多的专门讲这个的，暂时不说，因为只有当父View实现NestedScrollingParent才会用到这部分代码（网上有一个解释我觉得不错，这里用下：就是通过捕捉实现了NestedScrollingChild的view的事件，来通知实现了NestedScrollingParent 的view去做点什么事）。
