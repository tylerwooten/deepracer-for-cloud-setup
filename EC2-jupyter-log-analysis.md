## Setting up Jupyter notebook log analysis:
* Run “dr-start-loganalysis” on ec2
* Copy the URL that it outputs and replace the ip with your ec2’s ip: “http://ubuntu@{IP HERE}.compute-1.amazonaws.com:8888/lab?”
* Paste the url above into your local machine’s internet browser
* May prompt you to log in — paste the token that is at the end of the Url from your ec2

## Running log analysis:
* After creating the logs with the [log creation bash script](EC2-log-creation.md)
* Open and run Training_analysis.ipynb
* Either download [this .ipynb file](Training_analysis_updated.ipynb) and upload it to your Jupyter Notebook instance, or add the following at the end of the file for detailed analysis on how your model is performing:
```python
all_completed_laps = big_complete_ones[big_complete_ones['time']<200]
all_laps = big_simulation_agg
x=all_completed_laps['new_reward']
y=all_completed_laps['time']
plt.scatter(x,y)
plt.xlabel('Reward')
plt.ylabel('Time')
plt.show()
```

```
last_20_percent_laps = big_simulation_agg.tail( round( len(big_simulation_agg) * 0.2 ) )
last_20_percent_completed = last_20_percent_laps[last_20_percent_laps['progress']==100]

first_20_percent_laps = big_simulation_agg.head( round( len(big_simulation_agg) * 0.2 ) )
first_20_percent_completed = first_20_percent_laps[first_20_percent_laps['progress']==100]
```

```python
import numpy as np
x=all_completed_laps
y1=first_20_percent_completed
y2=last_20_percent_completed

values, counts = np.unique(y1.steps, return_counts=True)
first_20_plt = plt.vlines(values, 0, counts, color='C0', lw=6, label='First 20%')

values, counts = np.unique(y2.steps, return_counts=True)
last_20_plt = plt.vlines(values, 0, counts, color='red', lw=2, label='Last 20%')

plt.xlabel('Steps')
plt.ylabel('Occurences')
plt.title('Performance of Model Steps: First 20% vs Last 20%')
plt.legend(handles=[first_20_plt, last_20_plt])
plt.show()

print('A graph with red shifted more left than blue means the model has decreased the average number of steps over this training. This means the model is completing laps in less steps (good indicator that speed is increasing).')
```

```python
# laptime trend
from scipy import stats

jx=all_completed_laps['episode']
jy=all_completed_laps['time']

slope, intercept, r_value, p_value, std_err = stats.linregress(jx,jy)
line = slope*jx+intercept
plt.plot(jx, line, 'r', label='y={:.2f}x+{:.2f}'.format(slope,intercept))

plt.scatter(jx,jy)
plt.legend(fontsize=9)
plt.xlabel('Episode')
plt.ylabel('Time')
plt.title('Lap Completion Time per Episode')
plt.show()

print("A line that is down to the right signifies a model that is improving it's lap time.")
```

```python
print( 'Completed Laps:', len(all_completed_laps) )
print( 'Total Laps:', len(all_laps), '\n' )

print( 'Completion Rate:', len(all_completed_laps)/len(all_laps) )
print( 'Avg Time:', all_completed_laps['time'].mean(), '\n')

print( 'Completion Rate First 20%:', len(first_20_percent_completed)/len(first_20_percent_laps) )
print( 'Avg Time First 20%:', first_20_percent_completed['time'].mean(), '\n' )

print( 'Completion Rate Last 20%:', len(last_20_percent_completed)/len(last_20_percent_laps) )
print( 'Avg Time Last 20%:', last_20_percent_completed['time'].mean(), '\n' )

all_offtrack_laps = all_laps[all_laps['progress']!=100]
print('Stability Ratio:', len(all_completed_laps)/len(all_offtrack_laps))
```
