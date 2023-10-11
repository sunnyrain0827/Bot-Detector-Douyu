# Real Time Bot Detection And Heat Analysis

## 1.  Problem Definition
The main problem addressed is the presence of bots in live video streams that generate fake audiences and fake barrages to inflate the popularity of a host. The goal is to detect these bots in real-time and calculate the real heat of the hosts. Additionally, the system aims to analyze the increase in barrages and identify high-frequency words for host operation analysis.


## 2.  Challenges Faced
The challenges in this project include communication between batches, developing effective algorithms for bot detection and heat calculation, real-time visualization of data, determining suitable parameters, and designing unit tests.

## 3.   Description of The Technical Details
Call the API to get the room information. The information includes barrage, gifts, online audience from open.douyu.com.
### 3.1  Algorithm
### 3.1.1  Deal with Barrage: 
Accumulate all barrages to generate a current barrage dictionary.
Divide the streaming data into real bots and suspect bots based on records.
For real bots, accumulate their barrages into a barrage dictionary.
For suspect bots, assign scores based on user level, barrage content, interval, and current barrage. A score threshold determines if it's a bot.

### 3.1.2  Deal with Barrage: 
Accumulate the quantity of current barrages.
Split barrages into words and count the occurrences of each word.
 
### 3.1.3  Deal with gift:
Assign weighted points to each gift type.
Count the number of gifts of each category over a certain period and calculate weighted gift points.
  
### 3.1.4  Calculate heat:
- Every second, call the api to store the current information.
- Every 10 seconds, calculate the average online number of each 1-second time period.
- Every 10 seconds, utilize the processed gift and barrage information to calculate the current heat, the And the formula is :

online_number = (curr_number / pre_number) * 100                     (3-1)
curr_heat = pre_heat * α+ (online_number * 0.6 + gift * 0.2 + barrage * 0.2) * (1-α) (3-2)
pre_heat = curr_heat                                                 (3-3)
Update the data in the window size which is equal to 5. 
Store the curr_heat as pre_heat.

### 3.2  Real-time Data Visualization
 
Data is stored in Firebase, and the frontend is notified of new data using Firebase's "on" event, enabling real-time data visualization.
 

## 4  Streaming/Distributed Processing Used
Not fully exploited now, For barrage data is naturally streaming data, we use the streaming processing in the following way.
We get the barrage data and store them in a directory, the files in the directory create a stream. 
We use map, filter, reducebykey for the streaming processing. In our cases,
We use stream processing in: 
- bot detection process.
- counting the barrage.
- counting the number of different chat message.
- counting the gifts.

## 5  Streaming Algorithm Could Have Been Used
In the cases of counting the number of different chat message and pick the most frequent chat message process, it is actually the problem of finding the majority element of a stream. 
In this case, we could apply the Boyer–Moore majority vote algorithm which will only use O(1) space for one counter, and one element.

## 6  Future Work
Apply machine learning for more accurate bot detection.
Extend the system to handle multiple room information and identify the top 5 popular rooms.
Predict an anchor's potential heat and future hot topics.
Improve the thesaurus for better word analysis.


Contributors: Zirui Tan, Zichen Liu, Haopeng Zhang, Yuting Wang

References
[1] https://spark.apache.org/docs/latest/
[2] https://firebase.google.com/docs
[3] https://open.douyu.com/

