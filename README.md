# Predict-Hotel-Cancellation-With-Spark
I, Data Description:
   - Dataset contains booking information for a city hotel and a resort hotel, and includes information such as when the booking was made, length of stay, the number of adults, children, and/or babies, and the number of available parking spaces, among other things.
   - Link: https://www.kaggle.com/jessemostipak/hotel-booking-demand
   - Dataset include: 32 features x 119390 row
   - Data Semantics: 
   
   ![image](https://user-images.githubusercontent.com/62682595/145411918-7872e8c2-8af6-4cc9-94ef-db27ae7c558e.png)
    
II, Feature Selection:
   - We only select important features to reduce data dimension, noise data
   - There are some reason why we should choose and not with features:
       + company, agents: Booking through any company or agent has little influence on the customer's decision to cancel, so we remove these two columns
       + country: geographic factors also have little influence on the decision to cancel a room => remove
       + hotel, arrival_date_month: analyzing the chart of cancellation percentage for each month of the two types of hotels, we will see the difference when the resort type will have more cancellations in the summer, proving that the two properties **hotel** and **arrival_date_month** have both impact on the cancellation rate => select
       
       ![image](https://user-images.githubusercontent.com/62682595/145413904-16c16e3d-6766-4056-9bf4-477a03db6182.png)
       + leadtime: we can see that in practice people who book hotels closer to their arrival time tend to cancel less often than those who book a long time ago. This is reasonable so **lead_time** is also selected.

       ![image](https://user-images.githubusercontent.com/62682595/145414371-5d42d875-9ecb-40ee-b54b-fbade89d4459.png)
       + arrival_date_year: There are different events every year, so we don't use this attribute => remove
       + arrival_date_week_number: Have high correlation with arrival_date_month => remove
       + arrival_date_day_of_month: Less important feature => remove
       + stays_in_weekend_nights: The more bookings that fall on weekends, the lower the cancellation determination may be because on weekends we often go out, relax, less busy => select
       + stays_in_week_nights: The more bookings that fall on business days, the higher the probability of cancellation can increase => select
       + adults, children, babies:  We can see that in fact the larger the group of people and the more children and babies, the higher the probability of cancellation because of the risk of events (busy work, illness, ...) so we can select all 3 of these properties => select all
       + meal: Pre-order food, the type of food shows the customer's interest in their order => select
       + market_segment, distribution_channel: These are the properties of the booking form. Each booking form will also reflect a different cancellation rate. For example, a business or tour booking will be more difficult to cancel than an individual booking. as it usually has contractual terms. => select all
       + is_repeated_guest: less important feature => remove
       + previous_cancellation, previous_bookings_not_canceled: These attributes help determine the reputation of the customer, we can select both
       + reserved_room_type, assigned_room_type: less important features => remove
       + booking_changes: Customers who modify more tend to be more interested in their booking, so it's harder for the booking to be canceled => select
       + deposit_type: less important feature => remove
       + days_in_waiting_list: This feature changes over time and have many cases so the model is not generalizable => remove
       + customer_type: Reflect the bond between the customer and the hotel => select
       + adr: Rate are usually considered carefully by customers when deciding to book, so this attribute has little effect on cancellations => remove
       + required_car_parking_spaces: The chart show that cancellations related to parking space => select

       ![image](https://user-images.githubusercontent.com/62682595/145418350-75be6e4c-6819-4549-b25b-3e3f57c9d72d.png)
       + total_of_special_requests: The more special requests, the more important the properties, the lower the cancellation rate => select
       + reservation_status : noise features
       + reservation_status_date: less important features
  - Summary:
     + Remove: company, agents, country, arrival_date_year, arrival_date_week_number, arrival_date_day_of_month, is_repeated_guest,reserved_room_type, assigned_room_type, deposit_type,days_in_waiting_list, adr, reservation_status_date,reservation_status (14 features)
     + Remaining: 18 features - label = 17 features
 
III, Data Cleaning:
  - Drop 'NA' in children columns
  - In the meal column, convert "Undefined" that have not been converted to "SC" as described in the dataset
  - In the market_segment column , convert "Undefined" to "Online TA" (choose the value that appears most)
  - In the distribution_chanel column, convert "Undefined" to "TA/TO" (choose the value that appears most)
  - Drop all rows that have 0 adults, 0 children, 0 babies 
  - Convert datatype of children to integer
  - Convert reservation_status_date of children to integer
  => After process , dataset remain 87226 rows x 17 columns
  - Label 1 is 3 times more than label 3
  
  ![image](https://user-images.githubusercontent.com/62682595/145741802-4ee49490-338e-4233-8231-ec2aa9c81b48.png)

  
IV, Data Engineering
  - Convert categorical features to one-hot encoding 
 
  ![image](https://user-images.githubusercontent.com/62682595/145429319-dcf3f391-3cdf-4442-b650-2332ce81b49b.png)
  - Min-max scaler for numeric features:
  
  ![image](https://user-images.githubusercontent.com/62682595/145429434-70773ed7-daed-4831-851a-33f47474cffd.png)
 
 V, Train model
   - Split train:test = 8:2
   - Algorithm : Logistic Regresion ( Gradient Descent + Momentum)
    
 VI, Evaluate model
   - Loss chart
    
  ![image](https://user-images.githubusercontent.com/62682595/145510105-69453c91-453a-4458-8238-705ea21291e5.png)

   
   - Precision: 0.78
   - Recall: 0.93
   - F1-score: 0.85
   - Confusion matrix:
   
   ![image](https://user-images.githubusercontent.com/62682595/145510248-8d92bfab-7de7-4d7b-abf1-f47fe1954a7d.png)

    

  
   






   
