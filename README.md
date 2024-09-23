# test111



%matplotlib inline
import numpy as np
import matplotlib.pyplot as plt


np.random.seed()
pattern =(np.linspace(0, 4*np.pi,60)) 
noise = np.random.normal(0,60,1440)  
pollution_data = pattern + noise

def hourly_average(data):
    hourly_averages = np.mean(data.reshape(-1, 60), axis=1)
    return hourly_averages

def low_pass_filter(data,cutoff=0.1,fs=1.0,order=5):
    nyquist = 0.5*fs
    normal_cutoff = cutoff/nyquist
    b,a = butter(order,normal_cutoff,btype='low',analog=False)
    filtereddata = filtfilt(b,a,data)
    return filtered_data
filtered_pollution_data = low_pass_filter(pollution_data)


    
plt.figure(figsize=(14, 7))
plt.plot(pollution_data,color='blue',alpha=0.5)
plt.plot(filtered_pollution_data,color='orange')

hazardous_hours = np.where(avg_pm25_per_hour > 150)[0]
for hour in hazardous_hours:
    plt.barh(hour*60,(hour+1)*60,color='red',alpha=0.2)

plt.title('levels Over 24 Hours')
plt.xlabel('time')
plt.ylabel('PM2.5 levels')
plt.show()
    
