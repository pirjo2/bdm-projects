# BigData 2025 Template Repository

![TartuLogo](./images/logo_ut_0.png)

Project [Big Data](https://courses.cs.ut.ee/2025/bdm/spring/Main/HomePage) is provided by [University of Tartu](https://courses.cs.ut.ee/).

Students: Anna Maria Tammin, Maria Anett Kaha, Pirjo Vainjärv, Eidi Paas

## Queries 
### Query 1 - Computing utilization, idle time per taxi
The dataset was processed to calculate the idle time for each taxi driver. Repartition was used to get all trips of the same driver. The window function was used to get the previous drop-off time for each driver. The idle time was calculated by subtracting the previous drop-off time from the pick-up time. Then the data frame was filtered based on the idle times (times greater than 4 hours were removed), also we excluded cases where pickup time was earlier than the prev_dropoff time, just to avoid overlapping. Finally, the sum of the idle time of each taxi driver was calculated. Then the total_idle_time, total_ride_duration, and total_time were used to compute utilization_rate.   
#### Top 10 rows :
![pilt](https://github.com/user-attachments/assets/af72ab3b-8f06-467a-a615-003074248860)

*** We also computed an extra average utilization rate and got it 0.50287482780684.

### Query 2 - The average time it takes for a taxi to find its next fare(trip) per destination borough
This query calculates the average time a taxi waits for its next fare after dropping off a passenger in each borough. First, each trip is ordered by pickup time for each driver (hack_license), and the next pickup time is retrieved using a window function. The waiting time is then calculated as the difference between the next pickup and the previous dropoff time. To clean the data, rows with missing values or unusually long waiting times (over 4h) are removed. The average waiting time is then calculated for each dropoff borough, and borough codes are mapped to their names. The results are visualized in a bar plot, showing boroughs on the y-axis and their average waiting times in seconds on the x-axis.

Graph:

![pilt](https://github.com/user-attachments/assets/e9a2e50a-266d-41f8-8701-b616b4dfb780)
This bar chart shows the average taxi waiting time by drop-off borough, based on 20,000 rows of data.
Staten Island has the longest waiting time, much higher than other boroughs. This likely means fewer taxis and lower demand.
Manhattan has the shortest waiting time, which makes sense because of its high taxi availability and frequent ride requests.
Bronx, Brooklyn, and Queens fall in between, with Queens having slightly longer waits. This may be due to different demand levels and taxi availability.
Overall, the data suggests that busier areas like Manhattan have shorter waiting times, while quieter areas like Staten Island experience longer gaps between rides.

### Query 3 - The number of trips that started and ended within the same borough
In order to calculate how many taxi trips started and ended within the same borough, we filtered the given dataset to keep only trips where the pickup and dropoff borough were the same. Then data was grouped by the pickup borough to count the number of trips within each borough. First the boroughs were in the form of numerical codes, we created a DataFrame  borough_names_df  to give the borough the official names (Manhattan, Brooklyn, Queens, Bronx, Staten Island). 

Graph: 

![pilt](https://github.com/user-attachments/assets/85cb9131-7eb4-49ac-a115-2041ce154f85)
From these results, based on 20 000 rows of data, it is shown that the most same-borough trips are dominating in Manhattan, Staten Island has the lowest number of same-borough trips. 

### Query 4 - The number of trips that started in one borough and ended in another one
To analyze the trips between different boroughs, the dataset was filtered to count the number of trips where the pickup and dropoff locations were in different boroughs. We also grouped the data by pickup and dropoff borough and summed the number of such trips in every possible variation. The result, stored in different_borough_trips_df  ,represented the numbers of such trips. We again gave  the boroughs the names instead of the numeric code. The result on number of trips that started in one borough and ended in another one resulted in 3065 trips based on 20 000 rows of data.  

### Conclusions
The project successfully analyzed New York City taxi data by applying structured data transformations and borough mapping. The results showed that the average taxi utilization rate was 50.29%, with an average idle time of 10,895.78 seconds (approximately 3 hours). Manhattan had the shortest waiting time, while Staten Island had the longest, with an average idle time exceeding 4 hours in some cases. Based on 20,000 trips, most same-borough trips occurred in Manhattan, while Staten Island had the fewest. Inter-borough trips followed expected patterns, with high traffic between neighboring boroughs. These insights provide a clearer understanding of taxi movement and demand distribution in New York City.

## Requirements

    Start Docker Desktop App
    Open project_n folder in the command line
    Run "docker compose up -d"
    Go to page: http://localhost:8888/
    Run "docker compose down"

## Note for Students

* Clone the created repository offline;
* Add your name and surname into the Readme file and your teammates as collaborators
* Complete the field above 
* Make any changes to your repository according to the specific assignment;
* Ensure code reproducibility and instructions on how to replicate the results;
* Add an open-source license, e.g., Apache 2.0;
* convert README in pdf
* keep one report for all projects

