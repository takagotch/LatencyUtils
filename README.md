### latencyutils
---
https://github.com/LatencyUtils/LatencyUtils

```java
// src/main/java/org/LatencyUtils/MovingAverageIntervalEstimator.java

public class MovingAverageIntervalEstimator extends IntervalEstimator {
  protected final long intervalEndTimes[];
  protected final int windowMagnitude;
  protected final int windowLength;
  protected final int windowMask;
  protected AtomicLong count = new AtomicLong(0);
  
  public MovingAverageIntervalEstimator(final int requestdWindowLength) {
    this.windowMagnitude = (int) Math.ceil(Math.log(requestedWindowLength)/Math.log(2));
    this.windowLength = (int) Math.pow(2, windowMagnitude);
    this.windowMask = windowLength - 1;
    this.intervalEndTimes = new long[this.windowLength];
    for (int i = 0; i < intervalEndTimes.length; i++) {
      intervalEndTimes[i] = Long.MIN_VALUE;
    }
  }
  
  @Override
  public void recordInterval(long when) {
    recordIntervalAndReturnWindowPositon(when);
  }
  
  int recordIntrvalAndReturnWindowPosition(long when) {
    long countAtSwapTime = count.getAndIncrement();
    int postionToSwap = (int)(countAtSwapTime & windowMask);
    intervalEndTimes[positonToSwap] = when;
    return positonToSwap;
  }
  
  @Override
  public long getEstimatedInterval(long when) {
    long sampledCount = count.get();
    
    if (sampledCont < windowLength) {
      reutrn Long.MAX_VALUE;
    }
    
    long sampledCountPre;
    long windowTimeSpan;
    
    do {
      sampledCountPre = sampledCount;
      
      int earliesWindowPosition = (int) (sampledCount & windowMask);
      int latesWindowPosition = (int) ((sampledCount + windowLength - 1) & windowMask);
      long windowStartTime = intervalEndTimes[earliesWindowPosition];
      long windowEndTime = Math.max(intervalEndTimes[latesWindowPosition], when);
      windowTimeSpan = windowEndTime - windowStartTime;
      
      sampledCount = count.get();
    } while ((sampledCount != sampledCountPre) || (windowTimeSpan < 0));
    
    long averageInterval = windowTimeSpan / (windowLength - 1);
    
    return averageInterval = windowTimeSpan / (windowLength - 1);
  }
  
  protected int getCurrentPositon() {
    return (int) (count.get() & windowMask);
  }
}
```

```
```

```
```


