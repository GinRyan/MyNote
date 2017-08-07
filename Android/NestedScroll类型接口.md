**嵌套滚动的有关的接口和类，有四个：**

接口：NestedScrollingParent，NestedScrollingChild

辅助类：NestedScrollingChildHelper，NestedScrollingParentHelper

NestedScrollingParent 接口

```java
/**
 * This interface should be implemented by {@link android.view.ViewGroup ViewGroup} subclasses
 * that wish to support scrolling operations delegated by a nested child view.
 * 
 * <p>Classes implementing this interface should create a final instance of a
 * {@link NestedScrollingParentHelper} as a field and delegate any View or ViewGroup methods
 * to the <code>NestedScrollingParentHelper</code> methods of the same signature.</p>
 *
 * <p>Views invoking nested scrolling functionality should always do so from the relevant
 * {@link ViewCompat}, {@link ViewGroupCompat} or {@link ViewParentCompat} compatibility
 * shim static methods. This ensures interoperability with nested scrolling views on Android
 * 5.0 Lollipop and newer.</p>
 */
public interface NestedScrollingParent {
    /**
     * React to a descendant view initiating a nestable scroll operation, claiming the
     * nested scroll operation if appropriate.
     *
     * <p>This method will be called in response to a descendant view invoking
     * {@link ViewCompat#startNestedScroll(View, int)}. Each parent up the view hierarchy will be
     * given an opportunity to respond and claim the nested scrolling operation by returning
     * <code>true</code>.</p>
     *
     * <p>This method may be overridden by ViewParent implementations to indicate when the view
     * is willing to support a nested scrolling operation that is about to begin. If it returns
     * true, this ViewParent will become the target view's nested scrolling parent for the duration
     * of the scroll operation in progress. When the nested scroll is finished this ViewParent
     * will receive a call to {@link #onStopNestedScroll(View)}.
     * </p>
     *
     * @param child Direct child of this ViewParent containing target
     * @param target View that initiated the nested scroll
     * @param nestedScrollAxes Flags consisting of {@link ViewCompat#SCROLL_AXIS_HORIZONTAL},
     *                         {@link ViewCompat#SCROLL_AXIS_VERTICAL} or both
     * @return true if this ViewParent accepts the nested scroll operation
     */
    public boolean onStartNestedScroll(View child, View target, int nestedScrollAxes);

    /**
     * React to the successful claiming of a nested scroll operation.
     *
     * <p>This method will be called after
     * {@link #onStartNestedScroll(View, View, int) onStartNestedScroll} returns true. It offers
     * an opportunity for the view and its superclasses to perform initial configuration
     * for the nested scroll. Implementations of this method should always call their superclass's
     * implementation of this method if one is present.</p>
     *
     * @param child Direct child of this ViewParent containing target
     * @param target View that initiated the nested scroll
     * @param nestedScrollAxes Flags consisting of {@link ViewCompat#SCROLL_AXIS_HORIZONTAL},
     *                         {@link ViewCompat#SCROLL_AXIS_VERTICAL} or both
     * @see #onStartNestedScroll(View, View, int)
     * @see #onStopNestedScroll(View)
     */
    public void onNestedScrollAccepted(View child, View target, int nestedScrollAxes);

    /**
     * React to a nested scroll operation ending.
     *
     * <p>Perform cleanup after a nested scrolling operation.
     * This method will be called when a nested scroll stops, for example when a nested touch
     * scroll ends with a {@link MotionEvent#ACTION_UP} or {@link MotionEvent#ACTION_CANCEL} event.
     * Implementations of this method should always call their superclass's implementation of this
     * method if one is present.</p>
     *
     * @param target View that initiated the nested scroll
     */
    public void onStopNestedScroll(View target);

    /**
     * React to a nested scroll in progress.
     *
     * <p>This method will be called when the ViewParent's current nested scrolling child view
     * dispatches a nested scroll event. To receive calls to this method the ViewParent must have
     * previously returned <code>true</code> for a call to
     * {@link #onStartNestedScroll(View, View, int)}.</p>
     *
     * <p>Both the consumed and unconsumed portions of the scroll distance are reported to the
     * ViewParent. An implementation may choose to use the consumed portion to match or chase scroll
     * position of multiple child elements, for example. The unconsumed portion may be used to
     * allow continuous dragging of multiple scrolling or draggable elements, such as scrolling
     * a list within a vertical drawer where the drawer begins dragging once the edge of inner
     * scrolling content is reached.</p>
     *
     * @param target The descendent view controlling the nested scroll
     * @param dxConsumed Horizontal scroll distance in pixels already consumed by target
     * @param dyConsumed Vertical scroll distance in pixels already consumed by target
     * @param dxUnconsumed Horizontal scroll distance in pixels not consumed by target
     * @param dyUnconsumed Vertical scroll distance in pixels not consumed by target
     */
    public void onNestedScroll(View target, int dxConsumed, int dyConsumed,
            int dxUnconsumed, int dyUnconsumed);

    /**
     * React to a nested scroll in progress before the target view consumes a portion of the scroll.
     *
     * <p>When working with nested scrolling often the parent view may want an opportunity
     * to consume the scroll before the nested scrolling child does. An example of this is a
     * drawer that contains a scrollable list. The user will want to be able to scroll the list
     * fully into view before the list itself begins scrolling.</p>
     *
     * <p><code>onNestedPreScroll</code> is called when a nested scrolling child invokes
     * {@link View#dispatchNestedPreScroll(int, int, int[], int[])}. The implementation should
     * report how any pixels of the scroll reported by dx, dy were consumed in the
     * <code>consumed</code> array. Index 0 corresponds to dx and index 1 corresponds to dy.
     * This parameter will never be null. Initial values for consumed[0] and consumed[1]
     * will always be 0.</p>
     *
     * @param target View that initiated the nested scroll
     * @param dx Horizontal scroll distance in pixels
     * @param dy Vertical scroll distance in pixels
     * @param consumed Output. The horizontal and vertical scroll distance consumed by this parent
     */
    public void onNestedPreScroll(View target, int dx, int dy, int[] consumed);

    /**
     * Request a fling from a nested scroll.
     *
     * <p>This method signifies that a nested scrolling child has detected suitable conditions
     * for a fling. Generally this means that a touch scroll has ended with a
     * {@link VelocityTracker velocity} in the direction of scrolling that meets or exceeds
     * the {@link ViewConfiguration#getScaledMinimumFlingVelocity() minimum fling velocity}
     * along a scrollable axis.</p>
     *
     * <p>If a nested scrolling child view would normally fling but it is at the edge of
     * its own content, it can use this method to delegate the fling to its nested scrolling
     * parent instead. The parent may optionally consume the fling or observe a child fling.</p>
     *
     * @param target View that initiated the nested scroll
     * @param velocityX Horizontal velocity in pixels per second
     * @param velocityY Vertical velocity in pixels per second
     * @param consumed true if the child consumed the fling, false otherwise
     * @return true if this parent consumed or otherwise reacted to the fling
     */
    public boolean onNestedFling(View target, float velocityX, float velocityY, boolean consumed);

    /**
     * React to a nested fling before the target view consumes it.
     *
     * <p>This method siginfies that a nested scrolling child has detected a fling with the given
     * velocity along each axis. Generally this means that a touch scroll has ended with a
     * {@link VelocityTracker velocity} in the direction of scrolling that meets or exceeds
     * the {@link ViewConfiguration#getScaledMinimumFlingVelocity() minimum fling velocity}
     * along a scrollable axis.</p>
     *
     * <p>If a nested scrolling parent is consuming motion as part of a
     * {@link #onNestedPreScroll(View, int, int, int[]) pre-scroll}, it may be appropriate for
     * it to also consume the pre-fling to complete that same motion. By returning
     * <code>true</code> from this method, the parent indicates that the child should not
     * fling its own internal content as well.</p>
     *
     * @param target View that initiated the nested scroll
     * @param velocityX Horizontal velocity in pixels per second
     * @param velocityY Vertical velocity in pixels per second
     * @return true if this parent consumed the fling ahead of the target view
     */
    public boolean onNestedPreFling(View target, float velocityX, float velocityY);

    /**
     * Return the current axes of nested scrolling for this NestedScrollingParent.
     *
     * <p>A NestedScrollingParent returning something other than {@link ViewCompat#SCROLL_AXIS_NONE}
     * is currently acting as a nested scrolling parent for one or more descendant views in
     * the hierarchy.</p>
     *
     * @return Flags indicating the current axes of nested scrolling
     * @see ViewCompat#SCROLL_AXIS_HORIZONTAL
     * @see ViewCompat#SCROLL_AXIS_VERTICAL
     * @see ViewCompat#SCROLL_AXIS_NONE
     */
    public int getNestedScrollAxes();
}

```

NestedScrollingChild接口：

```java

/**
 * This interface should be implemented by {@link android.view.View View} subclasses that wish
 * to support dispatching nested scrolling operations to a cooperating parent
 * {@link android.view.ViewGroup ViewGroup}.
 *
 * <p>Classes implementing this interface should create a final instance of a
 * {@link NestedScrollingChildHelper} as a field and delegate any View methods to the
 * <code>NestedScrollingChildHelper</code> methods of the same signature.</p>
 *
 * <p>Views invoking nested scrolling functionality should always do so from the relevant
 * {@link ViewCompat}, {@link ViewGroupCompat} or {@link ViewParentCompat} compatibility
 * shim static methods. This ensures interoperability with nested scrolling views on Android
 * 5.0 Lollipop and newer.</p>
 */
public interface NestedScrollingChild {
    /**
     * Enable or disable nested scrolling for this view.
     *
     * <p>If this property is set to true the view will be permitted to initiate nested
     * scrolling operations with a compatible parent view in the current hierarchy. If this
     * view does not implement nested scrolling this will have no effect. Disabling nested scrolling
     * while a nested scroll is in progress has the effect of {@link #stopNestedScroll() stopping}
     * the nested scroll.</p>
     *
     * @param enabled true to enable nested scrolling, false to disable
     *
     * @see #isNestedScrollingEnabled()
     */
    public void setNestedScrollingEnabled(boolean enabled);

    /**
     * Returns true if nested scrolling is enabled for this view.
     *
     * <p>If nested scrolling is enabled and this View class implementation supports it,
     * this view will act as a nested scrolling child view when applicable, forwarding data
     * about the scroll operation in progress to a compatible and cooperating nested scrolling
     * parent.</p>
     *
     * @return true if nested scrolling is enabled
     *
     * @see #setNestedScrollingEnabled(boolean)
     */
    public boolean isNestedScrollingEnabled();

    /**
     * Begin a nestable scroll operation along the given axes.
     *
     * <p>A view starting a nested scroll promises to abide by the following contract:</p>
     *
     * <p>The view will call startNestedScroll upon initiating a scroll operation. In the case
     * of a touch scroll this corresponds to the initial {@link MotionEvent#ACTION_DOWN}.
     * In the case of touch scrolling the nested scroll will be terminated automatically in
     * the same manner as {@link ViewParent#requestDisallowInterceptTouchEvent(boolean)}.
     * In the event of programmatic scrolling the caller must explicitly call
     * {@link #stopNestedScroll()} to indicate the end of the nested scroll.</p>
     *
     * <p>If <code>startNestedScroll</code> returns true, a cooperative parent was found.
     * If it returns false the caller may ignore the rest of this contract until the next scroll.
     * Calling startNestedScroll while a nested scroll is already in progress will return true.</p>
     *
     * <p>At each incremental step of the scroll the caller should invoke
     * {@link #dispatchNestedPreScroll(int, int, int[], int[]) dispatchNestedPreScroll}
     * once it has calculated the requested scrolling delta. If it returns true the nested scrolling
     * parent at least partially consumed the scroll and the caller should adjust the amount it
     * scrolls by.</p>
     *
     * <p>After applying the remainder of the scroll delta the caller should invoke
     * {@link #dispatchNestedScroll(int, int, int, int, int[]) dispatchNestedScroll}, passing
     * both the delta consumed and the delta unconsumed. A nested scrolling parent may treat
     * these values differently. See
     * {@link NestedScrollingParent#onNestedScroll(View, int, int, int, int)}.
     * </p>
     *
     * @param axes Flags consisting of a combination of {@link ViewCompat#SCROLL_AXIS_HORIZONTAL}
     *             and/or {@link ViewCompat#SCROLL_AXIS_VERTICAL}.
     * @return true if a cooperative parent was found and nested scrolling has been enabled for
     *         the current gesture.
     *
     * @see #stopNestedScroll()
     * @see #dispatchNestedPreScroll(int, int, int[], int[])
     * @see #dispatchNestedScroll(int, int, int, int, int[])
     */
    public boolean startNestedScroll(int axes);

    /**
     * Stop a nested scroll in progress.
     *
     * <p>Calling this method when a nested scroll is not currently in progress is harmless.</p>
     *
     * @see #startNestedScroll(int)
     */
    public void stopNestedScroll();

    /**
     * Returns true if this view has a nested scrolling parent.
     *
     * <p>The presence of a nested scrolling parent indicates that this view has initiated
     * a nested scroll and it was accepted by an ancestor view further up the view hierarchy.</p>
     *
     * @return whether this view has a nested scrolling parent
     */
    public boolean hasNestedScrollingParent();

    /**
     * Dispatch one step of a nested scroll in progress.
     *
     * <p>Implementations of views that support nested scrolling should call this to report
     * info about a scroll in progress to the current nested scrolling parent. If a nested scroll
     * is not currently in progress or nested scrolling is not
     * {@link #isNestedScrollingEnabled() enabled} for this view this method does nothing.</p>
     *
     * <p>Compatible View implementations should also call
     * {@link #dispatchNestedPreScroll(int, int, int[], int[]) dispatchNestedPreScroll} before
     * consuming a component of the scroll event themselves.</p>
     *
     * @param dxConsumed Horizontal distance in pixels consumed by this view during this scroll step
     * @param dyConsumed Vertical distance in pixels consumed by this view during this scroll step
     * @param dxUnconsumed Horizontal scroll distance in pixels not consumed by this view
     * @param dyUnconsumed Horizontal scroll distance in pixels not consumed by this view
     * @param offsetInWindow Optional. If not null, on return this will contain the offset
     *                       in local view coordinates of this view from before this operation
     *                       to after it completes. View implementations may use this to adjust
     *                       expected input coordinate tracking.
     * @return true if the event was dispatched, false if it could not be dispatched.
     * @see #dispatchNestedPreScroll(int, int, int[], int[])
     */
    public boolean dispatchNestedScroll(int dxConsumed, int dyConsumed,
            int dxUnconsumed, int dyUnconsumed, int[] offsetInWindow);

    /**
     * Dispatch one step of a nested scroll in progress before this view consumes any portion of it.
     *
     * <p>Nested pre-scroll events are to nested scroll events what touch intercept is to touch.
     * <code>dispatchNestedPreScroll</code> offers an opportunity for the parent view in a nested
     * scrolling operation to consume some or all of the scroll operation before the child view
     * consumes it.</p>
     *
     * @param dx Horizontal scroll distance in pixels
     * @param dy Vertical scroll distance in pixels
     * @param consumed Output. If not null, consumed[0] will contain the consumed component of dx
     *                 and consumed[1] the consumed dy.
     * @param offsetInWindow Optional. If not null, on return this will contain the offset
     *                       in local view coordinates of this view from before this operation
     *                       to after it completes. View implementations may use this to adjust
     *                       expected input coordinate tracking.
     * @return true if the parent consumed some or all of the scroll delta
     * @see #dispatchNestedScroll(int, int, int, int, int[])
     */
    public boolean dispatchNestedPreScroll(int dx, int dy, int[] consumed, int[] offsetInWindow);

    /**
     * Dispatch a fling to a nested scrolling parent.
     *
     * <p>This method should be used to indicate that a nested scrolling child has detected
     * suitable conditions for a fling. Generally this means that a touch scroll has ended with a
     * {@link VelocityTracker velocity} in the direction of scrolling that meets or exceeds
     * the {@link ViewConfiguration#getScaledMinimumFlingVelocity() minimum fling velocity}
     * along a scrollable axis.</p>
     *
     * <p>If a nested scrolling child view would normally fling but it is at the edge of
     * its own content, it can use this method to delegate the fling to its nested scrolling
     * parent instead. The parent may optionally consume the fling or observe a child fling.</p>
     *
     * @param velocityX Horizontal fling velocity in pixels per second
     * @param velocityY Vertical fling velocity in pixels per second
     * @param consumed true if the child consumed the fling, false otherwise
     * @return true if the nested scrolling parent consumed or otherwise reacted to the fling
     */
    public boolean dispatchNestedFling(float velocityX, float velocityY, boolean consumed);

    /**
     * Dispatch a fling to a nested scrolling parent before it is processed by this view.
     *
     * <p>Nested pre-fling events are to nested fling events what touch intercept is to touch
     * and what nested pre-scroll is to nested scroll. <code>dispatchNestedPreFling</code>
     * offsets an opportunity for the parent view in a nested fling to fully consume the fling
     * before the child view consumes it. If this method returns <code>true</code>, a nested
     * parent view consumed the fling and this view should not scroll as a result.</p>
     *
     * <p>For a better user experience, only one view in a nested scrolling chain should consume
     * the fling at a time. If a parent view consumed the fling this method will return false.
     * Custom view implementations should account for this in two ways:</p>
     *
     * <ul>
     *     <li>If a custom view is paged and needs to settle to a fixed page-point, do not
     *     call <code>dispatchNestedPreFling</code>; consume the fling and settle to a valid
     *     position regardless.</li>
     *     <li>If a nested parent does consume the fling, this view should not scroll at all,
     *     even to settle back to a valid idle position.</li>
     * </ul>
     *
     * <p>Views should also not offer fling velocities to nested parent views along an axis
     * where scrolling is not currently supported; a {@link android.widget.ScrollView ScrollView}
     * should not offer a horizontal fling velocity to its parents since scrolling along that
     * axis is not permitted and carrying velocity along that motion does not make sense.</p>
     *
     * @param velocityX Horizontal fling velocity in pixels per second
     * @param velocityY Vertical fling velocity in pixels per second
     * @return true if a nested scrolling parent consumed the fling
     */
    public boolean dispatchNestedPreFling(float velocityX, float velocityY);
}

```

NestedScrollingParentHelper接口：
```java
/**
 * Helper class for implementing nested scrolling parent views compatible with Android platform
 * versions earlier than Android 5.0 Lollipop (API 21).
 *
 * <p>{@link android.view.ViewGroup ViewGroup} subclasses should instantiate a final instance
 * of this class as a field at construction. For each <code>ViewGroup</code> method that has
 * a matching method signature in this class, delegate the operation to the helper instance
 * in an overridden method implementation. This implements the standard framework policy
 * for nested scrolling.</p>
 *
 * <p>Views invoking nested scrolling functionality should always do so from the relevant
 * {@link android.support.v4.view.ViewCompat}, {@link android.support.v4.view.ViewGroupCompat} or
 * {@link android.support.v4.view.ViewParentCompat} compatibility
 * shim static methods. This ensures interoperability with nested scrolling views on Android
 * 5.0 Lollipop and newer.</p>
 */
public class NestedScrollingParentHelper {
    private final ViewGroup mViewGroup;
    private int mNestedScrollAxes;

    /**
     * Construct a new helper for a given ViewGroup
     */
    public NestedScrollingParentHelper(ViewGroup viewGroup) {
        mViewGroup = viewGroup;
    }

    /**
     * Called when a nested scrolling operation initiated by a descendant view is accepted
     * by this ViewGroup.
     *
     * <p>This is a delegate method. Call it from your {@link android.view.ViewGroup ViewGroup}
     * subclass method/{@link android.support.v4.view.NestedScrollingParent} interface method with
     * the same signature to implement the standard policy.</p>
     */
    public void onNestedScrollAccepted(View child, View target, int axes) {
        mNestedScrollAxes = axes;
    }

    /**
     * Return the current axes of nested scrolling for this ViewGroup.
     *
     * <p>This is a delegate method. Call it from your {@link android.view.ViewGroup ViewGroup}
     * subclass method/{@link android.support.v4.view.NestedScrollingParent} interface method with
     * the same signature to implement the standard policy.</p>
     */
    public int getNestedScrollAxes() {
        return mNestedScrollAxes;
    }

    /**
     * React to a nested scroll operation ending.
     *
     * <p>This is a delegate method. Call it from your {@link android.view.ViewGroup ViewGroup}
     * subclass method/{@link android.support.v4.view.NestedScrollingParent} interface method with
     * the same signature to implement the standard policy.</p>
     *
     * @param target View that initiated the nested scroll
     */
    public void onStopNestedScroll(View target) {
        mNestedScrollAxes = 0;
    }
}

```


NestedScrollingChildHelper辅助类：
```java

/**
 * Helper class for implementing nested scrolling child views compatible with Android platform
 * versions earlier than Android 5.0 Lollipop (API 21).
 *
 * <p>{@link android.view.View View} subclasses should instantiate a final instance of this
 * class as a field at construction. For each <code>View</code> method that has a matching
 * method signature in this class, delegate the operation to the helper instance in an overridden
 * method implementation. This implements the standard framework policy for nested scrolling.</p>
 *
 * <p>Views invoking nested scrolling functionality should always do so from the relevant
 * {@link android.support.v4.view.ViewCompat}, {@link android.support.v4.view.ViewGroupCompat} or
 * {@link android.support.v4.view.ViewParentCompat} compatibility
 * shim static methods. This ensures interoperability with nested scrolling views on Android
 * 5.0 Lollipop and newer.</p>
 */
public class NestedScrollingChildHelper {
    private final View mView;
    private ViewParent mNestedScrollingParent;
    private boolean mIsNestedScrollingEnabled;
    private int[] mTempNestedScrollConsumed;

    /**
     * Construct a new helper for a given view.
     */
    public NestedScrollingChildHelper(View view) {
        mView = view;
    }

    /**
     * Enable nested scrolling.
     *
     * <p>This is a delegate method. Call it from your {@link android.view.View View} subclass
     * method/{@link android.support.v4.view.NestedScrollingChild} interface method with the same
     * signature to implement the standard policy.</p>
     *
     * @param enabled true to enable nested scrolling dispatch from this view, false otherwise
     */
    public void setNestedScrollingEnabled(boolean enabled) {
        if (mIsNestedScrollingEnabled) {
            ViewCompat.stopNestedScroll(mView);
        }
        mIsNestedScrollingEnabled = enabled;
    }

    /**
     * Check if nested scrolling is enabled for this view.
     *
     * <p>This is a delegate method. Call it from your {@link android.view.View View} subclass
     * method/{@link android.support.v4.view.NestedScrollingChild} interface method with the same
     * signature to implement the standard policy.</p>
     *
     * @return true if nested scrolling is enabled for this view
     */
    public boolean isNestedScrollingEnabled() {
        return mIsNestedScrollingEnabled;
    }

    /**
     * Check if this view has a nested scrolling parent view currently receiving events for
     * a nested scroll in progress.
     *
     * <p>This is a delegate method. Call it from your {@link android.view.View View} subclass
     * method/{@link android.support.v4.view.NestedScrollingChild} interface method with the same
     * signature to implement the standard policy.</p>
     *
     * @return true if this view has a nested scrolling parent, false otherwise
     */
    public boolean hasNestedScrollingParent() {
        return mNestedScrollingParent != null;
    }

    /**
     * Start a new nested scroll for this view.
     *
     * <p>This is a delegate method. Call it from your {@link android.view.View View} subclass
     * method/{@link android.support.v4.view.NestedScrollingChild} interface method with the same
     * signature to implement the standard policy.</p>
     *
     * @param axes Supported nested scroll axes.
     *             See {@link android.support.v4.view.NestedScrollingChild#startNestedScroll(int)}.
     * @return true if a cooperating parent view was found and nested scrolling started successfully
     */
    public boolean startNestedScroll(int axes) {
        if (hasNestedScrollingParent()) {
            // Already in progress
            return true;
        }
        if (isNestedScrollingEnabled()) {
            ViewParent p = mView.getParent();
            View child = mView;
            while (p != null) {
                if (ViewParentCompat.onStartNestedScroll(p, child, mView, axes)) {
                    mNestedScrollingParent = p;
                    ViewParentCompat.onNestedScrollAccepted(p, child, mView, axes);
                    return true;
                }
                if (p instanceof View) {
                    child = (View) p;
                }
                p = p.getParent();
            }
        }
        return false;
    }

    /**
     * Stop a nested scroll in progress.
     *
     * <p>This is a delegate method. Call it from your {@link android.view.View View} subclass
     * method/{@link android.support.v4.view.NestedScrollingChild} interface method with the same
     * signature to implement the standard policy.</p>
     */
    public void stopNestedScroll() {
        if (mNestedScrollingParent != null) {
            ViewParentCompat.onStopNestedScroll(mNestedScrollingParent, mView);
            mNestedScrollingParent = null;
        }
    }

    /**
     * Dispatch one step of a nested scrolling operation to the current nested scrolling parent.
     *
     * <p>This is a delegate method. Call it from your {@link android.view.View View} subclass
     * method/{@link android.support.v4.view.NestedScrollingChild} interface method with the same
     * signature to implement the standard policy.</p>
     *
     * @return true if the parent consumed any of the nested scroll
     */
    public boolean dispatchNestedScroll(int dxConsumed, int dyConsumed,
            int dxUnconsumed, int dyUnconsumed, int[] offsetInWindow) {
        if (isNestedScrollingEnabled() && mNestedScrollingParent != null) {
            if (dxConsumed != 0 || dyConsumed != 0 || dxUnconsumed != 0 || dyUnconsumed != 0) {
                int startX = 0;
                int startY = 0;
                if (offsetInWindow != null) {
                    mView.getLocationInWindow(offsetInWindow);
                    startX = offsetInWindow[0];
                    startY = offsetInWindow[1];
                }

                ViewParentCompat.onNestedScroll(mNestedScrollingParent, mView, dxConsumed,
                        dyConsumed, dxUnconsumed, dyUnconsumed);

                if (offsetInWindow != null) {
                    mView.getLocationInWindow(offsetInWindow);
                    offsetInWindow[0] -= startX;
                    offsetInWindow[1] -= startY;
                }
                return true;
            } else if (offsetInWindow != null) {
                // No motion, no dispatch. Keep offsetInWindow up to date.
                offsetInWindow[0] = 0;
                offsetInWindow[1] = 0;
            }
        }
        return false;
    }

    /**
     * Dispatch one step of a nested pre-scrolling operation to the current nested scrolling parent.
     *
     * <p>This is a delegate method. Call it from your {@link android.view.View View} subclass
     * method/{@link android.support.v4.view.NestedScrollingChild} interface method with the same
     * signature to implement the standard policy.</p>
     *
     * @return true if the parent consumed any of the nested scroll
     */
    public boolean dispatchNestedPreScroll(int dx, int dy, int[] consumed, int[] offsetInWindow) {
        if (isNestedScrollingEnabled() && mNestedScrollingParent != null) {
            if (dx != 0 || dy != 0) {
                int startX = 0;
                int startY = 0;
                if (offsetInWindow != null) {
                    mView.getLocationInWindow(offsetInWindow);
                    startX = offsetInWindow[0];
                    startY = offsetInWindow[1];
                }

                if (consumed == null) {
                    if (mTempNestedScrollConsumed == null) {
                        mTempNestedScrollConsumed = new int[2];
                    }
                    consumed = mTempNestedScrollConsumed;
                }
                consumed[0] = 0;
                consumed[1] = 0;
                ViewParentCompat.onNestedPreScroll(mNestedScrollingParent, mView, dx, dy, consumed);

                if (offsetInWindow != null) {
                    mView.getLocationInWindow(offsetInWindow);
                    offsetInWindow[0] -= startX;
                    offsetInWindow[1] -= startY;
                }
                return consumed[0] != 0 || consumed[1] != 0;
            } else if (offsetInWindow != null) {
                offsetInWindow[0] = 0;
                offsetInWindow[1] = 0;
            }
        }
        return false;
    }

    /**
     * Dispatch a nested fling operation to the current nested scrolling parent.
     *
     * <p>This is a delegate method. Call it from your {@link android.view.View View} subclass
     * method/{@link android.support.v4.view.NestedScrollingChild} interface method with the same
     * signature to implement the standard policy.</p>
     *
     * @return true if the parent consumed the nested fling
     */
    public boolean dispatchNestedFling(float velocityX, float velocityY, boolean consumed) {
        if (isNestedScrollingEnabled() && mNestedScrollingParent != null) {
            return ViewParentCompat.onNestedFling(mNestedScrollingParent, mView, velocityX,
                    velocityY, consumed);
        }
        return false;
    }

    /**
     * Dispatch a nested pre-fling operation to the current nested scrolling parent.
     *
     * <p>This is a delegate method. Call it from your {@link android.view.View View} subclass
     * method/{@link android.support.v4.view.NestedScrollingChild} interface method with the same
     * signature to implement the standard policy.</p>
     *
     * @return true if the parent consumed the nested fling
     */
    public boolean dispatchNestedPreFling(float velocityX, float velocityY) {
        if (isNestedScrollingEnabled() && mNestedScrollingParent != null) {
            return ViewParentCompat.onNestedPreFling(mNestedScrollingParent, mView, velocityX,
                    velocityY);
        }
        return false;
    }

    /**
     * View subclasses should always call this method on their
     * <code>NestedScrollingChildHelper</code> when detached from a window.
     *
     * <p>This is a delegate method. Call it from your {@link android.view.View View} subclass
     * method/{@link android.support.v4.view.NestedScrollingChild} interface method with the same
     * signature to implement the standard policy.</p>
     */
    public void onDetachedFromWindow() {
        ViewCompat.stopNestedScroll(mView);
    }

    /**
     * Called when a nested scrolling child stops its current nested scroll operation.
     *
     * <p>This is a delegate method. Call it from your {@link android.view.View View} subclass
     * method/{@link android.support.v4.view.NestedScrollingChild} interface method with the same
     * signature to implement the standard policy.</p>
     *
     * @param child Child view stopping its nested scroll. This may not be a direct child view.
     */
    public void onStopNestedScroll(View child) {
        ViewCompat.stopNestedScroll(mView);
    }
}

```