# Java 版 WaitGroup

|    data    | tag |
|    ---     | --- |
| 2020/09/11 | AQS |

## 代码如下

> note: `Sync` 添加 `addToken` 方法

```java

//:WaitGroup.java

import java.io.Serializable;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.AbstractQueuedSynchronizer;

public class WaitGroup implements Serializable {

    private static final class Sync extends AbstractQueuedSynchronizer {
        private static final long serialVersionUID = 4982264981922014374L;

        Sync(int count) {
            setState(count);
        }

        int getCount() {
            return getState();
        }

        protected int tryAcquireShared(int acquires) {
            return (getState() == 0) ? 1 : -1;
        }

        protected boolean tryReleaseShared(int releases) {
            for (;;) {
                int c = getState();
                if (c == 0)
                    return false;
                int nextc = c - 1;
                if (compareAndSetState(c, nextc))
                    return nextc == 0;
            }
        }

        /**
        * add state
        */
        protected void addToken(int releases) {
            for (;;) {
                int c = getState();
                int nextc = c + releases;
                if (compareAndSetState(c, nextc))
                    break;
            }
        }
    }

    private final WaitGroup.Sync sync;

    public WaitGroup() {
        this.sync = new WaitGroup.Sync(0);
    }

    public WaitGroup(int count) {
        if (count < 0) throw new IllegalArgumentException("count < 0");
        this.sync = new WaitGroup.Sync(count);
    }

    public void await() throws InterruptedException {
        sync.acquireSharedInterruptibly(1);
    }

    public boolean await(long timeout, TimeUnit unit)
        throws InterruptedException {
        return sync.tryAcquireSharedNanos(1, unit.toNanos(timeout));
    }

    public void done() {
        sync.releaseShared(1);
    }

    public void add() {
        sync.addToken(1);
    }

    public long getCount() {
        return sync.getCount();
    }

    public String toString() {
        return super.toString() + "[Count = " + sync.getCount() + "]";
    }
}
//:~
```
