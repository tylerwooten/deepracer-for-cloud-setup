## Setting up Jupyter notebook log analysis:
* Run “dr-start-loganalysis” on ec2
* Copy the URL that it outputs and replace the ip with your ec2’s ip: “http://ubuntu@{IP HERE}.compute-1.amazonaws.com:8888/lab?”
* Paste the url above into your local machine’s internet browser
* May prompt you to log in — paste the token that is at the end of the Url from your ec2

## Running log analysis:
* After creating the logs with the [log creation bash script](EC2-log-creation.md)
* Open and run Training_analysis.ipynb
* Add the following at the end of the file for detailed analysis on how your model is performing:
```
lastTenPercentLaps = simulation_agg.tail( round( len(simulation_agg) * 0.1 ) )
lastTenPercentCompleted = lastTenPercentLaps[lastTenPercentLaps['progress']==100]
firstTenPercentLaps = simulation_agg.head( round( len(simulation_agg) * 0.1 ) )
firstTenPercentCompleted = firstTenPercentLaps[firstTenPercentLaps['progress']==100]
```

```python
import numpy as np
x=complete_ones[complete_ones['steps']>=50]
x=x[x['steps']<445]
x=x[x['time']>7]

first20=x.iteration<x.iteration.max()*0.2
last20=x.iteration>x.iteration.max()*0.8
y=x[first20]
z=x[last20]

values, counts = np.unique(y.steps, return_counts=True)
first20plt = plt.vlines(values, 0, counts, color='C0', lw=6, label='First 20%')
values, counts = np.unique(z.steps, return_counts=True)
last20plt = plt.vlines(values, 0, counts, color='red', lw=2, label='Last 20%')
plt.xlabel('Steps')
plt.ylabel('Occurences')
plt.title('Performance of Model Steps: First 20% vs Last 20%')
plt.legend(handles=[first20plt, last20plt])
plt.show()

print('A graph with red shifted more left than blue means the model has decreased the average number of steps over this training. This means the model is getting faster.')
```

```python
# laptime trend
from scipy import stats
jx=complete_ones[complete_ones['time']>7]['episode']
jy=complete_ones[complete_ones['time']>7]['time']
slope, intercept, r_value, p_value, std_err = stats.linregress(jx,jy)
line = slope*jx+intercept
plt.plot(jx, line, 'r', label='y={:.2f}x+{:.2f}'.format(slope,intercept))
plt.scatter(jx,jy)
plt.legend(fontsize=9)
plt.xlabel('Episode')
plt.ylabel('Time')
plt.title('Lap Completed Time per Episode')
plt.show()
print("A line that is down to the right signifies a model that is improving it's lap time.")
```

```python
print( 'Completed:', len(complete_ones) )
print( 'Total:', len(simulation_agg), '\n' )

print( 'Completion Rate:', len(complete_ones)/len(simulation_agg), '\n' )

print( 'Completion Rate First 10%:', len(firstTenPercentCompleted)/len(firstTenPercentLaps) )
print( 'Avg Speed first 10%:', sum(firstTenPercentCompleted['time'])/len(firstTenPercentCompleted['time']), '\n' )

print( 'Completion Rate Last 10%:', len(lastTenPercentCompleted)/len(lastTenPercentLaps) )
print( 'Avg Speed last 10%:', sum(lastTenPercentCompleted['time'])/len(lastTenPercentCompleted['time']) )
```
