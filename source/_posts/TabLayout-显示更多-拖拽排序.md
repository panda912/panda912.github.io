---
title: TabLayout 显示更多 拖拽排序
date: 2016-10-28 16:28:21
tags: 自定义 View
---

<table><tr>

<td>

<div align=center>

![](https://ws2.sinaimg.cn/large/ad5b14bfgw1f9cbz0w6uqg207i0dckjm.gif)

</div>

</td>

<td>

<div align=center>

![](https://ws4.sinaimg.cn/large/ad5b14bfgw1f982dfj8dpg207i0dcb2a.gif)

</div>

</td>

</tr></table>

### #实现思路

点击"更多"显示包含`RecyclerView`的`PopupWindow`，只要让`RecyclerView`选中item的position和`ViewPager`的currentItem对应就好了；当`RecyclerView`拖拽排序之后只要控制好当前选中的item的position的逻辑就好了。

不管怎么拖拽排序，只要当前选中的item的position没有变，则排序之后选中的position还是原position；如果排序之后当前选中的item的position发生变化，则根据具体情况`+1`或`-1`；如果当前拖拽的是选中的item，则拖拽排序之后当前选中item的position就是拖拽之后的position。

当`Viewpager`切换的时候，相应改变 `RecyclerView`适配器中记录的当前选中的item的值`mCurrentItem`，并更新适配器。

```java
@Override
public void onPageSelected(int position) {
    mRVAdapter.setCurrentItem(position);
    mRVAdapter.notifyDataSetChanged();
}
```

继承`ItemTouchHelper.Callback`重写其方法，处理拖拽的效果，并让适配器实现`OnItemMoveCallback`接口，把拖拽的相关逻辑放到适配器。

```java
public class TabItemTouchCallback extends ItemTouchHelper.Callback {
    @Override
    public int getMovementFlags(RecyclerView recyclerView, RecyclerView.ViewHolder viewHolder) {
        if (recyclerView.getLayoutManager() instanceof GridLayoutManager) {
            final int dragFlags = ItemTouchHelper.UP | ItemTouchHelper.DOWN | ItemTouchHelper.LEFT | ItemTouchHelper.RIGHT;
            final int swipeFlags = 0;
            return makeMovementFlags(dragFlags, swipeFlags);
        } else if (recyclerView.getLayoutManager() instanceof LinearLayoutManager) {
            final int dragFlags = ItemTouchHelper.UP | ItemTouchHelper.DOWN;
            final int swipeFlags = 0;
            return makeMovementFlags(dragFlags, swipeFlags);
        } else {
            return 0;
        }
    }

    @Override
    public boolean onMove(RecyclerView recyclerView, RecyclerView.ViewHolder viewHolder, RecyclerView.ViewHolder target) {
        int fromPosition = viewHolder.getAdapterPosition();
        int toPosition = target.getAdapterPosition();
        if (mOnItemMoveCallback != null) {
            mOnItemMoveCallback.onMove(fromPosition, toPosition);
        }
        return true;
    }

    @Override
    public void onSwiped(RecyclerView.ViewHolder viewHolder, int direction) {
        if (mOnItemMoveCallback != null) {
            mOnItemMoveCallback.onSwiped(viewHolder.getAdapterPosition());
        }
    }

    @Override
    public void onChildDraw(Canvas c, RecyclerView recyclerView, RecyclerView.ViewHolder viewHolder, float dX, float dY, int actionState, boolean isCurrentlyActive) {
        if (actionState == ItemTouchHelper.ACTION_STATE_SWIPE) {
            viewHolder.itemView.setTranslationX(dX);
        } else {
            super.onChildDraw(c, recyclerView, viewHolder, dX, dY, actionState, isCurrentlyActive);
        }
    }

    @Override
    public void onSelectedChanged(RecyclerView.ViewHolder viewHolder, int actionState) {
        super.onSelectedChanged(viewHolder, actionState);
    }

    @Override
    public void clearView(RecyclerView recyclerView, RecyclerView.ViewHolder viewHolder) {
        super.clearView(recyclerView, viewHolder);
        mOnItemMoveCallback.onFinished();
    }


    private OnItemMoveCallback mOnItemMoveCallback;

    public void setOnItemMoveCallback(OnItemMoveCallback callback) {
        mOnItemMoveCallback = callback;
    }

    public interface OnItemMoveCallback {
        void onMove(int fromPosition, int toPosition);

        void onSwiped(int position);

        void onFinished();
    }
}
```

重点看`onMove`方法：



```java
public abstract int getMovementFlags(RecyclerView recyclerView, ViewHolder viewHolder);
public abstract boolean onMove(RecyclerView recyclerView, ViewHolder viewHolder, ViewHolder target);
public abstract void onSwiped(ViewHolder viewHolder, int direction);
public void onSelectedChanged(ViewHolder viewHolder, int actionState) {}
public void clearView(RecyclerView recyclerView, ViewHolder viewHolder) {}
```

